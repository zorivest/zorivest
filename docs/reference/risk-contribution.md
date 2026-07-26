# Risk Contribution

## What Is It?

**Risk contribution** (also called component risk or risk attribution) measures how much of the total portfolio risk each position is responsible for. Unlike weight, risk contribution accounts for both position size, volatility, and correlation with the rest of the portfolio.

---

## The Math

### Marginal Contribution to Risk (MCTR)

$$
\text{MCTR}_i = \frac{\partial \sigma_P}{\partial w_i} = \frac{(\mathbf{\Sigma} \, \mathbf{w})_i}{\sigma_P}
$$

Where:
- $\mathbf{\Sigma}$ = covariance matrix
- $\mathbf{w}$ = weight vector
- $\sigma_P$ = portfolio volatility

### Component Risk (Total Risk Contribution)

$$
\text{RC}_i = w_i \times \text{MCTR}_i = \frac{w_i \, (\mathbf{\Sigma} \, \mathbf{w})_i}{\sigma_P}
$$

The sum of all component risks equals the total portfolio volatility:

$$
\sum_{i=1}^{N} \text{RC}_i = \sigma_P
$$

### Percentage Risk Contribution

$$
\text{PRC}_i = \frac{\text{RC}_i}{\sigma_P} \times 100\%
$$

---

## Example

| Position | Weight | Volatility | PRC |
|----------|--------|-----------|-----|
| AAPL | 30% | 25% | 42% |
| MSFT | 30% | 22% | 35% |
| TLT | 40% | 10% | 23% |

AAPL contributes 42% of portfolio risk despite being only 30% of weight — because it's the most volatile and correlated with MSFT.

---

## Why It Matters

- **True risk source**: Weight ≠ risk. A 10% position in a volatile, correlated stock can dominate risk.
- **Risk budgeting**: Allocate risk evenly (risk parity) or intentionally toward highest-conviction positions.
- **Rebalancing**: Trim positions whose risk contribution has drifted above target.

---

## How Zorivest Uses Risk Contribution

1. **Risk contribution table**: Bar-encoded breakdown of each position's PRC.
2. **Disclaimer**: Clearly states this is backward-looking and not forward guidance.
3. **Marginal risk tooltip**: Shows how adding/removing one unit changes total portfolio risk.

---

## References

- Menchero, J. (2000). "An Optimized Approach to Linking Attribution Effects Over Time." *Journal of Performance Measurement*, 5(1), 5–13.
- Roncalli, T. (2014). *Introduction to Risk Parity and Budgeting*. Chapman & Hall.
