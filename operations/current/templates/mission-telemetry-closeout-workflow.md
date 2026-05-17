# Mission Telemetry Closeout Workflow

---

## Purpose

Define how FlightOps should create, update, and validate per-mission telemetry YAML records during post-flight PIREP, debrief, and mission closeout workflows.

This workflow supports the canonical telemetry schema documented in:

`/operations/current/templates/mission-telemetry-schema.md`

Authoritative raw mission telemetry lives in:

`/operations/current/data/missions/YYYY/MISSION_ID.yaml`

Mission and debrief markdown files remain the human-readable narrative and historical record.

---

## Source of Truth

Telemetry source-of-truth rules:

- Per-mission telemetry YAML is authoritative for raw mission facts.
- Mission markdown remains the narrative mission record.
- Debrief markdown remains the pilot assessment and operational lessons record.
- `clients.yaml` remains authoritative for client continuity.
- `clients-summary.md` remains secondary/reference-only for client quick review.
- Derived statistics should be calculated from telemetry YAML rather than manually entered into mission telemetry.

---

## When to Create Telemetry YAML

Create a telemetry YAML file during mission closeout when:

- a mission has been completed
- a PIREP has been provided
- a mission record exists or is being finalized
- enough mission identity information exists to assign a `MISSION_ID`

Target path:

```text
/operations/current/data/missions/YYYY/MISSION_ID.yaml
```

Example:

```text
/operations/current/data/missions/2026/MISSION_006_EXAMPLE.yaml
```

Telemetry YAML should be created before analytics/statistics are updated or queried for the completed mission.

---

## When to Update Existing Telemetry YAML

Update an existing telemetry YAML file when:

- pilot-confirmed corrections are provided
- missing PIREP fields are later supplied
- source mission/debrief files are finalized after the telemetry file was created
- client references become known after client continuity backfill
- an unsupported field becomes supported by later evidence

Do not update telemetry YAML merely to make it look more complete.

If a value is unsupported, leave it as `null`, `[]`, or `unknown` according to schema rules.

---

## Closeout Sequence

During mission closeout, FlightOps should:

1. Review the completed mission record.
2. Review the PIREP provided by the pilot.
3. Review the debrief record if already created.
4. Identify the mission ID, mission number, and mission date.
5. Create or update the per-mission telemetry YAML file.
6. Populate fields only where supported by the PIREP, mission record, debrief, or pilot-confirmed correction.
7. Preserve unsupported values as `null`, `[]`, or `unknown`.
8. Add `client_refs` only when known client records exist in `clients.yaml`.
9. Add source file references for mission and debrief records.
10. Leave mission and debrief markdown intact as narrative/historical records.
11. Use telemetry YAML as the source for future statistics and analytics.

---

## PIREP-to-Telemetry Field Mapping

### Mission Identity

| PIREP / Source Field | Telemetry Field |
|---|---|
| Mission ID | `mission_id` |
| Mission number | `mission_number` |
| Mission date | `date` |

Rules:

- `mission_id` must match the mission markdown ID.
- `date` must use `YYYY-MM-DD`.
- Use numeric mission sequence for `mission_number`.

---

### Aircraft

| PIREP / Source Field | Telemetry Field |
|---|---|
| Tail number / registration | `aircraft.tail_number` |
| Aircraft model/type | `aircraft.type` |

Rules:

- Use the aircraft identity from the mission record or PIREP.
- Use `null` if unsupported.

---

### Route

| PIREP / Source Field | Telemetry Field |
|---|---|
| Departure airport | `route.departure` |
| Arrival airport | `route.arrival` |
| Approved / dispatched route | `route.planned_route` |
| Actual flown route | `route.flown_route` |
| Approved/flown distance | `route.distance_nm` |

Rules:

- Use ICAO airport identifiers.
- Store route strings as quoted strings.
- `distance_nm` should come from approved/flown flight plan data when available.
- Do not infer route distance unless explicitly approved or supplied.

---

### Timing

| PIREP / Source Field | Telemetry Field |
|---|---|
| OUT time | `timing.out_utc` |
| OFF time | `timing.off_utc` |
| Top of climb | `timing.toc_utc` |
| Top of descent | `timing.tod_utc` |
| ON time | `timing.on_utc` |
| IN time | `timing.in_utc` |
| Shutdown time | `timing.shutdown_utc` |
| Block time | `timing.block_time_hr` |
| Airborne time | `timing.airborne_time_hr` |

Rules:

- UTC event times should use quoted `HHMMZ` format.
- Durations should be numeric decimal hours when supported.
- Do not derive missing timestamps unless explicitly provided or pilot-confirmed.

---

### Fuel

| PIREP / Source Field | Telemetry Field |
|---|---|
| Departure fuel | `fuel.departure_gal` |
| Arrival fuel | `fuel.arrival_gal` |
| Fuel used | `fuel.used_gal` |
| Fuel uplift | `fuel.uplift_gal` |

Rules:

- Fuel values are numeric gallons.
- Do not calculate fuel used from departure/arrival unless the workflow explicitly approves that correction or the value is already recorded.
- Use `null` for unsupported fuel fields.

---

### Payload

| PIREP / Source Field | Telemetry Field |
|---|---|
| Passenger count | `payload.passengers` |
| Cargo / baggage weight | `payload.cargo_lb` |

Rules:

- Passenger count should be numeric.
- Cargo/baggage weight should be numeric pounds.
- Descriptive baggage text such as “light baggage” is not numeric telemetry and should remain `null`.
- Individual passenger weights should not be mapped unless names and weights are clearly linked by source records.

---

### Operations

| PIREP / Source Field | Telemetry Field |
|---|---|
| Mission type | `operations.mission_type` |
| Flight rules | `operations.flight_rules` |
| Mission result | `operations.result` |
| Diversion count | `operations.diversions` |
| Go-around count | `operations.go_arounds` |
| International flag | `operations.international` |

Rules:

- Use lowercase snake_case for enum-like values.
- Use numeric counts for diversions and go-arounds.
- Use boolean values for `international` when supported.
- Use `unknown` or `null` when unsupported by the source.

---

### Client References

| Source | Telemetry Field |
|---|---|
| Known client IDs from `clients.yaml` | `client_refs` |

Rules:

- Add `client_refs` when named clients match existing `clients.yaml` records.
- Use an empty list when no known client references exist.
- Do not duplicate client details in telemetry YAML.
- Client continuity remains authoritative in `/operations/current/data/clients.yaml`.

---

### Source Files

| Source Record | Telemetry Field |
|---|---|
| Mission markdown path | `source_files.mission` |
| Debrief markdown path | `source_files.debrief` |

Rules:

- Always reference the mission markdown file when available.
- Reference the debrief file when it exists.
- Use `null` for missing debrief references.

---

## Unknown / Unsupported Values

Use the standard schema representations:

| Situation | Representation |
|---|---|
| Unknown scalar value | `null` |
| Unknown enum-like value | `"unknown"` |
| Unsupported list | `[]` |
| Unsupported boolean | `null` |
| Unsupported numeric value | `null` |

Do not fill missing fields with guessed values.

Sparse accurate telemetry is better than complete-looking fiction. This is aviation, not a PowerPoint sales pipeline.

---

## Pilot-Confirmed Corrections

Pilot-confirmed corrections may update telemetry YAML when the pilot explicitly provides or confirms a value.

Examples:

- correcting a mission date
- confirming a route or distance value
- providing missing timestamps
- confirming fuel used
- confirming named client involvement
- correcting a go-around or diversion count

Correction rules:

- Update the telemetry field only when the correction is explicit.
- Preserve source traceability through the related mission/debrief record when possible.
- If the correction is not reflected in mission/debrief markdown, note the correction in the related issue or future debrief update.
- Do not silently infer unsupported corrections.

---

## Markdown Preservation

Mission and debrief markdown files remain intact as historical narrative records.

Telemetry YAML should not replace:

- mission overview
- dispatch narrative
- passenger experience narrative
- pilot advisory
- operational lessons learned
- debrief analysis

Telemetry YAML should capture raw facts only.

---

## Analytics and Statistics Boundary

Telemetry YAML should not contain manually persisted aggregate statistics.

Do not store derived career totals inside mission telemetry files, such as:

- total distance flown
- total fuel used across all missions
- average mission distance
- average fuel burn
- success rate
- aircraft utilization totals

Those values belong to Issue #35 analytics/statistics workflows and should be calculated from mission telemetry YAML.

---

## Future PIREP Capture Checklist

During future PIREP intake, FlightOps should ask for or capture these values when available:

- mission ID
- mission date
- aircraft tail number and type
- departure and arrival ICAO codes
- planned/approved route
- actual flown route
- distance flown in nautical miles
- OUT / OFF / TOC / TOD / ON / IN / shutdown UTC timestamps
- block time and airborne time
- departure fuel, arrival fuel, fuel used, and fuel uplift
- passenger count
- cargo/baggage weight
- mission type
- flight rules
- result
- diversion count
- go-around count
- international operation flag
- named clients/passengers
- source mission/debrief file paths

If values are not provided, preserve them as unknown rather than forcing completion.

---

## Prompt Update Requirements

Issue #39 should update the FlightOps prompt so it knows to:

- create telemetry YAML during mission closeout
- use `/operations/current/templates/mission-telemetry-schema.md` as the schema reference
- write telemetry files to `/operations/current/data/missions/YYYY/MISSION_ID.yaml`
- preserve mission/debrief markdown as narrative history
- map PIREP values into the correct YAML sections
- preserve unsupported values as `null`, `[]`, or `unknown`
- apply `client_refs` when known clients match `clients.yaml`
- update telemetry YAML when pilot-confirmed corrections are provided
- use telemetry YAML as the source for statistics and analytics queries

---

## Validation Scenarios

### New Completed Mission with Full PIREP Data

Expected behavior:

- create telemetry YAML
- populate all supported fields
- add source file references
- add client refs if applicable

### Completed Mission with Partial PIREP Data

Expected behavior:

- create telemetry YAML
- populate supported fields
- leave unsupported fields as `null`, `[]`, or `unknown`

### Pilot-Confirmed Correction After Closeout

Expected behavior:

- update supported corrected field
- preserve correction traceability
- do not infer related unsupported values

### Named Clients Available

Expected behavior:

- add matching `client_refs`
- do not duplicate client continuity details

### Mission with No Known Client References

Expected behavior:

- use `client_refs: []`

### Ambiguous Telemetry Values

Expected behavior:

- do not guess
- preserve ambiguity as unknown
- request clarification only when operationally necessary
