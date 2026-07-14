# SmartNet Dashboard Architecture

| Field | Value |
|---|---|
| Version | 1.0 |
| Owner | Web engineering |
## Purpose
Define the React/Vite operator dashboard.
## Scope
Authenticated web experience; Firebase serves data and Leaflet/OpenStreetMap renders maps.
## Audience
Web, UX, QA, accessibility reviewers.
## Dependencies
[Database](../docs/05_Database_Design.md), [security](../docs/07_Security.md).
## References
React, Tailwind, Leaflet, WCAG 2.2 guidance.
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | MVP UX specification |
## Table of contents
- [Structure](#structure)
- [Pages](#pages)
- [State and accessibility](#state-and-accessibility)

## Structure
```text
dashboard/src/{app,features/{auth,nets,recovery},components,map,services/firebase}/
```

## Pages
| Page | Purpose and widgets | Interactions / permissions |
|---|---|---|
| Sign in | Email/provider sign-in, error banner | Validates required email; unauthenticated only |
| Overview | Status cards, searchable/sortable net table, map markers | Member; table paginates, filters status, marker opens detail |
| Net detail | Last fix map, battery/RSSI cards, paginated telemetry history, recovery action | Member reads; operator/admin can recover with confirmation |
| Devices | Gateway/node status and diagnostics | Admin registers/disables; member read if policy permits |
| Settings | Profile, organization context, notification preference | User edits own allowed fields |

Every page has skeleton loading, explicit empty-state guidance, retryable error state, and responsive layout: map/table stack below 768 px. Keyboard focus, labeled controls, contrast, non-color status labels, and map-list alternative are required. Never use map position as the sole presentation of a net.

## State and accessibility

Use Firestore listeners for current nets and cursor pagination for history. Render `lastReceivedAt` and “stale” independently of browser cache state. Route guards wait for Auth resolution; errors include correlation ID if supplied. Recovery uses a modal confirmation and optimistic UI only after function success.
