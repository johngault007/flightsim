# Lost Bay Aviation FlightOps

AI-assisted persistent charter airline career operations framework for Microsoft Flight Simulator 2024.

Current architecture: **v2.2**

---

# Overview

Lost Bay Aviation FlightOps is a repository-backed operational framework for running a persistent charter aviation career using ChatGPT alongside Microsoft Flight Simulator 2024.

The project is designed to provide operational continuity across missions, allowing FlightOps to function as an AI-assisted dispatcher / operations director rather than a one-off mission generator.

Current focus areas:

- persistent operational continuity
- mission generation
- routing advisory support
- aircraft suitability awareness
- weather-aware operational planning
- structured client continuity
- PIREP intake and post-flight closeout support
- mission telemetry YAML capture
- telemetry-backed statistics and analytics
- debrief-driven state evolution
- repository-backed persistence
- validation and governance workflow discipline

Deferred future systems:

- financial simulation
- aircraft maintenance lifecycle simulation
- advanced dispatch decision intelligence
- CRM economics
- loyalty scoring
- client satisfaction scoring
- reputation monetization
- client-drama systems

Structured client continuity is active in v2.2. Broader CRM economics, loyalty scoring, satisfaction scoring, reputation monetization, finance, maintenance, and client-drama systems remain dormant unless explicitly activated later.

---

# Platform Compatibility

## Tested Environment

- ChatGPT Plus
- Microsoft Flight Simulator 2024

## AI Platform Notes

FlightOps v2.2 was designed and tested using ChatGPT Plus-class models with extended context handling.

This project has **not been validated on ChatGPT Free tier**, and free-tier platform limitations such as context size, message caps, or model availability may impact long-running persistent campaign usability.

For best results, ChatGPT Plus or equivalent is recommended.

---

# Repository Structure

```text
operations/
├── current/
│   ├── career-state/
│   │   ├── active-regions.md
│   │   ├── aircraft-fleet.md
│   │   ├── clients-summary.md
│   │   ├── company-profile.md
│   │   ├── finances.md
│   │   ├── operational-doctrine.md
│   │   ├── operational-history.md
│   │   ├── reputation-log.md
│   │   └── statistics.md
│   │
│   ├── data/
│   │   ├── clients.yaml
│   │   └── missions/
│   │       └── YYYY/
│   │           └── MISSION_ID.yaml
│   │
│   ├── missions/
│   │   └── YYYY/
│   │       └── MISSION_ID.md
│   │
│   ├── debriefs/
│   │   └── MISSION_ID_DEBRIEF.md
│   │
│   └── templates/
│       ├── client-continuity-closeout-workflow.md
│       ├── flightops-prompt-v22-workflow-validation.md
│       ├── mission-statistics-calculation-rules.md
│       ├── mission-statistics-incomplete-data-rules.md
│       ├── mission-statistics-summary-strategy.md
│       ├── mission-statistics-validation.md
│       ├── mission-telemetry-analytics-readiness.md
│       ├── mission-telemetry-closeout-workflow.md
│       └── mission-telemetry-schema.md
│
└── archive/
    ├── career-state/
    ├── data/
    ├── debriefs/
    ├── exports/
    ├── legacy/
    ├── missions/
    ├── templates/
    ├── ARCHIVE_POLICY.md
    ├── README.md
    └── RETENTION_RULES.md
```

---

# Operational Data Model

## Current State

Authoritative live operational state is maintained under:

```text
operations/current/
```

This includes:

- active company operational state
- aircraft fleet definitions
- operational doctrine
- active regional operating footprint
- structured client continuity
- structured mission telemetry
- mission and debrief narrative records
- telemetry-backed statistics summaries
- workflow and validation templates

This is the primary operational source used by FlightOps.

---

## Structured Data

Machine-readable operational data is maintained under:

```text
operations/current/data/
```

Current structured data includes:

- `clients.yaml` for authoritative client continuity
- `missions/YYYY/MISSION_ID.yaml` for authoritative raw mission telemetry

Structured data is used for continuity, telemetry normalization, and analytics.

---

## Human-Readable State

Human-readable current-state summaries are maintained under:

```text
operations/current/career-state/
```

Important summary files include:

- `clients-summary.md` for quick client continuity review
- `statistics.md` for quick statistics review
- `company-profile.md` for company identity and positioning
- `aircraft-fleet.md` for active aircraft status
- `operational-history.md` for operational continuity

Human-readable summaries are useful for quick review, but they do not override structured YAML where structured sources exist.

---

## Narrative Records

Mission and debrief markdown files remain narrative and historical records:

```text
operations/current/missions/
operations/current/debriefs/
```

These files preserve the operational story, briefing context, pilot observations, and debrief notes. They are not replaced by structured YAML.

---

## Historical State

Historical persistence is maintained under:

```text
operations/archive/
```

This includes:

- archived mission records
- archived debriefs
- historical operational state snapshots
- retained templates
- legacy preserved structures

Historical data exists for continuity and retention, not active operational control.

---

# Source of Truth Rules

FlightOps follows strict repository precedence rules:

1. Structured authoritative YAML under `operations/current/data/`
2. Mutable current-state markdown under `operations/current/career-state/`
3. Mission and debrief narrative records under `operations/current/missions/` and `operations/current/debriefs/`
4. Repository templates under `operations/current/templates/`
5. Archive records only when explicitly needed for historical reference

Structured YAML wins when it conflicts with human-readable summaries.

| Data Type | Authoritative Source | Human-Readable Layer |
|---|---|---|
| Client continuity | `operations/current/data/clients.yaml` | `operations/current/career-state/clients-summary.md` |
| Mission telemetry | `operations/current/data/missions/YYYY/MISSION_ID.yaml` | mission/debrief markdown |
| Derived statistics | calculated from mission telemetry YAML | `operations/current/career-state/statistics.md` |
| Narrative mission context | mission/debrief markdown | same files |
| Company identity | `operations/current/career-state/company-profile.md` | same file |

Additional rules:

- `operations/current/` is authoritative live operational state
- `operations/archive/` is historical reference only
- archive data does not override current state
- mission and debrief markdown remain historical narrative records
- structured YAML stores raw facts, not long-form narrative
- derived statistics are calculated from telemetry YAML
- `null` values represent unknown data, not zero
- unsupported values should remain unknown rather than invented

This separation exists to prevent continuity drift, state ambiguity, and spreadsheet cosplay pretending to be truth.

---

# Operational Workflow

Standard FlightOps operational cycle:

1. Initialize FlightOps session
2. Load current operational state from `operations/current/`
3. Load structured sources from `operations/current/data/` when present
4. Generate a mission assignment using aircraft, region, weather, history, and supported client continuity
5. Review weather / routing / aircraft suitability
6. Fly mission in Microsoft Flight Simulator 2024
7. Capture in-flight PIREPs when provided
8. Attach automatic UTC receipt timestamps to PIREPs
9. Complete post-flight debrief
10. Normalize supported PIREP/debrief facts into mission telemetry YAML
11. Update client continuity when supported
12. Recalculate or refresh statistics summaries from telemetry YAML when appropriate
13. Preserve mission/debrief markdown as narrative records
14. Archive historical records as appropriate

---

# Setup / Usage

## Initial Setup

Required:

- ChatGPT Plus or equivalent
- Microsoft Flight Simulator 2024
- access to this repository
- current FlightOps system prompt: `msfs2024_gpt_prompt.txt`

Recommended:

- structured debrief discipline
- consistent mission closeout workflow
- repository version control hygiene
- clear distinction between raw telemetry, summaries, and narrative records

---

## Session Startup

At the beginning of an operational session:

1. Load the FlightOps prompt into ChatGPT
2. Ensure FlightOps has access to current operational state files
3. Confirm active aircraft fleet status
4. Confirm operational doctrine
5. Confirm active regional footprint
6. Load structured client continuity and mission telemetry when relevant
7. Request mission generation

Example startup intent:

```text
Initialize FlightOps for Lost Bay Aviation. Load current operational state and prepare dispatch.
```

---

## Mission Generation

FlightOps can generate missions based on:

- operational continuity
- active operating region
- aircraft suitability
- weather conditions
- mission history
- company doctrine
- structured client continuity when relevant
- pilot preference trends
- narrative continuity

Example:

```text
Generate the next charter assignment for Lost Bay Aviation.
```

---

## PIREP and Telemetry Closeout

FlightOps supports in-flight and post-flight PIREP intake.

PIREPs may include:

- aircraft position or phase of flight
- altitude
- weather observations
- fuel state
- route changes
- ATC complications
- passenger/client observations
- timing values
- diversions or go-arounds
- pilot preference feedback

Every PIREP receives an automatic UTC receipt timestamp. That timestamp records when FlightOps received the report. It must not be used as an operational event timestamp such as OUT, OFF, TOC, TOD, ON, IN, or shutdown unless the pilot explicitly confirms that use.

Supported PIREP and debrief facts may be normalized into telemetry YAML during closeout.

---

## Post-Mission Debrief

After completing a mission, provide:

- flight summary
- operational anomalies
- weather observations
- aircraft performance notes
- passenger/client notes
- route deviations
- go-arounds / diversions / incidents
- fuel, timing, payload, or telemetry corrections when known

FlightOps uses debriefs to evolve operational continuity and update structured telemetry when facts are supported.

Example:

```text
Debrief mission MISSION002.
```

---

## Statistics and Analytics

FlightOps statistics and analytics are derived from mission telemetry YAML under:

```text
operations/current/data/missions/YYYY/MISSION_ID.yaml
```

`operations/current/career-state/statistics.md` is a human-readable summary only. It should reflect values derived from telemetry YAML and preserve incomplete-data caveats.

Current v2.2 statistics behavior includes:

- ready / partially ready / unavailable metric classification
- `Total` only for complete metrics
- `Known total` for partial metrics
- `Average known` for averages calculated from supported subsets
- incomplete-data caveats for missing fuel, cargo, timing, or other telemetry
- `null` treated as unknown, not zero

---

# Current FlightOps Scope

Implemented capabilities:

- persistent mission continuity
- regional operational retention logic
- weather-aware mission support
- routing advisory support
- aircraft suitability awareness
- repository-backed persistence
- structured client continuity
- PIREP intake with automatic UTC receipt timestamps
- mission telemetry YAML capture
- telemetry-backed statistics and analytics
- incomplete-data handling for statistics
- debrief-driven continuity evolution
- operational doctrine enforcement
- validation and governance artifacts

---

# Deferred Systems

Planned future enhancements:

- financial simulation
- aircraft maintenance lifecycle
- advanced dispatch decision intelligence
- CRM economics
- loyalty scoring
- client satisfaction scoring
- reputation monetization
- client-drama systems

These systems are intentionally dormant. They should not be simulated unless explicitly activated by repository state or pilot instruction.

---

# Version

Current architecture:

**v2.2**

Major architectural changes from v2.1:

- structured client continuity using `operations/current/data/clients.yaml`
- human-readable client summary via `operations/current/career-state/clients-summary.md`
- structured mission telemetry YAML under `operations/current/data/missions/YYYY/`
- telemetry-backed statistics and analytics
- `statistics.md` clarified as a human-readable summary only
- automatic UTC receipt timestamps for PIREPs
- PIREP-to-telemetry closeout workflow
- explicit incomplete-data handling for statistics
- final prompt workflow validation artifact
- strengthened source-of-truth boundaries between YAML, markdown summaries, narrative records, templates, and archive state

---

# Validation

FlightOps v2.2 prompt workflow validation is documented at:

```text
operations/current/templates/flightops-prompt-v22-workflow-validation.md
```

The validation confirms representative workflow behavior for:

- session initialization and source loading
- mission generation with supported client continuity
- in-flight PIREP intake
- PIREP receipt timestamp handling
- telemetry capture guidance
- mission closeout with client continuity deltas
- statistics from telemetry YAML
- incomplete-data caveats
- narrative markdown preservation
- no-fabrication behavior
- current/archive repository compatibility
