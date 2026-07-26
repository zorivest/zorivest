# AI Search & Generate Pipeline Steps

> **Requires:** `schema_version: 2` in your policy JSON.

Zorivest's pipeline engine includes two AI-powered step types for automated research and analysis:

## `web_search` — Search the Web

Executes a web search via a configured provider and returns normalised results.

### Parameters

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `provider` | `"tavily" \| "exa"` | ✅ | — | Which search provider to use |
| `query` | `string \| {ref}` | ✅ | — | Search query (or a `ctx.` reference) |
| `max_results` | `1–10` | — | `5` | Maximum results to return |
| `time_range` | `"day" \| "week" \| "month" \| "year"` | — | `null` | Recency filter |
| `start_date` | `YYYY-MM-DD` | — | `null` | Start date filter |
| `end_date` | `YYYY-MM-DD` | — | `null` | End date filter |
| `include_domains` | `string[]` | — | `null` | Only search these domains |
| `exclude_domains` | `string[]` | — | `null` | Exclude these domains |
| `include_answer` | `bool` | — | `null` | Request an AI-generated answer from the search provider |
| `output_key` | `string` | — | `"web_search"` | Key in `context.outputs` for downstream steps |

### Output Shape

```json
{
  "<output_key>": {
    "provider": "tavily",
    "query": "stock market trends",
    "results": [
      { "url": "...", "title": "...", "snippet": "...", "score": 0.95 }
    ],
    "response_time_ms": 150,
    "answer": "..."
  }
}
```

---

## `ai_generate` — AI Text Generation

Sends a prompt (optionally enriched with search results) to an AI chat model and returns the generated text with source citations.

### Parameters

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `prompt` | `string \| {ref}` | ✅ | — | User prompt for the AI model |
| `system` | `string` | — | `null` | System prompt (max 32,000 chars) |
| `provider` | `"ollama" \| "lm_studio" \| "openrouter"` | — | active | Override provider |
| `context_refs` | `string[]` | — | `[]` | Step outputs to inject as context (e.g., `["ctx.search"]`) |
| `max_tokens` | `1–4096` | — | `1024` | Maximum response tokens |
| `temperature` | `0.0–1.0` | — | `0.3` | Sampling temperature |
| `output_key` | `string` | — | `"ai_content"` | Key in `context.outputs` |

### Output Shape

```json
{
  "<output_key>": {
    "text": "AI-generated analysis...",
    "model": "llama-3.1-8b",
    "provider": "ollama",
    "sources": [
      { "id": 1, "url": "https://...", "title": "Source Title" }
    ]
  }
}
```

### Context Refs

The `context_refs` array lets the AI model see outputs from previous steps. Format: `ctx.<step_id>[.<key>]`.

When a context ref points to a `WebSearchResponse`-shaped dict (has a `results` array), each result is formatted as an XML `<source>` block and injected into the prompt with citation instructions.

---

## Provider Setup

### Tavily (Web Search)

1. Create a free account at [tavily.com](https://tavily.com)
2. Copy your API key (starts with `tvly-`)
3. In Zorivest → Settings → AI Providers, add a Tavily provider with your key
4. Validate the connection

### Exa (Web Search)

1. Create an account at [exa.ai](https://exa.ai)
2. Copy your API key
3. In Zorivest → Settings → AI Providers, add an Exa provider

### OpenRouter / Ollama / LM Studio (AI Generate)

- **OpenRouter**: Get an API key from [openrouter.ai](https://openrouter.ai), configure in Settings → AI Providers
- **Ollama**: Install locally, ensure it's running on the default port (11434)
- **LM Studio**: Install and load a model, configure the local server URL

---

## Credit & Pricing

| Provider | Free Tier | Paid | Notes |
|----------|-----------|------|-------|
| Tavily | 1,000 searches/month | $0.01/search | API key required |
| Exa | 1,000 searches/month | Usage-based | API key required |
| Ollama | Unlimited (local) | Free | Requires local GPU |
| LM Studio | Unlimited (local) | Free | Requires local GPU |
| OpenRouter | Pay-per-token | Varies by model | Many models available |

---

## Version Requirement

Both `web_search` and `ai_generate` are **v2 features**. Your policy JSON must specify:

```json
{
  "schema_version": 2,
  ...
}
```

Using these step types with `schema_version: 1` will cause a validation error:

> "Features used require schema_version >= 2: variables, query/compose/web_search/ai_generate step types, or assertion kind. Set schema_version: 2 in your policy JSON."

---

## Example: AI Morning Research Digest

A complete policy that searches financial news, generates an AI analysis, and emails it:

```json
{
  "schema_version": 2,
  "name": "AI Morning Research Digest",
  "trigger": {
    "cron_expression": "0 7 * * 1-5",
    "timezone": "America/New_York",
    "enabled": false
  },
  "steps": [
    {
      "id": "search_news",
      "type": "web_search",
      "params": {
        "provider": "tavily",
        "query": "stock market financial news today",
        "max_results": 5,
        "time_range": "day"
      }
    },
    {
      "id": "generate_digest",
      "type": "ai_generate",
      "params": {
        "prompt": "Analyze the following financial news and produce a concise morning research digest.",
        "context_refs": ["ctx.search_news"],
        "output_key": "ai_content",
        "max_tokens": 2048,
        "temperature": 0.3
      }
    },
    {
      "id": "render_report",
      "type": "render",
      "params": { "template": "ai_research_digest", "output_format": "html" }
    },
    {
      "id": "send_email",
      "type": "send",
      "params": {
        "channel": "email",
        "recipients": ["your@email.com"],
        "subject": "AI Morning Research Digest",
        "body_template": "ai_research_digest"
      }
    }
  ]
}
```

---

## Manual Smoke Test

```bash
# 1. Ensure Tavily key is configured in Settings → AI Providers
# 2. Create the policy via MCP or API:
curl -X POST httlocal workspace \
  -H "Content-Type: application/json" \
  -d @ai-digest-policy.json

# 3. Approve and trigger:
curl -X POST httlocal workspace
curl -X POST httlocal workspace

# 4. Check run history:
curl httlocal workspace
```

---

*Spec: 16-ai-web-search.md §16.6 (milestone-tracking)*
