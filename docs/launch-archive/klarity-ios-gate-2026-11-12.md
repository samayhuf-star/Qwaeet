# Klarity iOS 1.0 (build 5) — pre-resubmit gate

**Date:** 12 November 2026, Asia/Dhaka (UTC+6)  
**Method:** Static, read-only. Code at `/workspace/7apps/Klarity` commit `4630d61` via `git show` (tree was clean; nothing was checked out). No App Store Connect, repo, or listing edits.  
**Materials:** `klarity-review-notes-draft.md`, `docs/LISTING.md`, `docs/privacy.html`, `docs/terms.html`, and the 6.1" set at `/workspace/7apps/Klarity/store/screenshots/6.1/01.png`–`06.png` (onboarding, Today, Apps, pause, Insights, Settings). No `fastlane/` or `metadata/` folder. This is the only screenshot set (1206×2622).  
**Result:** **FAIL** (not ready to resubmit).

---

## Table

| Area | Result | Notes |
|---|---|---|
| Review notes vs Apple’s 6 asks | **Partial** | (b)(c)(d) are supported by the code. (e) is honest but still has a `[CHECK]`. (a) is only a shot list — no recording in this packet. (f) points at live legal pages that still show a date placeholder. |
| Review notes vs code `4630d61` | **Mostly accurate** | Pause flow, tabs, defaults (10 s wait, grow to 2 min, 5 min grace, goal 15, 6 min/open, 8:00 p.m. check-in, 4-digit PIN for 1/8/24 h, backup/restore, erase), no IAP/account/network/AI, iPhone only. |
| Screenshots (Guideline 2.3.3) | **PASS** | All six 6.1" shots viewed and compared to code at `4630d61`. Real app UI; no mockups, marketing overlays, or placeholder copy. See Screenshots section. Issue 1 (P0-1) is resolved. |
| Description / keywords vs the app | **Mostly match**, with one overclaim | Listing describes Shortcuts + an in-app pause, not system-wide blocking. One feature line overstates the connection check. Keyword `screen time` does not match a Screen Time API. No AI wording in the listing (and there is no in-app credits screen, and no AI in the app). |
| Privacy URL | **PASS, with a placeholder** | `https://samayhuf-star.github.io/Qwaeet/privacy.html` returns HTTP 200 and names **Samay Vashsht**. Effective date is still `[EFFECTIVE DATE – LAUNCH DAY]`. Terms at the same host also 200, same date placeholder. Site root `/Qwaeet/` is 404. |
| Device crash check (Guideline 2.1) | **PENDING** | Waiting on Samay’s iPhone screen recording. |
| Placeholders | **FAIL** | See issues 2, 3, 5. |

---

## Numbered issues

1. **P0 — No store screenshots were available to review (Guideline 2.3.3).** — **RESOLVED**  
   The 6.1" set is at `/workspace/7apps/Klarity/store/screenshots/6.1/` (`01.png`–`06.png`). All six **PASS** 2.3.3 against commit `4630d61` (see Screenshots section).

2. **P0 — Apple’s request for a screen recording is not fulfilled yet (Guideline 2.1 Information Needed, ask a).**  
   **Where:** review notes §2 has a solid 60–90 s shot list; no recording file is in this packet.  
   **Fix:** Record on a physical iPhone from that list, attach it in App Store Connect, and only then paste the notes. Until that recording exists, do not resubmit.

3. **P0 — Live Privacy and Terms still show a launch placeholder.**  
   **Where:** `https://samayhuf-star.github.io/Qwaeet/privacy.html` and `…/terms.html` (and `docs/privacy.html`, `docs/terms.html`): `Effective date: [EFFECTIVE DATE – LAUNCH DAY]`.  
   **Fix:** Put a real date on both pages and republish GitHub Pages before resubmit. (`docs/PRIVACY.md` already says “Last updated: 11 November 2026”, but that is not what the live HTML shows.)

4. **P1 — Listing overclaims the Shortcuts connection check.**  
   **Where:** `docs/LISTING.md` — “A setup check that tells you if the connection stops working.”  
   **Code:** Today shows “Setup not finished” only while `lastExternalPause` is null; Setup then shows “Connected” plus the time of the last Shortcuts pause. One successful pause forever counts as connected. It does not detect an automation that later stops.  
   **Fix:** Change the listing to match (for example: “a setup screen that shows whether Klarity has ever received a pause from Shortcuts”).

5. **P1 — `[CHECK]` text is still inside the pastable App Review Notes.**  
   **Where:** draft lines 21, 31, 33 (Shortcuts label caveat; App Store Connect territories; other regulatory files).  
   **Fix:** Do not paste `[CHECK]` to Apple. Either resolve those three points or drop the bracketed asides from the notes block.

6. **P2 — Support field in the listing draft is an email, not a URL.**  
   **Where:** `docs/LISTING.md` — “Support URL / contact: samayhuf@gmail.com”. The GitHub Pages root `https://samayhuf-star.github.io/Qwaeet/` 404s.  
   **Fix:** Host a simple support page (or use a working HTTPS URL) and use that as the App Store support URL.

7. **P2 — Keyword `screen time` does not match what the app does.**  
   **Where:** `docs/LISTING.md` keywords (97 of 100 characters). The app, review notes, and terms all state it does **not** use the Screen Time / Family Controls API.  
   **Fix:** Replace `screen time` with a term that does not imply Apple Screen Time.

---

## Screenshots (2.3.3)

Source: `/workspace/7apps/Klarity/store/screenshots/6.1/` — the only set, all **1206×2622** (correct for 6.1"). Viewed each PNG and checked against the app at `4630d61`. No device frames, marketing copy, lorem, or UI that is not in the code. “spotify” on the Apps screen is the in-app helper text from `apps.tsx`, not a leftover placeholder.

| File | Screen | Result | Notes |
|---|---|---|---|
| `01.png` | Onboarding | **PASS** | “Welcome to Klarity”, the on-device / no-account copy, all 14 catalog names in code order, and “Skip for now”. |
| `02.png` | Today (home) | **PASS** | Copy, tabs, 2/15 opens, “5 skipped · about 30 min kept today (estimate)” (5×6 min), 10-day streak, Instagram 2/8 next wait 20s (10+5×2), TikTok 0/5 wait 15s, YouTube 0 opens wait 10s — all producible by `decide()` / `minutesReclaimed()`. |
| `03.png` | Apps | **PASS** | Wait/limit lines match `apps.tsx` and the Today rules (TikTok base 15s, YouTube +3s / no limit). Remaining catalog chips use “+ {name}”. Custom-scheme field is clipped by the tab bar, as on a real short viewport. |
| `04.png` | Pause (wait) | **PASS** | Prompt is `PROMPTS[5]` (“Are you bored, tired, or avoiding something?”), 18s countdown, “Breathe in…”, “Not now”, “Wait: 18s”. 18s is a mid-count of a legal delay (e.g. the 20s Instagram wait). |
| `05.png` | Insights | **PASS** | Exact 7-day on-device copy. 65 skips × 6 min = ~390m. Day bars, top-three triggers, and “most around 20:00” match `insights.tsx` / `dayStats()`. Thu–Wed order is what the code shows when “today” is a Wednesday. |
| `06.png` | Settings | **PASS** | Goals (15 / 6 min), estimate copy, Shortcuts button, Evening check-in “Off”, lock copy, PIN hint “Choose a 4-digit PIN”, Lock 1h/8h/24h, backup copy and buttons, Privacy header. Lower Privacy/Data lines are below the fold, as in the real screen. |

No FIX items. P0-1 is resolved. The gate is still **FAIL** until the screen recording (issue 2), the live legal-date placeholder (issue 3), and the pending device crash check are done.

## Placeholders found (listing, legal pages, review notes)

- `[EFFECTIVE DATE – LAUNCH DAY]` on live and repo Privacy and Terms HTML.
- `[CHECK]` three times in the pastable review notes (and in the draft’s CHECK list).
- `docs/LISTING.md` still says the privacy URL is “not live until Pages is enabled” — stale; the URL is live.

No TODO, lorem, example.com, or dummy email/phone in the listing copy or in the notes’ contact line (`Samay Vashsht`, `+919953710111`, `samayhuf@gmail.com`). No AI mention in the store listing.

This gate does **not** authorise a resubmit.
