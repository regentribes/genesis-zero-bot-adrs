---
title: "Module-per-Feature Code Structure"
status: accepted
date: 2026-05-23
deciders: MyCoNet development team
severity: medium
---

# ADR-0006: Module-per-Feature Code Structure

## Status

Accepted

## Context

The codebase serves 14 modules. Contributors need to work on one module without understanding the entire platform.

## Decision

Three-layer code organisation in `web/src/`:

- `app/` — Next.js route files only (thin re-exports). Do not add logic here.
- `core/` — Shared infrastructure: Supabase clients, UI primitives, shell layout, types.
- `modules/mXX-name/` — One folder per feature module. Each has its own README.

Module folders are self-contained. Contributors work in their module folder. Shared code goes in `core/`.

No imports from legacy paths: `@/lib/`, `@/components/`, `@/contexts/`, `@/types/`.

## Consequences

Positive:
- Contributors can focus on one module
- Clear boundary between route files and business logic
- Shared code is explicit and importable

Negative:
- Some shared concerns cut across modules (e.g., module-meta.ts)
- Module boundaries may blur as features evolve

## References

- web/CONTRIBUTING.md
- web/src/modules/README.md