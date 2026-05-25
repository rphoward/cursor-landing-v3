# Cursor Landing — overview

Brownfield repos pick up leftover agent files and no shared glossary. **Cursor Landing** scans once, asks a few questions, writes durable guidance.

**This bundle (v2):** pseudo-Lisp scan report · **`/cursor-landing`** · install [`references/install.md`](references/install.md)

Not [Cursor’s IDE quickstart](https://cursor.com/docs/get-started/quickstart) — this skill writes repo-local files for agents to read.

## What you get

| Output | Role |
|--------|------|
| `(scan_report ...)` in chat | Phase 0 inventory |
| `.cursor/rules/*.mdc` | Cursor rules (conduct, safety, optional `project-proof.mdc`, stack globs) |
| `CONTEXT.md` | Glossary only |
| `AGENTS.md` | Optional — usually **left** if other hosts use it |
| `docs/EMERGENCY-HANDOFF.md` | Emergency mode only |

Phases and setup: [README.md](README.md). Workflow: [SKILL.md](SKILL.md). Issues: [troubleshooting.md](references/troubleshooting.md).
