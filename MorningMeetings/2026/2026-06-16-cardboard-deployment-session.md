# Cardboard Empires Deployment Session

Date: 2026-06-16

## Theme

Turn OpenCode into a safe staging deployment engineer and preserve the lessons in AIOS.

---

## Major Outcomes

- GPT-5.5 was configured as the OpenCode default model in `D:\MY AI\opencode.json`.
- `AGENTS.md` was updated to distinguish active runtime model, provider, stable fallback operator, local models, and image capability.
- Image input was confirmed working in the current GPT-5.5 session.
- Cardboard Empires was deployed successfully to local IIS staging on `Cardboard_Emp`.
- Staging web URL was confirmed: `http://192.168.4.35/`.
- `192.168.4.1` was identified as the gateway/router, not the server.
- SQL Server Express was installed/configured on `Cardboard_Emp`.
- SQL TCP connectivity to `192.168.4.35:1433` was confirmed.
- HTTP 80 connectivity was confirmed.
- HTTP 500.31 was resolved by installing the .NET 9 Hosting Bundle.
- `CardboardEmpiresDev` database was created and migrated.
- `cardboard_ai` read-only SQL access was confirmed from the development PC.
- The `Invincible Keepsake.csv` import was confirmed in SQL with `7030` `SetCheckItems` rows.
- SQL identity design and AI identity design were captured in AIOS.
- Durable architecture and secret-location documents were added to AIOS.

---

## Deployment Facts

Server:

- Name: `CARDBOARD_EMP`
- IP: `192.168.4.35`
- IIS site: `CardboardEmpires`
- Port: `80`
- Staging URL: `http://192.168.4.35/`
- App runtime: `.NET 9`
- Required server runtime: `.NET 9 ASP.NET Core Hosting Bundle`

Databases:

- `CardboardEmpiresDev`: IIS staging website database
- `CardboardEmpiresWorking`: AI/testing sandbox database

Deployment package baseline:

- V1 transport is RDP-assisted package deployment.
- No SMB/admin-share deployment for V1.
- Deployment ZIP includes app files, deploy script, DB script, and guide.
- Normal launcher is `Deploy-CardboardEmpires-Staging.cmd` to avoid PowerShell execution policy errors.

---

## SQL Identity Decisions

- Website runtime identity: `CARDBOARD_EMP\svc_cardboard_web`
- Website uses Integrated Security / Windows Auth.
- Website must not use SQL username/password.
- AI observer identity: `cardboard_ai`
- AI access is read-only.
- AI must not use `sa`.
- AI must not use `cardboard_admin`.
- Deployment/migration account: `cardboard_deploy`, limited unless Michael approves elevation.
- Do not store SQL passwords in repositories, scripts, ZIPs, or appsettings files.

Connection details are documented in `Index/infrastructure.md`.

Local secret storage for AI DB access:

- File: `D:\MY AI\.env`
- This file is ignored by `D:\MY AI\.gitignore`.
- Do not copy this file into repos or deployment packages.
- Do not print the password in summaries or logs.

---

## Deployment Issues Found And Fixed

- Original deployment script failed on `web.config` because `aspNetCore` is under a `location` element.
- Script was fixed to find `//aspNetCore`.
- SQL migration needed `sqlcmd -C` because ODBC Driver 18 requires trusted cert handling.
- SQL migration needed `sqlcmd -I` for `QUOTED_IDENTIFIER ON`.
- Deployment package now includes `.cmd` launcher with process-level `ExecutionPolicy Bypass`.
- Deployment script restarts the app pool so app pool identity changes are applied.
- `appsettings.Staging.json` now uses `Integrated Security=True`.

---

## Import Issues Found And Fixed

- Import page previously displayed a generic deployed error page.
- Import page now catches exceptions and shows a safe import result error.
- Import UI now displays rows read, inserted, deleted, skipped, and errors.
- `ReplaceExisting` now defaults to `false`.
- Import service blocks importing into an existing set unless replacement is explicitly selected.
- Import service blocks replacement if existing checklist rows are linked to user progress or user card copies.

Data-loss risk discovered:

- `SetProgress` and `UserCardCopy` both reference `SetCheckItemId`.
- Both relationships cascade on delete.
- Deleting and recreating `SetCheckItems` can destroy user collection progress and detailed card copy records.

Long-term fix needed:

- Replace delete/reinsert import behavior with stable upsert behavior preserving `SetCheckItemId`.

---

## Database Issue Found

The importer failed with:

`Invalid column name 'ImageFolder'`

Reason:

- Current code expects `Sets.ImageFolder`.
- The staging database did not initially contain the column.
- `ImageFolder` supports image URL construction with Cloudflare R2 / CDN image paths.

Confirmed direction:

- Images will be stored in Cloudflare R2.
- `FrontImageId` and `BackImageId` are part of the CDN image path.
- `ImageFolder` may remain nullable, but the database column is needed by current EF model/code.

---

## Current Confirmed Data

Read-only AI SQL access using `cardboard_ai` confirmed:

- `SetsCount`: `1`
- Imported set: `2025 Invincible Season 1 Keepsake Premiere Hobby Edition`
- `SetCheckItemsCount`: `7030`
- Example image IDs:
  - `laser_signapatch/600fr`
  - `laser_signapatch/600bk`

---

## Remaining Risks

- `ImageFolder` must be represented in a durable migration/deployment SQL path.
- Current import replacement is guarded, but long-term upsert is still needed.
- A nullable warning remains in `Pages\Binders\SetInventory.cshtml` around line `315`.
- A separate legacy documentation file in `CardboardEmpires.com` appears to contain an old Azure connection string with a password and should be cleaned up in a separate approved task.
- Production remains inactive and not deployable.

---

## Next Session Startup Notes

Load in this order:

1. `D:\MY AI\AGENTS.md`
2. `D:\MY AI\OpenCodeTooling.md`
3. `AI-Operating-System\Now.md`
4. `AI-Operating-System\Index\infrastructure.md`
5. `AI-Operating-System\Index\decisions.md`
6. This session summary

Recommended next work:

1. Add durable EF migration or SQL patch for `Sets.ImageFolder`.
2. Verify import after `ImageFolder` exists.
3. Implement safe upsert import later.
4. Package and deploy after Michael builds in Visual Studio.
5. Build M3 AI Integration: local AI reads SQL, reads website, then moves toward Cloudflare/R2 and one-button deployment.
