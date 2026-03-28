---
name: kedro-builder
description: Use when designing or implementing Kedro nodes, pipelines, or catalog entries. Always invoke for any Kedro authoring task — node creation, pipeline wiring, catalog registration, parameter configuration.
version: "0.1.0"
status: "stub — implement after ADR-0030"
---

# Kedro Builder

## Identity

This skill covers Kedro node, pipeline, and catalog authoring patterns with DeAcero-specific conventions. It is the primary agent for all Kedro development tasks within the `pipeline` capability.

## Scope

- **Node authoring**: Pure Python functions following the `(inputs: dict) → dict` contract. No side effects. No framework imports inside node functions.
- **Pipeline wiring**: Connecting nodes via `pipeline()` and `Pipeline()` constructs. Ensuring correct dependency ordering by matching output dataset names to input dataset names.
- **Catalog registration**: Registering datasets in `conf/base/catalog.yml` with correct typed dataset classes (`pandas.CSVDataset`, `spark.SparkDataset`, `polars.CSVDataset`, etc.).
- **Parameter configuration**: Defining parameters in `conf/base/parameters/<pipeline_name>.yml` and referencing them correctly in pipeline definitions.

## DeAcero-Specific Conventions

- Pipeline names follow `<domain>_<verb>_pipeline` (e.g., `ventas_ingest_pipeline`, `inventario_transform_pipeline`).
- All DeAcero data sources must be discovered via `squit-data` before catalog registration.
- Business rule documentation from `deacero-domain` must be referenced in node docstrings.

## Status

This skill is a stub. Full implementation will follow ADR-0030, which defines the Kedro integration architecture for DeAcero's data platform. Until ADR-0030 is finalized, refer to the official [Kedro documentation](https://docs.kedro.org) and apply DeAcero conventions as documented in `deacero-domain`.

## Related Skills

- `deacero-domain` — always invoke before accessing DeAcero data sources
- `squit-data` — for legacy SQL object discovery and mapping
- `kedroviz-specialist` — for DAG validation after pipeline wiring
