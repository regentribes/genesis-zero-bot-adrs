---
title: "Two-Layer Access Control — Page and Database"
status: accepted
date: 2026-05-23
deciders: MyCoNet development team
severity: high
---

# ADR-0003: Two-Layer Access Control — Page and Database

## Status

Accepted

## Context

MyCoNet serves guests, members at different role levels, and admins. Access must be enforced both at the UI layer (so guests see appropriate public content) and at the database layer (so direct API access cannot bypass UI checks).

## Decision

Access is enforced at two layers:

**1. Page level (server components):** Every page calls `supabase.auth.getUser()`. Guests (no session) see public content; members see role-scoped panels. Redirects to `/auth/login` only for pages with no public view.

**2. Database level (RLS):** Row-level security policies enforce the same rules at the data layer.

```sql
CREATE OR REPLACE FUNCTION is_admin()
RETURNS boolean LANGUAGE sql SECURITY DEFINER STABLE AS $$
  SELECT EXISTS (
    SELECT 1 FROM user_profiles
    WHERE id = auth.uid()
    AND role IN ('admin', 'circle_lead', 'project_lead')
  );
$$;
```

Never hard-code role strings (`role === 'admin'`). Use helpers from `@/core/lib/roles`.

## Consequences

Positive:
- Defense in depth: UI layer for UX, RLS for security
- Public content is accessible without auth for guest browsing
- Role checks in one place (`@/core/lib/roles.ts`) — not scattered across components

Negative:
- Two places to update when role system changes
- RLS policies must be kept in sync with page-level checks