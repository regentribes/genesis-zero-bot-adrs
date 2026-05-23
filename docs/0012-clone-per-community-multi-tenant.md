---
title: "Clone-per-Community Multi-Tenant Model"
status: accepted
date: 2026-05-23
deciders: MyCoNet development team
severity: high
---

# ADR-0012: Clone-per-Community Multi-Tenant Model

## Status

Accepted

## Context

MyCoNet v2 deploys as a single-community portal. Phase 1 requires supporting multiple communities on the platform, each isolated from each other.

## Decision

The codebase is designed to be cloned. Each community gets:
- Their own Supabase project (separate PostgreSQL database)
- Their own Cloudflare Workers deployment
- Their own isolated data

Community-specific identity (name, colors, Blueprint content) lives in the database — no hardcoded community names in the codebase.

**Phase 2 (Hive):** Communities opt in to listing themselves in a shared public Neighborhood Directory (M02 v2), creating network-level visibility across all portals.

No multi-tenancy via `community_id` scoping in v2. Each community is a separate deployment.

## Consequences

Positive:
- Clean isolation — no cross-community data leakage risk
- Each community controls their own Supabase project and Workers deployment
- Simple operational model

Negative:
- No shared infrastructure across communities
- Upgrades to base codebase must be applied manually per community
- Hive (M13) requires cross-portal discovery layer

## References

- ARCHITECTURE.md Section 11 (Multi-Community Architecture)
- EXECUTIVE-SUMMARY.md (Phase 1/2/3/4 roadmap)