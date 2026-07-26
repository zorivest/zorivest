# Slippage

## What Is It?

**Slippage** is the difference between the expected price of a trade and the actual execution price. It represents the cost of imperfect execution.

---

## The Math

$$
\text{Slippage (bps)} = \frac{|P_{\text{fill}} - P_{\text{expected}}|}{P_{\text{expected}}} \times 10{,}000
$$

Where:
- $P_{\text{fill}}$ = your actual fill price
- $P_{\text{expected}}$ = the expected price (typically the NBBO midpoint)
- 1 basis point (bps) = 0.01%

**Example**: Expected price $100.00, filled at $100.03 → slippage = 3 bps.

---

## Types of Slippage

| Type | Cause | Controllable? |
|------|-------|--------------|
| **Market impact** | Your order moves the market (large orders) | Partially — use limit orders, TWAP/VWAP |
| **Latency** | Price moves between order submission and execution | Partially — use faster connections |
| **Spread crossing** | Market orders fill at the ask (buy) or bid (sell) | Yes — use limit orders |
| **PFOF routing** | Broker routes to market maker for worse fill | Somewhat — choose DMA broker |

---

## Typical Slippage Ranges

| Market | Typical Slippage |
|--------|-----------------|
| Large-cap stocks (SPY, AAPL) | 0–2 bps |
| Mid-cap stocks | 2–5 bps |
| Small-cap / illiquid stocks | 5–20+ bps |
| Options | 10–50+ bps |

---

## How Zorivest Uses It

The **Exec Quality** tab measures slippage in basis points by comparing your fill price to the prevailing NBBO. The letter grade (A–F) is based on slippage magnitude.

---

## References

- Harris, L. (2003). *Trading and Exchanges: Market Microstructure for Practitioners*. Oxford University Press.
