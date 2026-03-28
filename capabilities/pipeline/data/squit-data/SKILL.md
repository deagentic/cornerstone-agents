---
name: squit-data
description: Use when querying DeAcero's data catalog, discovering table schemas, or mapping legacy SQL objects to Kedro datasets. Invoke before building any Kedro pipeline node that touches DeAcero data sources.
version: "0.1.0"
status: "stub — SQUIT integration tuned for pipeline data discovery"
---

# SQUIT Data Discovery

## Identity

This skill specializes the SQUIT semantic search capability for pipeline data discovery use cases. While the base `squit` skill (in the `archaeology` capability) is oriented toward code exploration and impact analysis, `squit-data` focuses on discovering data shapes, table schemas, and object-to-dataset mappings needed for Kedro catalog registration.

## Scope

- **Schema discovery**: Finding the column structure, data types, and constraints of tables and views.
- **Dataset mapping**: Mapping legacy SQL objects (tables, views, result sets of procedures) to typed Kedro datasets.
- **Source identification**: Finding which SQL servers and databases contain data relevant to a pipeline.
- **Lineage tracing**: Using `squit_dependencies` to trace how data moves through the legacy SQL layer.
- **Volume estimation**: Understanding approximate data volumes for partitioning and resource planning decisions.

## Workflow

```
1. Invoke squit_search with data-focused queries:
   - "tabla clientes columnas" → finds ClientesMaster schema
   - "view ventas diarias estructura" → finds vw_VentasDiarias definition

2. Invoke squit_get_code to retrieve full DDL/CREATE statements for schema extraction.

3. Invoke squit_dependencies to identify upstream data producers (INSERT/UPDATE sources).

4. Map discovered schemas to Kedro dataset types in catalog.yml.
```

## Status

This skill is a stub. Full implementation will integrate SQUIT's vector search with a schema extraction pipeline that automatically generates Kedro catalog entries from discovered SQL objects. Implementation is gated on ADR-0030.

## Related Skills

- `squit` (from `archaeology`) — base SQUIT MCP server access
- `deacero-domain` — business context for discovered schemas
- `kedro-builder` — consumes the catalog entries produced by this skill
