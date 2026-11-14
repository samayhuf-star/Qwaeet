# Klarity: Family Controls (Distribution) entitlement request

Prepared 12 Nov 2026 (BDT). Prep only; nothing has been filed.

Form: https://developer.apple.com/contact/request/family-controls-distribution
(Alternative: Certificates, Identifiers & Profiles > Identifiers > the App ID > Capability Requests tab.)

## App facts (from repo samayhuf-star/Qwaeet @ 6989690, main)
| Field | Value | Source |
|---|---|---|
| App name | Klarity | app.json `expo.name` |
| iOS bundle ID (main app) | `com.samayhuf.klarity` | app.json `ios.bundleIdentifier` |
| Apple Team ID | `PGLT2QZX88` | eas.json `submit.production.ios.appleTeamId` |
| App Store Connect Apple ID | `6833163323` | eas.json `ascAppId` |
| Developer | Samay Vashsht | |
| Contact email | samayhuf@gmail.com | |
| Phone | Leave for Samay to fill from his account profile. Do not invent one. | |

## Bundle IDs that need the entitlement
Apple grants Family Controls per bundle ID. Its doc says: "If your app includes a Screen Time API app extension such as Device Activity Monitor, Device Activity Report, Shield Action, or Shield Configuration, submit the same request for the extension." So file **5 requests**, one per row (same text each time, just change the bundle ID):

| # | Target | Proposed bundle ID | Also needs |
|---|---|---|---|
| 1 | Main app | `com.samayhuf.klarity` | App Group `group.com.samayhuf.klarity` |
| 2 | Shield Configuration ext. | `com.samayhuf.klarity.ShieldConfiguration` | App Group |
| 3 | Shield Action ext. | `com.samayhuf.klarity.ShieldAction` | App Group |
| 4 | DeviceActivity Monitor ext. | `com.samayhuf.klarity.DeviceActivityMonitor` | App Group |
| 5 | DeviceActivity Report ext. | `com.samayhuf.klarity.DeviceActivityReport` | App Group |

Extension IDs 2-5 are proposals: they don't exist yet. They must be registered as explicit App IDs (with App Groups + Family Controls capability) before or while filing, and the native code must use exactly these IDs. Lock them in before filing; changing them later means new requests.

## Form answers (paste-ready)

**App name:** Klarity

**Bundle ID:** (one of the five above per submission)

**If the form asks whether the app is live:** No. Version 1.0 is in review/rejected state (App Store Connect app ID 6833163323); this request supports the replacement blocking flow.

**Describe your app:**
Klarity is a personal focus and screen-time app for iPhone. The person using the app chooses which of their own apps and app categories distract them, sets a daily screen-time goal and focus sessions or schedules, and Klarity places a calm pause screen in front of those apps during the times they picked. From the pause screen they can take a short breathing pause and close the app, or deliberately open it for a few minutes. Klarity also shows the user their own weekly screen-time summary, top apps and pickups so they can see progress against their goal.

**Why do you need the Family Controls entitlement / how will you use it?**
Klarity is a self-regulation tool for an individual adult managing their own device. It uses:
- **FamilyControls** `AuthorizationCenter.requestAuthorization(for: .individual)` so the user authorizes Klarity on their own device, and `FamilyActivityPicker` so the user selects the apps, categories and websites they want help with. Klarity only ever receives opaque tokens; it never sees app names or content beyond what the system renders.
- **ManagedSettings** to apply shields to the user's selected apps during focus sessions and schedules, with a custom **ShieldConfiguration** extension (Klarity-styled pause screen) and **ShieldAction** extension (Close app, or open for a short user-chosen grace period, after which the shield returns).
- **DeviceActivity** monitor extension to start and end shields on the user's schedules and when daily usage thresholds they set are reached, and a **DeviceActivityReport** extension to show the user their own screen-time totals, top apps and pickups inside the app.

All selections, schedules and usage data stay on the device (shared between the app and its extensions only through the app's App Group). Klarity has no accounts, does not transmit Screen Time data off the device, does not use it for advertising, analytics, profiling or resale, and does not monitor any other person. The user can turn off shields or revoke authorization at any time in the app or in iOS Settings > Screen Time.

**Who is the user?** The device owner (individual authorization). Klarity is not a parental-control app and does not manage a child's device.

(Do not mention AI anywhere in the form.)

## Who must submit
**The Apple Developer Account Holder of team PGLT2QZX88 must submit it, signed in to developer.apple.com.** Apple's doc: "Before you distribute an app that uses Family Controls, your Apple Developer Account Holder must request permission to use the com.apple.developer.family-controls entitlement."
Source: https://developer.apple.com/documentation/familycontrols/requesting-the-family-controls-entitlement

If Samay is the Account Holder (individual account), Samay must file it himself, from his own signed-in session. An agent can't file it: it requires his Apple ID sign-in, and filing a form on his behalf needs his explicit approval anyway. The doc names only the Account Holder, so plan on Samay doing it rather than an Admin. Approximate time: 10 min for all five submissions.

After approval: Apple shows "Assigned" under Capability Requests. Then enable **Family Controls (Distribution)** on each of the 5 App IDs and regenerate the distribution profiles (EAS will do this on the next build with `eas credentials` / automatic profile regeneration).

Typical wait reported by developers: a few business days to a few weeks (third-party reports, not an Apple SLA). File today.

## Development builds don't need approval
The **Family Controls (Development)** capability (`com.apple.developer.family-controls` with development provisioning) can be enabled on the App IDs without Apple's approval. Development builds (EAS `development` profile / ad-hoc to registered devices) can therefore exercise authorization, picker, shields and schedules on a real iPhone while the request is pending. **TestFlight and App Store builds need the Distribution grant** for all five bundle IDs, or signing fails.
