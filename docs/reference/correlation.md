# Correlation & Correlation Matrix

## What Is It?

**Correlation** measures how two assets move relative to each other. The Pearson correlation coefficient ranges from −1 to +1:

| Value | Meaning |
|-------|---------|
| **+1** | Perfect lockstep — both move the same way, same time |
| **0** | No linear relationship |
| **−1** | Perfect opposites — one goes up, the other goes down |

---

## The Math

### Pearson Correlation

$$
\rho_{X,Y} = \frac{\text{Cov}(X, Y)}{\sigma_X \, \sigma_Y}
= \frac{\sum_{i=1}^{n}(X_i - \bar{X})(Y_i - \bar{Y})}{\sqrt{\sum_{i=1}^{n}(X_i - \bar{X})^2} \; \sqrt{\sum_{i=1}^{n}(Y_i - \bar{Y})^2}}
$$

Where:
- $X_i, Y_i$ = daily returns of assets X and Y
- $\bar{X}, \bar{Y}$ = mean returns
- $\sigma_X, \sigma_Y$ = standard deviations
- $\text{Cov}(X,Y)$ = covariance

### Correlation Matrix

For a portfolio of $N$ assets, the **correlation matrix** $\mathbf{C}$ is an $N \times N$ symmetric matrix where entry $C_{ij} = \rho_{i,j}$:

$$
\mathbf{C} = \begin{pmatrix}
1 & \rho_{1,2} & \cdots & \rho_{1,N} \\
\rho_{2,1} & 1 & \cdots & \rho_{2,N} \\
\vdots & \vdots & \ddots & \vdots \\
\rho_{N,1} & \rho_{N,2} & \cdots & 1
\end{pmatrix}
$$

Properties:
- Diagonal entries are always 1 (each asset is perfectly correlated with itself)
- Symmetric: $\rho_{i,j} = \rho_{j,i}$
- Must be positive semi-definite

---

## Why It Matters

- **Diversification**: Assets with low or negative correlation reduce portfolio risk — losses in one tend to be offset by gains in another.
- **Hidden clusters**: Five positions might *look* diversified but act like two if three are highly correlated.
- **Regime shifts**: Correlations tend to spike during market stress — assets that seemed uncorrelated suddenly move together.

---

## How Zorivest Uses Correlation

1. **Correlation Heatmap**: An N×N color-coded grid showing pairwise correlations across all positions.
2. **Avg Pairwise Correlation**: When the portfolio-wide average exceeds 0.7, a warning badge appears.
3. **Effective Positions**: Derived from the correlation matrix — how many independent bets your portfolio represents.
4. **Risk Contribution**: Uses the correlation structure to decompose total portfolio risk by position.

---

## References

- Markowitz, H. (1952). "Portfolio Selection." *The Journal of Finance*, 7(1), 77–91.
- Ledoit, O. & Wolf, M. (2004). "A well-conditioned estimator for large-dimensional covariance matrices." *Journal of Multivariate Analysis*, 88(2), 365–411.
