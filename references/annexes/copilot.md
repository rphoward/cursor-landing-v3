# GitHub Copilot — agent surfaces (scan reference)

**Scope:** Inventory only. This skill does **not** run inside Copilot.

**Official docs:** [Custom instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)

Reviewed: **May 2026**

---

## Instructions

| Surface | Path | Notes |
|---------|------|-------|
| Workspace root | `.github/copilot-instructions.md` | Applies to Copilot Chat in workspace |
| Legacy root | `copilot-instructions.md` | Still seen in brownfield repos |
| Scoped rules | `.github/instructions/*.instructions.md` | YAML frontmatter `applyTo` glob — **scan every file** |

---

## Agents and MCP

| Surface | Path |
|---------|------|
| Custom agents | `.github/agents/*.md` (if present) |
| MCP | `.github/mcp.json`, `.vscode/mcp.json`, `.vs/mcp.json` |

Flag duplicate MCP vs `.cursor/mcp.json` / `.mcp.json` — inventory only per [mcp.md](mcp.md).

---

## Cursor Landing conversion (when user chooses merge)

| Source | Cursor target |
|--------|----------------|
| Glossary-like terms | `CONTEXT.md` only |
| Scoped `applyTo` rules | Separate `.cursor/rules/*.mdc` with equivalent `globs` |
| Root copilot-instructions | Split: conduct → `.mdc`, boundaries/proof → `AGENTS.md` |
| Copilot-only workflows | **Leave** unless Q6 replace |

Do not collapse all scoped instructions into one always-on rule.

---

## Scan Report lines to emit

- Root + `.github/instructions/` file count and `applyTo` patterns
- MCP paths and duplicate server names
- Overlap with `AGENTS.md` / `CLAUDE.md` (duplicate proof commands)

---

## Forensics reference

Scoring signals (path tables above stay authoritative): [SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) — `copilot_scoped_instructions`, `mcp_multi_file`.
