# Klarity App IDs registered

Prepared 12 Nov 2026, 4:11 PM Asia/Dhaka. Team PGLT2QZX88, via the App Store Connect API only. No builds, profiles, certificates, deletions, app-record edits, or review changes. Family Controls was not enabled.

Verified by a fresh exact-identifier reread after the writes.

| Bundle ID | Existed or created | ASC id | Name | APP_GROUPS |
|---|---|---|---|---|
| `com.samayhuf.klarity` | Already existed (not modified except APP_GROUPS) | `P82GN6LMKN` | Klarity | Enabled now (was off) |
| `com.samayhuf.klarity.ShieldConfiguration` | Created (POST 201) | `DASM73CR55` | Klarity ShieldConfiguration | Enabled (POST 201) |
| `com.samayhuf.klarity.ShieldAction` | Created (POST 201) | `8SZ9ZTWKZC` | Klarity ShieldAction | Enabled (POST 201) |
| `com.samayhuf.klarity.DeviceActivityMonitor` | Created (POST 201) | `BQ76M73GBU` | Klarity DeviceActivityMonitor | Enabled (POST 201) |
| `com.samayhuf.klarity.DeviceActivityReport` | Created (POST 201) | `B8QWS9X8TF` | Klarity DeviceActivityReport | Enabled (POST 201) |

The four extension IDs were created with `platform: IOS`. Apple stored them as `UNIVERSAL`, same as the existing main ID. Seed ID on all five is `PGLT2QZX88`. Family Controls is not present on any of them.

Apple also attached `IN_APP_PURCHASE` on each newly created ID by default. That was not requested. The main ID already had `IN_APP_PURCHASE` and `PUSH_NOTIFICATIONS`; those were left as they were.

## What the API could not do

- **Create App Group `group.com.samayhuf.klarity`:** the App Store Connect API has no app-group resource. GET probes of `/v1/appGroups`, `/v1/applicationGroups`, `/v1/bundleIdGroups`, `/v1/identifiers`, `/v1/identifierGroups`, and `/v2/appGroups` all returned 404. No workaround was used.
- **Assign that group to the App IDs:** enabling `APP_GROUPS` turned the capability on with `settings: null`. The API has no way to attach a specific group identifier. That has to be done in the Apple Developer portal (Identifiers > App IDs / App Groups).
- **File the Family Controls request:** not attempted. Samay needs to file it himself, once per bundle ID, at https://developer.apple.com/contact/request/family-controls-distribution (see `entitlement-request.md`).

Klarity's app record, version, and review submission were not touched. Build 5 stays held.
