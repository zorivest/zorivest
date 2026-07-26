# Notional Amount

## What Is It?

The **notional amount** (or **notional value**) is the total dollar value of a position. It represents your full market exposure, regardless of how much capital you actually put up.

---

## The Math

$$
\text{Notional} = \text{Shares} \times \text{Price per Share}
$$

**Example**: 500 shares of AAPL at $200 = $100,000 notional.

For options:
$$
\text{Notional} = \text{Contracts} \times 100 \times \text{Underlying Price}
$$

**Example**: 10 AAPL call contracts at $200 underlying = $200,000 notional (even if you paid only $5,000 in premium).

---

## Why It Matters

- **PFOF cost scaling**: PFOF is typically charged per-share, so notional determines the total hidden cost.
- **Risk sizing**: Your true risk exposure is the notional value, not just your margin or premium paid.
- **Portfolio concentration**: Notional helps you see how much of your capital is exposed to a single position.

---

## How Zorivest Uses It

The **PFOF** tab multiplies estimated PFOF rate × notional value to compute the hidden cost of each trade. Larger notional trades have proportionally larger estimated PFOF costs.

---

## References

- Hull, J.C. (2018). *Options, Futures, and Other Derivatives*. Pearson. Chapter 1: Introduction.
