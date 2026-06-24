# Cardboard Empires Deployment And Cloudflare Handoff

Date: 2026-06-23

## Theme

Server migration, IIS staging deployment, SQL schema repair, domain bindings, HTTPS, WinRM access, and Cloudflare routing diagnosis.

---

## Current State

- Cardboard Empires IIS site exists on `CARDBOARD_EMP` / `192.168.4.35`.
- IIS site name: `CardboardEmpires`.
- IIS app pool name: `CardboardEmpires`.
- App pool is running as `CARDBOARD_EMP\svc_cardboard_web` again.
- HTTP port `80` is reachable from the development PC.
- HTTPS port `443` is reachable from the development PC.
- WinRM port `5985` is reachable from the development PC.
- Direct HTTP to `http://192.168.4.35/` returns `307` redirect to HTTPS.
- Direct HTTPS to `https://192.168.4.35/` resets because IP-based HTTPS has no SNI hostname.
- Hostname HTTPS to IIS works when forced to the server IP with SNI.

Confirmed working from development PC using host-header/SNI override:

```powershell
curl.exe -k --resolve cardboardempires.org:443:192.168.4.35 https://cardboardempires.org/
```

This returned the full Cardboard Empires home page.

---

## Deployment Package

Latest package created:

`D:\MY AI\Repos\CardboardEmpiresDev\deployment_packages\CardboardEmpires-Staging-20260622-175059.zip`

SHA256:

`A4E8D3A697CF51394D6026733873B561CE8AC1923984207A7EAA4936C641FB9F`

Package included:

- Published app files.
- `web.config`.
- `scripts\Deploy-CardboardEmpires-Staging.cmd`.
- `database\deploy-db.sql`.

The EF-generated SQL included:

- `20260321033900_AddSetImageFolder`.
- `20260622000000_AddItemTypeSortToSetCheckItems`.

Important issue discovered:

- Some schema dependencies were manual SQL files and not present in the EF migration package.
- Binder pages depend on `UserCollectionPreferences` through `CollectionPreferenceService`.
- If that table is missing, login works but Binder pages fail.

Manual SQL repair discussed:

- Create `dbo.UserCollectionPreferences` if missing.
- Confirm `SetProgress.WantedNote` and `SiteEventLogs` as needed.

---

## IIS And App Pool Notes

The `.cmd` launcher works for normal deployment and avoids PowerShell execution policy errors.

Critical bug found:

- Passing `-AppPoolCredential $cred` through the `.cmd` launcher corrupts the credential object.
- IIS app pool username was temporarily set to literal `System.Management.Automation.PSCredential`.
- This caused `Service Unavailable`.

Rule for future scripts:

- Use `.cmd` launcher for normal non-credential deployment.
- Do not pass PowerShell objects such as `PSCredential` through `.cmd`.
- Credential-based app pool setup needs either direct PowerShell execution with `-ExecutionPolicy Bypass` or a separate interactive credential-safe script path.

Service account:

- Local account exists: `svc_cardboard_web`.
- App pool expected identity: `CARDBOARD_EMP\svc_cardboard_web`.
- SQL database user exists for `CARDBOARD_EMP\svc_cardboard_web`.
- Confirm DB roles: `db_datareader`, `db_datawriter` on `CardboardEmpiresDev`.

---

## IIS Domain Bindings

Bindings were added for:

- `cardboardempires.org`
- `www.cardboardempires.org`
- `cardboardempires.me`
- `www.cardboardempires.me`

Protocols:

- HTTP `*:80:<hostname>`.
- HTTPS `*:443:<hostname>` with SNI (`sslFlags = 1`).

Firewall:

- Inbound TCP 443 rule added: `Cardboard Empires HTTPS 443`.

Local IIS binding tests succeeded when using host-header/SNI requests to `192.168.4.35`.

---

## Cloudflare State

Public DNS currently resolves to Cloudflare IPs, not directly to `192.168.4.35`.

Observed public behavior during session:

- `cardboardempires.org` returned Cloudflare `502 Bad Gateway` later in the session.
- `www.cardboardempires.org` returned Cloudflare `502 Bad Gateway` later in the session.
- `.me` domains redirected to `https://cardboardempires.org/`.

Earlier behavior included:

- `cardboardempires.org` Cloudflare `404`.
- `www.cardboardempires.org` Cloudflare `530`.

Interpretation:

- IIS bindings work locally.
- Remaining public failure is Cloudflare routing/origin/tunnel configuration.
- If Cloudflare Tunnel is used, tunnel origin must not target raw IP HTTPS like `https://192.168.4.35` because IIS resets HTTPS without SNI.

Recommended tunnel origin target:

```yaml
service: http://localhost:80
```

or a hostname/SNI-aware HTTPS target if configured correctly.

Potential Cloudflare SSL issue:

- If using self-signed/local IIS cert and Cloudflare connects directly, Cloudflare SSL mode should be `Full`, not `Full (strict)`.
- Long term, install Cloudflare Origin Certificate on IIS and use `Full (strict)`.

---

## Local Server Browsing Issue

Problem:

- Browsing `https://192.168.4.35/` or redirecting from `http://192.168.4.35/` fails because HTTPS bindings are hostname/SNI-based.

Local test fix on server:

Edit as Administrator:

`C:\Windows\System32\drivers\etc\hosts`

Add:

```text
127.0.0.1 cardboardempires.org
127.0.0.1 www.cardboardempires.org
127.0.0.1 cardboardempires.me
127.0.0.1 www.cardboardempires.me
```

Then browse locally:

`https://cardboardempires.org/`

---

## WinRM / Remote Access Status

Server-side WinRM was enabled successfully:

```powershell
Enable-PSRemoting -Force
Set-NetFirewallRule -Name "WINRM-HTTP-In-TCP" -RemoteAddress Any
```

Development PC can reach:

- `192.168.4.35:5985`.

Manual remoting worked from an elevated PowerShell session after setting TrustedHosts:

```powershell
Set-Item WSMan:\localhost\Client\TrustedHosts -Value "192.168.4.35" -Force
$cred = Get-Credential "CARDBOARD_EMP\cardboard_admin"
Enter-PSSession -ComputerName 192.168.4.35 -Credential $cred
```

OpenCode remoting was still blocked because the OpenCode process was not elevated and its WinRM client TrustedHosts was empty.

Attempted from OpenCode:

```powershell
Get-Item WSMan:\localhost\Client\TrustedHosts
```

Result:

- TrustedHosts blank.

Attempted to set from OpenCode:

```powershell
Set-Item WSMan:\localhost\Client\TrustedHosts -Value "192.168.4.35" -Force
```

Result:

- Access denied.

Credential file created by Michael:

`D:\MY AI\cardboard_admin.cred.xml`

Note:

- File is encrypted to Michael's Windows user context.
- Delete after remote diagnostics are complete.

Next session should either:

- Start OpenCode elevated after TrustedHosts is set, or
- Have Michael run remote diagnostic blocks manually in an elevated PowerShell session.

---

## Important Code / Logging Findings

Current app has `SiteEventLogger`, but unhandled page exceptions are not globally logged.

Current behavior:

- `SiteEventLogger` is registered.
- Import/preview workflows call it.
- General Binder page exceptions do not automatically write to `SiteEventLogs`.

Needed app work:

- Add global exception logging middleware in `Program.cs`.
- Improve `/Error` page so it does not show misleading default `Development Mode` guidance in staging.

---

## Known Risks

- Deployment SQL generation from EF does not include all manual schema SQL files.
- `publish_out` and `deployment_package_work` appear tracked/dirty in the repo after publish operations.
- App pool credential passing through `.cmd` is unsafe for `PSCredential`.
- Cloudflare tunnel/origin config remains unresolved.
- Public domains currently depend on Cloudflare path, not direct IIS DNS.
- HTTPS by raw IP fails because bindings are SNI hostname bindings.

---

## Recommended Next Actions

1. Confirm `UserCollectionPreferences`, `SiteEventLogs`, and `SetProgress.WantedNote` exist in `CardboardEmpiresDev`.
2. Confirm Binder pages work after schema repair.
3. Start OpenCode elevated or run remote diagnostics manually with `cardboard_admin`.
4. Inspect Cloudflare Tunnel config and logs on `CARDBOARD_EMP`.
5. Set tunnel service target to `http://localhost:80` unless HTTPS/SNI origin is explicitly configured.
6. Add global exception logging to `SiteEventLogs`.
7. Fix deployment script so credential-based app pool setup does not go through `.cmd` object serialization.
8. Convert manual SQL schema changes into EF migrations or include approved manual SQL patches in deployment package generation.
9. Delete `D:\MY AI\cardboard_admin.cred.xml` after remote diagnostics are complete.

---

## Commands Useful Next Session

Direct IIS host/SNI test from dev PC:

```powershell
curl.exe -k -v --resolve cardboardempires.org:443:192.168.4.35 https://cardboardempires.org/
```

Public Cloudflare test:

```powershell
curl.exe -I --max-time 20 https://cardboardempires.org/
curl.exe -I --max-time 20 https://www.cardboardempires.org/
```

Remote session from elevated dev PowerShell:

```powershell
$cred = Import-Clixml "D:\MY AI\cardboard_admin.cred.xml"
Enter-PSSession -ComputerName 192.168.4.35 -Credential $cred
```

Remote IIS quick check:

```powershell
Import-Module WebAdministration
Get-WebBinding -Name "CardboardEmpires" | Select-Object protocol,bindingInformation,sslFlags
Get-WebAppPoolState "CardboardEmpires"
Get-ItemProperty "IIS:\AppPools\CardboardEmpires" -Name processModel | Select-Object identityType,userName
```
