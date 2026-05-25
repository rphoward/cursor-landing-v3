; PROMPT NOTATION — grill questions. Policy: references/MERGE-TO-RULES.md

(grill
  (default_count 5)
  (cap 10)
  (Q15_counts_toward_cap_when_triggered)
  (forbid external_grill_skills)
  (no_repo_writes_until Q6_answered_for_each_found_artifact)
  (closeout_gate authority MERGE-TO-RULES)

  (Q1 (ask "Repo purpose in one sentence?") (feeds CONTEXT optional conduct_mdc))
  (Q2 (ask "What must agents never touch?") (feeds safety_mdc_or_AGENTS_if_Q6_merge))
  (Q3 (ask "Exact verify command?") (feeds project-proof.mdc alwaysApply))
  (Q4 (ask "Confirm or correct scan proposed_glossary rows?")
      (when proposed_glossary_present)
      (feeds CONTEXT))
  (Q5 (ask "Monorepo entry / app root?") (feeds project-proof.mdc_or_project.mdc))
  (Q6 (per_artifact CONTEXT AGENTS GEMINI cursor_rules CLAUDE)
      (choices replace merge leave)
      (feeds MERGE-TO-RULES)
      (required_when scan_found_any_of_above))
  (Q7 (ask "Runtime/deploy constraints?") (feeds deploy_or_stack_glob_mdc))
  (Q8 (ask "Biggest pain with current agent setup?") (feeds trim_priority))
  (Q9 (ask "Language/runtime pins?") (feeds project-proof.mdc_or_stack_glob_mdc not AGENTS_when_Q6_leave))
  (Q10 (ask "Compliance / license constraints?") (feeds safety_mdc not AGENTS_when_Q6_leave))
  (Q11 (when memory-bank_detected)
       (ask "Archive activeContext/progress to emergency handoff only, or skip?")
       (feeds run_state_routing))
  (Q12 (when duplicate_mcp_configs)
       (ask "MCP inventory only, or merge into .cursor/mcp.json with env vars?")
       (feeds mcp_policy))
  (Q13 (when phase2_creates_or_reshapes_cursor_rules)
       (ask "Several small always-on conduct+safety, or one combined conduct?")
       (default several_small)
       (feeds MDC-RULES-FORMAT))
  (Q14 (when dual_host AGENTS_and_GEMINI_or_agent)
       (ask "Leave AGENTS+GEMINI; thin CONTEXT; extract Cursor guardrails to .mdc?")
       (feeds dual_host_preset)
       (declining_Q14 does_not_end_grill))
  (Q16 (when root_AGENTS_and_GEMINI portable_overlap_not_mere_coexistence)
       (ask "Split portable rules GEMINI→AGENTS; leave GEMINI Gemini-only?")
       (requires Q6_merge_on_both_AGENTS_and_GEMINI)
       (feeds MERGE-TO-RULES Q16 content_kind_routing)
       (extraction_table_required before_writes))
  (Q15 (when stack_signals_tier_A_or_verify_commands_or_deploy_file)
       (ask "Persist scan proof/deploy/monorepo to .cursor/rules (.mdc), or scan-only (no stack .mdc writes)?")
       (choices persist_mdc scan_only)
       (default persist_mdc_when_user_completes_phase2)
       (feeds proposed_mdc_rules_filter)
       (forbid promote_stack_to_AGENTS_GEMINI_CLAUDE))

  (before_phase2_merge_or_extract
    (show extraction_table source_paths → target_mdc_in_chat)))

; ── WHEN TO ASK (conditional; see SKILL phase_1_grill) ──
; Q11–Q12: only when scan found memory-bank or duplicate MCP configs.
; Q13: when Phase 2 will create or reshape .cursor/rules/ (empty dir, Q6 replace/merge on rules).
; Q14: when GEMINI.md or .agent/ + AGENTS.md (dual-host preset before merge is “too hard”). No to Q14 → still Q6 per file.
; Q16: when portable overlap in root AGENTS+GEMINI. Requires merge on BOTH; see MERGE § Q16 post-split checklist.
; Q15: when stack_signals Tier A, verify_commands, or deploy files in scan — persist .mdc vs scan_only.
; Q6: per artifact when scan found CONTEXT, AGENTS, GEMINI, .cursor/rules, CLAUDE — no writes until answered.
; Scan-assisted: high-confidence proposed_glossary for Q1/Q5 → confirm or gap-fill only, do not re-interview.
; Default if Q13 skipped: conduct + safety templates; stack globs only from scan proposed_mdc_rules + Q15 persist.
