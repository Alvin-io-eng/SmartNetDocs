# SmartNet CAD Architecture

| Field | Value |
|---|---|
| Version | 1.0 |
## Purpose
Define the buoy enclosure and mechanical assembly for the prototype.
## Scope
SolidWorks assembly, enclosure, antenna mounts, seals, service access; PCB design excluded.
## Audience
Mechanical engineer, fabricator, field technician.
## Dependencies
[BOM](../../docs/03_Bill_of_Materials.md), [schematics](../Schematics/ARCHITECTURE.md).
## References
Component drawings; IP67 test methods.
## Revision history
| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-07-14 | Prototype constraints |
## Table of contents
- [Constraints](#constraints)
- [Assembly](#assembly)
- [Validation](#validation)

## Constraints

Provide positive buoyancy with a 2× mass margin, UV-resistant exterior, smooth net attachment point, protected solar face, and accessible sealed charging/service port. Maintain antenna separation from battery/ground planes and keep GPS antenna sky-facing. CAD must specify dimensions, mass, center of buoyancy, material, O-ring/gland interfaces, tolerances, and revision in drawing title blocks.

## Assembly

Use a two-part IP67-rated enclosure or equivalent gasketed design; route cables through rated glands with strain relief. Isolate sharp hardware from cells, provide drainage-free sealed internal volume, and avoid trapped water paths. Thermal expansion, solar heating, and pressure changes shall not compromise seal or battery limits.

## Validation

Weigh assembled unit, verify float attitude/solar exposure, perform 24 h immersion and post-immersion radio/GPS tests, inspect seals after every service opening, and document replacement intervals for seals, cable glands, and battery.
