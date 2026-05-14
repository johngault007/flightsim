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