# Maximum Favorable & Adverse Excursion (MFE / MAE)

## What Are They?

**MFE (Maximum Favorable Excursion)** and **MAE (Maximum Adverse Excursion)** measure the best and worst points a trade reached *before* you closed it.

- **MFE** = the highest unrealized profit during the life of the trade.
- **MAE** = the deepest unrealized loss during the life of the trade.

These concepts were popularized by **John Sweeney** in his 1996 book *Maximum Adverse Excursion: Analyzing Price Fluctuations for Trading Management*.

---

## Why They Matter

| Metric | Question It Answers | Actionable Insight |
|--------|--------------------|--------------------|
| MFE | "How good did it get before I closed?" | Did I exit too early? Could I have captured more? |
| MAE | "How bad did it get before I recovered?" | Are my stops too tight or too loose? |
| Capture % | "What fraction of the best move did I keep?" | Grade exit timing objectively. |

---

## The Math

### MFE (Max Favorable Excursion)

For a **long** trade opened at price $P_{\text{entry}}$:

$$
\text{MFE} = \max_{t \in [\text{entry}, \text{exit}]} (P_t - P_{\text{entry}})
$$

For a **short** trade, the direction is reversed:

$$
\text{MFE} = \max_{t \in [\text{entry}, \text{exit}]} (P_{\text{entry}} - P_t)
$$

MFE is always ≥ 0 (at minimum, the price equaled your entry at the moment of entry).

### MAE (Max Adverse Excursion)

For a **long** trade:

$$
\text{MAE} = \max_{t \in [\text{entry}, \text{exit}]} (P_{\text{entry}} - P_t)
$$

For a **short** trade:

$$
\text{MAE} = \max_{t \in [\text{entry}, \text{exit}]} (P_t - P_{\text{entry}})
$$

MAE is always ≥ 0. A trade that never went against you has MAE = 0.

### Capture % (Excursion Capture Ratio)

$$
\text{Capture \%} = \frac{\text{Realized P\&L}}{\text{MFE}} \times 100
$$

| Capture % | Interpretation |
|-----------|---------------|
| 100% | You closed at the absolute best point — perfect exit |
| 60–80% | Solid — captured most of the move |
| 30–60% | Gave back a significant portion |
| < 30% | Left most of the profit on the table |
| Negative | Closed at a loss despite having an unrealized profit at some point |

---

## How Zorivest Calculates It

Zorivest computes MFE and MAE from intraday OHLCV bar data between entry and exit timestamps:

1. **Entry bar** is identified from the trade's execution timestamp.
2. **High/Low of each bar** between entry and exit is scanned.
3. **MFE** = maximum favorable distance from entry price across all bars.
4. **MAE** = maximum adverse distance from entry price across all bars.
5. **Capture %** = realized P&L ÷ MFE × 100.

If OHLCV data is unavailable, the excursion panel shows a prompt to recalculate.

---

## Practical Use

- **Setting stop-losses**: If your winning trades rarely have MAE > 2%, a 2% stop captures most winners while cutting losers.
- **Setting profit targets**: If MFE clusters around 5% but you're closing at 2%, you're leaving money on the table.
- **Strategy comparison**: A strategy with high MFE but low Capture% has a timing problem, not an edge problem.

---

## References

- Sweeney, J. (1996). *Maximum Adverse Excursion: Analyzing Price Fluctuations for Trading Management*. Wiley.
- Tharp, V.K. (2006). *Trade Your Way to Financial Freedom*. McGraw-Hill. Chapter on expectancy and R-multiples.
