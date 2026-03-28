# AGENTS.archaeology+pipeline.md — Archaeology + Pipeline Integration Routing

## Purpose

This fragment is injected automatically when both the `archaeology` and `pipeline` capabilities are present. It governs the legacy-to-Kedro migration workflow.

## Activate automatically when

Both `capabilities/archaeology/` and `capabilities/pipeline/` are present in the sparse checkout.

## Integration Philosophy

**Characterize before you migrate.** Never map a legacy SQL transformation to a Kedro node without first understanding what that transformation actually does — including its edge cases, implicit dependencies, and undocumented business rules.

## Migration Workflow

```
1. archaeology: software-archeologist + squit
   → Discover all SQL objects in the target domain
   → Identify transformation logic and data flow

2. archaeology: characterization-tester
   → Write SQL-based characterization tests that capture current behavior
   → These become the acceptance tests for the migrated Kedro node

3. pipeline: deacero-domain + squit-data
   → Map legacy tables/views to Kedro catalog datasets
   → Identify source/sink boundaries

4. pipeline: kedro-builder
   → Implement Kedro nodes that replicate the characterized transformations
   → Wire into pipeline with correct dependency ordering

5. pipeline: kedroviz-specialist
   → Validate DAG matches the dependency graph discovered during archaeology
```

## Invariants

- A Kedro node MUST NOT be created for a transformation that has not been characterized.
- All characterization test outputs must be committed to `tests/characterization/` before the migration PR is merged.
- If the legacy transformation has non-deterministic outputs, document them in an ADR before proceeding.
