# Archive Operations

## Purpose

The `/operations/archive/` directory stores historical operational records for Lost Bay Aviation's persistent Microsoft Flight Simulator 2024 career environment.

This archive exists to preserve completed operational history while keeping the active operational state clean, lightweight, and authoritative.

Archive data supports:
- historical mission review
- debrief history preservation
- operational retrospective analysis
- long-term career continuity reference
- future archival lifecycle expansion

---

## Source of Truth

`/operations/archive/` is **not** the active source of truth.

Authoritative live operational state always resides under:

/operations/current/

FlightOps should use archived data only for historical reference unless explicitly directed otherwise.

Archive records must never override active operational state.

---

## Directory Structure

### missions/
Historical mission briefing records.

Examples:
- completed mission briefs
- superseded mission records
- archived operational assignments

---

### debriefs/
Historical post-flight operational debrief records.

Examples:
- completed mission debriefs
- legacy debrief history
- historical operational assessments

---

### career-state/
Historical snapshots of prior operational state.

Examples:
- legacy aircraft state
- prior operational history snapshots
- historical statistics
- deprecated operational state records

This directory exists for historical preservation only.

---

## Operational Philosophy

Archive storage exists to support continuity without polluting active operational workflows.

Active operations should remain focused on:
- current aircraft state
- active mission continuity
- current operational doctrine
- active regional operations
- current statistics and reputation

Archive exists to preserve history, not drive active dispatch behavior.

---

## Future Expansion

Potential future enhancements may include:
- automated archival rollover
- retention lifecycle management
- archive schema versioning
- legacy template preservation
- migration history tracking

These capabilities are optional future enhancements and are not required for current operation.