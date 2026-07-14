# SmartNet — Requirements Specification

| Field | Value |
|---|---|
| Document ID | SN-REQ-001 |
| Version | 1.0 |
| Status | Normative |

## Purpose

Define verifiable MVP requirements and their traceability.

## Scope

Product, hardware, firmware, cloud, dashboard, and mobile behavior.

## Audience

Engineering, QA, product, and acceptance-test owners.

## Dependencies

[Architecture](01_System_Architecture.md), [testing](08_Testing_Plan.md), and subsystem architecture documents.

## References

IEEE 29148; [protocol](04_Communication_Protocol.md); [security](07_Security.md).

## Revision history

| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | MVP baseline |

## Table of contents

- [Requirements](#requirements)
- [User stories](#user-stories)
- [Acceptance scenarios](#acceptance-scenarios)
- [Traceability](#traceability)

## Requirements

| ID | Priority | Requirement | Acceptance criteria | Dependencies | Verification |
|---|---|---|---|---|---|
| FR-NN-001 | Must | Node shall send node ID, sequence, UTC GPS, battery, and status in an SN1 packet. | 100 valid bench packets decode to the defined schema. | Protocol | Unit/integration test |
| FR-NN-002 | Must | Node shall deep-sleep between reporting cycles. | Mean sleep current meets ≤150 µA hardware target. | Hardware | Meter test |
| FR-GW-001 | Must | Gateway shall reject malformed, duplicate, stale, or invalid-MAC packets. | Each invalid fixture creates no telemetry write. | Protocol, security | Integration test |
| FR-GW-002 | Must | Gateway shall persist unsent writes across reset. | Queue is uploaded after reconnect/reboot. | Backend | Fault-injection test |
| FR-CLD-001 | Must | System shall retain a current state and immutable telemetry history per net. | Valid upload atomically updates both paths. | Database | Emulator test |
| FR-SEC-001 | Must | Only organization members may read their organization’s nets. | Cross-org read/query is denied. | Auth/rules | Rules test |
| FR-WEB-001 | Must | Dashboard shall show a map, net list, net detail/history, and recovery action. | All views work at 320 px and desktop widths. | Dashboard | E2E test |
| FR-MOB-001 | Must | Mobile app shall allow sign-in, map/list/detail, cached viewing, and alert receipt. | Airplane-mode cached state is labeled stale. | Mobile | E2E test |
| NFR-REL-001 | Must | Reported location freshness shall be visible. | UI marks records stale after configured threshold (15 min). | Clients | UI test |
| NFR-SEC-001 | Must | Cloud traffic shall use TLS and no credential shall ship in a client repository. | Secret scan and transport inspection pass. | Deployment | Review/test |
| NFR-USA-001 | Should | Primary recovery action shall require at most three interactions from net detail. | Usability walkthrough passes. | UX | Observation |

## User stories

- As a skipper, I want to see my nets on a map so that I can navigate to the last known location.
- As a skipper, I want an alert when a net is stale or low battery so that I can intervene before it is lost.
- As a fleet owner, I want history and recovery status so that I can audit incidents.
- As a technician, I want device diagnostics so that I can isolate installation failures.
- As an administrator, I want organization-scoped access so that fleet locations remain private.

## Acceptance scenarios

**AC-REC-001 — recovery:** Given a signed-in organization member and a current net, when the user selects “Mark recovered” and confirms, then `status` becomes `recovered`, `recoveredAt` is server-timestamped, and an audit event is recorded.

**AC-OFF-001 — gateway outage:** Given Wi-Fi loss after a valid LoRa packet, when the gateway restarts and Wi-Fi returns, then the event is uploaded once and its identity is not duplicated.

## Traceability

| Objective | Requirements | Module | Tests |
|---|---|---|---|
| Locate nets | FR-NN-001, FR-GW-001, FR-CLD-001 | net-node, boat-gateway, backend | T-INT-001, T-FLD-001 |
| Operate long-term | FR-NN-002, NFR-REL-001 | hardware, firmware, clients | T-PWR-001, T-UI-004 |
| Protect fleet data | FR-SEC-001, NFR-SEC-001 | backend, clients | T-SEC-001..004 |
