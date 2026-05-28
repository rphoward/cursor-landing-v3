# Adding or removing a scan host (maintainers)

Cursor Landing is a **skill**: [SKILL.md](../SKILL.md) links here for Phase 0 only. Do not paste annex bodies into SKILL.

## Add a host (minimum)

1. **Annex** — Copy shape from [annexes/cline.md](annexes/cline.md): scope, official docs, path tables, durable vs run-state, “Scan Report lines to emit.” Target **under ~120 lines**, scan-only.
2. **Index** — Add a row to [annexes/INDEX.md](annexes/INDEX.md).
3. **Checklist** — Add one bullet under “Agent surfaces” in [scan-checklist.md](scan-checklist.md) linking the annex.

That is enough for `/cursor-landing` Phase 0 to detect the tool in brownfield repos.

## Optional (later P1 slice or dogfood)

| When | Where |
|------|--------|
| Host is common in fixtures | [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) — add `signal_ids` entry when present (link annex; do not paste path tables into SKILL) |
| Phase 0 emit | [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) — document `host_id` in `(scan_report ...)` |
| User-facing name | Target `CONTEXT.md` glossary one row ([CONTEXT-FORMAT.md](CONTEXT-FORMAT.md)) |
| Host scoring / signal ids | [SCAN-REPORT-SCHEMA.md](SCAN-REPORT-SCHEMA.md) — `(legacy_host_signals_scoring)` and `(signal_ids)`; no separate `research/` tree in this bundle — see [../BUNDLE-MANIFEST.md](../BUNDLE-MANIFEST.md) |

## Remove a host

1. Delete `annexes/<host>.md`.
2. Remove INDEX row and checklist bullet.
3. Grep repo for `host_id` / glossary / `signal_ids` entries you added; drop only those optional steps.

## Annex template (sections)

```markdown
# <Tool> — agent surfaces (scan reference)

**Scope:** Inventory only. This skill does **not** run inside <Tool>.

**Official docs:** <links>

Reviewed: **Month YYYY** (verify locally if paths drift)

---

## <Surfaces> (tables)

## Run-state vs durable (if applicable)

## MCP (if applicable)

## Scan Report lines to emit
```

## Not in scope for host adds

- Converter app or automated scanner
- Merging MCP into target `.cursor/mcp.json` by default
- Copying run-state into target `CONTEXT.md`

See [../BUNDLE-MANIFEST.md](../BUNDLE-MANIFEST.md) for install bundle boundaries (BUNDLE-MANIFEST).
