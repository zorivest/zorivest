# AGENTS.md — Zorivest (Public / Contributor Guide)

> This file gives AI coding tools (Claude Code, GitHub Copilot, Cursor, Devin Desktop, Gemini CLI,
> etc.) the commands and constraints needed to work in this repository as an external
> contributor. It is intentionally slim — see CONTRIBUTING.md for the
> human process (values, PR flow, AI-disclosure policy) and
> docs/DEVELOPMENT.md for the full environment walkthrough.

## Project Identity

Zorivest is a local-first trading **portfolio planning and analysis** tool. It imports trade
results from execution platforms, analyzes performance, and generates trade plans. **It never
places, modifies, or cancels orders** — treat any code path that looks like order execution as
out of scope unless explicitly asked for.

## Architecture

- **Python backend**, a `uv` workspace: `packages/core` (domain + services),
  `packages/infrastructure` (persistence, market data), `packages/api` (FastAPI REST layer).
- **TypeScript frontend**: `ui/` — Electron + React, built with electron-vite.
- **MCP server**: `mcp-server/` — exposes Zorivest to AI agents over MCP.

## Dependency Rule (Non-Negotiable)

Domain → Application → Infrastructure. **`packages/core` must never import from
`packages/infrastructure` or `packages/api`.** Inner layers never import outer layers. This is
enforced by review and by `tools/validate_codebase.py`.

## Build Commands

```bash
uv sync                 # install Python deps (all 3 workspace packages)
cd ui && npm install    # install UI deps
cd mcp-server && npm install
```

## Test Commands

```bash
uv run pytest                                  # Python unit + integration tests
uv run pyright packages/                       # strict type check
uv run ruff check packages/                    # lint
cd ui && npm run test                          # vitest
cd ui && npm run lint && npm run typecheck
uv run python tools/validate_codebase.py       # full repo quality gate — run before any PR
```

> There is intentionally no `Makefile` — call `uv`/`npm` commands directly.

## Canonical Port

**`17787`** — the backend API, bound to `127.0.0.1` only. Every reference to "the API" in code,
docs, or tests means this port. (Vite dev server is `5173`, unrelated.)

## TDD Mandate

Tests are the specification. **Write the failing test first**, confirm it fails for the right
reason, then implement. Bug fixes require a regression test that reproduces the bug *before*
any production-code change. **Never modify a test's assertions to make it pass** — fix the
implementation instead. See CONTRIBUTING.md §7 for the full policy.

## Anti-Placeholder

Do not submit `TODO`, `FIXME`, or `NotImplementedError` stubs as finished work. If a task is
genuinely blocked, say so explicitly rather than leaving a placeholder.

## Cross-References

- Human contribution process, PR flow, AI-disclosure policy: CONTRIBUTING.md
- Full environment setup, troubleshooting, ports: docs/DEVELOPMENT.md
- Security vulnerability reporting: SECURITY.md
