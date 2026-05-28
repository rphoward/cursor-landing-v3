# Amazon Kiro — agent surfaces (scan reference)

**Scope:** Inventory only. This skill does **not** run inside Kiro.

**Official / primary sources:** [Amazon Kiro introduction (AWS Builder Center)](https://builder.aws.com/content/3CuKNklw8cDhXjreLYoznoj6dyx/amazon-kiro-use-cases-and-introduction), [Harness engineering with Kiro](https://builder.aws.com/content/3DlOO7A9RFAazBbwbNl2iV8WHr9/harness-engineering-with-kiro-spec-driven-development-for-the-multi-agent-era)

**Not confused with:** **Amazon Q** (AWS-integrated assistant). **Harness.io** (CI/CD platform) — Kiro may use a Harness MCP server; inventory the MCP name only, not “agent harness” engineering jargon.

Reviewed: **May 2026** — community path analysis exists; **verify locally** on installed Kiro version.

---

## Specs (spec-driven layout)

| Surface | Path | Cursor Landing |
|---------|------|----------------|
| Requirements | `.kiro/specs/**/requirements.md` | Durable intent; EARS-style criteria — glossary/terms only if stable |
| Design | `.kiro/specs/**/design.md` | Architecture notes → **AGENTS** / scoped rules, not CONTEXT dump |
| Tasks | `.kiro/specs/**/tasks.md` | **Run-state** — active execution sequence; handoff or trim |

---

## Steering and shared instructions

| Surface | Path | Notes |
|---------|------|-------|
| Steering | `.kiro/steering/*.md` | Long-lived constraints (always / auto / manual load modes) |
| Root agent file | `AGENTS.md` | Kiro may load as steering — scan overlap with Cursor conversion target |

---

## Hooks and MCP

| Surface | Path | Notes |
|---------|------|-------|
| Agent hooks | `.hooks/` | Natural-language file/git event hooks — flag; do not edit on init |
| MCP | `.kiro/settings/mcp.json` | Inventory server **names** only; flag secrets |

**Home (mention only):** `~/.aws/sso/cache/` — do not read without permission.

---

## Scan Report lines to emit

- `.kiro/specs/` present; split **requirements/design** (durable) vs **tasks.md** (run-state)
- `.kiro/steering/` files and load mode hints if visible in metadata
- `.hooks/` presence
- `.kiro/settings/mcp.json` — names + duplicate overlap with `.cursor/mcp.json` / `.mcp.json`
- Root `AGENTS.md` when Kiro + Cursor both signaled

---

## Forensics reference

Scoring signals (path tables above stay authoritative): [SCAN-REPORT-SCHEMA.md](../SCAN-REPORT-SCHEMA.md) — Amazon Q is not Kiro; use this annex for Kiro paths.
