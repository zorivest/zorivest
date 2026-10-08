# Reference Docs Index
<!--
  MAINTAINER NOTE: This file is the single source of truth for all reference docs
  and their usage locations in the Zorivest GUI. It is designed to be machine-parseable
  for automated updates via skills and workflows.

  FORMAT: Each entry follows a strict YAML-in-markdown pattern:
    - slug: filename without .md extension
    - title: human-readable title
    - used_by: list of { file, feature, field, term } objects

  TO ADD A NEW DOC:
    1. Create the .md file in this directory
    2. Append a new entry to the table below
    3. Add a usage record under ## Usage Map

  TO UPDATE VIA SKILL/WORKFLOW:
    - Grep for `deep_link:`, `deepLink:`, or `learnMoreUrl:` in ui/src/
    - Parse the slug from the GitHub URL
    - Reconcile against this index
    - Flag any orphaned docs (in index but not referenced) or missing docs (referenced but not in index)

  CODE / VALIDATION COMPANION:
    For math features (formulas ↔ domain modules ↔ API/MCP ↔ oracles ↔ tests),
    see math-features-index.md — this INDEX tracks theory ↔ GUI deep-links only.
-->

> **Math / validation companion:** math-features-index.md maps theory formulas to domain code, surfaces, oracles, and validation status.
>
> **Complex ops / validation companion:** complex-ops-index.md maps non-math operational logic (AI review, tax audit, scheduling/templates, import, security) for advanced-model review.

## Quick Reference

| # | Slug | Title | Used In |
|---|------|-------|---------|
| 1 | `mfe-mae-excursion` | MFE / MAE / Capture % | Trades → Excursion |
| 2 | `pfof` | Payment for Order Flow | Trades → PFOF, Analytics → Costs |
| 3 | `drawdown` | Drawdown & Recovery | Analytics → Overview, Analytics → Risk |
| 4 | `risk-reward-ratio` | Risk–Reward & Payoff Ratio | Analytics → Overview |
| 5 | `expectancy` | Expectancy & Profit Factor | Analytics → Overview |
| 6 | `sharpe-ratio` | Sharpe Ratio | Analytics → Overview |
| 7 | `sortino-ratio` | Sortino Ratio | Analytics → Overview |
| 8 | `calmar-ratio` | Calmar Ratio | Analytics → Overview |
| 9 | `tail-risk` | Tail Risk & Tail Ratio | Analytics → Overview |
| 10 | `win-rate` | Win Rate | Analytics → Overview |
| 11 | `kelly-criterion` | Kelly Criterion | Analytics → Overview |
| 12 | `monte-carlo` | Monte Carlo Simulation | Analytics → Risk |
| 13 | `percentile` | Percentiles | Analytics → Risk |
| 14 | `risk-of-ruin` | Risk of Ruin | Analytics → Risk |
| 15 | `sqn` | System Quality Number | Analytics → Strategies |
| 16 | `r-multiples` | R-Multiples | Analytics → Strategies |
| 17 | `commission` | Commission | Trades → Fees |
| 18 | `nbbo` | NBBO | Trades → Exec Quality |
| 19 | `slippage` | Slippage | Trades → Exec Quality |
| 20 | `direct-market-access` | Direct Market Access | Trades → PFOF |
| 21 | `notional-amount` | Notional Amount | Trades → PFOF |
| 22 | `fifo-lifo` | FIFO / LIFO | Trades → Round Trips |
| 23 | `options-strategy` | Options Strategies | Trades → Options |
| 24 | `iron-condor` | Iron Condor | Trades → Options |
| 25 | `irs-pub-550` | IRS Publication 550 | Tax → Dashboard |
| 26 | `irs-tax-brackets` | Federal Tax Brackets | Tax → Profiles, Tax → Schedules |
| 27 | `cost-basis` | Cost Basis | Tax → Lots |
| 28 | `wash-sales` | Wash Sales | Tax → Wash Sales |
| 29 | `capital-gains-losses` | Capital Gains & Losses | Tax → Simulator, Tax → Harvesting |
| 30 | `quarterly-estimates` | Quarterly Estimated Tax | Tax → Quarterly |
| 31 | `basis-of-assets` | Basis of Assets | Tax → Audit |
| 32 | `cron-expressions` | Cron Expressions | Scheduling → Policies |
| 33 | `jinja2-templates` | Jinja2 Templates | Scheduling → Templates |
| 34 | `correlation` | Correlation & Correlation Matrix | Analytics → Risk |
| 35 | `hhi` | Herfindahl–Hirschman Index | Analytics → Risk |
| 36 | `effective-positions` | Effective Positions | Analytics → Risk |
| 37 | `diversification-multiplier` | Diversification Multiplier | Analytics → Risk |
| 38 | `vol-regime` | Volatility Regime | Analytics → Risk |
| 39 | `risk-contribution` | Risk Contribution | Analytics → Risk |
| 40 | `handcrafted-weights` | Handcrafted Weights | Analytics → Risk |
| 41 | `marginal-risk` | Marginal Risk | Analytics → Risk |
| 42 | `ai-search-steps` | AI Search & Generate Steps | Scheduling → Policies |

---

## Usage Map

<!-- Each entry documents exactly where the doc is referenced in the codebase.
     Fields:
       - file: relative path from repo root to the source file containing the link
       - feature: GUI feature area (trades, analytics, tax, scheduling, planning)
       - field: the property name holding the URL (deep_link, deepLink, learnMoreUrl)
       - term: the UI label shown to the user
       - tab: the tab or sub-section within the feature
-->

### mfe-mae-excursion

- **Title:** MFE / MAE / Capture %
- **File:** mfe-mae-excursion.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/trades/trade-tab-explainers.ts` | trades | excursion | `deep_link` | MFE (Max Favorable Excursion) |
| `ui/src/renderer/src/features/trades/trade-tab-explainers.ts` | trades | excursion | `deep_link` | MAE (Max Adverse Excursion) |
| `ui/src/renderer/src/features/trades/trade-tab-explainers.ts` | trades | excursion | `deep_link` | Capture % (was "BSO") |

---

### pfof

- **Title:** Payment for Order Flow
- **File:** pfof.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/trades/trade-tab-explainers.ts` | trades | pfof | `deep_link` | PFOF (Payment for Order Flow) |
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | costs | `deepLink` | Cost of Free |
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | costs | `deepLink` | PFOF (Payment for Order Flow) |

---

### drawdown

- **Title:** Drawdown & Recovery Factor
- **File:** drawdown.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | overview | `deepLink` | Max Drawdown (Max DD) |
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | overview | `deepLink` | Recovery Factor |
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | Drawdown |

---

### risk-reward-ratio

- **Title:** Risk–Reward Ratio & Payoff Ratio
- **File:** risk-reward-ratio.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | overview | `deepLink` | Avg Win / Avg Loss |
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | overview | `deepLink` | Payoff Ratio |

---

### expectancy

- **Title:** Expectancy & Profit Factor
- **File:** expectancy.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | overview | `deepLink` | Expectancy |
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | overview | `deepLink` | Profit Factor |

---

### sharpe-ratio

- **Title:** Sharpe Ratio
- **File:** sharpe-ratio.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | overview | `deepLink` | Sharpe Ratio |

---

### sortino-ratio

- **Title:** Sortino Ratio
- **File:** sortino-ratio.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | overview | `deepLink` | Sortino Ratio |

---

### calmar-ratio

- **Title:** Calmar Ratio
- **File:** calmar-ratio.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | overview | `deepLink` | Calmar Ratio |

---

### tail-risk

- **Title:** Tail Risk & Tail Ratio
- **File:** tail-risk.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | overview | `deepLink` | Tail Ratio |

---

### win-rate

- **Title:** Win Rate
- **File:** win-rate.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | overview | `deepLink` | Win Rate |

---

### kelly-criterion

- **Title:** Kelly Criterion
- **File:** kelly-criterion.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | overview | `deepLink` | Kelly % |

---

### monte-carlo

- **Title:** Monte Carlo Simulation
- **File:** monte-carlo.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | Monte Carlo |

---

### percentile

- **Title:** Percentiles
- **File:** percentile.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | Percentile (5% / 50% / 95%) |

---

### risk-of-ruin

- **Title:** Risk of Ruin
- **File:** risk-of-ruin.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | Recommended Risk % |

---

### sqn

- **Title:** System Quality Number (SQN)
- **File:** sqn.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | strategies | `deepLink` | SQN (System Quality Number) |

---

### r-multiples

- **Title:** R-Multiples
- **File:** r-multiples.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | strategies | `deepLink` | R (and mean R / std R) |

---

### commission

- **Title:** Commission
- **File:** commission.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/trades/trade-tab-explainers.ts` | trades | fees | `deep_link` | Commission |

---

### nbbo

- **Title:** National Best Bid and Offer
- **File:** nbbo.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/trades/trade-tab-explainers.ts` | trades | exec_quality | `deep_link` | NBBO |

---

### slippage

- **Title:** Slippage
- **File:** slippage.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/trades/trade-tab-explainers.ts` | trades | exec_quality | `deep_link` | Slippage (bps) |

---

### direct-market-access

- **Title:** Direct Market Access (DMA)
- **File:** direct-market-access.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/trades/trade-tab-explainers.ts` | trades | pfof | `deep_link` | DMA (Direct Market Access) |

---

### notional-amount

- **Title:** Notional Amount
- **File:** notional-amount.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/trades/trade-tab-explainers.ts` | trades | pfof | `deep_link` | Notional |

---

### fifo-lifo

- **Title:** FIFO & LIFO Cost Basis Methods
- **File:** fifo-lifo.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/trades/trade-tab-explainers.ts` | trades | round_trips | `deep_link` | FIFO |

---

### options-strategy

- **Title:** Options Strategies
- **File:** options-strategy.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/trades/trade-tab-explainers.ts` | trades | options | `deep_link` | Legs |

---

### iron-condor

- **Title:** Iron Condor
- **File:** iron-condor.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/trades/trade-tab-explainers.ts` | trades | options | `deep_link` | Iron Condor / Vertical / etc. |

---

### irs-pub-550

- **Title:** IRS Publication 550
- **File:** irs-pub-550.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/tax/tax-help-content.ts` | tax | dashboard | `learnMoreUrl` | IRS Publication 550 — Investment Income and Expenses |

---

### irs-tax-brackets

- **Title:** Federal Tax Brackets & Rate Schedules
- **File:** irs-tax-brackets.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/tax/tax-help-content.ts` | tax | profiles | `learnMoreUrl` | IRS Tax Rate Schedules |
| `ui/src/renderer/src/features/tax/tax-help-content.ts` | tax | schedules | `learnMoreUrl` | IRS Revenue Procedure — Tax Bracket Adjustments |

---

### cost-basis

- **Title:** Cost Basis
- **File:** cost-basis.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/tax/tax-help-content.ts` | tax | lots | `learnMoreUrl` | IRS Publication 550 §4 — Basis of Investment Property |

---

### wash-sales

- **Title:** Wash Sales
- **File:** wash-sales.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/tax/tax-help-content.ts` | tax | wash-sales | `learnMoreUrl` | IRS Publication 550 §4 — Wash Sales |

---

### capital-gains-losses

- **Title:** Capital Gains & Losses
- **File:** capital-gains-losses.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/tax/tax-help-content.ts` | tax | simulator | `learnMoreUrl` | IRS Topic No. 409 — Capital Gains and Losses |
| `ui/src/renderer/src/features/tax/tax-help-content.ts` | tax | harvesting | `learnMoreUrl` | IRS Topic No. 409 — Capital Gains and Losses |

---

### quarterly-estimates

- **Title:** Quarterly Estimated Tax Payments
- **File:** quarterly-estimates.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/tax/tax-help-content.ts` | tax | quarterly | `learnMoreUrl` | IRS Form 1040-ES — Estimated Tax for Individuals |

---

### basis-of-assets

- **Title:** Basis of Assets (IRS Publication 551)
- **File:** basis-of-assets.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/tax/tax-help-content.ts` | tax | audit | `learnMoreUrl` | IRS Publication 551 — Basis of Assets |

---

### cron-expressions

- **Title:** Cron Expressions
- **File:** cron-expressions.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/scheduling/scheduling-help-content.ts` | scheduling | policies | `learnMoreUrl` | Cron Expression Reference — crontab.guru |

---

### jinja2-templates

- **Title:** Jinja2 Templates
- **File:** jinja2-templates.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/scheduling/scheduling-help-content.ts` | scheduling | templates | `learnMoreUrl` | Jinja2 Template Syntax — Official Documentation |

---

### correlation

- **Title:** Correlation & Correlation Matrix
- **File:** correlation.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | Correlation |
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | Correlation Matrix |

---

### hhi

- **Title:** Herfindahl–Hirschman Index (HHI)
- **File:** hhi.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | HHI (Herfindahl-Hirschman Index) |

---

### effective-positions

- **Title:** Effective Positions
- **File:** effective-positions.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | Effective Positions |

---

### diversification-multiplier

- **Title:** Diversification Multiplier
- **File:** diversification-multiplier.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | Diversification Multiplier |

---

### vol-regime

- **Title:** Volatility Regime
- **File:** vol-regime.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | Vol Regime |

---

### risk-contribution

- **Title:** Risk Contribution
- **File:** risk-contribution.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | Risk Contribution |

---

### handcrafted-weights

- **Title:** Handcrafted Weights
- **File:** handcrafted-weights.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | Handcrafted Weights |

---

### marginal-risk

- **Title:** Marginal Risk
- **File:** marginal-risk.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/analytics/analytics-tab-help-content.ts` | analytics | risk | `deepLink` | Marginal Risk |

---

### ai-search-steps

- **Title:** AI Search & Generate Pipeline Steps
- **File:** ai-search-steps.md
- **Usages:**

| Source File | Feature | Tab | Field | Term |
|------------|---------|-----|-------|------|
| `ui/src/renderer/src/features/scheduling/scheduling-help-content.ts` | scheduling | policies | `learnMoreUrl` | AI Search & Generate Steps |
| `ui/src/renderer/src/features/scheduling/scheduling-help-content.ts` | scheduling | templates | `learnMoreUrl` | AI Research Digest Template |

---

## Reconciliation Commands

```bash
# Find all reference links in the UI codebase
grep -rn "docs/reference/" ui/src/renderer/src/features/ --include="*.ts" --include="*.tsx"

# Find orphaned docs (in directory but not in any UI file)
for f in docs/reference/*.md; do
  slug=$(basename "$f" .md)
  if [ "$slug" != "INDEX" ]; then
    grep -rq "$slug" ui/src/renderer/src/features/ || echo "ORPHANED: $slug"
  fi
done

# Find missing docs (referenced in UI but file doesn't exist)
grep -roh "docs/reference/[a-z0-9-]*\.md" ui/src/ | sort -u | while read ref; do
  [ -f "$ref" ] || echo "MISSING: $ref"
done
```

---

<!-- LAST UPDATED: 2026-07-04 by session p16-meu350 -->
<!-- TOTAL DOCS: 42 | TOTAL USAGES: 52 -->
