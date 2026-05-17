# Mission Statistics Incomplete Data Rules

---

## Purpose

Define how FlightOps should handle incomplete, unknown, or partially available telemetry when producing mission statistics and analytics.

This document supports the mission statistics calculation rules in:

`/operations/current/templates/mission-statistics-calculation-rules.md`

Authoritative raw telemetry remains:

`/operations/current/data/missions/YYYY/MISSION_ID.yaml`

---

## Core Principle

Incomplete telemetry must be reported honestly.

Do not turn unknown values into zeros.
Do not silently drop missing values.
Do not fabricate values to make statistics look complete.

Sparse-but-supported statistics are better than confident nonsense with a spreadsheet hat.

---

## Metric Readiness Categories

FlightOps should classify each requested statistic into one of three categories.

| Category | Meaning | Output Behavior |
|---|---|---|
| Ready | All required telemetry fields are available for the requested scope | Report normally |
| Partially ready | Some required telemetry fields are missing, but a known subtotal or partial calculation is still useful | Report known value plus an incomplete-data caveat |
| Unavailable | Required telemetry fields are missing enough that the statistic would be misleading | Do not calculate; state which field(s) are missing |

---

## Telemetry Value Handling

| Telemetry Value | Meaning | Statistics Behavior |
|---|---|---|
| `null` | Unknown or unsupported scalar value | Treat as missing, not zero |
| `unknown` | Unknown enum-like value | Treat as unknown category only when relevant |
| `[]` | No supported list entries | Treat as empty known list |
| Numeric value | Supported measurement/count | Include in calculations |
| Boolean value | Supported true/false value | Include in applicable counts |

Rules:

- `null` must not be treated as `0` unless a specific metric explicitly defines that behavior.
- Empty lists are not the same as unknown lists; they mean no supported entries are present.
- `unknown` should not be counted as failure, success, domestic, international, or any other concrete category unless the metric is explicitly counting unknowns.

---

## Known Totals with Caveats

Known totals may be reported when some values are missing, as long as the output clearly states the limitation.

Example pattern:

```text
Known fuel used: 71.87 gal across 4 of 5 missions.
Incomplete: Mission 003 has fuel.used_gal: null.
```

Use this pattern for metrics such as:

- known fuel used
- known cargo transported
- known block time
- known airborne time
- known go-around count if any mission has unknown count
- known diversion count if any mission has unknown count

---

## When Not to Calculate

Do not calculate a metric when the missing fields would make the result misleading.

Examples:

- Do not calculate NM per gallon for missions missing either `route.distance_nm` or `fuel.used_gal`.
- Do not calculate cargo efficiency for missions missing either `payload.cargo_lb` or `route.distance_nm`.
- Do not calculate average fuel burn for missions missing fuel used or the selected time basis.
- Do not calculate route-distance metrics from route strings alone.
- Do not calculate fuel used from departure and arrival fuel unless explicitly pilot-confirmed or source-supported.

If unavailable, report the missing field(s) instead of guessing.

---

## Denominator Rules

Averages must clearly state their denominator.

Preferred wording examples:

```text
Average mission distance: 252.2 nm across 5 of 5 missions.
```

```text
Average known airborne fuel burn: 11.2 GPH across 3 missions with both fuel and airborne time available.
```

Rules:

- Use all missions only when all required fields are available.
- Use known-value denominators when some missions have missing fields.
- Label known-value averages as such when the denominator excludes missions.
- Do not silently exclude missions without saying so.

---

## Current Known Telemetry Gaps

As of the v2.2 statistics work, the current telemetry layer has these known gaps:

- Mission 003 has `fuel.used_gal: null`.
- Some missions have `payload.cargo_lb: null`.
- Some missions have incomplete timing values.
- `source_files.debrief` values are `null` where debrief links are not standardized.

Distance is no longer a blocking gap for Missions 001–005 after Issue #55.

---

## Current Metric Readiness

### Ready Metrics

These metrics are currently ready for Missions 001–005:

- mission count
- mission list by date
- aircraft utilization by tail number
- mission type breakdown
- flight rules breakdown
- success rate
- go-around totals
- diversion totals
- international operation count
- airport activity
- passenger totals
- total distance flown
- average mission distance
- passenger miles

### Partially Ready Metrics

These metrics are currently partially ready:

- total fuel used, because Mission 003 has `fuel.used_gal: null`
- NM per gallon, because Mission 003 lacks fuel used
- average fuel burn, because some missions lack fuel and/or timing values
- total cargo transported, because some missions have `payload.cargo_lb: null`
- cargo efficiency metrics, because some missions lack cargo values
- average mission duration, because some missions lack complete timing values

### Unavailable Metrics

No major currently scoped distance metric is unavailable after Issue #55.

Future metrics may be unavailable if they rely on fields not yet captured in telemetry.

---

## Output Language Rules

FlightOps should use clear language that distinguishes complete from partial results.

Recommended labels:

- `Total` only when complete for the requested scope.
- `Known total` when one or more values are missing.
- `Average` only when all records in scope are included.
- `Average known` when calculated from a subset with supported values.
- `Unavailable` when required telemetry is missing.

Examples:

```text
Total distance flown: 1,261 nm across 5 of 5 missions.
```

```text
Known fuel used: 71.87 gal across 4 of 5 missions. Mission 003 is missing fuel.used_gal.
```

```text
NM per gallon is partially available because Mission 003 is missing fuel.used_gal.
```

---

## No-Fabrication Rules

FlightOps must not fabricate or infer missing telemetry values.

Do not invent:

- fuel used
- cargo or baggage weight
- route distance
- block time
- airborne time
- UTC event timestamps
- go-around count
- diversion count
- mission result
- client references
- debrief file references

Allowed updates require explicit support from:

- telemetry YAML
- mission record
- debrief record
- PIREP
- pilot-confirmed correction

---

## Field-Specific Handling

### Fuel

If `fuel.used_gal` is missing:

- report known fuel used only
- exclude the mission from NM-per-gallon and fuel-burn calculations
- do not calculate fuel used from departure and arrival fuel unless explicitly confirmed

### Cargo

If `payload.cargo_lb` is missing:

- report known cargo transported only
- exclude the mission from cargo efficiency calculations
- do not treat descriptive baggage language as numeric cargo telemetry

### Timing

If timing values are missing:

- calculate duration metrics only from missions with supported timing values
- label whether block time or airborne time is used
- report excluded missions when relevant

### Debrief References

If `source_files.debrief` is `null`:

- do not invent a debrief path
- do not treat the mission as lacking a debrief unless separately confirmed
- report that debrief linkage is not standardized or unavailable

---

## Relationship to Statistics Summary

If `/operations/current/career-state/statistics.md` is used, it should preserve these incomplete-data rules.

The summary should:

- mark known totals clearly
- include caveats for partial metrics
- avoid hiding excluded missions
- reference telemetry YAML as the source of truth

The detailed human-readable summary strategy is handled by Issue #59.

---

## Prompt Update Boundary

Issue #39 should teach FlightOps to:

- classify statistics as ready, partially ready, or unavailable
- treat `null` as unknown, not zero
- report known totals with caveats
- identify missing fields when metrics cannot be calculated
- avoid fabricating missing telemetry
- avoid silently excluding missions from averages or totals

Prompt implementation is not part of this task, but these requirements must be captured for Issue #39.
