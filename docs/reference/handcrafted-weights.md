# Handcrafted Weights

## What Is It?

**Handcrafted weights** is a transparent, formula-based approach to portfolio weight allocation that uses the correlation structure of your positions — without the fragility of full mean-variance optimization. Developed by Robert Carver, the method groups correlated assets into clusters and distributes weights top-down.

---

## The Math

### Step 1: Cluster Correlated Assets

Group positions by correlation similarity (e.g., positions with ρ > 0.7 go into the same cluster).

### Step 2: Equal Risk Between Clusters

$$
W_{\text{cluster}_k} = \frac{1}{K}
$$

Where $K$ = number of clusters.

### Step 3: Equal Risk Within Clusters

$$
w_i = \frac{W_{\text{cluster}_k}}{n_k} \times \frac{1 / \sigma_i}{\sum_{j \in k} 1 / \sigma_j}
$$

Where:
- $n_k$ = number of assets in cluster $k$
- $\sigma_i$ = volatility of asset $i$

This gives each asset within a cluster a weight inversely proportional to its volatility, ensuring equal risk contribution within the cluster.

### Simplified Equal-Weight Version

When volatility data is limited:

$$
w_i = \frac{1}{K \times n_k}
$$

---

## Why It Matters

- **No black box**: Unlike mean-variance optimization, every step is transparent and explainable.
- **Robust**: Doesn't rely on estimated expected returns (which are notoriously noisy).
- **Intuitive**: Equal risk across independent bets, adjusted for within-group concentration.
- **Stable**: Small changes in input data produce small changes in weights.

---

## How Zorivest Uses Handcrafted Weights

1. **Suggested weights table**: Displayed in the Risk Lens panel's portfolio decomposition section.
2. **Disclaimer**: Clearly marked as "suggestion" — not investment advice or auto-rebalancing.
3. **Comparison**: Shown alongside actual weights so you can see where you deviate from the formula.

---

## References

- Carver, R. (2015). *Systematic Trading*. Harriman House.
- Carver, R. (2019). *Leveraged Trading*. Harriman House.
