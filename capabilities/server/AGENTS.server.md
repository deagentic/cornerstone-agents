# AGENTS.server.md — Server Capability Routing

## Purpose

This fragment is injected into AGENTS.md when the `server` capability is active. It governs backend service development following hexagonal architecture principles.

## Activate when

- Working in `src/`, `services/`, `adapters/`, `ports/`, or `domain/` directories
- Designing or implementing FastMCP tool endpoints
- Adding FastAPI routers, middleware, or request handlers
- Writing database migrations via Alembic
- Defining domain models, use cases, or application services

## Skills in this capability

| Skill | Trigger |
|---|---|
| `database-expert` | Any work involving schema design, migrations, query optimization, or ORM configuration |

## Hexagonal Architecture Mandate

All server code MUST follow the hexagonal (ports & adapters) pattern:

```
domain/          ← pure business logic, no framework imports
ports/           ← interface definitions (abstract classes / protocols)
adapters/
  inbound/       ← FastAPI routers, MCP tools, CLI entrypoints
  outbound/      ← DB adapters, HTTP clients, external service wrappers
```

Never import FastAPI, SQLAlchemy, or any infrastructure library inside `domain/`.

## FastMCP Tool Design Rules

- Each MCP tool must have a single, clearly scoped responsibility.
- Tool names must be snake_case verbs: `get_pipeline_status`, `run_node`, `list_catalogs`.
- All tools must validate inputs with Pydantic models before passing to the domain layer.
- Tools must return structured JSON-serializable responses.

## FastAPI Router Patterns

- Group routes by domain entity, not by HTTP method.
- Place routers in `adapters/inbound/routers/`.
- Use dependency injection for all infrastructure concerns (DB sessions, auth, config).

## ADR-First Mandate

No new dependency may be added to `src/` without a corresponding ADR in `docs/adr/`. Use `adr-writer` (from `common/`) before modifying `pyproject.toml` or `requirements*.txt`.
