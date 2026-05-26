# Cursor — agent surfaces (scan reference)

**v3 dev bundle:** invoke **`/cursor-landing`** on the target repo.

**Scope:** Inventory only. This cursor-landing skill **executes** in Cursor Agent; see [../cursor-notes.md](../cursor-notes.md) for invoke/runtime behavior.

**Official docs:** [Rules](https://cursor.com/docs/context/rules), [Skills](https://cursor.com/docs/skills), [Subagents](https://cursor.com/docs/agent/subagents)

Reviewed: **May 2026**

---

## Instructions (persistent context)

| Surface | Typical path | Notes |
|---------|--------------|-------|
| Project rules | `.cursor/rules/*.md`, `.cursor/rules/*.mdc` | Frontmatter: `alwaysApply`, `description`, `globs` / `paths` — **prefer `.mdc` for new rules** |
| Legacy root rules | `.cursorrules` | Still found in brownfield repos; **inventory and migrate content** to `.cursor/rules/*.mdc` on merge — do not delete without Q6 |
| Root instructions | `AGENTS.md` | Plain markdown alternative to rules; supported at root and in subdirs — **Cursor auto-loads** unless excluded |
| Nested instructions | `**/AGENTS.md` | More specific dirs override/generalize parent guidance |
| Cursor ignore file | `.cursorignore` | Repo root; excludes paths from Agent context, search, and `@` — **not** read by Antigravity |
| User rules | Cursor Settings → Rules | Global; not in repo scan |
| Team rules | Cursor dashboard | Enterprise/team; not in repo scan |

Nested `AGENTS.md`: instructions combine from root toward the working directory; nearer files weigh more for that area.

---

## Skills (Agent Skills standard)

| Scope | Path |
|-------|------|
| Project | `.agents/skills/`, `.cursor/skills/` (also nested subdirs in monorepos) |
| User | `~/.agents/skills/`, `~/.cursor/skills/` |

Each skill: folder with **`SKILL.md`** (YAML frontmatter: `name`, `description`; optional `paths`, `disable-model-invocation`).

**Compatibility loads (also scanned):** `.claude/skills/`, `.codex/skills/`, `~/.claude/skills/`, `~/.codex/skills/` — list in Scan Report under the **source tool**, not as Cursor-native config.

---

## Subagents

| Path | Notes |
|------|-------|
| `.cursor/agents/` | Custom agent definitions if present |

Phase 0 of `/cursor-landing` prefers built-in **Explore** (read-only), not a custom scanner agent.

---

## MCP

Project MCP often at `.cursor/mcp.json`. Cross-tool patterns: [mcp.md](mcp.md).

---

## Phase 2 write (cursor-landing init)

When `/cursor-landing` Phase 2 runs on the target repo:

- Prefer **`.cursor/rules/*.mdc`** over legacy `.cursorrules` for new always-on guidance.
- **Minimum (Cursor-only / merge AGENTS):** `conduct.mdc` + thin `safety.mdc` from default templates (`alwaysApply: true`; link `CONTEXT.md` and `AGENTS.md`) unless grill **Q13** chose one combined conduct rule.
- **Dual-host (Q14):** write **`.cursorignore`** from [cursorignore.dual-host.template](../../assets/cursorignore.dual-host.template); use **conduct-dual-host** + **safety-dual-host** templates; proof in **`project-proof.mdc`** — do not treat `AGENTS.md` as Cursor instruction source.
- **Extra rules:** only from Scan Report `proposed_mdc_rules` (`glob` or `agent_request`) — do not invent paths.
- **Legacy `.cursorrules`:** inventory in Phase 0; on Q6 **merge**, port substantive content into scoped `.mdc` files rather than leaving duplicate root rules.

Full write order and anti-hallucination rules: [MDC-RULES-FORMAT.md](../MDC-RULES-FORMAT.md) and [SKILL.md](../../SKILL.md) § Phase 2.

---

## Scan Report lines to emit

- Rules count and any `alwaysApply` vs glob-only split
- `proposed_mdc_rules` entries (filename, activation, evidence paths) when planning Phase 2 writes
- Skills dirs found (note `.cursor/` vs `.agents/` vs compatibility paths)
- Root vs nested `AGENTS.md` paths
- Bloated or duplicate guidance vs `CLAUDE.md` / other hosts

---

## Forensics reference

Scoring signals (path tables above stay authoritative): [SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) — `cursor_rules_mdc`, `cursor_legacy_cursorrules`, `mcp_multi_file`, `run_state_bloat`.
