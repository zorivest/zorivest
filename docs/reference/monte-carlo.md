# Monte Carlo Simulation

## What Is It?

**Monte Carlo simulation** is a computational technique that uses random sampling to estimate the probability of different outcomes. In trading, it replays your actual trades in thousands of random orders to estimate the range of possible future results.

Named after the Monte Carlo Casino in Monaco — the method relies on repeated random trials, much like gambling.

---

## How It Works in Trading

1. **Take your real trade results** (e.g., 200 closed trades with their P&L).
2. **Randomly reorder** those trades thousands of times (e.g., 10,000 simulations).
3. **For each simulation**, compute the equity curve, max drawdown, and ending balance.
4. **Aggregate results** into percentile bands (5th, 25th, 50th, 75th, 95th).

This answers: "If I keep trading with the same edge, what range of outcomes should I expect?"

---

## Key Outputs

| Output | What It Tells You |
|--------|------------------|
| **Equity Curve Bands** | The range of possible portfolio paths (best case, worst case, typical) |
| **Max Drawdown Distribution** | How bad your worst drop could get in different scenarios |
| **Risk of Ruin** | Probability of your account dropping below a threshold (e.g., losing 50%) |
| **Percentile Table** | P5/P25/P50/P75/P95 endpoints after N future trades |

---

## Why Random Reordering?

Your actual sequence of wins and losses is just one of many possible orderings. The same 200 trades could have occurred in a different order — leading to very different drawdowns.

Monte Carlo reveals whether your good results were due to lucky sequencing or a genuine edge.

---

## Limitations

- Assumes future trades will resemble past trades (no regime changes).
- Assumes trades are independent (no serial correlation).
- Does not model changing market conditions.
- Results are only as good as the trade sample — small samples produce unreliable estimates.

---

## How Zorivest Uses It

The **Risk** tab in Analytics runs Monte Carlo simulations on your closed trade history:
- Default: 10,000 simulations over your trade count as the horizon.
- Produces percentile bands (P5 through P95) for equity curves.
- Computes **risk of ruin** probability for configurable thresholds.
- Results update when you add new trades or change simulation parameters.

---

## References

- Metropolis, N. & Ulam, S. (1949). "The Monte Carlo Method." *Journal of the American Statistical Association*, 44(247), 335–341.
- Vince, R. (1992). *The Mathematics of Money Management*. Wiley.
