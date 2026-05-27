# Forum-friendly host comparison

Short, pasteable summary for Cursor forum / GitHub discussions. It condenses the host annexes in this directory; the annex files remain the source of truth.

## What each tool usually leaves behind

| Tool | Persistent instructions | Run-state / caches to avoid promoting | Cursor Landing default |
|------|-------------------------|---------------------------------------|------------------------|
| Cursor | `.cursor/rules/*.mdc`, `.cursorrules`, `AGENTS.md` | none by default | Write or merge scoped `.cursor/rules/*.mdc`; prefer `.mdc` over legacy `.cursorrules`. |
| Claude Code | `CLAUDE.md`, `.claude/CLAUDE.md`, `.claude/settings.json`, `.mcp.json` | local/user `.claude` state | Add `@AGENTS.md` / `@CONTEXT.md` bridge only when user chooses merge; otherwise inventory. |
| OpenAI Codex | `AGENTS.md`, `AGENTS.override.md`, `.agents/skills/**/SKILL.md`, hooks/config | `PLANS.md` and session plans | Treat `AGENTS.md` as portable; route `PLANS.md` to emergency handoff, not durable rules. |
| GitHub Copilot | `.github/copilot-instructions.md`, `.github/instructions/*.instructions.md` | none by default | Convert scoped `applyTo` rules into separate `.cursor/rules/*.mdc` when merging. |
| Windsurf | `.windsurf/rules/*.md`, `.windsurfrules` | workflows are tool-specific | Port scoped Windsurf rules to `.mdc`; leave workflows unless user asks. |
| Gemini / Antigravity | `GEMINI.md`, `AGENTS.md`, `.agents/rules/`, `.agents/skills/`, `.gemini/settings.json` | `.agents/workflows/`, migration leftovers | Ask whether the repo is dual-host; use `.cursorignore` when Cursor must not load left-in-place files. |
| Cline | `.clinerules`, `.clinerules/`, `memory-bank/` | `memory-bank/activeContext.md`, `progress.md` | Use durable parts as context/rules; route active progress to emergency handoff only. |
| Amazon Kiro | `.kiro/specs/**/{requirements,design,tasks}.md`, `.kiro/steering/*.md` | `.kiro/specs/**/tasks.md` | Requirements/design can inform durable context; tasks are run-state. |
| Augment Intent / Auggie | `.augment/settings.json`, BYOA host files | `.augment/settings.local.json`, worktree/session state | Inventory settings; also scan the underlying host files if BYOA is visible. |
| Amp Neo | `.amp/plugins/`, `@ampcode/cli` references | `.amp/threads/`, `*.chat` dumps | Inventory plugins; keep thread/chat dumps out of durable context. |
| MCP | `.cursor/mcp.json`, `.mcp.json`, `.github/mcp.json`, `.vscode/mcp.json`, `.agents/mcp_config.json` | inline secrets / duplicate local configs | Record server names and transport only; do not merge MCP by default. |

## Practical rule of thumb

1. **Keep one durable source of truth small.** Use `AGENTS.md` / `CONTEXT.md` / scoped `.mdc` rules for stable project facts, not session state.
2. **Do not copy run-state into always-on context.** Session plans, memory-bank progress, threads, and chat dumps belong in an emergency handoff or archive.
3. **For dual-host repos, isolate Cursor deliberately.** If `AGENTS.md` / `GEMINI.md` stays for another tool, write `.cursorignore` and give Cursor its own `.cursor/rules/*.mdc` instead of letting both hosts fight over the same files.
4. **Prefer scoped rules over mega prompts.** Use always-on rules for conduct/safety/proof; use globs or agent-requested rules for stack- or folder-specific guidance.
5. **Inventory MCP, do not blindly consolidate it.** MCP files often contain duplicate server definitions or secret-shaped values; merging them should be an explicit user choice.

## Discourse-safe short version

If your repo has been touched by multiple AI coding tools, split artifacts into three buckets:

- **Durable instructions:** `AGENTS.md`, `CLAUDE.md`, `.cursor/rules/*.mdc`, Copilot instructions, Windsurf rules, Gemini/Antigravity rules.
- **Tool/runtime config:** MCP JSON, settings files, hooks, skill folders.
- **Run-state:** plans, threads, memory-bank progress, chat dumps, task lists.

Cursor Landing should preserve or convert the first bucket, inventory the second, and avoid promoting the third into always-on context.
