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
- **Current version:** 1.3.1
- **Platforms:** Windows (x64), Linux (x64)

## What's new in 1.3.1

- **Full-page settings in eleven languages** — appearance, agent providers, CLI
  installation and authentication, usage limits, and developer tools in one place.
- **Unsloth local models** — configure a local server and API key, discover models,
  stream replies, and track token usage alongside LM Studio and Ollama.
- **CLI usage limits** — remaining allowances, reset dates, automatic refresh, and
  independent checks for installed, signed-in Codex, Claude, and Antigravity accounts.
- **Google Antigravity and Grok Build** — additional agent backends, account checks,
  model selection, session resume, streaming, and tool calls. Antigravity's Gemini
  models offer their supported reasoning levels separately from the model choice.
- **Reliable Antigravity responses** — completed answers now arrive after reasoning,
  transcript updates are recovered, and missing answers produce an explicit error.
- **OmniRoute startup fixes** — background prewarming, direct server startup, and
  built-in SQLite support; packaged gateway startup and persistence are verified.
- **Compact Codex activity and terminal improvements** — grouped work logs,
  a terminal shortcut, and persistent command history.
- **Connection fixes** — local-provider switching and cancellation, VPN account
  identity and traffic attribution, and loopback requests outside VPN interception.

Read every change in the [full changelog](1.3.1/RELEASE_NOTES.md) or the
[GitHub release](https://github.com/AS-CoreAI/Ascora-ADE/releases/tag/v1.3.1).

## Supported models — one agent, every backend

Switch provider and model per task from the composer; each chat remembers its
own choice.

| Backend | Models | Connection | Notes |
| --- | --- | --- | --- |
| **LM Studio** *(default)* | any local model you load (e.g. `qwen2.5-coder`) | OpenAI-compatible HTTP API (`http://localhost:1234/v1`), SSE streaming | Fully offline — your code never leaves the machine |
| **Unsloth** | local models served by Unsloth | OpenAI-compatible HTTP API with an API key | Model discovery, streaming, tool support, and per-chat model choice |
| **Ollama** | any local model you pull | OpenAI-compatible HTTP API (`http://localhost:11434/v1`), SSE streaming | Fully offline; probed in the background like LM Studio |
| **Ascora WProvider** | **Qwen · DeepSeek · Alice · Mistral · Claude · Grok · Gemini · ChatGPT** | provider's own web chat via a hidden browser window | Sign in once in a visible window; per-service sign-in status; works over SSH |
| **OpenRouter** | any hosted model on OpenRouter | OpenAI-compatible cloud API with your API key | Optional cloud backend |
| **Claude Code** | Claude **Opus · Sonnet · Haiku · Fable** | local `claude` CLI (auto-detected) | Selectable permission mode (e.g. accept-edits); aliases resolve to the newest model of each family |
| **Codex** | OpenAI **GPT‑5.x**, including the **GPT‑5.6** presets | local `codex` CLI (auto-detected) | Sandbox policy + reasoning-effort control |
| **Gemini CLI** | Google **Gemini** | local `gemini` CLI | Approval modes: plan / default / auto_edit / yolo; session resume |
| **Grok Build** | xAI Grok | local `grok` CLI | Account sign-in, reasoning effort, permission modes, and session resume |
| **Antigravity** | Gemini, Claude, and GPT models exposed by the local runtime | local Antigravity runtime / Python bridge | Model and reasoning selection, quotas, streaming, and session resume |
| **OmniRoute** | models from configured upstream providers | bundled local gateway | Provider catalog, routed chat, background startup, and usage tracking |
| **GLM / ZCode** | Zhipu **GLM** (e.g. `glm‑4.6`) | bundled ZCode agent (auto-detected) | Permission modes: plan / build / edit / yolo |

> **Bring your own model.** Run fully offline against a local LLM via LM Studio or
> Ollama, delegate a task to the Codex, Claude Code, Gemini, or GLM/ZCode CLI
> agents, or drive a provider's web chat directly through Ascora WProvider. Nothing
> is uploaded unless you choose a hosted backend.

## Features

### The agent

- **Agentic edit loop** — read → plan → edit → run → verify, in one place.
- **Diffs before disk** — every change is shown as a side-by-side diff you can
  approve or reject; or switch to **Auto-apply** to let a task run end to end.
- **Transparent reasoning** — a collapsible thought process for every backend,
  including the provider's own extended thinking (Grok, DeepSeek, Qwen) streamed
  live into a "Thought for 2m" block, plus streaming tool cards (output, diffs,
  exit codes) and a live token + time counter.
- **A real tool belt** — list/read/write/edit files, ripgrep-style search
  (regex, glob, context lines, case modes), one-shot shell commands, a bounded
  `run_typescript` runtime, and read-only internet access via `web_search` and
  `web_fetch` — locally and over SSH.
- **Per-chat model pinning** — each chat keeps its own model/provider selection
  independently of the workspace default.
- **Attachments** — files and images attach as visual chips with previews, kept
  out of the visible prompt text.

### Backends

- **Web-chat backends** — relay agent turns through eight provider web chats via
  the built-in Ascora WProvider: hidden-browser automation (off-screen even when
  a real Chrome/Edge is required), streaming, tool use, per-service sign-in
  status, and an authorized-models table.
- **Local, cloud, and CLI** — LM Studio and Ollama fully offline, OpenRouter by
  API key, and the Codex / Claude Code / Gemini / GLM CLI agents with account
  display and sign-in/sign-out from settings.

### Workspace

- **Monaco editor** — the editor that powers VS Code, bundled and fully offline.
- **Integrated terminal** — run builds and tests without leaving the workspace
  (xterm.js + PTY).
- **Git, with AI commits** — stage, diff, branch, and push, or let the agent
  write the commit message; unified diffs render with Monaco syntax
  highlighting, per-file `+`/`−` counts, and binary badges.
- **SSH terminals** — drive remote hosts from the same agent loop (`ssh2`), with
  per-host saved chat history.
- **Live preview** — built-in server renders your HTML as you change it, inside
  Ascora ADE or in the system browser.
- **Explorer with drag-and-drop** — move files and folders in the tree; rename
  workspace projects in place.
- **Dockable panels** — a VS Code-style layout you can split, stack, and re-dock.

### Automation

- **Blueprint studio** — project-independent scenarios on an Unreal-style node
  graph: conditional routing, bounded Repeat loops, canvas notes, File / Command /
  HTTP request actions, interval schedules, Telegram delivery, and webhooks.
- **Multi-agent teams** — several named WProvider agents with independent roles
  and sessions pass results to each other; agent mode gives every participant
  the full tool loop with a persisted audit trail.
- **Blueprint Hub** — connect one or several scenarios to a shared prompt and
  watch their pipelines stream into one team chat.

### Housekeeping

- **Archives that restore** — soft-delete tasks or whole projects (with every
  chat) and bring them back later; nothing on disk is deleted.
- **Usage analytics** — tokens, models, sessions, estimated API-equivalent cost,
  model share over time, and activity heatmaps across every workspace.
- **Localized UI** — English, Russian, Ukrainian, German, French, and Italian,
  switchable from the left rail.
- **In-app updates** — a title-bar badge appears when a newer release is
  published, with the changelog one click away.

## Download

### Windows (x64)

| File | Type | Notes |
| --- | --- | --- |
| [`Ascora-ADE-Setup-1.3.1.exe`](https://github.com/AS-CoreAI/Ascora-ADE/releases/download/v1.3.1/Ascora-ADE-Setup-1.3.1.exe) | NSIS installer | Start-menu shortcut, choose install dir, uninstaller |
| [`Ascora-ADE-Portable-1.3.1.exe`](https://github.com/AS-CoreAI/Ascora-ADE/releases/download/v1.3.1/Ascora-ADE-Portable-1.3.1.exe) | Portable | Single `.exe`, no install — just run |

> Windows packages use the same ASCore AI self-signed certificate as 1.3.0.
> This signature does not imply public CA trust or Windows SmartScreen reputation.

### Linux (x64)

| File | Package | For |
| --- | --- | --- |
| [`Ascora-ADE-1.3.1-amd64.deb`](https://github.com/AS-CoreAI/Ascora-ADE/releases/download/v1.3.1/Ascora-ADE-1.3.1-amd64.deb) | `.deb` | Ubuntu / Debian |
| [`Ascora-ADE-1.3.1-x86_64.rpm`](https://github.com/AS-CoreAI/Ascora-ADE/releases/download/v1.3.1/Ascora-ADE-1.3.1-x86_64.rpm) | `.rpm` | Fedora / CentOS / RHEL |
| [`Ascora-ADE-1.3.1-x64.pacman`](https://github.com/AS-CoreAI/Ascora-ADE/releases/download/v1.3.1/Ascora-ADE-1.3.1-x64.pacman) | `.pacman` | Arch |
| [`Ascora-ADE-1.3.1-x86_64.AppImage`](https://github.com/AS-CoreAI/Ascora-ADE/releases/download/v1.3.1/Ascora-ADE-1.3.1-x86_64.AppImage) | AppImage | Portable Linux application |

## Install

**Windows** — run the installer, or just double-click the portable `.exe`.

**Linux**

```bash
# Debian / Ubuntu
sudo apt install ./Ascora-ADE-1.3.1-amd64.deb

# Fedora / CentOS / RHEL
sudo rpm -i Ascora-ADE-1.3.1-x86_64.rpm

# Arch
sudo pacman -U Ascora-ADE-1.3.1-x64.pacman

# AppImage
chmod +x Ascora-ADE-1.3.1-x86_64.AppImage
./Ascora-ADE-1.3.1-x86_64.AppImage
```

## Verify the download

Each platform folder ships a `SHA256SUMS.txt`. Verify integrity before running:

```bash
# Linux / macOS
sha256sum -c SHA256SUMS.txt
```

```powershell
# Windows (PowerShell) — compare against the value in SHA256SUMS.txt
Get-FileHash .\Ascora-ADE-Setup-1.3.1.exe -Algorithm SHA256
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

Version folders retain release notes and platform checksums. For 1.3.1:

```
1.3.1/
├── RELEASE_NOTES.md
├── SHA256SUMS.txt
├── windows/   # Setup, Portable, SHA256SUMS.txt
└── linux/     # DEB, RPM, Pacman, AppImage, SHA256SUMS.txt
```

The six application packages are attached to the corresponding
[GitHub Release](https://github.com/AS-CoreAI/Ascora-ADE/releases/tag/v1.3.1).
They exceed GitHub's 100 MB repository file limit and are not committed to Git.
The combined release checksum file uses the downloadable asset names; each
platform folder also has its own checksum file.

## Build notes

Binaries are produced from the app source with electron-vite + electron-builder
(`npm run dist:win`, `npm run dist:linux`). The app is **Electron + React +
TypeScript** (renderer bundled by Vite); persistence uses `better-sqlite3` with an
atomic JSON-file fallback when native build tools are unavailable. electron-builder
bundles the main process (`out/`) plus its runtime dependencies; the renderer
libraries are bundled by Vite. Both platforms include the OmniRoute gateway,
and Windows includes WireGuard and OpenVPN. Linux packages are built natively
through WSL. Version 1.3.1 was built from original source commit `b7cb95f`.
The source history was subsequently sanitized before public publication,
which changed its commit identifiers. The current public source is available
in [AS-CoreAI/AscoraADE](https://github.com/AS-CoreAI/AscoraADE).

## Source licensing

The original source is published by **ASCoreAI** under the
[ASCoreAI Noncommercial Source License 1.0](https://github.com/AS-CoreAI/AscoraADE/blob/main/LICENSE.md).
For 13 years per version, commercial use, including paid employment,
freelance/client work, and internal business use, requires ASCoreAI's separate
express written permission. Each version then automatically becomes available
under MIT. This is a custom source-available license, not FSL or an OSI-approved
open-source license.

This source-publication change does not retroactively replace licenses already
granted with the 1.3.1 installers or other earlier copies. Third-party components
retain their own licenses. See the
[licensing and publication record](https://github.com/AS-CoreAI/AscoraADE/blob/main/LICENSING.md).

## Links

- Website & live demo — https://ade.ascoreai.com/
- Organization — https://github.com/AS-CoreAI
