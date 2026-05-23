---
title: "Achievement Catalog with Auto-Award System"
status: accepted
date: 2026-05-23
deciders: MyCoNet development team
severity: low
---

# ADR-0011: Achievement Catalog with Auto-Award System

## Status

Accepted

## Context

M08 Contribution Tracking makes regenerative action visible and rewarding. Achievements should be awarded automatically when conditions are met, not manually assigned.

## Decision

Achievement catalog with auto-award triggers:

| Key | Name | Earn condition |
|---|---|---|
| `profile_pioneer` | Profile Pioneer | Profile wizard reaches 100% |
| `community_joiner` | Community Joiner | Complete M05 Join and get accepted |

**Profile Pioneer:** `useEffect` in `/profile/edit` watches form state; on pct === 100 calls upsert with `onConflict: 'user_id,achievement_key', ignoreDuplicates: true`. Idempotent, fires at most once.

**Community Joiner:** auto-upserted for `joining+` roles on home page load.

11-check profile completion: avatar, first_name, headline, city, user_types, values_principles, skills, goals, 1+ active offer, 1+ active request, 1+ travel plan.

Schema: `user_achievements(id, user_id, achievement_key, achievement_name, earned_at)` with `UNIQUE(user_id, achievement_key)`. RLS: users read own rows, insert own rows.

## Consequences

Positive:
- Auto-award reduces admin overhead
- Idempotent upsert prevents double-awarding
- Profile completion drives engagement

Negative:
- Achievements are binary — no partial credit
- No way to revoke achievements if circumstances change