---
title: "Five-Pillar Circle System for Project Organisation"
status: accepted
date: 2026-05-23
deciders: MyCoNet development team
severity: medium
---

# ADR-0004: Five-Pillar Circle System for Project Organisation

## Status

Accepted

## Context

MyCoNet organises community work around five domains that map to the five circles of the Greenhollow Commons governance model.

## Decision

Projects in M07 carry a `circle` field. Members can be assigned `lead_circles`.

| Circle | Description |
|---|---|
| `ecology` | Natural systems, food production, water |
| `hardware` | Physical infrastructure, tools, buildings |
| `humanware` | Education, health, social fabric |
| `economy` | Resource flows, exchange, contributions |
| `tech` | Digital systems, connectivity, data |

Source of truth: `@/core/lib/pillars.ts` (`PILLARS`, `PILLAR_META`, `isPillar()`).

`user_profiles.lead_circles` is a text array. Set via `/admin/users` page for `circle_lead` role.

Circle drives both visual badge on project cards and circle-lead permission check on Kanban drag operations.

## Consequences

Positive:
- Aligns with Greenhollow Commons five-pillar governance model
- Enables scoped permissions (circle_lead can manage their circles' projects)
- Visual badge on Kanban provides quick context

Negative:
- Circle list is hardcoded — adding a new circle requires a migration
- Circle-to-module mapping is implicit (no enforcement that ecology projects belong to ecology circle)