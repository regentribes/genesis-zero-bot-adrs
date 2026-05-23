---
title: "Next.js 16 App Router with Cloudflare Workers"
status: accepted
date: 2026-05-23
deciders: MyCoNet development team
severity: high
---

# ADR-0001: Next.js 16 App Router with Cloudflare Workers

## Status

Accepted

## Context

MyCoNet requires a hosting platform that supports Next.js App Router, handles edge deployment, and integrates with Supabase. The team initially evaluated Vercel but encountered 60-second timeout incompatibility with MiniMax M2.7 API calls.

## Decision

Deploy to Cloudflare Workers using `@opennextjs/cloudflare` (OpenNext adapter). Use Next.js 16 App Router with Turbopack.

**Deploy command:** `cd web && npm run deploy:cf` (runs: opennextjs-cloudflare build && wrangler deploy)

**Worker name:** `myconet`
**Live URL:** `https://myconet.correa-oscar11.workers.dev`

## Consequences

Positive:
- Edge deployment reduces latency for global community members
- 60s timeout issue resolved
- Single command deploy pipeline

Negative:
- Some Next.js features may behave differently at edge vs Node.js
- Cold start latency on Workers (mitigated by Cloudflare's global network)

Alternatives considered:
- **Vercel** — rejected: 60s timeout incompatible with MiniMax M2.7
- **Railway/Fly.io** — not evaluated for this iteration