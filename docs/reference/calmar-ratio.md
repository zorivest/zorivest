# Calmar Ratio

## What Is It?

The **Calmar Ratio** compares annualized return to maximum drawdown. It tells you how much return you earned per unit of worst-case pain. Named after its creator **Terry W. Young** and his California Managed Accounts Reports newsletter (1991).

---

## The Math

$$
\text{Calmar Ratio} = \frac{\text{Annualized Return}}{|\text{Max Drawdown}|}
$$

| Calmar Ratio | Interpretation |
|-------------|---------------|
| < 0.5 | Poor — large drawdowns relative to returns |
| 0.5–1.0 | Below average |
| 1.0–2.0 | Good |
| 2.0–5.0 | Very good |
| > 5.0 | Excellent (or short track record) |

---

## Why It Matters

- Answers: "For every 1% of max pain, how many percent did I earn annually?"
- More intuitive than Sharpe for traders who think in terms of drawdowns.
- A Calmar of 2.0 means your annual return is 2× your worst drawdown.

---

## Limitations

- Uses only the single worst drawdown, which may be an outlier.
- Typically calculated over 3 years — shorter periods may be unreliable.
- Doesn't account for drawdown duration or recovery speed.

---

## How Zorivest Calculates It

Zorivest computes the Calmar Ratio from your trade history: annualized P&L return divided by the absolute value of your max drawdown percentage. It appears in the **Overview** tab of Analytics.

---

## References

- Young, T.W. (1991). "Calmar Ratio: A Smoother Tool." *Futures Magazine*.
