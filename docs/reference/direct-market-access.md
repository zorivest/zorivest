# Direct Market Access (DMA)

## What Is It?

**Direct Market Access (DMA)** means your order is sent directly to an exchange (NYSE, NASDAQ, CBOE, etc.) without being routed through a middleman or market maker.

---

## DMA vs. PFOF Routing

| Feature | DMA Broker | PFOF Broker |
|---------|-----------|-------------|
| Order routing | Directly to exchange | To market maker (Citadel, Virtu, etc.) |
| Commission | Explicit fee ($0.005/share typical) | $0 |
| Fill quality | Generally better — access to full order book | May be slightly worse |
| Transparency | Full control over routing | Broker chooses routing |
| Examples | IBKR Pro, TradeStation | Robinhood, Webull, Schwab |

---

## Why DMA Matters

- **Better fills**: Direct access to the exchange order book means tighter spreads.
- **No PFOF cost**: You pay an explicit commission, but avoid the hidden cost of worse fills.
- **Order type flexibility**: Access to advanced order types (iceberg, peg, etc.).
- **Transparency**: You know exactly where your order is going.

---

## When PFOF Is Acceptable

For small retail orders (< 500 shares in liquid stocks), the PFOF cost is often negligible and may even result in **price improvement** vs. the NBBO. Zero-commission platforms can be cost-effective for casual, small-lot trading.

---

## How Zorivest Uses It

Zorivest's PFOF analysis assigns each account a routing type (DMA or PFOF). DMA accounts show $0 estimated PFOF cost. The **Cost of Free** metric compares what you paid implicitly (via PFOF) vs. what you would have paid in explicit commissions on a DMA broker.

---

## References

- SEC. "Concept Release on Equity Market Structure." Release No. 34-61358 (2010).
- Harris, L. (2003). *Trading and Exchanges*. Oxford University Press.
