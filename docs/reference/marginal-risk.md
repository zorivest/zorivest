# Marginal Risk

## What Is It?

**Marginal risk** measures how much adding (or removing) one unit of a position would change your portfolio's total risk. It answers: "If I buy a little more of this stock, does my overall portfolio get riskier or safer?"

---

## The Math

### Marginal Contribution to Risk (MCTR)

$$
\text{MCTR}_i = \frac{\partial \sigma_P}{\partial w_i} = \frac{(\mathbf{\Sigma} \, \mathbf{w})_i}{\sigma_P}
$$

Where:
- $\mathbf{\Sigma}$ = covariance matrix of returns
- $\mathbf{w}$ = vector of portfolio weights
- $\sigma_P$ = current portfolio volatility

### Interpretation

| MCTR Value | Meaning |
|-----------|---------|
| **High (> avg)** | Adding more of this asset increases portfolio risk disproportionately |
| **Equal to avg** | Proportional risk impact — risk parity target |
| **Low (< avg)** | Adding more of this asset would *reduce* or barely increase total risk |
| **Negative** | Rare — adding this asset actually decreases total portfolio risk (strong diversifier) |

---

## Example

Portfolio: AAPL (40%), MSFT (30%), GLD (30%)

| Position | MCTR | Interpretation |
|----------|------|---------------|
| AAPL | 18.2% | Highest marginal risk — adding more increases portfolio vol most |
| MSFT | 15.1% | Moderate — correlated with AAPL |
| GLD | 4.3% | Low — acts as a diversifier, adding more barely increases risk |

---

## Why It Matters

- **Trim or add?**: High MCTR positions are where trimming has the biggest risk-reduction payoff.
- **Opportunity cost**: Low MCTR positions can be increased cheaply from a risk perspective.
- **Risk parity check**: In a risk-parity portfolio, all positions should have equal MCTR × weight.

---

## How Zorivest Uses Marginal Risk

1. **Tooltip in risk contribution table**: Shows each position's MCTR on hover.
2. **Decision support**: Helps users decide where to add or reduce exposure.

---

## References

- Litterman, R. (1996). "Hot Spots and Hedges." *Journal of Portfolio Management*, 22(5), 52–75.
- Roncalli, T. (2014). *Introduction to Risk Parity and Budgeting*. Chapman & Hall.
