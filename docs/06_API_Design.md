# SmartNet — API Design

| Field | Value |
|---|---|
| Document ID | SN-API-001 |
| Version | 1.0 |
## Purpose
Define cloud ingress and client command interfaces.
## Scope
Gateway-to-cloud HTTPS and Firebase client interfaces.
## Audience
Backend, gateway, dashboard, and mobile engineers.
## Dependencies
[Database](05_Database_Design.md), [security](07_Security.md).
## References
Firebase Admin SDK, callable functions, Firestore REST documentation.
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | Initial API contract |
## Table of contents
- [Ingress](#ingress)
- [Client operations](#client-operations)
- [Errors](#errors)

## Ingress

Prefer a Firebase HTTPS Cloud Function `POST /v1/telemetry:ingest` for a gateway, authenticated with a provisioned gateway bearer credential verified server-side. Direct Firestore REST writes are permitted only in an early private prototype and never with owner-level credentials embedded in firmware.

```json
{"gatewayId":"GW-0001","events":[{"eventId":"GW-0001:NN-0001:42","nodeId":"NN-0001","seq":42,"receivedAt":"server","fix":{"latE7":-123456789,"lonE7":361234567,"at":1760000000,"accuracyCm":850},"batteryMv":3980,"rssi":-92,"snr":7.5,"flags":1}]}
```

Response: `200 {"accepted":["..."],"duplicates":["..."],"rejected":[]}`. Batches max 20 events / 256 KiB. Server resolves node-to-organization mapping, derives timestamps, performs idempotent writes, updates latest state, and emits alerts. `400` schema, `401` auth, `403` unregistered gateway, `409` conflicting immutable event, `429/503` retryable.

## Client operations

| Operation | Interface | Permission |
|---|---|---|
| Read nets/history | Firestore SDK scoped query | member |
| Mark recovered | callable `recoverNet({orgId,netId,note})` | admin/operator |
| Register device | callable `registerNode` | admin |
| Alert acknowledgment | callable `ackAlert` | member |

All state-changing commands return an operation/audit ID. Clients use Firebase Auth ID tokens; they never use gateway credentials.

## Errors

Use `{ "code":"SN_<DOMAIN>_<REASON>", "message":"safe user message", "retryable":false, "correlationId":"..." }`. Do not return internal rules, keys, raw packet MACs, or tenant identifiers to unauthorized callers.
