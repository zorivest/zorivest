# Release Distribution Guide

## Purpose

Zorivest publishes unsigned Windows release candidates through the public
`zorivest/zorivest` GitHub Releases page. Each prerelease contains the setup
executable, the portable executable, and `SHA256SUMS.txt`.

## Verify a Download

1. Download all three assets from the same release.
2. Run `Get-FileHash -Algorithm SHA256` for both executables.
3. Compare the lowercase hashes with the two entries in `SHA256SUMS.txt`.
4. Do not launch an executable whose hash differs.

## Documentation

Reference material and user guides in this repository are reviewed public
outputs. Release candidates do not run deletion-capable documentation sync;
stable releases verify the reviewed source/output map before mirroring updates.

## Trust Status

The current release candidate is intentionally unsigned and marked as a
prerelease, not Latest. Windows may display SmartScreen or publisher warnings.
