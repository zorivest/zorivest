# FIFO and LIFO — Cost Basis Methods

## What Are They?

**FIFO** (First In, First Out) and **LIFO** (Last In, First Out) are methods for determining which shares you're selling when you have multiple lots of the same security purchased at different times and prices.

---

## How They Work

Suppose you bought AAPL in three lots:

| Lot | Date | Shares | Price |
|-----|------|--------|-------|
| 1 | Jan 1 | 100 | $150 |
| 2 | Mar 1 | 100 | $160 |
| 3 | May 1 | 100 | $170 |

You sell 100 shares at $180. Which lot is being sold?

| Method | Lot Sold | Cost Basis | Gain |
|--------|----------|-----------|------|
| **FIFO** | Lot 1 ($150) | $15,000 | $3,000 |
| **LIFO** | Lot 3 ($170) | $17,000 | $1,000 |
| **HIFO** | Lot 3 ($170) | $17,000 | $1,000 |

---

## Common Methods

| Method | Rule | Tax Impact |
|--------|------|-----------|
| **FIFO** | Sell oldest shares first | Often highest gain (shares bought cheapest) |
| **LIFO** | Sell newest shares first | Often lower gain (recent price closer to current) |
| **HIFO** | Sell highest-cost shares first | Minimizes taxable gain |
| **Specific ID** | You choose which lot to sell | Maximum flexibility |

---

## Tax Implications

- **FIFO** is the IRS default if you don't specify.
- **Specific ID** requires identifying the exact lot at time of sale and broker confirmation.
- **HIFO** is a popular tax-optimization strategy — it minimizes current-year gains.
- All methods eventually result in the same total tax (you're just timing when you pay).

---

## How Zorivest Uses It

- **Round Trips tab**: Matches buys and sells using FIFO to create complete round-trip trades.
- **Tax Lots**: The sync engine creates tax lots from trade executions using your configured cost basis method.
- **Tax Alpha**: Compares your actual cost basis method against FIFO to compute tax savings from lot optimization.

---

## References

- IRS Publication 550, §4: "Identifying Stock or Bonds Sold."
- IRS Publication 551: "Basis of Assets."
