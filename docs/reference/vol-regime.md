# Volatility Regime

## What Is It?

A **volatility regime** classifies the current market environment into distinct states — typically LOW, NORMAL, or HIGH — based on how bumpy (volatile) recent price action has been compared to historical norms.

---

## The Math

### Realized Volatility

$$
\sigma_{\text{realized}} = \sqrt{\frac{252}{n} \sum_{i=1}^{n} r_i^2}
$$

Where:
- $r_i$ = daily log return
- $n$ = lookback window (e.g. 20 days)
- 252 = annualization factor (trading days per year)

### Regime Classification

Zorivest compares current realized volatility to its rolling distribution:

| Regime | Condition | Typical Action |
|--------|-----------|---------------|
| **LOW** | $\sigma_{\text{current}} < \sigma_{\text{P25}}$ | May increase position sizes |
| **NORMAL** | $\sigma_{\text{P25}} \leq \sigma_{\text{current}} \leq \sigma_{\text{P75}}$ | Baseline sizing |
| **HIGH** | $\sigma_{\text{current}} > \sigma_{\text{P75}}$ | May reduce position sizes |

Where $\sigma_{\text{P25}}$ and $\sigma_{\text{P75}}$ are the 25th and 75th percentiles of the trailing volatility distribution.

---

## Why It Matters

- **Position sizing**: Reducing bet size in high-vol regimes limits drawdowns; increasing in low-vol captures calmer trends.
- **Regime shifts**: Transitioning from LOW to HIGH regime often accompanies market stress — an early warning.
- **Strategy selection**: Some strategies perform better in specific regimes (e.g., mean reversion in high-vol, trend-following in low-vol).

---

## How Zorivest Uses Vol Regime

1. **Regime badge**: Displays a colored badge (LOW / NORMAL / HIGH) in the Risk Lens panel.
2. **Lookback control**: User selects 30/60/90/180/252-day window for regime calculation.
3. **Context for risk metrics**: Correlation and risk contribution values are interpreted relative to the current regime.

---

## References

- Ang, A. & Bekaert, G. (2002). "International Asset Allocation With Regime Shifts." *The Review of Financial Studies*, 15(4), 1137–1187.
- Hamilton, J.D. (1989). "A New Approach to the Economic Analysis of Nonstationary Time Series and the Business Cycle." *Econometrica*, 57(2), 357–384.
