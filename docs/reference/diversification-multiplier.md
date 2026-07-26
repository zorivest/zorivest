# Diversification Multiplier

## What Is It?

The **diversification multiplier** (also called the diversification ratio or diversification benefit) quantifies how much risk reduction your portfolio achieves from holding multiple assets instead of a single concentrated position. A value above 1.0 means diversification is working — your total portfolio risk is lower than the weighted sum of individual risks.

---

## The Math

$$
\text{DR} = \frac{\sum_{i=1}^{N} w_i \, \sigma_i}{\sigma_P}
$$

Where:
- $w_i$ = weight of position $i$
- $\sigma_i$ = volatility (standard deviation) of position $i$
- $\sigma_P$ = portfolio volatility

The numerator is the *undiversified risk* — what your portfolio risk would be if all positions were perfectly correlated (+1). The denominator is the *actual* portfolio risk.

### Interpretation

| DR Value | Meaning |
|----------|---------|
| **1.0** | No diversification benefit — assets move in lockstep |
| **1.2** | Portfolio risk is ~17% lower than the undiversified sum |
| **1.5** | Portfolio risk is ~33% lower than the undiversified sum |
| **> 2.0** | Strong diversification — significant risk reduction |

---

## Why It Matters

- **Portfolio construction**: A rising DR after adding a new position confirms it contributes genuine diversification.
- **Regime monitoring**: DR tends to fall during market crises as correlations spike.
- **Rebalancing**: Falling DR may signal positions have converged and rebalancing is needed.

---

## How Zorivest Uses Diversification Multiplier

1. **Overview card**: Displayed in the Risk Lens panel as a summary metric.
2. **Risk check**: Combined with HHI and effective positions to give a full concentration picture.

---

## References

- Choueifaty, Y. & Coignard, Y. (2008). "Toward Maximum Diversification." *Journal of Portfolio Management*, 35(1), 40–51.
- Roncalli, T. (2014). *Introduction to Risk Parity and Budgeting*. Chapman & Hall.
