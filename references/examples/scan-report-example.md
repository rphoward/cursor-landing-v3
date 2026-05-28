; PROMPT NOTATION — example Phase 0 emit (fictional-app). Do not write into target repos.

(scan_report
  (schema_version 2.2)
  (scan_date 2026-05-24)
  (stack_signals
    (row (id docker_compose) (paths fictional-app/docker-compose.yml) (confidence medium) (note "dev Postgres service"))
    (row (id ci_github_actions) (paths fictional-app/.github/workflows/ci.yml) (confidence high))
    (row (id test_unit) (paths fictional-app/package.json) (confidence high)))
  (stack
    (detected_languages TypeScript)
    (detected_frameworks Express)
    (package_managers pnpm)
    (monorepo (value false) (note "single package at fictional-app root")))
  (layout
    (workspace_root fictional-app/)
    (source_roots fictional-app/src/)
    (nested_instruction_paths))
  (agent_surfaces_by_host
    (row (host_id claude) (active_surfaces CLAUDE.md .mcp.json) (skills_folders) (legacy_paths) (confidence high))
    (row (host_id cursor) (active_surfaces .cursorrules) (legacy_paths .cursorrules) (confidence medium))
    (row (host_id gemini) (active_surfaces GEMINI.md .agent/) (confidence high)))
  (legacy_host_signals
    (inferred_host claude)
    (confidence high)
    (evidence_signal_ids claude_instructions mcp_multi_file)
    (co_primary cursor))
  (proposed_glossary
    (row
      (term "Knowledge base")
      (meaning "Curated Q&A corpus indexed for retrieval in the fictional app")
      (source_paths fictional-app/CLAUDE.md)
      (confidence high)
      (not_confused_with "Vector store (infra layer)")
      (aliases KB))
    (row
      (term Widget)
      (meaning "Configurable domain object exposed in the public API")
      (source_paths fictional-app/CLAUDE.md fictional-app/README.md)
      (confidence medium))
    (row
      (term "Pipeline run")
      (meaning "Single end-to-end ingest job for one source document")
      (source_paths fictional-app/AGENTS.md)
      (confidence low)
      (not_confused_with "CI workflow run")))
  (trim_candidates
    (row
      (file_path fictional-app/node_modules/)
      (type_id indexing_noise)
      (reason "installed dependencies — not product source")
      (suggested_action append_indexing_ignore)
      (confidence high))
    (row
      (file_path fictional-app/dist/)
      (type_id indexing_noise)
      (reason "build output directory")
      (suggested_action append_indexing_ignore)
      (confidence high))
    (row
      (file_path fictional-app/memory-bank/progress.md)
      (type_id run_state)
      (reason "ephemeral task log from prior agent session")
      (suggested_action emergency_only)
      (confidence high)))
  (proposed_mdc_rules
    (rule
      (filename conduct.mdc)
      (activation always)
      (alwaysApply true)
      (rationale "Sparse repo; no existing .cursor/rules — default conduct from scan")
      (source_paths fictional-app/CLAUDE.md)
      (status write))
    (rule
      (filename safety.mdc)
      (activation always)
      (alwaysApply true)
      (rationale "Minimum always-on safety per MDC-RULES-FORMAT default set")
      (source_paths assets/safety.template.mdc)
      (status write))
    (rule
      (filename project-proof.mdc)
      (activation always)
      (alwaysApply true)
      (rationale "Q3/Q5: proof + workspace; AGENTS.md left for other hosts")
      (source_paths fictional-app/package.json fictional-app/.github/workflows/ci.yml assets/project-proof.template.mdc)
      (merge_preview "Proof: pnpm run lint && pnpm test; open fictional-app/ in Cursor")
      (status write))
    (rule
      (filename stack-typescript.mdc)
      (activation glob)
      (alwaysApply false)
      (globs src/**/*.ts src/**/*.tsx)
      (rationale "Dual-host example: extract stack guardrails without editing AGENTS.md")
      (source_paths fictional-app/AGENTS.md)
      (extract_from
        (slice (path fictional-app/AGENTS.md) (section "Project Map") (content_kind stack)))
      (dest_sections stack)
      (merge_preview "Port src/ layout bullets into glob rule; link AGENTS.md for Antigravity")
      (status write)))
  (conversion_plan
    (step 1
      (target CONTEXT.md)
      (source_paths fictional-app/CLAUDE.md fictional-app/README.md fictional-app/AGENTS.md)
      (merge_strategy merge)
      (notes "Glossary sources only — map from proposed_glossary"))
    (step 2
      (target AGENTS.md)
      (merge_strategy leave)
      (notes "mdc-only stack: proof in project-proof.mdc; Q6 leave AGENTS for Antigravity"))
    (step 3
      (target .cursor/rules/)
      (source_paths fictional-app/AGENTS.md fictional-app/GEMINI.md)
      (merge_strategy extract_to_mdc)
      (notes "Dual-host: leave AGENTS.md and GEMINI.md; proposed_mdc_rules carry extract_from")))
  (verify_commands
    (candidates "pnpm test" "pnpm run lint")
    (recommended_proof "pnpm run lint && pnpm test")
    (source "fictional-app/package.json scripts"))
  (open_questions
    (q Q12 "Duplicate MCP server name in .cursor/mcp.json and .mcp.json — inventory only or merge?"))
  (mcp_inventory
    (config_paths .cursor/mcp.json .mcp.json)
    (server_names example-db)
    (duplicate_servers example-db)
    (secrets_flagged false)
    (cursor_landing_default inventory_only)))
