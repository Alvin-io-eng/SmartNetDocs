# SmartNet Boat Gateway Firmware Architecture

| Field | Value |
|---|---|
| Version | 1.0 |
| Owner | Embedded/IoT engineering |
## Purpose
Implement an ESP32 LoRa receiver that securely relays telemetry to Firebase over Wi-Fi.
## Scope
Radio reception, validation, local queue, Wi-Fi/TLS upload, diagnostics.
## Audience
Embedded, backend, QA.
## Dependencies
[Protocol](../../docs/04_Communication_Protocol.md), [API](../../docs/06_API_Design.md), [security](../../docs/07_Security.md).
## References
ESP32 NVS/Wi-Fi documentation and Firebase HTTPS endpoint contract.
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | MVP behavior |
## Table of contents
- [Structure](#structure)
- [Pipeline](#pipeline)
- [Resilience](#resilience)

## Structure
```text
firmware/boat-gateway/src/{main.cpp,radio.cpp,validator.cpp,queue.cpp,wifi.cpp,ingest.cpp,diagnostics.cpp}/
```
## Pipeline
`receive ISR/task → length/schema/MAC/sequence validation → dedupe → append durable queue → HTTPS batch upload → server acknowledgement → remove acknowledged item`. Capture gateway receive timestamp, RSSI, and SNR. All cloud writes must target the ingestion endpoint; firmware has no general Firestore write access.

## Resilience

On boot, load device identity/config and replay queue before accepting noncritical work. Wi-Fi reconnect uses capped exponential backoff with jitter; TLS certificate validation is mandatory. Queue is CRC-protected and bounded; if full, preserve newest state per node, report overflow diagnostic, and never silently claim delivery. Watchdog monitors network and receive tasks. Serial diagnostics redact credentials and MAC values. USB flashing is the MVP update method; future OTA requires signed images, staged rollout, rollback, and power-safe update policy.
