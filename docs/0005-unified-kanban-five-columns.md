---
title: "Unified Five-Column Kanban for Projects and Deliverables"
status: accepted
date: 2026-05-23
deciders: MyCoNet development team
severity: medium
---

# ADR-0005: Unified Five-Column Kanban for Projects and Deliverables

## Status

Accepted

## Context

M07 Operations previously had two separate views: a project list and a deliverables board. The v3.41 restructure unified these into a single mental model: five columns shared between the main board (cards = projects) and the per-project board (cards = deliverables).

## Decision

Five columns everywhere, defined once in `@/core/lib/project-status`:

`Ideas -> Backlog -> In Progress -> Review -> Done`

`paused` exists in schema as an off-board state but does not appear on the board.

**Main `/ops` board:** cards = projects. Drag-and-drop changes `projects.status` with permission gating (admin / project_lead / circle_lead for their circles).

**Per-project `/ops/[id]` board:** cards = deliverables. Ideas column shows pending collaboration proposals (admin / project creator only). Drag-and-drop works across both card types.

**Proposal-to-deliverable flow:** Dropping a proposal from Ideas to any other column (or clicking Accept) calls `acceptProposal()`: flips agreement to `'accepted'` and inserts a new `deliverables` row with `from_agreement_id` linking back.

## Consequences

Positive:
- Single source of truth for column definitions
- Consistent UX across both surfaces
- Proposal-to-deliverable flow is drag-and-drop native

Negative:
- Requires understanding both projects and deliverables mental model
- `paused` state exists in schema but not on board — potential confusion

## References

- ARCHITECTURE.md Section 2 (Module Status M07)
- log.md 2026-05-23 (v3.41 change log)