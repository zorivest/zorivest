# Complex Ops Index — Operational Logic ↔ Validation Catalog

<!--
  MAINTAINER NOTE: This file is the validation-ready SSOT for complex *non-math*
  operational logic in Zorivest. Advanced models use it to review soundness of
  rules, state machines, sandboxes, AI orchestration, and destructive gates.

  Companion indexes (do NOT overload them):
    - docs/reference/math-features-index.md — formula / metric math
    - docs/reference/INDEX.md              — theory slug ↔ GUI deep-links
    - docs/build-plan/output-index.md      — computed outputs ↔ surfaces
    - docs/build-plan/gui-actions-index.md — GUI actions ↔ REST/MCP

  TO ADD AN OP:
    1. Implement under packages/core (or documented UI-main path for Electron-only ops)
    2. Append a Quick Reference row
    3. Append a matching object in the VERIFY:complex-ops block
    4. Link related_math / theory_doc / unit_tests when they exist

  ORPHAN DETECTION:
    - Screenshot / GUI surface for AI Review, Tax Audit, Scheduling Policies,
      Email Templates with no row here
    - Destructive MCP action without a confirmation/guard row
    - Pipeline step type without a row

  validation_status values:
    unvalidated | unit_covered | oracle_ready | model_reviewed | drift_suspected
  validated_by stays empty until a real validation pass.
-->

> Part of Reference Docs · Companion: Math Features Index · Output Index

Canonical registry of **complex operational logic** — AI orchestration, tax data-integrity rules, scheduling/policies/templates, import/sync, security gates, and UI-main ops — with **code locations** so advanced models can review that the logic is solid and sound.

This index deliberately **excludes pure formula math** (see math-features-index.md). When an op *uses* math, link via `related_math`.

---

## Legend

| Column / Field | Meaning |
|----------------|---------|
| **Category** | `ai` · `tax_ops` · `scheduling` · `import` · `data_integrity` · `workflow` · `security` · `ui_ops` |
| **Status** | `implemented` · `scaffold` · `deferred` · `theory_only` |
| **Validation** | `unvalidated` · `unit_covered` · `oracle_ready` · `model_reviewed` · `drift_suspected` |
| **Review focus** | What a model must verify (invariants, false-positive risk, security bounds) |

---

## Quick Reference

| # | ID | Name | Category | Module | Symbol | Validation |
|---|----|------|----------|--------|--------|------------|
| 1 | `ai-trade-review` | Multi-persona AI trade review | ai | `services/ai_review_client.py` | `OpenAICompatibleClient.review` | drift_suspected |
| 2 | `ai-persona-prompts` | Persona system-prompt contracts | ai | `services/ai_review_client.py` | `_build_system_prompt` | drift_suspected |
| 3 | `ai-provider-resolution` | Active AI provider selection | ai | `services/ai_provider_service.py` | `AIProviderService` | drift_suspected |
| 4 | `ai-pipeline-generate` | Pipeline `ai_generate` step | ai | `pipeline_steps/ai_generate_step.py` | `AIGenerateStep.execute` | drift_suspected |
| 5 | `ai-mistake-heuristics` | Mistake classification priority rules | ai | `domain/analytics/classify.py` | `classify_trade_mistake` | drift_suspected |
| 6 | `tax-transaction-audit` | Tax transaction audit orchestrator | tax_ops | `services/tax_service.py` | `TaxService.run_audit` | drift_suspected |
| 7 | `tax-audit-duplicate-lots` | Duplicate lot detection | tax_ops | `services/tax_service.py` | `run_audit` (duplicate key) | drift_suspected |
| 8 | `tax-audit-orphaned-lots` | Orphaned closed-lot rule | tax_ops | `services/tax_service.py` | `run_audit` | drift_suspected |
| 9 | `tax-audit-missing-basis` | Missing/zero cost-basis rule | tax_ops | `services/tax_service.py` | `run_audit` | drift_suspected |
| 10 | `tax-audit-invalid-proceeds` | Invalid proceeds on closed lots | tax_ops | `services/tax_service.py` | `run_audit` | drift_suspected |
| 11 | `tax-wash-chain-fsm` | Wash-sale chain state machine | tax_ops | `domain/tax/wash_sale_chain_manager.py` | `WashSaleChainManager` | drift_suspected |
| 12 | `tax-wash-conflict-warnings` | Wash conflict advisories | tax_ops | `domain/tax/wash_sale_warnings.py` | `check_conflicts` | drift_suspected |
| 13 | `tax-lot-reassignment` | T+1 lot-method reassignment | tax_ops | `domain/tax/lot_reassignment.py` | `can_reassign_lots` | drift_suspected |
| 14 | `tax-lot-sync-pipeline` | Trade→lot sync materialization | tax_ops | `services/tax_service.py` | `sync_lots` | drift_suspected |
| 15 | `tax-harvest-scanner` | Loss-harvest candidate scanner | tax_ops | `domain/tax/harvest_scanner.py` | `scan_harvest_candidates` | drift_suspected |
| 16 | `tax-deferred-loss-report` | Deferred/trapped wash losses | tax_ops | `services/tax_service.py` | `deferred_loss_report` | drift_suspected |
| 17 | `tax-alpha-counterfactual` | Tax alpha (actual vs FIFO) | tax_ops | `services/tax_service.py` | `tax_alpha_report` | drift_suspected |
| 18 | `tax-option-assignment-basis` | Option assignment basis adjust | tax_ops | `domain/tax/option_pairing.py` | `adjust_basis_for_assignment` | drift_suspected |
| 19 | `tax-lot-preview` | Lot selection preview | tax_ops | `domain/tax/lot_matcher.py` | `preview_lot_selection` | drift_suspected |
| 20 | `sched-policy-document` | Pipeline policy document model | scheduling | `domain/pipeline.py` | `PolicyDocument` | drift_suspected |
| 21 | `sched-policy-validator` | Policy validate + secret/SQL/AI caps | scheduling | `domain/policy_validator.py` | `validate_policy` | drift_suspected |
| 22 | `sched-policy-lifecycle` | Create / approve / run / schedule | scheduling | `services/scheduling_service.py` | `SchedulingService` | drift_suspected |
| 23 | `sched-cron-binding` | Cron → APScheduler binding | scheduling | `services/scheduler_service.py` | `SchedulerService` | drift_suspected |
| 24 | `sched-pipeline-runner` | Pipeline execution engine | scheduling | `services/pipeline_runner.py` | `PipelineRunner` | drift_suspected |
| 25 | `sched-policy-emulator` | Dry-run policy emulator | scheduling | `services/policy_emulator.py` | `PolicyEmulator` | drift_suspected |
| 26 | `sched-pipeline-guardrails` | Pipeline rate / abuse guardrails | scheduling | `services/pipeline_guardrails.py` | `PipelineGuardrails` | drift_suspected |
| 27 | `sched-email-templates` | Email template CRUD + preview | scheduling | email template repo + routes | template CRUD | drift_suspected |
| 28 | `sched-secure-jinja` | Hardened Jinja sandbox | scheduling | `services/secure_jinja.py` | `HardenedSandbox` | drift_suspected |
| 29 | `sched-sql-sandbox` | Read-only SQL query sandbox | scheduling | `services/sql_sandbox.py` | `SqlSandbox` | drift_suspected |
| 30 | `sched-compose-render-send` | Compose → render → email send | scheduling | `pipeline_steps/{compose,render,send}_step.py` | step execute | drift_suspected |
| 31 | `sched-web-search-step` | Web search context for AI reports | scheduling | `pipeline_steps/web_search_step.py` | `WebSearchStep` | drift_suspected |
| 32 | `import-broker-csv` | Broker CSV detect + import | import | `services/import_service.py` | `ImportService.import_file` | drift_suspected |
| 33 | `import-bank-statements` | Bank statement import | import | `services/bank_import_service.py` | `BankImportService` | drift_suspected |
| 34 | `import-broker-sync` | Live broker sync ingest | import | `services/broker_sync_service.py` | `BrokerSyncService` | drift_suspected |
| 35 | `sec-confirmation-tokens` | Destructive confirmation tokens | security | `services/auth_service.py` | `create_confirmation_token` | drift_suspected |
| 36 | `sec-mcp-unlock-state` | MCP write-guard unlock FSM | security | unlock state machine | `UnlockStateMachine` | drift_suspected |
| 37 | `ui-ops-db-bootstrap` | DB profile bootstrap / switch | ui_ops | `database_bootstrap_cli.py` + UI worker | bootstrap/rebind | drift_suspected |
| 38 | `workflow-account-review` | Account review checklist | workflow | `domain/account_review.py` | `prepare_review_checklist` | validated |
| 39 | `workflow-trade-report` | Post-trade report lifecycle | workflow | report service / MCP `zorivest_report` | report create/track | drift_suspected |
| 40 | `data-round-trip-recompute` | Round-trip recompute on edits | data_integrity | `services/round_trip_recompute.py` | recompute | drift_suspected |

---

## Implementation Map

Paths below are relative to `packages/core/src/zorivest_core/` unless noted.

### AI orchestration

| ID | Logic summary | Review focus | Surfaces |
|----|---------------|--------------|----------|
| `ai-trade-review` | Runs risk / technical / behavioral personas; parses JSON verdicts; budget-cap orchestration | Prompt injection resistance; budget accounting; partial-failure behavior; schema of returned personas | `POST /api/v1/analytics/ai-review`; MCP `ai_review`; GUI Trades → AI Review |
| `ai-persona-prompts` | Default persona system prompts + custom override | Prompt contracts stay role-separated; custom override cannot escalate provider settings | GUI prompt editor; `GET /ai-providers/default-prompt` |
| `ai-provider-resolution` | Lazy resolve active BYOK/OpenRouter/local provider | Secrets never logged; Settings changes apply without restart; disabled provider fails closed | Settings → AI; MCP `ai_providers_*` |
| `ai-pipeline-generate` | Policy `ai_generate` step with context injection + citation rules | Cap enforcement; citation grounding; schema_version 2 gate; no silent model swap | Scheduling policies |
| `ai-mistake-heuristics` | Priority heuristic classify (revenge, oversized, FOMO, …) | Threshold order; false positives; related math `trade-mistake-classify` | Analytics → Costs & Mistakes; MCP `mistake_summary` |

### Tax operations & integrity

| ID | Logic summary | Review focus | Surfaces |
|----|---------------|--------------|----------|
| `tax-transaction-audit` | Orchestrates audit findings into `AuditReport` | Severity taxonomy; filter by account/year; report completeness | Tax → Audit; `POST /api/v1/tax/audit` |
| `tax-audit-duplicate-lots` | Key = `(account_id, ticker, open_date, quantity)` → warning | Legitimate same-day split buys false-positive risk; severity = warning not error | Tax → Audit |
| `tax-audit-orphaned-lots` | Closed lot with empty `linked_trade_ids` | Distinguish intentional manual lots vs broken sync | Tax → Audit |
| `tax-audit-missing-basis` | `cost_basis is None or 0` → error | Zero-basis intentional gifts/spinoffs; severity = error | Tax → Audit |
| `tax-audit-invalid-proceeds` | Closed lot with invalid/missing proceeds | Boundary vs open lots | Tax → Audit |
| `tax-wash-chain-fsm` | DISALLOWED → ABSORBED → RELEASED/DESTROYED; basis add-back + holding-period tack | State transitions irreversible where required; no double-absorb; related math wash rows | Tax → Wash Sales |
| `tax-wash-conflict-warnings` | Advisory 30-day conflict categories (rebalance/spousal/DRIP) | Advisory-only (must not silently disallow); window math | Tax wash UI |
| `tax-lot-reassignment` | Settlement-window eligibility then re-select lots | T+1 gate; cannot reassign outside window; related math `tax-lot-method` | Tax → Lots (Reassign) |
| `tax-lot-sync-pipeline` | Two-pass BOT create + SLD close; Pass 2 consumes open lots **FIFO by quantity**, splitting a `-R` remainder on a partial fill; residue is reported as an `UnmatchedSell`, never dropped | Idempotency; abort conditions; no silent overwrite of adjusted basis; every sell either closes quantity or is counted unmatched | `POST /tax/sync-lots`; Process Tax Lots |
| `tax-harvest-scanner` | Scan open lots where MV < basis; threshold + wash-risk exclude. The route calls `scan_harvest_candidates` (not the trapped-loss report) and maps quote failures onto an explicit M1–M5 matrix — partial failure → 200 + `skipped_tickers`, total failure or unconfigured provider → 503 | Threshold config; wash-risk exclusion correctness; a failed scan must never surface as a 200 with a zero total; related math harvesting formula | Tax → Harvesting |
| `tax-deferred-loss-report` | Aggregate ABSORBED/DESTROYED vs reported P&L | Chain status filters; no double-count | Tax reports API |
| `tax-alpha-counterfactual` | Actual tax vs FIFO counterfactual + harvest savings | Counterfactual must use independent lot selection; related math lot method | `GET /tax/alpha` |
| `tax-option-assignment-basis` | Four IRS Pub 550 premium adjustment paths; `pair_option_assignment` validates the linkage **before** touching `cost_basis`, so a rejected pairing leaves the lot byte-identical | Call/put × exercise/assignment matrix; OCC symbol parse; guard-before-mutate ordering (no read-modify-write ahead of the `linked_trade_ids` check) | Tax lot / import |
| `tax-lot-preview` | Preview which lots close under selected method | Must match `select_lots_for_closing` behavior | Tax → Lots |

### Scheduling, policies, templates

| ID | Logic summary | Review focus | Surfaces |
|----|---------------|--------------|----------|
| `sched-policy-document` | Policy steps, triggers, refs, schema v1/v2 gates | Unknown step types rejected; feature gates for AI steps | Scheduling → Report Policies |
| `sched-policy-validator` | Cron validity, secret scan, SQL AST, unused vars, AI call caps | Secret detection completeness; SQL write rejection; AI caps | create/update/emulate |
| `sched-policy-lifecycle` | Create → approve (GUI-only) → schedule/run; content hash | Agents cannot approve; hash mismatch blocks run; audit log | MCP `zorivest_policy` (approve GUI-only) |
| `sched-cron-binding` | Cron + timezone → next run; misfire/coalesce | TZ correctness; DOW mapping; disabled scheduler safety | CronPicker; scheduler status |
| `sched-pipeline-runner` | Step graph execute with skip/retry/cancel/context | Ref resolution; cancel mid-run; retry bounds | `/policies/{id}/run` |
| `sched-policy-emulator` | Dry-run validate/simulate/render with mocks | Must not mutate production data or send real email | MCP `emulate` |
| `sched-pipeline-guardrails` | Rate limits on create/run/send via audit counts | Limit bypass; clock skew; env override safety | Enforced on run paths |
| `sched-email-templates` | Named Jinja templates for report email | Required variables; XSS in preview; CRUD auth | Scheduling → Email Templates; MCP `zorivest_template` |
| `sched-secure-jinja` | Immutable Jinja sandbox | No filesystem/network/attr escape; blocks unsafe filters | Template render/preview |
| `sched-sql-sandbox` | Read-only SQL for `query` steps | No DDL/DML; table allowlist; injection | MCP `zorivest_db`; validate-sql |
| `sched-compose-render-send` | Multi-source compose → HTML/PDF → SMTP | Fail closed on SMTP; no send without approve; attachment bounds | Report policies |
| `sched-web-search-step` | Fetch sources for citation-aware AI | Citation provenance; rate limits; empty-result handling | Policy `web_search` |

### Import, security, workflow, UI ops

| ID | Logic summary | Review focus | Surfaces |
|----|---------------|--------------|----------|
| `import-broker-csv` | **Deprecated (milestone-tracking).** Auto-detect broker CSV → map → write dispositions. The REST routes it fronted (`/api/v1/import/csv`, `/pdf`, `/preview`) now return **410 Gone**; the MCP `broker_csv` action stays registered but returns a deprecation. Superseded by JSON bulk import. | Wrong-broker mis-detect; duplicate fills; disposition accuracy | **Deprecated** — see `import-json-bulk` (Import Center; MCP `json_validate`/`json_stage`/`json_commit`) |
| `import-bank-statements` | Parse statements → cash movements | Account matching; currency; duplicate deposits | Bank import panel |
| `import-broker-sync` | Live broker execution ingest + conflict/orphan statuses | Confirmation gate; idempotent exec_id; report seeding | Broker sync; MCP `sync_broker` |
| `sec-confirmation-tokens` | Single-use ~60s tokens for destructive ops | TTL; one-time use; cannot reuse across actions | MCP `confirm_token` |
| `sec-mcp-unlock-state` | MCP write-guard locked/unlocked FSM | Auto-lock windows; unlock is human-gated by a single-use confirmation token bound to `mcp.unlock` | MCP guard status |
| `ui-ops-db-bootstrap` | Profile list/create/rename/switch + rebind | Path length; no cross-profile leak; scheduler rebind | Settings → Database |
| `workflow-account-review` | Periodic balance checklist prepare/apply/skip | Skip vs apply semantics; summary correctness | `AccountService` → `/accounts/review/{checklist,apply,skip}` → MCP `zorivest_account action=checklist`; no GUI (milestone-tracking) |
| `workflow-trade-report` | Post-trade report create + mistake track | Grades immutable after finalize?; delete confirmation | MCP `zorivest_report` |
| `data-round-trip-recompute` | Rebuild round trips when executions change | Cascades on edit/delete; related math `trade-round-trips` | Trade mutations |

---

## Screenshot → index map

Surfaces from `project configuration/docs/screenshots` that drove this catalog:

| Screenshot area | Feature ids |
|-----------------|-------------|
| Trades → AI Review | `ai-trade-review`, `ai-persona-prompts`, `ai-provider-resolution` |
| Tax → Audit | `tax-transaction-audit`, `tax-audit-duplicate-lots`, `tax-audit-orphaned-lots`, `tax-audit-missing-basis`, `tax-audit-invalid-proceeds` |
| Tax → Wash Sales / Harvesting | `tax-wash-chain-fsm`, `tax-harvest-scanner`, `tax-deferred-loss-report` |
| Tax → Lots (Reassign) | `tax-lot-reassignment`, `tax-lot-preview`, `tax-lot-sync-pipeline` |
| Scheduling → Report Policies | `sched-policy-*`, `sched-pipeline-*`, `sched-cron-binding`, `sched-compose-render-send`, `sched-web-search-step` |
| Scheduling → Email Templates | `sched-email-templates`, `sched-secure-jinja` |
| Analytics → Costs & Mistakes | `ai-mistake-heuristics` |

---

## Boundary with Math Features Index

| Concern | Use this index | Use math-features-index |
|---------|----------------|-------------------------|
| Formula correctness (Sharpe, wash §1091 math, Kelly, …) | — | Yes |
| Rule / FSM / sandbox / orchestration soundness | Yes | — |
| Mistake heuristics thresholds & priority | Yes (`ai-mistake-heuristics`) | Also linked as `trade-mistake-classify` |
| Wash *detection formula* vs wash *chain lifecycle* | Chain FSM here | Detection math there |
| Lot *selection method* vs lot *sync / audit / reassign gates* | Sync/audit/reassign here | FIFO/LIFO/HIFO there |

---

## Machine-readable ops block

<!-- VERIFY:complex-ops -->
```yaml
schema_version: 1
companion_math_index: docs/reference/math-features-index.md
module_root: packages/core/src/zorivest_core
features:
  - id: ai-trade-review
    name: Multi-persona AI trade review
    category: ai
    logic_summary: "Orchestrate risk/technical/behavioral persona reviews; parse JSON verdicts; enforce budget cap"
    review_focus: "Budget accounting; partial persona failure; prompt injection; response schema stability"
    theory_doc: null
    authority: "Build plan AI review / ReviewService AC-110.6-8"
    module: packages/core/src/zorivest_core/services/ai_review_client.py
    symbol: OpenAICompatibleClient.review
    service: ReviewService.ai_review
    api: POST /api/v1/analytics/ai-review
    mcp_action: ai_review
    gui: Trades → AI Review
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_ai_review_client.py
      - tests/unit/test_review_service_ai.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "DEFAULT_PERSONAS = risk, technical, behavioral. Not a math formula."

  - id: ai-persona-prompts
    name: Persona system-prompt contracts
    category: ai
    logic_summary: "Build per-persona system prompts; allow custom override from GUI"
    review_focus: "Role separation; custom override cannot change provider/auth; prompt length bounds"
    theory_doc: null
    authority: "AI review client DEFAULT_PERSONAS + GUI prompt editor"
    module: packages/core/src/zorivest_core/services/ai_review_client.py
    symbol: _build_system_prompt
    service: ReviewService
    api: GET /api/v1/ai-providers/default-prompt
    mcp_action: ai_review
    gui: Trades → AI Review (Customize System Prompt)
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_ai_review_client.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: ai-provider-resolution
    name: Active AI provider selection
    category: ai
    logic_summary: "Resolve BYOK/OpenRouter/local client from settings; lazy so Settings changes apply live"
    review_focus: "Secrets never logged; disabled/missing key fails closed; no cross-tenant leak (single-user local)"
    theory_doc: null
    authority: "AIProviderService"
    module: packages/core/src/zorivest_core/services/ai_provider_service.py
    symbol: AIProviderService
    service: AIProviderService
    api: /api/v1/ai-providers/*
    mcp_action: null
    gui: Settings → AI providers
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_ai_provider_service.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index, test"
    notes: ""

  - id: ai-pipeline-generate
    name: Pipeline AI generate step
    category: ai
    logic_summary: "Policy step ai_generate: chat completion with injected context, citation rules, schema_version 2"
    review_focus: "AI call caps; citation grounding; schema gate; provider resolution parity with trade review"
    theory_doc: ai-search-steps
    authority: "Phase 9 scheduling AI steps"
    module: packages/core/src/zorivest_core/pipeline_steps/ai_generate_step.py
    symbol: AIGenerateStep.execute
    service: PipelineRunner
    api: policy run paths
    mcp_action: null
    gui: Scheduling → Report Policies
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_ai_generate_step.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on index"
    notes: "UI copy: AI steps require schema_version 2."

  - id: ai-mistake-heuristics
    name: Mistake classification heuristics
    category: ai
    logic_summary: "Priority rules: revenge, oversized, FOMO, bad R:R, no-stop, overtrading, early/late exit"
    review_focus: "Rule order/thresholds; overlap categories; cost attribution"
    theory_doc: null
    authority: "Build plan behavioral analytics"
    module: packages/core/src/zorivest_core/domain/analytics/classify.py
    symbol: classify_trade_mistake
    service: ReviewService
    api: GET /api/v1/mistakes/summary
    mcp_action: mistake_summary
    gui: Analytics → Costs & Mistakes; Trades → Mistakes
    status: implemented
    related_math: [trade-mistake-classify]
    unit_tests:
      - tests/unit/test_mistake_repository.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "Also listed in math-features-index as trade-mistake-classify; review thresholds here."

  - id: tax-transaction-audit
    name: Tax transaction audit orchestrator
    category: tax_ops
    logic_summary: "Scan lots → findings[] + severity_summary (missing_basis, duplicate_lot, orphaned_lot, invalid_proceeds)"
    review_focus: "All AC-153 checks run; filters account/year; severity counts match findings"
    theory_doc: basis-of-assets
    authority: "AC-153; IRS Pub 551 referenced in GUI"
    module: packages/core/src/zorivest_core/services/tax_service.py
    symbol: TaxService.run_audit
    service: TaxService
    api: POST /api/v1/tax/audit
    mcp_action: null
    gui: Tax → Audit
    status: implemented
    related_math: [tax-lot-method, tax-st-lt]
    unit_tests:
      - tests/unit/test_tax_audit.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "GUI states rule-based, not AI."

  - id: tax-audit-duplicate-lots
    name: Duplicate lot detection
    category: tax_ops
    logic_summary: "Duplicate if same (account_id, ticker, open_date, quantity); severity=warning"
    review_focus: "False positives on legitimate same-day identical lots; message cites original_lot_id"
    theory_doc: basis-of-assets
    authority: "AC-153.4"
    module: packages/core/src/zorivest_core/services/tax_service.py
    symbol: TaxService.run_audit
    service: TaxService
    api: POST /api/v1/tax/audit
    mcp_action: null
    gui: Tax → Audit (Duplicate Lot warnings)
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_tax_audit.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "Screenshot rule matches code key exactly."

  - id: tax-audit-orphaned-lots
    name: Orphaned closed-lot rule
    category: tax_ops
    logic_summary: "Closed lot with empty linked_trade_ids → warning"
    review_focus: "Manual closed lots vs sync breakage; do not error open lots"
    theory_doc: basis-of-assets
    authority: "AC-153.5"
    module: packages/core/src/zorivest_core/services/tax_service.py
    symbol: TaxService.run_audit
    service: TaxService
    api: POST /api/v1/tax/audit
    mcp_action: null
    gui: Tax → Audit
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_tax_audit.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on surface"
    notes: ""

  - id: tax-audit-missing-basis
    name: Missing or zero cost basis
    category: tax_ops
    logic_summary: "cost_basis is None or 0 → error"
    review_focus: "Zero-basis edge cases (gift/spinoff); severity=error"
    theory_doc: cost-basis
    authority: "AC-153.3"
    module: packages/core/src/zorivest_core/services/tax_service.py
    symbol: TaxService.run_audit
    service: TaxService
    api: POST /api/v1/tax/audit
    mcp_action: null
    gui: Tax → Audit
    status: implemented
    related_math: [tax-lot-method]
    unit_tests:
      - tests/unit/test_tax_audit.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: tax-audit-invalid-proceeds
    name: Invalid proceeds on closed lots
    category: tax_ops
    logic_summary: "Closed lot with missing/invalid proceeds → finding"
    review_focus: "Open lots excluded; proceeds sign/null handling"
    theory_doc: capital-gains-losses
    authority: "AC-153.6"
    module: packages/core/src/zorivest_core/services/tax_service.py
    symbol: TaxService.run_audit
    service: TaxService
    api: POST /api/v1/tax/audit
    mcp_action: null
    gui: Tax → Audit
    status: implemented
    related_math: [tax-st-lt]
    unit_tests:
      - tests/unit/test_tax_audit.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on surface"
    notes: ""

  - id: tax-wash-chain-fsm
    name: Wash-sale chain state machine
    category: tax_ops
    logic_summary: "start_chain/absorb_loss/release_chain/continue_chain; statuses DISALLOWED→ABSORBED→RELEASED/DESTROYED"
    review_focus: "No double-absorb; basis add-back once; holding-period tack; irreversible DESTROYED"
    theory_doc: wash-sales
    authority: "WashSaleChainManager; IRC §1091 lifecycle"
    module: packages/core/src/zorivest_core/domain/tax/wash_sale_chain_manager.py
    symbol: WashSaleChainManager
    service: TaxService
    api: tax wash routes
    mcp_action: null
    gui: Tax → Wash Sales (ABSORBED badge)
    status: implemented
    related_math: [tax-same-account-wash, tax-cross-account-wash]
    unit_tests:
      - tests/unit/domain/tax/test_wash_sale_chain_manager.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "Detection math is in math-features-index; lifecycle is here."

  - id: tax-wash-conflict-warnings
    name: Wash conflict advisories
    category: tax_ops
    logic_summary: "check_conflicts for rebalance/spousal/DRIP-style 30-day advisories"
    review_focus: "Advisory only — must not auto-disallow; window boundaries"
    theory_doc: wash-sales
    authority: "wash_sale_warnings"
    module: packages/core/src/zorivest_core/domain/tax/wash_sale_warnings.py
    symbol: check_conflicts
    service: TaxService
    api: POST /api/v1/tax/wash-conflicts
    mcp_action: wash_conflicts
    gui: Tax → Wash Sales → Before you buy (WashPurchaseCheck)
    status: implemented
    related_math: [tax-same-account-wash]
    unit_tests:
      - tests/unit/domain/tax/test_wash_sale_warnings.py
      - tests/unit/domain/tax/test_wash_sale_purchase_advisory.py
      - tests/integration/test_wash_conflicts_route.py
      - tests/integration/test_surface_parity_tax.py
      - mcp-server/tests/compound/tax-wash-conflicts.test.ts
      - ui/src/renderer/src/features/tax/__tests__/wash-purchase-check.test.tsx
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: ""

  - id: tax-lot-reassignment
    name: T+1 lot method reassignment
    category: tax_ops
    logic_summary: "TaxService.reassign_basis calls can_reassign_lots for the eligibility gate, then reassign_lots via the lot selector. The window is anchored to the lot's DISPOSAL (close_date), not its acquisition."
    review_focus: "Settlement window gate is delegated, never re-inlined; window anchors on disposal; method parity with selector"
    theory_doc: fifo-lifo
    authority: "lot_reassignment domain"
    module: packages/core/src/zorivest_core/domain/tax/lot_reassignment.py
    symbol: can_reassign_lots
    service: TaxService.reassign_basis
    api: tax lot reassign routes
    mcp_action: null
    gui: Tax → Lots (Reassign)
    status: implemented
    related_math: [tax-lot-method]
    unit_tests:
      - tests/unit/domain/tax/test_lot_reassignment.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index; REMEDIATED-PARTIAL Claude Opus 5; 2026-08-03; milestone-tracking audit-lot-matching-basis; closed audit finding; status stays drift_suspected because the remaining findings on this row are out of milestone-tracking scope"
    notes: ""

  - id: tax-lot-sync-pipeline
    name: Trade-to-lot sync pipeline
    category: tax_ops
    logic_summary: "Two-pass BOT create + SLD close. Pass 2 consumes open lots FIFO by *quantity* — a sell larger than one lot walks the bucket, and a sell smaller than the candidate splits an `{lot_id}-R` remainder (`_REMAINDER_SUFFIX`) back into the bucket so a later sell can consume it. Quantity that finds no open lot is reported as an `UnmatchedSell` carrying `unmatched_quantity`; it is never silently skipped."
    review_focus: "Idempotency (an unmatched sell is re-reported on every run because it was never applied); abort vs continue; never clobber wash-adjusted basis silently; every SLD share either closes a lot or is counted unmatched"
    theory_doc: cost-basis
    authority: "TaxService.sync_lots"
    module: packages/core/src/zorivest_core/services/tax_service.py
    symbol: sync_lots
    service: TaxService
    api: POST /api/v1/tax/sync-lots
    mcp_action: null
    gui: Tax → Dashboard (Process Tax Lots)
    status: implemented
    related_math: [tax-lot-method, tax-st-lt]
    unit_tests:
      - tests/unit/test_tax_sync_service.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface; REMEDIATED-PARTIAL Claude Opus 5; 2026-08-03; milestone-tracking audit-lot-matching-basis; closed audit finding; status stays drift_suspected because the remaining findings on this row are out of milestone-tracking scope"
    notes: ""

  - id: tax-harvest-scanner
    name: Tax-loss harvest candidate scanner
    category: tax_ops
    logic_summary: "Scan open lots where market value < cost basis; apply min threshold; flag/exclude wash risk. `GET /tax/harvest` calls scan_harvest_candidates directly — it does *not* delegate to the trapped-loss report — and classifies quote outcomes on an explicit failure matrix: M1 no open lots → 200 empty; M2 partial quote failure → 200 with `skipped_tickers`; M3 zero usable quotes → 503; M4 no market-data provider configured → 503 with a distinct reason (M4 preempts M1, because the dependency resolves eagerly); M5 unexpected exception → 5xx."
    review_focus: "Threshold config; wash-risk exclusion; aggregation of deferred totals; NO blanket except-Exception — a failed scan must surface as 5xx/503, never as a 200 whose total_harvestable reads 0.00"
    theory_doc: capital-gains-losses
    authority: "Harvest scanner; IRS Topic 409 cited in GUI"
    module: packages/core/src/zorivest_core/domain/tax/harvest_scanner.py
    symbol: scan_harvest_candidates
    service: TaxService
    api: tax harvest routes
    mcp_action: null
    gui: Tax → Harvesting
    status: implemented
    related_math: [tax-same-account-wash, tax-carryforward-3k]
    unit_tests:
      - tests/unit/domain/tax/test_harvest_scanner.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index; REMEDIATED-PARTIAL Claude Opus 5; 2026-08-03; milestone-tracking audit-lot-matching-basis; closed audit finding, audit finding; status stays drift_suspected because the remaining findings on this row are out of milestone-tracking scope"
    notes: "Formula harvestable = basis - MV is operational selection here; tax rates applied elsewhere."

  - id: tax-deferred-loss-report
    name: Deferred / trapped wash-loss report
    category: tax_ops
    logic_summary: "Aggregate ABSORBED/DESTROYED chain losses vs real/reported P&L"
    review_focus: "Status filters; no double-count across chains"
    theory_doc: wash-sales
    authority: "TaxService.deferred_loss_report"
    module: packages/core/src/zorivest_core/services/tax_service.py
    symbol: deferred_loss_report
    service: TaxService
    api: GET /api/v1/tax/deferred-losses
    mcp_action: null
    gui: Tax reports / Harvesting deferred totals
    status: implemented
    related_math: [tax-same-account-wash]
    unit_tests:
      - tests/unit/test_tax_deferred_loss.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: tax-alpha-counterfactual
    name: Tax alpha counterfactual
    category: tax_ops
    logic_summary: "Compare actual tax outcome vs FIFO counterfactual + harvest savings"
    review_focus: "Counterfactual independence (must not reuse mutated lots); savings attribution"
    theory_doc: fifo-lifo
    authority: "TaxService.tax_alpha_report"
    module: packages/core/src/zorivest_core/services/tax_service.py
    symbol: tax_alpha_report
    service: TaxService
    api: GET /api/v1/tax/alpha
    mcp_action: null
    gui: Tax analytics surfaces
    status: implemented
    related_math: [tax-lot-method]
    unit_tests:
      - tests/unit/test_tax_alpha.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: tax-option-assignment-basis
    name: Option assignment / exercise basis adjust
    category: tax_ops
    logic_summary: "Four Pub 550 premium paths via parse_option_symbol + adjust_basis_for_assignment. TaxService.pair_option_assignment resolves and validates the linkage — lot lookup, OCC parse, `linked_trade_ids` membership — BEFORE any write to cost_basis, so a rejected pairing leaves the lot byte-identical rather than half-adjusted. Realized proceeds are recomputed from the adjusted basis in the same transaction."
    review_focus: "Call/put × exercise/assignment matrix; OCC symbol parse edge cases; guard-before-mutate ordering — no read-modify-write of cost_basis ahead of the linkage check, and no adjusted basis left paired with stale proceeds"
    theory_doc: options-strategy
    authority: "IRS Pub 550 option premium basis"
    module: packages/core/src/zorivest_core/domain/tax/option_pairing.py
    symbol: adjust_basis_for_assignment
    service: TaxService
    api: tax lot / import paths
    mcp_action: null
    gui: Tax → Lots (options)
    status: implemented
    related_math: [tax-lot-method]
    unit_tests:
      - tests/unit/test_option_pairing.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index; REMEDIATED-PARTIAL Claude Opus 5; 2026-08-03; milestone-tracking audit-lot-matching-basis; closed audit finding; status stays drift_suspected because the remaining findings on this row are out of milestone-tracking scope"
    notes: ""

  - id: tax-lot-preview
    name: Lot selection preview
    category: tax_ops
    logic_summary: "preview_lot_selection / get_lot_details explain which lots close under method"
    review_focus: "Must match select_lots_for_closing ordering"
    theory_doc: fifo-lifo
    authority: "lot_matcher"
    module: packages/core/src/zorivest_core/domain/tax/lot_matcher.py
    symbol: preview_lot_selection
    service: TaxService
    api: tax lot preview
    mcp_action: null
    gui: Tax → Lots / Simulator
    status: implemented
    related_math: [tax-lot-method]
    unit_tests:
      - tests/unit/domain/tax/test_lot_matcher.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: ""

  - id: sched-policy-document
    name: Pipeline policy document model
    category: scheduling
    logic_summary: "PolicyDocument/PolicyStep/TriggerConfig; schema v1/v2 feature gates for AI steps"
    review_focus: "Unknown step types rejected; AI steps require schema_version 2"
    theory_doc: cron-expressions
    authority: "Phase 9 pipeline models"
    module: packages/core/src/zorivest_core/domain/pipeline.py
    symbol: PolicyDocument
    service: SchedulingService
    api: /api/v1/scheduling/policies*
    mcp_action: null
    gui: Scheduling → Report Policies
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_pipeline_models.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "GUI: No portfolio math — policies define workflow steps."

  - id: sched-policy-validator
    name: Policy validator (secrets/SQL/AI caps)
    category: scheduling
    logic_summary: "validate_policy: cron, refs, secret scan, SQL AST, unused vars, AI call caps"
    review_focus: "Secret patterns; SQL writes rejected; AI cap bypass; unused-var noise vs real bugs"
    theory_doc: cron-expressions
    authority: "policy_validator"
    module: packages/core/src/zorivest_core/domain/policy_validator.py
    symbol: validate_policy
    service: SchedulingService
    api: create/update/emulate
    mcp_action: null
    gui: Scheduling → Report Policies
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_policy_validator.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: ""

  - id: sched-policy-lifecycle
    name: Policy create / approve / run / schedule
    category: scheduling
    logic_summary: "Lifecycle with GUI-only approve, content hash, next-run projection, audit log"
    review_focus: "Agents cannot approve; hash mismatch blocks run; audit completeness"
    theory_doc: cron-expressions
    authority: "SchedulingService; MCP policy approve GUI-only"
    module: packages/core/src/zorivest_core/services/scheduling_service.py
    symbol: SchedulingService
    service: SchedulingService
    api: /api/v1/scheduling/policies*
    mcp_action: null
    gui: Scheduling → Report Policies (approval flow)
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_scheduling_service.py
      - tests/unit/test_api_scheduling.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "MCP zorivest_policy create/emulate/run; approve is GUI-only."

  - id: sched-cron-binding
    name: Cron to APScheduler binding
    category: scheduling
    logic_summary: "Bind cron+timezone to jobs; misfire/coalesce; next-run calculation"
    review_focus: "Timezone correctness; DOW mapping; disabled scheduler does not fire"
    theory_doc: cron-expressions
    authority: "SchedulerService"
    module: packages/core/src/zorivest_core/services/scheduler_service.py
    symbol: SchedulerService
    service: SchedulerService
    api: GET /api/v1/scheduling/scheduler/status
    mcp_action: null
    gui: Scheduling CronPicker; next-run display
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_scheduling_timezone_setting.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain"
    notes: ""

  - id: sched-pipeline-runner
    name: Pipeline execution engine
    category: scheduling
    logic_summary: "Execute step graph with skip_if, retries, cancel, context ref resolution"
    review_focus: "Retry bounds; cancel mid-run; ref cycles; step order"
    theory_doc: null
    authority: "PipelineRunner"
    module: packages/core/src/zorivest_core/services/pipeline_runner.py
    symbol: PipelineRunner
    service: PipelineRunner
    api: POST /policies/{id}/run
    mcp_action: null
    gui: Scheduling run history
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_pipeline_runner.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index, test"
    notes: ""

  - id: sched-policy-emulator
    name: Policy dry-run emulator
    category: scheduling
    logic_summary: "Validate/simulate/render with anonymized mocks; no production side effects"
    review_focus: "Must not send email or write reports; mock fidelity"
    theory_doc: null
    authority: "PolicyEmulator"
    module: packages/core/src/zorivest_core/services/policy_emulator.py
    symbol: PolicyEmulator
    service: PolicyEmulator
    api: /emulator/run
    mcp_action: null
    gui: Scheduling emulate
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_policy_emulator.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "MCP policy emulate."

  - id: sched-pipeline-guardrails
    name: Pipeline rate / abuse guardrails
    category: scheduling
    logic_summary: "Rate-limit policy.create / pipeline.run / report.send via audit log counts"
    review_focus: "Limit bypass; clock skew; env override safety"
    theory_doc: null
    authority: "PipelineGuardrails"
    module: packages/core/src/zorivest_core/services/pipeline_guardrails.py
    symbol: PipelineGuardrails
    service: PipelineGuardrails
    api: enforced on run paths
    mcp_action: null
    gui: null
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_pipeline_guardrails.py
      - tests/unit/test_mcp_pipeline_security.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: ""

  - id: sched-email-templates
    name: Email template CRUD and preview
    category: scheduling
    logic_summary: "Named templates with required variables; preview + use in send step"
    review_focus: "Required-var validation; XSS in preview; unauthorized CRUD"
    theory_doc: jinja2-templates
    authority: "Email template repository + routes"
    module: packages/infrastructure/src/zorivest_infra
    symbol: email_template_repository
    service: SchedulingService
    api: /api/v1/scheduling/templates*
    mcp_action: null
    gui: Scheduling → Email Templates
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_email_template_repository.py
      - tests/unit/test_send_step_template.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "MCP zorivest_template. Module path is infra repo + API routes."

  - id: sched-secure-jinja
    name: Hardened Jinja sandbox
    category: scheduling
    logic_summary: "Immutable sandbox; block unsafe ops/filters/attr escape"
    review_focus: "Sandbox escape; filesystem/network access; prototype pollution via attrs"
    theory_doc: jinja2-templates
    authority: "secure_jinja HardenedSandbox"
    module: packages/core/src/zorivest_core/services/secure_jinja.py
    symbol: HardenedSandbox
    service: null
    api: template preview/render
    mcp_action: null
    gui: Email Templates preview
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_secure_jinja.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: ""

  - id: sched-sql-sandbox
    name: Read-only SQL query sandbox
    category: scheduling
    logic_summary: "Multi-layer SQL sandbox for policy query steps; validate-sql endpoint"
    review_focus: "No DDL/DML; injection; table allowlist; LIMIT enforcement"
    theory_doc: null
    authority: "SqlSandbox + query_step"
    module: packages/core/src/zorivest_core/services/sql_sandbox.py
    symbol: SqlSandbox
    service: null
    api: validate-sql; query steps
    mcp_action: null
    gui: Scheduling policy query steps
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_mcp_pipeline_security.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "MCP zorivest_db SQL validation."

  - id: sched-compose-render-send
    name: Compose render send pipeline
    category: scheduling
    logic_summary: "compose_step → render_step → send_step (+ store_report); HTML/PDF/email"
    review_focus: "No send without approval; SMTP fail-closed; attachment size; dry-run"
    theory_doc: jinja2-templates
    authority: "Phase 9 report delivery"
    module: packages/core/src/zorivest_core/pipeline_steps
    symbol: ComposeStep / RenderStep / SendStep
    service: PipelineRunner
    api: policy run
    mcp_action: null
    gui: Scheduling Report Policies
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_send_step_template.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: ""

  - id: sched-web-search-step
    name: Web search context step
    category: scheduling
    logic_summary: "Fetch web sources into context blocks for citation-aware ai_generate"
    review_focus: "Citation provenance; empty results; rate limits; no secret leakage in queries"
    theory_doc: ai-search-steps
    authority: "web_search_step"
    module: packages/core/src/zorivest_core/pipeline_steps/web_search_step.py
    symbol: WebSearchStep
    service: PipelineRunner
    api: policy run
    mcp_action: null
    gui: Scheduling policies with web_search
    status: implemented
    related_math: []
    unit_tests: []
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "Confirm dedicated unit tests when reviewing; may be covered only via pipeline integration."

  - id: import-broker-csv
    name: Broker CSV auto-detect and import (deprecated)
    category: import
    logic_summary: "Deprecated — superseded by JSON bulk import. Detect broker format → map columns → write dispositions (create/skip/conflict)"
    review_focus: "Mis-detect risk; duplicate fills; disposition accuracy; partial file failure"
    theory_doc: null
    authority: "ImportService"
    module: packages/core/src/zorivest_core/services/import_service.py
    symbol: ImportService.import_file
    service: ImportService
    api: "/api/v1/import/{csv,pdf,preview} — 410 Gone since milestone-tracking"
    mcp_action: null
    gui: Import dialog
    status: deprecated
    related_math: []
    unit_tests:
      - tests/unit/test_csv_import.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "Deprecated by milestone-tracking — MCP zorivest_import broker_csv stays registered but returns a deprecation naming the JSON bulk path; the REST routes return 410."

  - id: import-bank-statements
    name: Bank statement import
    category: import
    logic_summary: "Parse bank statements into account cash movements"
    review_focus: "Account matching; currency; duplicate deposits"
    theory_doc: null
    authority: "BankImportService"
    module: packages/core/src/zorivest_core/services/bank_import_service.py
    symbol: BankImportService
    service: BankImportService
    api: banking import routes
    mcp_action: null
    gui: Bank import panel
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_bank_import_service.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index, test"
    notes: ""

  - id: import-broker-sync
    name: Live broker sync ingest
    category: import
    logic_summary: "Ingest broker executions; conflict/orphan statuses; confirmation-gated"
    review_focus: "Idempotent exec_id; confirmation required; no silent overwrite"
    theory_doc: null
    authority: "BrokerSyncService"
    module: packages/core/src/zorivest_core/services/broker_sync_service.py
    symbol: BrokerSyncService
    service: BrokerSyncService
    api: /api/v1/brokers/*/sync*
    mcp_action: null
    gui: Broker sync panel
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_import_service_sync.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "Destructive/write sync uses confirmation tokens."

  - id: sec-confirmation-tokens
    name: Destructive confirmation tokens
    category: security
    logic_summary: "Single-use ~60s tokens for destructive MCP/API ops"
    review_focus: "TTL; one-time use; action binding; no reuse across ops"
    theory_doc: null
    authority: "Auth confirmation workflow"
    module: packages/core/src/zorivest_core/services/auth_service.py
    symbol: create_confirmation_token
    service: AuthService
    api: /api/v1/confirmation-tokens
    mcp_action: confirm_token
    gui: null (MCP/agent flow)
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_confirmation_gates.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "zorivest_system(action:confirm_token)."

  - id: sec-mcp-unlock-state
    name: MCP write-guard unlock FSM
    category: security
    logic_summary: "Locked/unlocked transitions for MCP write tools; auto-lock windows"
    review_focus: "Cannot write while locked; the unlock path is human-gated by a single-use confirmation token bound to mcp.unlock, minted through POST /api/v1/confirmation-tokens and submitted in the unlock request body (milestone-tracking / D10); lock reason surfaced"
    theory_doc: null
    authority: "MCP guard / UnlockStateMachine"
    module: packages/core/src/zorivest_core/services/unlock_state.py
    symbol: UnlockStateMachine
    service: null
    api: /api/v1/mcp-guard/status
    mcp_action: null
    gui: Settings MCP guard
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_mcp_guard_service.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: ""

  - id: ui-ops-db-bootstrap
    name: Database profile bootstrap and switch
    category: ui_ops
    logic_summary: "List/create/rename/switch DB profiles; path checks; rebind scheduler/API to active registry"
    review_focus: "No cross-profile data leak; path length; rebind atomicity; size_bytes accuracy"
    theory_doc: null
    authority: "database_bootstrap_cli + Electron worker"
    module: packages/api/src/zorivest_api/database_bootstrap_cli.py
    symbol: database bootstrap CLI / rebind
    service: null
    api: bootstrap CLI / IPC
    mcp_action: null
    gui: Settings → Database switcher
    status: implemented
    related_math: []
    unit_tests:
      - ui/src/main/__tests__/database-bootstrap-list-profiles-size.test.ts
      - tests/unit/test_list_profiles_size_bytes.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "Spans API CLI + Electron main process."

  - id: workflow-account-review
    name: Account review checklist
    category: workflow
    logic_summary: "prepare_review_checklist / apply_balance_update / skip summary"
    review_focus: "Skip vs apply semantics; balance update integrity"
    theory_doc: null
    authority: "account_review domain"
    module: packages/core/src/zorivest_core/domain/account_review.py
    symbol: prepare_review_checklist
    service: "AccountService.get_review_checklist / apply_review_balance / skip_review_account"
    api: "GET /api/v1/accounts/review/checklist; POST /api/v1/accounts/review/apply; POST /api/v1/accounts/review/skip"
    mcp_action: "zorivest_account action=checklist"
    gui: "not implemented — no review wizard exists; deferred to milestone-tracking"
    status: implemented
    related_math: []
    unit_tests:
      - tests/unit/test_account_review.py
      - tests/unit/test_account_review_wiring.py
      - tests/integration/test_account_review_routes.py
    validation_status: validated
    validated_by: "Claude Opus 5; 2026-08-02; milestone-tracking WP-8 remediation of audit finding; the domain module had no production caller and the MCP surface re-implemented the checklist in TypeScript against /brokers + /banking/accounts. AccountService now delegates to the domain, three REST routes expose it, and the MCP action proxies those routes. The prior row advertised a service, api and gui that did not exist; gui still does not, and now says so."
    notes: "No GUI wizard yet — the row named one before any existed."

  - id: workflow-trade-report
    name: Post-trade report lifecycle
    category: workflow
    logic_summary: "Create report with setup/execution grades; track/update/delete mistakes"
    review_focus: "Destructive delete confirmation; grade immutability rules; journal link integrity"
    theory_doc: null
    authority: "zorivest_report compound tool"
    module: packages/core/src/zorivest_core/services/review_service.py
    symbol: ReviewService report methods
    service: ReviewService
    api: /api/v1/reports*
    mcp_action: null
    gui: Trades → Journal / Mistakes / AI Review
    status: implemented
    related_math: [trade-execution-quality]
    unit_tests:
      - tests/unit/test_review_service_ai.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "MCP zorivest_report create/get/track_mistake/..."

  - id: data-round-trip-recompute
    name: Round-trip recompute on mutation
    category: data_integrity
    logic_summary: "Rebuild matched round trips when executions are edited/deleted"
    review_focus: "Cascade completeness; no stale open trips; related matcher math unchanged"
    theory_doc: fifo-lifo
    authority: "round_trip_recompute service"
    module: packages/core/src/zorivest_core/services/round_trip_recompute.py
    symbol: recompute
    service: TradeService
    api: trade mutation paths
    mcp_action: null
    gui: Trades → Round Trips (after edits)
    status: implemented
    related_math: [trade-round-trips]
    unit_tests:
      - tests/integration/test_round_trip_recompute.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: ""
```

---

## How models should use this index

1. Select a feature by `id` or GUI surface (screenshot map above).
2. Read `logic_summary` + `review_focus` + `authority`.
3. Open `module` / `symbol` and verify invariants in `review_focus`.
4. Run `unit_tests`; for security/sandbox rows, prefer adversarial cases (injection, escape, bypass).
5. If the op *uses* math, also open `related_math` rows in math-features-index.md — do not re-derive formulas here.
6. On completion, set `validation_status` to `model_reviewed` (or `drift_suspected`) and fill `validated_by` with model + date + evidence path.
