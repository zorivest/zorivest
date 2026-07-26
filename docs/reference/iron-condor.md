# Iron Condor

## What Is It?

An **iron condor** is a four-leg options strategy that profits when the underlying stock stays within a defined price range. It combines a bull put spread and a bear call spread.

---

## Structure

| Leg | Type | Side | Strike |
|-----|------|------|--------|
| 1 | Put | Buy | Lowest strike (protection) |
| 2 | Put | Sell | Lower-mid strike |
| 3 | Call | Sell | Upper-mid strike |
| 4 | Call | Buy | Highest strike (protection) |

All four legs share the **same expiration date**.

```
P&L at Expiration:

  Max Profit ───────────────┐          ┌───────────────
                            │          │
               ─────────────┘          └─────────────
  Max Loss ─────┘                                └─────
              A     B                    C     D
              Buy   Sell              Sell   Buy
              Put   Put               Call   Call
```

---

## The Math

$$
\text{Max Profit} = \text{Net Credit Received}
$$

$$
\text{Max Loss} = \text{Width of Wider Spread} - \text{Net Credit}
$$

$$
\text{Break-even (lower)} = \text{Short Put Strike} - \text{Net Credit}
$$

$$
\text{Break-even (upper)} = \text{Short Call Strike} + \text{Net Credit}
$$

---

## Example

Stock at $100. Sell an iron condor:

| Leg | Strike | Premium |
|-----|--------|---------|
| Buy 90 Put | $90 | −$0.50 |
| Sell 95 Put | $95 | +$1.50 |
| Sell 105 Call | $105 | +$1.50 |
| Buy 110 Call | $110 | −$0.50 |

**Net credit**: $2.00  
**Max loss**: $5.00 − $2.00 = $3.00  
**Break-evens**: $93 and $107  
**Profit zone**: Stock stays between $93 and $107

---

## When to Use It

- **Low implied volatility expectation** — you believe the stock will stay range-bound.
- **Earnings have passed** — volatility crush benefits credit strategies.
- **Time decay is your friend** — theta works in your favor.

---

## How Zorivest Detects It

The options strategy detector identifies iron condors by matching 4 legs: 2 puts (1 long, 1 short) + 2 calls (1 long, 1 short) with the same expiration and the sell strikes inside the buy strikes.

---

## References

- Natenberg, S. (1994). *Option Volatility and Pricing*. McGraw-Hill.
- Sinclair, E. (2010). *Option Trading: Pricing and Volatility Strategies and Techniques*. Wiley.
