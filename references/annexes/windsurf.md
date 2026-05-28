# Windsurf — agent surfaces (scan reference)

**Scope:** Inventory only. This skill does **not** run inside Windsurf.

**Official docs:** [Workflows](https://docs.windsurf.com/windsurf/cascade/workflows), [Rules](https://docs.windsurf.com/windsurf/cascade/memories) (verify current paths in docs)

Reviewed: **May 2026**

---

## Instructions

| Surface | Path | Notes |
|---------|------|-------|
| Legacy always-on | `.windsurfrules` | Migrate content to scoped rules when converting |
| Scoped rules | `.windsurf/rules/*.md` | Frontmatter activation (always-on, globs) — analogous to Cursor `.mdc` |
| Workflows | `.windsurf/workflows/` | Manual prompt chains — **leave** in place |

---

## Skills and MCP

| Surface | Path |
|---------|------|
| Cross-tool | `AGENTS.md` (if present) |
| MCP | User-level Codeium/Windsurf config — mention only; do not read home without permission |

---

## Cursor Landing conversion (when user chooses merge)

| Source | Cursor target |
|--------|----------------|
| `.windsurf/rules/*.md` | `.cursor/rules/*.mdc` with matching globs |
| Legacy `.windsurfrules` | Split into conduct `.mdc` + AGENTS boundaries |
| Workflows | Document in Scan Report; optional one line in AGENTS — do not port to always-on rules |

---

## Scan Report lines to emit

- Legacy `.windsurfrules` vs `.windsurf/rules/` count
- Workflow folder presence
- Duplicate style rules vs `.cursor/rules` or `CLAUDE.md`

---

## Forensics reference

Scoring signals (path tables above stay authoritative): [SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) — `windsurf_rules`.
