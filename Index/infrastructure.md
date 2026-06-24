# Infrastructure

## CARDBOARD_EMP SQL Identities

Server: CARDBOARD_EMP  
Internal IP: 192.168.4.35  
Website URL: http://192.168.4.35  
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

## Local AI SQL Connection

Local AI connection settings are stored outside repositories in:

`D:\MY AI\.env`

Rules:

- `.env` is ignored by `D:\MY AI\.gitignore`.
- The active AI SQL user is `cardboard_ai`.
- The active AI database is `CardboardEmpiresDev` unless Michael explicitly selects `CardboardEmpiresWorking`.
- Never print or copy the password into AIOS, repositories, deployment packages, appsettings files, or chat summaries.
- Use this connection for read-only inspection only.

Expected `.env` keys:

```env
DB_SERVER=192.168.4.35
DB_DATABASE=CardboardEmpiresDev
DB_USER=cardboard_ai
DB_PASSWORD=<stored locally only>
DB_TRUST_SERVER_CERTIFICATE=true
```

## Cloudflare R2 Image Path Direction

Images are expected to live in Cloudflare R2 and be served through the Cardboard Empires image CDN.

`SetCheckItems.FrontImageId` and `SetCheckItems.BackImageId` are part of the CDN path.

`Sets.ImageFolder` exists in current application code and should be present in the database, but it may remain nullable as image path handling evolves.
