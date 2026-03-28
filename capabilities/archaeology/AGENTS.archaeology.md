# AGENTS.archaeology.md — Archaeology Capability Routing

## Purpose

This fragment is injected into AGENTS.md when the `archaeology` capability is active. It routes agents to the correct specialists for legacy system discovery and reverse-engineering tasks.

## Activate when

- Touching legacy code (stored procedures, views, triggers, COBOL programs)
- Performing reverse-engineering or retro-engineering tasks
- Characterizing undocumented behavior before modification
- Exploring unknown domains or business systems with no documentation
- Running SQUIT semantic searches against the legacy SQL corpus

## Skills in this capability

| Skill | Trigger |
|---|---|
| `squit` | Semantic search over DeAcero's 5.7M SQL objects — invoke before any legacy SQL exploration |
| `cobol-analyst` | Analyzing or modernizing COBOL programs and copybooks |
| `problem-intake` | Structured intake of user problem statements before starting archaeology |
| `retro-engineer` | Bottom-up reverse-engineering of undocumented systems |
| `software-archeologist` | Primary orchestrator for full archaeology sessions |
| `unknown-domain-protocol` | Entry protocol when encountering a domain with no prior context |

## SQUIT Query Workflow

1. Invoke `problem-intake` to clarify the user's intent and identify relevant business domains.
2. Invoke `squit` with the discovered domain terms to find relevant SQL objects.
3. Use `squit_dependencies` and `squit_impact` before proposing any modifications.
4. Invoke `characterization-tester` (from `common/`) to capture current behavior before changes.
5. Document findings via `learning-protocol` (from `common/`).

## Bottom-Up Protocol

When no domain context exists, the sequence is:

```
unknown-domain-protocol → problem-intake → software-archeologist → retro-engineer → cobol-analyst (if COBOL) → characterization-tester
```

## ADR mandate

Any structural finding that reveals an architectural boundary, data contract, or system coupling that was not previously documented MUST produce an ADR via `adr-writer` (from `common/`).
