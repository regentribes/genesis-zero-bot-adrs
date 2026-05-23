---
title: "Data-Centric Architecture for MyCoNet Knowledge Layer"
status: proposed
date: 2026-05-23
deciders: RegenTribes tech circle
severity: high
---

# ADR-0010: Data-Centric Architecture for MyCoNet Knowledge Layer

## Context

MyCoNet (Greenhollow Commons platform) is currently app-centric: Cloudflare Workers + Supabase as the primary architecture. Community data outlives any specific platform, but the current architecture does not have a semantic layer that survives platform migrations.

The Data-Centric Architecture Forum (DCAF) research reveals patterns applicable to RegenTribes knowledge infrastructure:

- **Dave McComb thesis:** "Data is the enduring asset; applications are ephemeral"
- **KGraphRAG:** Graph-first (not document-first) RAG for community knowledge bases
- **Entity Resolution:** Real-time ER for member/resource graphs (scales 38 to 380+)
- **Medallion Architecture:** Bronze/silver/gold data quality layers mapping to Greenhollow 5-pillar model
- **MCP:** Model Context Protocol for AI agents consuming governed semantic data

## Decision

We adopt data-centric architecture principles for MyCoNet knowledge layer:

1. Extract the ontology as the long-term asset; Workers are the ephemeral app layer
2. Use KGraphRAG pattern (graph-first, then vector search) for the knowledge base
3. Implement entity resolution for member graph deduplication
4. Map medallion architecture (bronze/silver/gold) to Greenhollow 5-pillar maturity levels
5. Evaluate MCP for Genesis AI connection to community knowledge graph

## Consequences

**Positive:**
- Ontology survives platform migrations (Workers can be replaced, the graph endures)
- KGraphRAG pattern aligned with Genesis Brain / SurrealDB query architecture
- Entity resolution enables clean member graph scaling
- Medallion maturity model maps to existing 5-pillar scoring (Ecology 4.2, Economy 2.3, etc.)

**Negative:**
- Additional complexity: requires maintaining an ontology layer separate from app code
- Migration effort: existing Supabase data needs ontology mapping
- Skills gap: team needs to learn RDF/SHACL/ontology design

**Alternatives considered:**
- Pure vector RAG (document-first) — rejected because community knowledge is structured, not just text
- App-centric with Supabase as primary store — rejected per McComb's thesis; data does not outlive app
- LangGraph graph-first — rejected (OUT of Adopt on Thoughtworks Radar, lean agents preferred)

## References

- DCAF 2025/2026 themes: Scale, Speed, Strings; validity/grounding AI
- Dave McComb, The Data-Centric Revolution (2019)
- Schema App MCP Server pattern
- Senzing ERKG (Entity Resolved Knowledge Graphs)
- https://www.dcaforum.com/