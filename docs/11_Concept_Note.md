# SmartNet — Concept Note

| Field | Value |
|---|---|
| Document ID | SN-CN-001 |
| Version | 1.0 |
| Status | Draft |

## Purpose

Summarize the SmartNet project vision, architecture, requirements, procurement, and delivery plan in a single concept note for stakeholders, engineers, and pilot operators.

## Scope

One solar-powered Net Node, one boat-based gateway, Firebase-backed cloud and client apps, and a recovery workflow for small-scale fishing gear tracking.

## Problem Statement

Fishing nets and gear are vulnerable to loss, drift, and ghost fishing when they cannot be located quickly. The SmartNet MVP is designed to give fishers and fleet operators low-cost visibility into deployed nets, battery health, and recovery status using LoRa telemetry and a lightweight cloud dashboard.

## Vision and Mission

- Vision: No fishing net becomes invisible or unrecoverable due to lack of affordable telemetry.
- Mission: Provide small-scale fishing crews with a simple map-based way to locate deployed gear and react to missing movement, weak batteries, or stale reporting.

## Target Users

- Fisher / skipper: locate deployed gear and view current health.
- Fleet owner: review asset status and loss risk.
- Field technician: install, diagnose, and verify hardware.
- Administrator: onboard users, manage access, and protect tenant data.

## MVP Goals

- Deliver valid location, battery, and status telemetry from a solar-powered node.
- Use LoRa for node-to-gateway communication and Wi-Fi/cloud sync from the gateway.
- Provide dashboard and mobile views for map, list, history, and recovery actions.
- Keep prototype cost low and enforce safety and regulatory controls.

## System Architecture

- Net Node: ESP32-based LoRa telemetry device with GPS, battery sensing, solar charging, and low-power sleep behavior.
- Boat Gateway: ESP32-based LoRa receiver and Wi-Fi uplink device with local queueing and cloud ingestion.
- Cloud: Firebase Auth, Firestore, FCM, and server-side ingestion to persist current state, history, and alerts.
- Clients: React dashboard and Flutter mobile app reading tenant-scoped data and marking recovery.

## Architecture Decisions

- The gateway is the cloud writer so the node avoids embedded secrets and direct internet requirements.
- Firestore stores both current state and immutable telemetry history for efficient map queries and auditability.
- The LoRa protocol uses versioned SN1 payloads to support independent firmware evolution.
- Organization-scoped Firestore paths enforce tenant separation and least-privilege access.

## Requirements Summary

- Node sends packet fields including node ID, sequence, UTC GPS fix, battery, and status.
- Node deep-sleeps between reporting cycles and meets low sleep current targets.
- Gateway rejects malformed, duplicate, stale, or invalidly authenticated packets.
- Gateway persists unsent uploads across reset and replays once when connectivity returns.
- Cloud retains current state and event history, and clients support sign-in, map/list/detail, caching, and alerts.
- Security rules protect cross-tenant access and deny unauthorized data reads or writes.

## Communication Protocol

- LoRa packet payload is UTF-8 JSON limited to 220 bytes for MVP readability.
- Packet fields include version, type, node ID, sequence, timestamp, location, battery, flags, and MAC.
- Gateway validates schema, node identity, MAC, coordinates, sequence freshness, and replay.
- Timing is nominally every 10 minutes with retry windows at 30 seconds and 120 seconds while awake.

## Database and Cloud Model

- Firestore model includes organizations, members, nets, telemetry events, latest state, gateways, and audit trail.
- `organizations/{orgId}/nets/{netId}` holds current map state, while `telemetry` history records immutable events.
- Gateway metadata and audit events are captured separately.
- Composite indexes support status and recent-event queries while limiting free-tier load.

## API Design

- Gateway ingestion is exposed as a Firebase HTTPS function, authenticated by gateway bearer credentials.
- Batches of telemetry events are accepted with idempotent event IDs and server timestamps.
- Client operations use Firestore SDK and callable functions for recovery, registration, and alert acknowledgment.
- Error responses are structured, safe, and omit sensitive internal details.

## Security Architecture

- Assets include fleet location, membership, gateway credentials, device MAC keys, and telemetry integrity.
- Controls include organization-subcollection rules, HMAC-protected payloads, gateway credential rotation, and secret handling.
- Firestore rules require authenticated membership and prevent clients from writing telemetry or altering server-controlled fields.
- TLS is mandatory for all gateway/cloud and client/cloud traffic.

## Bill of Materials

### Node BOM

- ESP32-WROOM-32 DevKit — Ksh 2,500
- SX1276 LoRa 868 MHz module — Ksh 1,500
- NEO-6M GPS module — Ksh 1,000
- 3.7 V Li-ion battery ≥3000 mAh — Ksh 900
- TP4056 Type C charger — Ksh 200
- Solar panel: TBD
- Enclosure: predesigned
- Antennas/cable — Ksh 2,000

Node prototype subtotal: Ksh 8,100

### Gateway BOM

- ESP32 DevKit — Ksh 2,500
- SX1276 LoRa 868 MHz module — Ksh 1,500
- Power adapter — Ksh 1,000
- Enclosure: predesigned

Gateway prototype subtotal: Ksh 5,000

Total prototype cost estimate: Ksh 13,100

## Testing and Validation

- Unit tests for packet canonicalization, parsing, and firmware logic.
- Integration tests for packet delivery, rejection, gateway queueing, and cross-tenant rules.
- Hardware tests for sleep current, solar charge behavior, and waterproofing.
- UI tests for dashboard/mobile journeys, offline cache behavior, and stale-state labeling.
- Field tests for recovery trials and environmental resilience.

## Deployment and Operations

- Separate Firebase projects for development, staging, and pilot production.
- Staging deployment includes rules/functions/web preview and smoke tests before production release.
- Mobile distribution via pilot channel and firmware flashing over USB for MVP.
- Monitor gateway health, stale nets, quota usage, and battery alerts.

## Roadmap

- Phase 0: repository foundation, protocol fixtures, and dev environment.
- Phase 1: bench prototype with LoRa, GPS, and power measurement.
- Phase 2: cloud ingestion and client journeys.
- Phase 3: field validation with waterproofing and recovery tests.
- Phase 4: pilot deployment with operator acceptance.

### Future improvements

- Multi-gateway deduplication
- Encrypted LoRa payloads
- OTA signed firmware updates
- Cellular or satellite gateway backhaul
- Production solar charger and custom PCB
- Mesh/routing study and analytics
- Automated retention

## Conclusion

SmartNet is a focused MVP for low-cost deployed-net telemetry and recovery. The concept note ties product vision, architecture, security, and delivery into a single reference for project alignment and pilot planning.
