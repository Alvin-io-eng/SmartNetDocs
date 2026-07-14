# SmartNet — Project Roadmap

| Field | Value |
|---|---|
| Document ID | SN-RMP-001 |
| Version | 1.0 |
## Purpose
Define MVP delivery phases, decision gates, and future evolution.
## Scope
Prototype through pilot; future items are explicitly non-MVP.
## Audience
Product, engineering, funders, pilot operators.
## Dependencies
[Requirements](02_Requirements.md), [deployment](09_Deployment.md).
## References
Project overview and test plan.
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | Initial roadmap |
## Table of contents
- [Phases](#phases)
- [Gates](#gates)
- [Future improvements](#future-improvements)

## Phases

| Phase | Deliverable | Exit gate |
|---|---|---|
| 0 — Foundation | Repository, Firebase dev project, protocol fixtures | Rules and packet unit tests pass |
| 1 — Bench prototype | Node/gateway LoRa + GPS + power measurement | Valid packets and sleep-current evidence |
| 2 — Cloud and clients | Ingestion, dashboard, mobile core journeys | Staging end-to-end trace passes |
| 3 — Field validation | Waterproof buoy and recovery trials | T-FLD-001 and T-ENV-001 pass |
| 4 — Pilot | One-node monitored deployment | Operator acceptance and incident review |

## Gates

No phase advances on unresolved safety, data-isolation, or radio-compliance failures. Prototype component substitutions require electrical/protocol regression tests. Pilot expansion requires evidence that Firebase quotas, battery autonomy, and radio range suit the intended site.

## Future improvements

Multi-gateway deduplication, encrypted LoRa payloads, OTA with signed images, cellular/satellite backhaul, production solar charger/PCB, mesh/routing study, geofencing, multilingual UX, analytics, and automated retention are planned only after MVP validation.
