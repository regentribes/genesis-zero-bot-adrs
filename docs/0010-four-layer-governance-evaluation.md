---
title: "Four-Layer Governance Evaluation"
status: accepted
date: 2026-05-23
deciders: MyCoNet development team
severity: medium
---

# ADR-0010: Four-Layer Governance Evaluation

## Status

Accepted

## Context

M09 Governance must support multiple decision-making philosophies used in regenerative communities.

## Decision

Proposals evaluate across four layers:

1. **Consent** — No objections registered. Any `object` vote blocks.
2. **Democracy** — Majority of consent votes wins.
3. **Meritocracy** — Weighted by role or demonstrated expertise.
4. **AI Facilitation** — Algorithm-assisted evaluation.

A proposal becomes **LIVE** when at least 3 of 4 layers pass.

**Decision modes:** `consent` | `democracy` | `meritocracy` | `ai`

**Vote options:** consent | concern | object (for consent mode)

**Proposal lifecycle:** open -> closed -> decided

`evalLayers(proposal)` derives each layer's pass/fail from actual `proposal_votes`.

## Consequences

Positive:
- Flexible enough to support different community governance styles
- Quantifiable threshold (3/4 layers) for clear resolution
- Threaded discussion per proposal

Negative:
- Complexity: members may not understand all four layers
- AI layer is planned but not yet implemented