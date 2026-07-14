# SmartNet — Communication Protocol

| Field | Value |
|---|---|
| Document ID | SN-PRO-001 |
| Version | SN1 / 1.0 |

## Purpose
Define the wire contract between Net Node and Boat Gateway.
## Scope
LoRa telemetry only; cloud API is in [API design](06_API_Design.md).
## Audience
Firmware, backend, QA.
## Dependencies
[Security](07_Security.md), [firmware architectures](../firmware/net-node/ARCHITECTURE.md).
## References
SX127x datasheet; RFC 2104 (HMAC).
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | SN1 baseline |
## Table of contents
- [Packet](#packet)
- [Validation](#validation)
- [Timing](#timing)

## Packet

Payload is UTF-8 JSON for MVP readability, maximum 220 bytes. Radio configuration is an environment-specific provisioned profile; node and gateway must match frequency, bandwidth, spreading factor, coding rate, sync word, and TX power.

```json
{"v":1,"t":"telemetry","nodeId":"NN-0001","seq":42,"ts":1760000000,"latE7":-123456789,"lonE7":361234567,"accCm":850,"batMv":3980,"flags":3,"mac":"base64url-hmac"}
```

| Field | Type | Rule |
|---|---|---|
| `v` | integer | exactly `1` |
| `seq` | uint32 | monotonically increasing per node boot; persisted periodically |
| `ts` | epoch seconds | ±24 h of gateway time unless gateway time unknown |
| `latE7`,`lonE7` | int32 | WGS84 degrees × 10⁷; lat ±90, lon ±180 |
| `flags` | uint8 | bit 0 GPS valid, bit 1 low battery, bit 2 retry |
| `mac` | string | HMAC-SHA256 truncated to 16 bytes over canonical JSON without `mac` |

## Validation

Gateway rejects unknown node IDs, schema violations, MAC failure, impossible coordinates, sequence replay, and packets older than the per-node accepted window. It records rejection reason locally without uploading raw secrets. A valid packet yields event key `{gatewayId}:{nodeId}:{bootId?}:{seq}`; SN1 shall add `bootId` before production deployment to eliminate reset ambiguity.

## Timing

Default interval: 10 minutes; retry at 30 s and 120 s only while awake. Gateway sends no downlink in MVP. Maximum duty cycle and airtime must remain within regional regulation. Receiver RSSI/SNR are gateway metadata, not node claims.
