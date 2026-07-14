# SmartNet Engineering Blueprint

| Field | Value |
|---|---|
| Status | MVP implementation baseline |
| Version | 1.0 |
| Last updated | 2026-07-14 |

## Purpose

SmartNet is a low-cost fishing-net tracking platform. A solar buoy Net Node sends GPS and health telemetry over LoRa to a Boat Gateway, which synchronizes authorized data to Firebase for web and mobile recovery workflows.

## Scope

This repository is the blueprint for a one-node, one-gateway MVP: firmware, prototype hardware, Firebase backend, React dashboard, and Flutter application. Satellite backhaul, custom production hardware, and multi-gateway routing are future work.

## Audience

Engineering, QA, hardware fabricators, pilot operators, and product stakeholders.

## Dependencies

ESP32 Arduino toolchain, Firebase, LoRa/GPS modules, React/Vite, Flutter, and approved regional radio configuration.

## References

[Project overview](docs/00_Project_Overview.md) · [System architecture](docs/01_System_Architecture.md) · [Requirements](docs/02_Requirements.md)

## Revision history

| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | Initial MVP engineering baseline |

## Table of contents

- [Repository layout](#repository-layout)
- [Implementation order](#implementation-order)
- [Documentation map](#documentation-map)

## Repository layout

```text
SmartNet/
├── docs/                 # Cross-system specifications
├── backend/              # Firebase configuration and functions
├── dashboard/            # React web application
├── mobile/               # Flutter mobile application
├── firmware/{net-node,boat-gateway}/
└── hardware/{CAD,PCB,Schematics}/
```

## Implementation order

1. Bench-test Node and Gateway against the packet contract.
2. Provision Firebase Authentication, Firestore rules, indexes, and data model.
3. Implement gateway ingestion, then dashboard and mobile read paths.
4. Pass field, endurance, security-rule, and recovery acceptance tests before pilot deployment.

## Documentation map

`docs/` contains authoritative system contracts. Each subsystem `ARCHITECTURE.md` defines its build boundary. Conflicts resolve in favor of [requirements](docs/02_Requirements.md), then [system architecture](docs/01_System_Architecture.md).
