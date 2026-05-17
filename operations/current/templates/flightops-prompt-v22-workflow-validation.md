# FlightOps Prompt v2.2 Workflow Validation

---

## Purpose

Validate that the completed FlightOps v2.2 prompt update supports the required source-of-truth, client continuity, PIREP intake, telemetry capture, statistics, no-fabrication, and repository compatibility behavior.

This validation supports:

- Issue #39 — Update FlightOps prompt for v2.2 continuity and telemetry workflows
- Issue #69 — Validate FlightOps prompt v2.2 workflow behavior

Validated prompt artifact:

`msfs2024_gpt_prompt.txt`

---

## Validation Result

Status: **Ready for final v2.2 prompt handoff**

The prompt now contains the required v2.2 workflow behavior for:

- authoritative source loading
- structured client continuity
- PIREP intake with automatic UTC receipt timestamps
- mission telemetry YAML capture
- statistics and analytics from telemetry YAML
- incomplete-data reporting
- no-fabrication rules
- narrative markdown preservation
- current/archive repository compatibility

No major repository refactor assumptions were introduced.

---

## Validated Source Areas

Prompt areas reviewed:

- `V2.2 SOURCE OF TRUTH HIERARCHY`
- `SOURCE-OF-TRUTH DOMAIN MAP`
- `STRUCTURED CLIENT CONTINUITY DOCTRINE`
- `MISSION GENERATION WITH CLIENT CONTINUITY`
- `CLIENT CLOSEOUT REVIEW TRIGGERS`
- `PIREP INTAKE DOCTRINE`
- `AUTOMATIC UTC RECEIPT TIMESTAMP RULE`
- `PIREP-TO-TELEMETRY NORMALIZATION`
- `MISSION TELEMETRY SOURCE OF TRUTH`
- `UNKNOWN / UNSUPPORTED TELEMETRY VALUES`
- `PARSER-FRIENDLY YAML RULES`
- `TELEMETRY CLIENT REFERENCES`
- `MISSION STATISTICS AND ANALYTICS`
- `STATISTICS READINESS CATEGORIES`
- `INCOMPLETE DATA RULES`
- `CURRENT V2.2 VALIDATION BASELINE`
- `REPOSITORY SOURCE OF TRUTH`
- `PERSISTENCE WORKFLOW`
- `OPERATIONAL LIMITS AND FUTURE SYSTEM BOUNDARIES`

Related statistics artifacts reviewed:

- `/operations/current/templates/mission-statistics-calculation-rules.md`
- `/operations/current/templates/mission-statistics-incomplete-data-rules.md`
- `/operations/current/templates/mission-statistics-summary-strategy.md`
- `/operations/current/templates/mission-statistics-validation.md`
- `/operations/current/career-state/statistics.md`

---

## Scenario Validation

### 1. Session Initialization and Source Loading

Result: **Pass**

Validated behavior:

- Prompt identifies `/operations/current/` as active authoritative state.
- Prompt prioritizes structured YAML under `/operations/current/data/`.
- Prompt treats `/operations/current/data/clients.yaml` as authoritative for client continuity.
- Prompt treats `/operations/current/data/missions/YYYY/MISSION_ID.yaml` as authoritative for raw mission telemetry.
- Prompt treats `/operations/current/career-state/statistics.md` and `/operations/current/career-state/clients-summary.md` as human-readable summaries only.
- Prompt preserves mission/debrief markdown as narrative and historical records.
- Prompt states structured YAML wins when it conflicts with human-readable summaries.

Acceptance coverage:

- Validate prompt initializes FlightOps with the correct authoritative current-state files.

---

### 2. Mission Generation with Supported Client Continuity

Result: **Pass**

Validated behavior:

- Prompt instructs FlightOps to load and consider `clients.yaml` when client continuity matters.
- Prompt permits supported preferences and operational considerations to influence mission generation.
- Prompt prevents overusing repeat clients unless operationally justified.
- Prompt treats client continuity as realism support rather than artificial drama.
- Prompt prevents dormant CRM, loyalty, reputation monetization, satisfaction scoring, and client-drama behavior from activating.

Acceptance coverage:

- Validate prompt can generate a mission using supported client continuity without overusing repeat clients.

---

### 3. In-Flight PIREP Intake with Automatic UTC Receipt Timestamp

Result: **Pass**

Validated behavior:

- Prompt requires every PIREP or in-flight pilot report to receive an automatic UTC receipt timestamp.
- Prompt says the timestamp records when FlightOps received the report.
- Prompt preserves pilot-provided wording when practical.
- Prompt supports PIREP facts including aircraft position, weather, fuel, timing, route changes, client observations, and operational events.

Acceptance coverage:

- Validate prompt processes an in-flight PIREP and automatically applies a UTC receipt timestamp.

---

### 4. PIREP Timestamp Separation from Operational Event Times

Result: **Pass**

Validated behavior:

- Prompt explicitly distinguishes UTC receipt timestamp from operational event occurrence time.
- Prompt prevents OUT, OFF, TOC, TOD, ON, IN, shutdown, or other event timestamps from being backfilled from PIREP receipt time alone.
- Prompt allows event timestamps only from pilot-provided, simulator-reported, mission record, debrief record, other source-supported, or pilot-confirmed correction values.
- Prompt leaves unsupported event timestamps unknown/null.

Acceptance coverage:

- Validate prompt does not misuse PIREP receipt timestamps as OUT/OFF/TOC/TOD/ON/IN/shutdown event timestamps.

---

### 5. Simulated PIREP Closeout and Telemetry Capture Guidance

Result: **Pass**

Validated behavior:

- Prompt directs FlightOps to normalize supported PIREP facts into mission telemetry YAML during mission closeout.
- Prompt references `/operations/current/templates/mission-telemetry-closeout-workflow.md` and `/operations/current/templates/mission-telemetry-schema.md`.
- Prompt maps supported PIREP values into aircraft, route, timing, fuel, payload, operations, client_refs, and source_files.
- Prompt requires unsupported values to remain `null`, `unknown`, or `[]` as appropriate.
- Prompt defines canonical telemetry path `/operations/current/data/missions/YYYY/MISSION_ID.yaml`.
- Prompt preserves canonical telemetry sections.
- Prompt prevents derived statistics from being stored inside individual mission telemetry files.

Acceptance coverage:

- Validate prompt can complete a simulated PIREP and produce telemetry capture guidance.

---

### 6. Mission Closeout with Client Continuity Deltas

Result: **Pass**

Validated behavior:

- Prompt requires client continuity review after mission completion.
- Prompt defines triggers for named passengers, repeat clients, PIREP/debrief client details, and operationally useful passenger observations.
- Prompt requires checking `clients.yaml` before adding new clients.
- Prompt defines when new clients may be added.
- Prompt defines when existing clients should be updated.
- Prompt requires concise mission history entries and continuity notes rather than duplicating full mission records.
- Prompt requires `clients-summary.md` updates only after `clients.yaml` updates.

Acceptance coverage:

- Validate prompt can complete mission closeout and identify required client continuity updates.

---

### 7. Operational Statistics from Telemetry YAML

Result: **Pass**

Validated behavior:

- Prompt directs FlightOps to answer statistics and analytics questions from per-mission telemetry YAML.
- Prompt identifies telemetry YAML as the source of truth for raw mission facts used in analytics.
- Prompt treats `statistics.md` as a human-readable summary only.
- Prompt references the statistics calculation, incomplete-data, summary strategy, and validation artifacts.
- Prompt defines core statistic field sources such as `route.distance_nm`, `fuel.used_gal`, `payload.passengers`, `payload.cargo_lb`, `timing.airborne_time_hr`, `timing.block_time_hr`, `operations.result`, `operations.go_arounds`, `operations.diversions`, `operations.mission_type`, `operations.flight_rules`, `route.departure`, `route.arrival`, and `operations.international`.
- Prompt includes the Missions 001–005 validation baseline.

Validated current baseline:

| Metric | Validated Value |
|---|---:|
| Mission count | 5 |
| Successful missions | 5 |
| Success rate | 100% |
| Total distance flown | 1,261 nm |
| Average mission distance | 252.2 nm |
| Total passengers transported | 9 |
| Passenger miles | 2,435 passenger-nm |
| Go-arounds | 1 |
| Diversions | 0 |
| International missions | 1 |
| Known fuel used | 72.87 gal |
| Known cargo / baggage transported | 255 lb |

Acceptance coverage:

- Validate prompt answers operational statistics from telemetry YAML.

---

### 8. Incomplete-Data Caveats for Partial Statistics

Result: **Pass**

Validated behavior:

- Prompt classifies statistics as Ready, Partially ready, or Unavailable.
- Prompt uses `Total` only when complete for the requested scope.
- Prompt uses `Known total` and `Average known` when missions are excluded due to missing data.
- Prompt treats `null` as unknown, not zero.
- Prompt does not silently drop missions from calculations.
- Prompt identifies excluded missions when useful.
- Prompt includes current caveats:
  - Mission 003 missing `fuel.used_gal`
  - Missions 001 and 003 missing `payload.cargo_lb`

Acceptance coverage:

- Validate prompt includes incomplete-data caveats for partial statistics.

---

### 9. Narrative Markdown Preservation

Result: **Pass**

Validated behavior:

- Prompt explicitly preserves mission and debrief markdown as narrative and historical records.
- Prompt prevents mission/debrief markdown from being replaced by structured YAML.
- Prompt prevents narrative markdown scraping for metrics already represented in telemetry YAML.
- Prompt keeps mission briefing and debrief artifacts immutable once finalized unless correcting a confirmed typo or pilot-confirmed record error.

Acceptance coverage:

- Validate prompt preserves mission/debrief markdown as narrative records.

---

### 10. No-Fabrication and Data Integrity

Result: **Pass**

Validated behavior:

- Prompt prevents unsupported client, telemetry, timing, fuel, cargo, route, operational, and statistics values from being fabricated.
- Prompt preserves uncertainty through `null`, `unknown`, and `[]`.
- Prompt prevents route distance calculation from route strings unless explicitly approved.
- Prompt prevents fuel used from being calculated from departure and arrival values unless explicitly pilot-confirmed or source-supported.
- Prompt prevents unknown mission results, international flags, go-around counts, and diversion counts from being treated as concrete values.
- Prompt requires source support for corrections.

Acceptance coverage:

- Validate prompt does not fabricate missing client, telemetry, timing, fuel, cargo, route, operational, or statistics values.

---

### 11. Repository Compatibility and No Major Refactor

Result: **Pass**

Validated behavior:

- Prompt remains compatible with the existing `/operations/current/` and `/operations/archive/` model.
- Prompt uses `/operations/current/data/` for structured authoritative sources.
- Prompt keeps `/operations/current/career-state/` for mutable human-readable summaries and state files.
- Prompt keeps `/operations/current/missions/` and `/operations/current/debriefs/` as narrative historical records.
- Prompt keeps `/operations/current/templates/` as workflow and schema references.
- Prompt does not introduce a major repository structure change.

Acceptance coverage:

- Validate prompt does not introduce major repository refactor assumptions.

---

## Acceptance Criteria Mapping

| #69 Acceptance Criterion | Result |
|---|---|
| Validate prompt initializes FlightOps with the correct authoritative current-state files | Pass |
| Validate prompt can generate a mission using supported client continuity without overusing repeat clients | Pass |
| Validate prompt processes an in-flight PIREP and automatically applies a UTC receipt timestamp | Pass |
| Validate prompt does not misuse PIREP receipt timestamps as OUT/OFF/TOC/TOD/ON/IN/shutdown event timestamps | Pass |
| Validate prompt can complete a simulated PIREP and produce telemetry capture guidance | Pass |
| Validate prompt can complete mission closeout and identify required client continuity updates | Pass |
| Validate prompt answers operational statistics from telemetry YAML | Pass |
| Validate prompt includes incomplete-data caveats for partial statistics | Pass |
| Validate prompt preserves mission/debrief markdown as narrative records | Pass |
| Validate prompt does not fabricate missing client, telemetry, timing, fuel, cargo, route, operational, or statistics values | Pass |
| Validate prompt does not introduce major repository refactor assumptions | Pass |
| Add final readiness comment to Issue #39 | Pending until #69 review is complete |

---

## Final Notes

The prompt is ready for final #39 readiness handoff after this validation branch is reviewed and merged.

Recommended next step after merge:

- Add final readiness comment to Issue #39.
- Close Issue #69.
- Close or complete Story #39 if no additional prompt-governance gaps remain.
