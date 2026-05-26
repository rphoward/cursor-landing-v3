# Repository manifest (cursor-landing v3)

Pseudo-Lisp skill bundle. **Ship repo:** [cursor-landing-v3](https://github.com/rphoward/cursor-landing-v3) — this repository. Flat install: `~/.cursor/skills/cursor-landing/SKILL.md`. Slash: **`/cursor-landing`**.

**Prior public release:** [cursor-landing-v2](https://github.com/rphoward/cursor-landing-v2) — frozen at `5497390` (no P5 indexing); use v3 for current work unless you must stay on v2.

## Required layout

```text
SKILL.md                         ← pseudo-Lisp workflow (/cursor-landing)
README.md                        ← human quick start
OVERVIEW.md                      ← human positioning
LICENSE
.gitignore

references/
  SCAN-REPORT-SCHEMA.md          ← (scan_report ...) + extract_from_shape + stack_mdc_caps
  question-bank.md               ← pseudo-Lisp grill (Q1–Q15)
  scan-checklist.md              ← Phase 0 checkboxes (links catalog + schema caps)
  stack-signals.md               ← pseudo-Lisp (stack_signals_catalog …) — not markdown tables
  examples/scan-report-example.md
  install.md                     ← copy paths (~/.cursor/skills/cursor-landing/)
  troubleshooting.md
  cursor-notes.md                ← Cursor-only execution notes
  HOST-EXTENSION.md              ← maintainer add/remove host recipe
  MERGE-TO-RULES.md              ← dual-host merge (cites extract_from_shape in schema)
  MDC-RULES-FORMAT.md            ← Phase 2 .mdc write order
  CONTEXT-FORMAT.md
  AGENTS-FORMAT.md
  EMERGENCY-HANDOFF-FORMAT.md
  annexes/
    INDEX.md
    cursor.md codex.md claude.md gemini.md copilot.md cline.md windsurf.md
    kiro.md augment-intent.md amp.md mcp.md

assets/
  conduct.template.mdc
  conduct-dual-host.template.mdc
  safety.template.mdc
  safety-dual-host.template.mdc
  cursorignore.dual-host.template
  cursorindexingignore.baseline.template
  project-proof.template.mdc
  CONTEXT.template.md
  AGENTS.template.md
  CLAUDE.bridge.template.md
  EMERGENCY-HANDOFF.template.md

scripts/README.md                ← reserved (no scanner in v1)
```

Parent repo (not copied on install): `lisp_bundle_checks.py`, `check-parens.py`, `validate-lisp-bundle.py` — run before push.

**Authority map:** [MERGE-TO-RULES.md](references/MERGE-TO-RULES.md) = grill policy (Q14/Q16, extraction, closeout); `SKILL.md` + [question-bank.md](references/question-bank.md) = thin loaders; [SCAN-REPORT-SCHEMA.md](references/SCAN-REPORT-SCHEMA.md) = scan shape.

## Pseudo-Lisp vs markdown in this bundle

| File | Format |
|------|--------|
| `SKILL.md`, `SCAN-REPORT-SCHEMA.md`, `question-bank.md`, `stack-signals.md`, `examples/scan-report-example.md` | Leading `; PROMPT NOTATION` + s-expressions |
| Annexes, `install.md`, `troubleshooting.md`, `*-FORMAT.md`, `MERGE-TO-RULES.md`, `MDC-RULES-FORMAT.md` | Markdown (human + maintainer) |
| `assets/*.mdc` | YAML frontmatter + markdown body (target-repo rules) |

Phase 0 chat: one `(scan_report …)` only — no JSON block. `extract_from` shape: schema `(extract_from_shape …)` only.

## Omitted vs markdown v1 publish

- `scan-report.schema.json`, `tests/fixtures/scan-report-example.json` (JSON machine schema — v2 uses pseudo-Lisp schema + example)
- `research/AGENT-FORENSICS-RESEARCH-RESULTS.md` (legacy host scoring in `SCAN-REPORT-SCHEMA.md` `(legacy_host_signals_scoring …)`)
- `references/research/**` (including Antigravity migration guide — use annex tables + official URLs)

## Install check

- `~/.cursor/skills/cursor-landing/SKILL.md` exists (no extra folder layer under skill name)
- `/cursor-landing` appears in Agent `/` menu after Cursor restart
- Phase 0 emits one `(scan_report …)` block only

## Verify before push

- `python github-publish-lisp/validate-lisp-bundle.py` exits 0 (name, forbidden paths, invoke grep, parens)
- `python github-publish-lisp/check-parens.py` exits 0 (SKILL body + pseudo-Lisp refs including `stack-signals.md`)
- [../REVIEW-BEFORE-PUSH.md](../REVIEW-BEFORE-PUSH.md) checklist
