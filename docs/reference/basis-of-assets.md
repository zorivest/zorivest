# Basis of Assets (IRS Publication 551)

## What Is It?

**IRS Publication 551** explains how to determine the cost basis of various types of assets — the starting point for calculating gain or loss when you sell.

---

## Basis Determination Rules

### Purchased Assets

$$
\text{Basis} = \text{Purchase Price} + \text{Sales Tax} + \text{Commissions} + \text{Other Acquisition Costs}
$$

### Inherited Assets

- **Step-up in basis**: Basis = fair market value (FMV) on date of death.
- All prior gains are forgiven; no capital gains tax on appreciation during the decedent's lifetime.

### Gifted Assets

- If FMV at time of gift ≥ donor's basis → your basis = donor's basis (carryover basis).
- If FMV < donor's basis → your basis for loss = FMV at gift; for gain = donor's basis.

### Stock Splits and Dividends

- **Stock split**: Basis is divided among all shares (new + old).
- **Reinvested dividends**: Each reinvestment creates a new lot with basis = dividend amount.
- **Return of capital**: Reduces basis (doesn't create income until basis reaches $0).

---

## Why Basis Matters for Traders

Accurate basis determines:
- Whether a sale results in a gain or loss.
- Whether the gain is short-term or long-term.
- The amount of tax owed.
- Whether wash sale adjustments are correctly applied.

---

## Common Basis Errors

| Error | Impact |
|-------|--------|
| Missing commissions from basis | Overstates gains |
| Ignoring wash sale adjustments | Understates basis on replacement shares |
| Forgetting reinvested dividends | Overstates gains on sale |
| Wrong cost basis method | Different gain/loss amount |

---

## How Zorivest Uses It

The **Audit** tab scans for basis-related data quality issues:
- Missing or zero cost basis on lots.
- Orphaned lots without linked trades.
- Inconsistencies between trade data and lot basis.
- Invalid proceeds on closed lots.

---

## References

- IRS Publication 551: "Basis of Assets."
- IRS Publication 550, §4: "Basis of Investment Property."
