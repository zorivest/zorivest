# Kelly Criterion

## What Is It?

The **Kelly Criterion** is a formula for determining the optimal fraction of your capital to risk on each trade to maximize long-term growth. Developed by **John L. Kelly Jr.** at Bell Labs in 1956.

---

## The Math

### Kelly Percentage

$$
K\% = W' - \frac{L'}{R}
$$

Where:
- $W'$ = Conditional win rate = $\dfrac{\text{wins}}{\text{wins} + \text{losses}}$
- $L'$ = Conditional loss rate = $\dfrac{\text{losses}}{\text{wins} + \text{losses}}$
- $R$ = Payoff ratio (average win ÷ average loss)

### The rates are conditional — `KELLY_POPULATION`, not every trade

Kelly's derivation is a **two-outcome** wager: the stake either wins $R$ or
loses $1$. There is no third branch. So $W'$ and $L'$ must be taken over the
**non-scratch** subset of the rated trades — the population Zorivest calls
`KELLY_POPULATION`:

$$
\texttt{KELLY\_POPULATION} = \text{wins} + \text{losses}
$$

which is the rated population **minus scratches** (round trips that realised
exactly zero — see expectancy.md for the three-way partition).

Using the unconditional rates instead — wins over *all* rated trades — makes the
prescribed stake drift downward with every break-even trade recorded, which is
not information the growth-rate optimiser is defined over. A system that adds
100 scratches has not become worse at betting; it has only traded more. The
conditional rates are scratch-invariant by construction, which is the property
Kelly needs and the unconditional ones do not have.

**Example**: 55 wins, 45 losses, 1.5:1 payoff ratio:
$$
K\% = 0.55 - \frac{0.45}{1.5} = 0.55 - 0.30 = 25\%
$$

Append 100 scratches and $K\%$ is unchanged at 25% — the wins and losses are
still 55 and 45, so $W'$ and $L'$ are still $0.55$ and $0.45$.

This means betting 25% of capital per trade maximizes geometric growth rate.

### When Kelly is undefined

If `KELLY_POPULATION` is empty — every rated trade scratched — then $W'$ and
$L'$ are both $\tfrac{0}{0}$ and the question was never posed. Zorivest reports
`kelly_fraction` as **`null`** in that case, and every surface renders it as
*unavailable* rather than as a number.

`0` would be the wrong answer, not a safe one: zero is inside Kelly's value
domain — it is exactly the reading a negative-edge system earns, meaning *stake
nothing* — so publishing it would state a verdict on a system that has produced
no evidence either way.

---

## Why Most Traders Use "Fractional Kelly"

Full Kelly sizing maximizes long-term growth but comes with **extreme drawdowns** along the way. In practice:

| Fraction | Risk Level | Typical Use |
|----------|-----------|-------------|
| Full Kelly (100%) | Very aggressive — expects 50%+ drawdowns | Almost never used |
| Half Kelly (50%) | Aggressive but manageable | Experienced systematic traders |
| Quarter Kelly (25%) | Conservative | Most recommended |
| Tenth Kelly (10%) | Very conservative | New systems with limited data |

Half Kelly achieves ~75% of the growth rate with significantly less volatility and drawdown.

---

## Limitations

- Assumes you know your true win rate and payoff ratio — in practice, these are estimates.
- Assumes a binary outcome. Real trading produces a third, the scratch, which is
  handled by excluding it from `KELLY_POPULATION` rather than by folding it into
  either rate.
- Assumes trades are independent — correlated losses violate this assumption.
- Overestimates optimal size with small sample sizes.
- Does not account for transaction costs, slippage, or margin requirements.

---

## How Zorivest Uses It

The **Kelly %** in the Overview tab shows the full Kelly fraction computed from your historical **conditional** win rate and payoff ratio — that is, over `KELLY_POPULATION`, so it does not move when you record a break-even trade. It's meant as a theoretical upper bound — most traders should use ¼ to ½ of the displayed value.

When the account has no wins and no losses to draw on, the figure reads
*unavailable* instead of a percentage.

The **Position Calculator** can also apply a Kelly-based position size as one of its sizing methods.

---

## References

- Kelly, J.L. (1956). "A New Interpretation of Information Rate." *Bell System Technical Journal*, 35(4), 917–926.
- Thorp, E.O. (2006). "The Kelly Criterion in Blackjack, Sports Betting, and the Stock Market." *Handbook of Asset and Liability Management*.
