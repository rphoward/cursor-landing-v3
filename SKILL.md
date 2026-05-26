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

    (trim_candidates_indexing_noise
      (checklist references/scan-checklist.md Indexing_noise_section)
      (shape references/SCAN-REPORT-SCHEMA.md indexing_noise_rows)
      (emit type_id indexing_noise suggested_action append_indexing_ignore)
      (cap 8 rows_per_scan_report)
      (forbid
        paste_assets_cursorindexingignore_baseline_template_into_scan_report
        echo_baseline_ignore_globs_in_chat))

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
    (conditional Q11 memory-bank Q12 duplicate_mcp Q13 reshape_rules Q14 dual_host Q16 portable_overlap_AGENTS_GEMINI)
    (Q14 load references/question-bank.md Q14_plain_wording keep_both_or_cursor_only)
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
      (cursor_only_when Q14_answer_cursor_only
        (order CONTEXT_then cursor_rules CLAUDE_optional)
        (skip cursorignore.dual-host.template)
        (cursorignore_optional
          (only_when scan_or_grill never_show_Agent_paths)
          (append_only_if_exists at_target_repo_root)
          (examples tracked_secrets_user_confirmed)
          (forbid AGENTS_md GEMINI_md dot_agent_paths unless_user_explicit))
        (templates conduct.template.mdc safety.template.mdc unless Q13_combined)
        (still_run indexing_ignore always))
      (dual_host_when Q14_answer_keep_both_or_leave_AGENTS_GEMINI_no_Q16_and_not_Q16_split
        (order CONTEXT_then cursorignore_merge_then cursor_rules_only)
        (cursorignore
          (template assets/cursorignore.dual-host.template)
          (markers
            begin "# >>> cursor-landing:cursorignore:dual-host BEGIN >>>"
            end   "# <<< cursor-landing:cursorignore:dual-host END <<<")
          (if_missing create_from_template_including_markers_and_body)
          (if_exists_begin_marker_present replace_managed_block_from_template do_not_touch_lines_outside_markers)
          (if_exists_no_begin_marker append_managed_block_once_from_template do_not_remove_user_lines)
          (paths_outside_managed_block append_only_if_user_or_prior_init_added))
        (CLAUDE_md_in_cursorignore when Q14_sub_ask_yes)
        (templates conduct-dual-host.template.mdc safety-dual-host.template.mdc)
        (forbid conduct_links_AGENTS_as_cursor_source)
        (remind new_Cursor_chat_after_cursorignore_written)
        (still_run indexing_ignore always))
      (default
        (order CONTEXT AGENTS_if_not_leave cursor_rules CLAUDE_optional)
        (still_run indexing_ignore always)))

    (indexing_ignore
      (always_every_normal_phase_2_not_emergency)
      (target .cursorindexingignore at_target_repo_root)
      (template assets/cursorindexingignore.baseline.template managed_block_replace_or_skip)
      (phase_2_order
        (1 write_baseline_from_template)
        (2 append_indexing_noise_trim_candidates)
        (3 read_target_root_cursorindexingignore_once))
      (step_1_write_baseline
        (markers
          begin "# >>> cursor-landing:cursorindexingignore:baseline BEGIN >>>"
          end   "# <<< cursor-landing:cursorindexingignore:baseline END <<<")
        (if_missing create_from_template_including_markers_and_body)
        (if_exists_begin_marker_present replace_managed_block_from_template do_not_touch_lines_outside_markers)
        (if_exists_no_begin_marker append_managed_block_once_from_template do_not_remove_user_lines))
      (step_2_append_trim_candidates
        (source phase_0_scan_report_trim_candidates_from_chat)
        (filter
          (type_id indexing_noise)
          (suggested_action append_indexing_ignore))
        (cap 8 rows_max)
        (write_mode append_only_to_target .cursorindexingignore)
        (one_path_per_row use file_path from_each_row)
        (skip_if path_already_present_in_file))
      (step_3_read_indexing_ignore_once
        (read .cursorindexingignore at_target_repo_root)
        (purpose best_effort_index_nudge_same_session)
        (optional_one_read_under_single_appended_path only_if_needed_for_sanity_not_tree_scan))
      (forbid echo_baseline_globs_inside_phase_0_scan_report)
      (forbid auto_edit_target_dot_gitignore)
      (forbid repo_wide_glob_or_grep_resync_rituals))

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
    (load references/MERGE-TO-RULES.md)
    (phase_3_closeout_chat
      (authority MERGE-TO-RULES_initializer_closeout)
      (chat_only forbid_new_repo_doc_unless_user_asks)
      (forbid_in_user_chat skill_jargon SDK Merkle trim_candidates indexing_noise scan_report)
      (say_in_chat
        (authority MERGE-TO-RULES_initializer_closeout)
        (deliver plain_english_six_bullets adapt_to_actual_phase_2_writes per MERGE-TO-RULES)
        (gates
          (new_chat_when
            (or wrote_cursorignore_for_dual_host
                indexing_ignore_changed_and_user_cares_about_at_search))
          (re_run_safe_when
            (or wrote_cursorindexingignore wrote_cursorignore_for_dual_host)
            (say_per MERGE-TO-RULES_initializer_closeout bullet_7_plain_english))
          (proof_from Q3_unchanged_wording)
          (forbid skill_jargon_in_user_copy))))
    (link_check)
    (proof_from Q3_or_note_not_run)
    (gates per MERGE-TO-RULES)
    (dual_host_when Q14_or_dual_host_path
      (cursorignore_lists_left_in_place_files)
      (conduct_dual_host_no_AGENTS_as_cursor_source)
      (new_chat_via phase_3_closeout_chat — do not duplicate full script here))
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
