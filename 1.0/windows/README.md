# Ascora ADE 1.0 — Windows builds

| File | Type | Notes |
| --- | --- | --- |
| `Ascora-ADE-Setup-1.0.0.exe` | NSIS installer | Start-menu shortcut, choose install dir, uninstaller |
| `Ascora-ADE-Portable-1.0.0.exe` | Portable | Single exe, no install — just run |

- **Arch:** x64
- **Not code-signed** — Windows SmartScreen will show a "unknown publisher"
  warning on first run (More info → Run anyway). A signing cert removes this later.
- Verify integrity against `SHA256SUMS.txt`.

## Direct download URLs

```
https://github.com/AS-CoreAI/Ascora-ADE/raw/main/1.0/windows/Ascora-ADE-Setup-1.0.0.exe
https://github.com/AS-CoreAI/Ascora-ADE/raw/main/1.0/windows/Ascora-ADE-Portable-1.0.0.exe
```

Paste the installer URL into the Windows field in the site's `/control` panel to
publish this build to https://ade.ascoreai.com/.

> Built with `npm run dist:win` in the [Ascora ADE app repo](https://github.com/AS-CoreAI). electron-builder bundles only `out/` + `ssh2`; the renderer libs (Monaco, React, etc.) are already bundled by Vite, so each exe stays ~86 MB.
