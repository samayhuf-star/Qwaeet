# Klarity App Review Notes (draft)

Draft for Apple Guideline 2.1 (Information Needed) on Klarity iOS 1.0, build 5. Prepared 12 Nov 2026, 9:13 AM Asia/Dhaka (UTC+6) from `/workspace/7apps/Klarity` on `main` at `4630d61`. Application source under `src/` was not changed by the later docs/EAS commits on this branch. Claims were checked against `package.json`, `package-lock.json`, `app.json`, and the TypeScript sources. Former `[CHECK]` asides were removed from the paste-ready notes on 12 Nov 2026.

The block under “App Review Notes” is the text intended for App Store Connect (plain English, no markup required when pasted).

---

## App Review Notes

Klarity is a free iPhone self-control app. It inserts a short pause before the user opens apps they often open automatically (Instagram or YouTube). It is not a medical product and does not use Screen Time or Family Controls. After a one-time Shortcuts setup, opening a chosen app can open Klarity first; the user then chooses to do something else or to open the app. Klarity cannot see activity inside other apps and only stores pause, open, and skip events on this device. There is no account, login, in-app purchase, or subscription.

Flow from launch: welcome; pick apps from a built-in list or skip; setup explains a Shortcuts automation and shows a copyable link, klarity://pause/<app-id>. After Done, Today shows today’s opens, a streak, and a try-pause button. Tabs: Today, Apps, Insights, Settings. Review on iPhone (no tablet support).

Purpose and audience: people who want a pause before social or entertainment apps.

How to reach each main feature:

1. Pause without Shortcuts (recommended for review). On Today, tap “Open <app>”. A countdown and breathe animation appear (default 10-second wait, growing later that day up to 2 minutes). Then choose “Do something else” or “Open <app>”. “Not now” during the wait leads to swap suggestions. After an open, a 5-minute default grace period avoids a repeat pause.

2. Pause via Shortcuts (everyday use). In Settings, open “Shortcuts setup and status”. In Shortcuts, create an “App is Opened” Automation that runs immediately, add “Open URLs,” and paste the link shown for that app. Shortcuts labels may differ slightly by iOS version.

3. Apps and rules. On the Apps tab, add, remove, or edit an app (custom name and URL scheme). Rules cover the wait, optional daily limit, grace window, optional weekday-and-hour schedule (including overnight), and Strict mode. Strict mode only blocks while the user’s Shortcuts automation is on; the user can turn it off. Klarity does not block apps system-wide.

4. Insights. Last-7-days summary computed on the device.

5. Settings. Daily opens goal (default 15) and minutes per open (default 6) are used only for a “time kept” estimate. Evening check-in schedules a local 8:00 p.m. device-time notification if permission is granted. Optional 4-digit PIN locks rule changes for 1, 8, or 24 hours. Rules backup uses the share sheet; restore reads the clipboard. “Erase all data” clears local data and returns to welcome.

External services: none. The code does not use RevenueCat, StoreKit, payment APIs, AI, network APIs, or analytics SDKs. package.json and sources include only Expo and React Native modules (notifications, clipboard, linking, haptics, AsyncStorage). System uses: optional local notifications; Shortcuts and klarity://pause/<id> links; opening a chosen app by URL scheme after the user confirms; share sheet and clipboard for a rules backup. No user data is sent to us.

Regional differences: none. Dates, streaks, and schedules use the device’s local clock. The app works the same in all regions and has no regional pricing.

Regulatory documents: none. This is not a medical, financial, or other regulated-industry product. Privacy: https://samayhuf-star.github.io/Qwaeet/privacy.html. Terms: https://samayhuf-star.github.io/Qwaeet/terms.html.

Contact: Samay Vashsht, +919953710111, samayhuf@gmail.com.

---

## 1) Paid content and IAPs

There are no paid features, no subscriptions, and no in-app purchases.

Checked: `package.json` and `package-lock.json` have no RevenueCat, StoreKit, `react-native-purchases`, `expo-iap`, or similar payment packages. A search of the TypeScript sources found no product IDs, price strings, paywalls, or restore-purchases flows. Onboarding and Settings state that there is no account, no ads, and no subscription. `docs/LISTING.md` also states the app is free, with no subscription and no in-app purchases.

---

## 2) Shot list for a 60–90 second physical-device recording

Record on a physical iPhone, from a cold launch (force-quit Klarity first). There is no paywall, so this list does not include unlocking paid content or Restore Purchases; those flows do not exist. Use notification permission and Settings instead. If a catalog app such as Instagram is not installed, pick any listed app or add a custom one; the in-app “Open <app>” button still shows the pause even without Shortcuts.

1. Cold-launch Klarity from the home screen (about 5s). Show the welcome text (“No account, no ads, no subscription”) and pick one app from the list (for example Instagram). Tap Continue.
2. On “Connect your apps,” briefly scroll the Shortcuts steps and the `klarity://pause/...` link so the setup path is visible. Do not spend the whole video in Shortcuts. Tap Done (about 10s).
3. On Today, show the opens count and streak, then tap “Open <app>” (about 5s).
4. Stay on the pause screen through the default ~10-second breathing countdown (breathe in / breathe out). After the wait, show “Still want <app>?” (about 15s).
5. Tap “Do something else,” show the swap suggestions, then tap “Just close this” or one swap, and the “Nice. That's a win.” screen. Tap “Back to Klarity” (about 10s).
6. Open the Apps tab, open Edit rules for that app, and briefly show wait, daily limit, grace, scheduled block, and Strict mode. Go back (about 10s).
7. Open Insights and hold on the “Last 7 days. Computed on your phone and never uploaded.” line (about 5s).
8. Open Settings. Turn on Evening check-in and accept the iOS notification permission prompt if it appears (or show that it is already “On at 8 pm”). Scroll past the PIN lock and backup buttons, and show the privacy text and “Klarity 1.0 · Support: samayhuf@gmail.com.” There is no purchase or Restore Purchases control to show (about 15–20s).

Optional, only if time remains: return to Settings → Shortcuts setup and status, tap Open Shortcuts, and show where an App automation would be created. This is not required to demonstrate the pause.

---

## Notes on former [CHECK] items

The three `[CHECK]` asides were removed from the paste-ready App Review Notes:

- Shortcuts labels: the notes now say they may differ slightly by iOS version (the in-app setup copy is still the source of the “App is Opened” / “Open URLs” wording).
- Territories: the notes now say the app works the same in all regions, based on the code (no country- or region-specific behaviour).
- Regulatory documents: the notes now say none.
