# HHI (Herfindahl–Hirschman Index)

## What Is It?

The **Herfindahl–Hirschman Index (HHI)** measures concentration — how spread out or lopsided your portfolio weights are. A low HHI means diversified; a high HHI means concentrated in a few positions.

---

## The Math

$$
\text{HHI} = \sum_{i=1}^{N} w_i^2
$$

Where:
- $w_i$ = weight of position $i$ (as a decimal, e.g. 0.25 for 25%)
- $N$ = number of positions

### Normalized HHI

To make values comparable across different portfolio sizes:

$$
\text{HHI}^{*} = \frac{\text{HHI} - \frac{1}{N}}{1 - \frac{1}{N}}
$$

| HHI Range | Interpretation |
|-----------|---------------|
| $\frac{1}{N}$ (minimum) | Perfectly equal-weighted |
| 0.15–0.25 | Moderate concentration |
| > 0.25 | High concentration |
| 1.0 (maximum) | Entire portfolio in one position |

---

## Example

Portfolio with 4 positions: 50%, 25%, 15%, 10%

$$
\text{HHI} = 0.50^2 + 0.25^2 + 0.15^2 + 0.10^2 = 0.25 + 0.0625 + 0.0225 + 0.01 = 0.345
$$

An equal-weight 4-position portfolio would have HHI = 0.25. This portfolio is more concentrated than equal-weight.

---

## Why It Matters

- **Risk awareness**: High HHI means a single position's bad day dominates your P&L.
- **Rebalancing signal**: Rising HHI over time suggests drift toward concentration.
- **Comparison**: HHI lets you compare concentration across portfolios of different sizes.

---

## How Zorivest Uses HHI

1. **Overview card**: Displays HHI alongside effective positions count.
2. **Concentration warning**: When HHI exceeds a threshold, the Risk Lens panel flags it.

---

## References

- Hirschman, A.O. (1964). "The Paternity of an Index." *The American Economic Review*, 54(5), 761.
- U.S. Department of Justice. "Herfindahl–Hirschman Index." *Horizontal Merger Guidelines*.
