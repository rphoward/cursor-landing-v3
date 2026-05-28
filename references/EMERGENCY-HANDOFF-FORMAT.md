# EMERGENCY-HANDOFF.md format

Transient session state — **not** AGENTS or `.mdc`. Written into **target repos** at `docs/EMERGENCY-HANDOFF.md` by `/cursor-landing emergency`. Overwrite each run; delete after you resume.

Sections (say `unknown` rather than invent):

### 1. Branch

Branch name, or `not a git repository`, or `unknown (git error: …)`. Remote ahead/behind if available.

### 2. Git status

`git status --short` summary; or `n/a — not a git repository`; or one-line error + `handoff used user paste / chat only`. Dirty trees OK — summarize.

### 3. Current task

One paragraph; prefer user paste; note if **skip**. Include “done” signal if known.

If scan found **Cline** `memory-bank/`, prefer excerpts from `activeContext.md` / `progress.md` here (not in CONTEXT or rules). See [annexes/cline.md](annexes/cline.md).

If scan found **Codex** `PLANS.md` (repo root or path listed in scan inventory), read when present and excerpt task, blockers, and next steps here (~40 lines max). Say `unknown` if missing. Do **not** copy into CONTEXT, AGENTS, or `.mdc`. See [annexes/codex.md](annexes/codex.md).

### 4. Changed files

Union of `git diff --name-only`, `git diff --cached --name-only`, untracked from status; or user paste only (`source: user paste`). Flag secrets; no contents.

### 5. Last commands

3–8 commands + one-line result each.

### 6. Blockers

Or `none`.

### 7. Resume prompt

Paste-ready; starts “Continue …”; read order, next step, proof if known. Tool-agnostic — say “next coding-agent chat”, not a single IDE. ≤15 lines.

## Git best-effort (agent)

| Case | Branch | Status | Changed files |
|------|--------|--------|---------------|
| `.git` OK | `git branch --show-current` (note detached) | `git status --short` | `git diff --name-only`, `git diff --cached --name-only`, untracked |
| No repo | `not a git repository` | `n/a` | user paste / chat |
| Git error | `unknown (git error: …)` | one-line error | user paste; blocker `git unusable` |

No `git fsck`, reset, or repair unless user asks.

Template: [../assets/EMERGENCY-HANDOFF.template.md](../assets/EMERGENCY-HANDOFF.template.md).
