# Zorivest Public Roadmap

Zorivest is a local-first trading **portfolio planning and analysis** tool. It imports trade
results from execution platforms, analyzes performance, and generates trade plans. It never
places, modifies, or cancels orders.

This document is a high-level public roadmap. Detailed engineering milestones live in the
private development hub and are not mirrored here.

## Current Focus

### Planning surfaces

Trade plans, watchlists, and position sizing help traders prepare before execution. In-app
**How it Works** help cards explain each planning surface in plain language with links to
reference material on this repository.

### Help and documentation

User-facing help content deep-links to reviewed reference guides and methodology docs published
in the public repository. Help search and contextual cards are expanded across Accounts,
Planning, Scheduling, Analytics, Tax, and Trades.

### Distribution

Unsigned Windows release candidates publish through GitHub Releases on `zorivest/zorivest`.
Each prerelease ships the setup executable, portable executable, and `SHA256SUMS.txt` for
hash verification before launch. Stable releases sync reviewed documentation from the private
hub through an audited render pipeline — raw private docs are never publication inputs.

## Architecture (summary)

- **Python backend** — domain, services, and REST API on port 17787
- **Electron + React GUI** — desktop client for planning and analysis
- **MCP server** — exposes Zorivest to AI agents over MCP

## Contributing

See `AGENTS.md` and `CONTRIBUTING.md` in this repository for build commands, test expectations,
and the dependency rule: domain code must never import infrastructure or API layers.
