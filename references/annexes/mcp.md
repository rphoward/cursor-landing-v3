# MCP — config surfaces (scan reference)

**Scope:** Inventory only. MCP servers extend agents with tools; configs are **not** product source code.

Reviewed: **May 2026**

---

## Common project paths (flag if present)

| Host | Typical project file |
|------|------------------------|
| Cursor | `.cursor/mcp.json` |
| Claude Code | `.mcp.json` (repo root) |
| Generic / legacy | `mcp.json` |
| Codex | `.codex/config.toml` or plugin-bundled MCP |
| **Antigravity 2.0** | **`.agents/mcp_config.json`** (workspace); **`~/.gemini/config/mcp_config.json`** (global shared — mention only) |

---

## User / machine paths (mention only)

Do not read without user permission:

| Host | Typical user config |
|------|---------------------|
| Claude Code | `~/.claude.json` (includes MCP entries) |
| Claude desktop (legacy) | `claude_desktop_config.json` (still seen in brownfield repos) |
| Cursor | Cursor Settings → MCP (not always a repo file) |

---

## Scan behavior

- Record **server names** and **transport** hints (stdio vs HTTP) if visible in committed JSON
- **Flag secrets:** API keys, tokens, inline passwords — never paste values into Scan Report or `EMERGENCY-HANDOFF.md`
- Note duplicate MCP definitions across `.cursor/mcp.json`, `.mcp.json`, `.github/mcp.json`, `.vscode/mcp.json`, **`.agents/mcp_config.json`**, inline legacy Gemini `settings.json` MCP blocks (stdio port-collision risk)
- Also flag: `.github/mcp.json`, `.vscode/mcp.json` (Copilot)
- **Antigravity 2.0:** inline MCP in `~/.gemini/settings.json` or `.gemini/settings.json` is deprecated — expect standalone `mcp_config.json` per [gemini.md](gemini.md) and [gcli-migration](https://antigravity.google/docs/gcli-migration)

---

## Cursor Landing writes

Default initializer does **not** add or merge MCP servers.

If the user **explicitly asks** to consolidate for Cursor:

1. Offer a single `.cursor/mcp.json` draft — do not overwrite other tools’ files without Q6 **replace**.
2. Replace inline secrets with environment references (e.g. `${env:GITHUB_TOKEN}`); remind user to set vars locally.
3. Test stdio `command` strings in a real terminal before saving (non-interactive `npx` flags, absolute paths).

Emergency mode mentions MCP only if outage blocked on a specific server.

For host-specific MCP details see [claude.md](claude.md), [cursor.md](cursor.md), [codex.md](codex.md).

---

## Scoring reference (lisp bundle)

Path tables above stay authoritative. Duplicate MCP / stdio collision signals: [SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) `mcp_multi_file` and related `(legacy_host_signals_scoring …)`; Antigravity MCP layout: [gemini.md](gemini.md).
