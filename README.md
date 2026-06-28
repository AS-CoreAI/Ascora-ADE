# Ascora ADE

**Agentic Development Environment** — a cross-platform desktop IDE with an AI agent
inside it. Describe a task in plain language; Ascora ADE reads your code, plans,
edits files, runs commands, and shows a **diff before applying** — powered by the
models you run yourself. Local-first, offline-capable, and entirely in your control.

![Ascora ADE — agent editing a file with a reviewable diff](docs/screenshot.png)

This repository is the **release / distribution channel**: it hosts the prebuilt,
ready-to-run binaries and their checksums. The application and website source live
in their own repositories under the [AS-CoreAI](https://github.com/AS-CoreAI) org.

- **Live demo & downloads:** https://ade.ascoreai.com/
- **Current version:** 1.0.0
- **Platforms:** Windows (x64), Linux (x64)

## Supported models — one agent, every backend

Switch provider and model per task from the composer; each workspace remembers its
own choice.

| Backend | Models | Connection | Notes |
| --- | --- | --- | --- |
| **LM Studio** *(default)* | any local model you load (e.g. `qwen2.5-coder`) | OpenAI-compatible HTTP API (`http://localhost:1234/v1`), SSE streaming | Fully offline — your code never leaves the machine |
| **Claude Code** | Claude **Opus · Sonnet · Haiku** | local `claude` CLI (auto-detected) | Selectable permission mode (e.g. accept-edits) |
| **Codex** | OpenAI **GPT‑5.x** | local `codex` CLI (auto-detected) | Sandbox policy + reasoning-effort control |
| **GLM / ZCode** | Zhipu **GLM** (e.g. `glm‑4.6`) | bundled ZCode agent (auto-detected) | Permission modes: plan / build / edit / yolo |

> **Bring your own model.** Run fully offline against a local LLM via LM Studio, or
> delegate a task to the Codex, Claude Code, or GLM/ZCode CLI agents. Nothing is
> uploaded unless you choose a hosted backend.

## Features

- **Agentic edit loop** — read → plan → edit → run → verify, in one place.
- **Diffs before disk** — every change is shown as a side-by-side diff you can
  approve or reject; or switch to **Auto-apply** to let a task run end to end.
- **Transparent reasoning** — collapsible thought process, streaming tool cards
  (output, diffs, exit codes), and a live token + time counter.
- **Monaco editor** — the editor that powers VS Code, bundled and fully offline.
- **Integrated terminal** — run builds and tests without leaving the workspace
  (xterm.js + PTY).
- **Git, with AI commits** — stage, diff, branch, and push, or let the agent write
  the commit message.
- **SSH terminals** — drive remote hosts from the same agent loop (`ssh2`).
- **Live preview** — built-in server renders your HTML as you change it.
- **Usage analytics** — track tokens, models, and tasks across every workspace.
- **Dockable panels** — a VS Code-style layout you can split, stack, and re-dock.

## Download

### Windows (x64)

| File | Type | Notes |
| --- | --- | --- |
| [`Ascora-ADE-Setup-1.0.0.exe`](https://github.com/AS-CoreAI/Ascora-ADE/raw/main/1.0/windows/Ascora-ADE-Setup-1.0.0.exe) | NSIS installer | Start-menu shortcut, choose install dir, uninstaller |
| [`Ascora-ADE-Portable-1.0.0.exe`](https://github.com/AS-CoreAI/Ascora-ADE/raw/main/1.0/windows/Ascora-ADE-Portable-1.0.0.exe) | Portable | Single `.exe`, no install — just run |

> **Not code-signed.** On first launch Windows SmartScreen shows an "unknown
> publisher" warning — choose **More info → Run anyway**. A signing certificate
> will remove this in a later release.

### Linux (x64)

| File | Package | For |
| --- | --- | --- |
| [`Ascora-ADE-1.0.0-amd64.deb`](https://github.com/AS-CoreAI/Ascora-ADE/raw/main/1.0/linux/Ascora-ADE-1.0.0-amd64.deb) | `.deb` | Ubuntu / Debian |
| [`Ascora-ADE-1.0.0-x86_64.rpm`](https://github.com/AS-CoreAI/Ascora-ADE/raw/main/1.0/linux/Ascora-ADE-1.0.0-x86_64.rpm) | `.rpm` | Fedora / CentOS / RHEL |
| [`Ascora-ADE-1.0.0-x64.pacman`](https://github.com/AS-CoreAI/Ascora-ADE/raw/main/1.0/linux/Ascora-ADE-1.0.0-x64.pacman) | `.pacman` | Arch |

> macOS builds are published on the [website](https://ade.ascoreai.com/).

## Install

**Windows** — run the installer, or just double-click the portable `.exe`.

**Linux**

```bash
# Debian / Ubuntu
sudo apt install ./Ascora-ADE-1.0.0-amd64.deb

# Fedora / CentOS / RHEL
sudo rpm -i Ascora-ADE-1.0.0-x86_64.rpm

# Arch
sudo pacman -U Ascora-ADE-1.0.0-x64.pacman
```

## Verify the download

Each platform folder ships a `SHA256SUMS.txt`. Verify integrity before running:

```bash
# Linux / macOS
sha256sum -c SHA256SUMS.txt
```

```powershell
# Windows (PowerShell) — compare against the value in SHA256SUMS.txt
Get-FileHash .\Ascora-ADE-Setup-1.0.0.exe -Algorithm SHA256
```

## Quick start

1. **Open a project** — point Ascora ADE at any folder; it indexes the tree and
   connects to your chosen model.
2. **Describe a task** — in plain language ("add a dark-mode toggle", "fix the
   failing test", "refactor this").
3. **Review the diff** — the agent plans and proposes edits as clear, reviewable
   diffs.
4. **Apply with confidence** — approve to write to disk, or let Auto-apply run the
   whole task end to end.

## Repository layout

```
1.0/
├── windows/   # NSIS installer + portable .exe, SHA256SUMS, notes
└── linux/     # .deb / .rpm / .pacman packages, SHA256SUMS
```

Each new release adds a version folder (e.g. `1.1/`) alongside `1.0/`.

## Build notes

Binaries are produced from the app source with electron-vite + electron-builder
(`npm run dist:win`, `npm run dist:linux`). The app is **Electron + React +
TypeScript** (renderer bundled by Vite); persistence uses `better-sqlite3` with an
atomic JSON-file fallback when native build tools are unavailable. electron-builder
bundles the main process (`out/`) plus `ssh2`; the renderer libs (Monaco, React,
etc.) are already bundled by Vite, keeping each Windows binary at ~86 MB.

## Links

- Website & live demo — https://ade.ascoreai.com/
- Organization — https://github.com/AS-CoreAI
