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
- **Current version:** 1.2.4
- **Platforms:** Windows (x64), Linux (x64)

## What's new in 1.2.4

- **Reasoning you can read** — providers that expose their thinking (Grok's
  thinking pane, DeepSeek's and Qwen's think stream) now stream it live into a
  collapsible "Thought for 2m" block above the answer, persisted with the task.
- **Grok's long thinking no longer times out** — turns stay alive while the
  provider visibly reasons, instead of failing after 60 seconds with
  "did not render an answer".
- **Invisible web-chat automation** — real-browser chat turns and sign-in
  status checks run in an off-screen browser window; only interactive sign-in
  (including full Google OAuth in a real Chrome/Edge profile) opens visibly.
- **WProvider status at a glance** — the models table now lists every web
  service with an explicit Authorized / Not authorized state.
- **`run_typescript` workspace tool** — a bounded one-shot TypeScript runtime
  (define `main()` and it runs in the project root), available to local, SSH,
  and Blueprint agent runs; Mistral `/work` tool calls are captured and
  executed correctly.
- **Live Preview launch target selector** — open the active HTML file inside
  Ascora ADE or straight in the system browser from a split Go Live control.
- **Restorable project archive** — remove a project from the workspace without
  deleting files or history, and bring it back later with every chat intact.
- **Markdown emphasis in agent replies** — bold, inline code, and fenced code
  blocks render across every backend without trusting model HTML.
- **Quality-of-life fixes** — localized Claude re-authentication with an inline
  sign-in button, OpenRouter selectable during initial setup again, and a
  proper Blueprint deletion confirmation dialog.

See the full changelog in the [release notes](https://github.com/AS-CoreAI/Ascora-ADE/releases/tag/v1.2.4).

## Supported models — one agent, every backend

Switch provider and model per task from the composer; each chat remembers its
own choice.

| Backend | Models | Connection | Notes |
| --- | --- | --- | --- |
| **LM Studio** *(default)* | any local model you load (e.g. `qwen2.5-coder`) | OpenAI-compatible HTTP API (`http://localhost:1234/v1`), SSE streaming | Fully offline — your code never leaves the machine |
| **Ollama** | any local model you pull | OpenAI-compatible HTTP API (`http://localhost:11434/v1`), SSE streaming | Fully offline; probed in the background like LM Studio |
| **Ascora WProvider** | **Qwen · DeepSeek · Alice · Mistral · Claude · Grok · Gemini · ChatGPT** | provider's own web chat via a hidden browser window | Sign in once in a visible window; per-service sign-in status; works over SSH |
| **OpenRouter** | any hosted model on OpenRouter | OpenAI-compatible cloud API with your API key | Optional cloud backend |
| **Claude Code** | Claude **Opus · Sonnet · Haiku · Fable** | local `claude` CLI (auto-detected) | Selectable permission mode (e.g. accept-edits); aliases resolve to the newest model of each family |
| **Codex** | OpenAI **GPT‑5.x**, including the **GPT‑5.6** presets | local `codex` CLI (auto-detected) | Sandbox policy + reasoning-effort control |
| **Gemini CLI** | Google **Gemini** | local `gemini` CLI | Approval modes: plan / default / auto_edit / yolo; session resume |
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
| [`Ascora-ADE-Setup-1.2.4.exe`](https://github.com/AS-CoreAI/Ascora-ADE/releases/download/v1.2.4/Ascora-ADE-Setup-1.2.4.exe) | NSIS installer | Start-menu shortcut, choose install dir, uninstaller |
| [`Ascora-ADE-Portable-1.2.4.exe`](https://github.com/AS-CoreAI/Ascora-ADE/releases/download/v1.2.4/Ascora-ADE-Portable-1.2.4.exe) | Portable | Single `.exe`, no install — just run |

> **Not code-signed.** On first launch Windows SmartScreen shows an "unknown
> publisher" warning — choose **More info → Run anyway**. A signing certificate
> will remove this in a later release.

### Linux (x64)

| File | Package | For |
| --- | --- | --- |
| [`Ascora-ADE-1.2.4-amd64.deb`](https://github.com/AS-CoreAI/Ascora-ADE/releases/download/v1.2.4/Ascora-ADE-1.2.4-amd64.deb) | `.deb` | Ubuntu / Debian |
| [`Ascora-ADE-1.2.4-x86_64.rpm`](https://github.com/AS-CoreAI/Ascora-ADE/releases/download/v1.2.4/Ascora-ADE-1.2.4-x86_64.rpm) | `.rpm` | Fedora / CentOS / RHEL |
| [`Ascora-ADE-1.2.4-x64.pacman`](https://github.com/AS-CoreAI/Ascora-ADE/releases/download/v1.2.4/Ascora-ADE-1.2.4-x64.pacman) | `.pacman` | Arch |

> macOS builds are published on the [website](https://ade.ascoreai.com/).

## Install

**Windows** — run the installer, or just double-click the portable `.exe`.

**Linux**

```bash
# Debian / Ubuntu
sudo apt install ./Ascora-ADE-1.2.4-amd64.deb

# Fedora / CentOS / RHEL
sudo rpm -i Ascora-ADE-1.2.4-x86_64.rpm

# Arch
sudo pacman -U Ascora-ADE-1.2.4-x64.pacman
```

## Verify the download

Each platform folder ships a `SHA256SUMS.txt`. Verify integrity before running:

```bash
# Linux / macOS
sha256sum -c SHA256SUMS.txt
```

```powershell
# Windows (PowerShell) — compare against the value in SHA256SUMS.txt
Get-FileHash .\Ascora-ADE-Setup-1.2.4.exe -Algorithm SHA256
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
1.1/
1.2/
1.2.1/
1.2.2/
1.2.3/
1.2.4/
├── windows/   # SHA256SUMS (the .exe files live on the GitHub Release)
└── linux/     # .deb / .rpm / .pacman packages, SHA256SUMS
```

Each new release adds a version folder (e.g. `1.2.4/`) alongside the previous
ones. Starting with 1.2.4 the Windows executables exceed GitHub's 100 MB
in-repo file limit, so they are attached to the corresponding
[GitHub Release](https://github.com/AS-CoreAI/Ascora-ADE/releases) instead of
being committed to the repository.

## Build notes

Binaries are produced from the app source with electron-vite + electron-builder
(`npm run dist:win`, `npm run dist:linux`). The app is **Electron + React +
TypeScript** (renderer bundled by Vite); persistence uses `better-sqlite3` with an
atomic JSON-file fallback when native build tools are unavailable. electron-builder
bundles the main process (`out/`) plus `ssh2`; the renderer libs (Monaco, React,
etc.) are already bundled by Vite, keeping each Windows binary at ~108 MB.

## Links

- Website & live demo — https://ade.ascoreai.com/
- Organization — https://github.com/AS-CoreAI
