# Tail Risk & Tail Ratio

## What Is It?

**Tail risk** refers to the probability of extreme outcomes — the rare, large moves that sit in the "tails" of a return distribution. The **tail ratio** compares the magnitude of your positive tails to your negative tails.

---

## The Math

### Tail Ratio

$$
\text{Tail Ratio} = \frac{|P_{95}|}{|P_5|}
$$

Where:
- $P_{95}$ = the 95th percentile of returns (your rare big wins)
- $P_5$ = the 5th percentile of returns (your rare big losses)

| Tail Ratio | Interpretation |
|-----------|---------------|
| > 1.0 | Your rare big wins are larger than your rare big losses — favorable tail shape |
| = 1.0 | Symmetric tails — extreme gains and losses are similar in magnitude |
| < 1.0 | Your rare big losses exceed your rare big wins — unfavorable "fat left tail" |

---

## Why It Matters

Standard metrics like Sharpe Ratio assume returns are bell-curve shaped. In reality, trading returns often have **fat tails** — extreme events happen more frequently than a normal distribution predicts.

- A tail ratio > 1.0 means your "blow-up" risk is smaller than your "jackpot" upside.
- A tail ratio < 1.0 is a warning sign: when things go wrong, they go very wrong.

---

## Types of Fat Tails

| Term | Meaning |
|------|---------|
| **Leptokurtic** | Fatter tails than normal — more extreme events (common in markets) |
| **Platykurtic** | Thinner tails — fewer extreme events |
| **Positive skew** | Right tail is longer — rare big gains |
| **Negative skew** | Left tail is longer — rare big losses |

---

## How Zorivest Uses It

The tail ratio appears in the **Overview** tab of Analytics. It's computed from your trade P&L distribution using the 5th and 95th percentiles of returns. Values above 1.0 indicate your system handles extreme scenarios well.

---

## References

- Taleb, N.N. (2007). *The Black Swan: The Impact of the Highly Improbable*. Random House.
- Mandelbrot, B. & Hudson, R. (2004). *The (Mis)Behavior of Markets*. Basic Books.
