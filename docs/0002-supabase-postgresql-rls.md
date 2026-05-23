---
title: "Supabase as Primary Database and Auth Provider"
status: accepted
date: 2026-05-23
deciders: MyCoNet development team
severity: high
---

# ADR-0002: Supabase as Primary Database and Auth Provider

## Status

Accepted

## Context

MyCoNet requires PostgreSQL for structured relational data across 14 modules, Row Level Security for per-role access, and auth with magic link support. A real-time layer is needed for future agent coordination (Module 12).

## Decision

Use Supabase as the sole database and auth provider.

**Auth:** Supabase Auth with cookie-based sessions (`@supabase/ssr`). Server components read session via `createClient()` from `@/core/lib/supabase/server`.

**Database:** Supabase PostgreSQL. All 14 modules share a single database.

**Access control:** Supabase Row Level Security on every table. Helper function `is_admin()` checks `user_profiles.role` without exposing the service role key.

**Realtime:** Supabase Realtime for future module event subscriptions (Module 12 MycoNet Agent).

## Consequences

Positive:
- Single auth + database provider reduces integration complexity
- RLS enforces data access at the database layer
- Realtime available for event-driven agent coordination
- Magic link auth lowers onboarding friction

Negative:
- Vendor lock-in to Supabase
- Community data tied to Supabase project lifecycle

## References

- ARCHITECTURE.md Section 3 (System Architecture)
- CLAUDE.md Section (Auth)