# Mission Telemetry YAML Schema

---

## Purpose

Define the canonical per-mission telemetry YAML schema for Lost Bay Aviation mission operations.

This schema captures raw operational mission facts in a structured, parser-friendly format so analytics, statistics, prompt workflows, and future automation can consume mission telemetry without scraping narrative markdown files.

---

## Source of Truth

Authoritative raw mission telemetry lives in per-mission YAML files:

`/operations/current/data/missions/YYYY/MISSION_ID.yaml`

Example:

`/operations/current/data/missions/2026/MISSION_005_KEYW_MYAM.yaml`

Mission and debrief markdown files remain the human-readable operational narrative and historical record.

Rules:

- Telemetry YAML is authoritative for raw mission facts.
- Mission markdown remains authoritative for narrative mission context.
- Debrief markdown remains authoritative for pilot assessment and operational lessons learned.
- Derived statistics should be calculated from telemetry YAML, not manually entered into telemetry files.
- Do not scrape narrative markdown for statistics once telemetry YAML exists.

---

## File Path Convention

Telemetry files must use this path pattern:

```text
/operations/current/data/missions/YYYY/MISSION_ID.yaml
```

Naming rules:

- Use the same mission ID used by mission markdown files.
- Use uppercase mission identifiers, matching existing mission naming conventions.
- Use `.yaml` file extension.
- Group files by mission year.

Examples:

```text
/operations/current/data/missions/2026/MISSION_001_KPNS_KMTH.yaml
/operations/current/data/missions/2026/MISSION_002_KMTH_KAPF.yaml
/operations/current/data/missions/2026/MISSION_005_KEYW_MYAM.yaml
```

---

## Canonical YAML Structure

```yaml
schema_version: 1
mission_id: "MISSION_005_KEYW_MYAM"
mission_number: 5
date: "2026-05-15"

aircraft:
  tail_number: "N401LB"
  type: "Cessna 400 Corvalis TT"

route:
  departure: "KEYW"
  arrival: "MYAM"
  planned_route: "KEYW HEVEL NOLEE BIPIN GUCEL TAZER ZBV KOUGH HONEX MYAM"
  flown_route: "KEYW HEVEL NOLEE BIPIN GUCEL TAZER ZBV KOUGH HONEX WOMIS MYAM"
  distance_nm: null

timing:
  out_utc: "1331Z"
  off_utc: "1335Z"
  toc_utc: "1349Z"
  tod_utc: "1451Z"
  on_utc: "1502Z"
  in_utc: "1506Z"
  shutdown_utc: "1509Z"
  block_time_hr: 1.6
  airborne_time_hr: 1.5

fuel:
  departure_gal: 50.0
  arrival_gal: 33.03
  used_gal: 16.97
  uplift_gal: null

payload:
  passengers: 2
  cargo_lb: 95

operations:
  mission_type: "premium_leisure_international_charter"
  flight_rules: "vfr"
  result: "success"
  diversions: 0
  go_arounds: 0
  international: true

client_refs:
  - "client_00006"
  - "client_00007"

source_files:
  mission: "operations/current/missions/2026/MISSION_005_KEYW_MYAM.md"
  debrief: null
```

---

## Required Top-Level Fields

| Field | Type | Required | Notes |
|---|---|---:|---|
| `schema_version` | integer | Yes | Current version is `1` |
| `mission_id` | string | Yes | Must match mission record ID |
| `mission_number` | integer | Yes | Numeric mission sequence |
| `date` | string | Yes | ISO date, `YYYY-MM-DD` |
| `aircraft` | mapping | Yes | Aircraft identity |
| `route` | mapping | Yes | Departure, arrival, route, distance |
| `timing` | mapping | Yes | UTC times and durations |
| `fuel` | mapping | Yes | Raw fuel telemetry |
| `payload` | mapping | Yes | Passenger and cargo values |
| `operations` | mapping | Yes | Operational classification and result |
| `client_refs` | list | Yes | Empty list if no known client references |
| `source_files` | mapping | Yes | Historical source file references |

---

## Field Rules

### Aircraft

```yaml
aircraft:
  tail_number: "N401LB"
  type: "Cessna 400 Corvalis TT"
```

Rules:

- `tail_number` should use the aircraft registration used in mission records.
- `type` should use the aircraft type used in mission records.
- Use `null` if unsupported.

---

### Route

```yaml
route:
  departure: "KEYW"
  arrival: "MYAM"
  planned_route: "KEYW HEVEL NOLEE BIPIN GUCEL TAZER ZBV KOUGH HONEX MYAM"
  flown_route: "KEYW HEVEL NOLEE BIPIN GUCEL TAZER ZBV KOUGH HONEX WOMIS MYAM"
  distance_nm: null
```

Rules:

- `departure` and `arrival` should use ICAO codes.
- `planned_route` should reflect the approved or dispatched route when available.
- `flown_route` should reflect the actual flown route when available.
- `distance_nm` should come from the approved/flown flight plan when available.
- Do not calculate or infer distance unless the value is explicitly provided or approved for entry.
- Use `null` for unknown distance.

---

### Timing

```yaml
timing:
  out_utc: "1331Z"
  off_utc: "1335Z"
  toc_utc: "1349Z"
  tod_utc: "1451Z"
  on_utc: "1502Z"
  in_utc: "1506Z"
  shutdown_utc: "1509Z"
  block_time_hr: 1.6
  airborne_time_hr: 1.5
```

Rules:

- UTC event times should be stored as quoted `HHMMZ` strings.
- Use `null` for unsupported timestamps.
- `block_time_hr` and `airborne_time_hr` should be numeric decimal hours when supported.
- Do not derive missing times unless explicitly provided by the PIREP, mission record, debrief, or pilot-confirmed correction.

---

### Fuel

```yaml
fuel:
  departure_gal: 50.0
  arrival_gal: 33.03
  used_gal: 16.97
  uplift_gal: null
```

Rules:

- Fuel values should be numeric gallons.
- Use `null` for unsupported values.
- Do not derive fuel used from departure/arrival unless explicitly approved or already recorded.
- Keep fuel units in field names using `_gal`.

---

### Payload

```yaml
payload:
  passengers: 2
  cargo_lb: 95
```

Rules:

- `passengers` should be a numeric count.
- `cargo_lb` should include cargo/baggage weight when supported.
- Use `null` when cargo/baggage weight is not explicitly recorded.
- Do not map individual passenger weights unless the source clearly maps names to weights.
- Keep weight units in field names using `_lb`.

---

### Operations

```yaml
operations:
  mission_type: "premium_leisure_international_charter"
  flight_rules: "vfr"
  result: "success"
  diversions: 0
  go_arounds: 0
  international: true
```

Rules:

- Use lowercase snake_case for enum-like values.
- `mission_type` should reflect the mission classification.
- `flight_rules` should use values such as `vfr`, `ifr`, or `vfr_with_flight_following` when supported.
- `result` should use values such as `success`, `partial`, `failed`, or `unknown`.
- `diversions` and `go_arounds` should be numeric counts when supported.
- `international` should be boolean when supported, otherwise `null`.

---

### Client References

```yaml
client_refs:
  - "client_00006"
  - "client_00007"
```

Rules:

- Use client keys from `/operations/current/data/clients.yaml`.
- Use an empty list if no known client references are supported.
- Do not duplicate client continuity details in mission telemetry YAML.
- Mission telemetry may reference clients, but `clients.yaml` remains authoritative for client continuity.

---

### Source Files

```yaml
source_files:
  mission: "operations/current/missions/2026/MISSION_005_KEYW_MYAM.md"
  debrief: null
```

Rules:

- `mission` should point to the source mission markdown file.
- `debrief` should point to the source debrief file when one exists.
- Use `null` when no debrief file exists.
- Source references support traceability and should not be used to scrape derived statistics once telemetry YAML exists.

---

## Unknown and Unsupported Values

Use consistent explicit representations:

| Situation | Representation |
|---|---|
| Unknown scalar value | `null` |
| Unknown enum-like value | `"unknown"` |
| Unsupported list | `[]` |
| Unsupported boolean | `null` |
| Unsupported numeric value | `null` |

Examples:

```yaml
route:
  distance_nm: null

client_refs: []

operations:
  result: "unknown"
  international: null
```

Do not fabricate missing telemetry to make files look complete. Sparse but accurate data beats confident nonsense. Obviously.

---

## Parser-Friendly YAML Rules

- Use two-space indentation.
- Quote string IDs, ICAO routes, and UTC timestamps.
- Use lowercase snake_case for field names.
- Use lowercase snake_case for enum-like values.
- Use numeric values for quantities when supported.
- Include units in field names, such as `_gal`, `_lb`, `_hr`, and `_nm`.
- Use `null` instead of blank values.
- Use empty lists instead of omitted list fields.
- Keep all canonical top-level sections present, even when some nested values are unknown.
- Avoid prose-heavy mission narrative inside telemetry YAML.

---

## Supported Initial Mission Backfill

This schema supports Missions 001–005 because it allows both complete and partial telemetry records:

- Mission 001 has limited named client/route/timing detail and can preserve unsupported values as `null`.
- Mission 002 has stronger client, route, fuel, payload, and go-around data.
- Mission 003 has concise executive charter timing/fuel/route data.
- Mission 004 has premium leisure route, timing, fuel, and payload data.
- Mission 005 has detailed international timing, route, fuel, payload, and client data.

No mission telemetry file should require fabricated values to satisfy the schema.

---

## Relationship to Analytics

Issue #35 should consume telemetry YAML files for derived statistics.

Telemetry YAML should support calculations such as:

- total distance flown
- total fuel used
- total cargo transported
- average mission distance
- average fuel burn
- average mission duration
- passenger miles
- success rate
- mission type breakdown
- flight rules breakdown
- airport activity
- international operation totals

Derived aggregate values should not be stored in per-mission telemetry YAML unless a future caching strategy explicitly approves it.

---

## Relationship to Prompt Work

Issue #39 should update the FlightOps prompt so future mission closeout workflows:

- create per-mission telemetry YAML after mission completion
- update telemetry YAML when pilot-confirmed corrections are provided
- preserve unknowns instead of guessing
- keep mission/debrief markdown as narrative history
- use telemetry YAML as the source for statistics queries
