# SmartNet — Bill of Materials

| Field | Value |
|---|---|
| Document ID | SN-BOM-001 |
| Version | 1.0 |

## Purpose

Specify prototype procurement, alternatives, and acceptance checks.
## Scope
One Net Node and one Boat Gateway prototype; prices are indicative and must be quoted before purchase.
## Audience
Hardware engineer, procurement, and field technician.
## Dependencies
[Hardware architecture](../hardware/Schematics/ARCHITECTURE.md), [protocol](04_Communication_Protocol.md).
## References
Manufacturer datasheets and regional radio approvals.
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | Prototype BOM |
## Table of contents
- [Node BOM](#node-bom)
- [Gateway BOM](#gateway-bom)
- [Procurement controls](#procurement-controls)

## Node BOM
| Item | Qty | Specification / approved alternative | Acceptance |
|---|---:|---|---|
| MCU | 1 | ESP32-WROOM-32 DevKit | Boot and deep-sleep test |
| LoRa | 1 | SX1276/78 module, legal regional band | SPI receive/transmit range test |
| GPS | 1 | NEO-6M or current low-power equivalent | UTC and position lock outdoors |
| Battery | 1 | Protected 3.7 V Li-ion, ≥3000 mAh | Capacity/thermal inspection |
| Charger | 1 | TP4056 with protection, USB input | Charge termination test |
| Solar panel | 1 | 5–6 V, ≥1 W | Charge current in sunlight |
| Enclosure | 1 | UV-stable floating IP67 enclosure | 24 h immersion test |
| Antennas/cable | 2 | Matched 50 Ω LoRa/GPS parts | VSWR/continuity check |

## Gateway BOM
| Item | Qty | Specification | Acceptance |
|---|---:|---|---|
| MCU | 1 | ESP32 DevKit | Wi-Fi and reset test |
| LoRa module/antenna | 1/1 | Same regional configuration as node | Packet receive test |
| Power | 1 | 5 V USB/boat adapter, fused | Brownout test |
| Enclosure | 1 | Splash-resistant service enclosure | Cable strain-relief inspection |

## Procurement controls

Do not substitute radio frequency, battery chemistry, or charger/protection topology without electrical review. Keep lot, supplier, module revision, and measured current in the build record. The TP4056 is suitable only for a supervised prototype; production solar charging requires a charger designed for panel input and temperature-safe Li-ion charging.
