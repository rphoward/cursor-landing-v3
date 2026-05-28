; PROMPT NOTATION — stack/infra signal catalog (Phase 0 load; occasional maintainer edits).
; Emit caps: (stack_signals ...) and (stack_mdc_caps) in SCAN-REPORT-SCHEMA.md — not duplicated here.

(stack_signals_catalog
  (phase_0_inventory_only)
  (forbid append_stack_facts_to AGENTS GEMINI CLAUDE)
  (grill Q15 references/question-bank.md)
  (guardrails
    (no_read .git)
    (no_treat_all_json_as_api_evidence)
    (flag redis:// and DB URLs in risks never_paste_secrets)
    (languages via scan-checklist manifests → detected_languages only no_lang_table_here))

  (tiers
    (A → proposed_mdc_rules e.g. project-proof.mdc deploy-railway.mdc stack_globs)
    (B → scan_report_only obvious manifests)
    (C → omit_v1))

  (signals containerization
    (docker (paths Dockerfile .dockerignore) (tier B))
    (docker_compose (paths docker-compose.yml docker-compose.yaml) (tier A_if multi_service_dev))
    (rkt (hint rkt_run rkt_fetch in_scripts) (tier C))
    (lxc (hint lxc.conf lxc-create in_scripts) (tier C)))

  (signals architecture
    (arch_microservices (hint multiple_package_roots compose_services) (tier A monorepo_note))
    (arch_monolith (hint single_root pom.xml Gemfile one_app_tree) (tier B))
    (arch_serverless (paths serverless.yml template.yaml sst.config.ts amplify/) (tier A))
    (arch_cqrs (hint commands_and_queries_dirs) (tier C))
    (arch_soa (hint wsdl esb_volume) (tier C)))

  (signals ci_vcs
    (ci_github_actions (paths .github/workflows/*) (tier B))
    (ci_gitlab (paths .gitlab-ci.yml) (tier B))
    (ci_jenkins (paths Jenkinsfile jenkinsfile) (tier B))
    (ci_azure_pipelines (paths azure-pipelines.yml azure-pipelines.yaml) (tier B))
    (ci_circleci (paths .circleci/config.yml) (tier B))
    (ci_bitbucket (paths bitbucket-pipelines.yml) (tier B))
    (note duplicate_ci_vendors → tier_A plus risks_or_grill))

  (signals apis
    (api_openapi (paths openapi.yaml openapi.json swagger.yaml swagger.json) (tier A glob_links_file))
    (api_soap_wsdl (paths *.wsdl) (tier B))
    (api_gson_java (hint com.google.gson in pom.xml build.gradle when_java_manifest) (tier C)))

  (signals caching
    (cache_redis (paths redis.conf redis:// in_config flag_secrets) (tier A_if non_obvious))
    (cache_cdn (hint cloudflare cloudfront akamai in_config_filenames) (tier C))
    (cache_server_side (hint Django_CACHES memory-cache dep) (tier B))
    (cache_client_side (hint service_worker Cache-Control frontend) (tier C)))

  (signals testing
    (test_unit (paths tests/unit/ *.test.js *.spec.ts test_*.py jest pytest in_manifest) (tier B))
    (test_integration (paths tests/integration/ *IT.java *.integration.test.js) (tier A_if non_standard_layout))
    (test_functional (paths cypress.config.* playwright.config.* selenium_config) (tier A proof_candidate)))

  (signals frameworks
    (fw_django (paths manage.py settings.py wsgi.py) (tier B))
    (fw_express (hint express in package.json) (tier B))
    (fw_laravel (paths artisan bootstrap/app.php) (tier B))
    (fw_rails (paths bin/rails config/routes.rb) (tier B))
    (note roll_up detected_frameworks no_framework_only_mdc))

  (signals databases
    (db_postgres (hint postgresql:// psycopg2 pg_hba.conf) (tier B))
    (db_mysql (hint mysql:// mysql2 my.cnf) (tier B))
    (db_mariadb (hint mariadb:// mariadb.cnf) (tier B))
    (db_mongo (hint mongodb:// mongoose pymongo) (tier B))
    (db_dynamodb (hint AWS_SDK DynamoDB client) (tier B))
    (db_couchdb (hint port_5984 local.ini) (tier C))
    (db_graph (hint neo4j.conf Cypher config) (tier C))
    (db_oracle (hint tnsnames.ora oracledb cx_Oracle) (tier C))
    (db_rethinkdb (hint rethinkdb import conf) (tier C)))

  (signals deploy
    (deploy_railway (paths railway.toml railway.json) (tier A → deploy-railway.mdc or agent_request))
    (deploy_fly (paths fly.toml) (tier A))
    (deploy_render (paths render.yaml) (tier A))
    (deploy_vercel (paths vercel.json) (tier B))
    (note no_generic_host_prose_in_rules link README_section_if_found))

  (emit_instructions
    (stack_signals (row (id signal_id) (paths ...) (confidence high|medium|low) (note optional)))
    (tier_A → proposed_mdc_rules with source_paths see MDC-RULES-FORMAT.md)
    (verify_commands from CI manifest → grill Q3 → project-proof.mdc not AGENTS)
    (conflicting_proof duplicate_ci → risks or open_questions)))
