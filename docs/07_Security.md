# SmartNet — Security Architecture

| Field | Value |
|---|---|
| Document ID | SN-SEC-001 |
| Version | 1.0 |
## Purpose
Define threat controls, identity boundaries, secrets handling, and incident response.
## Scope
Device, radio, cloud, client, and operational security for the MVP.
## Audience
All implementers, security reviewers, operators.
## Dependencies
[Protocol](04_Communication_Protocol.md), [database](05_Database_Design.md), [deployment](09_Deployment.md).
## References
OWASP ASVS/MASVS; Firebase Rules guidance; NIST incident handling principles.
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | MVP controls |
## Table of contents
- [Assets and threats](#assets-and-threats)
- [Controls](#controls)
- [Rules model](#rules-model)
- [Response](#response)

## Assets and threats

Assets: fleet location, organization membership, gateway credentials, device MAC keys, telemetry integrity, and availability. Primary threats: stolen client session, cross-tenant Firestore query, radio replay/spoofing, leaked firmware secret, physical node capture, and denial of service.

## Controls

| Threat | Required control |
|---|---|
| Cross-tenant data access | Organization-subcollection rules; test deny cases |
| Radio spoof/replay | Per-node HMAC key, canonical payload MAC, sequence/freshness window |
| Gateway impersonation | Unique rotating credential in protected device storage; server mapping |
| Secret leakage | `.env` ignored, CI secret store, secret scanning, no keys in app/firmware source |
| Lost device | Disable gateway/node record; rotate its credential/key; audit action |
| Excessive access | Least-privilege roles, rate limits, bounded queries |

Use TLS for all gateway/cloud and client/cloud traffic. HMAC does not provide radio confidentiality; treat LoRa payload positions as potentially observable in MVP. Encrypting payloads is a planned hardening item before large deployment.

## Rules model

Firestore reads require `request.auth != null` and membership at `organizations/{orgId}/members/{uid}`. Members may read organization data; only admins modify membership/device registration; clients cannot create telemetry or alter `lastReceivedAt`, location, battery, or audit fields. Cloud Function/Admin writes bypass rules and must validate equivalent authorization explicitly.

## Response

On suspected compromise: disable affected identity, preserve audit/log evidence, rotate secrets, notify organization owner, assess scope, and test restored access. Never revoke all tenants to solve a single-device incident unless platform compromise is confirmed.
