# Lost Bay Aviation Statistics Summary

---

## Source Note

This file is a human-readable summary only.

Authoritative raw mission telemetry lives in:

`/operations/current/data/missions/YYYY/MISSION_ID.yaml`

Calculation rules live in:

`/operations/current/templates/mission-statistics-calculation-rules.md`

If this file conflicts with telemetry YAML, telemetry YAML wins.

---

## Current Summary

Telemetry range:

- Missions included: `MISSION_001_KPNS_KMTH` through `MISSION_005_KEYW_MYAM`
- Mission count: 5
- Aircraft: `N401LB` — Cessna 400 Corvalis TT

| Metric | Value |
|---|---:|
| Missions Completed | 5 |
| Successful Missions | 5 |
| Success Rate | 100% |
| Total Distance Flown | 1,261 nm |
| Average Mission Distance | 252.2 nm |
| Total Passengers Transported | 9 |
| Passenger Miles | 2,435 passenger-nm |
| Go-Arounds | 1 |
| Diversions | 0 |
| International Missions | 1 |

---

## Career Totals

| Metric | Value | Completeness |
|---|---:|---|
| Total Distance Flown | 1,261 nm | Complete across 5 of 5 missions |
| Average Mission Distance | 252.2 nm | Complete across 5 of 5 missions |
| Known Fuel Used | 72.87 gal | Partial; Mission 003 has `fuel.used_gal: null` |
| Known Cargo / Baggage Transported | 255 lb | Partial; some missions have `payload.cargo_lb: null` |
| Total Passengers Transported | 9 | Complete across 5 of 5 missions |
| Passenger Miles | 2,435 passenger-nm | Complete across 5 of 5 missions |
| Known Cargo Efficiency | 53,745 lb-nm | Partial; only missions with known cargo are included |
| Known NM per Gallon | 16.11 nm/gal | Partial; Mission 003 excluded because fuel used is unknown |

---

## Distance & Route Statistics

| Mission | Route | Distance NM |
|---|---|---:|
| `MISSION_001_KPNS_KMTH` | `KPNS CEW CTY SRQ KMTH` | 550 |
| `MISSION_002_KMTH_KAPF` | `KMTH MHIWX MNAWX WNNER VKZ BHHIA PALMZ CULMA KAPF` | 181 |
| `MISSION_003_KAPF_KFXE` | `KAPF KBUNK JMACA KFXE` | 87 |
| `MISSION_004_KFXE_KEYW` | `KFXE BHHIA PALMZ PIBDE KEYW` | 160 |
| `MISSION_005_KEYW_MYAM` | `KEYW HEVEL NOLEE BIPIN GUCEL TAZER ZBV KOUGH HONEX WOMIS MYAM` | 283 |

Distance notes:

- Distance-based metrics are now available for Missions 001–005.
- Distance values are sourced from mission telemetry YAML.

---

## Fuel Statistics

| Metric | Value | Completeness |
|---|---:|---|
| Known Fuel Used | 72.87 gal | Partial; 4 of 5 missions have known fuel used |
| Known NM per Gallon | 16.11 nm/gal | Partial; Mission 003 excluded |
| Average Known Airborne Fuel Burn | 12.71 GPH | Partial; only missions with known fuel and airborne time included |
| Average Known Block Fuel Burn | 11.19 GPH | Partial; only missions with known fuel and block time included |

Fuel caveats:

- Mission 003 has `fuel.used_gal: null`.
- Fuel used should not be derived from departure and arrival fuel unless explicitly pilot-confirmed or source-supported.

---

## Payload Statistics

| Metric | Value | Completeness |
|---|---:|---|
| Total Passengers | 9 | Complete across 5 of 5 missions |
| Passenger Miles | 2,435 passenger-nm | Complete across 5 of 5 missions |
| Known Cargo / Baggage | 255 lb | Partial; 3 of 5 missions have known cargo weight |
| Known Cargo Efficiency | 53,745 lb-nm | Partial; only missions with known cargo are included |

Payload caveats:

- Mission 001 has `payload.cargo_lb: null`.
- Mission 003 has `payload.cargo_lb: null`.
- Descriptive baggage language should not be treated as numeric telemetry.

---

## Operational Events

| Event Type | Count | Completeness |
|---|---:|---|
| Successful Missions | 5 | Complete |
| Go-Arounds | 1 | Complete |
| Diversions | 0 | Complete |
| International Missions | 1 | Complete |

Notable events:

- Mission 001: inaugural charter operation and long coastal Florida routing
- Mission 002: repeat client continuity, Miami Bravo transit, and tower-directed go-around
- Mission 003: executive charter from KAPF to KFXE
- Mission 004: premium leisure charter from KFXE to KEYW
- Mission 005: first international charter and first Bahamas arrival

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

### Total Activity

| Airport | Activity Count |
|---|---:|
| KEYW | 2 |
| KAPF | 2 |
| KFXE | 2 |
| KMTH | 2 |
| KPNS | 1 |
| MYAM | 1 |

---

## International Operations

| Metric | Value |
|---|---:|
| Domestic Missions | 4 |
| International Missions | 1 |
| First International Mission | `MISSION_005_KEYW_MYAM` |
| Countries Operated In | 2 |

---

## Data Completeness Notes

Current known telemetry gaps:

- Mission 003 has `fuel.used_gal: null`.
- Mission 001 has `payload.cargo_lb: null`.
- Mission 003 has `payload.cargo_lb: null`.
- Some missions have incomplete timing values.
- `source_files.debrief` values are `null` where debrief links are not standardized.

Summary rules:

- `Total` means complete for the current telemetry scope.
- `Known total` means one or more missions have missing telemetry for that metric.
- Missing values are not treated as zero.
- Partial metrics must include caveats.

---

## Future Tracking Categories

As operations expand, continue tracking:

- executive charter count
- Bahamas missions
- Caribbean missions
- maintenance events
- reposition flights
- IFR mission count
- weather diversions
- recurring client frequency
- distance trends by region
- fuel efficiency by aircraft
- cargo and passenger utilization
