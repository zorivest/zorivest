# Federal Tax Brackets & Rate Schedules

## What Are They?

The US federal income tax uses a **progressive** bracket system — different portions of your income are taxed at different rates. Each year, the IRS adjusts bracket thresholds for inflation.

---

## How Progressive Brackets Work

Your income fills each bracket sequentially. Only the portion *within* each bracket is taxed at that bracket's rate.

**Example** (simplified, 2025 Single):

| Bracket | Rate | Income Range |
|---------|------|-------------|
| 1 | 10% | $0 – $11,925 |
| 2 | 12% | $11,926 – $48,475 |
| 3 | 22% | $48,476 – $103,350 |
| 4 | 24% | $103,351 – $197,300 |
| 5 | 32% | $197,301 – $250,525 |
| 6 | 35% | $250,526 – $626,350 |
| 7 | 37% | Over $626,350 |

A person earning $60,000 pays:
- 10% on the first $11,925 = $1,192.50
- 12% on $11,926–$48,475 = $4,386
- 22% on $48,476–$60,000 = $2,535
- **Total**: $8,113.50 (effective rate: ~13.5%)

---

## Long-Term Capital Gains Rates (Tax Year 2025)

Long-term capital gains (assets held > 1 year) are taxed at preferential rates. Thresholds are inflation-adjusted annually by the IRS.

| Filing Status | 0% Rate Up To | 15% Rate Up To | 20% Rate Above |
|--------------|---------------|----------------|----------------|
| Single | $48,350 | $533,400 | Over $533,400 |
| Married Joint | $96,700 | $600,050 | Over $600,050 |
| Head of Household | $64,750 | $566,700 | Over $566,700 |
| Married Separate | $48,350 | $300,000 | Over $300,000 |

*Source: [IRS Topic 409 — Capital Gains and Losses](https://www.irs.gov/taxtopics/tc409) and [IRS Rev. Proc. 2024-40](https://www.irs.gov/filing/federal-income-tax-rates-and-brackets).*

Short-term capital gains are taxed as ordinary income (using the brackets above).

---

## Additional Taxes

- **NIIT (Net Investment Income Tax)**: 3.8% surcharge on investment income above $200K (single) / $250K (joint). Thresholds are fixed by statute (not inflation-adjusted).
- **State taxes**: Vary by state — see your Tax Profile in Zorivest.

---

## How Zorivest Uses It

The **Tax Schedules** tab stores IRS bracket data by year and filing status. The tax engine walks the bracket table to compute marginal taxes on your capital gains. Schedules for 2025 and 2026 are seeded automatically from IRS Revenue Procedures.

You can create custom schedules for projected future years to model tax scenarios.

---

## Source

IRS Revenue Procedure — published annually with inflation-adjusted figures. The most recent is for tax year 2026.
