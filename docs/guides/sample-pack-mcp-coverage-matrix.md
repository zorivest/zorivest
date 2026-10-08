# Sample Pack — MCP Coverage Matrix

> **Scope:** which sample-pack entity classes are entered through MCP during agentic authoring
> (Phase 18B, milestone-tracking), and which are out of MCP scope by design.
>
> **This document is derived, not maintained.** The table below records exactly what
> `mcp-server/tests/integration/sample-mcp-coverage-matrix.test.ts` derives from the shipped tool
> inventory `mcp-server/zorivest-tools.json`. When the inventory changes, the test decides and
> this table follows. Editing a row here without the test agreeing fails the suite.
>
> **Sources:** gap analysis §2.2 (the write path per entity), narrowed by §7.1 (human directions
> D8a–D8g, 2026-08-21, and its "Net effect on §2.2"). Plan:
> an internal record (AC-572.1).

## Coverage matrix

| Entity class | Track | Tool | Actions | Basis |
|---|---|---|---|---|
| account | MCP-exercised | `zorivest_account` | `create`, `balance` | §7.1 Net effect |
| trade | MCP-exercised | `zorivest_trade` | `create`, `delete` | §7.1 Net effect |
| plan | MCP-exercised | `zorivest_plan` | `create`, `update` | §7.1 Net effect |
| report | MCP-exercised | `zorivest_report` | `create`, `track_mistake` | §7.1 Net effect |
| watchlist | MCP-exercised | `zorivest_watchlist` | `create`, `add_ticker` | §7.1 Net effect |
| analytics | MCP-exercised | `zorivest_analytics` | `record_trial` | §7.1 Net effect |
| tax | MCP-exercised | `zorivest_tax` | `create_profile` | §7.1 Net effect |
| template | MCP-exercised | `zorivest_template` | `create`, `preview` | §7.1 Net effect |
| policy | MCP-exercised | `zorivest_policy` | `create` | §7.1 Net effect |
| news | Excluded by design | — | — | D8a |
| ai_review_persistence | Excluded by design | — | — | D8c |
| stored_reports | Excluded by design | — | — | D8d |
| broker_records | Excluded by design | — | — | D8e |

How to read the rows:

- **account** covers the 5 accounts (`create`) and the 32 balance snapshots (`balance`).
- **trade** covers execution rows. `delete` is listed because D8g makes delete-and-recreate the
  only way to correct an execution; there is no `update` action.
- **plan** covers all four plan outcomes, `expired` included, now that `MCP-PLAN-EXPIRED-ENUM` is
  fixed (D8f).
- **report** covers trade-bound human reviews (`create`) and mistakes (`track_mistake`).
- **watchlist** is used as-is, as a scratch pad (D8b). Items carry a ticker and notes, with no
  link to plans or trades.
- **analytics** covers the strategy trials (`record_trial`).
- **news (D8a):** catalyst evidence comes from historical web search and is recorded as trade
  reasoning. `zorivest_market news` fetches current news and is not used to author the pack.
- **ai_review_persistence (D8c):** the 8 frozen AI reviews are manifest-only. No MCP action
  writes fabricated AI output into the review store.
- **stored_reports (D8d):** report policies and templates ship in Draft through the compiler.
  Activating them is a documented user step.
- **broker_records (D8e):** no broker record ships in the pack. Providers are yfinance and
  TradingView only, and they need no credentials.

## Track invariant

Every entity class is in **exactly one** track. The two tracks are fixed by §7.1, the
Human-approved direction; they are never set by what the inventory can currently do:

1. **No entity moves between tracks to make coverage look complete.** If a tool or action behind
   an `MCP-exercised` entity disappears from the inventory, the derivation test names that
   entity as *missing*. The fix is to restore the surface or bring a new human direction. Moving
   the row to `Excluded by design` is not a fix, and the test refuses it: any track change
   against the derivation is reported as a problem.
2. **An excluded entity cites a D8 direction** (D8a, D8c, D8d, D8e), never a surface gap. An
   excluded entity that gains an MCP surface stays excluded until §7.1 is amended.
3. **The inventory fails closed.** An unknown toolset or a malformed tool name makes the
   derivation throw. It never produces a narrower matrix.

## Defect log

Defects found in existing actions while establishing coverage. The three §2.3 "pre-log these"
defects were resolved before this initiative; SP-6 row 1 re-verified them
(`$env:TEMP/zorivest/sp6-preflight.json`: `"defects": "resolved"`), and the current code confirms
each fix.

| Defect | Surface | Status | Evidence at HEAD |
|---|---|---|---|
| `MCP-PLAN-EXPIRED-ENUM` | `zorivest_plan` status enum | resolved 2026-08-23 (milestone-tracking) | `mcp-server/src/compound/plan-tool.ts` — `PLAN_STATUS_SET` includes `expired` and backs the `status` Zod enum |
| `API-MCP-TOOLSETS-FROZEN-EMPTY` | `/api/v1/mcp/toolsets` in packaged builds | resolved 2026-08-23 | `packages/api/src/zorivest_api/routes/mcp_toolsets.py` — the frozen build reads the manifest from `sys._MEIPASS` |
| `MCP-EXECUTION-QUALITY-PARAM-MISMATCH` | `zorivest_analytics` `execution_quality` | resolved (milestone-tracking) | `mcp-server/src/compound/analytics-tool.ts` — sends `trade_exec_id` via `buildQs` |

Defects found in exercised tools while writing the milestone-tracking runbook (SP-6 row 8). Both are
low severity and not vital to continuation, so they are logged under the runbook's
known-issues protocol (`discovered_during: sample-seed-dataset`) and not fixed here: SP-6 does
not edit MCP tool files.

| Defect | Surface | Status | Evidence at HEAD |
|---|---|---|---|
| `MCP-TRADE-CREATE-TOKEN-DESC-STALE` | `zorivest_trade` tool description | open (logged 2026-09-21) | `mcp-server/src/compound/trade-tool.ts` — the tool description says `create` needs a `confirmation_token`; the `create` handler and its summary need none (`$env:TEMP/zorivest/sp6-repro-trade-desc.txt`) |
| `MCP-READBACK-GAP-BALANCES-MISTAKES` | `zorivest_account`, `zorivest_report` read actions | open (logged 2026-09-21) | no MCP action lists balance history or a trade's mistakes; the runbook's U4 and U6 read back through REST `GET` (`$env:TEMP/zorivest/sp6-repro-readback.txt`) |

Defects that turn up in the milestone-tracking dual run (dev stdio and packaged) go in the known-issues
SSOT under the runbook's known-issues protocol, and are added to this log.
