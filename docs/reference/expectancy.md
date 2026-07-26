# Expectancy & Profit Factor

## What Is Expectancy?

**Expectancy** is the average amount you can expect to win (or lose) per trade over many trades. It tells you whether your trading system has a positive edge.

---

## The Math

### Expectancy (Dollar)

$$
\text{Expectancy} = (W\% \times \overline{W}) - (L\% \times \overline{L})
$$

Where:
- $W\%$ = Win rate (fraction of trades that are winners)
- $L\%$ = Loss rate = $1 - W\%$
- $\overline{W}$ = Average winning trade (dollars)
- $\overline{L}$ = Average losing trade (dollars, absolute value)

**Example**: 55% win rate, average win $300, average loss $200:
$$
E = (0.55 \times 300) - (0.45 \times 200) = 165 - 90 = +\$75 \text{ per trade}
$$

### Profit Factor

$$
\text{Profit Factor} = \frac{\sum \text{Gross Winning Trades}}{\sum |\text{Gross Losing Trades}|}
$$

| Profit Factor | Interpretation |
|--------------|---------------|
| < 1.0 | Losing money — losses exceed wins |
| 1.0 | Break-even |
| 1.5–2.0 | Good — solid edge |
| 2.0–3.0 | Very good |
| > 3.0 | Excellent (or small sample size — verify) |

---

## Why They Matter

- **Expectancy** answers: "Is my system profitable?"
- **Profit Factor** answers: "For every $1 lost, how many dollars did I win?"
- Both are more meaningful than win rate alone. A 30% win rate with 5:1 payoff ratio is highly profitable ($E = 0.30 \times 5 - 0.70 \times 1 = +\$0.80$ per $1 risked).

---

## How Zorivest Calculates It

1. Groups all closed trades.
2. Separates winners and losers.
3. Computes average win size and average loss size.
4. Applies the expectancy formula.
5. Profit Factor = sum of all positive P&L ÷ sum of all negative P&L (absolute).

Both metrics are available in the **Overview** tab of Analytics and are also computed per-strategy in the **Strategy Breakdown**.

---

## References

- Tharp, V.K. (2006). *Trade Your Way to Financial Freedom*. McGraw-Hill. Chapter 7: Expectancy.
- Elder, A. (2002). *Come Into My Trading Room*. Wiley.
