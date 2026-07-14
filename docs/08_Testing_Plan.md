# SmartNet — Testing and Validation Plan

| Field | Value |
|---|---|
| Document ID | SN-TST-001 |
| Version | 1.0 |
## Purpose
Define evidence required to release the MVP pilot.
## Scope
Unit, integration, system, field, security, hardware, and acceptance testing.
## Audience
QA, developers, field technicians, product owner.
## Dependencies
[Requirements](02_Requirements.md), all subsystem architecture documents.
## References
IEEE 29119 concepts; Firebase Emulator Suite documentation.
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | Initial test baseline |
## Table of contents
- [Strategy](#strategy)
- [Test matrix](#test-matrix)
- [Exit criteria](#exit-criteria)

## Strategy

Automate deterministic logic and rules; execute hardware-in-loop and field tests with recorded environmental conditions. Each defect receives severity, reproduction steps, firmware/build version, and evidence. Production-like Firebase must never be the first environment for rules or destructive test data.

## Test matrix

| ID | Layer | Test | Pass criterion | Requirements |
|---|---|---|---|---|
| T-UNIT-001 | Firmware | GPS/parser and packet canonicalization | All valid/invalid fixtures match expected result | FR-NN-001 |
| T-INT-001 | Radio | Node→gateway valid, corrupt, replay, and weak signal packets | Valid persisted once; invalid rejected | FR-NN-001, FR-GW-001 |
| T-INT-002 | Cloud | Gateway queue across Wi-Fi loss/reboot | Event uploaded once after reconnect | FR-GW-002 |
| T-SEC-001 | Rules | Cross-org read/write/query attempts | All denied | FR-SEC-001 |
| T-PWR-001 | Hardware | Sleep current and solar-charge profile | Meets design target / no unsafe charge behavior | FR-NN-002 |
| T-UI-001 | Web/mobile | Map/list/detail/recovery responsive journeys | Critical flow passes keyboard and touch use | FR-WEB-001, FR-MOB-001 |
| T-UI-004 | Clients | Offline cache and stale labels | No cached value represented as current | NFR-REL-001 |
| T-FLD-001 | System | Three buoy recovery trials | ≥90% valid fixes; location within 30 m CEP | Success metrics |
| T-ENV-001 | Hardware | 24 h immersion plus post-test radio check | No ingress or function loss | Hardware |

## Exit criteria

All Must requirements pass; no open critical/high security defect; recovery test meets target; security-rule suite has 100% required allow/deny cases; release candidate versions and test evidence are archived. A failed waterproofing, unsafe charging, or unauthorized access test blocks field deployment.
