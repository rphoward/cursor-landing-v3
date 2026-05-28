# Augment Intent / Auggie CLI — agent surfaces (scan reference)

**Scope:** Inventory only. This skill does **not** run inside Augment Intent.

**Official docs:** [Intent overview](https://docs.augmentcode.com/intent/overview), [CLI configuration](https://docs.augmentcode.com/cli/config)

Intent is a Mac-native orchestration workspace; execution may **BYOA** (Claude Code, Codex, OpenCode, etc.) — if detected, also scan those hosts per their annexes.

Reviewed: **May 2026**

---

## Project settings (Auggie / Augment)

| Level | Path (typical) | Cursor Landing |
|-------|----------------|----------------|
| Project (committed) | `.augment/settings.json` | Team MCP profiles and shared settings — inventory |
| Project local | `.augment/settings.local.json` | **Run-state / secrets** — do not promote to CONTEXT; flag for trim |
| User | `~/.augment/settings.json` | Mention only unless user grants access |
| Managed (org) | `/etc/augment/settings.json`, `C:\ProgramData\augment\settings.json` | Mention only |

---

## Execution notes (scan layout, not paths)

- Intent may use **isolated git worktrees** per workspace — note in Scan Report `layout` if evidence exists (`.git/worktrees`, branch names), not as instruction files.
- Requires **Node.js 22+** and `@augmentcode/auggie` globally for CLI workflows — record in `stack` if manifests hint at Augment.

---

## Scan Report lines to emit

- `.augment/settings.json` vs `.augment/settings.local.json` (flag local as run-state/secrets)
- Overlap with Codex / Claude / Cursor agent files when BYOA is inferred
- No automatic merge of `.augment` MCP into `.cursor/mcp.json`

---

## Forensics reference

Scoring signals (path tables above stay authoritative): [SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) — rely on this annex + checklist for Augment paths.
