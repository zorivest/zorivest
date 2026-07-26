# System Quality Number (SQN)

> A statistical metric that grades the overall quality and tradability of a trading system.
> Created by Dr. Van K. Tharp (1946–2022).

---

## Formula

$$
\text{SQN} = \sqrt{N} \times \frac{\overline{R}}{\sigma_R}
$$

Where:

| Symbol | Meaning |
|--------|---------|
| **N** | Number of trades in the sample |
| **R̄** (mean R) | Average R-multiple across all trades (see R-Multiples) |
| **σ_R** (std R) | Standard deviation of the R-multiples |

The formula is essentially a **t-statistic** applied to R-multiples: it measures how many standard deviations the mean return is away from zero, scaled by sample size.

---

## Equivalent Forms

The formula is sometimes written with raw P&L instead of R-multiples:

$$
\text{SQN} = \frac{\text{Mean P\&L per trade} \times \sqrt{N}}{\text{Std Dev of P\&L per trade}}
$$

Both forms are mathematically equivalent when P&L is expressed in R-units.

---

## Scoring Scale

Van Tharp provided the following interpretive scale:

| SQN | Rating | What It Means |
|-----|--------|---------------|
| < 1.5 | Poor | Very hard to trade profitably |
| 1.5 – 2.0 | Below average | Marginal edge; high discipline needed |
| 2.0 – 3.0 | Average | Workable system with consistent execution |
| 3.0 – 5.0 | Good | Strong, reliable edge |
| 5.0 – 7.0 | Excellent | Very consistent; rare in practice |
| > 7.0 | Superb | Treat with caution — may indicate curve-fitting |

> **Note:** SQN scores above 7 are unusual in live trading. Extremely high values often result from over-optimisation on historical data. Cross-validate with out-of-sample testing.

---

## Worked Example

| Trade # | R-Multiple |
|---------|-----------|
| 1 | +2.0 |
| 2 | −1.0 |
| 3 | +1.5 |
| 4 | −1.0 |
| 5 | +3.0 |
| 6 | −0.5 |
| 7 | +1.0 |
| 8 | −1.0 |
| 9 | +2.5 |
| 10 | +0.5 |

**Step 1 — Mean R:**

$$
\overline{R} = \frac{2.0 + (-1.0) + 1.5 + (-1.0) + 3.0 + (-0.5) + 1.0 + (-1.0) + 2.5 + 0.5}{10} = \frac{7.0}{10} = 0.70
$$

**Step 2 — Standard deviation of R:**

$$
\sigma_R = \sqrt{\frac{\sum (R_i - \overline{R})^2}{N - 1}} \approx 1.418
$$

**Step 3 — SQN:**

$$
\text{SQN} = \sqrt{10} \times \frac{0.70}{1.418} = 3.162 \times 0.494 = 1.56
$$

**Interpretation:** SQN ≈ 1.6 → *Below average*. The system has a positive edge but results are inconsistent. Improving win consistency (lowering σ_R) or increasing mean R would raise the score.

---

## Why √N?

Multiplying by √N converts the ratio into a **t-statistic**. This rewards systems with more trades (higher statistical confidence) while penalising systems tested on only a handful of trades.

- 10 trades → √10 ≈ 3.16
- 100 trades → √100 = 10
- 1000 trades → √1000 ≈ 31.6

A system with mean R = 0.5 and σ_R = 1.0 would score:
- SQN = 1.58 with 10 trades
- SQN = 5.0 with 100 trades
- SQN = 15.8 with 1000 trades

Van Tharp recommended **capping N at 100** when computing SQN to prevent inflated scores from high-frequency systems.

---

## Relationship to Other Metrics

| Metric | Relationship |
|--------|-------------|
| **Expectancy** | SQN uses expectancy (mean R) as the numerator |
| **Sharpe Ratio** | Conceptually similar — both are return/risk ratios. Sharpe uses portfolio returns; SQN uses R-multiples |
| **Profit Factor** | Gross profit / gross loss. SQN accounts for consistency; profit factor does not |

---

## Limitations

1. **Proprietary origin** — The metric was created by a single author (Van Tharp) and is not a standard in academic finance literature.
2. **Sample-size sensitivity** — Without the N-cap, high-frequency systems can produce misleadingly high SQN values.
3. **Assumes normal distribution** — R-multiples in real trading often have fat tails. SQN does not account for tail risk.
4. **Does not distinguish win rate from payoff** — A 30% win-rate system with large winners can have the same SQN as a 70% win-rate system with small winners.

---

## References

- Tharp, Van K. *Trade Your Way to Financial Freedom*. McGraw-Hill, 2nd ed., 2006. Chapter 7.
- Tharp, Van K. *Super Trader*. McGraw-Hill, 2009. Appendix: SQN calculations.
- Tharp, Van K. *The Definitive Guide to Position Sizing Strategies*. IITM, 2008.
