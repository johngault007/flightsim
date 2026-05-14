# Lost Bay Aviation FlightOps

AI-assisted persistent charter airline career operations framework for Microsoft Flight Simulator 2024.

---

# Overview

Lost Bay Aviation FlightOps is a repository-backed operational framework for running a persistent charter airline career using ChatGPT alongside Microsoft Flight Simulator 2024.

The project is designed to provide operational continuity across missions, allowing FlightOps to function as an AI-assisted dispatcher / operations director rather than a one-off mission generator.

Current focus areas:

- persistent operational continuity
- mission generation
- routing advisory support
- aircraft suitability awareness
- weather-aware operational planning
- debrief-driven state evolution
- repository-backed persistence

Deferred future systems:

- financial simulation
- maintenance lifecycle simulation
- persistent client management
- advanced dispatch intelligence

---

# Platform Compatibility

## Tested Environment

- ChatGPT Plus
- Microsoft Flight Simulator 2024

## AI Platform Notes

FlightOps v2.1 was designed and tested using ChatGPT Plus-class models with extended context handling.

This project has **not been validated on ChatGPT Free tier**, and free-tier platform limitations (context size, message caps, model availability) may impact long-running persistent campaign usability.

For best results, ChatGPT Plus or equivalent is recommended.

---

# Repository Structure

```text
operations/
├── current/
│   ├── career-state/
│   │   ├── active-regions.md
│   │   ├── aircraft-fleet.md
│   │   ├── company-profile.md
│   │   ├── finances.md
│   │   ├── operational-doctrine.md
│   │   ├── operational-history.md
│   │   ├── reputation-log.md
│   │   └── statistics.md
│   │
│   ├── missions/
│   │   └── 2026/
│   │
│   ├── debriefs/
│   │
│   └── templates/
│
└── archive/
    ├── career-state/
    ├── debriefs/
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
- live mission records
- current statistics
- active regional operating footprint

This is the primary operational source of truth used by FlightOps.

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

- `operations/current/` is authoritative live operational state
- `operations/archive/` is historical reference only
- archive data does not override current state
- historical records should remain immutable unless correction is explicitly required
- current state should always reflect the latest operational truth

This separation exists to prevent continuity drift and state ambiguity.

---

# Operational Workflow

Standard FlightOps operational cycle:

1. Initialize FlightOps session
2. Load current operational state
3. Generate mission assignment
4. Review weather / routing / aircraft suitability
5. Fly mission in Microsoft Flight Simulator 2024
6. Complete post-flight debrief
7. Update current operational state
8. Archive historical records as appropriate

---

# Setup / Usage

## Initial Setup

Required:

- ChatGPT Plus
- Microsoft Flight Simulator 2024
- local copy of repository files
- FlightOps system prompt

Recommended:

- structured debrief discipline
- consistent mission archival
- repository version control hygiene

---

## Session Startup

At the beginning of an operational session:

1. Load the FlightOps prompt into ChatGPT
2. Ensure FlightOps has access to current operational state files
3. Confirm active aircraft fleet status
4. Confirm operational doctrine
5. Confirm active regional footprint
6. Request mission generation

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
- narrative continuity

Example:

```text
Generate the next charter assignment for Lost Bay Aviation.
```

---

## Post-Mission Debrief

After completing a mission:

- provide flight summary
- operational anomalies
- weather observations
- aircraft performance notes
- passenger/client notes
- deviations
- go-arounds / diversions / incidents

FlightOps uses debriefs to evolve operational continuity.

Example:

```text
Debrief mission MISSION002.
```

---

# Current FlightOps Scope

Implemented capabilities:

- persistent mission continuity
- regional operational retention logic
- weather-aware mission support
- routing advisory support
- aircraft suitability awareness
- repository-backed persistence
- debrief-driven continuity evolution
- operational doctrine enforcement

---

# Deferred Systems

Planned future enhancements:

- financial simulation
- aircraft maintenance lifecycle
- client persistence / CRM-lite continuity
- advanced dispatch decision intelligence

These systems are intentionally deferred pending v2.1 operational validation.

---

# Version

Current architecture:

**v2.1**

Major architectural changes from earlier versions:

- repository refactor to current/archive operational model
- deterministic source-of-truth separation
- improved continuity control
- structured persistence boundaries
- future subsystem architectural scaffolding

---