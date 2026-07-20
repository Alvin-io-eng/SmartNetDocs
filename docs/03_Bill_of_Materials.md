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
| Item | Qty | Specification / approved alternative | Price | Acceptance |
|---|---:|---|---:|---|
| MCU | 1 | ESP32-WROOM-32 DevKit | Ksh 2,500 | Boot and deep-sleep test |
| LoRa | 1 | SX1276 LoRa 868 MHz long-range wireless RF | Ksh 1,500 | SPI receive/transmit range test |
| GPS | 1 | NEO-6M or current low-power equivalent | Ksh 1,000 | UTC and position lock outdoors |
| Battery | 1 | Protected 3.7 V Li-ion, ≥3000 mAh | Ksh 900 | Capacity/thermal inspection |
| Charger | 1 | TP4056 Type C with protection | Ksh 200 | Charge termination test |
| Solar panel | 1 | TBD, we will review the specific panel requirements | | Charge current in sunlight |
| Enclosure | 1 | Must be predesigned | | 24 h immersion test |
| Antennas/cable | 2 | Matched 50 Ω LoRa/GPS parts | Ksh 2,000 | VSWR/continuity check |
| **Node subtotal** |  |  | **Ksh 8,100** |  |

## Gateway BOM
| Item | Qty | Specification | Price | Acceptance |
|---|---:|---|---:|---|
| MCU | 1 | ESP32 DevKit | Ksh 2,500 | Wi-Fi and reset test |
| LoRa module | 1 | SX1276 LoRa 868 MHz long-range wireless RF | Ksh 1,500 | Packet receive test |
| Power | 1 | 5 V USB/boat adapter, fused | Ksh 1,000 | Brownout test |
| Enclosure | 1 | Must be predesigned | | Cable strain-relief inspection |
| **Gateway subtotal** |  |  | **Ksh 5,000** |  |

## Total prototype cost estimate
| Scope | Estimated cost |
|---|---:|
| Node prototype components | Ksh 8,100 |
| Gateway prototype components | Ksh 5,000 |
| Combined node + gateway total | Ksh 13,100 |

## Procurement controls

Do not substitute radio frequency, battery chemistry, or charger/protection topology without electrical review. Keep lot, supplier, module revision, and measured current in the build record. The TP4056 is suitable only for a supervised prototype; production solar charging requires a charger designed for panel input and temperature-safe Li-ion charging.
