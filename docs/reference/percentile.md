# Percentiles in Trading Analytics

## What Are They?

A **percentile** indicates the value below which a given percentage of observations fall. In Monte Carlo simulation results, percentiles show the range of outcomes from worst-case to best-case.

---

## The Math

If you have N sorted values, the k-th percentile $P_k$ is the value at position:

$$
\text{index} = \frac{k}{100} \times (N + 1)
$$

(with interpolation between adjacent values when the index is not a whole number)

---

## Common Percentiles in Zorivest

| Percentile | Meaning in Monte Carlo | Interpretation |
|-----------|----------------------|---------------|
| **P5 (5th)** | Worst 5% of simulations | "In a really bad scenario…" |
| **P25 (25th)** | Below-average outcome | "If things don't go well…" |
| **P50 (50th)** | Median / typical outcome | "The most likely result" |
| **P75 (75th)** | Above-average outcome | "If things go well…" |
| **P95 (95th)** | Best 5% of simulations | "In a really good scenario…" |

---

## How to Read the Risk Table

When Zorivest runs 10,000 Monte Carlo simulations:

- **P5** tells you: "There's a 5% chance your result will be *this bad* or worse."
- **P50** tells you: "The typical outcome is around here."
- **P95** tells you: "There's only a 5% chance your result will be *this good* or better."

The gap between P5 and P95 represents the **uncertainty range** — wider gaps mean more variable outcomes.

---

## References

- Hyndman, R.J. & Fan, Y. (1996). "Sample Quantiles in Statistical Packages." *The American Statistician*, 50(4), 361–365.
