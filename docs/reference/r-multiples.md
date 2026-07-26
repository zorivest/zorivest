# R-Multiples

> A standardised way to express trade outcomes as multiples of the initial risk.
> Concept popularised by Dr. Van K. Tharp (1946–2022).

---

## Core Idea

Every trade has an **initial risk** — the dollar amount you would lose if your stop-loss is hit. That amount is called **1R**. All trade outcomes are then expressed as multiples of that risk.

- A **+2R** trade made **twice** what you risked.
- A **−1R** trade lost **exactly** what you risked (a standard stop-out).
- A **+0.5R** trade made half of what you risked.

This normalisation lets you compare trades across different instruments, position sizes, and account balances on a single scale.

---

## Formula

### Step 1 — Define Initial Risk (1R)

$$
1R = (\text{Entry Price} - \text{Stop-Loss Price}) \times \text{Position Size}
$$

For a short trade, reverse the subtraction: Stop-Loss − Entry.

### Step 2 — Calculate the R-Multiple

$$
R\text{-Multiple} = \frac{\text{Actual Profit or Loss}}{1R}
$$

---

## Worked Example

| Detail | Value |
|--------|-------|
| Entry price | $100 |
| Stop-loss | $95 |
| Position size | 100 shares |
| **1R (initial risk)** | ($100 − $95) × 100 = **$500** |

### Outcome A — Profitable exit at $110

$$
\text{Profit} = (\$110 - \$100) \times 100 = \$1{,}000
$$

$$
R\text{-Multiple} = \frac{\$1{,}000}{\$500} = +2R
$$

### Outcome B — Stop-loss hit at $95

$$
\text{Loss} = (\$95 - \$100) \times 100 = -\$500
$$

$$
R\text{-Multiple} = \frac{-\$500}{\$500} = -1R
$$

### Outcome C — Partial loss, exit at $97

$$
\text{Loss} = (\$97 - \$100) \times 100 = -\$300
$$

$$
R\text{-Multiple} = \frac{-\$300}{\$500} = -0.6R
$$

---

## Mean R (Average R-Multiple)

The **mean R** (or **expectancy in R**) is the average R-multiple across all your trades:

$$
\overline{R} = \frac{1}{N} \sum_{i=1}^{N} R_i
$$

Where *N* is the number of trades and *R_i* is the R-multiple of trade *i*.

**Interpretation:**
- Mean R > 0 → positive edge (on average, you make money)
- Mean R < 0 → negative edge (on average, you lose money)
- Mean R = 0 → break-even

### Example

| Trade | R-Multiple |
|-------|-----------|
| 1 | +2.0 |
| 2 | −1.0 |
| 3 | +1.5 |
| 4 | −1.0 |
| 5 | +3.0 |

$$
\overline{R} = \frac{2.0 + (-1.0) + 1.5 + (-1.0) + 3.0}{5} = \frac{4.5}{5} = +0.90R
$$

On average, each trade returns **0.9× the initial risk**. This is a positive edge.

---

## Std R (Standard Deviation of R-Multiples)

The **standard deviation of R** measures how spread out (bumpy / inconsistent) your trade results are:

$$
\sigma_R = \sqrt{\frac{\sum_{i=1}^{N} (R_i - \overline{R})^2}{N - 1}}
$$

**Interpretation:**
- Low σ_R → consistent results (tight distribution around the mean)
- High σ_R → volatile results (wide swings between big winners and big losers)

Using the same 5 trades:

$$
\sigma_R = \sqrt{\frac{(2.0-0.9)^2 + (-1.0-0.9)^2 + (1.5-0.9)^2 + (-1.0-0.9)^2 + (3.0-0.9)^2}{4}}
$$

$$
= \sqrt{\frac{1.21 + 3.61 + 0.36 + 3.61 + 4.41}{4}} = \sqrt{\frac{13.20}{4}} = \sqrt{3.30} \approx 1.82
$$

---

## Relationship to Other Metrics

| Metric | How R-Multiples Relate |
|--------|----------------------|
| **Expectancy** | Expectancy in R = mean R. Often expressed as: `(Win% × Avg Win R) − (Loss% × Avg Loss R)` |
| **SQN** | SQN = √N × mean(R) / stdev(R). See SQN |
| **Profit Factor** | Gross R-won / Gross R-lost |
| **Win Rate** | Percentage of trades with R > 0 |
| **Payoff Ratio** | Average winning R / Average losing R |

---

## Why Use R-Multiples?

1. **Normalisation** — Compare a $5 stock trade to a $500 futures trade on the same scale.
2. **Risk-first thinking** — Forces you to define risk (stop-loss) before entering a trade.
3. **Position sizing** — If you always risk 1R per trade, your equity curve is a direct function of your R-multiple distribution.
4. **Emotional detachment** — Thinking in R removes the dollar anchoring that leads to emotional decisions.

---

## Limitations

1. **Requires a defined stop-loss** — If you don't set a stop, 1R is undefined.
2. **Assumes single-risk entry** — Scaling in/out complicates the 1R calculation.
3. **Proprietary origin** — Not a standard academic metric; created by Van Tharp for his trading psychology courses.
4. **Discretionary interpretation** — What counts as "initial risk" can vary between traders.

---

## References

- Tharp, Van K. *Trade Your Way to Financial Freedom*. McGraw-Hill, 2nd ed., 2006. Chapters 6–7.
- Tharp, Van K. *The Definitive Guide to Position Sizing Strategies*. IITM, 2008.
- Tharp, Van K. *Super Trader*. McGraw-Hill, 2009.
