# SmartNet — Deployment Guide

| Field | Value |
|---|---|
| Document ID | SN-DEP-001 |
| Version | 1.0 |
## Purpose
Define repeatable environment provisioning, release, rollback, and pilot operation.
## Scope
Firebase, Vercel, mobile distribution, and ESP32 firmware releases.
## Audience
DevOps, release manager, technicians.
## Dependencies
[Security](07_Security.md), [API](06_API_Design.md), [testing](08_Testing_Plan.md).
## References
Firebase CLI, Vercel, Flutter, and ESP32 toolchain documentation.
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | MVP release process |
## Table of contents
- [Environments](#environments)
- [Release flow](#release-flow)
- [Operations](#operations)

## Environments

Use separate Firebase projects: `smartnet-dev` (emulators/default), `smartnet-staging` (integration), `smartnet-prod` (pilot). Configure authorized domains, OAuth redirect URLs, Firestore rules/indexes, FCM, and server secrets separately. Vercel preview deployments target staging only; production dashboard uses production public configuration. Firebase public config is not a secret; gateway credentials and HMAC keys are secrets.

## Release flow

```mermaid
flowchart LR
 C[Commit + lint/test] --> S[Deploy staging rules/functions/web]
 S --> V[Run emulator + integration + field smoke]
 V --> A[Approve tagged release]
 A --> P[Deploy production]
 P --> M[Monitor and record versions]
```

1. Version and tag every firmware, app, and backend release.
2. Apply Firestore indexes/rules before dependent client code; test rules in emulator.
3. Deploy functions, then web. Publish mobile through a pilot channel after smoke testing.
4. Flash gateway/node over USB in MVP; record hardware ID, firmware version, key reference, and installer.
5. Roll back web/functions by previous tagged deployment; firmware rollback requires physical reflash in MVP.

## Operations

Monitor function errors, Firestore quota, gateway last-seen, stale nets, queue depth, and battery warnings. Back up/export only under approved access controls. Deployment is blocked if secrets appear in source, rules tests fail, or radio configuration lacks local regulatory approval.
