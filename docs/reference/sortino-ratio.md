# Sortino Ratio

## What Is It?

The **Sortino Ratio** is a modification of the Sharpe Ratio that only penalizes **downside** volatility. It was developed by **Frank A. Sortino** in the early 1980s.

Unlike the Sharpe Ratio, which treats all volatility as bad, the Sortino Ratio recognizes that upside volatility (big wins) is desirable.

---

## The Math

$$
\text{Sortino Ratio} = \frac{R_p - R_f}{\sigma_d}
$$

Where:
- $R_p$ = portfolio return (annualized)
- $R_f$ = risk-free rate or minimum acceptable return (MAR)
- $\sigma_d$ = downside deviation (standard deviation of negative returns only)

### Downside Deviation

$$
\sigma_d = \sqrt{\frac{1}{N} \sum_{i=1}^{N} \min(R_i - R_f, 0)^2}
$$

Only returns below the target rate contribute to the denominator.

| Sortino Ratio | Interpretation |
|--------------|---------------|
| < 0 | Losing money |
| 0–1.0 | Below average |
| 1.0–2.0 | Good |
| 2.0–3.0 | Very good |
| > 3.0 | Excellent |

---

## Sharpe vs. Sortino

| | Sharpe | Sortino |
|--|--------|---------|
| Penalizes | All volatility | Only downside volatility |
| Best for | Symmetric return distributions | Skewed returns (e.g., trend following) |
| Limitation | Punishes big wins | Requires enough losing trades for stable calculation |

---

## How Zorivest Calculates It

Zorivest computes the Sortino Ratio from your closed trade P&L series. It uses downside deviation (returns below the risk-free rate) as the denominator. The ratio appears in the **Overview** tab of Analytics alongside the Sharpe Ratio.

---

## References

- Sortino, F.A. & van der Meer, R. (1991). "Downside Risk." *Journal of Portfolio Management*, 17(4), 27–31.
- Sortino, F.A. & Price, L.N. (1994). "Performance Measurement in a Downside Risk Framework." *Journal of Investing*, 3(3), 59–64.
