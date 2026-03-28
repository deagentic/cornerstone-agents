# AGENTS.frontend.md — Frontend Capability Routing

## Purpose

This fragment is injected into AGENTS.md when the `frontend` capability is active. It routes agents to the correct specialists for UI design and React component development.

## Activate when

- Working in `frontend/`, `ui/`, `components/`, `pages/`, or `app/` directories
- Generating or refining React components
- Running the Stitch design loop (design-md → enhance-prompt → component generation)
- Configuring shadcn-ui themes or component libraries
- Creating Remotion animations or video components

## Skills in this capability

| Skill | Trigger |
|---|---|
| `design-md` | Converting design intent into structured design-md specs |
| `enhance-prompt` | Refining image or design prompts before generation |
| `react-components` | Generating and refining React component implementations |
| `remotion` | Creating Remotion video compositions and animations |
| `shadcn-ui` | Configuring, extending, or composing shadcn-ui components |
| `stitch-design` | Primary orchestrator for the Stitch visual design workflow |
| `stitch-loop` | Iterative design refinement — runs design-md → enhance-prompt → react-components in a loop |

## Stitch Design Loop

The canonical frontend workflow:

```
User intent
  → stitch-design (orchestrate)
    → design-md (produce DESIGN.md spec)
    → enhance-prompt (refine visual prompts)
    → react-components (generate component code)
    → shadcn-ui (integrate into component library)
  → stitch-loop (iterate until approved)
```

## Component Conventions

- All components must be TypeScript with explicit prop interfaces.
- Prefer composition over inheritance; use shadcn-ui primitives as the base layer.
- Co-locate styles with components (Tailwind utility classes, no separate CSS files).
- Storybook stories are expected alongside every non-trivial component.

## Design-md Spec Format

Every design session produces a `DESIGN.md` that documents:
1. Layout intent (wireframe in ASCII or Mermaid)
2. Color palette and typography tokens
3. Interaction states (hover, focus, error, loading)
4. Accessibility requirements (ARIA roles, keyboard nav)
