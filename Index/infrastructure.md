# Infrastructure

## CARDBOARD_EMP SQL Identities

Server: CARDBOARD_EMP  
Internal IP: 192.168.4.35  
Website URL: http://192.168.4.35  
Public production URL: https://cardboardempires.org/
Primary website database: CardboardEmpiresDev  
AI/testing sandbox database: CardboardEmpiresWorking

Staging website URL: `http://192.168.4.35/`  
Gateway/router: `192.168.4.1`

| Identity | Type | Purpose | Rights |
|---|---|---|---|
| sa | SQL Auth | Emergency break-glass admin only | sysadmin |
| cardboard_admin | SQL Auth | Michael DBA/admin account | elevated admin |
| CARDBOARD_EMP\cardboard_admin | Windows Auth | Windows/server admin | admin/elevated |
| CARDBOARD_EMP\svc_cardboard_web | Windows Auth | IIS application pool identity for Cardboard Empires | db_datareader, db_datawriter |
| cardboard_ai | SQL Auth | AI observer account | db_datareader only |
| cardboard_deploy | SQL Auth | Future deployment/migration account | currently limited; elevated only by approval |

Rules:

- Website uses `CARDBOARD_EMP\svc_cardboard_web`.
- Website uses Windows Authentication / Integrated Security.
- Website must not use SQL username/password.
- AI uses `cardboard_ai`.
- AI access is read-only.
- AI must never use `sa`.
- AI must never use `cardboard_admin`.
- Database modifications require Michael approval.
- Deployment/migration rights for `cardboard_deploy` require approval before elevation.
- Do not store SQL passwords in repositories, scripts, ZIPs, or appsettings files.
- Production `appsettings.Production.json` on the server must keep the server-local Integrated Security connection string and must not be overwritten by the repository placeholder.

## Local AI SQL Connection

Local AI connection settings are stored outside repositories in:

`D:\MY AI\Repos\AI-Operating-System\.secrets\vault.json`

Rules:

- `vault.json` is ignored by `D:\MY AI\.gitignore`.
- The active AI SQL user is `cardboard_ai`.
- The active AI database is `CardboardEmpiresDev` unless Michael explicitly selects `CardboardEmpiresWorking`.
- Never print or copy the password into AIOS, repositories, deployment packages, appsettings files, or chat summaries.
- Use this connection for read-only inspection only.

Expected `vault.json` keys:

```json
{
  "database": { "server": "192.168.4.35", "database": "CardboardEmpiresDev", "user": "cardboard_ai" },
  "cardboardEmp": { "host": "192.168.4.35" },
  "cloudflare": { "accountId": "..." }
}
```

## CARDBOARD_EMP Server Access

AI can connect to CARDBOARD_EMP via WinRM using credentials from `D:\MY AI\Repos\AI-Operating-System\.secrets\vault.json`.

```
CARDBOARD_EMP\cardboard_admin  /  password in .env.json
Host: 192.168.4.35
WinRM: port 5985
RDP: port 3389
```

Connection script: `D:\MY AI\Scripts\Connect-CardboardEmp.ps1`
- First run prompts for creds and saves encrypted XML
- Supports `-RDP` flag for Remote Desktop
- `vault.json` is gitignored and excluded from repos

Known access note from 2026-07-01:

- WinRM works by IP `192.168.4.35`.
- The host name `CARDBOARD_EMP` may need TrustedHosts configuration if used directly.
- `D:\MY AI\Scripts\Connect-CardboardEmp.ps1` and `D:\MY AI\cardboard_admin.cred.xml` were referenced by older notes but were not present during the 2026-07-01 session.

## Cardboard Empires Production Deployment

IIS site/app pool:

`CardboardEmpires`

Target path:

`C:\inetpub\CardboardEmpires`

Verified production backup from 2026-06-30:

`C:\inetpub\CardboardEmpires_Backups\verified-current-20260630-163041`

Backup contents:

- `db-CardboardEmpiresDev.bak`
- `web.zip`
- `web-files\`

Deployment docs/scripts in `D:\MY AI\Repos\CardboardEmpiresDev`:

- `docs\production-deployment.md`
- `tools\deployment\New-CardboardDeploymentArtifacts.ps1`
- `tools\deployment\Invoke-CardboardProductionDeploy.ps1`

Operational notes:

- SQL Server Express does not support `BACKUP DATABASE ... WITH COMPRESSION`.
- SQL service may not be able to write directly to `C:\inetpub\CardboardEmpires_Backups`; deployment script backs up to SQL's default backup directory and then copies.
- Normal local builds can fail if `devenv.exe` or `iisexpress.exe` locks `bin\Debug`; use isolated output for validation.

## Cloudflare R2 Image Path Direction

Images live in Cloudflare R2 bucket `ce-images`, served through the CDN at `https://images.cardboardempires.me`.

`SetCheckItems.FrontImageId` and `SetCheckItems.BackImageId` are part of the CDN path.

`Sets.ImageFolder` exists in current application code and should be present in the database, but it may remain nullable as image path handling evolves.

### AI Access (MCP Servers)

Registered in `D:\MY AI\opencode.json`. All servers live at `D:\MY AI\MCP\`.

| Server | Purpose | Source |
|--------|---------|--------|
| `cf-r2` | Cloudflare R2 object storage | `D:\MY AI\MCP\cf-r2\server.js` |
| `mssql-reader` | Read-only SQL Server access to CardboardEmpiresDev | `npx @connorbritain/mssql-mcp-reader` |
| `playwright` | Browser automation for web testing | `npx @playwright/mcp` |
| `comfyui` | Local image generation through ComfyUI/JuggernautXL | `D:\MY AI\MCP\comfyui\server.js` |

See `Index/connection-locations.md` for full tool lists per server.

### Image Audit Workflow

Reusable repo instructions and script:

- `D:\MY AI\Repos\CardboardEmpiresDev\docs\image-library-r2-audit.md`
- `D:\MY AI\Repos\CardboardEmpiresDev\tools\image-library\Test-CardImageSet.ps1`

2026-07-01 Marvel Masterpieces Platinum audit:

- DB set: `SetId=4`, `2024 Marvel Masterpieces '92 Platinum`
- `Sets.ImageFolder=2024_upper_deck_marvel_masterpieces_'92_platinum`
- Local root: `D:\MY AI\Lobby\Outbox\tcdb\2024_upper_deck_marvel_masterpieces_'92_platinum`
- DB rows: `3270`
- DB front/back references: `6540`
- Unique expected images: `470`
- Local JPGs: `470`
- Missing: `0`
- Extra local JPGs: `0`
- R2 prefix was empty at audit time: `2024_upper_deck_marvel_masterpieces_'92_platinum/`

Audit outputs were generated under:

`C:\Users\Micha\AppData\Local\Temp\opencode\mmp-audit\`

Use `cf-r2` MCP for upload only after audit approval. DB image keys can exist before R2 objects; CDN 404s are expected until upload completes.
