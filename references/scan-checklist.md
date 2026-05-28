# Scan checklist (Phase 0)

Read-only. Record in Scan Report per [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md); do not fix. Per-host paths: [annexes/INDEX.md](annexes/INDEX.md). New hosts: [HOST-EXTENSION.md](HOST-EXTENSION.md).

## Stack and layout

- [ ] Manifests: `package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, `Gemfile`, `pom.xml`
- [ ] Build/CI: `Makefile`, `justfile`, `Dockerfile`, `.github/workflows/*`, etc.
- [ ] Source roots + monorepo hints
- [ ] Stack / infra: load [stack-signals.md](stack-signals.md) `(stack_signals_catalog …)`; emit `(stack_signals …)` + Tier A `proposed_mdc_rules` per caps in [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) `(stack_mdc_caps …)`; grill **Q15** before Phase 2 `.mdc` writes
- [ ] Nested `**/AGENTS.md`, `**/CLAUDE.md`, nested `.cursor/rules/` — note which folder to open in Cursor

## Entry and verify

- [ ] Entry config; test/lint hints in scripts or CI
- [ ] Candidate proof commands (for Scan Report `verify_commands` and grill Q3)

## Agent surfaces

- [ ] Universal: `**/AGENTS.md`, `**/CLAUDE.md`, `.claude/CLAUDE.md`, `.agents/skills/**/SKILL.md`, `.cursor/skills/**/SKILL.md`
- [ ] Cursor: `.cursor/rules/*.mdc`, `.cursor/agents/` if present, `.cursor/hooks.json` if present (inventory only; [Hooks](https://cursor.com/docs/hooks)), legacy `.cursorrules` — [annexes/cursor.md](annexes/cursor.md)
- [ ] Codex: `.codex/config.toml`, `.codex/hooks.json`, `AGENTS.override.md` — [annexes/codex.md](annexes/codex.md)
- [ ] Claude: `.claude/settings.json`, `.claude/settings.local.json`, `.claude/rules/`, `.claude/skills/`, `.claude/agents/` — [annexes/claude.md](annexes/claude.md)
- [ ] Gemini / Antigravity: `**/GEMINI.md`, root `AGENTS.md`, `.agents/` (rules, skills, workflows, `mcp_config.json`), `.agent/rules/` (legacy), `.gemini/settings.json` **`context.fileName`**, `layout.workspace_root` vs git root — [annexes/gemini.md](annexes/gemini.md)
- [ ] Copilot: `.github/copilot-instructions.md`, `.github/instructions/*.instructions.md`, `.github/agents/` if present — [annexes/copilot.md](annexes/copilot.md)
- [ ] Windsurf: `.windsurfrules`, `.windsurf/rules/`, `.windsurf/workflows/` — [annexes/windsurf.md](annexes/windsurf.md)
- [ ] Cline: `.clinerules`, `.clinerules/`, `memory-bank/` — [annexes/cline.md](annexes/cline.md)
- [ ] Kiro: `.kiro/specs/`, `.kiro/steering/`, `.hooks/`, `.kiro/settings/mcp.json` — [annexes/kiro.md](annexes/kiro.md)
- [ ] Augment Intent: `.augment/settings.json`, `.augment/settings.local.json` — [annexes/augment-intent.md](annexes/augment-intent.md)
- [ ] Amp Neo: `.amp/plugins/`, `.amp/threads/` — [annexes/amp.md](annexes/amp.md)
- [ ] Continue: `.continue/rules/`, `.continuerc.json` — **checklist-only** (no annex; signal `continue_rules` in [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md))
- [ ] Aider: `.aider.conf.yml`, `CONVENTIONS.md` — **checklist-only** (no annex; signal `aider_config` in [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md))
- [ ] Amazon Q Developer: rare/internal IDE paths (e.g. `.amazonq/`, Q-related settings) — **checklist-only**; not Kiro ([annexes/kiro.md](annexes/kiro.md))
- [ ] MCP — [annexes/mcp.md](annexes/mcp.md) (names only; flag secrets and duplicates)

Home dirs (`~/.cursor`, `~/.codex`, etc.): mention only; don’t read without permission.

## Glossary candidates (CONTEXT.md)

- [ ] Candidate domain nouns from README, agent glossary sections (`content_kind: glossary`), and public API / type names — populate `(scan_report (proposed_glossary ...))`; exclude architecture, run-state, proof, and stack trees

## Run-state exclusions (do not promote to CONTEXT or always-on rules)

- [ ] `memory-bank/activeContext.md`, `memory-bank/progress.md` (Cline)
- [ ] Kiro `.kiro/specs/**/tasks.md` (active task sequence)
- [ ] `.augment/settings.local.json`, `.amp/threads/`, `.chat` dumps (Amp/Augment)
- [ ] Codex `PLANS.md` or similar session plans (if present)
- [ ] Claude auto memory: `~/.claude/projects/*/memory/` (home; mention only) — not the same as in-repo `.claude/agent-memory/` (subagent memory)
- [ ] AGENTS/CLAUDE sections matching: Current task, Next steps, Sprint, Slice, STATUS (`run_state_bloat` in [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) → `trim_candidates`)

Route to `docs/EMERGENCY-HANDOFF.md` or trim — not `CONTEXT.md`.

## Indexing noise (`.cursorindexingignore` trim candidates)

Phase 0 only — record in `(scan_report (trim_candidates …))`; Phase 2 appends paths. See [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) `(indexing_noise_rows …)`.

- [ ] **Do not** paste globs from `assets/cursorindexingignore.baseline.template` into the scan report — baseline is written at Phase 2 from that template, not echoed in chat
- [ ] Repo-specific build output, vendor trees, caches, and similar paths that waste codebase search — emit as `trim_candidates` with `type_id` **`indexing_noise`**, `suggested_action` **`append_indexing_ignore`**, repo-relative `file_path` (directory or glob line as the target file will receive)
- [ ] **Cap 8** `indexing_noise` rows per scan report (prioritize largest noise: e.g. `node_modules/`, `dist/`, `build/`, `.next/`, `target/`, `__pycache__/`, `.venv/`, `vendor/`, coverage output — only when present on disk)
- [ ] Do not use `indexing_noise` for run-state logs (`run_state` + `emergency_only` or `exclude_from_context` instead)

## Health

- [ ] Bloated AGENTS / run-state; duplicate rules across tools; secrets in MCP JSON; stale README verify
- [ ] Dual-host (AGENTS + GEMINI / `.agent/`): note Q14 path — propose `.cursorignore` paths (`AGENTS.md`, `GEMINI.md`, `**/GEMINI.md`, `.agent/`; optional `CLAUDE.md` in `open_questions`); `proposed_mdc_rules` with `(extract_from (slice …) …)` per [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) `(extract_from_shape …)` and [MERGE-TO-RULES.md](MERGE-TO-RULES.md); dual-host templates — do not paste whole AGENTS/GEMINI into always-on `.mdc`
- [ ] Legacy host signals (`legacy_host_signals` — scoring in [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) `(legacy_host_signals_scoring …)`)
- [ ] Conflicting proof commands across agent files

## Scan Report shape

**Emit:** one fenced pseudo-Lisp `(scan_report ...)` per [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) (schema **2.2** optional `stack_signals`). Example: [examples/scan-report-example.md](examples/scan-report-example.md). Chat only in Phase 0 — no JSON, no parallel markdown summary table. Stack persistence: **`.mdc` only** — catalog [stack-signals.md](stack-signals.md); caps in schema `(stack_mdc_caps …)`.
