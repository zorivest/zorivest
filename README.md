# Zorivest

> **Privacy-first, local-first desktop trading journal & portfolio tracker**

Zorivest helps you plan, track, and analyze your trading portfolio — entirely on your own machine. Your data stays local, encrypted, and under your control.

## ⚠️ Safety Statement

**Zorivest does NOT execute trades.** It imports trade results from execution platforms (Interactive Brokers, Alpaca, Tradier, etc.), analyzes performance, and generates trade plans. It never places, modifies, or cancels orders on your behalf.

## Features

- **Local-First Architecture** — All data stored locally in an encrypted SQLite database (SQLCipher AES-256). No cloud dependency for core functionality.
- **Multi-Broker Import** — Import trades from Interactive Brokers (TWS), Alpaca, Tradier, and CSV files from most brokers.
- **Portfolio Analytics** — Sharpe ratio, drawdown analysis, P&L tracking, win/loss ratios, and custom metrics.
- **Tax Reporting** — Wash sale detection, Schedule D / Form 8949 preparation, tax-lot optimization.
- **Trade Planning** — Create and evaluate trade plans before execution. Model position sizing, risk limits, and expected outcomes.
- **Options Chain Analysis** — Real-time options data with Greeks, probability of ITM, and put/call ratios.
- **Policy Engine** — Define trading rules and constraints; get alerts when planned trades violate your policies.

## Download

### Windows

| Artifact | Description |
|----------|-------------|
| `zorivest-*-setup.exe` | Windows installer (recommended) |
| `zorivest-*-portable.exe` | Portable — no installation required |

Download the latest release from the [Releases page](https://github.com/zorivest/zorivest/releases).

### Verify Your Download

Every release includes a `SHA256SUMS.txt` file. To verify your download:

```powershell
# PowerShell
Get-FileHash zorivest-*-setup.exe -Algorithm SHA256 | Format-List
# Compare the hash with SHA256SUMS.txt
```

### Windows SmartScreen

On first launch, Windows SmartScreen may show a "Windows protected your PC" warning because the app is new and building reputation. Click **"More info"** → **"Run anyway"** to proceed. This is expected for new applications.

## Privacy Architecture

```
┌─────────────────────────────┐
│  Your Machine               │
│  ┌───────────────────────┐  │
│  │ Zorivest Desktop App  │  │
│  │  React GUI + Electron │  │
│  │  Python FastAPI Backend│  │
│  │  127.0.0.1:17787      │  │
│  └──────────┬────────────┘  │
│             │               │
│  ┌──────────▼────────────┐  │
│  │ SQLCipher Database    │  │
│  │ AES-256 Encrypted     │  │
│  │ Local storage only    │  │
│  └───────────────────────┘  │
└─────────────────────────────┘
         No cloud.
         No telemetry.
         No account required.
```

- **No cloud dependency** — The app runs a local Python backend on `127.0.0.1:17787`. All API calls stay on your machine.
- **No telemetry** — Zero analytics, tracking, or phone-home behavior.
- **No account required** — Use the full application without creating an account or providing an email.
- **Optional encryption** — Enable SQLCipher AES-256 encryption for your database with your own passphrase.

## Documentation

- [Reference Documentation](https://github.com/zorivest/zorivest/tree/main/docs/reference) — Detailed explanations of features and calculations
- [User Guides](https://github.com/zorivest/zorivest/tree/main/docs/guides) — How-to guides for common workflows

## Reporting Issues

- [Bug Reports](https://github.com/zorivest/zorivest/issues/new?template=bug_report.yml)
- [Feature Requests](https://github.com/zorivest/zorivest/issues/new?template=feature_request.yml)
- [Security Vulnerabilities](https://github.com/zorivest/zorivest/security/advisories/new) — **Do not file public issues for security vulnerabilities**

## License

Zorivest is proprietary software distributed as compiled binaries. See [TERMS.md](TERMS.md) for the terms governing binary downloads and usage. Source code is not publicly available.
