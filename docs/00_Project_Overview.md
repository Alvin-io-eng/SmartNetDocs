# SmartNet — Project Overview

| Field | Value |
|---|---|
| Document ID | SN-OV-001 |
| Version | 1.0 |
| Status | Approved MVP baseline |

## Purpose

Define the product vision, MVP boundary, stakeholders, success measures, and operating assumptions.

## Scope

One solar Net Node, one Boat Gateway, Firebase-backed web and mobile monitoring, and a recovery workflow.

## Audience

Product, engineering, operations, QA, and pilot fishermen.

## Dependencies

[Architecture](01_System_Architecture.md), [requirements](02_Requirements.md), and the [BOM](03_Bill_of_Materials.md).

## References

IEEE 29148 requirements practice; Firebase documentation; local radio regulations applicable to the deployment region.

## Revision history

| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | Initial release |

## Table of contents

- [Vision and mission](#vision-and-mission)
- [Problem and goals](#problem-and-goals)
- [Stakeholders and personas](#stakeholders-and-personas)
- [Scope](#scope-1)
- [Success metrics](#success-metrics)
- [Risks](#risks)

## Vision and mission

**Vision:** no fishing net becomes invisible or unrecoverable due to lack of affordable telemetry. **Mission:** give small-scale fishing crews a simple map-based way to locate deployed gear and react to missing movement, weak batteries, or stale reporting.

## Problem and goals

Lost or drifting nets cause economic loss and ghost fishing. The MVP shall identify a net, show its last known location and status, and retain sufficient history to guide recovery. Goals: low prototype cost, daylight energy autonomy, point-to-point LoRa operation, and usable operation on intermittent internet.

## Stakeholders and personas

| Stakeholder/persona | Need | Success outcome |
|---|---|---|
| Fisher / skipper | Find deployed gear quickly | Last known position and health are visible |
| Fleet owner | Manage assets and loss risk | Owns nets and can review history |
| Field technician | Install and diagnose hardware | Clear identifier, logs, and test mode |
| Administrator | Onboard users and protect data | Tenant-isolated fleet data |

## Scope

In scope: GPS fixes, battery telemetry, LoRa uplink, gateway cloud sync, Firebase sign-in, map/list/history views, recovery marking, and push alerts. Out of scope: automatic winches, fish analytics, satellite or cellular gateway backhaul, paid subscriptions, custom production PCB, and multiple gateway routing.

## Assumptions and constraints

- The node has sky view periodically and floats with the net.
- The boat gateway has Wi-Fi when synchronization is required.
- LoRa frequency, output power, duty cycle, and antenna choices must comply with local law.
- Firebase Spark/free-tier quotas constrain polling, retention, and writes.

## Success metrics

| Metric | MVP target | Measurement |
|---|---:|---|
| Valid fix delivery | ≥90% during field route | Sent vs. persisted test packet count |
| Location freshness | ≤15 min while gateway is in range | Firestore timestamps |
| Recovery workflow | Locate test buoy within 30 m CEP | Three controlled trials |
| Node endurance | 7 days under agreed duty cycle | Field battery log |
| Unauthorized read/write | 0 permitted rule tests | Firebase emulator suite |

## Risks

| Risk | Mitigation |
|---|---|
| GPS/LoRa obstruction | Externalized antenna, retry queue, last-good fix |
| Water ingress | IP67 enclosure, glands, soak test |
| Battery depletion | Solar budget, low-power states, battery alerts |
| Cloud outage | Gateway queue and offline client cache |
| Misuse of location data | Owner-scoped rules and least-privilege access |
