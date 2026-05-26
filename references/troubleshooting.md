# Troubleshooting (new users)

**v3 dev bundle:** `/cursor-landing`, folder `cursor-landing`. Phase 0 = one `(scan_report ...)` in chat ([SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md)). Proof/stack → `.mdc` ([MDC-RULES-FORMAT.md](MDC-RULES-FORMAT.md), catalog [stack-signals.md](stack-signals.md), caps in schema); default **leave** `AGENTS.md` / `GEMINI.md`.

Manual skill — type the **`/` command** in **Cursor Agent** on the **target repo**.

## Skill does not appear when you type `/`

| Cause | Fix |
|-------|-----|
| Not installed | Flat copy to `~/.cursor/skills/cursor-landing/` — [install.md](install.md) |
| Wrong name | Pick **cursor-landing** in the `/` menu |
| Stale skill list | Restart Cursor |
| Old experimental install | Remove `~/.cursor/skills/cursor-landing-lisp/` if present; reinstall to `cursor-landing/` |
| Project-local install | Open the repo that contains `.cursor/skills/cursor-landing/` |
| Opened the skill source repo | Install globally, then open your **app** repo |
| Not in Agent | Use Agent chat, not Tab-only |

Plain English without `/cursor-landing` does not load the skill.

## Install path mistakes

| Mistake | Fix |
|---------|-----|
| Nested folder | `SKILL.md` must be `.../cursor-landing/SKILL.md`, not `.../cursor-landing/cursor-landing-skill/SKILL.md` |
| Wrong folder name | Install folder must be `cursor-landing` (matches skill `name` in frontmatter) |
| Global vs local | Global: `~/.cursor/skills/cursor-landing/`; local: `.cursor/skills/cursor-landing/` in target repo |

## Wrong folder open in Cursor

Open **project root** (manifests, app code). Monorepo: repo root unless you mean one package.

## Initializer vs emergency

| Goal | Command |
|------|---------|
| Glossary + rules | `/cursor-landing` |
| Mid-task resume | `/cursor-landing emergency` |

## Git (emergency)

Dirty or missing git is OK — agent still writes `docs/EMERGENCY-HANDOFF.md`. Paste context or **skip**.

## Files created

| Path | Notes |
|------|--------|
| `.cursorindexingignore` | Phase 2: baseline template + up to 8 scan `indexing_noise` paths (append-only); best-effort `read` after write |
| `.cursorignore` | Dual-host (Q14): blocks Cursor from loading left-in-place `AGENTS.md` / `GEMINI.md` / `.agent/` |
| `.cursor/rules/*.mdc` | Init: conduct + safety + optional `project-proof.mdc` (Q3); stack globs if Q15 persist |
| `CONTEXT.md` | Glossary only |
| `AGENTS.md` | Only if Q6 replace/merge — else unchanged |
| `docs/EMERGENCY-HANDOFF.md` | Emergency only |

**Q6** per file. **Q14** dual-host: leave AGENTS/GEMINI, write **`.cursorignore`**, dual-host conduct/safety templates — [MERGE-TO-RULES.md](MERGE-TO-RULES.md). Proof fails → fix **`project-proof.mdc`**, not AGENTS when left.

## Cursor still loads factory AGENTS after Q14

**Symptoms:** Cursor Agent quotes SageClone/swarm/ADK text from root `AGENTS.md` even though `.cursor/rules/` point at `CONTEXT.md`.

**Cause:** Cursor [auto-loads `AGENTS.md`](https://cursor.com/docs/context/rules). Leaving the file for Antigravity does not stop Cursor. Conduct text saying “ignore AGENTS” is not mechanical.

**Fix:**

1. Confirm **`.cursorignore`** at repo root lists `AGENTS.md`, `GEMINI.md`, `**/GEMINI.md`, `.agent/` (and `CLAUDE.md` only if you chose that in Q14).
2. Confirm **dual-host** `conduct.mdc` does not use `AGENTS.md` as Cursor proof or guardrails source.
3. Start a **new Cursor chat** after `.cursorignore` changes so context reloads.

Re-run Phase 2 dual-host steps from [MERGE-TO-RULES.md](MERGE-TO-RULES.md) if `.cursorignore` is missing.

## Wrong repo / other tools

Revert mistaken init on the skill source repo. Skill runs only in Cursor Agent on the target repo; Kiro/Augment/Amp paths are scan-only — [annexes/INDEX.md](annexes/INDEX.md). MCP merge only if you ask (Q12).

## Grill didn’t finish / my answers weren’t applied

Phase 2 waits for **Q6 per artifact** and grill **closeout** you OK’d — see [MERGE-TO-RULES.md](MERGE-TO-RULES.md) and [SKILL.md](../SKILL.md) Phase 1. Declining Q14 or Q16 does **not** end the grill. Say **finish the grill** if writes started early.

## Indexing and ignore files

`.cursorindexingignore` and `.cursorignore` steer what Cursor indexes and loads in the editor and in **@** search — they are **not** full security boundaries; terminals, other tools, and scripts may still read those paths. See [Cursor ignore files](https://cursor.com/docs/reference/ignore-file).

## Dual-host / Q16 split looks broken in Antigravity or Gemini CLI

After **Q16**, agent echoes the post-split checklist in [MERGE-TO-RULES.md](MERGE-TO-RULES.md) § Q16. CLI ignores AGENTS? Add `AGENTS.md` to `context.fileName` — [annexes/gemini.md](annexes/gemini.md).

## Still stuck

[README.md](../README.md) → [install.md](install.md). Try: `/cursor-landing` + *“Confirm this folder before writing files.”*
