# Payment for Order Flow (PFOF)

## What Is It?

**Payment for Order Flow (PFOF)** is a practice where a brokerage receives compensation from a market maker (wholesale dealer) in exchange for routing customer orders to that market maker for execution.

When you place a stock order on a "commission-free" platform, your broker may send that order to a firm like Citadel Securities or Virtu Financial. That firm pays your broker a small fee per share (typically $0.002–$0.004/share for equities, more for options) for the right to fill your order.

---

## How It Works

```
You → Place order → Broker → Routes to Market Maker → Fills your order
                       ↑                    |
                       └── Receives payment ─┘
```

1. You submit a buy or sell order.
2. Your broker routes the order to a market maker instead of directly to an exchange.
3. The market maker executes your trade and pays your broker a small fee.
4. You get your fill — but potentially at a slightly worse price than the best available on exchanges.

---

## The Hidden Cost

The market maker profits by filling your order at a price slightly worse than the **NBBO** (National Best Bid and Offer). This gap is called **price improvement** when it's in your favor, or **implicit cost** when it's not.

| Scenario | You Pay |
|----------|---------|
| DMA broker (e.g., IBKR Pro) | Explicit commission (~$1/trade), but best price |
| PFOF broker (e.g., Robinhood) | $0 commission, but potentially worse fill price |

For small orders (< 1,000 shares), the PFOF cost is often negligible. For large or frequent orders, it can exceed what a commission would have been.

---

## Zorivest's PFOF Analysis

Zorivest estimates PFOF cost by:

1. Looking at your broker's routing type (DMA vs. PFOF).
2. Applying industry-average PFOF rates per share.
3. Multiplying by your trade volume to estimate total hidden cost.
4. Comparing this to what you would have paid in explicit commissions on a DMA broker.

The **"Cost of Free"** metric is the dollar amount you implicitly paid through worse fills on a zero-commission platform.

---

## Regulatory Context

- **SEC Rule 606** requires brokers to disclose order routing practices quarterly.
- The SEC considered banning PFOF in 2022–2023 but ultimately proposed enhanced disclosure requirements instead.
- PFOF is banned in the UK, Canada, and Australia.

---

## References

- SEC. "Payment for Order Flow." [sec.gov/fast-answers/answerspaborhtm.html](https://www.sec.gov/fast-answers/answerspaborhtm.html)
- SEC Rule 606 quarterly reports from major brokers.
