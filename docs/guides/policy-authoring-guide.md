# Zorivest Pipeline Policy Authoring Guide

> **Purpose:** Complete reference for writing pipeline policies. Documents every step type, every parameter, what's dynamic (configurable via policy JSON) vs what's hardcoded in the codebase.

## Table of Contents

- [Policy Structure](#policy-structure)
  - [Canonical Example](#canonical-example)
- [Step 1: Fetch](#step-1-fetch)
- [Step 2: Transform](#step-2-transform)
  - [Write Rules](#write-rules-validated-before-any-run)
  - [Dry Run](#dry-run)
- [Step 3: Store Report](#step-3-store-report)
- [Step 4: Render](#step-4-render)
- [Step 5: Send](#step-5-send)
- [Dataflow Wiring](#dataflow-wiring)
- [Available Templates](#available-templates)
- [Available Providers](#available-providers)
- [Available Validation Schemas](#available-validation-schemas)
- [Available Field Mappings](#available-field-mappings)
- [Hardcoded vs Dynamic Summary](#hardcoded-vs-dynamic-summary)
- [Architectural Gaps](#architectural-gaps)

---

## Policy Structure

A policy is a JSON document stored in the `policies` table. The pipeline runner reads `steps` in order and executes them sequentially.

```json
{
  "name": "My Daily Quote Report",
  "description": "Fetch AAPL/MSFT quotes → transform → email",
  "schedule": "0 9 * * 1-5",
  "timezone": "America/New_York",
  "steps": [
    { "id": "fetch_quotes",     "type": "fetch",        "params": { ... } },
    { "id": "transform_quotes", "type": "transform",    "params": { ... } },
    { "id": "store_snapshot",   "type": "store_report",  "params": { ... } },
    { "id": "render_html",      "type": "render",       "params": { ... } },
    { "id": "send_email",       "type": "send",         "params": { ... } }
  ]
}
```

### Top-Level Fields

| Field | Type | Dynamic | Description |
|-------|------|---------|-------------|
| `name` | string | ✅ | Human-readable policy name |
| `description` | string | ✅ | Free-text description |
| `schedule` | string | ✅ | Cron expression (5-field) |
| `timezone` | string | ✅ | IANA timezone for schedule evaluation |
| `steps` | array | ✅ | Ordered list of step definitions |
| `enabled` | boolean | ✅ | Whether the scheduler should execute this policy |

### Step Definition Fields

| Field | Type | Dynamic | Description |
|-------|------|---------|-------------|
| `id` | string | ✅ | Unique step identifier (referenced by other steps) |
| `type` | string | ✅ | Step type: `fetch`, `transform`, `store_report`, `render`, `send` |
| `params` | object | ✅ | Step-specific parameters (see below) |
| `skip_if` | string | ✅ | Condition expression — skip step when truthy |
| `error_mode` | string | ✅ | `"fail"` (default), `"continue"`, `"retry"` |

> ⚠️ **`PolicyStep` is a closed object.** The published schema sets
> `"additionalProperties": false` on `PolicyStep` (milestone-tracking, Boundary Input Contract item 4),
> so a step may only carry the fields in the table above (`id`, `type`, `params`, `skip_if`,
> `error_mode`, plus `on_error`, `required`, `retry`, `timeout`). An unknown step key such as
> `config` is refused with a 422 naming the exact path — `steps.<i>.<key>` — instead of being
> dropped silently and leaving `params` to default to `{}`. This is enforced at create, update,
> emulate, and run.

### Canonical Example

`tests/fixtures/policy_contract/example-policy.json`
is the executable reference for this guide — every field it uses is a real, accepted field, and
it is exercised directly by the contract test suite. When this guide and the fixture ever
disagree, the fixture wins.

```json
{
  "name": "Test FAANG Yahoo Daily Notes",
  "schema_version": 1,
  "trigger": {
    "cron_expression": "55 11 * * *",
    "enabled": true,
    "timezone": "America/New_York"
  },
  "steps": [
    {
      "id": "fetch_yahoo_faang",
      "type": "fetch",
      "params": {
        "provider": "yahoo",
        "data_type": "quote",
        "criteria": { "tickers": ["META", "AAPL", "AMZN", "NFLX", "GOOGL"] },
        "use_cache": false
      }
    },
    {
      "id": "store_quotes",
      "type": "transform",
      "params": {
        "target_table": "market_quotes",
        "mapping": "auto",
        "write_disposition": "append",
        "source_step_id": "fetch_yahoo_faang"
      }
    }
  ]
}
```

Run `tools/export_policy_schema.py --check tests/fixtures/policy_contract/policy-document.schema.json`
to prove the committed schema snapshot still matches what `PolicyDocument.model_json_schema()`
publishes — a mismatch means this guide's schema claims (closed keys, `additionalProperties`)
are stale.

---

## Step 1: Fetch

**Type name:** `fetch`
**Side effects:** No (read-only HTTP calls)
**Source:** `fetch_step.py`

Retrieves market data from an external provider via HTTP. Supports caching, criteria resolution, and multi-ticker iteration.

### Parameters

| Parameter | Type | Default | Dynamic | Description |
|-----------|------|---------|---------|-------------|
| `provider` | string | *required* | ✅ | Either the provider slug (e.g. `yahoo`) or the display name (e.g. `Yahoo Finance`) is accepted — both spellings work at policy create and at run time (see [Available Providers](#available-providers)) |
| `data_type` | string | *required* | ✅ | Data type: `ohlcv`, `quote`, `news`, `fundamentals` |
| `criteria` | object | `{}` | ✅ | Criteria for data selection (see [Criteria Types](#criteria-types)) |
| `batch_size` | integer | `100` | ✅ | Max records per fetch batch (1–500) |
| `use_cache` | boolean | `true` | ✅ | Check cache before fetching |

### Criteria Types

Each key in the `criteria` object can use different resolution strategies:

> ⚠️ **The `criteria` key set is closed.** `FetchStep.Criteria` sets
> `model_config = {"extra": "forbid"}` (milestone-tracking AC-11), so a key outside the table below is a
> **422 at policy save**, not a silently ignored field. Only these keys are accepted:
> `tickers`, `symbol`, `date_range`, `exchange`, `outputsize`, `vendor`, `table`, `id_type`,
> `form_type` — each one a key some URL builder actually reads. The closure is deliberate: a
> key no builder reads produces an empty result set on the next scheduled run, so a typo such
> as `tikcers` is far cheaper to catch at the write boundary than at 3 a.m. The published
> `/scheduling/step-types` schema (milestone-tracking) shows `criteria` as this same closed object —
> `"additionalProperties": false` with exactly the nine keys above — because `Params.criteria`
> is typed as `FetchStep.Criteria` itself, so the schema an agent reads and the model that
> refuses its input are one definition.

#### Static values (no `type` key — passed through unchanged)

```json
{
  "criteria": {
    "tickers": ["AAPL", "MSFT", "GOOGL"],
    "exchange": "US"
  }
}
```

> **Do not pass `interval`.** Earlier revisions of this guide showed `"interval": "1d"` here.
> It was never read from `criteria` — the Yahoo builder hardcodes
> `params = {"interval": "1d"}` (`url_builders.py:178`) — and since AC-11 closed the schema it
> is rejected outright. The example above is corrected to a key the builders genuinely consume.

#### Relative dates (`type: "relative"`)

Resolves to `{start_date, end_date}` at execution time.

```json
{
  "criteria": {
    "tickers": ["AAPL"],
    "date_range": {
      "type": "relative",
      "expr": "-30d"
    }
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `expr` | string | Relative expression: `-Nd` where N = days back |

> **Hardcoded:** Only `d` (days) unit is supported. No hours, weeks, months.

#### Incremental / High-Water Mark (`type: "incremental"`)

Reads last cursor from `pipeline_state` table. Falls back to 30 days if no prior state.

```json
{
  "criteria": {
    "date_range": {
      "type": "incremental",
      "policy_id": "my-policy",
      "provider_id": "yahoo",
      "data_type": "ohlcv"
    }
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `policy_id` | string | Policy identifier for state lookup |
| `provider_id` | string | Provider slug for state lookup |
| `data_type` | string | Data type for state lookup |
| `entity_key` | string | Optional sub-entity key |

#### DB Query (`type: "db_query"`)

Executes read-only SQL to resolve date range. Must return `(start_date, end_date)` as first row.

```json
{
  "criteria": {
    "date_range": {
      "type": "db_query",
      "sql": "SELECT MIN(trade_date), MAX(trade_date) FROM trades WHERE account_id = 1"
    }
  }
}
```

> ⚠️ **This only resolves criteria dates.** It does NOT fetch records for the pipeline. See [Architectural Gaps](#architectural-gaps).

### Output Shape

FetchStep writes to `context.outputs[step_id]`:

```json
{
  "content": "<raw bytes or pre-merged JSON array>",
  "cache_status": "miss",
  "provider": "Yahoo Finance",
  "data_type": "quote",
  "resolved_criteria": { ... }
}
```

### What's Hardcoded

| Item | Status | Details |
|------|--------|---------|
| Provider registry | 🔒 Hardcoded | 13 providers (11 API-key + 2 free) defined in `provider_registry.py`. Adding a new provider requires code change. |
| URL patterns | 🔒 Hardcoded | Per-provider URL builders in `url_builders.py`. Endpoint paths are not configurable. |
| Yahoo multi-ticker | 🔒 Hardcoded | Yahoo v8/chart API only supports 1 ticker per request. Adapter iterates internally. |
| Cache TTL | 🔒 Hardcoded | Base TTL comes from cache repo. Market-closed multiplier is 4× (weekends/holidays). |
| Market-closed logic | 🔒 Hardcoded | Checks US market hours (NYSE: 9:30–16:00 ET, weekdays). No per-provider schedule. |
| Rate limiter | 🔒 Hardcoded | `default_rate_limit` per provider from registry. Not tunable per-policy. |

---

## Step 2: Transform

**Type name:** `transform`
**Side effects:** Yes (writes to DB via `db_writer`)
**Source:** `transform_step.py`

Extracts records from raw fetch output, maps fields to canonical schema, validates via Pandera, writes to DB, and applies presentation mapping for downstream templates.

### Parameters

| Parameter | Type | Default | Dynamic | Description |
|-----------|------|---------|---------|-------------|
| `target_table` | string | *required* | ✅ | DB table name for validated records |
| `mapping` | string | `"auto"` | ✅ | Field mapping strategy (currently only `"auto"` is used) |
| `write_disposition` | string | `"append"` | ✅ | Write mode: `append`, `replace`, `merge` |
| `validation_rules` | string | `"ohlcv"` | ✅ | Pandera schema name (see [Available Schemas](#available-validation-schemas)) |
| `quality_threshold` | float | `0.8` | ✅ | Minimum valid/total ratio (0.0–1.0). Below → step FAILS. |
| `source_step_id` | string | `null` | ✅ | Explicit step ID to read source data from |
| `output_key` | string | `"records"` | ✅ | Key name for validated records in step output |
| `min_records` | integer | `0` | ✅ | Minimum expected records. 0 actual + min_records > 0 → WARNING status |

### Source Resolution (3-tier fallback)

When `source_step_id` is not set, TransformStep auto-discovers its input:

1. **Explicit:** `source_step_id` → `context.outputs[source_step_id]`
2. **Auto-discover:** Scans `context.outputs` for any dict with both `content` and `provider` keys (FetchStep output signature)
3. **Legacy fallback:** `context.outputs["fetch_result"]`

### Output Shape

```json
{
  "target_table": "market_quotes",
  "write_disposition": "append",
  "records_written": 7,
  "records_quarantined": 0,
  "quality_ratio": 1.0,
  "<output_key>": [
    { "symbol": "AAPL", "price": 198.50, "change": 2.30, "change_pct": 1.17, "volume": 54321000 },
    ...
  ]
}
```

### Write Rules (validated before any run)

Rule 15 (milestone-tracking `WRITE_RULES`) checks a transform's write against the same contracts the live
write itself enforces — it fires at **create, update, emulate, and run**, so a policy that only
looks wrong at the first live run instead fails at the boundary where it is cheapest to fix. The
transform has no `data_type` of its own; the rule infers it from the `fetch` step named by
`source_step_id`.

(a) **`WRITE_TARGET_MISMATCH`** — `target_table` must equal the contract table for the source
fetch's `data_type`:

| `data_type` | `target_table` |
|-------------|-----------------|
| `ohlcv` | `market_ohlcv` |
| `earnings` | `market_earnings` |
| `dividends` | `market_dividends` |
| `splits` | `market_splits` |
| `insider` | `market_insider` |
| `fundamentals` | `market_fundamentals` |
| `quote` | `market_quotes` |
| `news` | `market_news` |

(b) **`WRITE_MERGE_NO_NATURAL_KEY`** — `write_disposition: merge` needs a target table with a
declared natural key. Only these tables qualify:

| Table | Natural key columns |
|-------|---------------------|
| `market_ohlcv` | `ticker`, `timestamp`, `provider` |
| `market_earnings` | `ticker`, `fiscal_period`, `fiscal_year` |
| `market_dividends` | `ticker`, `ex_date` |
| `market_splits` | `ticker`, `execution_date` |
| `market_insider` | `ticker`, `name`, `transaction_date`, `transaction_code` |
| `market_fundamentals` | `ticker`, `metric`, `period`, `provider` |

`market_quotes` has **no** natural key — use `write_disposition: append` for it, not `merge`.

(c) **`WRITE_UNSUPPORTED_DATA_TYPE`** — a `data_type` with no entry in the `data_type →
target_table` table above (i.e. no direct-write contract) cannot feed any `target_table` at all.

### Dry Run

A dry run skips write steps; nothing was written. A green dry run only proves the read path
(criteria resolution, cache lookups, provider fetch) succeeded — it does not prove a live write
will succeed, since Rule 15's write-contract checks and the writer's own constraints still apply
the first time a real `write_disposition` actually runs.

### What's Hardcoded

| Item | Status | Details |
|------|--------|---------|
| Presentation mapping | 🔒 Hardcoded | `ticker → symbol`, `last → price`. Cannot add custom renames via policy. |
| Field mapping registry | 🔒 Hardcoded | `FIELD_MAPPINGS` in `field_mappings.py`. Per-provider canonical mappings. |
| Provider slug map | 🔒 Hardcoded | `_PROVIDER_SLUG_MAP` in `field_mappings.py`. Display name → slug normalization. |
| Response extractors | 🔒 Hardcoded | `response_extractors.py` — Yahoo envelope unwrapping is provider-specific code. |
| Validation schemas | 🔒 Hardcoded | `SCHEMA_REGISTRY` in `validation_gate.py`. Adding a schema requires code. |
| `_extra` column drop | 🔒 Hardcoded | Unmapped fields are captured in `_extra` dict but dropped before DB write. |
| Enrichment fields | 🔒 Hardcoded | `provider` and `timestamp` are auto-injected if missing. |

---

## Step 3: Store Report

**Type name:** `store_report`
**Side effects:** Yes (writes to `reports` table)
**Source:** `store_report_step.py`

Snapshots data via SQL queries, computes a SHA-256 content hash, and persists to the `reports` table for dedup and audit.

### Parameters

| Parameter | Type | Default | Dynamic | Description |
|-----------|------|---------|---------|-------------|
| `report_name` | string | *required* | ✅ | Name for the report record |
| `spec` | object | `{}` | ✅ | Report specification (sections, layout metadata) |
| `data_queries` | array | `[]` | ✅ | Named SQL queries for snapshot data |

### `data_queries` Format

Each entry runs a read-only SQL query against the sandboxed DB connection:

```json
{
  "data_queries": [
    {
      "name": "recent_trades",
      "sql": "SELECT * FROM trades WHERE trade_date >= date('now', '-7 days') ORDER BY trade_date DESC LIMIT 50"
    },
    {
      "name": "watchlist_summary",
      "sql": "SELECT ticker, target_price, notes FROM watchlist_items WHERE active = 1"
    }
  ]
}
```

> **This is the only step that can currently query internal DB tables.** The results go into `snapshot_json` for report persistence. They do NOT flow into the transform pipeline or template rendering via the standard dataflow.

### Output Shape

```json
{
  "report_name": "daily_quote_report",
  "report_id": "uuid-here",
  "snapshot_hash": "sha256-hex",
  "snapshot_json": "{ ... }",
  "spec_json": "{ ... }",
  "query_count": 2
}
```

### What's Hardcoded

| Item | Status | Details |
|------|--------|---------|
| Hash algorithm | 🔒 Hardcoded | SHA-256 for snapshot hash. |
| Serialization | 🔒 Hardcoded | `json.dumps(sort_keys=True, separators=(",",":"))` — deterministic. |
| Report table | 🔒 Hardcoded | Always writes to `reports` table via `report_repository`. |

### Context Dependencies

Requires in `context.outputs`:
- `db_connection` — only if `data_queries` is non-empty
- `report_repository` — always required

---

## Step 4: Render

**Type name:** `render`
**Side effects:** No
**Source:** `render_step.py`

Renders report data to HTML or Markdown using Jinja2 templates.

### Parameters

| Parameter | Type | Default | Dynamic | Description |
|-----------|------|---------|---------|-------------|
| `template` | string | *required* | ✅ | Jinja2 template name |
| `output_format` | string | `"html"` | ✅ | Output: `html` or `markdown` only (PDF removed per §9H) |
| `chart_settings` | object | `{}` | ✅ | Chart rendering config (reserved for future use) |

### Output Shape

```json
{
  "html": "<!DOCTYPE html>...",
  "template": "daily_quote_summary",
  "output_format": "html"
}
```

For `output_format: "markdown"`:

```json
{
  "markdown": "# Report Title\n| col1 | col2 |\n...",
  "template": "daily_quote_summary",
  "output_format": "markdown"
}
```

### What's Hardcoded

| Item | Status | Details |
|------|--------|---------|
| Template engine | 🔒 Hardcoded | Looks for `template_engine` in context. Falls back to inline Jinja2 `from_string()`. |
| Markdown renderer | 🔒 Hardcoded | `_render_markdown()` generates Markdown tables from records/quotes data. |
| Data source | 🔒 Hardcoded | Reads from `context.outputs["report_data"]`. Only populated by `StoreReportStep`. |

> ⚠️ **RenderStep reads from `report_data` in context**, which is set by StoreReportStep. If you skip StoreReportStep, RenderStep has no data. For the email-only workflow (Fetch → Transform → Send), RenderStep is typically skipped — SendStep handles template rendering internally.

---

## Step 5: Send

**Type name:** `send`
**Side effects:** Yes (sends emails, writes files)
**Source:** `send_step.py`

Delivers reports via email (SMTP) or local file copy.

### Parameters

| Parameter | Type | Default | Dynamic | Description |
|-----------|------|---------|---------|-------------|
| `channel` | string | *required* | ✅ | Delivery channel: `email` or `local_file` |
| `recipients` | array | *required* | ✅ | List of email addresses or file paths (max 5) |
| `subject` | string | `""` | ✅ | Email subject line |
| `body_template` | string | `""` | ✅ | Template name from `EMAIL_TEMPLATES` registry |
| `report_id` | string | `null` | ✅ | Report ID from store_report step (for dedup) |
| `snapshot_hash` | string | `null` | ✅ | Snapshot hash from store_report step (for dedup) |
| `html_body` | string | `null` | ✅ | Pre-rendered HTML body (overrides template) |
| `attachment_path` | string | `null` | ✅ | Optional path to a `.md` file to attach (§9H.2). Only `.md` files accepted; `.pdf` rejected with error. |

### Body Resolution (4-tier priority)

SendStep resolves the email body using this priority chain:

1. **`html_body`** — Explicit HTML override (e.g., from RenderStep)
2. **`body_template` → EMAIL_TEMPLATES** — Looks up template name, renders via Jinja2 with pipeline context
3. **`body_template` raw string** — If name not found in registry, uses the string as-is
4. **Default** — `<p>Report attached</p>` when nothing provided

### Template Context Variables

When using Tier 2 (template rendering), these variables are available in Jinja2:

| Variable | Source | Always Available |
|----------|--------|-----------------|
| `generated_at` | Auto-generated ISO timestamp | ✅ |
| `policy_id` | `StepContext.policy_id` | ✅ |
| `run_id` | `StepContext.run_id` | ✅ |
| *All step outputs* | Merged from `context.outputs` | Depends on prior steps |

**Two-level merge:** For dict-valued outputs from prior steps, inner keys are promoted to the top-level template context. For example, if TransformStep outputs `{"quotes": [...], "records_written": 7}` under key `transform_quotes`, then both `quotes` and `records_written` become available as template variables.

### `local_file` Channel

When `channel: "local_file"`, `recipients` contains destination file paths. The rendered HTML body is written as a `.md` file to each path:

```json
{
  "channel": "local_file",
  "recipients": ["C:/reports/daily_quote.md"],
  "html_body": "<h1>Daily Report</h1><p>Content here</p>"
}
```

### What's Hardcoded

| Item | Status | Details |
|------|--------|---------|
| Template registry | 🔒 Hardcoded | `EMAIL_TEMPLATES` in `email_templates.py`. Adding a template requires code. |
| SMTP config source | 🔒 Hardcoded | Read from `context.outputs["smtp_config"]`. Set at backend startup from env vars. |
| Dedup mechanism | 🔒 Hardcoded | SHA-256 of `(report_id, channel, recipient, snapshot_hash)`. |
| Max recipients | 🔒 Hardcoded | 5 per step (Pydantic `max_length=5`). |
| Financial filters | 🔒 Hardcoded | `currency` Jinja2 filter from `create_template_engine()`. |
| Dedup fallback | 🔒 Hardcoded | When `snapshot_hash` is absent, falls back to `run_id` for dedup key. |

---

## Dataflow Wiring

### Common Pipeline Patterns

#### Pattern A: Fetch → Transform → Send (email with inline body)

This is the most common pattern. No RenderStep or StoreReportStep needed.

```json
{
  "steps": [
    {
      "id": "fetch_quotes",
      "type": "fetch",
      "params": {
        "provider": "Yahoo Finance",
        "data_type": "quote",
        "criteria": {
          "tickers": ["AAPL", "MSFT", "GOOGL"]
        }
      }
    },
    {
      "id": "transform_quotes",
      "type": "transform",
      "params": {
        "target_table": "market_quotes",
        "validation_rules": "quote",
        "output_key": "quotes",
        "min_records": 1
      }
    },
    {
      "id": "send_email",
      "type": "send",
      "params": {
        "channel": "email",
        "recipients": ["you@example.com"],
        "subject": "Daily Quote Report",
        "body_template": "daily_quote_summary"
      }
    }
  ]
}
```

**Key wiring:**
- TransformStep auto-discovers FetchStep output (no `source_step_id` needed)
- `output_key: "quotes"` matches the template variable `{% for q in quotes %}`
- SendStep's two-level merge promotes `quotes` from transform output into template context

#### Pattern B: Fetch → Transform → Store → Render → Send (full pipeline with HTML)

```json
{
  "steps": [
    {
      "id": "fetch_data",
      "type": "fetch",
      "params": {
        "provider": "Yahoo Finance",
        "data_type": "ohlcv",
        "criteria": {
          "tickers": ["AAPL"],
          "date_range": { "type": "relative", "expr": "-30d" }
        }
      }
    },
    {
      "id": "transform_data",
      "type": "transform",
      "params": {
        "target_table": "market_ohlcv",
        "validation_rules": "ohlcv",
        "write_disposition": "append"
      }
    },
    {
      "id": "store_snapshot",
      "type": "store_report",
      "params": {
        "report_name": "monthly_performance",
        "data_queries": [
          {
            "name": "price_history",
            "sql": "SELECT * FROM market_ohlcv WHERE ticker = 'AAPL' ORDER BY timestamp DESC LIMIT 30"
          }
        ]
      }
    },
    {
      "id": "render_report",
      "type": "render",
      "params": {
        "template": "generic_report",
        "output_format": "html"
      }
    },
    {
      "id": "send_report",
      "type": "send",
      "params": {
        "channel": "email",
        "recipients": ["you@example.com"],
        "subject": "Monthly Performance Report",
        "body_template": "generic_report",
        "report_id": { "ref": "ctx.store_snapshot.report_id" },
        "snapshot_hash": { "ref": "ctx.store_snapshot.snapshot_hash" }
      }
    }
  ]
}
```

### Ref Resolution

Any step parameter can use `{ "ref": "ctx.<step_id>.<field>" }` to reference output from a prior step:

```json
{
  "report_id": { "ref": "ctx.store_snapshot.report_id" },
  "snapshot_hash": { "ref": "ctx.store_snapshot.snapshot_hash" }
}
```

The ref resolver walks `context.outputs[step_id][field]` at execution time.

---

## Available Templates

Templates are registered in `email_templates.py`.

### `daily_quote_summary`

Professional quote report table. Expects `quotes` as a list of dicts.

**Required template variables:**

| Variable | Type | Description |
|----------|------|-------------|
| `quotes` | list[dict] | List of quote records |
| `quotes[].symbol` | string | Ticker symbol |
| `quotes[].price` | float | Current price |
| `quotes[].change` | float | Price change (absolute) |
| `quotes[].change_pct` | float | Price change (percentage) |
| `quotes[].volume` | int | Trading volume |
| `generated_at` | string | Auto-injected timestamp |

**Wiring requirement:** TransformStep must set `output_key: "quotes"`.

### `generic_report`

Auto-generated table from any record set. Iterates over dict keys dynamically.

**Required template variables:**

| Variable | Type | Description |
|----------|------|-------------|
| `records` | list[dict] | Any list of dicts — columns from keys |
| `title` | string | Optional report title (defaults to "Zorivest Report") |
| `generated_at` | string | Auto-injected timestamp |

**Wiring requirement:** TransformStep `output_key` must be `"records"` (the default).

> 🔒 **Adding new templates requires a code change** to `email_templates.py`. This is tracked in [PIPE-NOLOCALQUERY] and [MCP-TOOLDISCOVERY] for future dynamic template support.

---

## Available Providers

From `provider_registry.py`:

| Provider Name | Slug | Auth Required | Rate Limit | Supported data_types |
|--------------|------|---------------|------------|---------------------|
| Yahoo Finance | `yahoo` | ❌ No | 100/min | `ohlcv`, `quote`, `news` |
| TradingView | `tradingview` | ❌ No | 60/min | (scanner only) |
| Alpha Vantage | `alpha_vantage` | ✅ API key | 5/min | `ohlcv`, `quote` |
| Polygon.io | `polygon` | ✅ Bearer | 5/min | `ohlcv`, `quote`, `news`, `fundamentals` |
| Finnhub | `finnhub` | ✅ Header | 60/min | `ohlcv`, `quote`, `news` |
| Financial Modeling Prep | `fmp` | ✅ API key | 250/min | `ohlcv`, `fundamentals` |
| EODHD | `eodhd` | ✅ API key | 20/min | `ohlcv` |
| Nasdaq Data Link | `nasdaq_dl` | ✅ API key | 50/min | `fundamentals` |
| SEC API | `sec_api` | ✅ Header | 60/min | `fundamentals` |
| API Ninjas | `api_ninjas` | ✅ Header | 60/min | `quote` |
| OpenFIGI | `openfigi` | ✅ Header | 10/min | (identifier mapping) |
| Alpaca | `alpaca` | ✅ Key + Secret | 200/min | `ohlcv`, `quote` |
| Tradier | `tradier` | ✅ Bearer | 120/min | `ohlcv`, `quote` |

> `market_contract.PROVIDER_SLUGS` also carries `Interactive Brokers` (`ibkr`) and
> `SEC EDGAR` (`sec_edgar`), but neither has a `PROVIDER_REGISTRY` entry: IBKR OHLCV arrives
> through the broker sync path, not an HTTP fetch step, so no `fetch` step can name either one.

Slugs are the `PROVIDER_SLUGS` mapping in
`market_contract.py`; a
`fetch` step's `provider` field accepts either the slug or the display name in this table.

> ⚠️ **Only Yahoo Finance has full URL builder + response extractor implementation.** Other providers have registry entries but their URL builders and extractors are generic/incomplete. Adding production support for a new provider requires implementing its `UrlBuilder` and response extractor.

---

## Available Validation Schemas

From `validation_gate.py`:

| Schema Name | Required Fields | Optional Fields | Constraints |
|-------------|----------------|-----------------|------------|
| `ohlcv` | `open`, `high`, `low`, `close`, `volume` | (any extra) | All prices > 0, volume ≥ 0 |
| `quote` | `ticker`, `last`, `timestamp`, `provider` | `bid`, `ask`, `volume` | `last` > 0, `bid`/`ask` > 0 when present |
| `news` | `headline`, `source`, `url`, `published_at` | `sentiment` | headline non-empty, url starts with `http`, sentiment ∈ [-1, 1] |
| `fundamentals` | `ticker`, `metric`, `value`, `period` | (any extra) | period matches `YYYY-(Q1-4\|FY\|H1-2)` |

All schemas use `strict=False` — extra columns are allowed and passed through.

---

## Available Field Mappings

From `field_mappings.py`:

### Provider → Canonical Mappings

| Provider | data_type | Source Fields → Canonical |
|----------|-----------|--------------------------|
| Yahoo | quote | `regularMarketPrice → last`, `regularMarketVolume → volume`, `regularMarketChange → change`, `regularMarketChangePercent → change_pct`, `symbol → ticker` |
| Yahoo | news | `title → headline`, `publisher → source`, `link → url`, `providerPublishTime → published_at` |
| Polygon | ohlcv | `o → open`, `h → high`, `l → low`, `c → close`, `v → volume`, `vw → vwap`, `n → trade_count`, `t → timestamp` |
| Polygon | quote | `bidPrice → bid`, `askPrice → ask`, `lastTrade → last` |
| Polygon | news | `title → headline`, `publisher → source`, `article_url → url`, `published_utc → published_at` |
| IBKR | ohlcv | `wap → vwap`, `count → trade_count` (rest identity) |
| Generic | ohlcv | `o/h/l/c/v` short-form + identity mappings |
| Generic | quote | `bid`, `ask`, `last`, `volume`, `timestamp` identity |

### Presentation Mapping (canonical → template)

Applied after validation, before output:

| Canonical | Template | Purpose |
|-----------|----------|---------|
| `ticker` | `symbol` | Template-friendly name |
| `last` | `price` | Template-friendly name |

> 🔒 **Both field mappings and presentation mappings are hardcoded.** Custom field rename rules cannot be specified in the policy JSON.

---

## Hardcoded vs Dynamic Summary

### ✅ Dynamic — Configurable via Policy JSON (no code changes)

| What | Where |
|------|-------|
| Which provider to fetch from | `fetch.params.provider` |
| Which tickers to fetch | `fetch.params.criteria.tickers` |
| Which data type (ohlcv/quote/news) | `fetch.params.data_type` |
| Date range (relative or incremental) | `fetch.params.criteria.date_range` |
| Whether to use cache | `fetch.params.use_cache` |
| Target DB table for writes | `transform.params.target_table` |
| Write mode (append/replace/merge) | `transform.params.write_disposition` |
| Validation schema selection | `transform.params.validation_rules` |
| Quality threshold | `transform.params.quality_threshold` |
| Output key for template variable name | `transform.params.output_key` |
| Min records warning threshold | `transform.params.min_records` |
| Source step linkage | `transform.params.source_step_id` |
| Report name | `store_report.params.report_name` |
| SQL queries for snapshot | `store_report.params.data_queries` |
| Report spec metadata | `store_report.params.spec` |
| Render output format | `render.params.output_format` |
| Delivery channel (email/local) | `send.params.channel` |
| Email recipients | `send.params.recipients` |
| Email subject | `send.params.subject` |
| Template selection | `send.params.body_template` |
| Step skip conditions | Step-level `skip_if` |
| Step error handling | Step-level `error_mode` |
| Cross-step data references | `{ "ref": "ctx.step_id.field" }` |
| Schedule (cron) | Top-level `schedule` |
| Timezone | Top-level `timezone` |

### 🔒 Hardcoded — Requires Code Changes

| What | Where to Change | Priority |
|------|----------------|----------|
| Provider registry (add new APIs) | `provider_registry.py` | Medium |
| URL endpoint patterns per provider | `url_builders.py` | Medium |
| Response envelope extraction per provider | `response_extractors.py` | Medium |
| Field mapping rules per provider | `field_mappings.py` | Medium |
| Presentation mapping (canonical → template) | `transform_step.py` `_PRESENTATION_MAP` | Low |
| Validation schemas (Pandera) | `validation_gate.py` `SCHEMA_REGISTRY` | Low |
| Email templates | `email_templates.py` `EMAIL_TEMPLATES` | **High** |
| SMTP configuration | `main.py` startup, env vars | Low |
| Max recipients per send | `send_step.py` Pydantic `max_length=5` | Low |
| Cache TTL / market-closed multiplier | `fetch_step.py` constants | Low |
| Provider slug normalization | `field_mappings.py` `_PROVIDER_SLUG_MAP` | Low |
| Jinja2 custom filters (`currency`) | `template_engine.py` | Low |
| **Local DB query as data source** | **Not implemented** | **High** |

---

## Architectural Gaps

### 1. No Local DB Query Step ([PIPE-NOLOCALQUERY])

**Gap:** Cannot query internal tables (trades, trade_plans, watchlists, accounts) and pipe those records through Transform → Send like external data.

**Current state:**
- `FetchStep` = external HTTP only
- `StoreReportStep.data_queries` = SQL → snapshot (not pipeline-integrated)
- `CriteriaResolver.db_query` = SQL → date range only

**Proposed:** New `QueryStep` (type `"query"`) — runs read-only SQL, outputs records in same shape as FetchStep. See known-issues.md for full spec.

### 2. No Dynamic Email Templates

**Gap:** Adding a new email template requires editing Python source code.

**Proposed:** Template registry exposed via DB table or MCP resource. Policies would reference custom templates by name.

### 3. No Dynamic Field Mappings

**Gap:** Field mapping rules are hardcoded per provider. Cannot override or extend mappings via policy.

**Proposed:** Allow `transform.params.custom_mappings: { "src_field": "canonical_field", ... }` to supplement/override the built-in registry.

### 4. No Dynamic Presentation Mapping

**Gap:** `ticker → symbol` and `last → price` are the only presentation renames. Cannot configure custom renames per policy.

**Proposed:** Allow `transform.params.presentation_mapping: { "canonical": "display_name", ... }`.

### 5. No Custom Validation Schemas

**Gap:** Only 4 schemas (ohlcv, quote, news, fundamentals) exist. Cannot define custom schemas per policy.

**Proposed:** Allow inline Pandera-like schema definitions in policy JSON, or a schema registry table.

## Try it on the sample

The Improving Investor sample ships five draft policies and twenty frozen reports:

- [Activating your sample reports](https://github.com/zorivest/zorivest/blob/main/docs/user-guide/09-activating-sample-reports.md) — approve, enable and run a sample policy.
- [Stored reports](https://github.com/zorivest/zorivest/blob/main/docs/user-guide/08-stored-reports.md) — what each sample report contains.
