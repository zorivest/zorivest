# Behavioral Features Guide

> **Learn by doing:** This guide walks you through Zorivest's 6 behavioral analysis features using a single realistic trade scenario. By the end, you'll know how to journal trades, track mistakes, get AI feedback, and uncover behavioral patterns costing you money.

---

## The Scenario

You're a swing trader. On **May 19, 2026**, you see NVIDIA (NVDA) spike pre-market after an earnings beat. Headlines everywhere: *"NVDA crushes estimates, AI demand surges."* You feel the FOMO building and buy 200 shares at **$142.50** without checking the chart or setting a stop loss. Your normal position size is $10K–$15K — this one is **$28,500**.

Two days later, on **May 21**, a broader tech sell-off drags NVDA down. With no stop loss and an oversized position, panic sets in. You sell at **$131.75**, locking in a **$2,152 loss**.

Now let's use Zorivest to review this trade and learn from it.

---

## Step 1: Create a Trade Report

> **What it does:** Creates a structured post-trade review where you grade your setup, execution, emotional state, and capture lessons learned.

After closing the NVDA trade, create a review report:

```
zorivest_report(
  action: "create",
  trade_id: "NVDA-SLD-20260521",
  setup_quality: "F",
  execution_quality: "D",
  followed_plan: false,
  emotional_state: "fearful",
  lessons_learned: "Never enter a position based on headlines alone.
    Always wait for a pullback and set a stop loss before entering.
    The FOMO cost me $2,152."
)
```

**What you get back:**

```json
{
  "id": 3,
  "trade_id": "NVDA-SLD-20260521",
  "setup_quality": "F",
  "execution_quality": "D",
  "followed_plan": false,
  "emotional_state": "fearful",
  "lessons_learned": "Never enter a position based on headlines alone..."
}
```

**Why it matters:** You're not just tracking P&L — you're capturing *why* the trade went wrong. Over time, you'll see patterns: maybe your worst trades always happen when you're "fearful" or when `followed_plan: false`.

### Grading Scale

| Grade | Meaning |
|-------|---------|
| **A** | Textbook setup/execution — followed plan perfectly |
| **B** | Solid — minor improvements possible |
| **C** | Average — some rules broken, acceptable outcome |
| **D** | Poor — multiple rules broken, preventable loss |
| **F** | Failed — no plan, pure emotion, broke every rule |

### Emotional States

`calm` · `anxious` · `fearful` · `greedy` · `frustrated` · `confident` · `neutral`

---

## Step 2: Track Your Mistakes

> **What it does:** Tags a trade with specific mistake categories and an estimated dollar cost. You can tag the same trade with multiple mistakes.

This NVDA trade had **three** distinct mistakes. Tag each one:

### Mistake 1: FOMO Entry

```
zorivest_report(
  action: "track_mistake",
  trade_exec_id: "NVDA-SLD-20260521",
  category: "fomo_entry",
  estimated_cost: 1200,
  notes: "Entered based on earnings headline hype
    without checking the chart or waiting for a pullback"
)
```

### Mistake 2: No Stop Loss

```
zorivest_report(
  action: "track_mistake",
  trade_exec_id: "NVDA-SLD-20260521",
  category: "no_stop",
  estimated_cost: 800,
  notes: "No stop loss set. Would have been stopped out at $138
    for a $900 loss instead of $2,152"
)
```

### Mistake 3: Oversized Position

```
zorivest_report(
  action: "track_mistake",
  trade_exec_id: "NVDA-SLD-20260521",
  category: "oversized",
  estimated_cost: 500,
  notes: "200 shares at $142.50 = $28,500.
    Normal position size is $10,000-15,000"
)
```

**What you get back** (for each):

```json
{
  "trade_exec_id": "NVDA-SLD-20260521",
  "category": "fomo_entry",
  "estimated_cost": "1200.0",
  "notes": "Entered based on earnings headline hype..."
}
```

**Why it matters:** You can't fix what you don't measure. The `estimated_cost` forces you to quantify each mistake — not just "I messed up" but "this specific mistake cost me $1,200." Over time, you'll know exactly which habits to prioritize fixing.

### Available Mistake Categories

| Category | Description |
|----------|-------------|
| `fomo_entry` | Entered because of fear of missing out |
| `late_exit` | Held too long, gave back profits |
| `early_exit` | Sold too early, missed further gains |
| `oversized` | Position too large for account size |
| `no_stop` | Didn't set or honor a stop loss |
| `revenge_trade` | Traded to "get back" after a loss |
| `ignored_plan` | Had a plan, didn't follow it |
| `overtrading` | Too many trades, churning the account |
| `chasing` | Entered after a move was already extended |
| `bad_rr` | Risk/reward ratio was unfavorable |
| `stress` | Traded while stressed or distracted |
| `other` | Anything not covered above |

---

## Step 3: Link Trade to Journal

> **What it does:** Creates a bidirectional link between your trade execution and your review report, so you can jump from trade → review or review → trade.

```
zorivest_report(
  action: "journal_link",
  trade_exec_id: "NVDA-SLD-20260521",
  report_id: "3"
)
```

**What you get back:**

```json
{
  "id": 3,
  "trade_id": "NVDA-SLD-20260521",
  "linked": true
}
```

**Why it matters:** When you're reviewing your P&L months later and see a –$2,152 NVDA loss, one click takes you to your review: the emotional state (fearful), the grades (F/D), the lessons learned. Your journal becomes a connected knowledge base, not a pile of disconnected notes.

---

## Step 4: Get AI Review

> **What it does:** Sends your trade to AI personas (risk manager, technical analyst, behavioral coach) for multi-perspective feedback.

```
zorivest_analytics(
  action: "ai_review",
  trade_exec_id: "NVDA-SLD-20260521"
)
```

**What you get back:**

When no external AI provider is configured, Zorivest automatically activates **agent-fallback mode** — it fetches your trade data and instructs the calling AI agent to perform the review itself:

```json
{
  "success": true,
  "ai_provider": "agent_fallback",
  "message": "No external AI provider configured. You (the calling AI agent) should perform the trade review yourself using the trade data below.",
  "trade_data": {
    "exec_id": "NVDA-SLD-20260521",
    "instrument": "NVDA",
    "action": "SLD",
    "quantity": 200,
    "price": 131.75,
    "realized_pnl": -2152,
    "notes": "Panic sold after NVDA dropped on broader tech sell-off. No stop loss was in place."
  },
  "instructions": [
    "Analyze this trade from 3 perspectives:",
    "1. RISK MANAGER: Evaluate position sizing, stop loss placement, risk/reward ratio.",
    "2. TECHNICAL ANALYST: Assess entry/exit timing, support/resistance, trend direction.",
    "3. BEHAVIORAL COACH: Identify emotional patterns, plan adherence, improvements.",
    "For each persona, provide: (a) a letter grade A-F, (b) 2-3 observations, (c) one actionable improvement.",
    "End with an overall trade score and the single most important lesson."
  ]
}
```

The AI agent receiving this will then produce a multi-persona review like:

> - **Risk Manager (Grade: F):** "Position size of $28,500 is 190% of your normal range. The lack of a stop loss on an earnings play violates risk management basics. *Action: Never risk more than 2% of account on a single trade.*"
> - **Technical Analyst (Grade: D):** "NVDA was already extended 8% above its 20-day MA at entry. The chart suggested waiting for a pullback to the $135 support level. *Action: Wait for a pullback to at least the 10-day MA before entering.*"
> - **Behavioral Coach (Grade: F):** "Classic FOMO pattern — entering on headlines without chart confirmation. This is compounded by oversized positioning driven by overconfidence. *Action: Create a pre-trade checklist that must be completed before any entry.*"

> **Tip:** Once you configure an external AI provider (OpenAI, Anthropic, DeepSeek, etc.) in settings, reviews will be generated by that provider instead. The agent-fallback ensures you always get a review, even without a configured provider.

**Why it matters:** You get objective, multi-angle feedback on every trade — like having three mentors reviewing your work. The budget cap ensures you control AI costs (default $0.10 per review).

---

## Step 5: Check Options Strategy

> **What it does:** When you trade multiple option legs, this auto-detects what strategy you built (spread, straddle, iron condor, etc.).

```
zorivest_analytics(
  action: "options_strategy",
  leg_exec_ids: ["NVDA-BUY-20260519", "NVDA-SLD-20260521"]
)
```

**What you get back:**

```json
{
  "strategy_type": "unknown",
  "leg_count": 0,
  "legs": [],
  "is_complete": true
}
```

**Why this returned "unknown":** Our NVDA trades are equity (stock) trades, not options. The strategy detector correctly identifies them as not forming an options strategy.

> **When it shines:** If you had bought an AAPL 200 Call and sold an AAPL 210 Call, the detector would return:
>
> ```json
> {
>   "strategy_type": "bull_call_spread",
>   "leg_count": 2,
>   "net_premium": -150.00,
>   "max_risk": 850.00,
>   "max_profit": 150.00
> }
> ```

**Why it matters:** Many traders build multi-leg options strategies without formally tracking them. This auto-labels your positions so your journal, P&L reporting, and risk calculations are accurate.

---

## Step 6: View Mistake Summary

> **What it does:** Aggregates all your tracked mistakes into a dashboard — counts by category, total dollar cost, broken down by type.

```
zorivest_analytics(
  action: "mistake_summary"
)
```

**What you get back:**

```json
{
  "by_category": {
    "fomo_entry": 2,
    "no_stop": 2,
    "revenge_trade": 1,
    "oversized": 2
  },
  "total_cost": "4250.00",
  "total_entries": 7,
  "period": "ytd"
}
```

**Reading this data:**

- **`fomo_entry: 2`** → You've FOMO'd into 2 trades this year
- **`no_stop: 2`** → Twice you forgot/ignored stop losses
- **`oversized: 2`** → Twice your position was too big
- **`total_cost: $4,250`** → These 7 mistakes cost you $4,250 total

**The insight:** FOMO entries and missing stop losses are your biggest problems. If you fixed just these two habits, you could potentially save thousands per year.

You can also filter by account:

```
zorivest_analytics(
  action: "mistake_summary",
  account_id: "99bb9b00-fc7a-44cf-b816-a6bb4dabfaca"
)
```

**Why it matters:** This is where behavioral self-improvement becomes data-driven. Instead of vaguely feeling like "I need to be more disciplined," you can see: *"FOMO entries cost me $3,400 across 5 trades this quarter. That's my #1 priority to fix."*

---

## The Complete Workflow

Here's how all 6 features fit together in your post-trade routine:

```
Close a trade
    │
    ▼
📝  Step 1: CREATE REPORT
    Grade setup (A-F), execution (A-F),
    record emotional state, lessons learned
    │
    ▼
🏷️  Step 2: TRACK MISTAKES
    Tag what went wrong (FOMO, no stop, oversized...)
    Estimate the cost of each mistake
    │
    ▼
🔗  Step 3: JOURNAL LINK
    Connect trade ↔ report for easy navigation
    │
    ▼
🧠  Step 4: AI REVIEW
    Get multi-persona AI feedback on your trade
    │
    ▼
📊  Step 5: OPTIONS STRATEGY (if applicable)
    Auto-detect what strategy your option legs form
    │
    ▼
📈  Step 6: MISTAKE SUMMARY (weekly/monthly)
    Review your mistake patterns and costs over time
    Identify your most expensive habits
    Prioritize what to fix first
```

### Pro Tips

1. **Be honest with grades.** An "F" setup that you acknowledge teaches more than a fake "B" you forget.
2. **Tag multiple mistakes.** Most bad trades have 2-3 compounding errors (like our NVDA: FOMO + no stop + oversized).
3. **Estimate costs conservatively.** Even a rough estimate creates accountability. "What would this trade have looked like if I hadn't made this mistake?"
4. **Review the summary weekly.** The real power comes from aggregate patterns, not individual trades.
5. **Use the AI review sparingly.** Focus it on your worst losses and your best wins — those have the most to teach.

---

## Quick Reference

| Feature | Tool | Action | Key Inputs |
|---------|------|--------|------------|
| Create Report | `zorivest_report` | `create` | `trade_id`, `setup_quality`, `execution_quality`, `emotional_state`, `followed_plan`, `lessons_learned` |
| Track Mistake | `zorivest_report` | `track_mistake` | `trade_exec_id`, `category`, `estimated_cost`, `notes` |
| Journal Link | `zorivest_report` | `journal_link` | `trade_exec_id`, `report_id` |
| AI Review | `zorivest_analytics` | `ai_review` | `trade_exec_id`, (optional: `budget_cap`) |
| Options Strategy | `zorivest_analytics` | `options_strategy` | `leg_exec_ids` (array of 2+ exec IDs) |
| Mistake Summary | `zorivest_analytics` | `mistake_summary` | (optional: `account_id`, `period`) |
