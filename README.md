# Cursor Landing Skill

Your tokens just ran out. Mid-task.

Not your fault. A news cycle, a market spike, a million people asking the same question at 2am — demand overwhelmed supply and your session hit the wall. The work is still there. The agent that understood your repo is gone.

Now you're looking at Cursor. New tool, fresh start, competitive model, high limits. The switch makes sense. But your repo has history — words your team uses that mean something specific, folders nobody should touch, a test command that isn't obvious. You taught your last agent all of it. Piece by piece, session by session.

You're about to do it again.

**Cursor Landing** stops that. One command reads your repo and writes durable agent guidance. [Install](references/install.md) the skill to `~/.cursor/skills/cursor-landing/`, open your app repo in Cursor Agent, and run **`/cursor-landing`**.

---

## Your repo stays yours

The first pass is **look only** — a scan in chat, not edits to your app or a takeover of configs another tool already owns.

Full init: nothing hits disk until the short grill and your **OK** — then usually a glossary and Cursor rules. **`AGENTS.md`**, Claude, Copilot, Windsurf, and MCP files default to **leave** unless you pick replace or merge. Your manifests, source, and CI stay out of scope unless you say so.

**Emergency** (`/cursor-landing emergency`) skips the grill and writes only **`docs/EMERGENCY-HANDOFF.md`** so you can resume; delete it after. Still no changes to `AGENTS.md`, `CONTEXT.md`, or rules unless you ask.

The scan simply notices more (skills folders, hooks, workflows) so Cursor can ask better questions — not so something else can rip out what already works.

---

## What it writes

| Output | What it is |
|--------|------------|
| **Scan (chat)** | One `(scan_report ...)` block — Phase 0 only |
| **`.cursorignore`** | Dual-host (Q14): blocks Cursor from loading `AGENTS.md` / `GEMINI.md` / `.agent/` left for Antigravity |
| **`.cursor/rules/*.mdc`** | conduct + safety; optional `project-proof.mdc` (Q3); stack globs if Q15 says persist |
| **`CONTEXT.md`** | Glossary only |
| **`AGENTS.md`** | Only if Q6 replace/merge — default **leave** when other hosts use it |
| **`docs/EMERGENCY-HANDOFF.md`** | `/cursor-landing emergency` only |

Details: [OVERVIEW.md](OVERVIEW.md) · [SKILL.md](SKILL.md)

---

## Crisis?

```text
/cursor-landing emergency
```

Install first ([install.md](references/install.md)). Paste context when asked or `skip`.

---

## Quick start

1. [Install](references/install.md) — flat copy to `~/.cursor/skills/cursor-landing/`
2. Open **your app** repo root in Cursor **Agent**
3. `/cursor-landing` — scan, then grill (often ~5 questions, up to ~10 on messy repos), then writes

Saying **no** to a preset (dual-host Q14, GEMINI split Q16) is fine — the agent should **keep asking** until you choose **replace / merge / leave** for each file it found. If it stops early, say **finish the grill**. It should show a short summary and wait for your **OK** before writing files.

Plain English does not load this skill. New to Cursor Agent? [Quickstart](https://cursor.com/docs/get-started/quickstart) (~10 min).

**Check:** `CONTEXT.md` is glossary-only; run proof from `project-proof.mdc` if present; dual-host → `.cursorignore` + leave AGENTS/GEMINI — start a **new Cursor chat** after init — [troubleshooting.md](references/troubleshooting.md)

---

## License

MIT — [LICENSE](LICENSE)
