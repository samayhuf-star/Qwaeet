# Klarity: Screen Time API rebuild, native plan

Prepared 12 Nov 2026 (BDT). Prep only. Repo `samayhuf-star/Qwaeet` main @ 6989690 (Expo SDK 58, RN 0.88, expo-router). No code changed.

## Approach
Replace the Shortcuts + URL-scheme interception with Apple's Screen Time stack:
FamilyControls (`.individual` auth + `FamilyActivityPicker`) -> ManagedSettings shields -> custom ShieldConfiguration/ShieldAction UI -> DeviceActivity schedules and thresholds -> DeviceActivityReport for Insights.

### Build tooling (pick one, recommendation first)
1. **Recommended: `@bacons/apple-targets` (v5) + a small local Expo module.** apple-targets generates the 4 Swift extension targets from `targets/*/expo-target.config.js` during `expo prebuild`, including entitlements and App Group. Full control, covers DeviceActivityReport.
2. Alternative: `react-native-device-activity` (kingstinct, v0.6.1, peer expo >=52). Ships an Expo plugin, JS API and the Monitor/ShieldAction/ShieldConfiguration extensions, which saves ~1.5 days. It doesn't provide a DeviceActivityReport extension, so add that target with apple-targets anyway. Check it on SDK 58 / RN 0.88 (New Architecture) in the first dev build before committing to it.

### Targets, IDs, entitlements
| Target | Bundle ID | Entitlements |
|---|---|---|
| Klarity (app) | `com.samayhuf.klarity` | `com.apple.developer.family-controls`, `com.apple.security.application-groups = [group.com.samayhuf.klarity]` |
| ShieldConfiguration | `.ShieldConfiguration` | family-controls, app group |
| ShieldAction | `.ShieldAction` | family-controls, app group |
| DeviceActivityMonitor | `.DeviceActivityMonitor` | family-controls, app group |
| DeviceActivityReport | `.DeviceActivityReport` | family-controls, app group |
Team `PGLT2QZX88`. Deployment target iOS 16.0+ (needed for `.individual` auth).

### Native module `KlarityScreenTime` (Swift, Expo Modules API)
JS surface (≈10 calls):
- `getAuthStatus()`, `requestAuthorization()` -> `AuthorizationCenter.shared.requestAuthorization(for: .individual)`
- `<ActivityPickerView>` native view wrapping `FamilyActivityPicker` (SwiftUI via ExpoView/UIHostingController); returns a selection id + counts (apps/categories/domains). Tokens never go to JS; they're encoded as `FamilyActivitySelection` (Codable) into the App Group.
- `setShield(groupId, on)` / `clearAllShields()` -> named `ManagedSettingsStore(named:)` per group.
- `startFocus(minutes, groupIds)` / `endFocus()` -> shield now + `DeviceActivityCenter.startMonitoring` for the end.
- `setSchedule(groupId, {days, start, end})`, `setDailyLimit(groupId, minutes)` -> `DeviceActivitySchedule` + `DeviceActivityEvent(threshold:)`.
- `grantGrace(groupId, minutes)` (used by "Open anyway for 5 min").
- `getEvents()` -> shield shown / closed / opened-anyway counts written by the extensions to the App Group (for streaks and "pauses that worked").
- `<UsageReportView context="week|today">` -> SwiftUI `DeviceActivityReport` hosted in RN.

### Shared storage (App Group `group.com.samayhuf.klarity`)
- `UserDefaults(suiteName:)`: encoded selections per group, schedules, limits, grace-until timestamps, shield copy/theme, event counters.
- Extensions read config from it (ShieldConfiguration for copy/colors, Monitor for which store to shield) and append events.
- JS keeps its own AsyncStorage for UI-only prefs. The App Group is the source of truth for anything the extensions need.

### Extension behaviour
- **ShieldConfiguration**: Klarity-dark shield with the icon, the "Take a breath" title, subtitle from the App Group, a primary button "Close app" and a secondary "Open anyway for 5 min". (System shield: only icon, title, subtitle, 2 buttons and colors can be customized. The animated breathing ring is in-app only, see risks.)
- **ShieldAction**: primary -> `.close`, log event. Secondary -> write grace, remove that app's token from the store, start a ≥15-min DeviceActivity interval whose `intervalDidEnd`/threshold re-applies the shield after 5 min (Apple's minimum schedule length is 15 min, so use the shifted-start trick or an event threshold). Return `.defer`/`.none`. It can't open Klarity directly; optional local notification "Tap for a 30-second pause" deep-links to the in-app breathe screen.
- **DeviceActivityMonitor**: `intervalDidStart/End` toggle schedule shields; `eventDidReachThreshold` applies daily-limit shields; re-shield after grace.
- **DeviceActivityReport**: SwiftUI scenes `TotalActivity` (today ring vs goal), `Week` (7-day bars), `TopApps`, `Pickups`. This data is sandboxed: it can only be *displayed* by the extension, never read back into JS or sent anywhere. Today ring and Insights must be report views embedded in RN.

## What gets removed (Shortcuts flow)
- `src/app/setup.tsx` (the whole Shortcuts guide, copy links, "Open Shortcuts").
- `klarity://pause/<id>` external entry + `lastExternalPause` / "stale setup" banner in `(tabs)/index.tsx`, `lib/store.tsx`.
- URL-scheme catalog `lib/catalog.ts` (`scheme`, `LSApplicationQueriesSchemes` in app.json) and the bounce-back `Linking.openURL(rule.scheme)` / grace bounce in `pause/[id].tsx`.
- Shortcuts copy in `rule/[id].tsx` and `(tabs)/settings.tsx` ("Shortcuts setup and status").
- Logic kept/reused: delay growth, daily opens, windows, swaps (`lib/logic.ts` + tests) become *schedule/limit config* instead of a runtime gate. `pause/[id].tsx` is reworked as the in-app breathe screen.

## Files to add / change
Add:
- `targets/shield-configuration/{expo-target.config.js, ShieldConfigurationExtension.swift}`
- `targets/shield-action/{expo-target.config.js, ShieldActionExtension.swift}`
- `targets/device-activity-monitor/{expo-target.config.js, DeviceActivityMonitorExtension.swift}`
- `targets/device-activity-report/{expo-target.config.js, ReportExtension.swift, TotalActivityView.swift, WeekView.swift, TopAppsView.swift}`
- `targets/_shared/AppGroup.swift` (keys, Codable models, shared by all targets)
- `modules/klarity-screentime/{expo-module.config.json, ios/KlarityScreenTimeModule.swift, ios/ActivityPickerView.swift, ios/UsageReportView.swift, index.ts}`
- `src/lib/screentime.ts` (typed JS wrapper + mocks for web/Android)
- New screens: `src/app/onboarding.tsx` (rewritten: explainer -> authorize), `(tabs)/index.tsx` (Today ring + session), `(tabs)/apps.tsx` (groups + picker), `(tabs)/insights.tsx` (report views), `src/app/focus.tsx` (start session sheet), `src/app/breathe.tsx`
- `src/components/ui.tsx` theme refresh (navy, single accent)
Change: `app.json` (plugins `@bacons/apple-targets`, `ios.appleTeamId`, `ios.deploymentTarget` 16.0 via expo-build-properties, entitlements, app group; drop `LSApplicationQueriesSchemes`), `package.json`, `eas.json` (dev profile for real device), `lib/store.tsx`, `lib/logic.ts`, `docs/PRIVACY.md` + `privacy.html` (Screen Time data on-device only), `docs/LISTING.md`, `README.md` (new setup + entitlement notes), App Review notes.
Delete: `src/app/setup.tsx`, `src/lib/catalog.ts`.

## Testing limits
- Screen Time APIs don't work in the iOS Simulator (authorization, picker tokens, shields, DeviceActivity callbacks, reports). **A real iPhone (iOS 16+) is required**, with Screen Time enabled and the passcode set.
- Firebase Test Lab / cloud device farms can't grant Family Controls authorization or show shields, so Testing E2E Bot can automate only the JS UI. Shield/schedule QA must be done by Samay on device against a written checklist, plus screen recordings.
- DeviceActivity monitor callbacks can lag (often 1-several min) and have a 15-min minimum interval. QA must allow for this.
- Extensions have tight memory limits (the Monitor extension is commonly reported at ~6 MB). Keep them SwiftUI-light with no RN inside.

## Effort and builds
| Work | Days |
|---|---|
| Targets + plugin + App Group + entitlements, first dev build on device | 1.5 |
| Native module (auth, picker view, shields, schedules, grace) | 2 |
| 4 extensions (shield UI, action + grace re-shield, monitor, report views) | 2.5 |
| JS screens redesign (5 screens + focus/breathe), remove Shortcuts flow | 2.5 |
| Privacy/listing/review notes/README, on-device QA fixes | 1.5 |
| **Total** | **~10 working days** (~8.5 if `react-native-device-activity` works on SDK 58) |

EAS builds:
- Development: 1-2 `development` builds (Family Controls Development, no approval needed) to a registered iPhone. Samay's UDID must be registered for ad-hoc/dev.
- **Distribution: 1 production build -> TestFlight**, only after all 5 bundle IDs show Family Controls "Assigned". Then QA, then the resubmission with Samay's go-ahead. Build 5 (Shortcuts) is not resubmitted.

## Risks
1. **Entitlement wait / partial grant.** All 5 bundle IDs need approval. A missing extension grant blocks TestFlight signing. Apple may also ask questions or reject vague use cases. Mitigation: file all 5 today with the same clear self-regulation text, and keep dev builds moving.
2. **App Review 2.5.1** ("use APIs and frameworks for their intended purposes and indicate that integration in their app description"): the listing and review notes must state that Klarity uses Screen Time / Family Controls for the user's own screen-time management. Also 2.5.1 public APIs only. 4.2/4.3 minimum functionality: blocking must clearly work in the review build, so give a demo video in notes.
3. **Privacy 5.1.1 / 5.1.2:** privacy policy must say what Screen Time data is used and that it stays on-device. No data collection without consent, nothing sent to third parties, no analytics or ads on usage data. The report extension's sandbox enforces this, but the policy and App Privacy labels must match. (The guidelines don't name Screen Time in 5.1.2(vi), but Apple's entitlement review applies the same standard.)
4. **Platform limits vs. design:** the shield screen is a fixed system template, so there's no animated breathing on the shield itself. "Open anyway for 5 min" needs a 15-min-minimum DeviceActivity workaround. Report data can't be read into JS (custom streaks must come from our own event counters). Users can revoke access in Settings, so handle `.denied`/revoked gracefully.
5. **Expo SDK 58 / RN 0.88 compatibility** of apple-targets / RNDA with New Architecture; extension memory limits; Android has no equivalent (iOS-only feature, Android build unaffected or hidden).
