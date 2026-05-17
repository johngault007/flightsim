# Mission Statistics Calculation Rules

---

## Purpose

Define how Lost Bay Aviation mission statistics and analytics should be calculated from structured mission telemetry.

This document establishes the source-of-truth model, metric calculation rules, and boundaries between raw telemetry, derived statistics, and human-readable summaries.

---

## Source of Truth

Authoritative raw mission telemetry lives in per-mission YAML files:

`/operations/current/data/missions/YYYY/MISSION_ID.yaml`

Example:

`/operations/current/data/missions/2026/MISSION_005_KEYW_MYAM.yaml`

Telemetry YAML is authoritative for raw mission facts used by statistics and analytics.

Mission and debrief markdown files remain the human-readable narrative and historical records. They should not be scraped for analytics when the same metric is represented in telemetry YAML.

---

## Source-of-Truth Hierarchy

| Data Type | Authoritative Source | Human-Readable Layer |
|---|---|---|
| Mission telemetry | `/operations/current/data/missions/YYYY/MISSION_ID.yaml` | mission markdown / debrief markdown |
| Derived statistics | calculated from mission telemetry YAML | `/operations/current/career-state/statistics.md` or generated summary |
| Client continuity | `/operations/current/data/clients.yaml` | `/operations/current/career-state/clients-summary.md` |
| Narrative mission context | mission/debrief markdown | same files |
| Company identity | `/operations/current/career-state/company-profile.md` | same file |

Rules:

- Raw mission facts live in telemetry YAML.
- Derived statistics are calculated from telemetry YAML.
- Human-readable statistics summaries may exist, but they are not authoritative data stores.
- Mission/debrief markdown remains narrative and historical, not the primary analytics source.
- Client references in mission telemetry link to `clients.yaml`; client details should not be duplicated in statistics.

---

## Persistence Decision

Derived statistics should not be manually persisted as authoritative values unless a future caching/reporting strategy explicitly approves it.

Current approved behavior:

- Store raw telemetry in per-mission YAML.
- Calculate aggregate statistics from telemetry YAML.
- Allow human-readable summaries for operational convenience.
- Treat generated or manually refreshed summaries as secondary/reference-only.

Not approved by this task:

- manually maintaining hidden career total values
- treating `statistics.md` as the source of truth
- storing aggregate totals inside individual mission telemetry files
- scraping narrative markdown when telemetry YAML exists

If cached aggregate statistics are introduced later, the cache must declare:

- source telemetry range
- generation timestamp
- generation method
- cache ownership rules
- invalidation/update behavior

---

## Telemetry Records Included

Statistics should consume all relevant mission telemetry records matching:

```text
/operations/current/data/missions/YYYY/*.yaml
```

For current v2.2 work, validated records include:

- `/operations/current/data/missions/2026/MISSION_001_KPNS_KMTH.yaml`
- `/operations/current/data/missions/2026/MISSION_002_KMTH_KAPF.yaml`
- `/operations/current/data/missions/2026/MISSION_003_KAPF_KFXE.yaml`
- `/operations/current/data/missions/2026/MISSION_004_KFXE_KEYW.yaml`
- `/operations/current/data/missions/2026/MISSION_005_KEYW_MYAM.yaml`

Future statistics should include additional years and missions unless the user asks for a specific date range, aircraft, region, or subset.

---

## General Calculation Rules

- Use telemetry YAML fields as the primary data source.
- Treat `null` as unknown, not zero.
- Treat `unknown` as unknown, not as a countable category unless explicitly analyzing unknowns.
- Treat empty lists as no supported entries.
- Sum only supported numeric values.
- Report incomplete-data caveats when source fields are missing for one or more missions.
- Do not infer missing values from route strings, prose, or narrative unless explicitly pilot-confirmed or source-supported.
- Use mission count denominators carefully: some averages should use all missions, while others should use only missions with supported values.

---

## Core Metric Calculation Rules

### Mission Count

Field source:

- telemetry file count

Calculation:

```text
mission_count = count(mission telemetry YAML records)
```

Notes:

- Count one record per mission.
- Exclude non-mission schema/template files.

---

### Total Distance Flown

Field source:

- `route.distance_nm`

Calculation:

```text
total_distance_nm = sum(route.distance_nm for missions where route.distance_nm is not null)
```

Notes:

- Distance must be numeric nautical miles.
- If any mission has `route.distance_nm: null`, report a known total with an incomplete-data caveat.
- Do not calculate distance from route strings unless explicitly approved.

---

### Average Mission Distance

Field source:

- `route.distance_nm`

Preferred calculation when all missions have distance:

```text
average_mission_distance_nm = total_distance_nm / mission_count
```

Fallback calculation when some missions lack distance:

```text
known_average_mission_distance_nm = sum(known route.distance_nm) / count(missions with known route.distance_nm)
```

Notes:

- If calculated from only known values, label it as average known mission distance.
- Do not silently exclude unknown distances without warning.

---

### Total Fuel Used

Field source:

- `fuel.used_gal`

Calculation:

```text
total_fuel_used_gal = sum(fuel.used_gal for missions where fuel.used_gal is not null)
```

Notes:

- Fuel must be numeric gallons.
- If any mission has `fuel.used_gal: null`, report the result as known fuel used with an incomplete-data caveat.
- Do not derive fuel used from departure and arrival values unless explicitly pilot-confirmed or source-supported.

---

### Average Fuel Burn

Field sources:

- `fuel.used_gal`
- `timing.airborne_time_hr` or `timing.block_time_hr`

Preferred airborne calculation:

```text
average_airborne_fuel_burn_gph = sum(fuel.used_gal) / sum(timing.airborne_time_hr)
```

Alternative block-time calculation:

```text
average_block_fuel_burn_gph = sum(fuel.used_gal) / sum(timing.block_time_hr)
```

Rules:

- Use only missions where both fuel used and the selected time field are known.
- Label whether the calculation uses airborne time or block time.
- Include incomplete-data caveats when missions are excluded.

---

### Nautical Miles Per Gallon

Field sources:

- `route.distance_nm`
- `fuel.used_gal`

Calculation:

```text
nm_per_gallon = sum(route.distance_nm) / sum(fuel.used_gal)
```

Rules:

- Use only missions where both distance and fuel used are known.
- Include incomplete-data caveats when missions are excluded.
- Do not calculate using missions with `fuel.used_gal: null`.

---

### Passenger Totals

Field source:

- `payload.passengers`

Calculation:

```text
total_passengers = sum(payload.passengers for missions where payload.passengers is not null)
```

Notes:

- Passenger count should be numeric.
- If passenger count is unknown for any mission, report known total with an incomplete-data caveat.

---

### Passenger Miles

Field sources:

- `payload.passengers`
- `route.distance_nm`

Calculation:

```text
passenger_miles = sum(payload.passengers * route.distance_nm for missions where both values are known)
```

Notes:

- Use nautical passenger-miles unless otherwise specified.
- If either passengers or distance is missing for a mission, exclude that mission and report a caveat.

---

### Cargo Totals

Field source:

- `payload.cargo_lb`

Calculation:

```text
total_cargo_lb = sum(payload.cargo_lb for missions where payload.cargo_lb is not null)
```

Notes:

- Cargo/baggage must be numeric pounds.
- Descriptive baggage text is not numeric telemetry.
- If cargo is unknown for any mission, report known cargo transported with an incomplete-data caveat.

---

### Cargo Efficiency

Field sources:

- `payload.cargo_lb`
- `route.distance_nm`

Possible calculation:

```text
cargo_lb_nm = sum(payload.cargo_lb * route.distance_nm for missions where both values are known)
```

Notes:

- This is pound-nautical-miles, useful as a workload/transport metric.
- If cargo or distance is missing, exclude that mission and report a caveat.

---

### Mission Success Rate

Field source:

- `operations.result`

Calculation:

```text
success_rate = count(operations.result == "success") / count(missions with known operations.result)
```

Rules:

- Use known result values only.
- Treat `unknown` as unknown, not failure.
- Report unknown result count if present.

---

### Go-Around Totals

Field source:

- `operations.go_arounds`

Calculation:

```text
total_go_arounds = sum(operations.go_arounds for missions where operations.go_arounds is not null)
```

Notes:

- Use numeric counts.
- If unknown, report caveat rather than treating unknown as zero.

---

### Diversion Totals

Field source:

- `operations.diversions`

Calculation:

```text
total_diversions = sum(operations.diversions for missions where operations.diversions is not null)
```

Notes:

- Use numeric counts.
- If unknown, report caveat rather than treating unknown as zero.

---

### Mission Type Breakdown

Field source:

- `operations.mission_type`

Calculation:

```text
mission_type_breakdown = count by operations.mission_type
```

Notes:

- Use lowercase snake_case values from telemetry YAML.
- Include `unknown` only if unknown values are present and relevant.

---

### Flight Rules Breakdown

Field source:

- `operations.flight_rules`

Calculation:

```text
flight_rules_breakdown = count by operations.flight_rules
```

Notes:

- Use normalized telemetry values such as `vfr`, `ifr`, or `vfr_with_flight_following`.

---

### Airport Activity

Field sources:

- `route.departure`
- `route.arrival`

Possible calculations:

```text
departures_by_airport = count by route.departure
arrivals_by_airport = count by route.arrival
airport_activity = departures + arrivals by airport
```

Notes:

- Use ICAO identifiers.
- Keep departure and arrival counts available separately when useful.

---

### International Operations

Field source:

- `operations.international`

Calculation:

```text
international_operations = count(operations.international == true)
domestic_operations = count(operations.international == false)
```

Notes:

- If `operations.international` is `null`, report unknown international status count.

---

## Current Readiness Alignment

This calculation model aligns with the completed telemetry work:

- Issue #37 created the canonical telemetry architecture.
- Issue #47 defined the mission telemetry YAML schema.
- Issue #48 backfilled Missions 001–005 telemetry records.
- Issue #49 documented telemetry closeout behavior.
- Issue #50 validated analytics readiness.
- Issue #55 backfilled route distance values for Missions 001–005.

Distance-based metrics are now available for Missions 001–005.

Remaining known partial areas:

- Mission 003 has `fuel.used_gal: null`.
- Some missions have `payload.cargo_lb: null`.
- Some missions have incomplete timing values.
- Some `source_files.debrief` references are `null`.

---

## Human-Readable Summary Boundary

A human-readable statistics summary may be maintained at:

`/operations/current/career-state/statistics.md`

If used, it should:

- state that telemetry YAML is authoritative
- identify the telemetry range used for the summary
- show generated or updated date when available
- show incomplete-data caveats
- avoid becoming an independent data source

The detailed strategy for this file is handled by Issue #59.

---

## Prompt Update Boundary

Issue #39 should teach FlightOps to:

- answer statistics questions from telemetry YAML
- apply these calculation rules
- classify metrics as complete, partial, or unavailable
- preserve incomplete-data caveats
- avoid narrative markdown scraping when telemetry YAML exists

Prompt implementation is not part of this task, but the requirement should be captured for #39.
