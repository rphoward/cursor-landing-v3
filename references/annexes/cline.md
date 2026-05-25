# Cline — agent surfaces (scan reference)

**Scope:** Inventory only. This skill does **not** run inside Cline.

**Official docs:** [Cline rules](https://docs.cline.bot/customization/cline-rules), [Memory Bank](https://docs.cline.bot/best-practices/memory-bank)

Reviewed: **May 2026**

---

## Rules

| Surface | Path | Notes |
|---------|------|-------|
| Legacy | `.clinerules` | Single file |
| Scoped | `.clinerules/` | Topic / conditional rules |

---

## Memory Bank (run-state — critical)

Community pattern: `memory-bank/` directory.

| File | Role | Cursor Landing |
|------|------|----------------|
| `projectbrief.md` | Stable product context | Glossary/source for **CONTEXT.md** (terms only) |
| `techContext.md` | Stack context | Short gotchas → **AGENTS.md** if durable |
| `systemPatterns.md` | Patterns | Conduct/boundaries → `.mdc` / AGENTS — not glossary |
| `activeContext.md` | **Ephemeral** current focus | **Emergency handoff only** |
| `progress.md` | **Ephemeral** task log | **Emergency handoff only** |

**Do not** copy `activeContext.md` or `progress.md` into `CONTEXT.md` or `alwaysApply` rules.

Grill **Q11** when `memory-bank/` detected.

---

## MCP

| Surface | Path |
|---------|------|
| Project | `mcp_settings.json` or MCP config in Cline settings (inventory filename if present) |

---

## Scan Report lines to emit

- `.clinerules` vs `.clinerules/` presence
- `memory-bank/` files present; flag activeContext/progress as **run-state**
- Overlap with other tools’ AGENTS/CLAUDE files

---

## Forensics reference

Scoring signals (path tables above stay authoritative): [SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) — `cline_memory_bank`, `run_state_bloat`.
