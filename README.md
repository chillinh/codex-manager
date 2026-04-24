# Codex Manager

Switch Codex accounts, monitor quota, repair auth sessions, and control Codex Desktop from a Windows desktop app, tray menu, or local API.

> Current release: v1.0.34. Public Windows build first. Full source code will be released later.

![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11-0078D6?style=for-the-badge)
![Built with](https://img.shields.io/badge/built%20with-Electron-2B2E3A?style=for-the-badge&logo=electron&logoColor=9FEAF9)
![Latest release](https://img.shields.io/github/v/release/chillinh/codex-manager?style=for-the-badge)

![Codex Manager Dashboard](docs/screenshots/dashboard.jpg)

## Download

- Windows installer: [Download the latest build](https://github.com/chillinh/codex-manager/releases/latest/download/codex-manager-setup-windows-x64.exe)
- Release page: [GitHub Releases](https://github.com/chillinh/codex-manager/releases)
- Local API guide: [docs/LOCAL_API_USAGE.md](docs/LOCAL_API_USAGE.md)
- v1.0.34 notes: [docs/releases/v1.0.34.md](docs/releases/v1.0.34.md)
- v1.0.32 notes: [docs/releases/v1.0.32.md](docs/releases/v1.0.32.md)

## Why Codex Manager

Codex Manager is built for people who work with multiple Codex accounts and need a faster workflow for switching sessions, watching quota, warming accounts, and keeping Codex Desktop auth state in sync without manually editing files.

## Features

- One-click account switching for saved Codex ChatGPT sessions.
- Quota monitoring dashboard with weekly and 5-hour windows.
- Warmup flow for account sessions.
- Import and export account bundles.
- Tray menu that stays alive when the main window is closed.
- Quick switch, active account actions, Codex Desktop status, and re-auth controls from the tray.
- Detects whether Codex Desktop is running and reopens it after account switch when needed.
- Prevents File Explorer from opening during switch/reopen flows.
- Repairs auth drift between Codex Manager store and live `.codex/auth.json`.
- Handles expired or reused refresh tokens by starting a repair re-auth flow.
- Switches with a valid saved ChatGPT auth snapshot even when Codex app-server reports `requiresOpenaiAuth` too aggressively.
- Can complete a repair flow from an already refreshed saved account snapshot if the OAuth browser callback does not return to the app-server runtime.
- Allows cancelling or restarting an unfinished OAuth/repair flow without restarting the app.
- Clears selected Codex Desktop session state before reopening to reduce loading-screen hangs.
- Shows the estimated 10-day re-auth countdown on account cards and account details.
- Local API for state, switching, refresh, warmup, repair, Codex Desktop status, and OAuth cancellation.

## Local API

Codex Manager starts a localhost API by default.

```txt
http://127.0.0.1:3219
```

Common routes:

```txt
GET  /api/state
GET  /api/accounts
GET  /api/active
GET  /api/oauth/current
POST /api/oauth/cancel
POST /api/accounts/:accountId/switch
POST /api/accounts/:accountId/repair
POST /api/accounts/:accountId/refresh
POST /api/accounts/:accountId/warmup
```

See [docs/LOCAL_API_USAGE.md](docs/LOCAL_API_USAGE.md) for PowerShell examples.

## Screenshots

### Dashboard

![Dashboard](docs/screenshots/dashboard.jpg)

### Accounts

![Accounts](docs/screenshots/account.jpg)

### Import / Export

![Import Export](docs/screenshots/import-export.jpg)

### Activity Logs

![Activity Logs](docs/screenshots/log.jpg)

### Settings

![Settings](docs/screenshots/settings.jpg)

## What This Repository Contains

This repository currently focuses on distribution:

- Windows installer builds through GitHub Releases
- Product screenshots
- Public release notes
- Local API usage documentation

Full source code is planned for a later public release.

## Source Code Status

The current repository is being used as the public distribution page for installer releases, screenshots, release notes, and docs. The application source code is not fully published yet, but it is planned for a future update.

## Stable Download Link

Use this URL when a website button should always fetch the newest installer:

```txt
https://github.com/chillinh/codex-manager/releases/latest/download/codex-manager-setup-windows-x64.exe
```

As long as each release uses the same asset name, the link downloads the newest version automatically.

## Integrity

Each release includes `SHA256SUMS.txt` so users can verify the downloaded installer.

## Requirements

- Windows 10 or 11.
- Codex CLI installed and authenticated at least once for Codex workflows.

## Author

- Linh Bui
- GitHub: [@chillinh](https://github.com/chillinh)
- Facebook: [chillinh.page](https://facebook.com/chillinh.page)
- Email: chillinh.page@gmail.com
- Website: `chilling.page` (coming soon)

## Notes

- If Windows SmartScreen appears, users can verify the file hash from the release page before installing.
- If a refresh token is already reused, it cannot be silently recovered. Re-authenticate that account once in Codex Manager.
- If Windows shows an old taskbar icon after installing, unpin the old shortcut and open/pin the new app from the Start Menu because Windows can cache pinned shortcut icons.
