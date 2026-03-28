# AGENTS.pipeline.md — Pipeline Capability Routing

## Purpose

This fragment is injected into AGENTS.md when the `pipeline` capability is active. It routes agents to the correct specialists for Kedro pipeline authoring and DeAcero data infrastructure work.

## Activate when

- Working in `pipelines/`, `nodes/`, `catalog/`, or Kedro project directories
- Designing or implementing Kedro nodes, pipelines, or catalog entries
- Querying or mapping DeAcero's legacy SQL data sources for pipeline use
- Visualizing pipeline DAGs or exploring node dependencies
- Registering parameters, datasets, or pipeline hooks

## Skills in this capability

| Skill | Trigger |
|---|---|
| `kedro-builder` | Designing or implementing Kedro nodes, pipelines, or catalog entries — invoke for any Kedro authoring task |
| `deacero-domain` | Any query or transformation involving DeAcero's data infrastructure (PE systems, SQL DBs, business rules) |
| `squit-data` | Discovering table schemas, mapping legacy SQL objects to Kedro datasets |
| `kedroviz-specialist` | Visualizing pipeline DAGs, exploring node dependencies, generating pipeline documentation |

## Kedro Authoring Workflow

```
deacero-domain (understand data sources and business rules)
  → squit-data (discover and map legacy SQL objects)
    → kedro-builder (design nodes, pipelines, catalog entries)
      → kedroviz-specialist (validate DAG structure)
```

## Kedro Node/Pipeline/Catalog Conventions

- Nodes must be pure functions: `(inputs: dict) → dict`. No side effects.
- Pipeline names must follow the pattern: `<domain>_<verb>_pipeline` (e.g., `ventas_ingest_pipeline`).
- Catalog entries must use typed datasets (`pandas.CSVDataset`, `spark.SparkDataset`).
- All parameters live in `conf/base/parameters/` keyed by pipeline name.

## DeAcero Domain Context

Always invoke `deacero-domain` before accessing any DeAcero-specific data source to ensure correct schema, business rules, and data lineage understanding.

## KedroViz Integration

Use `kedroviz-specialist` to generate pipeline DAG images and dependency reports. These must be committed to `docs/pipelines/` and referenced in the pipeline's README.
