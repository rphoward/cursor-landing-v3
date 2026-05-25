# OpenAI Codex — agent surfaces (scan reference)

**Scope:** Inventory and **emergency handoff source** when user switches **from Codex → Cursor**. This skill does **not** run inside Codex.

**Official docs:** [AGENTS.md guide](https://developers.openai.com/codex/guides/agents-md), [Skills](https://developers.openai.com/codex/skills), [Hooks](https://developers.openai.com/codex/hooks), [Config](https://developers.openai.com/codex/config-reference)

Reviewed: **May 2026**

---

## AGENTS.md (instructions)

Discovery order (simplified):

1. **Global:** `~/.codex/AGENTS.override.md` or `~/.codex/AGENTS.md` (one non-empty file)
2. **Project:** walk from repo root toward CWD; per directory: `AGENTS.override.md`, then `AGENTS.md`, then optional fallbacks in `config.toml` (`project_doc_fallback_filenames`)
3. **Merge:** concatenated root → leaf; later files override in prompt order
4. **Limit:** combined size capped (`project_doc_max_bytes`, default 32 KiB)

**Scan:** list all `**/AGENTS.md` and `**/AGENTS.override.md`; flag run-state paragraphs for trim into durable guardrails vs `docs/EMERGENCY-HANDOFF.md`.

**Home dir:** `CODEX_HOME` can point at a non-default profile (e.g. repo-local `.codex/`). Do not read `~/.codex/` unless user grants access — note “ask user to paste global AGENTS if relevant.”

---

## Skills

Codex uses the [Agent Skills](https://agentskills.io/) layout: directory + **`SKILL.md`** with `name` and `description`.

| Scope | Path (typical) |
|-------|----------------|
| Repo / CWD walk | `$CWD/.agents/skills`, parents up to repo root |
| User | `$HOME/.agents/skills` |
| Admin / system | `/etc/codex/skills`, bundled system skills |

Also listed in docs: **`$REPO_ROOT/.agents/skills`**. Progressive disclosure: metadata first, full skill on use.

Repo may also contain **`.codex/skills/`** — Cursor loads these for compatibility; still attribute to Codex in Scan Report.

Disable via `~/.codex/config.toml`: `[[skills.config]] path = "..." enabled = false`.

---

## Hooks

| Location | File |
|----------|------|
| User | `~/.codex/hooks.json`, `~/.codex/config.toml` `[hooks]` |
| Project | `.codex/hooks.json`, `.codex/config.toml` |

Events include session start, pre/post tool use, permission request, user prompt, stop. Flag in scan; do not edit during `/cursor-landing` unless user asks.

---

## Plugins / distribution

Reusable distribution via Codex **plugins** (skills + optional MCP bundled). Scan for plugin manifests if present; treat as Codex-specific.

---

## Emergency handoff (Codex → Cursor)

When `/cursor-landing emergency` follows Codex work, capture in `docs/EMERGENCY-HANDOFF.md` per [EMERGENCY-HANDOFF-FORMAT.md](../EMERGENCY-HANDOFF-FORMAT.md):

- Branch and git status
- Last Codex task from chat (and **`PLANS.md` excerpt** when present — see below)
- Files Codex touched
- Last shell commands / test results
- Blockers (auth, sandbox, failing tests)
- Resume prompt aimed at **Cursor Agent**

Do **not** copy transient Codex session state into slim `AGENTS.md`, **CONTEXT.md**, or always-on `.mdc`.

### PLANS.md (run-state)

Codex may leave a session plan file — treat it like Cline `memory-bank/activeContext.md` / `progress.md`: **emergency capture only**, not durable agent docs.

| Typical path | Role |
|--------------|------|
| Repo root `PLANS.md` | Most common; scan checklist flags when present |
| Other paths | If scan inventory lists a Codex plan file, use that path |

**Excerpt into handoff:** When `PLANS.md` (or scan-listed equivalent) exists, read the file and pull relevant lines into `docs/EMERGENCY-HANDOFF.md` **section 3 Current task** (`handoff_section_3` in SKILL; or a short labeled subsection under it): current task, blockers, next steps. Cap total excerpt at **~40 lines**; prefer headings and bullet lists over pasting the whole file. If the file is missing or empty, write `unknown` for that portion — do not invent task state.

**Do not promote** `PLANS.md` into `CONTEXT.md`, `AGENTS.md`, or `.cursor/rules/*.mdc`. Grill **Q11** applies only to Cline `memory-bank/` — there is no separate grill question for `PLANS.md`.

---

## Forensics reference

Scoring signals (path tables above stay authoritative): [SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) — `codex_agents_md`, `mcp_multi_file`, `run_state_bloat`.
