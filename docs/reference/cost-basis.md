# Cost Basis

## What Is It?

**Cost basis** is the original value of an investment for tax purposes. When you sell, your gain or loss is calculated as proceeds minus cost basis.

$$
\text{Gain/Loss} = \text{Proceeds} - \text{Cost Basis}
$$

---

## What's Included in Cost Basis

| Component | Included? |
|-----------|----------|
| Purchase price | ✅ Always |
| Commissions (buy side) | ✅ Yes |
| Commissions (sell side) | ❌ No (reduces proceeds instead) |
| Wash sale adjustments | ✅ Added to replacement shares |
| Stock splits | ✅ Basis is divided among new shares |
| Reinvested dividends | ✅ Increases basis |
| Return of capital | ❌ Reduces basis |

---

## Cost Basis Methods

See fifo-lifo.md for detailed comparison. Summary:

| Method | Rule | Best For |
|--------|------|---------|
| **FIFO** | Sell oldest first | IRS default |
| **LIFO** | Sell newest first | Lower short-term gains |
| **HIFO** | Sell highest-cost first | Minimize taxable gain |
| **Specific ID** | You choose | Maximum control |
| **Average Cost** | Average of all shares | Mutual funds only |

---

## Adjusted Basis

Your basis can be adjusted up or down after purchase:

| Adjustment | Direction |
|-----------|-----------|
| Wash sale disallowed loss | ↑ Added to replacement basis |
| Stock split | ↔ Divided among new shares |
| Return of capital dividend | ↓ Reduces basis |
| Reinvested dividend | ↑ New lot at dividend price |

---

## How Zorivest Uses It

- **Tax Lots tab**: Shows cost basis for each lot, including adjustments.
- **Process Tax Lots**: Creates lots from trade executions with basis = purchase price.
- **Wash Sale Scanner**: Adjusts basis when disallowed losses are absorbed.
- **Tax Alpha Report**: Compares your actual basis method vs. FIFO counterfactual.

---

## References

- IRS Publication 550, §4: "Basis of Investment Property."
- IRS Publication 551: "Basis of Assets."
