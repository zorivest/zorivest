# Encryption Posture

> **milestone-tracking** (§7A.12e AC-3) — User-facing documentation of the Zorivest encryption posture.

## Summary

Zorivest stores portfolio data in a SQLite database. The encryption status depends on
your operating system and installed packages.

## Windows

**Status: Plaintext at rest (not encrypted)**

The database is stored without encryption on Windows. This is because SQLCipher
(`sqlcipher3-binary`), the library that provides database-level encryption, does not
publish pre-built Windows wheels (`win_amd64`). There is no way to install it via
`pip` or `uv` on Windows without compiling from source.

### Recommended Mitigation: BitLocker

Enable **BitLocker** full-disk encryption to protect all data at rest, including the
Zorivest database:

1. Open **Windows Settings** → **Privacy & Security** → **Device Encryption**
2. Enable BitLocker on your system drive
3. Store your recovery key in a secure location

BitLocker encrypts the entire disk transparently — no application changes needed.

> **Note:** BitLocker is available on Windows 10/11 Pro, Enterprise, and Education editions.
> Windows Home editions support Device Encryption on compatible hardware.

## Linux

**Status: Encrypted at rest (optional)**

On Linux, install the `[crypto]` extra to enable SQLCipher database encryption:

```bash
uv pip install zorivest-infra[crypto]
# or
pip install zorivest-infra[crypto]
```

This installs `sqlcipher3-binary` and `argon2-cffi`, enabling transparent database
encryption with AES-256. When installed, Zorivest automatically uses SQLCipher for
all database connections.

## macOS

**Status: Same as Linux** — the `[crypto]` extra is available and installs
`sqlcipher3-binary` via pre-built wheels.

## How to Verify

Use the **Setup Readiness** panel in Settings to check your encryption status:

- **SQLCipher installed:** Database is encrypted at rest ✅
- **SQLCipher not installed:** Database is plaintext at rest ⚠️
  - On Windows: BitLocker recommendation is shown
  - On Linux/macOS: Install command for `[crypto]` extra is shown

## Technical Details

- **ADR:** ADR-001: Optional SQLCipher Encryption Contract
- **Build Plan:** `07a-distribution-portable.md` §7A.12e
- **Implementation:** `packages/infrastructure/src/zorivest_infra/persistence/database.py`
- **Readiness Check:** `packages/api/src/zorivest_api/routes/readiness.py`
