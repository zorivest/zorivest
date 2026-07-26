# Win Rate

## What Is It?

**Win rate** (also called **hit rate** or **batting average**) is the percentage of trades that are profitable.

---

## The Math

$$
\text{Win Rate} = \frac{\text{Number of Winning Trades}}{\text{Total Number of Trades}} \times 100\%
$$

A trade is a "win" if its realized P&L is > $0, and a "loss" if ≤ $0.

| Win Rate | Typical Strategy Type |
|----------|----------------------|
| 25–40% | Trend-following, breakout systems |
| 40–55% | Swing trading, mean reversion |
| 55–70% | Scalping, high-frequency |
| 70%+ | Options selling, premium collection |

---

## Win Rate Alone Is Misleading

Win rate without payoff ratio tells an incomplete story:

- A 90% win rate losing $10,000 per loss and winning $100 per win is **deeply unprofitable**.
- A 30% win rate with 5:1 winners is **highly profitable**.

The meaningful metric is **expectancy**, which combines win rate with average win/loss size.

---

## How Zorivest Calculates It

Zorivest counts all closed trades with positive realized P&L as winners. The win rate appears in the **Overview** tab of Analytics and is also computed per-strategy in the **Strategy Breakdown**.

---

## References

- Tharp, V.K. (2006). *Trade Your Way to Financial Freedom*. McGraw-Hill.
