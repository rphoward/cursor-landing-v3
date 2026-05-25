# Merge to Cursor rules (dual-host and brownfield)

Phase 0 **proposes** extractions; Phase 1 **confirms** them; Phase 2 **writes** `.mdc` files with traceable `source_paths`. Do not copy whole `AGENTS.md` or `GEMINI.md` into always-on rules.

**Target pattern:** thin always-on (`conduct.mdc`, `safety.mdc`, optional `project-proof.mdc`) + glob-scoped detail — see [MDC-RULES-FORMAT.md](MDC-RULES-FORMAT.md).

Scan shape and trim signals: [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) (artifact routing in `proposed_mdc_rules`, `trim_candidates`).

---

## When to use which path

| User goal | AGENTS.md / GEMINI.md | CONTEXT.md | `.cursor/rules/` |
|-----------|------------------------|------------|------------------|
| **Dual-host** (Cursor + Antigravity) | **leave** — native tools keep loading them | **write** or **merge** glossary only | **write** + **extract** guardrails from AGENTS/GEMINI into `.mdc` |
| **Cursor-only brownfield** | **merge** → slim [AGENTS-FORMAT.md](AGENTS-FORMAT.md) or **replace** from template | **merge** / **replace** | **merge** / **replace** per Q6 on rules |
| **Greenfield** (no agent files) | **write** from template + grill | **write** | **write** default conduct + safety |

**Dual-host is a successful init** when Cursor gets glossary + rules; leaving AGENTS/GEMINI is intentional, not a failed slim-AGENTS step.

**Declining Q14 or Q16 does not end the grill** — still complete **Q6 per artifact**. See [question-bank.md](question-bank.md) and [SKILL.md](../SKILL.md) grill closeout gate.

---

## Content routing (`content_kind` → destination)

Use during Phase 0 `extract_from`, Phase 1 extraction tables, and **Q16** split rows. **Forbidden during Q16:** Cursor-only bullets → **AGENTS.md**; deleting GEMINI `leave_native` without a confirmed row.

| `content_kind` | Destination | Cursor? | Antigravity / Gemini? |
|----------------|-------------|---------|-------------------------|
| **boundaries**, **conduct**, **proof** (cross-tool) | **AGENTS.md** | Yes | Yes — AGENTS wins |
| **workflow**, Antigravity tone, sandbox | **GEMINI.md** (`leave_native`) | If `@` cited | Yes |
| Cursor style, `.mdc` hints | **`.cursor/rules/`** | Yes | No unless `@` |
| **glossary** | **CONTEXT.md** | Via links | If in `context.fileName` |
| **run_state** | trim / emergency | No | No |

---

## Q6 — per artifact (not one answer for all files)

Ask **separately** when scan found the file (or offer the dual-host preset in one shot):

| Artifact | replace | merge | leave |
|----------|---------|-------|-------|
| `CONTEXT.md` | New glossary from grill | Add terms; dedupe with scan | No CONTEXT writes |
| `AGENTS.md` | Slim [AGENTS-FORMAT](AGENTS-FORMAT.md) file | Reshape to template; fold boundaries; strip run-state; **Q3 proof → `project-proof.mdc`** not AGENTS | **No edits** (default brownfield / dual-host) |
| `GEMINI.md` | Rare — only if user wants Cursor to own it | Add cross-links only; do not duplicate AGENTS body | **No edits** (Antigravity-native) |
| `.cursor/rules/` | Replace set with proposed rules | Merge into existing `.mdc` per scan | No rule writes |
| `CLAUDE.md` | Bridge template | Add `@AGENTS.md` / `@CONTEXT.md` imports | No edits |

**Dual-host preset (offer when `GEMINI.md` or `.agent/` + `AGENTS.md` detected):**

> Leave `AGENTS.md` and `GEMINI.md` unchanged; write thin `CONTEXT.md`; **write or merge `.cursorignore`** so Cursor does not auto-load left-in-place agent files; build `.cursor/rules/` from **dual-host templates** (`conduct-dual-host`, `safety-dual-host`, `project-proof`) by extracting Cursor-facing guardrails from AGENTS, GEMINI, `.agent/rules/`, and legacy `.cursorrules`.

**Why `.cursorignore`:** Cursor [auto-loads root `AGENTS.md`](https://cursor.com/docs/context/rules). MDC text alone cannot block that. `.cursorignore` excludes Agent context, search, and `@` mentions. Antigravity does not read `.cursorignore`.

**Default `.cursorignore` paths (Q14 yes):** `AGENTS.md`, `GEMINI.md`, `**/GEMINI.md`, `.agent/` — template [assets/cursorignore.dual-host.template](../assets/cursorignore.dual-host.template). **Merge append-only** if `.cursorignore` already exists. **Optional:** `CLAUDE.md` when Q14 sub-ask is yes (Claude Code still uses `CLAUDE.md`; Cursor will not load it when ignored).

---

## Glossary merge (CONTEXT.md)

Before Phase 2 writes `CONTEXT.md`, merge:

1. Scan Report **`proposed_glossary`** (approved rows)
2. Phase 1 disambiguation (Q4, duplicate-term resolution)

Write the four-column table per [CONTEXT-FORMAT.md](CONTEXT-FORMAT.md). One canonical term per concept; aliases in the table. Do not copy proof, boundaries, or stack trees into CONTEXT.

---

## Phase 0 — evidence before merge

For each row in `proposed_mdc_rules`, cite **concrete** sources. In Phase 1, show a short **extraction table** (chat) before any write:

| Target `.mdc` | `source_paths` | Section / signal | Lands as |
|---------------|----------------|------------------|----------|
| `conduct.mdc` | `AGENTS.md` §4 | Behavioral constraints bullets 1–3 | always-on conduct |
| `safety.mdc` | `GEMINI.md`, `.agent/rules/safety.md` | Destructive / secrets | always-on safety |
| `stack-python.mdc` | `AGENTS.md` §3, `pyproject.toml` | Project map bullets (not proof command) | glob `**/*.py` |

**Required in Scan Report** `(scan_report ...)` when merging — canonical shape **`(extract_from_shape …)`** in [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) only (no JSON object literals in Phase 0 chat):

```text
(extract_from
  (slice (path AGENTS.md) (section "Behavioral constraints") (content_kind conduct))
  (slice (path GEMINI.md) (content_kind safety)))
(dest_sections conduct)
(merge_preview "One line: what lands in this .mdc")
```

- **`content_kind`** — `glossary` | `boundaries` | `conduct` | `safety` | `proof` | `stack` | `workflow` | `run_state` | `leave_native`
- **`run_state`** → trim candidate / emergency only — never always-on `.mdc`
- **`leave_native`** → stays in AGENTS/GEMINI; rule may **link** only

If the agent cannot fill `extract_from` + `merge_preview` for a proposed rule, **do not claim merge** — ask the user or narrow scope.

---

## Phase 2 — extract, do not duplicate

**Order (Q16 yes):** Split AGENTS/GEMINI per extraction table **first** → `CONTEXT.md` → `.cursor/rules/` (extract from **updated** files). **Forbid** split until Q6 **merge on both** AGENTS and GEMINI.

**Order (dual-host, no Q16):** `CONTEXT.md` → **`.cursorignore`** (merge) → `.cursor/rules/` → optional `CLAUDE.md` bridge (only when not in `.cursorignore`). **Skip** `AGENTS.md` / `GEMINI.md` when Q6 = leave.

1. **CONTEXT** — approved `proposed_glossary` + Phase 1 Q4 disambiguation; fallback: nouns from AGENTS/GEMINI **glossary sections only** (not project map trees). See [CONTEXT-FORMAT.md](CONTEXT-FORMAT.md).
2. **`.cursorignore`** — from [cursorignore.dual-host.template](../assets/cursorignore.dual-host.template); include paths user confirmed in Q14 closeout; uncomment `CLAUDE.md` only when Q14 sub-ask is yes.
3. **conduct.mdc + safety.mdc** — from **dual-host templates** ([conduct-dual-host.template.mdc](../assets/conduct-dual-host.template.mdc), [safety-dual-host.template.mdc](../assets/safety-dual-host.template.mdc)), then **graft** bullets from `extract_from` (dedupe; max ~40 lines always-on each). **Do not** link `AGENTS.md` or `GEMINI.md` as Cursor instruction or proof source in always-on rules.
4. **Glob / agent_request rules** — one concern per file; link [`CONTEXT.md`](../../CONTEXT.md). Optional deferral lines for humans only (not Cursor instruction source):

   ```markdown
   - Terms: [CONTEXT.md](../../CONTEXT.md)
   - Antigravity (other host): see GEMINI.md at repo root — not loaded in Cursor when listed in .cursorignore
   ```

5. **Proof** — see dual-host proof table below.

**Dual-host proof:**

| Q6 on AGENTS | Where proof lives |
|--------------|-------------------|
| **leave** (default dual-host) | Q3 → **`project-proof.mdc`** or agent_request verify; AGENTS may keep Antigravity prose |
| **merge** (incl. Q16) | Proof in **AGENTS** per [AGENTS-FORMAT.md](AGENTS-FORMAT.md); `.mdc` links only |

Omit `project-proof.mdc` when Q15 **scan_only**.

**Forbidden (dual-host):**

- Pasting full AGENTS or GEMINI into always-on `.mdc`
- Always-on `.mdc` linking **AGENTS.md** or **GEMINI.md** as Cursor guardrails or proof source (MDC “ignore AGENTS” is advisory only)
- Skipping `.cursorignore` on Q14 / dual-host path when AGENTS or GEMINI stay at repo root
- Rewriting GEMINI/AGENTS “to slim” when Q6 = leave
- Inventing `extract_from` paths not in scan inventory
- **Cursor-only** content into **AGENTS.md** during Q16

**Alternatives when `.cursorignore` is not enough:**

- Thin Cursor-only root `AGENTS.md` + rename factory file — breaks Antigravity `@./AGENTS.md` unless symlink (poor on Windows)
- Move factory agent docs under `.agent/` and update Antigravity imports — larger migration
- User global Cursor ignore list — less portable than repo `.cursorignore`

---

## Q16 — GEMINI split (portable vs Gemini-only)

**When:** Root `AGENTS.md` + `GEMINI.md` and scan shows **portable overlap** (duplicate boundaries/conduct/proof).

**Hard gates:** Q6 **merge on both** — GEMINI-only merge invalid. Extraction table with `content_kind` per row; user confirms. **Forbid Phase 2** until closeout OK per [SKILL.md](../SKILL.md).

**Post-split checklist (echo done/skipped in chat):**

1. **`context.fileName`** includes **`AGENTS.md`** if portable rules moved there.
2. **`@` imports** removed from GEMINI still reachable from **AGENTS**.
3. **Reconcile `.mdc`** — no duplicate bullets now only in AGENTS.
4. **Nested** — matching **`**/AGENTS.md`** or **`.agents/rules/`** if nested GEMINI trimmed.

---

## Phase 3 — verify dual-host init

| Check | Pass |
|-------|------|
| `CONTEXT.md` | Glossary-only per [CONTEXT-FORMAT.md](CONTEXT-FORMAT.md); four-column table; ≥1 row; no proof/path trees; ≤ ~60 lines unless user OK |
| `.cursor/rules/` | ≥2 `.mdc` (or Q13 one combined); each `proposed_mdc_rules` row has matching file or documented skip |
| `AGENTS.md` / `GEMINI.md` | Unchanged if Q6 leave (git or user confirm) |
| Extraction trace | Chat or handback lists `source_paths` → rule mapping |
| Proof | Q3 command runnable or noted *proof not run* |
| Q16 post-split | If Q16 yes: checklist (1)–(4) echoed |
| `.cursorignore` | Exists at repo root; lists `AGENTS.md`, `GEMINI.md`, `.agent/` (and `CLAUDE.md` if Q14 sub-ask yes); merge did not remove unrelated user entries |
| `conduct.mdc` (dual-host) | Uses dual-host template shape; proof from `project-proof.mdc` — not `AGENTS.md` as Cursor source |
| New Cursor chat | User reminded to start a **new** Cursor chat after `.cursorignore` write (context reload) |
| Grill closeout | Phase 1 summary confirmed before Phase 2 — [SKILL.md](../SKILL.md); Q14 closeout listed ignore paths |

---

## Mapping scan signals → rules

| Source signal | Default destination |
|---------------|---------------------|
| Glossary / nouns in CLAUDE, GEMINI, AGENTS intros | `CONTEXT.md` |
| Boundaries, never-touch dirs, secrets, compliance (Q10) | `safety.mdc` (or conduct if Q13 combined) |
| Reply style, task focus | `conduct.mdc` |
| Proof / test / CI commands (Q3); runtime pins (Q9); monorepo root (Q5) | **`project-proof.mdc`** (`alwaysApply`) — not AGENTS when Q6 leave |
| Stack, monorepo layout, package roots | glob `stack-*.mdc` (Q15 persist) |
| Run-state (tasks, sprint, STATUS) | emergency / exclude — not `.mdc` |
| Architecture / ADRs | glob `docs/**` or link in rule body |
| Antigravity-only workflows | link `GEMINI.md`; optional glob on `DocTools/**` etc. |

---

## Weak merge — escalation

If the user asks for merge but the agent cannot produce `extract_from` + `merge_preview` for the planned rules:

1. Offer **dual-host preset** (leave AGENTS/GEMINI; rules + CONTEXT only).
2. Or **replace** `.cursor/rules/` only with template + grill answers.
3. Do not silently choose **leave** for everything.
