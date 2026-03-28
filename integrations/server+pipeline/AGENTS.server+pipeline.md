# AGENTS.server+pipeline.md — Server + Pipeline Integration Routing

## Purpose

This fragment is injected automatically when both the `server` and `pipeline` capabilities are present in a project. It activates the Kedro MCP adapter integration layer.

## Activate automatically when

Both `capabilities/server/` and `capabilities/pipeline/` are present in the sparse checkout. This integration layer is not opt-in — it engages whenever both capabilities co-exist.

## Skills in this integration

| Skill | Trigger |
|---|---|
| `kedro-mcp-adapter` | Designing or implementing MCP tools that expose Kedro pipeline operations |

## Kedro MCP Adapter Design

The adapter bridges the `server` and `pipeline` capability boundaries by exposing Kedro pipeline operations as MCP tools consumable by LLM agents:

```
MCP Client (Claude Code / Cursor)
  → FastMCP Server (server capability)
    → kedro-mcp-adapter (this integration)
      → Kedro Session / DataCatalog (pipeline capability)
```

## MCP Tool Surface (planned — see ADR-0030)

| Tool | Description |
|---|---|
| `list_pipelines` | Returns all registered pipeline names and descriptions |
| `describe_pipeline` | Returns nodes, inputs, outputs for a named pipeline |
| `run_pipeline` | Executes a named pipeline with given parameters |
| `get_catalog_entry` | Returns metadata and schema for a catalog dataset |
| `get_node_outputs` | Returns the latest outputs of a completed node run |
| `visualize_dag` | Returns a Mermaid DAG representation of a pipeline |

## Routing Rules

- When a user asks to "run a pipeline", "list pipelines", or "describe a node", invoke `kedro-mcp-adapter`.
- When a user asks about data sources or schemas, combine `kedro-mcp-adapter` with `squit-data` (from `pipeline` capability).
- ADR-0030 governs the full design of this adapter — read it before making implementation decisions.
