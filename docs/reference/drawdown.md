# Drawdown

## What Is It?

A **drawdown** measures the decline from a peak to a subsequent trough in the value of a portfolio or trading account. It represents the worst-case loss you experienced before recovering.

---

## The Math

$$
\text{Drawdown}_t = \frac{V_t - V_{\text{peak}}}{V_{\text{peak}}} \times 100\%
$$

Where:
- $V_t$ = portfolio value at time $t$
- $V_{\text{peak}}$ = highest portfolio value before time $t$

### Max Drawdown

$$
\text{Max Drawdown} = \max_{t} \left( \frac{V_{\text{peak}} - V_t}{V_{\text{peak}}} \right)
$$

The single largest peak-to-trough decline across your entire history.

### Recovery Factor

$$
\text{Recovery Factor} = \frac{\text{Total Net Profit}}{|\text{Max Drawdown}|}
$$

| Recovery Factor | Interpretation |
|----------------|---------------|
| < 1.0 | Total profit hasn't covered the worst drop yet |
| 1.0–3.0 | Recovered, but not by a wide margin |
| 3.0–5.0 | Strong — profits far exceed the worst dip |
| > 5.0 | Very resilient system |

---

## Why It Matters

- **Risk tolerance**: A 30% max drawdown means your $100,000 account dropped to $70,000 at some point. Could you handle that psychologically?
- **Strategy evaluation**: Two strategies with the same return but different max drawdowns have very different risk profiles.
- **Position sizing**: Knowing your expected drawdown helps size positions to keep the dip within your comfort zone.

---

## Drawdown Duration

The time between the peak and the recovery back to that peak level:

- **Drawdown period**: Peak → Trough
- **Recovery period**: Trough → New Peak
- **Total underwater time**: Peak → Recovery

Long underwater periods test discipline even when the percentage drop is modest.

---

## How Zorivest Uses Drawdown

1. **Max Drawdown card**: Shows your worst historical peak-to-trough decline.
2. **Recovery Factor**: Total profit ÷ Max Drawdown — how well you bounced back.
3. **Calmar Ratio**: Annual return ÷ Max Drawdown — reward per unit of worst-case risk.
4. **Drawdown Simulation**: Monte Carlo runs that project future drawdown probability.
5. **Equity Curve**: Visual chart showing drawdown periods shaded in red.

---

## References

- Magdon-Ismail, M. & Atiya, A. (2004). "Maximum Drawdown." *Risk Magazine*.
- Tharp, V.K. (2006). *Trade Your Way to Financial Freedom*. McGraw-Hill.
