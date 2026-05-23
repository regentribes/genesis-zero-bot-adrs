---
title: "Collaboration Agreements with Proposal-to-Deliverable Flow"
status: accepted
date: 2026-05-23
deciders: MyCoNet development team
severity: medium
---

# ADR-0009: Collaboration Agreements with Proposal-to-Deliverable Flow

## Status

Accepted

## Context

M06 Agreements enables `joining+` members to propose collaboration on open projects. Accepted proposals should automatically create tracked deliverables.

## Decision

A collaboration proposal includes:
- `work_description` — what the member will do
- `expected_reward` — what they expect in return
- `conditions` — timing, availability, dependencies (optional)

One proposal per (project, user) combination: `UNIQUE(project_id, user_id)`. Upsert with `onConflict: 'project_id,user_id'` prevents duplicate-key errors.

Admin reviews and accepts proposals. On acceptance:
1. `collaboration_agreements.status` set to `'accepted'`
2. New `deliverables` row inserted with `from_agreement_id` linking back
3. Proposal appears in Ideas column of the per-project Kanban

## Consequences

Positive:
- Proposal-to-deliverable flow is automatic and traceable
- `from_agreement_id` links work back to the original proposal
- Prevents duplicate proposals per project

Negative:
- Agreement lifecycle (pending -> accepted -> active -> completed) requires careful state management
- No payment integration in v3.41 (Stripe planned)