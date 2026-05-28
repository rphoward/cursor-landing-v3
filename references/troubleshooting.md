# Troubleshooting2

You are probably tired and in a hurry. Pick **one section** below that matches what went wrong.

**Quick checks**

| Check | What to do |
|-------|------------|
| Command | Type **`/cursor-landing`** in **Cursor Agent** (not Tab-only) on your **app repo**, not this skill’s source repo |
| Install folder | `~/.cursor/skills/cursor-landing/SKILL.md` must exist — [install.md](install.md) |
| After install | Restart Cursor so `/` shows **cursor-landing** |

Plain chat without `/cursor-landing` does **not** run the skill.

---

## `/cursor-landing` does not show up

| Cause | Fix |
|-------|-----|
| Not installed yet | Copy the skill flat into `~/.cursor/skills/cursor-landing/` — [install.md](install.md) |
| Wrong name in menu | Choose **cursor-landing** (not `cursor-landing-lisp` or a nested folder name) |
| Cursor has not refreshed | Quit and restart Cursor |
| Leftover old install | Delete `~/.cursor/skills/cursor-landing-lisp/` if it exists, then install again as **cursor-landing** |
| Skill only in one project | Either use global `~/.cursor/skills/cursor-landing/` or open the repo that has `.cursor/skills/cursor-landing/` |
| Opened the skill source tree | Install globally, then open **your project** repo |
| Wrong chat mode | Use **Agent** chat |

---

## Install path mistakes

| Mistake | Fix |
|---------|-----|
| Extra nested folder | Path must end with `.../cursor-landing/SKILL.md` — not `.../cursor-landing/cursor-landing-skill/SKILL.md` inside skills |
| Wrong folder name | Folder under skills must be **`cursor-landing`** (matches the skill name) |
| Global vs project | Global: `~/.cursor/skills/cursor-landing/` · Project-only: `.cursor/skills/cursor-landing/` in that repo |

---

## Wrong folder open in Cursor

Open your **project root** (where your app code and config live). In a monorepo, that is usually the repo root unless you meant one package only.

---

## Which command?

| You want | Run |
|----------|-----|
| Normal setup (glossary + rules) | `/cursor-landing` |
| Stuck mid-task, need a handoff file | `/cursor-landing emergency` |

---

## Git (emergency only)

Dirty or missing git is fine. The agent can still write `docs/EMERGENCY-HANDOFF.md`. Paste context when asked, or say **skip**.

---

## Files the skill may create

| File | Why it exists |
|------|----------------|
| `.cursorindexingignore` | Keeps heavy folders out of Cursor search; baseline + up to 8 scan paths |
| `.cursorignore` | **Dual-host only:** stops Cursor loading `AGENTS.md` / `GEMINI.md` you keep for another tool |
| `.cursor/rules/*.mdc` | Cursor rules (conduct, safety, optional proof rule) |
| `CONTEXT.md` | Glossary only |
| `AGENTS.md` | Only if you chose replace/merge in the grill — otherwise unchanged |
| `docs/EMERGENCY-HANDOFF.md` | Emergency command only |

If proof fails after init, fix **`project-proof.mdc`** in `.cursor/rules/`, not a left-in-place `AGENTS.md` when you chose dual-host. Details: [MERGE-TO-RULES.md](MERGE-TO-RULES.md).

---

## Cursor still reads old `AGENTS.md` after setup

**What you see:** Agent quotes text from root `AGENTS.md` even though you wanted Cursor to use `CONTEXT.md` and `.cursor/rules/`.

**Why:** Cursor [auto-loads `AGENTS.md`](https://cursor.com/docs/context/rules). Leaving that file for Antigravity does not stop Cursor by itself.

**Fix (in order):**

1. Root **`.cursorignore`** lists `AGENTS.md`, `GEMINI.md`, `**/GEMINI.md`, `.agent/` (and `CLAUDE.md` only if you chose that in the grill).
2. **Dual-host** `conduct.mdc` does not point proof or guardrails at `AGENTS.md`.
3. **New Cursor chat** after `.cursorignore` changes.

Missing `.cursorignore`? Re-run the dual-host steps in [MERGE-TO-RULES.md](MERGE-TO-RULES.md).

---

## Ran init on the wrong repo

If you ran `/cursor-landing` on this skill’s GitHub checkout by mistake, revert those files or restore from git. The skill is meant for **your application repo**, not the install bundle.

Other AI tools (Gemini CLI, etc.) are scan-only unless the grill says otherwise — [annexes/INDEX.md](annexes/INDEX.md). MCP config is not merged unless you ask (Q12).

---

## Grill stopped early or ignored my answers

Phase 2 waits for **your answer per file (Q6)** and a **closeout you approve**. Saying no to Q14 or Q16 does **not** finish the grill. If files appeared too soon, say **finish the grill** and continue [MERGE-TO-RULES.md](MERGE-TO-RULES.md).

---

## Indexing and ignore files

`.cursorindexingignore` and `.cursorignore` affect what Cursor indexes and loads in the editor and in **@** search. They are **not** full security — terminals and other tools may still read those paths. Official reference: [Cursor ignore files](https://cursor.com/docs/reference/ignore-file).

**Running init again:** It is **safe to run `/cursor-landing`** on a repo that already went through setup — the skill refreshes its marked sections instead of duplicating the same baseline or dual-host paths. You might see **duplicate lines** only if the marker comments were removed from an ignore file or you copied paths in by hand outside the skill’s block. Details: [MERGE-TO-RULES.md](MERGE-TO-RULES.md) Phase 2 and Phase 3 closeout.

---

## Antigravity or Gemini CLI after a split (Q16)

Use the checklist in [MERGE-TO-RULES.md](MERGE-TO-RULES.md) § Q16. If the CLI ignores `AGENTS.md`, add it to `context.fileName` — [annexes/gemini.md](annexes/gemini.md).

---

## Still stuck

1. [install.md](install.md) — install steps  
2. [README.md](../README.md) — what the skill does  

Then in Agent on **your repo**: `/cursor-landing` and say *“Confirm this folder before writing files.”*
