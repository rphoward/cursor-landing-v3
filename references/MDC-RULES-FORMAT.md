# `.cursor/rules` output format (target repos)

Phase 0 **proposes** a rule set (Scan Report `proposed_mdc_rules`). Phase 2 **writes** only rules grounded in scan + grill — do not invent paths, globs, or foreign-doc claims not in the repo.

**Merge / dual-host:** When `AGENTS.md` or `GEMINI.md` stays in place for another host, extract Cursor-facing bullets into `.mdc` per [MERGE-TO-RULES.md](MERGE-TO-RULES.md). Each proposed rule should list `extract_from` + `merge_preview` in the Phase 0 `(scan_report ...)` block (see [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) `proposed_mdc_rules`).

**Cursor behavior (verify locally):** [Rules](https://cursor.com/docs/context/rules) — project rules in `.cursor/rules/*.mdc`; precedence Team → Project → User; `alwaysApply: true` loads every chat; `globs` attach when matching files are in context; `description` enables agent-requested rules. **`AGENTS.md`** is separate plain markdown (root and nested). **`GEMINI.md` / Antigravity paths** are not Cursor-native — inventory via annexes; do not assume Cursor loads them unless the user @-mentions or a rule links out.

**Pattern:** several small always-on rules + glob-scoped rules (this bundle — no maintainer `.cursor/rules/` tree shipped here).

---

## Template selection

| Path | Templates | `AGENTS.md` in always-on? |
|------|-----------|---------------------------|
| **Cursor-only** (Q6 merge/replace AGENTS) | `conduct.template.mdc`, `safety.template.mdc` | Yes — link for guardrails/proof fallback |
| **Dual-host** (Q14 or leave AGENTS + GEMINI) | `conduct-dual-host.template.mdc`, `safety-dual-host.template.mdc` + `.cursorignore` | **No** — Cursor isolation via `.cursorignore`; proof in `project-proof.mdc` |

## Default rule set (brownfield init)

| File (typical) | `alwaysApply` | Purpose |
|----------------|---------------|---------|
| `conduct.mdc` | true | Links [`CONTEXT.md`](../../CONTEXT.md) + [`AGENTS.md`](../../AGENTS.md); reply style; points to `project-proof.mdc` for proof when written |
| `safety.mdc` | true | Destructive commands, scope, secrets, pause before large edits |

Templates: [assets/conduct.template.mdc](../assets/conduct.template.mdc), [assets/safety.template.mdc](../assets/safety.template.mdc), [assets/conduct-dual-host.template.mdc](../assets/conduct-dual-host.template.mdc), [assets/safety-dual-host.template.mdc](../assets/safety-dual-host.template.mdc), [assets/project-proof.template.mdc](../assets/project-proof.template.mdc) (Q3/Q5 — replace placeholders; omit if Q15 scan_only). Dual-host: [assets/cursorignore.dual-host.template](../assets/cursorignore.dual-host.template).

**Stack / infra (mdc-only):** Catalog [stack-signals.md](stack-signals.md) `(stack_signals_catalog …)`; emit caps in [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) `(stack_mdc_caps …)`. Tier **A** → `proposed_mdc_rules` only — **never** append proof, deploy, or DB/framework lists to `AGENTS.md` / `GEMINI.md` / `CLAUDE.md`. Grill **Q15**: persist Tier A to `.mdc` or scan-only.

| Scan signal | Typical `.mdc` | Activation |
|-------------|----------------|------------|
| Q3 proof | `project-proof.mdc` | `alwaysApply: true` — proof line + link existing AGENTS if present |
| Q5 monorepo root | same file or note in `project-proof.mdc` | always |
| Q7 deploy | `deploy-railway.mdc` (etc.) | `agent_request` or glob on config path |
| `api_openapi` | `api-contract.mdc` | glob or link `openapi.yaml` — do not duplicate spec |
| Legacy AGENTS stack bullets | `stack-*.mdc` | glob + `extract_from` — leave AGENTS unchanged |

**Activation pattern (no hard cap on rule count):** Propose only what scan + grill support. Prefer **thin always-on** (conduct + safety) plus **glob** or **agent_request** rules for stack/docs/architecture. Do not add always-on rules to hit a quota.

| Scan / grill signal | Typical pattern |
|---------------------|-----------------|
| Sparse repo, few guardrails | conduct + **thin** safety always-on; optional one glob from stack |
| Heavy existing `.mdc` / strict CI / ADRs | merge or glob-scope legacy content; avoid duplicating always-on |
| User Q13 “minimal” | conduct + thin safety only |
| User Q13 “thorough” | more glob rules + links to `docs/`; still avoid mega always-on |

**Yolo / fast-moving repos:** Keep **thin safety** always-on; reduce conduct/architecture always-on; use globs and external doc links instead.

**Add more** only when scan or grill justifies them (stack, monorepo package, `src/` vs `docs/`):

| File (example) | Activation | Source |
|----------------|------------|--------|
| `stack-typescript.mdc` | `globs: **/*.{ts,tsx}` | Existing `.cursor/rules`, Copilot `applyTo`, or package layout |
| `docs-style.mdc` | `globs: docs/**,**/*.md` | Repo `docs/` conventions or README |

Prefer **linking** durable foreign docs in rule bodies over copying them:

```markdown
- Domain terms: [CONTEXT.md](../../CONTEXT.md)
- ADRs: `docs/adr/001-auth.md` in the target repo (link in rule body when scan found it)
- API contract: `openapi.yaml` at the target repo root — do not duplicate in rules
```

If a path is **not** in the scan inventory, omit the link or list under Scan Report `open_questions` — do not hallucinate files.

---

## Scan Report: `proposed_mdc_rules` (Phase 0)

Repeat per planned rule:

| Field | Required | Example |
|-------|----------|---------|
| `filename` | yes | `safety.mdc` |
| `activation` | yes | `always` \| `glob` \| `agent_request` |
| `alwaysApply` | yes | true when `activation` is `always` |
| `globs` | if `glob` | `src/**/*.py` |
| `description` | if `agent_request` | Standards for Python services |
| `rationale` | yes | One line from scan/grill evidence |
| `source_paths` | yes | `.cursorrules`, `.github/instructions/foo.instructions.md` |
| `external_links` | optional | `docs/CONTRIBUTING.md` |
| `status` | yes | `write` \| `merge_existing` \| `skip_not_found` |
| `extract_from` | when merging from AGENTS/GEMINI/legacy | `(extract_from (slice …) …)` per [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) `(extract_from_shape …)`; workflow [MERGE-TO-RULES.md](MERGE-TO-RULES.md) |
| `dest_sections` | optional | `conduct` \| `safety` \| `boundaries` \| `proof` \| `stack` \| `workflow` (schema enum) |
| `merge_preview` | when `extract_from` present | One line: what lands in this `.mdc` |

`status: skip_not_found` when a glob or link target has no evidence in the tree.

---

## Phase 2 write order (target repo)

Authoritative workflow: [SKILL.md](../SKILL.md) `(indexing_ignore …)` and route-specific orders in [MERGE-TO-RULES.md](MERGE-TO-RULES.md).

**Always first (normal init, not emergency):** `.cursorindexingignore` at target repo root — baseline from [assets/cursorindexingignore.baseline.template](../assets/cursorindexingignore.baseline.template) (managed block replace-or-skip when `cursor-landing:cursorindexingignore:baseline` markers exist; first-time create or append managed block once otherwise) → append up to **8** Phase 0 `trim_candidates` (`type_id` `indexing_noise`, `suggested_action` `append_indexing_ignore`, `skip_if` path already present) → **`read`** that file once (best-effort index nudge). Do not echo baseline globs inside Phase 0 `(scan_report …)`; do not auto-edit target `.gitignore`.

Then (per grill path — see MERGE):

1. `CONTEXT.md` (glossary)
2. **Dual-host only:** `.cursorignore` (managed block replace-or-skip per dual-host template; append-only outside markers) — then skip step 3 below for AGENTS/GEMINI when Q6 = leave
3. `AGENTS.md` — **default Q6 leave** when brownfield file exists; proof/deploy/monorepo go to **`project-proof.mdc`** / stack globs instead
4. `.cursor/rules/` — **conduct + safety** (template per table above); then **`proposed_mdc_rules`** from scan + Q15 (stack Tier A); skip stack `.mdc` writes when Q15 **scan_only**
5. Optional `CLAUDE.md` bridge per annex (omit from `.cursorignore` when Q14 CLAUDE sub-ask is no)

Q6 applies **per artifact** — see [MERGE-TO-RULES.md](MERGE-TO-RULES.md). Grill **Q13** when Phase 2 creates or reshapes the rules set; **Q14** for dual-host preset (see [question-bank.md](question-bank.md)).

---

## Anti-hallucination

- Empty scan for `.cursor/rules/` → create default pair from templates; do not claim pre-existing rules.
- Scan found N legacy rules → list in report; merge content into proposed set; do not drop without Q6.
- No single mega-`alwaysApply` unless user explicitly chose one combined rule in grill.
