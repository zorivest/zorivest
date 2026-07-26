# Sharpe Ratio

## What Is It?

The **Sharpe Ratio** measures risk-adjusted return — how much excess return you earned for each unit of volatility (risk). It was developed by Nobel laureate **William F. Sharpe** in 1966.

---

## The Math

$$
\text{Sharpe Ratio} = \frac{R_p - R_f}{\sigma_p}
$$

Where:
- $R_p$ = portfolio return (annualized)
- $R_f$ = risk-free rate (typically 3-month T-bill yield)
- $\sigma_p$ = standard deviation of portfolio returns (annualized)

| Sharpe Ratio | Interpretation |
|-------------|---------------|
| < 0 | Returns worse than the risk-free rate |
| 0–0.5 | Below average |
| 0.5–1.0 | Acceptable |
| 1.0–2.0 | Good |
| 2.0–3.0 | Very good |
| > 3.0 | Excellent (verify sample size) |

---

## Limitations

- Treats upside and downside volatility equally (see **Sortino Ratio** for a fix).
- Assumes returns are normally distributed — doesn't capture tail risk well.
- Sensitive to the measurement period and return frequency.

---

## How Zorivest Calculates It

Zorivest computes the Sharpe Ratio from your closed trade P&L series using the annualized method with a configurable risk-free rate (default: 0%). The ratio appears in the **Overview** tab of Analytics.

---

## References

- Sharpe, W.F. (1966). "Mutual Fund Performance." *Journal of Business*, 39(1), 119–138.
- Sharpe, W.F. (1994). "The Sharpe Ratio." *Journal of Portfolio Management*, 21(1), 49–58.
