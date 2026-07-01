# Cardboard Empires Mobile Production And Image Library Handoff

Date: 2026-07-01

## Theme

Cardboard Empires moved from local/staging stabilization into verified public production with a mobile Collector Mode experience and a reusable image-library audit path for R2 uploads.

---

## Repository Work

Primary repo:

`D:\MY AI\Repos\CardboardEmpiresDev`

Pushed commits already present before this AIOS handoff:

- `94da6f7 Add mobile collector experience and universe preferences`
- `8673cec Add production deployment runbook and scripts`
- `dfb9873 Document production connection config requirement`

Uncommitted at handoff-writing time and intended for a separate commit:

- `docs\image-library-r2-audit.md`
- `tools\image-library\Test-CardImageSet.ps1`

---

## Product Direction Preserved

- Mobile is not responsive desktop. Mobile is collector interaction.
- Desktop remains collection management: dense tables, power-user controls, and larger-screen organization.
- SetInventory and Inventory mobile use 4-card, 2x2 carousel pages.
- Mobile filters should feel like binder navigation: `Find`, `Find a Card`, `Set & Inserts`, and `Browse` language is preferred over SQL-like filter language.
- Card artwork tap flips front/back using a Sets-style 3D flip.
- Card images should use `object-fit: contain` and show the full card art.

---

## Mobile Collector Mode Completed

Implemented and verified areas:

- Mobile bottom nav with active tab handling.
- Binder home mobile compact grid.
- SetInventory mobile Collector Mode with drawer, carousel, AJAX set/insert switching, drawer persistence, visible range updates, and front/back flip.
- Inventory/My Cards mobile Collector Mode using real user inventory rows.
- Inventory drawer filters for universe, brand, set, and subset/insert.
- Inventory mode switching for Owned, Wanted, and Trade.
- Inventory quantity +/- actions wired to existing autosave.
- Inventory bottom card-detail sheet with image, number, name, set, and active quantity.
- Public landing page tightened for mobile and creator section converted to a horizontal carousel.

Build validation used isolated output to avoid Visual Studio/IIS Express file locks:

```powershell
dotnet build "D:\MY AI\Repos\CardboardEmpiresDev\CardboardEmpires.Web\CardboardEmpires.Web.csproj" -p:OutDir="C:\Users\Micha\AppData\Local\Temp\opencode\ce-web-build-check\" -p:UseSharedCompilation=false
```

Result: `0 Warning(s), 0 Error(s)`.

---

## Production Deployment State

Production server:

- Host: `CARDBOARD_EMP`
- IP: `192.168.4.35`
- IIS site/app pool: `CardboardEmpires`
- Target: `C:\inetpub\CardboardEmpires`
- DB: `CardboardEmpiresDev`
- Public URL: `https://cardboardempires.org/`

Deployment work completed:

- EF migration applied: `20260630174500_AddUniversesAndCollectionRole`.
- HTTPS bindings restored for `cardboardempires.org`, `www.cardboardempires.org`, `cardboardempires.me`, and `www.cardboardempires.me`.
- Verified production backup created at `C:\inetpub\CardboardEmpires_Backups\verified-current-20260630-163041`.
- Backup contains `db-CardboardEmpiresDev.bak`, `web.zip`, and `web-files\`.

Critical production config fix:

- Production login initially failed with HTTP 500 because server-local `appsettings.Production.json` had a placeholder connection string.
- The server-local Integrated Security connection string was restored.
- Deployment must not overwrite the server-local production config with the repository placeholder.

---

## Production Verification

Public Playwright verification passed:

- Desktop home returned `200`.
- Mobile home returned `200`.
- Mobile bottom nav visible.
- Creator carousel scrolls horizontally.
- Auth modal/register pane works.
- Public routes did not error.
- Protected routes auth-gated without 500 errors.

Authenticated Playwright verification passed with the user-provided test login:

- Login succeeds.
- `/Binders`, `/Binders/Sets`, and `/Binders/Inventory` load authenticated.
- Inventory Collector Mode is visible.
- Inventory filter drawer opens, stays open while filters change, and closes.
- Inventory card sheet opens and closes.
- Inventory card flips to back.
- No app console/page/request failures after the production config fix.

Do not store the test password in future handoff docs.

---

## Image Library And R2 Audit

Reusable audit files added in `CardboardEmpiresDev`:

- `docs\image-library-r2-audit.md`
- `tools\image-library\Test-CardImageSet.ps1`

Marvel Masterpieces Platinum audit results:

- DB set: `SetId=4`, `2024 Marvel Masterpieces '92 Platinum`
- `Sets.ImageFolder=2024_upper_deck_marvel_masterpieces_'92_platinum`
- Local root: `D:\MY AI\Lobby\Outbox\tcdb\2024_upper_deck_marvel_masterpieces_'92_platinum`
- DB rows: `3270`
- DB front/back references: `6540`
- Unique expected images: `470`
- Local JPGs: `470`
- Missing: `0`
- Extra local JPGs: `0`
- No placeholders required.

Local folder counts verified:

- `base_set`: `200`
- `battle_spectrum`: `10`
- `corner_box_creations`: `40`
- `fantastic_covers`: `20`
- `variant_cover`: `200`

R2 state at audit time:

- Bucket: `ce-images`
- Prefix: `2024_upper_deck_marvel_masterpieces_'92_platinum/`
- Object count: `0`

Audit outputs:

- `C:\Users\Micha\AppData\Local\Temp\opencode\mmp-audit\subset-summary.csv`
- `C:\Users\Micha\AppData\Local\Temp\opencode\mmp-audit\missing.csv`
- `C:\Users\Micha\AppData\Local\Temp\opencode\mmp-audit\extra-local-keys.txt`
- `C:\Users\Micha\AppData\Local\Temp\opencode\mmp-audit\r2-upload-manifest.csv`

Important rule:

- DB image keys may exist before R2 objects. CDN 404s are expected until the matching R2 objects are uploaded.
- Placeholder files must exist at exact expected image-key paths if used. Generic placeholder files do not satisfy the app URL contract.

---

## Known Blockers And Risks

- Normal `bin\Debug` builds can fail when `devenv.exe` or `iisexpress.exe` locks web DLLs; use isolated output for validation.
- `D:\MY AI\Scripts\Connect-CardboardEmp.ps1` was referenced by AIOS but missing during this session.
- `D:\MY AI\cardboard_admin.cred.xml` was referenced by older notes but missing during this session.
- WinRM works by IP `192.168.4.35`; hostname use may require TrustedHosts configuration.
- SQL Server Express does not support `BACKUP DATABASE ... WITH COMPRESSION`.
- The SQL service may not be able to write directly to `C:\inetpub\CardboardEmpires_Backups`; scripts should back up to SQL default backup directory then copy.

---

## Recommended Next Steps

1. Commit and push the CardboardEmpiresDev image-library audit docs/script.
2. Commit and push this AIOS handoff separately.
3. Upload Marvel Masterpieces Platinum images to R2 only after approval, using the generated upload manifest or `cf-r2` MCP.
4. After upload, spot-check CDN front/back URLs under `https://images.cardboardempires.me/2024_upper_deck_marvel_masterpieces_'92_platinum/`.
5. Re-run authenticated Playwright verification after any deployment or image-upload-related UI work.
