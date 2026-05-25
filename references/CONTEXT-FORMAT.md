# CONTEXT.md format

Glossary only — ubiquitous language for **this repo** (DDD-style terms and disambiguation). Built from Phase 0 `proposed_glossary` and Phase 1 ambiguity resolution; draft in chat until Q6 clears write strategy when agent files already exist.

## Table shape (required)

Use `## Glossary` with a four-column table:

| Term | Meaning | Not confused with | Aliases (optional) |
|------|---------|-------------------|--------------------|

- **Meaning** — one line; what the term means *in this codebase*.
- **Not confused with** — required per row when two agents could disagree (nearby concepts, homonyms).
- **Aliases** — optional; one canonical term per concept; list alternate names here.

Starter template: [../assets/CONTEXT.template.md](../assets/CONTEXT.template.md). Example scan-derived glossary rows: [examples/scan-report-example.md](examples/scan-report-example.md) (`proposed_glossary`).

## Quality bar

| Rule | Detail |
|------|--------|
| **Cap** | **12–20 terms** in v1; expand only with explicit user OK |
| **Inclusion test** | Include only if two agents could disagree on meaning in this repo |
| **Separation** | Glossary → CONTEXT; proof → `project-proof.mdc`; boundaries, conduct → `.mdc` / AGENTS only if Q6 merge — not in CONTEXT |
| **Brownfield sources** | README, types/modules, glossary sections of AGENTS/GEMINI/CLAUDE — not Project Map, run-state, or stack trees |
| **Dedup** | One canonical term; resolve duplicate `proposed_glossary` rows per [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) (aliases column, not a second term) |

## Optional `## Contexts`

Add only when the scan finds a CONTEXT-MAP or multiple bounded contexts worth naming. Do not invent contexts from folder trees alone.

## Exclude

Implementation how-tos, file trees, run state, proof commands (`npm`, `pytest`, CI one-liners), copied architecture, workflow steps.

Merge if file exists; do not delete without asking.
