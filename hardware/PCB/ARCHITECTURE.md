# SmartNet PCB Architecture

| Field | Value |
|---|---|
| Version | 1.0 |
## Purpose
Define the post-prototype EasyEDA PCB design rules and interfaces.
## Scope
Net Node PCB only; breadboard/module prototype remains the MVP implementation.
## Audience
PCB engineer, hardware reviewer, manufacturer.
## Dependencies
[Schematics](../Schematics/ARCHITECTURE.md), [CAD](../CAD/ARCHITECTURE.md).
## References
ESP32/SX127x reference layouts and PCB fab capabilities.
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | Production-readiness baseline |
## Table of contents
- [Stack-up](#stack-up)
- [Placement and routing](#placement-and-routing)
- [Manufacturing](#manufacturing)

## Stack-up

Use a 2-layer FR-4 board only after EMI/radio review: 1.6 mm nominal, 1 oz copper, continuous ground reference wherever possible. A four-layer board is preferred if RF, switching noise, or return paths cannot be controlled. Define board outline, mounting holes, keep-outs, fab tolerances, test pads, fiducials, and panelization in EasyEDA outputs.

## Placement and routing

Keep RF modules and antenna connectors at board edge; reserve their vendor keep-out zones. Keep GPS and LoRa antennas physically separated and away from battery, solar wires, and switching/charger currents. Place battery/charger protection near connector, use appropriately wide power traces, local decoupling at every IC, and a solid low-impedance ground. Avoid routing under RF antenna regions. Label all connectors and provide programming UART/test pads.

## Manufacturing

Release Gerber, drill, pick-and-place, BOM, assembly drawing, revision, and electrical test procedure as one controlled package. Inspect polarity/orientation, shorts, current draw, radio operation, and charging before enclosure assembly. No production run proceeds without DRC/ERC review and a first-article functional test.
