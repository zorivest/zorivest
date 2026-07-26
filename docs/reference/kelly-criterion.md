# Kelly Criterion

## What Is It?

The **Kelly Criterion** is a formula for determining the optimal fraction of your capital to risk on each trade to maximize long-term growth. Developed by **John L. Kelly Jr.** at Bell Labs in 1956.

---

## The Math

### Kelly Percentage

$$
K\% = W - \frac{1 - W}{R}
$$

Where:
- $W$ = Win rate (probability of winning)
- $R$ = Payoff ratio (average win ÷ average loss)

**Example**: 55% win rate, 1.5:1 payoff ratio:
$$
K\% = 0.55 - \frac{0.45}{1.5} = 0.55 - 0.30 = 25\%
$$

This means betting 25% of capital per trade maximizes geometric growth rate.

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
- Assumes trades are independent — correlated losses violate this assumption.
- Overestimates optimal size with small sample sizes.
- Does not account for transaction costs, slippage, or margin requirements.

---

## How Zorivest Uses It

The **Kelly %** in the Overview tab shows the full Kelly fraction computed from your historical win rate and payoff ratio. It's meant as a theoretical upper bound — most traders should use ¼ to ½ of the displayed value.

The **Position Calculator** can also apply a Kelly-based position size as one of its sizing methods.

---

## References

- Kelly, J.L. (1956). "A New Interpretation of Information Rate." *Bell System Technical Journal*, 35(4), 917–926.
- Thorp, E.O. (2006). "The Kelly Criterion in Blackjack, Sports Betting, and the Stock Market." *Handbook of Asset and Liability Management*.
