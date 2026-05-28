# Host annexes (scan reference)

Human setup guide (**Cursor Landing**): [../../README.md](../../README.md) — not this file. New-user install mistakes: [../troubleshooting.md](../troubleshooting.md).

**Purpose:** Researched notes on where each AI tool stores project instructions, skills, and config. Used during **Phase 0 scan** to inventory what already exists in a brownfield repo.

**Critical separation:**

| Layer | Location | Role |
|-------|----------|------|
| **This skill’s runtime** | [../cursor-notes.md](../cursor-notes.md) | How to run `/cursor-landing` **in Cursor Agent only** |
| **Host annexes (this folder)** | `cursor.md`, `codex.md`, `claude.md`, `gemini.md`, `copilot.md`, `cline.md`, `windsurf.md`, `kiro.md`, `augment-intent.md`, `amp.md`, `mcp.md` | What files **each tool** may read — for **detection and trimming**, not for executing this skill |
| **Add/remove hosts** | [../HOST-EXTENSION.md](../HOST-EXTENSION.md) | Maintainer recipe — annex + INDEX + checklist |
| **Shared scan** | [../scan-checklist.md](../scan-checklist.md), [../stack-signals.md](../stack-signals.md), [../SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) | Checklist + stack/infra signals + Phase 0 `(scan_report ...)` shape |
| **Grill** | [../question-bank.md](../question-bank.md) | Question ids Q1–Q16 (Q15 stack → `.mdc` or scan-only; Q16 GEMINI split) |
| **Skill packaging** | [../../BUNDLE-MANIFEST.md](../../BUNDLE-MANIFEST.md) | Ship boundary + required files (install bundle) |

**Do not** merge annex content into `AGENTS.md`, `CONTEXT.md`, or `.mdc` during init unless the user asked to document a specific tool.

**Do not** assume Codex paths apply to Claude, or Gemini CLI paths apply to Antigravity IDE, without checking the matching annex.

Annexes last reviewed against vendor docs: **May 2026**. Vendor docs drift — especially **Google Antigravity** after recent IDE + CLI releases; see [gemini.md](gemini.md) freshness note.

| Annex | Tool | This skill runs here? |
|-------|------|------------------------|
| [cursor.md](cursor.md) | Cursor Agent | **Yes** (only host) |
| [codex.md](codex.md) | OpenAI Codex CLI / IDE extension | No — scan + emergency handoff **from** Codex |
| [claude.md](claude.md) | Claude Code | No — scan only |
| [gemini.md](gemini.md) | Gemini CLI + Google Antigravity (IDE & CLI) | No — scan only; doc set in flux |
| [copilot.md](copilot.md) | GitHub Copilot (VS Code / agents) | No — scan only |
| [cline.md](cline.md) | Cline (+ Memory Bank) | No — scan only |
| [windsurf.md](windsurf.md) | Windsurf Cascade | No — scan only |
| [kiro.md](kiro.md) | Amazon Kiro | No — scan only; verify paths locally |
| [augment-intent.md](augment-intent.md) | Augment Intent / Auggie CLI | No — scan only |
| [amp.md](amp.md) | Amp Neo CLI (`@ampcode/cli`) | No — scan only |
| [mcp.md](mcp.md) | MCP config (cross-tool) | No — inventory only |

## Stack / infrastructure (not a host)

| Reference | Role |
|-----------|------|
| [../stack-signals.md](../stack-signals.md) | Pseudo-Lisp `(stack_signals_catalog …)` → emit `(stack_signals ...)`; caps in schema `(stack_mdc_caps …)` (not AGENTS) |

## Checklist-only hosts (no annex)

Inventory via [scan-checklist.md](../scan-checklist.md) and [SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) `(signal_ids)` / `(legacy_host_signals_scoring)` — do not add `references/annexes/<host>.md` unless a fixture later demands it.

| Host | Typical paths | Schema signal |
|------|---------------|---------------|
| **Continue** | `.continue/rules/*.md`, `.continuerc.json` | `continue_rules` |
| **Aider** | `.aider.conf.yml`, `CONVENTIONS.md` | `aider_config` |
| **Amazon Q Developer** | Rare/internal (e.g. `.amazonq/`) — not Kiro | checklist-only; distinct from [kiro.md](kiro.md) |

---

**Naming:** **Harness.io** = CI/CD vendor (optional Kiro MCP). **Agent harness** = engineering pattern (scaffolding around models), not a file path to scan.
