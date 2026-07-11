# Changelog

All notable changes to Zorivest will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added
- Initial public release of Zorivest desktop application
- Windows installer (.exe) and portable executable distributions
- Portfolio tracking with multi-broker import support (Interactive Brokers, Alpaca, Tradier, CSV)
- Trade journal with tagging, notes, and screenshot attachments
- Analytics dashboard: Sharpe ratio, drawdown, P&L, win/loss ratios
- Options chain browser with Greeks and probability of ITM
- Tax reporting: wash sale detection, Schedule D preparation
- Policy engine for trading rule enforcement
- Local-first architecture with optional SQLCipher AES-256 encryption
- SHA256SUMS.txt verification for all release artifacts

### Security
- All data stored locally — no cloud dependency, no telemetry
- Optional database encryption with user-controlled passphrase
- Local API server bound to 127.0.0.1 only (not exposed to network)
