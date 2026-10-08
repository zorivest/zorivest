# Expectancy & Profit Factor

## What Is Expectancy?

**Expectancy** is the average amount you can expect to win (or lose) per trade over many trades. It tells you whether your trading system has a positive edge.

---

## The Math

### Expectancy (Dollar)

$$
\text{Expectancy} = (W\% \times \overline{W}) - (L\% \times \overline{L})
$$

Where, over the **rated population** $N$ (every trade that has realised a P&L):

- $W\%$ = Win rate = $\dfrac{\text{wins}}{N}$
- $L\%$ = Loss rate = $\dfrac{\text{losses}}{N}$ — **counted, never complemented**
- $\overline{W}$ = Average winning trade (dollars)
- $\overline{L}$ = Average losing trade (dollars, absolute value)

### Three outcomes, not two

A closed trade lands in exactly one of three buckets, and the counts partition
the rated population:

$$
\text{wins} + \text{losses} + \text{scratches} = N
$$

A **scratch** is a round trip that realised exactly zero — commissions aside, it
went nowhere. It is neither a win nor a loss, so it belongs in neither rate.

This is why the loss rate is counted rather than derived. Defining
$L\%$ as the complement of the win rate silently reclassifies every scratch as a
loss, which then subtracts $\overline{L}$ dollars for trades that lost nothing.
The error grows with the number of break-even trades: a book of 6 wins and 4
losses that is genuinely profitable can be reported as *negative* expectancy
once enough scratches are appended, purely from the misclassification. Zorivest
counts all three buckets and exposes `win_count`, `loss_count`, `scratch_count`
and `rated_count` on `ExpectancyResult` so the partition is checkable from
outside the calculation.

**Example (no scratches)**: 55 wins, 45 losses, average win $300, average loss $200:
$$
E = (0.55 \times 300) - (0.45 \times 200) = 165 - 90 = +\$75 \text{ per trade}
$$

**Example (with scratches)**: the same 55 wins and 45 losses plus 100 scratches,
so $N = 200$. The rates are now $W\% = 0.275$ and $L\% = 0.225$ — **not** $0.725$:
$$
E = (0.275 \times 300) - (0.225 \times 200) = 82.5 - 45 = +\$37.50 \text{ per trade}
$$

The per-trade figure halves because it is averaged over twice as many trades,
which is correct — the same total profit spread over more round trips. The sign
does not flip, because no scratch was counted as a loss.

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

1. Takes the **rated population** — closed trades that carry a realised P&L.
   Open positions and unmatched executions are excluded, not zeroed.
2. Separates that population into winners, losers, and scratches. The three
   counts must sum to the rated count; this partition is asserted in the test
   suite, not merely assumed.
3. Computes average win size and average loss size over the winners and losers
   respectively.
4. Computes both rates as counts over the rated population — the loss rate is
   `losses / rated`, never `1 − win_rate`.
5. Applies the expectancy formula.
6. Profit Factor = sum of all positive P&L ÷ sum of all negative P&L (absolute).

> Kelly is **not** computed over this population. `f*` is defined on a
> two-outcome wager, so it uses the **non-scratch** subset — see
> kelly-criterion.md.

Both metrics are available in the **Overview** tab of Analytics and are also computed per-strategy in the **Strategy Breakdown**.

---

## References

- Tharp, V.K. (2006). *Trade Your Way to Financial Freedom*. McGraw-Hill. Chapter 7: Expectancy.
- Elder, A. (2002). *Come Into My Trading Room*. Wiley.
