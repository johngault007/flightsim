# Client Continuity Closeout Workflow

---

## Purpose

Define how FlightOps should evaluate and update client continuity after a mission, PIREP, or debrief.

This workflow supports the authoritative structured client continuity file:

`/operations/current/data/clients.yaml`

Mission and debrief markdown files remain historical evidence sources. They feed client continuity updates but do not replace the structured client continuity data store.

---

## Source of Truth

Authoritative client continuity data:

`/operations/current/data/clients.yaml`

Secondary human-readable reference:

`/operations/current/career-state/clients-summary.md`

Company-level market context:

`/operations/current/career-state/company-profile.md`

Rules:

- `clients.yaml` is authoritative for individual client continuity.
- `clients-summary.md` is secondary/reference-only.
- If `clients-summary.md` conflicts with `clients.yaml`, the YAML file wins.
- `company-profile.md` provides general client market positioning only.
- Mission and debrief files are historical evidence sources.

---

## When to Review Client Continuity

Client continuity must be reviewed during mission closeout when any of the following are true:

- named passengers or clients are included in the mission, PIREP, or debrief
- a mission involves a repeat client
- passenger preferences, behavior, seating, service expectations, or operational considerations are observed
- a debrief includes useful client continuity notes
- a client-related inconsistency is discovered between mission records and `clients.yaml`

If no named client/passenger information exists and no continuity-relevant details are reported, no client continuity update is required.

---

## Closeout Sequence

During mission closeout, FlightOps should:

1. Review the mission record and PIREP/debrief for named clients or passengers.
2. Check `clients.yaml` for existing matching client records.
3. Determine whether each named client is new or existing.
4. Update `clients.yaml` first.
5. Append mission history entries where supported.
6. Append continuity notes where useful and supported.
7. Update `first_mission`, `last_mission`, and `repeat_count` where appropriate.
8. Leave unsupported values as `null`, empty lists, or `unknown`.
9. Update `clients-summary.md` only after `clients.yaml` is updated, if the summary needs to reflect the change.
10. Preserve mission and debrief markdown as historical records.

---

## Adding New Clients

Add a new client to `clients.yaml` when:

- a named client or passenger appears in a mission, PIREP, or debrief
- no existing client record clearly matches that person
- the name is specific enough to support continuity tracking

New client rules:

- Use the next available client key in the format `client_#####`.
- Store the numeric ID as a quoted string in `client_id`.
- Do not encode client type, region, status, or mission type into the ID.
- Set unsupported values to `null`, empty lists, or `unknown`.
- Add a mission history entry for the first supported mission.
- Add a continuity note only if there is useful supported context.

Example ID pattern:

```yaml
clients:
  client_00008:
    client_id: "00008"
    display_name: "Client Name"
```

---

## Updating Existing Clients

Update an existing client record when:

- the client appears in a new completed mission
- the PIREP/debrief provides supported new preference or operational information
- mission history needs to reflect a new completed client flight
- repeat count, first mission, or last mission should change
- pilot-confirmed correction or backfill is explicitly provided

Do not update an existing client record when:

- a detail is only implied by mission type
- a preference is guessed from narrative tone
- a weight is present but not mapped to a specific named passenger
- a VIP status is not explicitly supported
- a passenger behavior detail is ambiguous or not operationally useful

---

## Mission History Updates

Append a mission history entry for each completed mission involving the client when supported.

Recommended structure:

```yaml
mission_history:
  - mission_id: "MISSION_006_EXAMPLE"
    date: "YYYY-MM-DD"
    route: "ICAO → ICAO"
    departure: "ICAO"
    arrival: "ICAO"
    mission_type: "mission_type"
    role: "client"
    notes: "Concise supported mission-specific continuity note."
```

Mission history should not duplicate the full mission record. It should provide enough continuity context to support future mission generation and client tracking.

---

## Continuity Notes

Append continuity notes when the mission, PIREP, or debrief includes useful supported client information.

Use continuity notes for:

- passenger preferences
- behavior relevant to future flights
- seating or service considerations
- repeat-client context
- pilot-confirmed backfill or correction
- operationally relevant reactions to flight events

Recommended structure:

```yaml
continuity_notes:
  - date_utc: "YYYY-MM-DD"
    source: "MISSION_ID or pilot-confirmed backfill"
    text: |
      Supported continuity note text.
```

Continuity notes should be appended rather than overwriting useful historical context.

---

## No-Fabrication Rules

FlightOps must not fabricate unsupported client data.

Use:

- `null` for unknown scalar values
- empty lists for unsupported list fields
- `unknown` for unknown enum-like fields

Do not invent:

- client preferences
- VIP status
- passenger weights
- mission history
- preferred regions
- operational considerations
- repeat-client status
- personal traits beyond what is supported by records or pilot confirmation

Pilot-confirmed backfill is allowed, but must be clearly annotated when the original mission record did not contain the detail.

---

## Dormant Systems Boundary

This workflow does not activate dormant or future systems.

Do not introduce:

- billing
- CRM economics
- loyalty scoring
- reputation monetization
- finance-driven customer valuation
- AI-generated client drama outside actual flight operations

Client continuity is used only for operational realism, mission continuity, and dispatch context unless future systems are explicitly activated.

---

## Prompt Update Requirements

The FlightOps prompt should be updated so it knows to:

- load `clients.yaml` as the authoritative client continuity source
- treat `clients-summary.md` as secondary/reference-only
- review client continuity during mission closeout
- add new clients when supported by named mission/debrief data
- update existing clients when supported by completed mission records or pilot-confirmed corrections
- append mission history and continuity notes rather than overwriting useful history
- preserve uncertainty instead of guessing
- avoid activating dormant finance, CRM, loyalty, or reputation systems

Prompt implementation is tracked under Issue #39.

---

## Validation Scenarios

Use these scenarios to validate workflow behavior:

### New Client on First Mission

Expected behavior:

- add a new client ID
- create a client profile
- add first mission history
- leave unsupported details unknown

### Repeat Client on Follow-Up Mission

Expected behavior:

- update existing client
- append mission history
- increment repeat count
- update last mission
- append supported continuity notes

### Named Passengers with Limited Details

Expected behavior:

- add or update client identity and mission history
- leave preferences, weight, and VIP status unknown unless explicitly supported

### Operational Consideration Observed

Expected behavior:

- append relevant operational consideration or continuity note
- avoid overstating unsupported traits

### No Client Continuity Delta

Expected behavior:

- make no client continuity update
- preserve existing `clients.yaml` unchanged

### Ambiguous Debrief Detail

Expected behavior:

- do not guess
- preserve ambiguity as unknown
- optionally add a note only if the uncertainty itself is operationally relevant
