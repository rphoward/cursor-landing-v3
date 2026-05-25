; PROMPT NOTATION — not compilable Lisp. Shape for Phase 0 (scan_report ...) emit.

(scan_report_schema
  (version 2.2)
  (optional_version 2.0 2.1)

  ; ── REQUIRED TOP-LEVEL KEYS ──
  (required
    schema_version scan_date
    stack layout agent_surfaces_by_host legacy_host_signals
    trim_candidates proposed_mdc_rules conversion_plan
    verify_commands open_questions mcp_inventory)

  (optional proposed_glossary stack_signals)

  (stack_signals
    (caps stack_mdc_caps)
    (row id paths confidence note_optional)
    (tier_A → proposed_mdc_rules_only not AGENTS GEMINI CLAUDE)
    (catalog references/stack-signals.md))

  (stack
    (keys detected_languages detected_frameworks package_managers monorepo)
    (monorepo (value bool) (note string_optional)))

  (layout
    (keys workspace_root source_roots nested_instruction_paths))

  (agent_surfaces_by_host
    (row host_id active_surfaces skills_folders legacy_paths confidence))

  (legacy_host_signals
    (keys inferred_host confidence evidence_signal_ids co_primary_optional))

  (proposed_glossary
    (row term meaning source_paths confidence
          not_confused_with_optional aliases_optional)
    (forbid content_kind architecture run_state proof stack))

  (trim_candidates
    (row file_path type_id reason suggested_action confidence)
    (suggested_action emergency_only exclude_from_context merge_to_agents ask_user)
    (forbid promote_run_state_to_CONTEXT_or_always_on_mdc))

  (proposed_mdc_rules
    (row filename activation alwaysApply globs_optional description_optional
          rationale source_paths external_links_optional status
          extract_from_optional dest_sections_optional merge_preview_optional)
    (activation always glob agent_request)
    (status write merge_existing skip_not_found)
    (extract_from_shape_ref extract_from_shape))

  ; ── EXTRACT_FROM (dual-host merge evidence; canonical shape — MERGE/MDC cite here only) ──
  (extract_from_shape
    (location inside each proposed_mdc_rules rule when merging from AGENTS GEMINI legacy)
    (per_slice
      (path repo_relative_required)
      (section optional_heading_or_anchor)
      (content_kind required))
    (content_kinds
      glossary boundaries conduct safety proof stack workflow run_state leave_native)
    (dest_sections_optional
      conduct safety boundaries proof stack workflow)
    (merge_preview required_when extract_from_present)
    (forbid json_object_literals in_phase_0_chat)
    (example
      (extract_from
        (slice (path AGENTS.md) (section "Behavioral constraints") (content_kind conduct))
        (slice (path GEMINI.md) (content_kind safety)))))

  (stack_mdc_caps
    (stack_signals_rows_max 12)
    (alwaysApply_project_rules_max 1)
    (glob_or_agent_request_stack_rules_max 3)
    (authoritative_with stack_signals proposed_mdc_rules))

  (conversion_plan
    (row step target_file source_paths merge_strategy notes_optional)
    (step1_CONTEXT glossary_sources_only))

  (verify_commands
    (keys candidates recommended_proof source))

  (open_questions (row question_id description))

  (mcp_inventory
    (keys config_paths server_names duplicate_servers secrets_flagged cursor_landing_default))

  ; ── LEGACY HOST SCORING (legacy_host_signals_scoring) ──
  (legacy_host_signals_scoring
    (rule +1_per_signal_per_host highest_wins)
    (confidence high_if_gte_3 medium_if_2 low_if_1)
    (tie → co_primary_and_grill)
    (host_signals
      (cursor .cursor/rules/*.mdc .cursor/mcp.json)
      (claude CLAUDE.md .claude/settings.json .mcp.json)
      (codex .codex/config.toml .codex/hooks.json AGENTS.md)
      (antigravity .agents/rules .agents/workflows)
      (cline memory-bank/projectbrief.md .clinerules)
      (copilot .github/instructions/*.instructions.md)
      (windsurf .windsurf/rules)
      (continue .continue/rules)
      (aider .aider.conf.yml CONVENTIONS.md)))

  ; ── SIGNAL IDS (evidence_signal_ids; annex paths stay authoritative) ──
  (signal_ids
    cursor_rules_mdc cursor_legacy_cursorrules
    claude_instructions codex_agents_md antigravity_agents_dir
    copilot_scoped_instructions cline_memory_bank windsurf_rules
    continue_rules aider_config mcp_multi_file run_state_bloat)

  (emit_rules
    (single_artifact scan_report)
    (forbid json_block markdown_summary_tables repo_writes_phase_0)))
