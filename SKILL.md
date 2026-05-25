---
name: cursor-landing
description: >-
  Cursor Agent only. Cursor Landing (pseudo-Lisp body): read-only scan emits one
  (scan_report ...) block; mini grill; CONTEXT glossary + slim AGENTS + rules.
  Emergency: /cursor-landing emergency writes docs/EMERGENCY-HANDOFF.md.
disable-model-invocation: true
---

; PROMPT NOTATION — not compilable Lisp. Execute as agent instructions.

(cursor_landing_skill
  (host_gate
    (require Cursor_Agent)
    (else_stop "This skill requires Cursor Agent; install from ~/.cursor/skills/cursor-landing/."))

  (commands
    (initializer /cursor-landing)
    (emergency "/cursor-landing emergency"))

  (outcomes
    (scan_report chat_only_read_only)
    (CONTEXT_md glossary_only)
    (mdc_primary proof_in_project-proof_md AGENTS_if_Q6_merge)
    (mdc conduct_links_CONTEXT)
    (EMERGENCY_HANDOFF docs_only))

  ; ── PHASE 0 — READ-ONLY SCAN ──
  (phase_0_scan
    (prefer Explore_read_only)
    (else (load references/scan-checklist.md) (load references/stack-signals.md) (load references/annexes/INDEX.md))
    (per_host_paths annexes_and_checklist_authoritative)
    (scoring (load references/SCAN-REPORT-SCHEMA.md legacy_host_signals_scoring signal_ids))
    (forbid edit_target_repo paste_annex_tables_into_chat))

    (emit
      (single_artifact scan_report)
      (format pseudo_lisp)
      (shape_ref references/SCAN-REPORT-SCHEMA.md)
      (example_ref references/examples/scan-report-example.md)
      (forbid json_block markdown_summary_tables repo_writes))

    (conversion_plan_step1
      (CONTEXT glossary_sources_only)
      (forbid architecture run_state proof stack_trees))

    (merge_heavy
      (when AGENTS_md GEMINI_md dot_agent legacy_cursorrules)
      (populate proposed_mdc_rules extract_from merge_preview)
      (load references/MERGE-TO-RULES.md)
      (phase0_propose phase2_write)
      (forbid paste_whole_foreign_agent_files_into_always_on_mdc)))

  ; ── PHASE 0e — EMERGENCY ──
  (phase_0e_emergency
    (trigger "/cursor-landing emergency")
    (steps
      (0 scan_if_needed_for_agent_context)
      (1 ask_once "Paste prior-tool task/blockers/commands or skip")
      (2 git_best_effort references/EMERGENCY-HANDOFF-FORMAT.md)
      (3 run_state_excerpts
         (cline memory-bank → handoff_section_3 activeContext progress)
         (codex PLANS_md → handoff_section_3 ~40_lines)
         (missing → unknown)
         (format_ref references/EMERGENCY-HANDOFF-FORMAT.md section_3))
      (4 write docs/EMERGENCY-HANDOFF.md template assets/EMERGENCY-HANDOFF.template.md)
      (5 forbid update AGENTS CONTEXT mdc unless_user_asks)
      (6 echo Resume_prompt_in_chat))
    (forbid fail_handoff_on_broken_git))

  ; ── PHASE 1 — MINI GRILL ──
  (phase_1_grill
    (initializer_only)
    (load references/question-bank.md)
    (one_question_at_a_time prefer_scan)
    (ambiguity_only no_external_grill_skills)
    (Q4_when proposed_glossary confirm_or_correct_rows)
    (conditional Q11 memory-bank Q12 duplicate_mcp Q13 reshape_rules Q16 portable_overlap_AGENTS_GEMINI)
    (declining_Q14_or_Q16 does_not_end_grill still_complete_Q6_per_artifact)
    (no_repo_writes_until Q6_done glossary_in_chat_only)
    (before_phase2_merge_or_Q16_split
      (show extraction_table content_kind_per_row per MERGE-TO-RULES extract_from_shape))
    (load references/MERGE-TO-RULES.md)
    (closeout_gate blocking authority MERGE-TO-RULES)
    (optional_save AGENT-INIT-SCAN.md at_phase1_start_if_user_asks))

  ; ── PHASE 2 — WRITE ──
  (phase_2_write
    (initializer_only)
    (phase_2_route
      (Q16_split_when user_yes_Q16
        (order split_AGENTS_GEMINI_then CONTEXT_then cursor_rules))
      (dual_host_when Q14_or_leave_AGENTS_GEMINI_no_Q16
        (order CONTEXT_then cursorignore_merge_then cursor_rules_only)
        (cursorignore template assets/cursorignore.dual-host.template append_only_if_exists)
        (templates conduct-dual-host.template.mdc safety-dual-host.template.mdc)
        (forbid conduct_links_AGENTS_as_cursor_source))
      (default
        (order CONTEXT AGENTS_if_not_leave cursor_rules CLAUDE_optional)))

    (CONTEXT (load references/CONTEXT-FORMAT.md) (cap_terms 12..20))
    (AGENTS (load references/AGENTS-FORMAT.md) (skip_if Q6_leave))
    (GEMINI (skip_if Q6_leave))
    (rules
      (load references/MDC-RULES-FORMAT.md references/stack-signals.md)
      (minimum
        (dual_host conduct-dual-host.template.mdc + safety-dual-host.template.mdc)
        (default conduct.template.mdc + safety.template.mdc)
        unless Q13_combined)
      (stack_infra
        (persist_via proposed_mdc_rules_only)
        (Q15_scan_only → skip Tier_A stack mdc writes keep conduct+safety)
        (default_Q6_leave AGENTS GEMINI CLAUDE_when_present)
        (project-proof.mdc_from Q3 Q5 template assets/project-proof.template.mdc optional_alwaysApply)
        (forbid stack_inventory_in_AGENTS))
      (additional_rules only_from scan_report_proposed_mdc_rules_with_evidence)
      (forbid mega_always_on unless Q13_combined))

    (Q6 per_artifact replace merge leave — load references/MERGE-TO-RULES.md)
    (CLAUDE_bridge
      (load references/annexes/claude.md)
      (top_imports @AGENTS.md @CONTEXT.md)
      (windows @_imports_not_symlinks)
      (template assets/CLAUDE.bridge.template.md))

    (mcp (forbid merge unless_user_asks) (Q12_when_duplicates) (load references/annexes/mcp.md))
    (cline (forbid promote memory-bank_to_CONTEXT) (Q11) (load references/annexes/cline.md))
    (Q13_when create_or_reshape_rules))

  ; ── PHASE 3 — VERIFY ──
  (phase_3_verify
    (link_check)
    (proof_from Q3_or_note_not_run)
    (gates per MERGE-TO-RULES)
    (dual_host_when Q14_or_dual_host_path
      (cursorignore_lists_left_in_place_files)
      (conduct_dual_host_no_AGENTS_as_cursor_source)
      (note_new_chat_after_cursorignore_written))
    (emergency all_sections_plus_resume_prompt))

  (edge_cases
    (load references/troubleshooting.md)
    (no_invoke → reply_exact_command_only)
    (empty_scan → confirm_project_root)
    (create docs cursor_rules_if_missing)
    (no_bulk_delete_without_choice)
    (cursor-landing_SKILL_at_workspace_root → warn_stop_offer))

  (on_demand_only
    (human_setup README.md)
    (format_refs CONTEXT AGENTS MDC MERGE EMERGENCY))
