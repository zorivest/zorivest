# Risk–Reward Ratio & Payoff Ratio

## What Is It?

The **risk–reward ratio** (also called **reward-to-risk ratio** or **payoff ratio**) compares what you stand to gain against what you stand to lose on a trade.

---

## The Math

### Risk–Reward Ratio (Pre-Trade)

$$
\text{Risk–Reward} = \frac{\text{Target Price} - \text{Entry Price}}{\text{Entry Price} - \text{Stop Loss}}
$$

A 1:3 risk–reward means you're risking $1 to potentially make $3.

### Payoff Ratio (Post-Trade / Historical)

$$
\text{Payoff Ratio} = \frac{\text{Average Winning Trade}}{\text{Average Losing Trade}}
$$

| Payoff Ratio | Interpretation |
|-------------|---------------|
| < 1.0 | Average winners are smaller than average losers |
| 1.0 | Winners and losers are the same size on average |
| 1.5–2.0 | Healthy — winners are bigger than losers |
| > 3.0 | Excellent — your winners dwarf your losers |

### Avg Win / Avg Loss

Simply the dollar amounts:

$$
\text{Avg Win} = \frac{\sum \text{Winning Trades P\&L}}{\text{Number of Winners}}
$$

$$
\text{Avg Loss} = \frac{\sum |\text{Losing Trades P\&L}|}{\text{Number of Losers}}
$$

---

## Relationship to Win Rate

Payoff ratio and win rate are inversely related — you can be profitable with a low win rate if your payoff ratio is high:

| Win Rate | Required Payoff Ratio to Break Even |
|----------|-------------------------------------|
| 30% | 2.33:1 |
| 40% | 1.50:1 |
| 50% | 1.00:1 |
| 60% | 0.67:1 |
| 70% | 0.43:1 |

---

## How Zorivest Uses It

- **Overview tab**: Displays Avg Win, Avg Loss, and Payoff Ratio as headline metrics.
- **Position Calculator**: Computes pre-trade risk–reward ratio from your entry, stop, and target prices.
- **Expectancy calculation**: Combines win rate × payoff ratio to determine edge.

---

## References

- Tharp, V.K. (2006). *Trade Your Way to Financial Freedom*. McGraw-Hill.
- Schwager, J.D. (2012). *Market Wizards*. Wiley.
