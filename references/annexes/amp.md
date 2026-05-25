# Amp Neo CLI — agent surfaces (scan reference)

**Scope:** Inventory only. This skill does **not** run inside Amp.

**Official docs:** [Amp, Rebuilt (Neo)](https://ampcode.com/news/neo), [Chronicle / releases](https://ampcode.com/chronicle)

Package migration (May 2026): **`@ampcode/cli`** replaces legacy **`@sourcegraph/amp`** — flag old package references in repo docs or lockfiles.

Reviewed: **May 2026**

---

## Project and global config

| Surface | Path | Cursor Landing |
|---------|------|----------------|
| Project plugins | `.amp/plugins/` | TypeScript/Bun plugins — inventory; do not execute |
| Project threads | `.amp/threads/` | **Run-state** / conversation dumps — handoff or trim |
| Global plugins | `~/.config/amp/plugins/` | Mention only |
| Chat dumps | `*.chat` or paths noted in project | **Run-state** — exclude from CONTEXT |

Neo removed filesystem rollback and manual thread handoff — do not expect Amp snapshot/rollback artifacts.

---

## Scan Report lines to emit

- `.amp/plugins/` and `.amp/threads/` presence
- Legacy `@sourcegraph/amp` vs `@ampcode/cli` in `package.json` / docs
- Thread/chat files flagged as run-state
- Overlap with other tools if user ran Amp alongside Cursor/Codex

---

## Forensics reference

Scoring signals (path tables above stay authoritative): [SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) — rely on this annex + checklist for Amp paths.
