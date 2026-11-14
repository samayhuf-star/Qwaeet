# Klarity: Family Controls entitlement, paste sheet

**Form:** https://developer.apple.com/contact/request/family-controls-distribution
Sign in as the Account Holder for team PGLT2QZX88. Submit the form **5 times**, once per bundle ID below. Only the App/extension name and Bundle ID change between submissions.
Before filing, register the 4 extension IDs as explicit App IDs (Certificates, Identifiers & Profiles > Identifiers > +), each with App Groups (`group.com.samayhuf.klarity`) and Family Controls enabled.

## 1. com.samayhuf.klarity

| Field | Paste |
|---|---|
| Name | Samay Vashsht |
| Email | samayhuf@gmail.com |
| Phone | _(fill in yourself)_ |
| Team ID | PGLT2QZX88 |
| App name | Klarity (main app) |
| Bundle ID | `com.samayhuf.klarity` |
| App Store Connect Apple ID | 6833163323 |
| App description | See **Description** below |
| Why Family Controls / how it's used | See **Justification** below |

## 2. com.samayhuf.klarity.ShieldConfiguration

| Field | Paste |
|---|---|
| Name | Samay Vashsht |
| Email | samayhuf@gmail.com |
| Phone | _(fill in yourself)_ |
| Team ID | PGLT2QZX88 |
| App name | Klarity Shield Configuration extension |
| Bundle ID | `com.samayhuf.klarity.ShieldConfiguration` |
| App Store Connect Apple ID | 6833163323 |
| App description | See **Description** below |
| Why Family Controls / how it's used | See **Justification** below |

## 3. com.samayhuf.klarity.ShieldAction

| Field | Paste |
|---|---|
| Name | Samay Vashsht |
| Email | samayhuf@gmail.com |
| Phone | _(fill in yourself)_ |
| Team ID | PGLT2QZX88 |
| App name | Klarity Shield Action extension |
| Bundle ID | `com.samayhuf.klarity.ShieldAction` |
| App Store Connect Apple ID | 6833163323 |
| App description | See **Description** below |
| Why Family Controls / how it's used | See **Justification** below |

## 4. com.samayhuf.klarity.DeviceActivityMonitor

| Field | Paste |
|---|---|
| Name | Samay Vashsht |
| Email | samayhuf@gmail.com |
| Phone | _(fill in yourself)_ |
| Team ID | PGLT2QZX88 |
| App name | Klarity DeviceActivity Monitor extension |
| Bundle ID | `com.samayhuf.klarity.DeviceActivityMonitor` |
| App Store Connect Apple ID | 6833163323 |
| App description | See **Description** below |
| Why Family Controls / how it's used | See **Justification** below |

## 5. com.samayhuf.klarity.DeviceActivityReport

| Field | Paste |
|---|---|
| Name | Samay Vashsht |
| Email | samayhuf@gmail.com |
| Phone | _(fill in yourself)_ |
| Team ID | PGLT2QZX88 |
| App name | Klarity DeviceActivity Report extension |
| Bundle ID | `com.samayhuf.klarity.DeviceActivityReport` |
| App Store Connect Apple ID | 6833163323 |
| App description | See **Description** below |
| Why Family Controls / how it's used | See **Justification** below |

## Description (same for all 5)

Klarity is a personal focus and screen-time app for iPhone. The user chooses which of their own apps and categories distract them, sets a daily screen-time goal and focus sessions or schedules, and Klarity shows a calm pause screen in front of those apps at the times they picked. From the pause screen they can close the app or deliberately open it for a few minutes. Klarity also shows the user their own weekly screen time, top apps and pickups.

## Justification (same for all 5)

Klarity is a self-regulation tool for an individual managing their own iPhone (AuthorizationCenter .individual). It uses FamilyActivityPicker so the user selects their own apps and categories (Klarity only receives opaque tokens); ManagedSettings shields with a custom ShieldConfiguration and ShieldAction extension to pause those apps during the user's focus sessions and schedules; a DeviceActivity monitor extension to start and end shields on schedules and daily limits; and a DeviceActivityReport extension to show the user their own usage. All data stays on the device, shared only between the app and its extensions through the App Group group.com.samayhuf.klarity. Klarity has no accounts, sends no Screen Time data off the device, and does not use it for advertising, analytics or profiling. It is not a parental-control app and does not monitor anyone else. Users can turn it off any time in the app or in Settings > Screen Time.

_If the form has fields not listed here, the two text blocks cover them. Don't mention AI._
