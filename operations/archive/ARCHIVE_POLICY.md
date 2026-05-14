# Archive Policy

## Policy Purpose

This document defines operational handling rules for historical persistence within:

/operations/archive/

The archive exists to preserve historical operational continuity while protecting the integrity of the active operational environment.

---

## Authority Rules

Authoritative live operational state always resides under:

/operations/current/

Archive data is historical reference only.

Archive data must not be treated as authoritative active operational state unless explicitly directed by the pilot.

Priority order:

1. /operations/current/
2. latest active mission and debrief records
3. operational snapshots
4. archive historical records

Archive exists for continuity reference, not active operational control.

---

## Archive Eligibility

The following data may be archived:

- completed mission briefings
- completed mission debriefs
- superseded operational state snapshots
- historical statistics
- deprecated operational doctrine records
- retired operational templates
- legacy company operational artifacts

Archive candidates are records no longer required for active operational continuity.

---

## Immutability Policy

Archived records should be treated as immutable historical artifacts.

Modification should occur only when:
- correcting repository errors
- repairing broken file structure
- intentional migration work
- documented archival maintenance

Archive history should remain stable.

---

## Active Operations Separation

Archive data must not:

- override current aircraft location
- override active company state
- override active operational doctrine
- override active statistics
- drive current mission generation
- replace active continuity state

Current operations always use:

/operations/current/

This separation protects deterministic operational continuity.

---

## Archival Workflow

Current archival behavior is manual.

Expected flow:

1. active operational record becomes historical
2. record is moved or copied into archive
3. active operational state remains current and authoritative

No automated archival lifecycle exists at this time.

---

## Future Policy Expansion

Potential future enhancements:

- automated archival rollover
- retention lifecycle rules
- archive pruning strategy
- schema version migration support
- historical template preservation
- long-term operational snapshots

These are future enhancements and are not currently required.

---

## Operational Principle

Archive preserves history.

Current drives operations.

Do not confuse the two unless you enjoy debugging continuity ghosts.