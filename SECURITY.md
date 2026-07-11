# Security Policy

Zorivest is a local-first trading **portfolio planning and analysis** tool. It imports
trade results from execution platforms, analyzes performance, and generates trade plans —
it **never places, modifies, or cancels orders**. Because it handles financial records and
(optionally) encrypted local data, we take security reports seriously and appreciate your
help in keeping contributors and users safe.

## Supported Versions

Zorivest is pre-1.0 and evolves on a rolling basis. Security fixes are applied to the
default branch and the most recent tagged release.

| Version | Supported |
|---------|:---------:|
| `main` (latest commit) | ✅ |
| Latest tagged release | ✅ |
| Older tagged releases | ❌ (upgrade to the latest release) |

## Reporting a Vulnerability

**Do not file a public GitHub issue for security vulnerabilities.** Public disclosure
before a fix is available puts every user at risk.

Use a private reporting channel instead:

1. **Preferred — GitHub Private Vulnerability Reporting (PVR):**
   [Open a private advisory](https://github.com/zorivest/zorivest/security/advisories/new).
   This keeps the report confidential and lets us collaborate on a fix in a private fork.
2. **Fallback:** if you cannot use GitHub PVR, contact the maintainer privately through
   their GitHub profile ([@matbanik](https://github.com/matbanik)) and request a secure
   channel before sharing any details.

### What to include

A good report lets us reproduce and assess impact quickly. Please provide:

- **Description** — what the vulnerability is and the component it affects.
- **Reproduction steps** — a minimal, ordered sequence (and a proof-of-concept if you have one).
- **Impact assessment** — what an attacker can read, modify, or execute.
- **Suggested fix** — optional, but very welcome.

## Response Targets (SLA)

We aim to meet the following timelines for confirmed reports:

| Stage | Target |
|-------|--------|
| Acknowledgment of your report | Within **48 hours** |
| Initial triage and severity assessment | Within **5 business days** |
| Fix for critical issues | Within **30 days** of triage |

These are targets, not guarantees — Zorivest is maintained by a small team. We will keep you
updated if a fix requires longer, and we will credit you (with your consent) in the advisory.

## Scope

**In scope** — vulnerabilities in Zorivest's own code that could lead to:

- Unauthorized read/write/exfiltration of local portfolio or trade data.
- Bypass of the optional SQLCipher encryption or key handling.
- Remote code execution, command injection, SQL injection, or path traversal.
- Compromise of the local API (`127.0.0.1:17787`) or the Electron main↔renderer IPC boundary.
- Exposure of secrets/API keys stored or logged by the application.

**Out of scope:**

- Vulnerabilities in third-party dependencies with no exploitable path through Zorivest
  (report those upstream; tell us if Zorivest's usage makes them exploitable).
- Findings that require a pre-compromised host, physical access, or a malicious OS account.
- Social-engineering, phishing, or issues in community/unofficial data-source providers'
  own systems (see the README data-source disclaimer).
- Missing security hardening with no demonstrated impact (e.g., "header X is absent").

## CVE Handling

For confirmed critical or high-severity vulnerabilities, we coordinate disclosure and, where
warranted, request a CVE identifier via GitHub's advisory workflow (CNA) or by coordinating
with MITRE. We follow coordinated disclosure: a public advisory is published once a fix is
released, crediting the reporter unless anonymity is requested.

## No Bug Bounty

**Zorivest does not offer a bug bounty or monetary rewards at this time.** We deeply value
responsible disclosure and will publicly credit reporters in the security advisory (with
your consent).

## Confidentiality

Please keep vulnerability details confidential until a fix has been released and an advisory
published. **Do not file public issues for security vulnerabilities** — use the private
channels above.
