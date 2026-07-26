# Risk of Ruin

## What Is It?

**Risk of ruin** is the probability that a trader's account will decline to a specified threshold (e.g., losing 50% of capital) before recovering. It quantifies the chance of catastrophic loss.

---

## The Math

For a simplified binomial model:

$$
\text{Risk of Ruin} = \left(\frac{1 - \text{Edge}}{\text{1 + Edge}}\right)^{\text{Capital Units}}
$$

Where:
- **Edge** = (Win% × Avg Win) − (Loss% × Avg Loss), normalized
- **Capital Units** = account size ÷ amount risked per trade

In practice, Zorivest uses **Monte Carlo simulation** for a more accurate estimate that accounts for variable trade sizes and non-symmetric distributions.

---

## Factors That Affect Ruin Probability

| Factor | Effect on Ruin Probability |
|--------|---------------------------|
| Higher win rate | ↓ Decreases ruin probability |
| Higher payoff ratio | ↓ Decreases ruin probability |
| Larger position size (% of capital) | ↑ Increases ruin probability |
| Longer time horizon | ↑ Increases ruin probability |
| Higher ruin threshold (e.g., 50% vs 25%) | ↓ Decreases (easier to stay above) |

---

## Recommended Risk Per Trade

Based on risk-of-ruin analysis:

| Risk Per Trade | Ruin Risk (typical edge) | Suitability |
|---------------|-------------------------|-------------|
| 0.5% | Very low (< 1%) | Conservative / large accounts |
| 1.0% | Low (1–5%) | Standard recommendation |
| 2.0% | Moderate (5–15%) | Aggressive but manageable |
| 5.0% | High (15–40%) | Very aggressive |
| 10%+ | Near-certain ruin | Not recommended |

---

## How Zorivest Uses It

The **Risk** tab in Analytics runs Monte Carlo simulations and reports:
- **Ruin probability**: Chance of account dropping below the configurable threshold.
- **Recommended risk %**: Suggested per-trade risk to keep ruin probability acceptably low.
- These values are computed from your actual trade history, not theoretical models.

---

## References

- Vince, R. (1992). *The Mathematics of Money Management*. Wiley.
- Tharp, V.K. (2006). *Trade Your Way to Financial Freedom*. McGraw-Hill.
