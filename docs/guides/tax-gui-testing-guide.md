# Tax GUI Testing Guide

> **Last Updated:** 2026-05-15 — Post Phase 3F (sync-lots pipeline wired)

## Overview

This guide walks through preparing test data and manually verifying every Tax GUI tab. Unlike the pre-3F version, **lot materialization is now live** via the "Process Tax Lots" button, so most features should render real data.

---

## Test Data Summary

All data is created via MCP tools before starting GUI testing.

### Accounts (2)

| Account | Type | Institution | Tax-Advantaged | Balance |
|---------|------|-------------|----------------|---------|
| **Tax Audit Broker** | BROKER | Interactive Brokers | ❌ No | $150,000 |
| **Tax Audit IRA** | IRA | Fidelity | ✅ Yes | $75,000 |

### Trades (9 total)

#### Taxable Broker Account — Closed Round Trips (3 pairs)

| Ticker | Buy Date | Buy Price | Sell Date | Sell Price | Qty | Expected P&L | Holding Period | Tax Scenario |
|--------|----------|-----------|-----------|------------|-----|-------------|----------------|--------------|
| **AAPL** | 2025-01-15 | $170.50 | 2025-04-15 | $195.00 | 100 | **+$2,450** | ~3 months | **Short-term gain** |
| **MSFT** | 2024-01-15 | $380.00 | 2025-03-10 | $435.00 | 75 | **+$4,125** | ~14 months | **Long-term gain** |
| **TSLA** | 2025-02-10 | $250.00 | 2025-04-20 | $210.00 | 50 | **−$2,000** | ~2 months | **Short-term loss** |

**Expected Dashboard totals:**
- ST Gains: $2,450 (AAPL) − $2,000 (TSLA) = **$450**
- LT Gains: **$4,125** (MSFT)
- Total Realized: **$4,575**

#### Taxable Broker Account — Open Positions (2)

| Ticker | Buy Date | Buy Price | Qty | Purpose |
|--------|----------|-----------|-----|---------|
| **NVDA** | 2025-03-01 | $850.00 | 40 | Open lot — simulator & harvesting testing |
| **TSLA** | 2025-05-02 | $215.00 | 30 | **Wash sale trigger** — re-bought 12 days after TSLA loss sale |

#### IRA Account — Open Position (1)

| Ticker | Buy Date | Buy Price | Qty | Purpose |
|--------|----------|-----------|-----|---------|
| **VTI** | 2025-01-15 | $240.00 | 200 | Tax-advantaged — should be excluded from taxable views |

### Wash Sale Timeline

```
TSLA sell at loss:  Apr 20, 2025  (−$2,000 loss)
TSLA re-buy:        May 2, 2025   (12 days later — WITHIN 30-day window)
→ Wash sale triggered: $2,000 disallowed loss, basis adjusted on new lot
```

### Trade Plans (2)

| Ticker | Strategy | Direction | Entry | Stop | Target | Conviction |
|--------|----------|-----------|-------|------|--------|------------|
| **AAPL** | Pullback Entry | Long | $165 | $155 | $195 | High |
| **NVDA** | Earnings Breakout | Long | $900 | $830 | $1,050 | Medium |

### Watchlists (2)

| Watchlist | Tickers | Purpose |
|-----------|---------|---------|
| **Tech Momentum** | NVDA, MSFT, AAPL | Swing trade candidates |
| **Tax Loss Harvest Candidates** | TSLA, AMD | Loss harvesting watch |

---

## MCP Data Preparation Commands

Run these in order to populate the database. Skip any that are already created from prior sessions.

### Step 1: Create Accounts

```
zorivest_account(action:"create", name:"Tax Audit Broker", account_type:"BROKER", institution:"Interactive Brokers", is_tax_advantaged:false)
zorivest_account(action:"create", name:"Tax Audit IRA", account_type:"IRA", institution:"Fidelity", is_tax_advantaged:true)
```

Record the returned `account_id` values — needed for all subsequent commands.

### Step 2: Record Balances

```
zorivest_account(action:"balance", account_id:"<broker_id>", balance:150000)
zorivest_account(action:"balance", account_id:"<ira_id>", balance:75000)
```

### Step 3: Create Trades

Get a confirmation token for each trade:
```
zorivest_system(action:"confirm_token", tool_action:"create trade")
```

#### Broker Account Trades (8 executions)

```
# AAPL buy
zorivest_trade(action:"create", account_id:"<broker_id>", instrument:"AAPL", trade_action:"BOT", quantity:100, price:170.50, time:"2025-01-15T10:00:00", confirmation_token:"<token>")

# AAPL sell
zorivest_trade(action:"create", account_id:"<broker_id>", instrument:"AAPL", trade_action:"SLD", quantity:100, price:195.00, time:"2025-04-15T14:00:00", confirmation_token:"<token>")

# MSFT buy
zorivest_trade(action:"create", account_id:"<broker_id>", instrument:"MSFT", trade_action:"BOT", quantity:75, price:380.00, time:"2024-01-15T10:00:00", confirmation_token:"<token>")

# MSFT sell
zorivest_trade(action:"create", account_id:"<broker_id>", instrument:"MSFT", trade_action:"SLD", quantity:75, price:435.00, time:"2025-03-10T14:00:00", confirmation_token:"<token>")

# TSLA buy (will be sold at loss)
zorivest_trade(action:"create", account_id:"<broker_id>", instrument:"TSLA", trade_action:"BOT", quantity:50, price:250.00, time:"2025-02-10T10:00:00", confirmation_token:"<token>")

# TSLA sell (loss — triggers wash sale with re-buy below)
zorivest_trade(action:"create", account_id:"<broker_id>", instrument:"TSLA", trade_action:"SLD", quantity:50, price:210.00, time:"2025-04-20T14:00:00", confirmation_token:"<token>")

# NVDA buy (open position)
zorivest_trade(action:"create", account_id:"<broker_id>", instrument:"NVDA", trade_action:"BOT", quantity:40, price:850.00, time:"2025-03-01T10:00:00", confirmation_token:"<token>")

# TSLA re-buy (wash sale trigger — within 30 days of loss sale)
zorivest_trade(action:"create", account_id:"<broker_id>", instrument:"TSLA", trade_action:"BOT", quantity:30, price:215.00, time:"2025-05-02T10:00:00", confirmation_token:"<token>")
```

#### IRA Account Trade (1 execution)

```
# VTI buy (tax-advantaged, should be excluded from taxable calcs)
zorivest_trade(action:"create", account_id:"<ira_id>", instrument:"VTI", trade_action:"BOT", quantity:200, price:240.00, time:"2025-01-15T10:00:00", confirmation_token:"<token>")
```

### Step 4: Create Trade Plans

```
zorivest_plan(action:"create", ticker:"AAPL", strategy_name:"Pullback Entry", direction:"long", entry:165, stop:155, target:195, conviction:"high", conditions:"Wait for RSI < 40 on daily", timeframe:"2-4 weeks")

zorivest_plan(action:"create", ticker:"NVDA", strategy_name:"Earnings Breakout", direction:"long", entry:900, stop:830, target:1050, conviction:"medium", conditions:"Above 50MA, volume > 2x avg", timeframe:"1-2 weeks")
```

### Step 5: Create Watchlists

```
zorivest_watchlist(action:"create", name:"Tech Momentum", description:"Swing trade candidates")
zorivest_watchlist(action:"add_ticker", watchlist_id:<id>, ticker:"NVDA")
zorivest_watchlist(action:"add_ticker", watchlist_id:<id>, ticker:"MSFT")
zorivest_watchlist(action:"add_ticker", watchlist_id:<id>, ticker:"AAPL")

zorivest_watchlist(action:"create", name:"Tax Loss Harvest Candidates", description:"Loss harvesting watch")
zorivest_watchlist(action:"add_ticker", watchlist_id:<id>, ticker:"TSLA")
zorivest_watchlist(action:"add_ticker", watchlist_id:<id>, ticker:"AMD")
```

---

## GUI Testing Walkthrough

### Prerequisites

- Backend running: `uv run fastapi dev packages/api/src/zorivest_api/main.py`
- UI running: `cd ui && npm run dev`
- All MCP data preparation steps above are complete

### Step 0: Process Tax Lots (CRITICAL — Do This First)

1. Navigate to **Tax** → **Dashboard** tab
2. Click **Process Tax Lots** button (top-right, purple)
3. **Wait** for "Sync complete" message
4. **Verify toast:** Shows `created: X, updated: Y, conflicts: 0`
5. **Expected:** At minimum 5-6 lots created (3 closed pairs + 2 open positions from broker account)

> [!IMPORTANT]
> This step materializes trades into TaxLot records. Without it, the Lots, Wash Sales, Harvesting, and Simulator tabs will show empty data. The sync only needs to run once — lots persist in the database.

### Test 1: Tax Dashboard

1. After sync, the **Dashboard** tab should auto-refresh
2. **Verify 7 summary cards render with labels:**
   - ST Gains — expected ~$450 (AAPL gain − TSLA loss)
   - LT Gains — expected ~$4,125 (MSFT)
   - Wash Sale Adj — expected ~$2,000 (TSLA)
   - Estimated Tax — non-zero (computed from gains)
   - Loss Carryforward — $0 (no prior year losses)
   - Harvestable Losses — depends on market data availability
   - Tax Alpha — depends on harvest scan
3. **Verify YTD P&L by Symbol table:**
   - Should show AAPL, MSFT, TSLA rows with ST P&L, LT P&L, Total columns
4. **Verify disclaimer banner** is visible ("This is an estimator, not tax advice")
5. **Verify year selector** — change to 2024, confirm data changes (MSFT buy was in 2024, but sell was in 2025)

### Test 2: Tax Lot Viewer

1. Click **Lots** tab
2. **Verify table renders** with columns: Ticker, Qty, Cost Basis, Proceeds, Gain/Loss, Type (ST/LT), Method
3. **Expected rows:**
   - AAPL: 100 shares, cost $17,050, proceeds $19,500, +$2,450, ST
   - MSFT: 75 shares, cost $28,500, proceeds $32,625, +$4,125, LT
   - TSLA (closed): 50 shares, cost $12,500, proceeds $10,500, −$2,000, ST
   - NVDA (open): 40 shares, cost $34,000, no proceeds, open
   - TSLA (open): 30 shares, cost $6,450, no proceeds, open — may show wash sale adjusted basis
4. **Verify Close & Reassign buttons** are present but disabled (Phase 3G placeholder)
5. **Verify filter controls:**
   - Status filter: Open / Closed / All
   - Ticker filter: type "AAPL" and verify it filters
6. **Verify ST/LT classification badge:**
   - Open ST lots should show 🕐 countdown (days to long-term)
   - NVDA opened Mar 1 should show ~270 days remaining

### Test 3: Wash Sale Monitor

1. Click **Wash Sales** tab
2. **Verify** the monitor renders
3. **Expected:** TSLA wash sale chain should appear:
   - Sold TSLA at loss on Apr 20 → Re-bought on May 2 (12 days)
   - Disallowed loss amount: ~$2,000
   - Chain status: active
4. **Click on the TSLA chain** to open the detail panel
5. **Verify detail panel** shows the individual trades in the chain with dates, prices, and wash amounts

> [!NOTE]
> If wash sales show empty, the wash sale detection engine may require lot materialization AND a separate wash sale scan. Try running `zorivest_tax(action:"wash_sales")` via MCP to verify backend detection.

### Test 4: What-If Simulator

1. Click **Simulator** tab
2. **Test with open position NVDA:**
   - Ticker: `NVDA`
   - Quantity: `20` (partial lot sell)
   - Price: `900`
   - Cost Basis Method: FIFO
3. Click **Run Simulation**
4. **Expected result panel shows:**
   - Total proceeds: $18,000
   - Cost basis: ~$17,000 (20 × $850)
   - Realized P&L: ~+$1,000
   - Classification: ST (held < 1 year from Mar 2025)
   - Estimated tax impact: non-zero
   - Wash sale risk: false (no recent NVDA losses)
5. **Test with TSLA:**
   - Ticker: `TSLA`, Quantity: `30`, Price: `220`
   - **Expected:** Wash sale risk: true (recent TSLA loss)

### Test 5: Loss Harvesting Tool

1. Click **Harvesting** tab
2. **Verify page renders** — may show opportunities or empty state
3. **Expected opportunities** depend on current market prices vs cost basis:
   - NVDA: If current price < $850, shows as harvestable loss
   - TSLA (open lot): If current price < $215, shows as harvestable
   - Wash sale risk flag should be true for TSLA (recent sale within 30 days)
4. **If empty:** This is normal if market data providers aren't configured — harvesting needs live prices to compute unrealized losses
5. **Verify table columns:** Ticker, Unrealized Loss, Current Price, Cost Basis, Qty, Holding Days, Classification, Wash Risk

### Test 6: Quarterly Estimates

1. Click **Quarterly** tab
2. **Verify 4 quarter cards** render (Q1, Q2, Q3, Q4)
3. **Each card should show:**
   - Estimated amount (computed from YTD gains)
   - Paid amount ($0 initially)
   - Due date
   - Status pill (paid/partial/due/overdue/upcoming)
4. **Test payment entry:**
   - Select quarter: Q2
   - Enter amount: `5000`
   - Click Submit
   - **Verify:** Q2 card updates to show $5,000 paid, status changes to "partial" or "paid"
5. **Verify year selector** works (change year, data refreshes)

### Test 7: Transaction Audit

1. Click **Audit** tab
2. Click **Run Audit** button
3. **Verify results panel** appears showing:
   - Total trades audited count
   - Audit timestamp
   - Findings table with severity badges (high/medium/low/info)
4. **Expected findings** may include:
   - Wash sale detection (TSLA)
   - Open positions without stop losses
   - Unmatched lot references

### Test 8: Cross-Feature Data Consistency

1. Navigate to **Trades** section → Verify 9 trades listed
2. Navigate to **Accounts** section → Verify both accounts with correct types/balances
3. Navigate to **Planning** section → Verify 2 trade plans (AAPL Pullback, NVDA Breakout)
4. Navigate back to **Tax** → **Dashboard**
5. **Verify:** Data is consistent — dashboard totals should match lot viewer sums

### Test 9: Account Filtering

1. On Tax pages that show account filters, verify dropdown includes:
   - "All accounts" (or default)
   - "Tax Audit Broker" (taxable)
   - "Tax Audit IRA" (tax-advantaged)
2. Select IRA → Verify taxable-only features exclude or note the tax-advantaged status
3. Select Broker → Verify only broker account lots/trades shown

---

## Expected Feature-Data Matrix

| Tab | API Endpoint | Trigger | Depends On | Data Available After Sync? |
|-----|-------------|---------|------------|---------------------------|
| **Dashboard** | `GET /tax/ytd-summary` | Auto on mount | Materialized lots | ✅ Yes |
| **Lots** | `GET /tax/lots` | Auto on tab click | Materialized lots | ✅ Yes |
| **Wash Sales** | `POST /tax/wash-sales` | Auto on tab click | Materialized lots + wash scan | ⚠️ Maybe (depends on wash scan engine) |
| **Simulator** | `POST /tax/simulate` | Manual submit | Open lots for ticker | ✅ Yes (for NVDA, TSLA) |
| **Harvesting** | `GET /tax/harvest` | Auto on tab click | Open lots + market prices | ⚠️ Maybe (needs market data provider) |
| **Quarterly** | `GET /tax/quarterly` | Auto on tab click | YTD gains estimate | ✅ Yes |
| **Audit** | `POST /tax/audit` | Manual "Run" click | Trades exist | ✅ Yes |

---

## Known Limitations

1. **Loss Harvesting requires market data** — The harvesting engine compares open lot cost basis against current market prices. If no market data provider is configured (FMP, Polygon, etc.), the opportunities list will be empty. This is correct behavior, not a bug.

2. **Wash sale detection accuracy** — The TSLA wash sale chain (sell Apr 20 → re-buy May 2) should be detected, but detection depends on the wash sale engine running against materialized lots. If the Wash Sales tab is empty, verify via MCP: `zorivest_tax(action:"wash_sales")`.

3. **Quarterly estimates are approximate** — The quarterly tax estimator uses a simplified LTCG model (research-backed simplified estimator, not a full stacked-worksheet calculation). Values are advisory only.

4. **Close & Reassign buttons are disabled** — These are Phase 3G features (not yet implemented). The buttons should render with a tooltip explaining they're coming soon.

5. **IRA exclusion behavior** — The IRA account (VTI position) should be excluded from taxable gain/loss calculations. If it appears in taxable views, that's a filtering bug.
