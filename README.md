# cornerstone-agents

The shared brain for all Cornerstone projects. This repository is the central library of agent skills, capability definitions, and integration routing fragments used by every project bootstrapped with the [Cornerstone agentic CI framework](https://github.com/deagentic/cookiecutter-agentic-ci).

Governed by **ADR-0029** (composable capability taxonomy).

---

## Three-Layer Structure

```
cornerstone-agents/
├── common/          ← ships to EVERY project, always
├── capabilities/    ← opt-in by project type
└── integrations/    ← activated when two capabilities coexist
```

### Layer 1: `common/`

Universal skills that every Cornerstone project receives regardless of its domain. These include core operating protocols (learning-protocol, telemetry), cross-cutting tools (tool-writer), and the software quality baseline (adr-writer, architect, bdd-writer, code-reviewer, characterization-tester, technical-writer).

### Layer 2: `capabilities/`

Domain-specific skill bundles. Each capability is independently opt-in and ships only to projects that need it. Current capabilities:

| Capability | Description |
|---|---|
| `cli` | CLI tooling — semantic versioning, release management |
| `archaeology` | Legacy system discovery — SQUIT, COBOL analysis, retro-engineering |
| `server` | Backend services — hexagonal architecture, FastMCP, FastAPI, Alembic |
| `frontend` | UI development — Stitch design loop, shadcn-ui, React, Remotion |
| `hardware` | Hardware interfaces — serial, USB-HID, Bluetooth, NFC, embedded |
| `pipeline` | Data pipelines — Kedro node/pipeline/catalog authoring, KedroViz |

### Layer 3: `integrations/`

Integration routing fragments that activate automatically when two capabilities coexist in a project. They define the inter-capability contract and workflow.

| Integration | Activates when |
|---|---|
| `server+pipeline` | Both server and pipeline capabilities are present |
| `server+frontend` | Both server and frontend capabilities are present |
| `archaeology+pipeline` | Both archaeology and pipeline capabilities are present |
| `archaeology+server` | Both archaeology and server capabilities are present |

---

## How Projects Pull from This Repo

Projects consume `cornerstone-agents` via **sparse checkout**, selecting only the layers they need. The `cornerstone sync` CLI command manages this:

```bash
# In a Cornerstone project root:
cornerstone sync --capabilities archaeology,server
```

This command performs a Git sparse checkout that includes:
- `common/` (always)
- `capabilities/archaeology/`
- `capabilities/server/`
- `integrations/archaeology+server/` (auto-detected because both are present)

The result is placed under `.agents/skills/` in the target project, making all skills available to Claude Code, Cursor, and other agent runtimes.

### Sparse Paths per Capability

Each capability's `capability.json` defines its `sparse_paths`:

```json
{
  "name": "server",
  "sparse_paths": ["common/", "capabilities/server/"]
}
```

When an integration is also active, its `integration.json` adds additional paths:

```json
{
  "name": "archaeology+server",
  "sparse_paths": ["integrations/archaeology+server/"]
}
```

---

## How Learnings Push Back

When an agent running in a project discovers a new pattern, improves a skill, or creates a new sub-agent, the `learning-protocol` skill mandates persisting that knowledge. Generic learnings — patterns useful across any project — are pushed back to this repository:

```
Project session
  → learning-protocol invoked
    → Generic finding identified
      → docs/upstream_contributions/ documented
        → PR opened against cornerstone-agents
          → Merged → available to all future projects via next sync
```

This creates a **continuous growth loop**: every archaeology session, every pipeline build, every hardware integration makes the shared brain more capable.

---

## The Continuous Growth Loop

```
                    ┌─────────────────────────────┐
                    │   cornerstone-agents (this)  │
                    │   Central skill library      │
                    └──────────────┬──────────────┘
                                   │ cornerstone sync
                    ┌──────────────▼──────────────┐
                    │   Project .agents/skills/    │
                    │   Sparse checkout of skills  │
                    └──────────────┬──────────────┘
                                   │ Agent sessions run
                    ┌──────────────▼──────────────┐
                    │   learning-protocol          │
                    │   Captures new patterns      │
                    └──────────────┬──────────────┘
                                   │ Generic learnings
                    ┌──────────────▼──────────────┐
                    │   PR to cornerstone-agents   │
                    │   Skills improve globally    │
                    └─────────────────────────────┘
```

Every project benefits from every other project's learnings. The brain grows automatically.

---

## Governance

- All structural changes to this repository (new capabilities, new integrations, taxonomy changes) require an ADR in the source Cornerstone CLI repo (`docs/adr/`).
- ADR-0029 establishes the composable capability taxonomy implemented here.
- ADR-0030 (planned) will govern the Kedro MCP adapter design (`integrations/server+pipeline/`).
- The `common/software/architecture/adr-writer` skill must be used for all ADR authoring.

---

## Repository

This repository is maintained by the DeAcero AI Engineering team under the `deagentic` GitHub organization.
