# Jinja2 Templates

## What Is It?

**Jinja2** is a templating engine for Python that allows you to create dynamic content by embedding variables and logic in text templates. Zorivest uses Jinja2 for email report templates.

---

## Basic Syntax

### Variables

```jinja2
Hello {{ user_name }}, your report is ready.
```

Variables are enclosed in `{{ }}` and are replaced with actual values when the template is rendered.

### Filters

```jinja2
{{ amount | round(2) }}
{{ date | default("N/A") }}
{{ name | upper }}
```

Filters modify variable output. Common filters:
- `round(n)` — round to n decimal places
- `default(value)` — fallback if variable is undefined
- `upper` / `lower` — case conversion
- `date` — format datetime objects

### Conditionals

```jinja2
{% if gain > 0 %}
  You're up {{ gain }}!
{% else %}
  You're down {{ gain | abs }}.
{% endif %}
```

### Loops

```jinja2
{% for trade in trades %}
  {{ trade.ticker }}: {{ trade.pnl }}
{% endfor %}
```

---

## Common Zorivest Template Variables

| Variable | Type | Description |
|----------|------|-------------|
| `{{ report_name }}` | string | Name of the pipeline report |
| `{{ generated_at }}` | datetime | When the report was generated |
| `{{ trades }}` | list | Trade records from the pipeline |
| `{{ summary }}` | dict | Aggregated metrics |

---

## Template Formats

Zorivest supports two body formats:

| Format | Use For |
|--------|---------|
| **HTML** | Rich email with styling, tables, colors |
| **Markdown** | Simple plain-text emails converted to HTML |

---

## Preview Before Deploy

Use the **Preview** button in the Templates tab to render your template with sample data before attaching it to a pipeline policy. This catches syntax errors and layout issues without sending a real email.

---

## How Zorivest Uses It

1. Create a template with Jinja2 variables in the **Templates** tab.
2. Reference the template in a pipeline policy's email step.
3. When the pipeline runs, the scheduler renders the template with real data and sends the email.

---

## References

- Jinja2 Official Documentation: [jinja.palletsprojects.com](https://jinja.palletsprojects.com/en/3.1.x/templates/)
- Jinja2 Template Designer Documentation: Covers all syntax, filters, and control structures.
