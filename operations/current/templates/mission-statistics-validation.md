# Mission Statistics Validation

---

## Purpose

Validate that the current Lost Bay Aviation mission statistics can be reproduced from structured mission telemetry YAML.

This validation supports Issue #35 and confirms the handoff requirements for Issue #39 prompt updates.

---

## Validated Source Files

Authoritative telemetry source:

`/operations/current/data/missions/YYYY/MISSION_ID.yaml`

Validated records:

- `/operations/current/data/missions/2026/MISSION_001_KPNS_KMTH.yaml`
- `/operations/current/data/missions/2026/MISSION_002_KMTH_KAPF.yaml`
- `/operations/current/data/missions/2026/MISSION_003_KAPF_KFXE.yaml`
- `/operations/current/data/missions/2026/MISSION_004_KFXE_KEYW.yaml`
- `/operations/current/data/missions/2026/MISSION_005_KEYW_MYAM.yaml`

Related rules:

- `/operations/current/templates/mission-statistics-calculation-rules.md`
- `/operations/current/templates/mission-statistics-incomplete-data-rules.md`
- `/operations/current/templates/mission-statistics-summary-strategy.md`

Human-readable summary:

- `/operations/current/career-state/statistics.md`

---

## Validation Result

Current statistics are reproducible from telemetry YAML without scraping narrative mission or debrief markdown.

Mission/debrief markdown remains the narrative and historical layer only.

---

## Ready Metrics

These metrics are ready and complete for Missions 001–005.

| Metric | Value | Source Fields |
|---|---:|---|
| Mission count | 5 | telemetry file count |
| Successful missions | 5 | `operations.result` |
| Success rate | 100% | `operations.result` |
| Total distance flown | 1,261 nm | `route.distance_nm` |
| Average mission distance | 252.2 nm | `route.distance_nm` |
| Total passengers transported | 9 | `payload.passengers` |
| Passenger miles | 2,435 passenger-nm | `payload.passengers`, `route.distance_nm` |
| Go-arounds | 1 | `operations.go_arounds` |
| Diversions | 0 | `operations.diversions` |
| International missions | 1 | `operations.international` |

---

## Route Distance Validation

| Mission | Distance NM |
|---|---:|
| `MISSION_001_KPNS_KMTH` | 550 |
| `MISSION_002_KMTH_KAPF` | 181 |
| `MISSION_003_KAPF_KFXE` | 87 |
| `MISSION_004_KFXE_KEYW` | 160 |
| `MISSION_005_KEYW_MYAM` | 283 |
| **Total** | **1,261** |

Average mission distance:

```text
1,261 nm / 5 missions = 252.2 nm
```

---

## Passenger Miles Validation

| Mission | Passengers | Distance NM | Passenger-NM |
|---|---:|---:|---:|
| `MISSION_001_KPNS_KMTH` | 2 | 550 | 1,100 |
| `MISSION_002_KMTH_KAPF` | 2 | 181 | 362 |
| `MISSION_003_KAPF_KFXE` | 1 | 87 | 87 |
| `MISSION_004_KFXE_KEYW` | 2 | 160 | 320 |
| `MISSION_005_KEYW_MYAM` | 2 | 283 | 566 |
| **Total** | **9** | — | **2,435** |

---

## Mission Type Breakdown

| Mission Type | Count |
|---|---:|
| `premium_leisure_charter` | 2 |
| `premium_leisure_return_charter` | 1 |
| `executive_charter` | 1 |
| `premium_leisure_international_charter` | 1 |

---

## Flight Rules Breakdown

| Flight Rules | Count |
|---|---:|
| `vfr_with_flight_following` | 3 |
| `vfr` | 2 |

---

## Airport Activity

### Departures

| Airport | Count |
|---|---:|
| KPNS | 1 |
| KMTH | 1 |
| KAPF | 1 |
| KFXE | 1 |
| KEYW | 1 |

### Arrivals

| Airport | Count |
|---|---:|
| KMTH | 1 |
| KAPF | 1 |
| KFXE | 1 |
| KEYW | 1 |
| MYAM | 1 |

### Combined Activity

| Airport | Activity Count |
|---|---:|
| KAPF | 2 |
| KEYW | 2 |
| KFXE | 2 |
| KMTH | 2 |
| KPNS | 1 |
| MYAM | 1 |

---

## Partially Ready Metrics

These metrics can be reported only as known totals or known averages because some telemetry values remain unknown.

| Metric | Known Value | Caveat |
|---|---:|---|
| Known fuel used | 72.87 gal | Mission 003 has `fuel.used_gal: null` |
| Known NM per gallon | 16.11 nm/gal | Mission 003 excluded because fuel used is unknown |
| Known cargo / baggage transported | 255 lb | Missions 001 and 003 have `payload.cargo_lb: null` |
| Known cargo efficiency | 53,745 lb-nm | Only missions with known cargo are included |
| Average known airborne fuel burn | 12.71 GPH | Only missions with known fuel and airborne time are included |
| Average known block fuel burn | 11.19 GPH | Only missions with known fuel and block time are included |

---

## Partial Metric Calculations

### Known Fuel Used

Included missions:

- Mission 001: 32.90 gal
- Mission 002: 12.00 gal
- Mission 004: 11.00 gal
- Mission 005: 16.97 gal

```text
32.90 + 12.00 + 11.00 + 16.97 = 72.87 gal
```

Mission 003 is excluded because `fuel.used_gal: null`.

### Known NM per Gallon

Included missions with both distance and fuel used:

- Mission 001: 550 nm / 32.90 gal
- Mission 002: 181 nm / 12.00 gal
- Mission 004: 160 nm / 11.00 gal
- Mission 005: 283 nm / 16.97 gal

```text
(550 + 181 + 160 + 283) / 72.87 = 16.11 nm/gal
```

Mission 003 is excluded because `fuel.used_gal: null`.

### Known Cargo / Baggage

Included missions:

- Mission 002: 60 lb
- Mission 004: 100 lb
- Mission 005: 95 lb

```text
60 + 100 + 95 = 255 lb
```

Missions 001 and 003 are excluded because `payload.cargo_lb: null`.

### Known Cargo Efficiency

```text
(60 * 181) + (100 * 160) + (95 * 283) = 53,745 lb-nm
```

---

## Remaining Telemetry Gaps

Known gaps remaining after validation:

- Mission 003 has `fuel.used_gal: null`.
- Mission 001 has `payload.cargo_lb: null`.
- Mission 003 has `payload.cargo_lb: null`.
- Some missions have incomplete timing values.
- `source_files.debrief` values remain `null` where debrief links are not standardized.

These gaps do not block Issue #35 completion, but they must remain visible in statistics output.

---

## Prompt Handoff Requirements

Issue #39 should update FlightOps so statistics queries:

- use telemetry YAML as the source of truth
- calculate ready metrics directly from telemetry YAML
- label partial metrics as known totals or known averages
- include incomplete-data caveats
- avoid treating `null` as zero
- avoid fabricating missing fuel, cargo, timing, or debrief-reference values
- avoid narrative markdown scraping when telemetry YAML exists
- treat `statistics.md` as a human-readable summary only

---

## Issue #35 Readiness

After Issues #57, #58, #59, and #60, Story #35 has enough guidance to close when the final branch is merged.

Completed supporting artifacts:

- `/operations/current/templates/mission-statistics-calculation-rules.md`
- `/operations/current/templates/mission-statistics-incomplete-data-rules.md`
- `/operations/current/templates/mission-statistics-summary-strategy.md`
- `/operations/current/templates/mission-statistics-validation.md`
- `/operations/current/career-state/statistics.md`

The structured statistics layer is now defined as:

- telemetry YAML for authoritative raw mission facts
- calculation rules for derived statistics
- incomplete-data rules for partial metrics
- `statistics.md` as a human-readable summary
- prompt handoff requirements documented for Issue #39
