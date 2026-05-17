# Mission Statistics Summary Strategy

---

## Purpose

Define the role, format, and source-of-truth boundary for the Lost Bay Aviation human-readable mission statistics summary.

This strategy applies to:

`/operations/current/career-state/statistics.md`

The summary exists for quick human review and FlightOps situational awareness. It must not become the authoritative data source.

---

## Decision

`statistics.md` should be maintained as a useful human-readable statistics snapshot.

It should summarize calculated values from mission telemetry YAML and clearly identify incomplete-data caveats.

It should not store authoritative raw data or become a second analytics database.

---

## Source of Truth Boundary

Authoritative raw mission telemetry lives in:

`/operations/current/data/missions/YYYY/MISSION_ID.yaml`

Calculation rules live in:

`/operations/current/templates/mission-statistics-calculation-rules.md`

Human-readable summary lives in:

`/operations/current/career-state/statistics.md`

Rules:

- Telemetry YAML wins if it conflicts with `statistics.md`.
- `statistics.md` is secondary/reference-only.
- Derived statistics must be calculated from telemetry YAML before being summarized.
- Mission and debrief markdown remain narrative/historical records.
- `statistics.md` does not replace mission markdown, debrief markdown, or telemetry YAML.

---

## Recommended Summary Structure

`statistics.md` should use this structure:

```markdown
# Lost Bay Aviation Statistics Summary

## Source Note

## Current Summary

## Career Totals

## Distance & Route Statistics

## Fuel Statistics

## Payload Statistics

## Operational Events

## Mission Type Breakdown

## Flight Rules Breakdown

## Airport Activity

## International Operations

## Data Completeness Notes

## Future Tracking Categories
```

Sections may be omitted if not relevant yet, but the source note and data completeness notes should remain.

---

## Required Source Note

The summary should include a clear source note near the top:

```markdown
This file is a human-readable summary only.
Authoritative raw mission telemetry lives in:
`/operations/current/data/missions/YYYY/MISSION_ID.yaml`

If this file conflicts with telemetry YAML, telemetry YAML wins.
```

---

## Incomplete-Data Caveats

The summary must clearly distinguish complete and partial metrics.

Use labels such as:

- `Total` for complete metrics
- `Known total` for partial metrics
- `Average` for complete averages
- `Average known` for averages calculated from a subset
- `Unavailable` when required telemetry fields are missing

Examples:

```markdown
| Known Fuel Used | 72.87 gal across 4 of 5 missions |
| Fuel Caveat | Mission 003 has `fuel.used_gal: null` |
```

```markdown
| Total Distance Flown | 1,261 nm across 5 of 5 missions |
```

---

## Update Behavior

During future mission closeout, FlightOps should update `statistics.md` only after telemetry YAML is created or updated.

Recommended sequence:

1. Complete mission closeout.
2. Create/update mission telemetry YAML.
3. Apply any supported pilot-confirmed corrections.
4. Recalculate statistics from telemetry YAML.
5. Refresh `statistics.md` as a human-readable snapshot.
6. Preserve incomplete-data caveats.

Do not update `statistics.md` from memory or narrative-only assumptions when telemetry YAML exists.

---

## What Belongs in `statistics.md`

Appropriate content:

- career-level summary totals
- ready metric values
- known partial metric values with caveats
- breakdown tables
- data completeness notes
- operational trends derived from supported records
- future tracking categories

Avoid:

- raw mission telemetry duplication
- client profile details
- full mission narratives
- hidden manually maintained totals
- unsupported estimates presented as fact
- values that cannot be reproduced from telemetry YAML

---

## Markdown Preservation

Mission and debrief markdown files remain the narrative and historical layer.

`statistics.md` should not replace:

- mission overview
- route narrative
- passenger/service narrative
- pilot assessment
- debrief observations
- operational lessons learned

The summary should point to telemetry-derived results, not rewrite the story.

---

## Relationship to Prompt Work

Issue #39 should update FlightOps so it knows:

- `statistics.md` is a human-readable summary only
- telemetry YAML is authoritative
- statistics should be recalculated from telemetry YAML before summary updates
- incomplete-data caveats must be preserved
- `statistics.md` should not be used as the source of truth for analytics when telemetry YAML is available

---

## Validation Rules

Before updating or relying on `statistics.md`, confirm:

- source note is present
- metrics are reproducible from telemetry YAML
- partial metrics have caveats
- telemetry YAML remains authoritative
- no narrative markdown scraping is required for represented metrics
- mission/debrief markdown remains preserved
