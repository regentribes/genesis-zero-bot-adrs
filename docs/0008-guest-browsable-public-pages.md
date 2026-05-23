---
title: "Guest-Browsable Public Pages"
status: accepted
date: 2026-05-23
deciders: MyCoNet development team
severity: medium
---

# ADR-0008: Guest-Browsable Public Pages

## Status

Accepted

## Context

MyCoNet is a community portal that should demonstrate value to prospective members before they sign up. All main pages need a public view accessible without authentication.

## Decision

All main pages (Dashboard, Network, Blueprint, Join, Agreements, Ops) are publicly browsable without an account. Pages check for a session and render a guest-appropriate view:

- No proposal buttons
- No admin panels
- Join/sign-in CTAs instead of gated actions

Guests see member profiles (M01), the Blueprint (M04), and open projects (M06) to understand the community before applying.

Exceptions: `/profile/edit` and `/contributions` require `member` role.

## Consequences

Positive:
- Prospective members can evaluate the community before committing
- Reduces signup friction
- Demonstrates transparency of community processes

Negative:
- Public data exposure must be carefully managed
- Guest views require separate UX from member views

## References

- ARCHITECTURE.md Section 3 (Guest access)
- CLAUDE.md (What NOT to do — no hard-coded role checks)