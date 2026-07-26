# Help Search — In-Context Search for Help Cards

## What It Does

Help Search adds a **local search field** inside every expandable help card (Analytics, Trades, Tax, Scheduling, Options Chain, Accounts, Trade Plans, Watchlists). Type a keyword and the card instantly filters its content to show only matching items, with highlights on matching text.

## How to Use

1. **Expand** any help card (click the ℹ️ toggle)
2. **Type** in the search box — results filter as you type
3. **View** highlighted matches (yellow `<mark>` tags)
4. **Read** the match count (e.g., "3 of 12") in the live region
5. **Press Esc** to clear the search and see all content again
6. **Collapse** the card to automatically reset search

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| Type any text | Filter items in real-time (AND-token matching) |
| `Escape` | Clear search and restore all items |
| `Tab` / `Shift+Tab` | Navigate between search field and content |

## Behavior Details

### AND-Token Matching

Searches use **AND-token matching**: each space-separated word in your query must appear somewhere in an item's searchable fields. For example, `"trade profit"` matches items containing both "trade" and "profit" (in any field).

### Diacritic + Case Insensitive

Searches are case-insensitive and normalize diacritics. `"Δ"` matches `"delta"`, and `"café"` matches `"cafe"`.

### Card-Specific Behavior

| Card | Searchable Fields | Notes |
|------|------------------|-------|
| **Analytics** (tab-scoped) | term, tldr, whatFor | Filters individual metric items |
| **Analytics** (legacy) | term, definition, interpretation | Filters metric definition cards |
| **Trade Tab Explainer** | term, tldr, purpose | Filters explainer terms |
| **Tax Help Card** | what, source, calculation, learnMoreLabel | Single-card flat match (shows/hides entire body) |
| **Scheduling Help Card** | what, source, calculation, learnMoreLabel (+ optional tutorialLabel) | Same flat pattern as Tax; Watch tutorial via openExternal |
| **Options Chain** | Overview: what, source, howToUse; Section titles; Term: term, definition, interpretation | Section-aware: hides sections when all terms filtered out; section stays if title matches |
| **Accounts Help Card** | what, source, calculation, learnMoreLabel (+ optional tutorialLabel) | Flat match; milestone-tracking How it Works host |
| **Trade Plan Help Card** | what, source, calculation, learnMoreLabel (+ optional tutorialLabel) | Flat match; milestone-tracking How it Works host |
| **Watchlist Help Card** | what, source, calculation, learnMoreLabel (+ optional tutorialLabel) | Flat match; milestone-tracking How it Works host |

### Empty State

When no items match the query, a message appears: _"No help terms match 'your query'"_

### Reset on Collapse

Collapsing a help card automatically clears the search. When you re-expand, you see all content.

## Architecture

The search is powered by three shared primitives in `@/components/help/`:

- **`useHelpSearch(records, query)`** — React hook that normalizes text, applies AND-token matching, and returns `{ filteredIds, isFiltering, matchCount, totalCount }`
- **`HelpSearchField`** — Controlled input with `role="searchbox"`, Esc-to-clear, and accessible count region
- **`highlightSegments(text, query)`** — Splits text into segments and wraps matches in `<mark data-testid="help-search-mark">`

Each host card builds an adapter that converts its content shape into `HelpSearchRecord[]` (flat array of `{ id, fields[] }`).
