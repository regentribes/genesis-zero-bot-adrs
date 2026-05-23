---
title: "Three-Agent Coordination Model — Genesis, Quinn, MycoNet"
status: proposed
date: 2026-05-23
deciders: MyCoNet development team
severity: high
---

# ADR-0013: Three-Agent Coordination Model — Genesis, Quinn, MycoNet

## Status

Proposed

## Context

The MyCoNet platform plan includes three AI agents that coordinate community operations: a Telegram bridge (Genesis), a personal AI per member (Quinn), and a community brain (MycoNet). These are Phase 4 modules (M10, M11, M12).

## Decision

Three agents with distinct scopes:

**Genesis (M10) — Telegram Bridge:**
- Present in Community Group and Leadership Group
- Receives member requests: `@Genesis record that we decided X`
- Posts approval requests to Leadership Group
- Confirms outcomes in Community Group
- All requests require human approval before database write

**Quinn (M11) — Personal AI:**
- One per community member
- Daily reminders, goal tracking, routing
- Pushes via in-app + Telegram + WhatsApp
- Maintains per-member memory in PostgreSQL

**MycoNet (M12) — Community Brain:**
- Subscribes to all module events (via Supabase Realtime)
- Maintains community memory (JSONB + pgvector)
- **MycoNet Pro:** pushes proactive updates to Leadership Group on every module write
- Holds Genesis approval queue
- 30-minute heartbeat for pending decisions

**MycoNet Dashboard (M00):** Real-time command center for CM and Department Leads.

## Event System

Every module write (01-09, 11, 12, 13) fires an event via Supabase Realtime.

| Event | Triggered By | Who Receives |
|---|---|---|
| `genesis.request.pending` | Genesis | MycoNet Dashboard |
| `genesis.request.approved` | Leader | Genesis -> Community Group |
| `task.completed` | Module 07 | MycoNet Pro -> Leadership, Quinn -> assignee |

## Consequences

Positive:
- Clear separation of concerns between agents
- Human approval ensures no autonomous database writes
- Event-driven architecture enables real-time coordination

Negative:
- Complex multi-agent system — significant implementation work
- Three bots in two Telegram groups is a high cognitive load
- pgvector dependency for MycoNet memory

## References

- AiNSP_RnDev.md (full agent documentation)
- techstack_AiNSP_RnDev.md (event schema)