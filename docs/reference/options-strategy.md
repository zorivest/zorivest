# Options Strategies

## What Are They?

An **options strategy** is a combination of one or more option contracts (calls and/or puts) designed to create a specific risk/reward profile. Each individual option contract in a combo trade is called a **leg**.

---

## Common Strategies

### Single-Leg

| Strategy | Legs | View | Max Profit | Max Loss |
|----------|------|------|-----------|----------|
| **Long Call** | 1 call (buy) | Bullish | Unlimited | Premium paid |
| **Long Put** | 1 put (buy) | Bearish | Strike − Premium | Premium paid |
| **Covered Call** | Long stock + 1 call (sell) | Neutral/mildly bullish | Premium + (strike − entry) | Entry − Premium |

### Vertical Spreads (2 legs)

| Strategy | Legs | View | Max Profit | Max Loss |
|----------|------|------|-----------|----------|
| **Bull Call Spread** | Buy lower call, sell higher call | Bullish | Width − debit | Debit paid |
| **Bear Put Spread** | Buy higher put, sell lower put | Bearish | Width − debit | Debit paid |
| **Bull Put Spread** | Sell higher put, buy lower put | Bullish | Credit received | Width − credit |

### Multi-Leg (3–4 legs)

| Strategy | Legs | View | Max Profit | Max Loss |
|----------|------|------|-----------|----------|
| **Iron Condor** | 4 legs (put spread + call spread) | Neutral / range-bound | Net credit | Width − credit |
| **Iron Butterfly** | 4 legs (at same center strike) | Low volatility | Net credit | Width − credit |
| **Straddle** | Buy call + buy put (same strike) | High volatility | Unlimited | Premium paid |
| **Strangle** | Buy call + buy put (different strikes) | High volatility | Unlimited | Premium paid |

---

## Key Terminology

| Term | Definition |
|------|-----------|
| **Leg** | One individual option contract within a multi-leg strategy |
| **Width** | Distance between strikes in a spread |
| **Debit** | Net cost to enter the trade |
| **Credit** | Net premium received when entering the trade |
| **Greeks** | Delta, Gamma, Theta, Vega — sensitivity measures |

---

## How Zorivest Uses It

The **Options** tab in Trade Detail uses Zorivest's **options strategy detector** to automatically identify multi-leg structures from your executions. It groups related legs and labels the strategy type.

---

## References

- Hull, J.C. (2018). *Options, Futures, and Other Derivatives*. Pearson.
- Natenberg, S. (1994). *Option Volatility and Pricing*. McGraw-Hill.
