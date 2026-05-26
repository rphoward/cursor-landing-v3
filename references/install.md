# Install

**Setup walkthrough:** [README.md](../README.md) · **Mistakes:** [troubleshooting.md](troubleshooting.md) · [Cursor Skills](https://cursor.com/docs/skills)

**Bundle:** `cursor-landing` — copy **flat** to `~/.cursor/skills/cursor-landing/SKILL.md` (not `.../cursor-landing-skill/SKILL.md` inside skills). Invoke **`/cursor-landing`**.

## Steps

1. Copy this folder’s contents to `~/.cursor/skills/cursor-landing/` (`references/`, `assets/`, `SKILL.md`, etc.). No build script — [scripts/README.md](../scripts/README.md).
2. Restart Cursor; confirm **`/cursor-landing`** in Agent (`/` menu).
3. Open a **target** repo root; run `/cursor-landing` or `/cursor-landing emergency`.

## Install from GitHub

**Current (v3):** Clone [cursor-landing-v3](https://github.com/rphoward/cursor-landing-v3), then copy the **repository root** flat into `~/.cursor/skills/cursor-landing/` as above.

**Legacy (v2):** [cursor-landing-v2](https://github.com/rphoward/cursor-landing-v2) — frozen prior release (no indexing-ignore P5); only if you cannot use v3 yet.

Restart Cursor and confirm **`/cursor-landing`** on a target repo.

**Windows (factory maintainer copy):**

```powershell
Copy-Item -Recurse -Force "C:\Project\cursor-landing\github-publish-lisp\cursor-landing-skill\*" "$env:USERPROFILE\.cursor\skills\cursor-landing\"
```

**Project-local (optional):** `.cursor/skills/cursor-landing/` or `.agents/skills/cursor-landing/` in the target repo — open that repo in Cursor.

**Do not:** nest an extra `cursor-landing-skill/` folder under skills, or run init on this source tree unless dogfooding. Remove old `~/.cursor/skills/cursor-landing-lisp/` if present from earlier experimental installs.
