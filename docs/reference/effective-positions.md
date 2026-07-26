# Effective Positions

## What Is It?

**Effective positions** (also called "effective number of bets") measures how many truly independent bets your portfolio represents, accounting for both weight concentration and correlation structure. Five positions that are all highly correlated may only behave like two independent bets.

---

## The Math

### Weight-Based (from HHI)

$$
N_{\text{eff}} = \frac{1}{\text{HHI}} = \frac{1}{\sum_{i=1}^{N} w_i^2}
$$

Where $w_i$ is the weight of position $i$.

### Correlation-Aware: Meucci Effective Number of Bets (ENB)

When a full covariance matrix is available, Zorivest computes the **Meucci ENB** — the exponential of the Shannon entropy of the eigenvalue spectrum of the covariance matrix:

$$
p_k = \frac{\lambda_k}{\sum_j \lambda_j}, \quad
N_{\text{ENB}} = \exp\!\left(-\sum_{k=1}^{N} p_k \ln p_k\right)
$$

Where $\lambda_k$ are the eigenvalues of the portfolio covariance matrix. This captures how many independent risk sources (principal components) your portfolio actually has, accounting for both correlation and weight concentration.

> **Note:** An earlier version of this document described the diversification ratio² (DR²) formula. That measure is *not* implemented in Zorivest — the engine uses Meucci ENB exclusively for the effective-bets metric.

---

## Example

| Scenario | Positions | Effective N |
|----------|-----------|-------------|
| 5 positions, equal-weight, zero correlation | 5 | ≈ 5.0 |
| 5 positions, equal-weight, all ρ = 0.8 | 5 | ≈ 1.6 |
| 10 positions, one at 60%, rest at ~4.5% | 10 | ≈ 2.6 |

---

## Why It Matters

- **True diversification**: Nominal position count is misleading when assets are correlated.
- **Risk sizing**: If effective N is low, a single market shock affects most of your portfolio.
- **Comparison**: Lets you compare real diversification across different portfolio constructions.

---

## How Zorivest Uses Effective Positions

1. **Risk Lens card**: The "Effective Positions" card shows the Meucci ENB value — how many independent risk bets the portfolio represents.
2. **Capital concentration (1/HHI)**: A separate weight-only measure shown alongside HHI. This does NOT account for correlation — it only measures weight dispersion.
3. **Diversification check**: Low ENB relative to nominal position count signals that correlation is reducing your effective diversification.

---

## References

- Laakso, M. & Taagepera, R. (1979). "Effective Number of Parties: A Measure with Application to West Europe." *Comparative Political Studies*, 12(1), 3–27.
- Roncalli, T. (2014). *Introduction to Risk Parity and Budgeting*. Chapman & Hall.
