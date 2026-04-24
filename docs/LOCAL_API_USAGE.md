# Codex Manager Local API

Codex Manager starts a localhost JSON API inside the Electron main process. It does not start a second tray or helper process.

Default base URL:

```txt
http://127.0.0.1:3219
```

If port `3219` is busy, the app tries the next ports up to `3228`. The active URL is shown in the tray menu as `Local API: ...`.

You can override the default port before starting the app:

```powershell
$env:CODEX_MANAGER_API_PORT = "3220"
```

## Response Shape

Successful responses:

```json
{
  "ok": true,
  "data": {}
}
```

Error responses:

```json
{
  "ok": false,
  "error": "Message"
}
```

## PowerShell Examples

Health check:

```powershell
Invoke-RestMethod "http://127.0.0.1:3219/api/health"
```

Show API help:

```powershell
Invoke-RestMethod "http://127.0.0.1:3219/api/help"
```

List accounts:

```powershell
Invoke-RestMethod "http://127.0.0.1:3219/api/accounts"
```

Get active account:

```powershell
Invoke-RestMethod "http://127.0.0.1:3219/api/active"
```

Get current OAuth or repair attempt:

```powershell
Invoke-RestMethod "http://127.0.0.1:3219/api/oauth/current"
```

Cancel the current OAuth or repair attempt:

```powershell
Invoke-RestMethod "http://127.0.0.1:3219/api/oauth/cancel" -Method POST
```

Check if Codex desktop app is running:

```powershell
Invoke-RestMethod "http://127.0.0.1:3219/api/codex-app"
```

Open Codex desktop app:

```powershell
Invoke-RestMethod "http://127.0.0.1:3219/api/codex-app/open" -Method POST
```

Switch account:

```powershell
$body = @{ action = "accounts.switch"; accountId = "ACCOUNT_ID" } | ConvertTo-Json
Invoke-RestMethod "http://127.0.0.1:3219/api/invoke" -Method POST -ContentType "application/json" -Body $body
```

Repair or re-authenticate one account:

```powershell
$body = @{ action = "accounts.repair"; accountId = "ACCOUNT_ID" } | ConvertTo-Json
Invoke-RestMethod "http://127.0.0.1:3219/api/invoke" -Method POST -ContentType "application/json" -Body $body
```

Cancel an OAuth or repair attempt by invoke action:

```powershell
$body = @{ action = "accounts.cancelOauth"; attemptId = "ATTEMPT_ID" } | ConvertTo-Json
Invoke-RestMethod "http://127.0.0.1:3219/api/invoke" -Method POST -ContentType "application/json" -Body $body
```

Refresh one account:

```powershell
$body = @{ action = "quota.refreshOne"; accountId = "ACCOUNT_ID" } | ConvertTo-Json
Invoke-RestMethod "http://127.0.0.1:3219/api/invoke" -Method POST -ContentType "application/json" -Body $body
```

Warm up one account:

```powershell
$body = @{ action = "quota.warmupOne"; accountId = "ACCOUNT_ID" } | ConvertTo-Json
Invoke-RestMethod "http://127.0.0.1:3219/api/invoke" -Method POST -ContentType "application/json" -Body $body
```

Refresh all accounts:

```powershell
Invoke-RestMethod "http://127.0.0.1:3219/api/refresh-all" -Method POST
```

Warm up all accounts:

```powershell
Invoke-RestMethod "http://127.0.0.1:3219/api/warmup-all" -Method POST
```

## Direct REST Routes

```txt
GET  /api/health
GET  /api/help
GET  /api/state
GET  /api/accounts
GET  /api/accounts/:accountId
GET  /api/active
GET  /api/oauth/current
POST /api/oauth/cancel
GET  /api/logs
GET  /api/codex-app
POST /api/codex-app/open
POST /api/accounts/:accountId/switch
POST /api/accounts/:accountId/repair
POST /api/accounts/:accountId/refresh
POST /api/accounts/:accountId/warmup
POST /api/refresh-all
POST /api/warmup-all
POST /api/invoke
POST /api/invoke/:action
```

## Invoke Actions

```txt
state.get
accounts.list
accounts.switch
accounts.repair
accounts.cancelOauth
quota.refreshOne
quota.refreshAll
quota.warmupOne
quota.warmupAll
logs.list
settings.get
codex.status
codex.open
```

## Notes

- The API binds to localhost only by default.
- Browser calls with an `Origin` header are allowed only from `localhost` or `127.0.0.1`.
- Switching account writes the selected auth snapshot to the live Codex `auth.json`.
- If a refresh token is no longer usable, the app can start a repair OAuth flow for that account instead of repeatedly retrying the dead token.
- If Codex desktop is already running and switch behavior is `Auto reopen Codex if running`, Codex Manager closes Codex, switches auth, then reopens Codex from the executable path. It does not open File Explorer.
