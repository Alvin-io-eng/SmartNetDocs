# SmartNet Mobile Architecture

| Field | Value |
|---|---|
| Version | 1.0 |
| Owner | Mobile engineering |
## Purpose
Define the Flutter companion application for field recovery.
## Scope
iOS/Android authenticated monitoring, offline read cache, notifications, and recovery actions.
## Audience
Mobile, QA, accessibility reviewers.
## Dependencies
[API](../docs/06_API_Design.md), [database](../docs/05_Database_Design.md).
## References
Flutter Firebase SDK and platform notification policies.
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | MVP architecture |
## Table of contents
- [Navigation](#navigation)
- [Screens](#screens)
- [Offline and notifications](#offline-and-notifications)

## Navigation
`AuthGate → SignIn | HomeTabs(Map, Nets, Alerts) → NetDetail → RecoveryConfirm`; Settings is available from Home. Use named routes/deep link to a net only after authentication and authorization.

## Screens
| Screen | Content | States and rules |
|---|---|---|
| Sign in | Identity provider/form, support error | Validates fields, explains denied/disabled account |
| Map | Markers, current-location permission, net summary sheet | Map alternative list; stale/empty/loading/error states |
| Nets | Filter/sort list with battery/freshness | Cursor pagination; offline banner |
| Net detail | Last known map, telemetry history, recovery CTA | CTA only for operator/admin; confirmation required |
| Alerts | Low-battery/stale/recovery items | Acknowledge action and deep links |
| Settings | profile, active org, notification preference | Permission rationale and accessible labels |

## Offline and notifications

Enable Firestore persistence. Read cached data while offline, display “cached” and last sync time, and never queue a recovery action unless its callable operation can be authenticated and confirmed after reconnect. FCM notification payload carries net ID and safe summary only; app fetches authorized detail after opening. Support screen readers, 44×44 logical-pixel controls, scalable text, and non-color status cues.
