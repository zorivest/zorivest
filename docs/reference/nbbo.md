# NBBO (National Best Bid and Offer)

## What Is It?

The **NBBO** is the best available bid price (highest a buyer will pay) and the best available ask price (lowest a seller will accept) across all US exchanges at any given moment.

It is mandated by **SEC Regulation NMS** (National Market System), which requires brokers to execute customer orders at or better than the NBBO.

---

## How It Works

```
Exchange A:  Bid $50.10  /  Ask $50.15
Exchange B:  Bid $50.12  /  Ask $50.14
Exchange C:  Bid $50.11  /  Ask $50.16

NBBO:        Bid $50.12  /  Ask $50.14
              (best from B)  (best from B)
```

The NBBO is the **tightest possible spread** available anywhere in the market at that instant.

---

## Why It Matters for Execution Quality

Your fill price should be compared against the NBBO at the time of execution:

| Your Fill | vs. NBBO | Interpretation |
|-----------|----------|---------------|
| At NBBO | — | Fair fill |
| Better than NBBO | Price improvement | Great fill — broker got you a better deal |
| Worse than NBBO | Negative price improvement | Possible PFOF cost or adverse selection |

---

## NBBO and Zorivest

The **Exec Quality** tab uses the NBBO as the fairness benchmark. Slippage is measured as the distance between your actual fill and the prevailing NBBO. The execution grade (A–F) is based on how close your fill was to the NBBO.

---

## References

- SEC. "Regulation NMS." 17 CFR § 242.600–612.
- SEC. "National Market System Plan." [sec.gov](https://www.sec.gov/rules/sro/nms.htm)
