# Math Features Index — Code ↔ Theory ↔ Validation Catalog

<!--
  MAINTAINER NOTE: This file is the validation-ready SSOT for advanced math features
  implemented in Zorivest. Theory docs under docs/reference/ explain formulas; this
  index maps each feature to domain code, API/MCP/GUI surfaces, oracles, and tests.

  Companion indexes (do NOT overload them):
    - docs/reference/INDEX.md          — theory slug ↔ GUI deep-links
    - docs/build-plan/output-index.md  — computed outputs ↔ surface contracts
    - an internal record
                                       — independent oracles for seed-trade validation

  TO ADD A FEATURE:
    1. Implement domain math under packages/core (never infra/api)
    2. Append a Quick Reference row
    3. Append a matching object in the VERIFY:math-features block
    4. Link theory_doc / oracle_ref / unit_tests when they exist

  ORPHAN DETECTION (manual or future checker):
    - Theory math slug in INDEX.md with no row here
    - Public domain analytics/calculator/greeks symbol with no row
    - zorivest_analytics compute action with no row
    - oracle feature id with no oracle_ref pointing at it

  validation_status values:
    unvalidated | unit_covered | oracle_ready | model_reviewed | drift_suspected
  validated_by stays empty until a real validation pass (test suite name, review file, or model+date).
-->

> Part of Reference Docs · Companion: Output Index · Complex Ops Index · Oracles: feature-config-oracle-spec.md

Canonical registry of **math features** used in calculators, analytics, risk, trade analysis, options, and tax formula logic — with **code locations** so advanced models can validate correctness from this index.

For **non-math operational logic** (AI Review, tax audit rules, cron/policies/templates, import, confirmation gates), use the Complex Ops Index.

---

## Legend

| Column / Field | Meaning |
|----------------|---------|
| **Category** | `calculator` · `performance` · `risk` · `trade_analysis` · `sizing` · `tax` · `options` · `quant` |
| **Status** | `implemented` · `scaffold` · `deferred` · `theory_only` |
| **Validation** | `unvalidated` · `unit_covered` · `oracle_ready` · `model_reviewed` · `drift_suspected` |
| **Oracle** | Feature id in seed-trade `feature-config-oracle-spec.md`, or `none` |

---

## Quick Reference

| # | ID | Name | Category | Module | Symbol | MCP | Validation |
|---|----|------|----------|--------|--------|-----|------------|
| 1 | `calc-position-size` | Position size (fixed-risk) | calculator | `domain/calculator.py` | `calculate_position_size` | `position_size` | drift_suspected |
| 2 | `calc-vol-target-size` | Volatility-target size | sizing | `domain/analytics/position_sizer.py` | `vol_target_size` | `vol_target_size` | drift_suspected |
| 3 | `calc-forward-cost` | Forward transaction cost | trade_analysis | `domain/analytics/fee_decomposition.py` | `estimate_forward_cost` | `forward_cost` | drift_suspected |
| 4 | `trade-expectancy` | Expectancy & profit factor | performance | `domain/analytics/expectancy.py` | `calculate_expectancy` | `expectancy` | drift_suspected |
| 5 | `trade-win-rate` | Win rate | performance | `domain/analytics/expectancy.py` | `calculate_expectancy` | `expectancy` | drift_suspected |
| 6 | `trade-kelly-criterion` | Kelly fraction | performance | `domain/analytics/expectancy.py` | `calculate_expectancy` | `expectancy` | drift_suspected |
| 7 | `trade-risk-reward-ratio` | Payoff / R:R ratio | performance | `domain/analytics/expectancy.py` | `calculate_expectancy` | `expectancy` | drift_suspected |
| 8 | `perf-sharpe-ratio` | Sharpe ratio | performance | `domain/analytics/performance.py` | `compute_performance` | `performance_metrics` | drift_suspected |
| 9 | `perf-sortino-ratio` | Sortino ratio | performance | `domain/analytics/performance.py` | `compute_performance` | `performance_metrics` | drift_suspected |
| 10 | `perf-calmar-ratio` | Calmar ratio | performance | `domain/analytics/performance.py` | `compute_performance` | `performance_metrics` | drift_suspected |
| 11 | `perf-tail-ratio` | Tail ratio | performance | `domain/analytics/performance.py` | `compute_performance` | `performance_metrics` | drift_suspected |
| 12 | `perf-max-drawdown` | Max drawdown & recovery | performance | `domain/analytics/performance.py` | `compute_performance` | `performance_metrics` | drift_suspected |
| 13 | `risk-monte-carlo-drawdown` | Monte Carlo drawdown table | risk | `domain/analytics/drawdown.py` | `drawdown_probability_table` | `drawdown` | drift_suspected |
| 14 | `risk-recommended-risk-pct` | Recommended risk % (Kelly≤2%) | risk | `domain/analytics/drawdown.py` | `_compute_recommended_risk` | `drawdown` | drift_suspected |
| 15 | `risk-of-ruin` | Risk of ruin (MC+) | risk | `domain/analytics/drawdown.py` | `risk_of_ruin` | `monte_carlo_plus` | drift_suspected |
| 16 | `risk-equity-curve-bands` | Equity-curve percentile bands | risk | `domain/analytics/drawdown.py` | `equity_curve_bands` | `monte_carlo_plus` | drift_suspected |
| 17 | `risk-percentile` | Drawdown percentiles | risk | `domain/analytics/drawdown.py` | `drawdown_probability_table` | `drawdown` | drift_suspected |
| 18 | `trade-sqn` | System Quality Number | trade_analysis | `domain/analytics/sqn.py` | `calculate_sqn` | `sqn` | drift_suspected |
| 19 | `trade-r-multiples` | R-multiples (SQN input) | trade_analysis | `domain/analytics/sqn.py` | `calculate_sqn` | `sqn` | unit_covered |
| 20 | `trade-mfe-mae-excursion` | MFE / MAE / capture % | trade_analysis | `domain/analytics/excursion.py` | `calculate_mfe_mae` | `excursion` | drift_suspected |
| 21 | `trade-execution-quality` | Execution quality (NBBO) | trade_analysis | `domain/analytics/execution_quality.py` | `score_execution` | `execution_quality` | drift_suspected |
| 22 | `trade-pfof` | PFOF impact | trade_analysis | `domain/analytics/pfof.py` | `analyze_pfof` | `pfof_impact` | drift_suspected |
| 23 | `trade-cost-of-free` | Cost of free trading | trade_analysis | `domain/analytics/cost_of_free.py` | `cost_of_free` | `cost_of_free` | drift_suspected |
| 24 | `trade-fee-breakdown` | Fee decomposition | trade_analysis | `domain/analytics/fee_decomposition.py` | `decompose_fees` | `fee_breakdown` | drift_suspected |
| 25 | `trade-round-trips` | Round-trip matcher | trade_analysis | `domain/analytics/round_trip_matcher.py` | `match_executions_to_round_trips` | `round_trips` | drift_suspected |
| 26 | `trade-options-strategy` | Options strategy detect | options | `domain/analytics/options_strategy.py` | `detect_options_strategy` | `options_strategy` | drift_suspected |
| 27 | `trade-strategy-breakdown` | Strategy P&L buckets | trade_analysis | `domain/analytics/strategy.py` | `breakdown_by_strategy` | `strategy_breakdown` | drift_suspected |
| 28 | `trade-mistake-classify` | Mistake classification | trade_analysis | `domain/analytics/classify.py` | `classify_trade_mistake` | `mistake_summary` | drift_suspected |
| 29 | `risk-ewma-vol` | EWMA volatility | risk | `domain/analytics/volatility.py` | `ewma_vol` | `ewma_vol` | drift_suspected |
| 30 | `risk-correlation-matrix` | Correlation matrix | risk | `domain/analytics/portfolio.py` | `correlation_matrix` | `correlation_matrix` | drift_suspected |
| 31 | `risk-hhi` | Herfindahl–Hirschman Index | risk | `domain/analytics/portfolio.py` | `hhi` | `decompose_portfolio_risk` | drift_suspected |
| 32 | `risk-effective-positions` | Effective bets (Meucci ENB) | risk | `domain/analytics/portfolio.py` | `effective_bets` | `decompose_portfolio_risk` | drift_suspected |
| 33 | `risk-diversification-multiplier` | Diversification multiplier | risk | `domain/analytics/portfolio.py` | `diversification_multiplier` | `decompose_portfolio_risk` | drift_suspected |
| 34 | `risk-contribution` | Euler risk decomposition | risk | `domain/analytics/portfolio.py` | `decompose_risk` | `decompose_portfolio_risk` | drift_suspected |
| 35 | `risk-handcrafted-weights` | Handcrafted / grouped weights | risk | `domain/analytics/portfolio.py` | `handcrafted_weights` | `decompose_portfolio_risk` | drift_suspected |
| 36 | `risk-marginal-risk` | Marginal risk contribution | risk | `domain/analytics/portfolio.py` | `decompose_risk` | `decompose_portfolio_risk` | drift_suspected |
| 37 | `risk-vol-regime` | Volatility regime | risk | `domain/analytics/regime.py` | `classify_vol_regime` | `regime_state` | drift_suspected |
| 38 | `options-bsm-greeks` | Black–Scholes–Merton Greeks | options | `options/greeks.py` | `black_scholes_greeks` | `null` | drift_suspected |
| 39 | `quant-trial-ledger` | Trial ledger (DSR substrate) | quant | `domain/quant/trial.py` | trial record types | `record_trial` / `list_trials` | drift_suspected |
| 40 | `tax-st-lt` | ST/LT holding period | tax | `domain/tax/holding_period.py` | `is_long_term_holding` | `null` | unit_covered |
| 41 | `tax-lot-method` | FIFO/LIFO/HIFO/spec-ID | tax | `domain/tax/lot_selector.py` | `select_lots_for_closing` | `null` | drift_suspected |
| 42 | `tax-same-account-wash` | Same-account wash sale | tax | `domain/tax/wash_sale_detector.py` | wash detect | `null` | drift_suspected |
| 43 | `tax-cross-account-wash` | Cross-account wash sale | tax | `domain/tax/wash_sale_detector.py` | wash detect | `null` | drift_suspected |
| 44 | `tax-roth-rr2008-5` | Roth replacement (RR 2008-5) | tax | `domain/tax/replacement_suggestions.py` | `suggest_replacements` | `null` | drift_suspected |
| 45 | `tax-drip-dividend-reinvest` | DRIP as wash replacement | tax | `domain/tax/replacement_suggestions.py` | `suggest_replacements` | `null` | drift_suspected |
| 46 | `tax-1256-60-40` | §1256 60/40 | tax | `null` | `null` | `null` | drift_suspected |
| 47 | `tax-988-fx` | §988 FX ordinary gain | tax | `null` | `null` | `null` | drift_suspected |
| 48 | `tax-niit` | NIIT 3.8% | tax | `domain/tax/niit.py` | `compute_niit` | `null` | drift_suspected |
| 49 | `tax-carryforward-3k` | $3k loss carryforward | tax | `domain/tax/loss_carryforward.py` | `apply_capital_loss_rules` | `null` | drift_suspected |
| 50 | `tax-quarterly-estimate` | Quarterly safe harbor | tax | `domain/tax/quarterly.py` | `compute_safe_harbor` | `null` | drift_suspected |

---

## Implementation Map

Paths below are relative to `packages/core/src/zorivest_core/` unless noted.

### Calculator & sizing

| ID | Formula summary | Theory | API | GUI |
|----|-----------------|--------|-----|-----|
| `calc-position-size` | `share_size = floor((balance × risk_pct/100) / \|entry−stop\|)` | — (see output-index §1) | `POST /api/v1/calculator/position-size` | Calculator |
| `calc-vol-target-size` | `size ∝ target_vol / realized_vol` | — | `POST /api/v1/calculator/vol-target` | Calculator |
| `calc-forward-cost` | commission + spread + slippage estimate | commission, slippage | `POST /api/v1/calculator/forward-cost` | Calculator / Costs |

### Performance & expectancy

| ID | Formula summary | Theory | API | GUI |
|----|-----------------|--------|-----|-----|
| `trade-expectancy` | `(WR×AW) − (LR×AL)`; `net_edge = expectancy − avg_commission` | expectancy | `GET /api/v1/analytics/expectancy` | Analytics → Overview |
| `trade-win-rate` | `wins / n` | win-rate | same | Analytics → Overview |
| `trade-kelly-criterion` | `WR − (LR / payoff)`; product shows full Kelly %; recommended risk caps at 2% | kelly-criterion | same | Analytics → Overview |
| `trade-risk-reward-ratio` | `avg_win / avg_loss` (payoff) | risk-reward-ratio | same | Analytics → Overview |
| `perf-sharpe-ratio` | `mean(excess)/std(excess) × √N` (sample std) | sharpe-ratio | `GET /api/v1/analytics/performance` | Analytics → Overview |
| `perf-sortino-ratio` | `mean(excess)/downside_RMS × √N` | sortino-ratio | same | Analytics → Overview |
| `perf-calmar-ratio` | `CAGR / \|max_drawdown\|` | calmar-ratio | same | Analytics → Overview |
| `perf-tail-ratio` | `\|P95\| / \|P5\|` of returns | tail-risk | same | Analytics → Overview |
| `perf-max-drawdown` | peak-to-trough of cumulative equity; recovery = net / \|MDD\| | drawdown | same | Analytics → Overview / Risk |

### Risk (Monte Carlo, vol, portfolio)

| ID | Formula summary | Theory | API | GUI |
|----|-----------------|--------|-----|-----|
| `risk-monte-carlo-drawdown` | bootstrap trade P&L → percentile MDD table | monte-carlo | `GET /api/v1/analytics/drawdown` | Analytics → Risk |
| `risk-recommended-risk-pct` | Kelly % capped at 2% | kelly-criterion | same | Analytics → Risk |
| `risk-of-ruin` | fraction of MC paths hitting ruin threshold | risk-of-ruin | `GET /api/v1/analytics/monte-carlo` | Analytics → Risk |
| `risk-equity-curve-bands` | percentile bands of bootstrapped equity | percentile, monte-carlo | same | Analytics → Risk |
| `risk-percentile` | 5%–95% MDD percentiles from MC table | percentile | drawdown endpoint | Analytics → Risk |
| `risk-ewma-vol` | `vol_t = λ·vol_{t−1} + (1−λ)·\|r_t − r̄_{t−1}\|`, λ=0.94, published as `vol × √(π/2)` (σ-equivalent) | — | `GET /api/v1/analytics/ewma-vol` | Analytics → Risk |
| `risk-correlation-matrix` | pairwise corr of return panel | correlation | `GET /api/v1/analytics/risk/correlation` | Analytics → Risk (Risk Lens) |
| `risk-hhi` | `Σ wᵢ²` | hhi | decomposition | Risk Lens |
| `risk-effective-positions` | Meucci ENB entropy of PCA variance fractions (not DR²) | effective-positions | same | Risk Lens |
| `risk-diversification-multiplier` | Carver-style DM from correlations | diversification-multiplier | same | Risk Lens |
| `risk-contribution` | `RCᵢ = wᵢ (Σw)ᵢ / σₚ` | risk-contribution | `GET /api/v1/analytics/risk/decomposition` | Risk Lens |
| `risk-handcrafted-weights` | grouped / handcrafted weights | handcrafted-weights | same | Risk Lens |
| `risk-marginal-risk` | marginal contribution from `decompose_risk` | marginal-risk | same | Risk Lens |
| `risk-vol-regime` | rolling-vol percentile buckets LOW/NORMAL/HIGH | vol-regime | `GET /api/v1/analytics/risk/regime` | Risk Lens |

### Trade analysis

| ID | Formula summary | Theory | API | GUI |
|----|-----------------|--------|-----|-----|
| `trade-sqn` | `mean(R)/std(R) × √n` (Van Tharp) | sqn | `GET /api/v1/analytics/sqn` | Analytics → Strategies |
| `trade-r-multiples` | `R = P&L / initial_risk` | r-multiples | same | Analytics → Strategies |
| `trade-mfe-mae-excursion` | max favorable / adverse vs entry; capture % | mfe-mae-excursion | `POST /api/v1/analytics/excursion/{id}` | Trades → Excursion |
| `trade-execution-quality` | fill vs NBBO → slippage bps → A–F | nbbo, slippage | `GET /api/v1/analytics/execution-quality` | Trades → Exec Quality |
| `trade-pfof` | PFOF vs DMA routing cost estimate | pfof | `GET /api/v1/analytics/pfof-report` | Trades → PFOF |
| `trade-cost-of-free` | hidden cost of commission-free | pfof | `GET /api/v1/analytics/cost-of-free` | Analytics → Costs |
| `trade-fee-breakdown` | 14-category fee classification | commission | `GET /api/v1/fees/summary` | Trades → Fees |
| `trade-round-trips` | FIFO lot matching open→close | fifo-lifo | `GET /api/v1/round-trips` | Trades → Round Trips |
| `trade-options-strategy` | multi-leg strategy classification | options-strategy | `POST /api/v1/analytics/options-strategy` | Trades → Options |
| `trade-strategy-breakdown` | per-strategy expectancy buckets | expectancy | `GET /api/v1/analytics/strategy-breakdown` | Analytics → Strategies |
| `trade-mistake-classify` | heuristic mistake categories + cost | — | `GET /api/v1/mistakes/summary` | Analytics / Reports |

### Options & quant substrate

| ID | Formula summary | Theory | API | GUI |
|----|-----------------|--------|-----|-----|
| `options-bsm-greeks` | BSM delta/gamma/theta/vega (Hull) | — | options chain routes | Options chain |
| `quant-trial-ledger` | append-only trial records; DSR/MinBTL deferred | — | trials routes | — |

### Tax formula features (oracle-backed)

| ID | Formula summary | Theory | Module status |
|----|-----------------|--------|---------------|
| `tax-st-lt` | LT iff `sell_date` ≥ the **calendar anniversary** of `acquisition_date + 1 day` (IRS Pub. 550) — not a day count | capital-gains-losses | implemented |
| `tax-lot-method` | FIFO/LIFO/HIFO/spec-ID lot selection | fifo-lifo, cost-basis | implemented |
| `tax-same-account-wash` / `tax-cross-account-wash` | §1091 ±30d | wash-sales | implemented |
| `tax-roth-rr2008-5` / `tax-drip-dividend-reinvest` | replacement rules | wash-sales | partial / suggestions |
| `tax-1256-60-40` / `tax-988-fx` | 60/40; FX ordinary | — | **deferred** (no dedicated module). milestone-tracking N-10: the §1256/§475 profile toggles are **inert** — the REST models reject them (422) and the GUI renders them disabled as "Not yet supported", so no estimate can appear to honour an election that is not computed. |
| `tax-niit` | `3.8% × min(NII, max(0, MAGI−threshold))` | — | implemented |
| `tax-carryforward-3k` | net loss − $3k cap → carryforward | capital-gains-losses | implemented |
| `tax-quarterly-estimate` | Pub 505 safe harbor | quarterly-estimates | implemented |

---

## Known drift / validation notes

1. **Oracle `risk-monte-carlo-risk-of-ruin`** still records `no_module: true` in the seed-trade oracle spec, but `risk_of_ruin` and `equity_curve_bands` are implemented in `drawdown.py`. Index status: `drift_suspected` until the oracle row is updated. **Audit verdict 2026-07-26 — `confirmed`, still open.** Re-checked against source: the oracle row still carries `no_module: true` while `drawdown.py` defines both `risk_of_ruin` and `equity_curve_bands`. The note is accurate and must stay until the oracle row is corrected — an oracle generator reading `no_module: true` produces no fixture for two shipped functions.
2. **Oracle `risk-calmar-drawdown`** points at `drawdown.py`; Calmar is computed in `performance.py` via `compute_performance`. Prefer this index’s module path. **Audit verdict 2026-07-26 — `confirmed`, still open.** The token `calmar` occurs 0 times in `drawdown.py` and 11 times in `performance.py`, so preferring this index's module path is correct. The residual defect is that the oracle spec still pairs the stale `drawdown.py` pointer with `no_module: false`, so `perf-calmar-ratio` is queued for oracle validation against a row that resolves to a file with no Calmar code.
3. **Effective positions** use **Meucci ENB**, not diversification-ratio² (see effective-positions and milestone-tracking / `test_metric_definitions_correctness.py`). **Audit verdict 2026-07-26 — `confirmed` accurate, no open drift.** `effective_bets` was read in full and uses the eigenvalue-entropy (Meucci ENB) construction, and `test_metric_definitions_correctness.py` pins it against an independent reference. This note is a clarification worth keeping, not an open defect.
4. **Kelly in product UI** shows full Kelly % from expectancy; **recommended risk** from the drawdown path is Kelly **capped at 2%**. **Audit verdict 2026-07-26 — `confirmed` by design, disclosed (full-Kelly leg: an internal record; capped leg: an internal record coverage record for `risk-recommended-risk-pct`).** Both legs were traced: the two numbers genuinely differ, the 2% cap and the non-positive-Kelly branch behave as documented, and the product discloses the difference in its own Analytics help content. No defect filed against either function on this ground.
5. **DSR / PBO / MinBTL** remain deferred; only trial-ledger substrate + `sharpe_non_annualized` exist for future use. **Audit verdict 2026-07-26 — `confirmed`, still deferred.** `quant/trial.py` defines only `TrialOutcome` and `TrialRecord`; no DSR, PBO or MinBTL estimator exists anywhere in it, and `sharpe_non_annualized` is the only substrate exposed. The note is still true — leave it in place until the estimators ship.

---

## Machine-readable feature block

<!-- VERIFY:math-features -->
```yaml
schema_version: 1
oracle_spec: an internal record
module_root: packages/core/src/zorivest_core
features:
  - id: calc-position-size
    name: Position size (fixed-risk)
    category: calculator
    formula_summary: "share_size = floor((balance * risk_pct/100) / |entry-stop|); R:R = |target-entry|/|entry-stop|"
    theory_doc: null
    authority: "Build plan 06h / Van Tharp fixed fractional risk"
    module: packages/core/src/zorivest_core/domain/calculator.py
    symbol: calculate_position_size
    service: null
    api: POST /api/v1/calculator/position-size
    mcp_action: position_size
    gui: Calculator
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_calculator.py
      - tests/unit/test_payoff_calculator.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain"
    notes: "Equity-mode core; futures/options/forex GUI modes expand via API/output-index §1a–1c."

  - id: calc-vol-target-size
    name: Volatility-target position size
    category: sizing
    formula_summary: "position size scales as target_vol / realized_vol (vol targeting)"
    theory_doc: null
    authority: "Build plan 14-quant-foundation (vol targeting); Carver-style sizing"
    module: packages/core/src/zorivest_core/domain/analytics/position_sizer.py
    symbol: vol_target_size
    service: AnalyticsService
    api: POST /api/v1/calculator/vol-target
    mcp_action: vol_target_size
    gui: Calculator
    status: implemented
    oracle_ref: risk-ewma-volatility-target
    unit_tests:
      - tests/unit/test_vol_target_sizer.py
      - tests/unit/test_vol_target_parity.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on test, surface"
    notes: "Coexists with fixed-risk calc-position-size; not a replacement."

  - id: calc-forward-cost
    name: Forward transaction cost estimate
    category: trade_analysis
    formula_summary: "commission + spread_cost + slippage_cost (+ itemized ForwardCostResult)"
    theory_doc: commission
    authority: "Build plan forward-cost / TCA"
    module: packages/core/src/zorivest_core/domain/analytics/fee_decomposition.py
    symbol: estimate_forward_cost
    service: AnalyticsService
    api: POST /api/v1/calculator/forward-cost
    mcp_action: forward_cost
    gui: Calculator / Costs
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_fee_decomposition.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: trade-expectancy
    name: Expectancy & profit factor
    category: performance
    formula_summary: "expectancy = (WR*AW)-(LR*AL); profit_factor = gross_wins/|gross_losses|; net_edge = expectancy - avg_commission"
    theory_doc: expectancy
    authority: "Van Tharp; Build Plan §13 edge includes commissions"
    module: packages/core/src/zorivest_core/domain/analytics/expectancy.py
    symbol: calculate_expectancy
    service: AnalyticsService
    api: GET /api/v1/analytics/expectancy
    mcp_action: expectancy
    gui: Analytics → Overview
    status: implemented
    oracle_ref: strategy-expectancy-per-bucket-gates
    unit_tests:
      - tests/unit/test_expectancy_edge.py
      - tests/property/test_expectancy_properties.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "Per-strategy buckets also via trade-strategy-breakdown."

  - id: trade-win-rate
    name: Win rate
    category: performance
    formula_summary: "wins / n"
    theory_doc: win-rate
    authority: "Standard trading statistics"
    module: packages/core/src/zorivest_core/domain/analytics/expectancy.py
    symbol: calculate_expectancy
    service: AnalyticsService
    api: GET /api/v1/analytics/expectancy
    mcp_action: expectancy
    gui: Analytics → Overview
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_expectancy_edge.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "Field on ExpectancyResult from same symbol as trade-expectancy."

  - id: trade-kelly-criterion
    name: Kelly criterion
    category: performance
    formula_summary: "kelly = WR - (LR / payoff); recommended_risk_pct = min(kelly*100, 2)"
    theory_doc: kelly-criterion
    authority: "Kelly (1956); product caps recommended risk at 2%"
    module: packages/core/src/zorivest_core/domain/analytics/expectancy.py
    symbol: calculate_expectancy
    service: AnalyticsService
    api: GET /api/v1/analytics/expectancy
    mcp_action: expectancy
    gui: Analytics → Overview
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_expectancy_edge.py
      - tests/unit/test_drawdown.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "UI shows full Kelly %; drawdown recommended_risk uses _compute_recommended_risk cap."

  - id: trade-risk-reward-ratio
    name: Payoff / risk-reward ratio
    category: performance
    formula_summary: "payoff_ratio = avg_win / avg_loss; break_even_win_rate = 1/(1+payoff)"
    theory_doc: risk-reward-ratio
    authority: "Standard payoff ratio"
    module: packages/core/src/zorivest_core/domain/analytics/expectancy.py
    symbol: calculate_expectancy
    service: AnalyticsService
    api: GET /api/v1/analytics/expectancy
    mcp_action: expectancy
    gui: Analytics → Overview
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_expectancy_edge.py
      - tests/unit/test_payoff_calculator.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "Calculator also emits reward_risk_ratio from entry/stop/target."

  - id: perf-sharpe-ratio
    name: Sharpe ratio
    category: performance
    formula_summary: "sharpe = mean(r-rf)/sample_std(r-rf) * sqrt(periods_per_year); also exposes sharpe_non_annualized"
    theory_doc: sharpe-ratio
    authority: "Sharpe 1966/1994; 14-quant-foundation §14.6"
    module: packages/core/src/zorivest_core/domain/analytics/performance.py
    symbol: compute_performance
    service: AnalyticsService
    api: GET /api/v1/analytics/performance
    mcp_action: performance_metrics
    gui: Analytics → Overview
    status: implemented
    oracle_ref: risk-sharpe-sortino
    unit_tests:
      - tests/unit/test_performance_metrics.py
      - tests/unit/test_performance_data_quality.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, test, surface"
    notes: "Default rf=0; periods_per_year default 252."

  - id: perf-sortino-ratio
    name: Sortino ratio
    category: performance
    formula_summary: "sortino = mean(excess) / downside_RMS(excess) * sqrt(periods_per_year)"
    theory_doc: sortino-ratio
    authority: "Sortino; 14-quant-foundation §14.6 (target RMS, OQ-2)"
    module: packages/core/src/zorivest_core/domain/analytics/performance.py
    symbol: compute_performance
    service: AnalyticsService
    api: GET /api/v1/analytics/performance
    mcp_action: performance_metrics
    gui: Analytics → Overview
    status: implemented
    oracle_ref: risk-sharpe-sortino
    unit_tests:
      - tests/unit/test_performance_metrics.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, test, surface"
    notes: ""

  - id: perf-calmar-ratio
    name: Calmar ratio
    category: performance
    formula_summary: "calmar = CAGR / |max_drawdown|"
    theory_doc: calmar-ratio
    authority: "Young 1991; 14-quant-foundation §14.6"
    module: packages/core/src/zorivest_core/domain/analytics/performance.py
    symbol: compute_performance
    service: AnalyticsService
    api: GET /api/v1/analytics/performance
    mcp_action: performance_metrics
    gui: Analytics → Overview
    status: implemented
    oracle_ref: risk-calmar-drawdown
    unit_tests:
      - tests/unit/test_performance_metrics.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, test, surface"
    notes: "Oracle module path drawdown.py is stale; implementation is performance.py."

  - id: perf-tail-ratio
    name: Tail ratio
    category: performance
    formula_summary: "tail_ratio = |P95(returns)| / |P5(returns)|"
    theory_doc: tail-risk
    authority: "14-quant-foundation §14.6"
    module: packages/core/src/zorivest_core/domain/analytics/performance.py
    symbol: compute_performance
    service: AnalyticsService
    api: GET /api/v1/analytics/performance
    mcp_action: performance_metrics
    gui: Analytics → Overview
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_performance_metrics.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, test, surface"
    notes: ""

  - id: perf-max-drawdown
    name: Max drawdown & recovery factor
    category: performance
    formula_summary: "MDD = max peak-to-trough on cumulative equity; recovery = (final-1)/|MDD|"
    theory_doc: drawdown
    authority: "14-quant-foundation §14.6"
    module: packages/core/src/zorivest_core/domain/analytics/performance.py
    symbol: compute_performance
    service: AnalyticsService
    api: GET /api/v1/analytics/performance
    mcp_action: performance_metrics
    gui: Analytics → Overview / Risk
    status: implemented
    oracle_ref: risk-calmar-drawdown
    unit_tests:
      - tests/unit/test_performance_metrics.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on test, surface"
    notes: "Distinct from Monte Carlo drawdown distribution (risk-monte-carlo-drawdown)."

  - id: risk-monte-carlo-drawdown
    name: Monte Carlo drawdown probability table
    category: risk
    formula_summary: "bootstrap trade P&L paths; percentile table of max drawdown (5%-95%)"
    theory_doc: monte-carlo
    authority: "Build plan analytics drawdown; bootstrap MC"
    module: packages/core/src/zorivest_core/domain/analytics/drawdown.py
    symbol: drawdown_probability_table
    service: AnalyticsService
    api: GET /api/v1/analytics/drawdown
    mcp_action: drawdown
    gui: Analytics → Risk
    status: implemented
    oracle_ref: risk-monte-carlo-risk-of-ruin
    unit_tests:
      - tests/unit/test_drawdown.py
      - tests/property/test_drawdown_properties.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "Shares oracle_ref with risk-of-ruin family."

  - id: risk-recommended-risk-pct
    name: Recommended risk percent (Kelly capped)
    category: risk
    formula_summary: "min(kelly_fraction*100, 2); 0 if expectancy<=0"
    theory_doc: kelly-criterion
    authority: "AC-112.5 Kelly capped at 2%"
    module: packages/core/src/zorivest_core/domain/analytics/drawdown.py
    symbol: _compute_recommended_risk
    service: AnalyticsService
    api: GET /api/v1/analytics/drawdown
    mcp_action: drawdown
    gui: Analytics → Risk
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_drawdown.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "Returned as part of DrawdownResult from drawdown_probability_table."

  - id: risk-of-ruin
    name: Risk of ruin (Monte Carlo Plus)
    category: risk
    formula_summary: "P(ruin) = fraction of bootstrap equity paths hitting ruin_threshold from starting_capital"
    theory_doc: risk-of-ruin
    authority: "14-quant-foundation §14.8; milestone-tracking/281"
    module: packages/core/src/zorivest_core/domain/analytics/drawdown.py
    symbol: risk_of_ruin
    service: AnalyticsService
    api: GET /api/v1/analytics/monte-carlo
    mcp_action: monte_carlo_plus
    gui: Analytics → Risk
    status: implemented
    oracle_ref: risk-monte-carlo-risk-of-ruin
    unit_tests:
      - tests/unit/test_monte_carlo_plus.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "Oracle still says no_module:true — update oracle row to drawdown.py."

  - id: risk-equity-curve-bands
    name: Equity-curve percentile bands
    category: risk
    formula_summary: "percentile bands of bootstrapped equity curves"
    theory_doc: percentile
    authority: "14-quant-foundation §14.8"
    module: packages/core/src/zorivest_core/domain/analytics/drawdown.py
    symbol: equity_curve_bands
    service: AnalyticsService
    api: GET /api/v1/analytics/monte-carlo
    mcp_action: monte_carlo_plus
    gui: Analytics → Risk
    status: implemented
    oracle_ref: risk-monte-carlo-risk-of-ruin
    unit_tests:
      - tests/unit/test_monte_carlo_plus.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: risk-percentile
    name: Drawdown percentiles
    category: risk
    formula_summary: "percentile ranks of simulated max drawdowns"
    theory_doc: percentile
    authority: "Order statistics / bootstrap percentiles"
    module: packages/core/src/zorivest_core/domain/analytics/drawdown.py
    symbol: drawdown_probability_table
    service: AnalyticsService
    api: GET /api/v1/analytics/drawdown
    mcp_action: drawdown
    gui: Analytics → Risk
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_drawdown.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: trade-sqn
    name: System Quality Number
    category: trade_analysis
    formula_summary: "SQN = mean(R)/std(R)*sqrt(n); Van Tharp grades"
    theory_doc: sqn
    authority: "Van Tharp"
    module: packages/core/src/zorivest_core/domain/analytics/sqn.py
    symbol: calculate_sqn
    service: AnalyticsService
    api: GET /api/v1/analytics/sqn
    mcp_action: sqn
    gui: Analytics → Strategies
    status: implemented
    oracle_ref: risk-sqn-system-quality
    unit_tests:
      - tests/unit/test_analytics_sqn.py
      - tests/unit/test_unmeasured_risk_surfaces.py
      - tests/unit/test_math_index_tokens.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-08-17; milestone-tracking/432 audit remediation; closed audit finding (degenerate input now withholds sqn/grade under UNDEFINED_SENTINEL_POLICY), audit finding (dollar- vs R-denominated scores no longer share the Van Tharp bands), audit finding (TUI stat strip now binds mean_r/expectancy); STILL drift_suspected on three unclosed findings — audit finding (untyped -> dict response schema, deferred to §20.10), audit finding (_grade_sqn half-open intervals vs the cited discrete band table), audit finding (mock-dominated tests); milestone-tracking 2026-08-23 removed the *reachability* premise — per-trade INITIAL_RISK is persisted and analytics_service.py now supplies the mapping, so R_DENOMINATED is a production outcome rather than an unreachable branch — but it closed none of those three, so validation_status deliberately stays drift_suspected (the milestone-tracking E-20 precedent: flipping it would be the false confidence this index exists to remove); prior stamp Claude Opus 5 2026-07-26 platform-architect feature-validation audit"
    notes: "Requires R-multiples (stops / initial risk). Since milestone-tracking the R-denominated path is reachable in production: per-trade initial risk is persisted for executions a governing plan defines, and AnalyticsService.get_sqn asks for R_DENOMINATED first, falling back to the dollar-denominated score only when the population is not wholly R-measurable. A book with any unplanned fill therefore still receives DOLLAR_DENOMINATED with the Van Tharp bands withheld — that is the fallback, not a defect. See trade-r-multiples."

  - id: trade-r-multiples
    name: R-multiples
    category: trade_analysis
    formula_summary: "R = realized_pnl / initial_risk"
    theory_doc: r-multiples
    authority: "Van Tharp R-multiple framework"
    module: packages/core/src/zorivest_core/domain/analytics/sqn.py
    symbol: calculate_sqn
    service: AnalyticsService
    api: GET /api/v1/analytics/sqn
    mcp_action: sqn
    gui: Analytics → Strategies
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_analytics_sqn.py
      - tests/unit/test_math_index_tokens.py
      - tests/unit/test_trade_initial_risk.py
      - tests/integration/test_sqn_r_denominated.py
    validation_status: unit_covered
    validated_by: "Claude Opus 5; 2026-08-23; milestone-tracking (trade-initial-risk-schema); status moved scaffold → implemented because the condition milestone-tracking set for the flip is met: AnalyticsService.get_sqn builds the exec_id → risk mapping from the projected round trips and passes it to calculate_sqn with units=R_DENOMINATED. validation_status held at unit_covered — the path is pinned by unit and integration tests, but no external oracle was run, and moving it further would claim a check nobody performed. Prior stamp Claude Opus 5 2026-08-16, milestone-tracking audit remediation; audit finding resolved by correcting this entry rather than the code — status moved implemented → scaffold"
    notes: "Implemented since milestone-tracking (2026-08-23), which supplied the two halves milestone-tracking left missing: Trade.initial_risk persists the risk an execution was actually taken with, stamped from the governing plan's entry and stop through ReportService.link_plan_to_trade, and AnalyticsService.get_sqn carries it through project_closed_round_trips into calculate_sqn as the exec_id → risk mapping. R is defined only where a plan governs the fill: an unplanned execution, an option plan, a mismatched ticker or side, or a non-positive risk-per-share all leave initial_risk None rather than inventing a denominator, and a population containing any such trade falls back to the dollar-denominated score with the Van Tharp bands withheld. Consumed as input series inside SQN grading."

  - id: trade-mfe-mae-excursion
    name: MFE / MAE / capture percent
    category: trade_analysis
    formula_summary: "MFE=max favorable vs entry; MAE=max adverse; capture from MFE vs realized"
    theory_doc: mfe-mae-excursion
    authority: "Sweeney 1996 excursion analysis"
    module: packages/core/src/zorivest_core/domain/analytics/excursion.py
    symbol: calculate_mfe_mae
    service: AnalyticsService
    api: "POST /api/v1/analytics/excursion/{trade_exec_id}"
    mcp_action: excursion
    gui: Trades → Excursion
    status: implemented
    oracle_ref: risk-mfe-mae-excursion
    unit_tests:
      - tests/unit/test_excursion.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "Needs OHLCV bar path for hold period."

  - id: trade-execution-quality
    name: Execution quality vs NBBO
    category: trade_analysis
    formula_summary: "slippage_bps from fill vs NBBO; grade A-F"
    theory_doc: nbbo
    authority: "TCA / NBBO mid comparison"
    module: packages/core/src/zorivest_core/domain/analytics/execution_quality.py
    symbol: score_execution
    service: AnalyticsService
    api: GET /api/v1/analytics/execution-quality
    mcp_action: execution_quality
    gui: Trades → Exec Quality
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_execution_quality.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "Also see theory slippage.md."

  - id: trade-pfof
    name: PFOF impact estimate
    category: trade_analysis
    formula_summary: "estimated PFOF cost vs DMA routing comparison"
    theory_doc: pfof
    authority: "Build plan PFOF analysis"
    module: packages/core/src/zorivest_core/domain/analytics/pfof.py
    symbol: analyze_pfof
    service: AnalyticsService
    api: GET /api/v1/analytics/pfof-report
    mcp_action: pfof_impact
    gui: Trades → PFOF
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_pfof.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, test, surface"
    notes: ""

  - id: trade-cost-of-free
    name: Cost of free trading
    category: trade_analysis
    formula_summary: "aggregate hidden costs of commission-free routing"
    theory_doc: pfof
    authority: "Build plan cost-of-free"
    module: packages/core/src/zorivest_core/domain/analytics/cost_of_free.py
    symbol: cost_of_free
    service: AnalyticsService
    api: GET /api/v1/analytics/cost-of-free
    mcp_action: cost_of_free
    gui: Analytics → Costs
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_analytics_cost_of_free.py
      - tests/unit/test_service_cost_of_free.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: trade-fee-breakdown
    name: Fee decomposition
    category: trade_analysis
    formula_summary: "classify fees into commission/regulatory/exchange/clearing/... aggregates"
    theory_doc: commission
    authority: "Build plan fee decomposition"
    module: packages/core/src/zorivest_core/domain/analytics/fee_decomposition.py
    symbol: decompose_fees
    service: AnalyticsService
    api: GET /api/v1/fees/summary
    mcp_action: fee_breakdown
    gui: Trades → Fees
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_fee_decomposition.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "Confirm exact public symbol name if re-exported under alternate alias."

  - id: trade-round-trips
    name: Round-trip matching
    category: trade_analysis
    formula_summary: "match executions into open/closed round trips with realized P&L (lot-aware)"
    theory_doc: fifo-lifo
    authority: "FIFO lot matching (default)"
    module: packages/core/src/zorivest_core/domain/analytics/round_trip_matcher.py
    symbol: match_executions_to_round_trips
    service: TradeService
    api: GET /api/v1/round-trips
    mcp_action: round_trips
    gui: Trades → Round Trips
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_round_trip_matcher.py
      - tests/unit/test_trade_service_round_trips.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, test"
    notes: ""

  - id: trade-options-strategy
    name: Options strategy detection
    category: options
    formula_summary: "classify multi-leg option structures (spreads, iron condor, butterfly, ...)"
    theory_doc: options-strategy
    authority: "Standard options strategy taxonomy"
    module: packages/core/src/zorivest_core/domain/analytics/options_strategy.py
    symbol: detect_options_strategy
    service: AnalyticsService
    api: POST /api/v1/analytics/options-strategy
    mcp_action: options_strategy
    gui: Trades → Options
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_options_strategy.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "See also iron-condor.md."

  - id: trade-strategy-breakdown
    name: Strategy P&L / expectancy buckets
    category: trade_analysis
    formula_summary: "group trades by strategy tag; per-bucket expectancy under sample-size gates"
    theory_doc: expectancy
    authority: "Van Tharp expectancy; sample-size gates"
    module: packages/core/src/zorivest_core/domain/analytics/strategy.py
    symbol: breakdown_by_strategy
    service: AnalyticsService
    api: GET /api/v1/analytics/strategy-breakdown
    mcp_action: strategy_breakdown
    gui: Analytics → Strategies
    status: implemented
    oracle_ref: strategy-expectancy-per-bucket-gates
    unit_tests:
      - tests/unit/test_analytics_service.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, test"
    notes: ""

  - id: trade-mistake-classify
    name: Mistake classification & cost
    category: trade_analysis
    formula_summary: "heuristic categories (revenge, oversized, FOMO, bad R:R, ...); cost aggregation"
    theory_doc: null
    authority: "Build plan behavioral analytics"
    module: packages/core/src/zorivest_core/domain/analytics/classify.py
    symbol: classify_trade_mistake
    service: ReviewService
    api: GET /api/v1/mistakes/summary
    mcp_action: mistake_summary
    gui: Analytics / Reports
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_mistake_repository.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "ai_review MCP action is orchestration, not a pure math formula."

  - id: risk-ewma-vol
    name: EWMA volatility
    category: risk
    formula_summary: "exponential MAD: vol_t = lambda*vol_{t-1} + (1-lambda)*|r_t - mean_prev|, lambda=0.94; published as vol * MAD_TO_SIGMA (sqrt(pi/2)) so the reported figure is a sigma-equivalent, then annualized by sqrt(252)"
    theory_doc: null
    authority: "Carver, Systematic Trading ch.3 (robust EWMA MAD + MAD-to-sigma constant); 14-quant-foundation.md:37 D6 (MAD, explicitly NOT exponential standard deviation); lambda=0.94 retained from RiskMetrics 1996 daily"
    module: packages/core/src/zorivest_core/domain/analytics/volatility.py
    symbol: ewma_vol
    service: AnalyticsService
    api: GET /api/v1/analytics/ewma-vol
    mcp_action: ewma_vol
    gui: Analytics → Risk
    status: implemented
    oracle_ref: risk-ewma-volatility-target
    unit_tests:
      - tests/unit/test_ewma_volatility.py
    validation_status: model_reviewed
    validated_by: "Claude Opus 5; 2026-08-16; milestone-tracking AC-28/AC-29 (audit finding remediation); Q2 direct human approval decision A recorded in an internal record; formula_summary and authority reconciled with volatility.py; tests/unit/test_ewma_volatility.py::TestEwmaVolSigmaCalibration measures the estimator against a population sigma"
    notes: "Also annualized_vol / rolling_vol_percentile in same module. Residual disclosed by AC-28: the sqrt(pi/2) constant is exact only under Gaussianity, so a fat-tailed series at equal true sigma still estimates ~10% lower — pinned by test_cross_distribution_residual_is_pinned_not_claimed_fixed rather than claimed fixed."

  - id: risk-correlation-matrix
    name: Correlation matrix
    category: risk
    formula_summary: "pairwise Pearson correlation of aligned return series"
    theory_doc: correlation
    authority: "Standard correlation; Risk Lens"
    module: packages/core/src/zorivest_core/domain/analytics/portfolio.py
    symbol: correlation_matrix
    service: AnalyticsService
    api: GET /api/v1/analytics/risk/correlation
    mcp_action: correlation_matrix
    gui: Analytics → Risk (Risk Lens)
    status: implemented
    oracle_ref: risk-hhi-concentration-correlation
    unit_tests:
      - tests/unit/domain/analytics/test_portfolio.py
      - tests/unit/services/test_analytics_risk_service.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: risk-hhi
    name: Herfindahl–Hirschman Index
    category: risk
    formula_summary: "HHI = sum(w_i^2)"
    theory_doc: hhi
    authority: "Standard HHI concentration"
    module: packages/core/src/zorivest_core/domain/analytics/portfolio.py
    symbol: hhi
    service: AnalyticsService
    api: GET /api/v1/analytics/risk/decomposition
    mcp_action: decompose_portfolio_risk
    gui: Analytics → Risk (Risk Lens)
    status: implemented
    oracle_ref: risk-hhi-concentration-correlation
    unit_tests:
      - tests/unit/domain/analytics/test_portfolio.py
      - tests/unit/test_metric_definitions_correctness.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: risk-effective-positions
    name: Effective number of bets (Meucci ENB)
    category: risk
    formula_summary: "ENB = exp(-sum p_i ln p_i) where p_i are weight-aware PCA variance fractions; fallback 1/HHI without cov"
    theory_doc: effective-positions
    authority: "Meucci Managing Diversification 2009; 15c C3 (not DR²)"
    module: packages/core/src/zorivest_core/domain/analytics/portfolio.py
    symbol: effective_bets
    service: AnalyticsService
    api: GET /api/v1/analytics/risk/decomposition
    mcp_action: decompose_portfolio_risk
    gui: Analytics → Risk (Risk Lens)
    status: implemented
    oracle_ref: risk-hhi-concentration-correlation
    unit_tests:
      - tests/unit/test_metric_definitions_correctness.py
      - tests/unit/domain/analytics/test_portfolio.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "DR² documented as not implemented; do not validate against diversification-ratio²."

  - id: risk-diversification-multiplier
    name: Diversification multiplier
    category: risk
    formula_summary: "Carver-style DM from portfolio correlations / grouped weights"
    theory_doc: diversification-multiplier
    authority: "Carver; 15c risk-lens correctness"
    module: packages/core/src/zorivest_core/domain/analytics/portfolio.py
    symbol: diversification_multiplier
    service: AnalyticsService
    api: GET /api/v1/analytics/risk/decomposition
    mcp_action: decompose_portfolio_risk
    gui: Analytics → Risk (Risk Lens)
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_metric_definitions_correctness.py
      - tests/unit/domain/analytics/test_portfolio.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: risk-contribution
    name: Euler risk contribution
    category: risk
    formula_summary: "RC_i = w_i * (Sigma w)_i / sigma_p; sum RC = sigma_p"
    theory_doc: risk-contribution
    authority: "Euler decomposition"
    module: packages/core/src/zorivest_core/domain/analytics/portfolio.py
    symbol: decompose_risk
    service: AnalyticsService
    api: GET /api/v1/analytics/risk/decomposition
    mcp_action: decompose_portfolio_risk
    gui: Analytics → Risk (Risk Lens)
    status: implemented
    oracle_ref: risk-contribution-decomposition
    unit_tests:
      - tests/unit/test_risk_decomposition_correctness.py
      - tests/unit/test_risk_decomposition_correctness_service.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: risk-handcrafted-weights
    name: Handcrafted / grouped weights
    category: risk
    formula_summary: "handcrafted_weights(...) builds grouped portfolio weights for Risk Lens"
    theory_doc: handcrafted-weights
    authority: "15c risk-lens correctness (handcrafted weights)"
    module: packages/core/src/zorivest_core/domain/analytics/portfolio.py
    symbol: handcrafted_weights
    service: AnalyticsService
    api: GET /api/v1/analytics/risk/decomposition
    mcp_action: decompose_portfolio_risk
    gui: Analytics → Risk (Risk Lens)
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_metric_definitions_correctness.py
      - tests/unit/domain/analytics/test_portfolio.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: risk-marginal-risk
    name: Marginal risk
    category: risk
    formula_summary: "marginal contribution extracted from decompose_risk / PortfolioRiskResult"
    theory_doc: marginal-risk
    authority: "Euler marginal risk"
    module: packages/core/src/zorivest_core/domain/analytics/portfolio.py
    symbol: decompose_risk
    service: AnalyticsService
    api: GET /api/v1/analytics/risk/decomposition
    mcp_action: decompose_portfolio_risk
    gui: Analytics → Risk (Risk Lens)
    status: implemented
    oracle_ref: risk-contribution-decomposition
    unit_tests:
      - tests/unit/test_risk_decomposition_correctness.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: ""

  - id: risk-vol-regime
    name: Volatility regime classification
    category: risk
    formula_summary: "bucket rolling realized vol vs historical percentiles → LOW/NORMAL/HIGH (+ crisis)"
    theory_doc: vol-regime
    authority: "15-quant-risk-lens regime classification"
    module: packages/core/src/zorivest_core/domain/analytics/regime.py
    symbol: classify_vol_regime
    service: AnalyticsService
    api: GET /api/v1/analytics/risk/regime
    mcp_action: regime_state
    gui: Analytics → Risk (Risk Lens)
    status: implemented
    oracle_ref: risk-regime-classification
    unit_tests:
      - tests/unit/domain/analytics/test_regime.py
      - tests/unit/test_regime_correctness.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, surface"
    notes: "Also correlation_regime_warning in same module."

  - id: options-bsm-greeks
    name: Black–Scholes–Merton Greeks
    category: options
    formula_summary: "vectorized BSM delta, gamma, theta (per day), vega (per 1% IV), p_itm"
    theory_doc: null
    authority: "Hull Options Futures and Other Derivatives §15.4; build plan 08b"
    module: packages/core/src/zorivest_core/options/greeks.py
    symbol: black_scholes_greeks
    service: OptionsService
    api: options chain routes
    mcp_action: null
    gui: Options chain
    status: implemented
    oracle_ref: none
    unit_tests:
      - tests/unit/test_greeks.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "NaN when T<=0 or IV<=0."

  - id: quant-trial-ledger
    name: Strategy trial ledger
    category: quant
    formula_summary: "append-only trial records; substrate for deferred DSR/MinBTL"
    theory_doc: null
    authority: "14-quant-foundation D7 trial ledger"
    module: packages/core/src/zorivest_core/domain/quant/trial.py
    symbol: trial record types
    service: null
    api: trials routes (/api/v1 trials)
    mcp_action: record_trial
    gui: null
    status: scaffold
    oracle_ref: none
    unit_tests:
      - tests/unit/test_trial_ledger.py
      - tests/integration/test_trial_repository.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on index, test"
    notes: "DSR/PBO/MinBTL deferred; list_trials is the read side. Not a completed math validator."

  - id: tax-st-lt
    name: Short-term / long-term holding period
    category: tax
    formula_summary: "long-term iff disposal_date >= one-year calendar anniversary of acquisition_date + 1 day; 29 Feb has no anniversary in a non-leap year, so 1 Mar is used (Rev. Rul. 66-7). NOT a day count — the 365/366-day form misclassified every leap-year-spanning holding."
    theory_doc: capital-gains-losses
    authority: "IRS Pub 550; IRC §1223; Rev. Rul. 66-7"
    module: packages/core/src/zorivest_core/domain/tax/holding_period.py
    symbol: is_long_term_holding
    service: TaxService
    api: tax routes
    mcp_action: null
    gui: Tax
    status: implemented
    oracle_ref: tax-st-lt
    unit_tests:
      - tests/unit/domain/tax/test_holding_period.py
      - tests/unit/domain/tax/test_holding_period_delegation.py
      - tests/unit/test_gains_calculator.py
    validation_status: unit_covered
    validated_by: "Claude Opus 5; 2026-08-03; milestone-tracking audit remediation; audit finding remediated — day-count classification replaced by the calendar-anniversary rule at all call sites; holding-period unit tests pin the calendar-anniversary rule"
    notes: "Day-count classification (holding_period_days >= 366) was wrong at seven call sites; all now route through tax.holding_period.is_long_term_holding."

  - id: tax-lot-method
    name: Cost-basis lot selection
    category: tax
    formula_summary: "FIFO/LIFO/HIFO/spec-ID select_lots_for_closing"
    theory_doc: fifo-lifo
    authority: "IRS Pub 550; IRC §1012"
    module: packages/core/src/zorivest_core/domain/tax/lot_selector.py
    symbol: select_lots_for_closing
    service: TaxService
    api: tax routes
    mcp_action: null
    gui: Tax → Lots
    status: implemented
    oracle_ref: tax-lot-method-fifo-lifo-hifo-specid
    unit_tests:
      - tests/unit/test_lot_selector.py
      - tests/unit/domain/tax/test_lot_matcher.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "Also cost-basis.md."

  - id: tax-same-account-wash
    name: Same-account wash sale
    category: tax
    formula_summary: "§1091 disallowance when repurchase within ±30 days; basis deferral"
    theory_doc: wash-sales
    authority: "IRC §1091; IRS Pub 550"
    module: packages/core/src/zorivest_core/domain/tax/wash_sale_detector.py
    symbol: wash sale detect
    service: TaxService
    api: tax routes
    mcp_action: null
    gui: Tax → Wash Sales
    status: implemented
    oracle_ref: tax-same-account-wash
    unit_tests:
      - tests/unit/domain/tax/test_wash_sale_detector.py
      - tests/unit/domain/tax/test_wash_sale.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: ""

  - id: tax-cross-account-wash
    name: Cross-account wash sale
    category: tax
    formula_summary: "§1091 across taxpayer accounts including IRA replacements (RR 2008-5 interaction)"
    theory_doc: wash-sales
    authority: "IRC §1091; Rev. Rul. 2008-5"
    module: packages/core/src/zorivest_core/domain/tax/wash_sale_detector.py
    symbol: wash sale detect
    service: TaxService
    api: tax routes
    mcp_action: null
    gui: Tax → Wash Sales
    status: implemented
    oracle_ref: tax-cross-account-wash
    unit_tests:
      - tests/unit/domain/tax/test_wash_sale_cross_account.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: ""

  - id: tax-roth-rr2008-5
    name: Roth IRA replacement wash
    category: tax
    formula_summary: "taxable loss + Roth repurchase → permanent disallowance, no IRA basis add-back"
    theory_doc: wash-sales
    authority: "Rev. Rul. 2008-5"
    module: packages/core/src/zorivest_core/domain/tax/replacement_suggestions.py
    symbol: suggest_replacements
    service: TaxService
    api: tax routes
    mcp_action: null
    gui: Tax
    status: implemented
    oracle_ref: tax-roth-rr2008-5
    unit_tests:
      - tests/unit/domain/tax/test_wash_sale_detector.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "Oracle module is replacement_suggestions; detector enforces wash math."

  - id: tax-drip-dividend-reinvest
    name: DRIP as wash replacement
    category: tax
    formula_summary: "reinvested dividend lots can trigger §1091 within window"
    theory_doc: wash-sales
    authority: "IRC §1091; IRS Pub 550"
    module: packages/core/src/zorivest_core/domain/tax/replacement_suggestions.py
    symbol: suggest_replacements
    service: TaxService
    api: tax routes
    mcp_action: null
    gui: Tax
    status: implemented
    oracle_ref: tax-drip-dividend-reinvest
    unit_tests: []
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index, test"
    notes: "Limited unit coverage; prefer oracle when substrate exists."

  - id: tax-1256-60-40
    name: Section 1256 60/40
    category: tax
    formula_summary: "gain = 0.60*LT + 0.40*ST MTM at year-end"
    theory_doc: null
    authority: "IRC §1256; IRS Pub 550"
    module: null
    symbol: null
    service: null
    api: null
    mcp_action: null
    gui: null
    status: deferred
    oracle_ref: tax-1256-60-40
    unit_tests: []
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index, test"
    notes: "Oracle no_module:true — formula documented for future implementation. milestone-tracking N-10: the profile toggle is inert and no longer settable — POST/PUT reject `section_1256_eligible` with 422 and the GUI shows it disabled as 'Not yet supported'. The ORM column is retained so a future implementation has an election to read."

  - id: tax-988-fx
    name: Section 988 FX gain/loss
    category: tax
    formula_summary: "fx_gain = (disposal_rate - acquisition_rate)*units as ordinary income"
    theory_doc: null
    authority: "IRC §988"
    module: null
    symbol: null
    service: null
    api: null
    mcp_action: null
    gui: null
    status: deferred
    oracle_ref: tax-988-fx
    unit_tests: []
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index, test"
    notes: "Oracle no_module:true."

  - id: tax-niit
    name: Net Investment Income Tax
    category: tax
    formula_summary: "niit = 0.038 * min(NII, max(0, MAGI - threshold))"
    theory_doc: null
    authority: "IRC §1411"
    module: packages/core/src/zorivest_core/domain/tax/niit.py
    symbol: compute_niit
    service: TaxService
    api: tax routes
    mcp_action: null
    gui: Tax
    status: implemented
    oracle_ref: tax-niit
    unit_tests:
      - tests/unit/test_tax_niit.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "milestone-tracking repaired the YTD/estimate path: §1(h) preferential rates now price long-term gain and the §1411 threshold test reads MAGI *including* the realized gain. Known remaining gap: `POST /api/v1/tax/simulate` (`TaxService.simulate_impact`) still prices at the blended ordinary rate with no NIIT — the audit census did not enumerate that surface; tracked as a milestone-tracking follow-up."

  - id: tax-carryforward-3k
    name: Capital loss carryforward ($3k cap)
    category: tax
    formula_summary: "carryforward = max(0, net_capital_loss - 3000) after year netting"
    theory_doc: capital-gains-losses
    authority: "IRC §1211(b)/§1212; IRS Pub 550"
    module: packages/core/src/zorivest_core/domain/tax/loss_carryforward.py
    symbol: apply_capital_loss_rules
    service: TaxService
    api: tax routes
    mcp_action: null
    gui: Tax
    status: implemented
    oracle_ref: tax-carryforward-3k
    unit_tests:
      - tests/unit/test_loss_carryforward.py
      - tests/unit/test_loss_carryforward_parameterized.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index"
    notes: "milestone-tracking: the profile now stores one balance per character (§1212(b)(1)(A)/(B)) and each nets only against gains of its own kind, and the unused remainder is persisted onto the year's profile and seeded into the next year's on create instead of being computed and discarded."

  - id: tax-quarterly-estimate
    name: Quarterly estimated-tax safe harbor
    category: tax
    formula_summary: "required = min(90% current, prior_year_pct * prior); farming/fishing exception per §6654(i)"
    theory_doc: quarterly-estimates
    authority: "IRS Pub 505; IRC §6654"
    module: packages/core/src/zorivest_core/domain/tax/quarterly.py
    symbol: compute_safe_harbor
    service: TaxService
    api: tax routes
    mcp_action: null
    gui: Tax → Quarterly
    status: implemented
    oracle_ref: tax-quarterly-estimate-near-miss
    unit_tests:
      - tests/unit/test_tax_quarterly.py
    validation_status: drift_suspected
    validated_by: "Claude Opus 5; 2026-07-26; platform-architect feature-validation audit; flagged on domain, index, test"
    notes: "milestone-tracking repaired the §6654 ladder: true quarterly periods, the §6654(d)(1)(C)(i) 110% prior-year test keyed to prior-year AGI, an unknown prior-year tax left NULL (never read as zero), and payment dates recorded so a late payment is not credited as timely."
```

---

## MCP analytics action coverage

Live `zorivest_analytics` compute/read actions (2026-07-25) mapped into this index:

| MCP action | Feature id(s) |
|------------|-----------------|
| `position_size` | `calc-position-size` |
| `vol_target_size` | `calc-vol-target-size` |
| `forward_cost` | `calc-forward-cost` |
| `expectancy` | `trade-expectancy`, `trade-win-rate`, `trade-kelly-criterion`, `trade-risk-reward-ratio` |
| `performance_metrics` | `perf-sharpe-ratio`, `perf-sortino-ratio`, `perf-calmar-ratio`, `perf-tail-ratio`, `perf-max-drawdown` |
| `drawdown` | `risk-monte-carlo-drawdown`, `risk-recommended-risk-pct`, `risk-percentile` |
| `monte_carlo_plus` | `risk-of-ruin`, `risk-equity-curve-bands` |
| `sqn` | `trade-sqn`, `trade-r-multiples` |
| `excursion` | `trade-mfe-mae-excursion` |
| `execution_quality` | `trade-execution-quality` |
| `pfof_impact` | `trade-pfof` |
| `cost_of_free` | `trade-cost-of-free` |
| `fee_breakdown` | `trade-fee-breakdown` |
| `round_trips` | `trade-round-trips` |
| `options_strategy` | `trade-options-strategy` |
| `strategy_breakdown` | `trade-strategy-breakdown` |
| `mistake_summary` | `trade-mistake-classify` |
| `ewma_vol` | `risk-ewma-vol` |
| `correlation_matrix` | `risk-correlation-matrix` |
| `decompose_portfolio_risk` | `risk-hhi`, `risk-effective-positions`, `risk-diversification-multiplier`, `risk-contribution`, `risk-handcrafted-weights`, `risk-marginal-risk` |
| `regime_state` | `risk-vol-regime` |
| `record_trial` / `list_trials` | `quant-trial-ledger` |
| `ai_review` | *(orchestration — not a pure math feature row)* |

---

## How models should use this index

1. Select a feature row by `id` or `mcp_action`.
2. Read `formula_summary` + `authority` + `theory_doc` (theory only — do not treat theory prose as code truth).
3. Open `module` / `symbol` and compare implementation to the formula and oracle (when `oracle_ref != none`).
4. Run `unit_tests` first; for oracle_ready rows, prefer independent expected values from the oracle spec (never compute the oracle with Zorivest).
5. On completion, set `validation_status` to `model_reviewed` (or leave `drift_suspected` if mismatch) and fill `validated_by` with model + date + evidence path.
