# changelog

all notable changes to lost bay aviation flightops will be documented in this file.

---

## v1

initial operational prototype architecture.

baseline characteristics:

- ai-assisted charter mission generation
- persistent operational continuity concepts
- repository-backed markdown state management
- operational doctrine enforcement
- aircraft fleet awareness
- weather-aware mission planning
- routing advisory support
- debrief-driven continuity updates

limitations:

- flatter repository structure
- less deterministic startup behavior
- weaker source-of-truth separation
- limited archival strategy
- reduced continuity conflict handling
- no formal future subsystem boundaries

---

## v2.1

major architectural refactor and operational framework hardening.

### architecture

- refactored canonical flightops prompt architecture
- migrated repository structure to `operations/current` and `operations/archive`
- added historical persistence scaffolding for long-term campaign continuity
- defined authoritative current state vs historical archive behavior
- hardened repository source-of-truth precedence rules
- formalized immutable vs mutable persistence behavior
- added dormant future subsystem boundaries:
  - finance
  - maintenance
  - clients
  - dispatch ai

---

### flightops operational logic

- added deterministic startup and session initialization logic
- added continuity conflict detection and recovery handling
- improved deterministic mission generation and operational weighting logic
- formalized regional retention mission behavior ("stay in area" continuity)
- formalized aircraft operational constraints and loading doctrine enforcement
- formalized live weather-aware operational planning integration
- improved routing advisory doctrine and airac compatibility guidance
- improved flightops role definition and execution model
- expanded deterministic dispatch decision priority ordering
- tightened operational realism doctrine
- removed duplicate operational instructions

---

### persistence and governance

- formalized archive governance behavior
- added archive retention policy framework
- standardized repository template enforcement behavior
- improved continuity state discipline across sessions

---

### documentation

- updated repository operational documentation for v2.1 architecture
- added structured setup and usage guidance
- documented operational workflow lifecycle
- documented platform assumptions and runtime expectations
- documented current flightops scope boundaries
- documented deferred future enhancement scope

---

### tested environment

validated environment:

- chatgpt plus
- microsoft flight simulator 2024

note:

chatgpt free-tier compatibility has not been formally tested.

---

## v2.2

structured continuity, telemetry, statistics, and prompt-governance update.

### architecture and source of truth

- added active structured data layer under `operations/current/data/`
- defined `/operations/current/data/clients.yaml` as authoritative client continuity
- defined `/operations/current/data/missions/YYYY/MISSION_ID.yaml` as authoritative raw mission telemetry
- clarified `/operations/current/career-state/clients-summary.md` as a human-readable client summary only
- clarified `/operations/current/career-state/statistics.md` as a human-readable statistics summary only
- reinforced structured yaml precedence over human-readable markdown summaries
- preserved mission and debrief markdown as narrative and historical records
- preserved `operations/current` as live operational state and `operations/archive` as historical reference only

---

### client continuity

- added structured client continuity using stable `client_#####` yaml keys and quoted numeric `client_id` values
- added active client records in `clients.yaml`
- added client continuity summary support through `clients-summary.md`
- defined client closeout review triggers for named passengers, repeat clients, debrief details, PIREPs, and operationally useful passenger observations
- defined new-client and existing-client update rules
- preserved uncertainty with `null`, `unknown`, and empty lists where appropriate
- prevented unsupported client preferences, vip status, weights, history, operational considerations, or client drama from being fabricated
- kept broader crm economics, loyalty scoring, satisfaction scoring, reputation monetization, and client-drama systems dormant

---

### mission telemetry and pirep workflow

- added canonical mission telemetry yaml path convention: `/operations/current/data/missions/YYYY/MISSION_ID.yaml`
- added mission telemetry schema guidance with canonical top-level sections:
  - `schema_version`
  - `mission_id`
  - `mission_number`
  - `date`
  - `aircraft`
  - `route`
  - `timing`
  - `fuel`
  - `payload`
  - `operations`
  - `client_refs`
  - `source_files`
- added telemetry closeout workflow for normalizing supported PIREP and debrief facts into mission telemetry yaml
- added parser-friendly yaml rules for indentation, quoted mission ids, quoted route strings, quoted client ids, utc timestamp handling, snake_case fields, and unit-bearing field names
- added automatic utc receipt timestamp behavior for every PIREP or in-flight pilot report
- clarified that PIREP receipt timestamps record when FlightOps received the report, not when the operational event occurred
- prevented PIREP receipt timestamps from being backfilled as OUT, OFF, TOC, TOD, ON, IN, shutdown, or other operational event timestamps unless explicitly pilot-confirmed
- required unsupported telemetry values to remain `null`, `unknown`, or empty lists rather than being guessed

---

### statistics and analytics

- added structured mission statistics calculation rules
- added incomplete-data handling rules for partial statistics
- added statistics summary strategy for `/operations/current/career-state/statistics.md`
- validated current statistics from telemetry yaml without scraping mission or debrief narrative markdown
- defined ready, partially ready, and unavailable metric classifications
- required `total` only for complete metrics and `known total` or `average known` for partial metrics
- reinforced that `null` is unknown, not zero
- prevented missing fuel, cargo, timing, route distance, operational results, and statistics values from being fabricated
- defined telemetry-backed metric sources including `route.distance_nm`, `fuel.used_gal`, `payload.passengers`, `payload.cargo_lb`, timing fields, operation fields, and airport fields

current validated statistics baseline:

| metric | value |
|---|---:|
| mission count | 5 |
| successful missions | 5 |
| success rate | 100% |
| total distance flown | 1,261 nm |
| average mission distance | 252.2 nm |
| total passengers transported | 9 |
| passenger miles | 2,435 passenger-nm |
| go-arounds | 1 |
| diversions | 0 |
| international missions | 1 |
| known fuel used | 72.87 gal |
| known cargo / baggage transported | 255 lb |

known incomplete-data caveats:

- mission 003 has `fuel.used_gal: null`
- missions 001 and 003 have `payload.cargo_lb: null`
- some timing and debrief source references remain incomplete where source support is unavailable

---

### flightops prompt behavior

- updated the FlightOps prompt to version 2.2
- added v2.2 source-of-truth loading behavior
- added structured client continuity behavior
- added PIREP intake and mission telemetry capture behavior
- added statistics and analytics behavior
- reinforced no-fabrication behavior across client continuity, telemetry, timing, fuel, cargo, route, operations, and statistics
- preserved dormant finance, maintenance, reputation expansion, fleet lifecycle, dispatch intelligence expansion, crm economics, loyalty scoring, satisfaction scoring, reputation monetization, and client-drama systems unless explicitly activated

---

### validation and governance

- added final FlightOps prompt v2.2 workflow validation artifact
- validated representative workflows for:
  - session initialization and source loading
  - mission generation with supported client continuity
  - in-flight PIREP intake with automatic utc receipt timestamp
  - PIREP receipt timestamp separation from operational event timestamps
  - PIREP closeout and telemetry capture guidance
  - mission closeout with client continuity deltas
  - operational statistics from telemetry yaml
  - incomplete-data caveats for partial statistics
  - mission/debrief markdown preservation
  - no-fabrication behavior
  - current/archive repository compatibility
- completed final prompt readiness handoff for the v2.2 continuity and telemetry workflow update

---

### documentation

- updated top-level README documentation for v2.2 architecture and source-of-truth behavior
- documented the active structured data layer
- documented current active vs dormant systems
- documented telemetry-backed statistics behavior
- documented PIREP and telemetry closeout expectations
- documented final validation artifact location

---