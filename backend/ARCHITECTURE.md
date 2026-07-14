# SmartNet Backend Architecture

| Field | Value |
|---|---|
| Version | 1.0 |
| Owner | Backend engineering |
## Purpose
Implement Firebase identity, tenant authorization, telemetry ingestion, alerts, and audit operations.
## Scope
Firestore rules/indexes, Cloud Functions, configuration, and emulator tests; UI and firmware are excluded.
## Audience
Backend, security, QA, DevOps.
## Dependencies
[Database](../docs/05_Database_Design.md), [API](../docs/06_API_Design.md), [security](../docs/07_Security.md).
## References
Firebase Admin SDK and Emulator Suite documentation.
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | MVP architecture |
## Table of contents
- [Structure](#structure)
- [Responsibilities](#responsibilities)
- [Validation](#validation)

## Structure
```text
backend/
├── functions/src/{ingest.ts,net.ts,alerts.ts,auth.ts}
├── firestore.rules
├── firestore.indexes.json
└── tests/{rules,functions}/
```
## Responsibilities

`ingest` authenticates gateways, validates normalized events, resolves node ownership, and performs idempotent transaction/batch writes to history and latest. `net` exposes recovery/registration callables with role checks. `alerts` derives stale/low-battery notifications without exposing location to nonmembers. Rules protect direct client reads and prohibit client telemetry writes.

## Validation

Use schema validation before any Admin SDK write, explicit organization membership checks in functions, correlation IDs, structured redacted logs, and emulator rules tests. Function retries must be idempotent by event ID. Keep secrets in managed configuration, never source files.
