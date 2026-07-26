# Wash Sales

## What Is It?

A **wash sale** occurs when you sell a security at a loss and buy the same or a "substantially identical" security within **30 days before or after** the sale. The IRS disallows the loss deduction — it is deferred, not eliminated.

---

## The Rule

**IRC Section 1091**: If you sell stock or securities at a loss and, within 30 days before or after the sale, you:
- Buy substantially identical stock or securities,
- Acquire substantially identical stock or securities in a fully taxable trade,
- Acquire a contract or option to buy substantially identical stock or securities

…then the loss is **disallowed** for the current tax year.

---

## The 61-Day Window

```
←── 30 days before ──→ Sale Date ←── 30 days after ──→
     Wash sale zone                  Wash sale zone
```

The window is **61 calendar days** total (30 before + sale day + 30 after).

---

## What Happens to the Disallowed Loss?

The disallowed loss is **added to the cost basis** of the replacement shares:

| Event | Amount |
|-------|--------|
| Original purchase | 100 shares at $50 = $5,000 basis |
| Sold at loss | 100 shares at $40 = $4,000 proceeds → $1,000 loss |
| Repurchased within 30 days | 100 shares at $42 |
| **Result** | Loss $1,000 is disallowed; new basis = $42 + $10 = $52/share |

The loss isn't gone — it's **deferred** into the higher basis of the replacement shares.

---

## "Substantially Identical"

The IRS has not precisely defined this term, but generally:

| Same? | Wash Sale? |
|-------|-----------|
| Same stock (AAPL → AAPL) | ✅ Yes |
| Same ETF (SPY → SPY) | ✅ Yes |
| Different ETFs tracking same index (SPY → IVV) | ⚠️ Grey area |
| Stock → option on same stock | ✅ Yes |
| Different companies in same sector | ❌ No |

---

## How Zorivest Detects Wash Sales

1. **Scan**: The wash sale scanner examines all closed lots with losses.
2. **Match**: For each loss, it searches for purchases of the same ticker within the 61-day window.
3. **Chain**: When a wash sale is detected, a chain is created linking the loss lot to the replacement lot.
4. **Adjust**: The disallowed loss amount is recorded and added to the replacement lot's cost basis.

The **Wash Sales** tab shows all detected chains with their status (ABSORBED = deferred into replacement, DESTROYED = replacement also sold at a loss).

---

## References

- IRC § 1091: "Loss from wash sales of stock or securities."
- IRS Publication 550, §4: "Wash Sales."
