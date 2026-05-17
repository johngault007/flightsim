# Client Continuity Summary

---

## Source of Truth

The authoritative client continuity data source is:

`/operations/current/data/clients.yaml`

This summary is a secondary human-readable reference only.

If this summary conflicts with `clients.yaml`, the YAML file wins.

---

## Purpose

This file provides a quick operational overview of known Lost Bay Aviation clients without replacing the structured client continuity data store.

Use this summary for:

- quick client awareness during mission planning
- lightweight repeat-client review
- high-level operational continuity context
- human-readable scanning

Do not use this summary as the authoritative source for:

- client IDs
- detailed mission history
- structured continuity notes
- passenger weights
- VIP status
- operational source-of-truth decisions

---

## Current Client Roster

| Client ID | Client | Type | Status | Repeat Count | First Mission | Last Mission |
|---|---|---|---|---:|---|---|
| 00001 | Chad Johnson | Premium Leisure | Active | 2 | MISSION_001_KPNS_KMTH | MISSION_002_KMTH_KAPF |
| 00002 | Nancy Johnson | Premium Leisure | Active | 2 | MISSION_001_KPNS_KMTH | MISSION_002_KMTH_KAPF |
| 00003 | Elias Mercer | Executive | Active | 1 | MISSION_003_KAPF_KFXE | MISSION_003_KAPF_KFXE |
| 00004 | Emily Whitmore | Premium Leisure | Active | 1 | MISSION_004_KFXE_KEYW | MISSION_004_KFXE_KEYW |
| 00005 | Daniel Whitmore | Premium Leisure | Active | 1 | MISSION_004_KFXE_KEYW | MISSION_004_KFXE_KEYW |
| 00006 | Richard Bennett | Premium Leisure International | Active | 1 | MISSION_005_KEYW_MYAM | MISSION_005_KEYW_MYAM |
| 00007 | Claire Bennett | Premium Leisure International | Active | 1 | MISSION_005_KEYW_MYAM | MISSION_005_KEYW_MYAM |

---

## Operational Highlights

### Chad Johnson

- Repeat premium leisure client
- Aviation-curious
- Known weight: 200 lb
- Should generally be seated aft unless operationally appropriate
- Interested in airline traffic and cockpit/system details

### Nancy Johnson

- Repeat premium leisure client
- Scenery-focused
- Known weight: 160 lb
- Calm passenger profile
- Enjoys photo opportunities and scenic routing

### Elias Mercer

- Executive charter client
- First recorded mission: KAPF → KFXE
- No supported personal preferences or weight details currently recorded

### Emily Whitmore

- Premium leisure client
- First recorded mission: KFXE → KEYW
- Individual passenger weight is not mapped by name in the source record

### Daniel Whitmore

- Premium leisure client
- First recorded mission: KFXE → KEYW
- Individual passenger weight is not mapped by name in the source record

### Richard Bennett

- Premium leisure international client
- First recorded mission: KEYW → MYAM
- Associated with Lost Bay Aviation's first international charter
- Individual passenger weight is not mapped by name in the source record

### Claire Bennett

- Premium leisure international client
- First recorded mission: KEYW → MYAM
- Associated with Lost Bay Aviation's first international charter
- Individual passenger weight is not mapped by name in the source record

---

## Maintenance Notes

Update this summary only after updating `/operations/current/data/clients.yaml`.

This file should remain concise. Detailed client continuity belongs in `clients.yaml`.

Do not add unsupported preferences, VIP status, weights, or mission history here unless they are supported by the authoritative YAML data or historical mission/debrief records.
