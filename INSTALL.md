# Install Zorivest (Windows)

Zorivest publishes two unsigned Windows artifacts: `zorivest-*-setup.exe` (installer) and `zorivest-*-portable.exe`. There is no other OS build. Download both executables and `SHA256SUMS.txt` from the same GitHub Release and verify hashes before you run anything. See the public README for the checksum commands.

## Why Windows shows a warning

These binaries do not carry an Authenticode signature. Microsoft SmartScreen application reputation accrues over time; an unrecognized or newly published file can warn even after a future signature exists. Signing does not remove the warning immediately.

Primary source: https://learn.microsoft.com/en-us/windows/apps/package-and-deploy/smartscreen-reputation

## SmartScreen sequence

1. Windows may display **Windows protected your PC**.
2. Click **More info**.
3. Click **Run anyway**.

Windows 11 **Smart App Control** may hard-block an unsigned file with no Run anyway control. That is a possible hard stop on some machines, not a guaranteed bypass.

## Unblock-File (download zone)

If File Explorer attached a download mark, you can remove the `Zone.Identifier` alternate data stream:

```powershell
Unblock-File -Path .\zorivest-*-setup.exe
```

`Unblock-File` removes `Zone.Identifier`. It does not guarantee launch and does not bypass SmartScreen, Smart App Control, or enterprise policy.

Reference: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/unblock-file
