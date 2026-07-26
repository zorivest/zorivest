# How it Works — Help Content Outlines (milestone-tracking)

Draft outlines for five feature areas. **No HelpCard UI in this milestone** — implementation is milestone-tracking after human content approval.

Field shape mirrors Local Canon `SchedulingHelpContent` (`scheduling-help-content.ts`) plus `tutorialUrl` for DOC-HOWITWORKS-VIDEO.

---

## Accounts

- what: Portfolio accounts you track in Zorivest — cash and brokerage identities used when importing trades, computing balances, and attributing P&L. Zorivest plans and evaluates; it never places broker orders.
- source: Account records live in your active local database. Broker-linked accounts may refresh holdings via configured adapters (IBKR/TWS managed accounts or REST brokers); manual accounts accept balance updates you enter in the UI or via MCP/API from your IDE.
- calculation: Portfolio % and balance views sum account equity using stored balances and linked positions. Currency display follows account settings; multi-currency conversion is deferred where not yet configured.
- learnMoreUrl: https://github.com/zorivest/zorivest/blob/main/AGENTS.md
- learnMoreLabel: Project contributor guide (accounts context)
- tutorialUrl: https://github.com/zorivest/zorivest#readme

---

## Trade Plans

- what: Planned ticker execution you design to reflect on before pulling the trigger. Here are planning artifacts, not live broker orders.
- source: Plans are stored in the active Zorivest database via the Plans API. You create and edit them in Planning; status transitions (draft, active, executed, expired, cancelled) are explicit user actions.
- calculation: Plan metrics (debit/credit, max loss/gain sketches), quantities, and premiums you enter. They do not pull live fills unless you import matching trades separately.
- learnMoreUrl: https://github.com/zorivest/zorivest/blob/main/docs/BUILD_PLAN.md
- learnMoreLabel: Build plan (planning surfaces)
- tutorialUrl: https://github.com/zorivest/zorivest#readme

---

## Watchlists

- what: Named ticker lists for symbols you want to monitor — research and planning scaffolding, not automatic trading signals.
- source: Watchlists and items persist in the active database (/api/v1 watchlists routes). Notes on items are user-authored and stored locally.
- calculation: No derived scoring in the base watchlist view — rows reflect the tickers and notes you added. Market quotes appear only when a market-data provider is configured and fetching.
- learnMoreUrl: https://github.com/zorivest/zorivest/blob/main/docs/reference/help-search.md
- learnMoreLabel: Help search reference
- tutorialUrl: https://github.com/zorivest/zorivest#readme

---

## Report Policies

- what: Automated pipeline schedules — named policies that define what data to fetch, how to process it, and when to run. Policies with AI steps require schema_version 2.
- source: Policies are stored locally in your Zorivest database. The scheduler runs approved policies at configured times. AI steps use Settings → AI Providers (BYOK or local LLMs).
- calculation: No portfolio math — policies define workflow steps (fetch, transform, report, email, web_search, ai_generate). Next-run times come from cron evaluation in your configured timezone; AI steps respect provider rate limits and credit budgets.
- learnMoreUrl: https://github.com/zorivest/zorivest/blob/main/docs/reference/ai-search-steps.md
- learnMoreLabel: AI Search & Generate Steps Reference
- tutorialUrl: https://github.com/zorivest/zorivest#readme

---

## Email Templates

- what: Reusable email templates for pipeline report delivery (variables like report_name / generated_at), including AI research digest layouts with numbered sources.
- source: Templates live in your Zorivest database. Built-ins cover common report types; you can duplicate, customize, or create new templates.
- calculation: Templates render server-side with Jinja2 when a pipeline email step runs. Preview uses sample data so you can verify layout before deploy — no P&L calculation.
- learnMoreUrl: https://github.com/zorivest/zorivest/blob/main/docs/reference/ai-search-steps.md
- learnMoreLabel: AI Search & Generate Steps Reference
- tutorialUrl: https://github.com/zorivest/zorivest#readme

---

## Tutorial link policy (DOC-HOWITWORKS-VIDEO)

Resolved policy for milestone-tracking implementation:

1. Open tutorial URLs only via `shell.openExternal` / `window.electron.openExternal`.
2. **Do not** embed third-party video players inside the app chrome (external browser only).
3. **Do not** widen CSP for third-party framed media hosts.
4. Canonical tutorial URLs belong in co-located `*-help-content.ts` files at implementation time (same pattern as `learnMoreUrl` today).
5. Outlines above use placeholder README tutorial targets until product picks final video URLs at milestone-tracking.

---

## Human Approval Checklist (gates milestone-tracking)

Before IT4-S4 / milestone-tracking HelpCard implementation:

- [X] Accounts prose accurate (no invented broker-order behavior)
- [X] Trade Plans prose accurate (planning-only; not execution)
- [X] Watchlists prose accurate (no fake scoring claims)
- [X] Report Policies prose aligned with Scheduling help card + AI steps docs
- [X] Email Templates prose aligned with Jinja2 / digest template reality
- [X] Tutorial URLs approved (or confirmed as temporary README placeholders)
- [X] learnMoreUrl / learnMoreLabel pairs approved per area
- [X] Explicit human approval recorded to start milestone-tracking

**Handoff status:** human content approval recorded 2026-07-24 (checklist 8/8). milestone-tracking unblocked.
Public URL targets depend on milestone-tracking (`§7A.12g`) so `zorivest/zorivest` paths resolve after sync.
