# Klarity: App Store Connect listing text (QA fixes, 12 Nov 2026)

## Description
Most phone pickups are automatic. Klarity gives you a breath of space before you open the apps that pull you in, so you can choose on purpose.

HOW IT WORKS
Set up a one-time automation in the Shortcuts app. From then on, when you open a chosen app, Klarity appears first with a short breathing pause and a question. Then you decide: do something else, or carry on.

WHAT YOU GET
- A pause that gets longer each time you open an app in a day
- Reflection questions that rotate, so they never go stale
- Daily open limits per app, and an optional strict mode
- Scheduled focus windows by day and hour, including overnight
- A grace window so you aren't interrupted again right after you choose to open
- Swap ideas: a glass of water, a stretch, a message to a friend
- A streak with a "life happens" rule: one slip a week is forgiven
- Weekly insights: skips, estimated time kept, top triggers, peak hour
- A settings lock with a PIN you can give to someone you trust
- Back up and restore your rules after a reinstall
- A setup screen that shows “Not connected yet” until Klarity receives its first pause from Shortcuts, then “Connected”

PRIVATE BY DESIGN
No account. No ads. No tracking. Everything stays on your iPhone. Klarity can't see what you do inside other apps.

GOOD TO KNOW
Klarity works through Apple's Shortcuts automations. You are always in control and can turn any automation off in Shortcuts at any time.

## Keywords (95/100)
focus,mindful,pause,app limit,habit,digital wellbeing,doomscroll,breathe,self control,awareness

## URLs
Support: https://samayhuf-star.github.io/Qwaeet/support.html
Privacy: https://samayhuf-star.github.io/Qwaeet/privacy.html
Terms: https://samayhuf-star.github.io/Qwaeet/terms.html
Marketing/home: https://samayhuf-star.github.io/Qwaeet/

## Effective date one-liner (run on resubmit day from /workspace/7apps/Klarity, then commit, push and merge)
DATE='12 November 2026'; sed -i "s/\[EFFECTIVE DATE – LAUNCH DAY\]/${DATE}/g" docs/privacy.html docs/terms.html
