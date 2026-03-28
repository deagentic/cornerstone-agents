---
name: kedro-mcp-adapter
description: Use when designing or implementing MCP tools that expose Kedro pipeline operations — listing pipelines, describing nodes, running nodes, querying catalog entries, or generating DAG representations. This is the integration bridge between the server and pipeline capabilities.
version: "0.1.0"
status: "stub — detailed design in ADR-0030"
---

# Kedro MCP Adapter

## Identity

This skill governs the design and implementation of the Kedro MCP adapter — the integration bridge that exposes Kedro pipeline operations as MCP tools consumable by LLM agents (Claude Code, Claude Desktop, Cursor, etc.).

## Scope

- **Pipeline introspection**: MCP tools for listing, describing, and querying Kedro pipelines and their nodes.
- **Pipeline execution**: MCP tools for running pipelines and individual nodes with parameter overrides.
- **Catalog access**: MCP tools for querying dataset metadata, schemas, and latest outputs.
- **DAG representation**: MCP tools for returning Mermaid or JSON representations of pipeline graphs.
- **Session management**: Managing Kedro sessions within the FastMCP server context (initialization, teardown, caching).

## Planned MCP Tool Surface (see ADR-0030)

| Tool | Description |
|---|---|
| `list_pipelines` | Returns all registered pipeline names and descriptions |
| `describe_pipeline` | Returns nodes, inputs, outputs for a named pipeline |
| `run_pipeline` | Executes a named pipeline with given parameters |
| `get_catalog_entry` | Returns metadata and schema for a catalog dataset |
| `get_node_outputs` | Returns the latest outputs of a completed node run |
| `visualize_dag` | Returns a Mermaid DAG representation of a pipeline |

## Architecture

```
MCP Client
  → FastMCP Server (adapters/inbound/mcp/)
    → KedroMCPAdapter (this integration)
      → Kedro Session (KedroContext / DataCatalog / Pipeline)
        → Data sources (catalogs, parameters, runners)
```

The adapter must follow hexagonal architecture: the FastMCP tool functions are the inbound port; the Kedro session interaction is the outbound adapter.

## Status

This skill is a stub. Full implementation is gated on ADR-0030, which will define the Kedro session lifecycle, error handling, parameter schema, and security model for pipeline execution via MCP.

## Related Skills

- `kedro-builder` (from `pipeline`) — builds the pipelines this adapter exposes
- `kedroviz-specialist` (from `pipeline`) — provides DAG representations
- FastAPI router patterns (from `server` AGENTS fragment) — governs the inbound adapter structure
