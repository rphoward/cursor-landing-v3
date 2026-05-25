# Claude Code — agent surfaces (scan reference)

**Scope:** Inventory only. This skill does **not** run inside Claude Code.

**Official docs:** [Settings / configuration](https://code.claude.com/docs/en/configuration), [MCP](https://code.claude.com/docs/en/mcp), [Memory / CLAUDE.md](https://code.claude.com/en/memory), [Skills](https://code.claude.com/en/skills), [Hooks](https://code.claude.com/en/hooks)

Reviewed: **May 2026**

---

## CLAUDE.md (memory / instructions)

| Scope | Path |
|-------|------|
| User | `~/.claude/CLAUDE.md` |
| Project | `CLAUDE.md` or `.claude/CLAUDE.md` |
| Local (gitignored) | `CLAUDE.local.md` |
| Managed | org policy / `claudeMd` in managed settings |

Closest file to working directory wins for directory-specific overrides. Scan nested `**/CLAUDE.md` if present.

**Scan:** flag duplicate guidance that also appears in `AGENTS.md` or `.cursor/rules`.

**Auto-load:** Claude Code injects **`CLAUDE.md` at session start**, not `AGENTS.md` (see [Memory / CLAUDE.md](https://code.claude.com/en/memory#agentsmd)). Cross-tool portability: project `CLAUDE.md` should import shared repo docs:

```markdown
@AGENTS.md
@CONTEXT.md
```

Symlink `CLAUDE.md` → `AGENTS.md` works on Unix; on Windows prefer `@` imports. Init Phase 2 can add missing import lines when Q6 is not **leave**.

---

## Settings JSON

| Scope | Path |
|-------|------|
| User | `~/.claude/settings.json` |
| Project (shared) | `.claude/settings.json` |
| Project (local) | `.claude/settings.local.json` |

Precedence (high → low): Managed → CLI args → Local → Project → User.

Other state: `~/.claude.json` (OAuth, user/local MCP, caches).

---

## Subagents

| Scope | Path |
|-------|------|
| User | `~/.claude/agents/` |
| Project | `.claude/agents/` |

---

## Skills

Claude Code skills use **`SKILL.md`** in skill folders (often via plugins or `.claude` skill paths). Settings keys include `skillListingBudgetFraction`, `skillOverrides`, `disableSkillShellExecution` (policy).

Scan for `.claude/skills/` or plugin-declared skills; list names only in Scan Report.

---

## MCP

| Scope | Path |
|-------|------|
| Project (VC) | `.mcp.json` at repo root |
| User / local | `~/.claude.json` (per-project entries) |

Team-shared servers belong in **`.mcp.json`**. See [mcp.md](mcp.md).

---

## Hooks

Configured in `settings.json` under `hooks`. HTTP hooks may use `allowedHttpHookUrls`. Managed deployments can restrict with `allowManagedHooksOnly`.

---

## Plugins / marketplace

Plugins can bundle skills, agents, hooks, MCP. Scan `.claude/` for plugin config; do not conflate with Cursor rules.

---

## Scan Report lines to emit

- `CLAUDE.md` vs `AGENTS.md` overlap; missing `@AGENTS.md` / `@CONTEXT.md` imports
- `.claude/settings.json` vs `.cursor/rules` — same project described inconsistently (review; not a runtime conflict)
- `.mcp.json` present (note for team)

---

## Forensics reference

Scoring signals (path tables above stay authoritative): [SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) — `claude_instructions`, `mcp_multi_file`.
