# AGENTS.archaeology+server.md — Archaeology + Server Integration Routing

## Purpose

This fragment is injected automatically when both the `archaeology` and `server` capabilities are present. It governs the legacy API wrapper pattern — exposing legacy behavior through a clean hexagonal adapter.

## Activate automatically when

Both `capabilities/archaeology/` and `capabilities/server/` are present in the sparse checkout.

## Integration Philosophy

**Wrap before you rewrite.** Never implement a clean hexagonal adapter for a legacy endpoint without first characterizing the existing behavior. The adapter's contract is defined by what the legacy system actually does, not what the documentation says it should do.

## Legacy API Wrapper Workflow

```
1. archaeology: problem-intake
   → Clarify which legacy endpoints or SQL objects need to be wrapped
   → Identify consumers (other systems calling the legacy endpoint)

2. archaeology: software-archeologist + squit
   → Discover the full implementation of the target legacy objects
   → Map inputs, outputs, side effects, and error conditions

3. archaeology: characterization-tester
   → Write characterization tests that capture the legacy behavior
   → These become the regression suite for the new adapter

4. server: architect + adr-writer
   → Define the hexagonal port interface for the legacy capability
   → Write ADR documenting the wrapping decision and strangler fig strategy

5. server: database-expert (if needed)
   → Design the outbound adapter that calls the legacy stored procedure or DB

6. server: FastAPI router
   → Implement the inbound adapter exposing the capability via REST or MCP
```

## Strangler Fig Pattern

When wrapping a large legacy system:
- Start with the highest-traffic or highest-risk endpoints.
- Each wrapped endpoint gets its own port + adapter pair.
- Route traffic through the new adapter incrementally, validating against characterization tests.
- Remove the direct legacy dependency only after the new adapter has been in production for at least one release cycle.

## Invariants

- The hexagonal port interface MUST be defined before implementation begins.
- No legacy SQL object may be modified during the wrapping phase — characterize and wrap only.
- All wrapping decisions must be documented in an ADR.
