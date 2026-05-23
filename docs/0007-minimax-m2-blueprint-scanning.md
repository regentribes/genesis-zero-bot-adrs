---
title: "MiniMax M2.7 for Blueprint AI Document Scanning"
status: accepted
date: 2026-05-23
deciders: MyCoNet development team
severity: high
---

# ADR-0007: MiniMax M2.7 for Blueprint AI Document Scanning

## Status

Accepted

## Context

M04 Blueprint uses AI to extract structured data from uploaded community planning documents. The AI model must handle long documents (up to 5000 chars per chunk) with parallel processing.

## Decision

Use MiniMax M2.7 (reasoning model) via `/api/scan` route.

**Critical configuration (do not change):**
- `CHUNK_SIZE = 5000` chars — do not increase
- `CONCURRENCY = 4` parallel chunk calls — do not increase
- `max_completion_tokens: 8192` — do not raise (524 timeout)
- `temperature: 0.3`
- No system message — adding one makes M2.7 think harder, burning token budget
- `extractJSON()` function strips `<think>...` blocks before parsing

**No `export const runtime = 'edge'`** on any route — breaks OpenNext bundler.

## Consequences

Positive:
- Handles long documents via chunked parallel processing
- Reasoning model provides contextual extraction
- Token budget is managed by avoiding system prompts

Negative:
- MiniMax is a third-party API — rate limits and availability risk
- 8192 token ceiling can truncate very long documents
- No fallback model if MiniMax is unavailable