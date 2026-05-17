# Mission Telemetry Analytics Readiness Validation

---

## Purpose

Validate that the per-mission telemetry YAML layer is ready for Issue #35 statistics and analytics consumption.

This document confirms what can be calculated from the current telemetry records, what remains incomplete, and what the prompt should preserve during future mission closeout workflows.

---

## Validated Data Source

Authoritative telemetry files:

`/operations/current/data/missions/YYYY/MISSION_ID.yaml`

Current validated records:

- `/operations/current/data/missions/2026/MISSION_001_KPNS_KMTH.yaml`
- `/operations/current/data/missions/2026/MISSION_002_KMTH_KAPF.yaml`
- `/operations/current/data/missions/2026/MISSION_003_KAPF_KFXE.yaml`
- `/operations/current/data/missions/2026/MISSION_004_KFXE_KEYW.yaml`
- `/operations/current/data/missions/2026/MISSION_005_KEYW_MYAM.yaml`

Schema reference:

`/operations/current/templates/mission-telemetry-schema.md`

Closeout workflow reference:

`/operations/current/templates/mission-telemetry-closeout-workflow.md`

---

## Schema Readiness

The current mission telemetry records include the canonical schema sections required for analytics consumption:

- `schema_version`
- `mission_id`
- `mission_number`
- `date`
- `aircraft`
- `route`
- `timing`
- `fuel`
- `payload`
- `operations`
- `client_refs`
- `source_files`

All current mission telemetry records are structured consistently enough for Issue #35 to consume without scraping narrative markdown.

---

## Distance Backfill Status

Distance telemetry has been backfilled for Missions 001–005 using pilot-provided / planner-supported values.

| Mission | Distance NM |
|---|---:|
| `MISSION_001_KPNS_KMTH` | 550 |
| `MISSION_002_KMTH_KAPF` | 181 |
| `MISSION_003_KAPF_KFXE` | 87 |
| `MISSION_004_KFXE_KEYW` | 160 |
| `MISSION_005_KEYW_MYAM` | 283 |

Mission 002 route correction:

`WINNER` was corrected to `WNNER` in telemetry and mission markdown.

---

## Analytics Capability Matrix

| Metric / Query | Current Readiness | Notes |
|---|---|---|
| Mission count | Ready | Count telemetry YAML records |
| Mission list by date | Ready | Use `date` and `mission_number` |
| Aircraft utilization by tail number | Ready | Use `aircraft.tail_number` |
| Mission type breakdown | Ready | Use `operations.mission_type` |
| Flight rules breakdown | Ready | Use `operations.flight_rules` |
| Success rate | Ready | Use `operations.result` |
| Go-around totals | Ready | Use `operations.go_arounds` |
| Diversion totals | Ready | Use `operations.diversions` |
| International operation count | Ready | Use `operations.international` |
| Airport activity | Ready | Use `route.departure` and `route.arrival` |
| Passenger totals | Ready | Use `payload.passengers` |
| Total distance flown | Ready | Use `route.distance_nm` |
| Average mission distance | Ready | Use `route.distance_nm` |
| Passenger miles | Ready | Use `route.distance_nm` and `payload.passengers` |
| Known cargo totals | Partially ready | Sum only non-null `payload.cargo_lb`; report incomplete data where null exists |
| Total fuel used | Partially ready | Sum only non-null `fuel.used_gal`; Mission 003 currently has `null` fuel used |
| Average fuel burn | Partially ready | Requires known `fuel.used_gal` and timing values; nulls must be excluded or reported |
| Average mission duration | Partially ready | Some missions have null timing durations |
| NM per gallon | Partially ready | Distance is available, but Mission 003 has `fuel.used_gal: null` |
| Cargo efficiency metrics | Partially ready | Distance is available, but some cargo values are `null` |

---

## Known Data Gaps

The telemetry layer is architecturally ready, but current backfilled data still has some gaps from historical source records.

### Fuel Used

Mission 003 has `fuel.used_gal: null` because fuel used was not explicitly recorded.

Impact:

- total fuel used can be reported as known total with incomplete-data warning
- full career fuel total remains incomplete until the value is supported
- NM per gallon can be calculated for missions with both distance and fuel used, but career-wide NM per gallon should include an incomplete-data caveat

Recommended handling:

- do not derive fuel used from departure and arrival fuel unless explicitly approved as a correction
- future PIREPs should capture fuel used directly

### Cargo / Baggage Weight

Some missions have `payload.cargo_lb: null` where records contain descriptive baggage language instead of numeric payload data.

Impact:

- total cargo transported can be reported as known cargo total with incomplete-data warning
- cargo efficiency metrics remain incomplete where cargo is missing

Recommended handling:

- future PIREPs should capture cargo/baggage in pounds
- descriptive baggage text should not be treated as numeric telemetry

### Timing

Some missions lack complete UTC event timestamps and duration values.

Impact:

- average duration metrics are partially available
- timing-based analytics should exclude null values or report incomplete data

Recommended handling:

- future PIREPs should capture OUT / OFF / TOC / TOD / ON / IN / shutdown UTC timestamps when available
- block time and airborne time should be captured as numeric decimal hours

---

## Client Reference Validation

Telemetry records use `client_refs` to link to `/operations/current/data/clients.yaml`.

Validated references:

- Mission 001: `client_00001`, `client_00002`
- Mission 002: `client_00001`, `client_00002`
- Mission 003: `client_00003`
- Mission 004: `client_00004`, `client_00005`
- Mission 005: `client_00006`, `client_00007`

Rules:

- mission telemetry references client records by ID only
- client continuity details remain in `clients.yaml`
- do not duplicate client profiles inside telemetry YAML

---

## Source File Traceability

Each telemetry file includes a `source_files` section pointing back to the source mission markdown file.

Current debrief references are `null` where debrief file references are not explicitly linked in telemetry.

Future closeout should populate `source_files.debrief` when the debrief file exists and is known.

---

## Analytics Contract for Issue #35

Issue #35 should consume mission telemetry YAML using the following principles:

- treat telemetry YAML as authoritative for raw mission facts
- calculate derived statistics from YAML fields
- do not scrape narrative markdown for metrics already represented in telemetry YAML
- handle `null` values explicitly
- report incomplete metrics honestly rather than silently dropping missing data without notice
- distinguish between fully available metrics, partially available metrics, and unavailable metrics

Recommended output behavior:

- fully supported metrics can be reported normally
- partially supported metrics should include known totals plus an incomplete-data note
- unsupported metrics should state which source fields are missing

---

## Prompt Contract for Issue #39

Issue #39 should update the FlightOps prompt so future mission closeout workflows:

- create or update telemetry YAML for completed missions
- capture distance, fuel, cargo, timing, and operational counts during PIREP intake when available
- preserve unsupported values as `null`, `[]`, or `unknown`
- add `client_refs` only when known clients match `clients.yaml`
- populate `source_files.mission` and `source_files.debrief` when available
- use telemetry YAML for statistics queries
- avoid deriving unsupported metrics from narrative text or route strings without explicit approval

---

## Follow-Up Gaps

No blocking follow-up task is required for Issue #37 or Issue #35 distance readiness.

Potential future follow-up if desired:

- backfill Mission 003 `fuel.used_gal` only if pilot-confirmed or supported by a source update
- add debrief file references to `source_files.debrief` if debrief paths are standardized

These are data completeness improvements, not blockers for telemetry analytics readiness.
