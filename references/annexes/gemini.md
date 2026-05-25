# Google Gemini CLI & Antigravity — agent surfaces (scan reference)

**Scope:** Inventory only. This skill does **not** run inside Gemini CLI or Antigravity.

**Doc freshness (May 2026):** Google shipped **Antigravity IDE** and **Antigravity CLI** on a fast release cadence. Paths and precedence below mix **stable Gemini CLI docs** with **Antigravity 2.0** behavior. Treat conflicting blog/post vs official pages as **verify in the user’s installed version** — do not assume one global layout.

When scanning, **ask which product** the team uses: Gemini CLI only, Antigravity IDE, Antigravity CLI, or more than one.

**Official references:** [get-started](https://antigravity.google/docs/get-started), [gcli-migration](https://antigravity.google/docs/gcli-migration) — precedence, MCP migration, detection env vars, context budget (tables below stay in-repo).

---

## Three products (do not conflate)

| Product | What it is | Primary instruction files |
|---------|------------|---------------------------|
| **Gemini CLI** | Terminal agent (`google-gemini/gemini-cli`) | `GEMINI.md`, `~/.gemini/settings.json` |
| **Antigravity IDE** | Agentic IDE (Google Antigravity) | `AGENTS.md`, `GEMINI.md`, `.agents/rules/`, `.agents/workflows/` (legacy: `.agent/rules/`) |
| **Antigravity CLI** | CLI companion (`agy`) | Shares `~/.gemini/` — **confirm locally** |

Official entry points (check for updates):

- Gemini CLI context: [gemini-md.md](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/gemini-md.md)
- Antigravity: [antigravity.google/docs](https://antigravity.google/docs/get-started)
- Migration: [gcli-migration](https://antigravity.google/docs/gcli-migration) — legacy Gemini CLI deprecation **June 18, 2026**

---

## Root file roles (Antigravity 2.0)

| File | Role |
|------|------|
| **Root `AGENTS.md`** | **Portable cross-tool** constitution — boundaries, conduct, proof (agents.md standard) |
| **Root `GEMINI.md`** | **Gemini/Antigravity-only** — tone, sandbox notes, Google workflows, optional `@` bridges |
| **Nested `**/GEMINI.md`** | **JIT** Gemini-specific overrides when tools touch that subtree (Gemini CLI ancestor walk; Antigravity path-scoped loads — verify locally) |
| **Nested portable** | **`**/AGENTS.md`** or **`.agents/rules/*.md`** with YAML globs — not a second full portable copy inside nested GEMINI |

**Precedence when both exist at workspace root (Antigravity 2.0 — verify locally):**

1. Enterprise / host policy overrides (if present)
2. **`AGENTS.md`** — **wins on conflicts** with GEMINI for the same topic
3. **`GEMINI.md`** — appended / secondary; Gemini-only sections stay here
4. **`.agents/rules/`** (legacy: `.agent/rules/`) — path-scoped modular rules
5. Global `~/.gemini/GEMINI.md` or user memory files
6. Built-in harness defaults

**Do not** tell users “GEMINI often wins over AGENTS” for Antigravity 2.0. That was pre-2.0 / blog drift. Operational `settings.json` permission gates can still override markdown for specific commands — see migration guide.

**Gemini CLI-only repos:** CLI may load only files listed in `context.fileName` — if portable rules live in `AGENTS.md`, scan must flag missing `AGENTS.md` in that list.

---

## Settings and `context.fileName`

| Product | User settings | Workspace settings |
|---------|---------------|-------------------|
| Gemini CLI | `~/.gemini/settings.json` | `<project>/.gemini/settings.json` (workspace overrides user) |
| Antigravity CLI | `~/.gemini/antigravity-cli/settings.json` | `.agents/` workspace tree; may share `.gemini/` |

**Scan:** read `context.fileName` in project `.gemini/settings.json` (and `~/.gemini/` only with permission). Record **all** configured names — e.g. `["AGENTS.md", "CONTEXT.md", "GEMINI.md"]` — not only `GEMINI.md`.

**Modular imports:** `@./path/to/file.md` in GEMINI.md or AGENTS.md (Memory Import Processor). Use for dual-host bridges (e.g. `@./.cursor/rules/conduct.mdc` only when user approves). Antigravity does **not** load `.mdc` natively — Cursor-only content stays in `.cursor/rules/` unless explicitly `@`-imported.

---

## Antigravity 2.0 layout (condensed)

| Signal | Path | Notes |
|--------|------|--------|
| **2.0 workspace signature** | `.agents/` at **opened workspace root** | `rules/`, `skills/`, `workflows/`, `plugins/` |
| **Legacy Gemini workspace** | `.gemini/` | May coexist during migration |
| **MCP (global)** | `~/.gemini/config/mcp_config.json` | Decoupled from inline `settings.json` |
| **MCP (workspace)** | `.agents/mcp_config.json` | Not inline in `.gemini/settings.json` |
| **Skills migration** | `.gemini/skills/` → `.agents/skills/` | Global `~/.gemini/skills/` shared |
| **Detection env** | `ANTIGRAVITY_EXECUTABLE_DATA_DIR` | Set when sidecar/background agents run |
| **Context budget** | ~3000 lines auto-truncation | Keep root AGENTS **under ~200 lines**; core prohibitions in first ~20 lines; detail in `.agents/rules/` |

**Monorepo:** harness uses **opened workspace root**, not always git root. If the user opens `apps/foo/`, parent-repo `AGENTS.md` / `.agents/` may be **ignored**. Emit `layout.workspace_root` in Scan Report and `open_questions` — do not plan to hoist all subtree rules to git root only.

---

## GEMINI.md (Gemini CLI hierarchy)

1. **Global:** `~/.gemini/GEMINI.md`
2. **Workspace:** `GEMINI.md` in workspace dirs and parents
3. **JIT:** loaded when tools touch a path (ancestors up to trusted root)

Commands: `/memory show`, `/memory reload`, `/memory add`.

---

## Skills

Antigravity and Gemini CLI adopt **Agent Skills**-style folders (`SKILL.md`, progressive load). Scan:

- `.agents/skills/**/SKILL.md`
- `~/.gemini/skills/` (global; mention only without permission)
- Any user-named Antigravity skill dirs

---

## Scan Report lines to emit

- Which Google product(s) evidence suggests (CLI vs IDE vs both)
- **`**/GEMINI.md`** locations (workspace + nested JIT)
- **`AGENTS.md`** (root + nested) — portable cross-tool
- **`.agents/`**, `mcp_config.json` (workspace + note global path)
- **`.gemini/settings.json`**, **`context.fileName`** array
- **`layout.workspace_root`** vs git root when monorepo signals exist
- **`ANTIGRAVITY_EXECUTABLE_DATA_DIR`** only if scan script/env inspection allowed
- **Doc uncertainty:** one line if paths conflict with vendor blog vs CLI readme

---

## Scoring reference (lisp bundle)

Path tables above stay authoritative. Host scoring: [SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) `(legacy_host_signals_scoring …)` and signal `antigravity_agents_dir` — not a separate forensics file in this install tree.

---

## Cursor Landing conversion

- **Q14 dual-host** or Q6 **leave** on `AGENTS.md` + `GEMINI.md`: extract Cursor-facing bullets into `.cursor/rules/` per [MERGE-TO-RULES.md](../MERGE-TO-RULES.md) — link GEMINI/AGENTS in rule bodies; do not paste full files into always-on `.mdc`.
- **Q16 GEMINI split** (merge on **both** AGENTS and GEMINI): portable → AGENTS; `leave_native` → GEMINI; Cursor-only → `.mdc` — see MERGE-TO-RULES § Q16 and post-split checklist.
- Populate Scan Report `(extract_from …)` + `merge_preview` per `(extract_from_shape …)` in schema.
