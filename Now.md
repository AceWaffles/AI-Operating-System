
# Current State

**Date:** 2026-07-01

## Season

**Stabilization** (2026-04-15 → 2027-04-15)

---

## Current Priorities
1. New Job
Goal:
Become highly effective and comfortable in the role.

---

2. AI Operating System
Goal:
Build institutional memory and AI workflows.

---

3. Cardboard Empires

Goal:
Build sustainable foundations.
No rush.

---

4. Support Lulah's Cardboard Media

Goal:

Support technically.

Lulah owns creative direction.

---

5. Inventory Reduction

Goal:
Reduce business leverage over time.

---

6. Mobile Site

Goal:
Get the mobile version of the site up and running this week.

Status: DONE - mobile Collector Mode is live for Binder home, Sets, and My Cards. Mobile is intentionally collector-first, while desktop remains collection-management oriented.

---

7. MCP Tooling

Goal:
Build MCP server infrastructure for AI tool access.

Status: 🟢 DONE — 4 MCP servers registered in `D:\MY AI\opencode.json`:
- **cf-r2** — Cloudflare R2 bucket operations (credentials in `AIOS/.secrets/vault.json`)
- **mssql-reader** — Read-only SQL Server access to CardboardEmpiresDev
- **playwright** — Browser automation via `@playwright/mcp`
- **comfyui** — Local ComfyUI image generation via `D:\MY AI\MCP\comfyui\server.js`

All MCP servers live at `D:\MY AI\MCP\`.

---

## Avoid

* New businesses
* New debt
* New side quests

---

## Core Rule

**Build systems before growth.**

Do not scale projects until repeatable systems exist.

---

# Current Focus (2026-07-01)

Goal:

Keep Cardboard Empires production stable after the mobile Collector Mode launch while preserving deployment, R2 image-library, and AIOS handoff memory.

Active Build:

Production validation and image-library upload readiness. Marvel Masterpieces Platinum local images are fully audited against DB image keys and ready for approved R2 upload.

Success Metric:

**A repeatable deploy/verify/audit workflow where future AIs can deploy safely, confirm public/authenticated production health, and upload audited card images without rediscovering server, vault, or R2 details.**

---

# Current System State

Date: 2026-07-01

## Server Infrastructure

✅ IIS deployed

URL:

http://192.168.4.35

Runtime:

.NET 9 Hosting Bundle installed

## Database Infrastructure

✅ SQL Server Express installed on Cardboard_Emp

Primary database:

CardboardEmpiresDev

AI sandbox database:

CardboardEmpiresWorking

## Connectivity

✅ SQL TCP 1433 enabled

✅ HTTP 80 enabled

## Application Status

✅ Website operational locally:

http://localhost

✅ Website operational over LAN:

http://192.168.4.35

✅ Public production verified:

https://cardboardempires.org/

## AI Integration Status

✅ AI can query SQL read-only through `cardboard_ai` using local `.env`

✅ AI can browse and verify the website through Playwright.

## Server Access

✅ WinRM connected — AI can remote into CARDBOARD_EMP

✅ RDP accessible — port 3389 open

✅ SQL queries work via `cardboard_ai`

Credentials: `D:\MY AI\Repos\AI-Operating-System\.secrets\vault.json`

## Cloudflare Status

✅ Public domain routing is working for `cardboardempires.org`.

✅ Cloudflare R2 credentials are available through `AIOS/.secrets/vault.json` and the `cf-r2` MCP server.

⚠️ Image object upload may lag DB image-key readiness. CDN 404s are expected for sets whose R2 prefix has not been uploaded yet.

## Deployment Status

✅ Production deployment scripts and runbook exist in `CardboardEmpiresDev`.

✅ Latest production deploy verified with public and authenticated Playwright checks.

## Next Milestone

M3 - Production Stability And Image Library

Goals:

Maintain safe deploys, authenticated production verification, and audited R2 image uploads.

---

# Cardboard Empires Objective

Build a repeatable local deployment pipeline between the Development PC and Cardboard_Emp.

## Target Workflow

Development PC

→ Visual Studio Build

→ OpenCode Publish

→ Package Deployment

→ RDP to Cardboard_Emp

→ Execute Deploy Script

→ Run Approved Database Migrations

→ Verify IIS Site Health

## Staging Transport Rule

Version 1 deployment uses RDP to Cardboard_Emp.

Do not assume SMB, admin shares, or direct copy paths such as `\\Cardboard_Emp\c$\inetpub\CardboardEmpires`.

Port 445 was previously found unavailable.

SMB-based deployment may not be used until connectivity is proven and Michael approves the change.

## Active Infrastructure Notes

SQL identity map is stored in `Index/infrastructure.md`.

Current website DB identity:

`CARDBOARD_EMP\svc_cardboard_web`

Current AI DB identity:

`cardboard_ai` read-only.

Current AI SQL connection details are stored locally in `AIOS/.secrets/vault.json` and documented in `Index/infrastructure.md`. Do not print or store the password in AIOS, repositories, appsettings files, scripts, or deployment packages.

Latest deployment/session summary:

`MorningMeetings/2026/2026-07-01-cardboard-mobile-production-image-handoff.md`

Previous product/UI session summary:

`MorningMeetings/2026/2026-06-18-cardboard-empires-ui-session.md`

Latest CardVoyager Chapter 1 summary:

`MorningMeetings/2026/2026-06-25-cardvoyager-chapter1-poc.md`

Latest CardVoyager Phase 2 summary:

`MorningMeetings/2026/2026-06-26-cardvoyager-phase2-exploration-graphics.md`

---

# Environment Definitions

Development

* Michael's Development PC
* Visual Studio environment

Staging

* Cardboard_Emp
* Local IIS Web Server

Production

* Public Cardboard Empires hosting on `CARDBOARD_EMP` / IIS
* **Status:** ACTIVE
* Public URL: `https://cardboardempires.org/`
* Server-local `appsettings.Production.json` contains the real Integrated Security connection string and must not be overwritten by repo placeholders.

---

# Database Rules

AI may:

* Inspect schemas
* Inspect DbContext files
* Inspect migrations
* Inspect SQL scripts
* Generate migrations
* Generate SQL scripts
* Generate deployment packages

AI may NOT:

* Execute database changes without approval

Before any database execution, AI must provide:

1. Tables affected
2. Columns affected
3. Data-loss risk
4. Rollback plan
5. Backup recommendation

---

# Deployment Rules

AI may:

* Publish web projects
* Generate deployment artifacts
* Generate deployment scripts
* Generate IIS configuration steps
* Generate idempotent database scripts
* Validate deployment packages

AI may NOT:

* Auto-deploy to production
* Auto-commit Git changes
* Auto-push Git changes
* Delete files
* Modify secrets or credentials
* Execute deployments without approval

Human approval is required before any deployment execution.

---

# Configuration Rules

Use environment-specific configuration files.

Maintain:

* appsettings.json
* appsettings.Development.json
* appsettings.Staging.json
* appsettings.Production.json

Rules:

* No secrets in repository-tracked files.
* No passwords in appsettings files.
* No API keys in appsettings files.
* No tokens in appsettings files.
* Use Trusted_Connection for local SQL environments.
* Explain all configuration changes before applying them.

---

# Approval Gate

No file modifications may occur without explicit approval from Michael.

Approval must be a direct instruction such as:

- Proceed
- Approved
- Apply changes
- Execute

Questions, discussions, exploration, and suggestions are NOT approval.

Before any execution, AI must:

1. Explain the task.
2. List files to be modified.
3. Explain why each file will change.
4. Wait for approval.

If approval is absent, stop and wait.

## AI Trust Model

AI is allowed to automate repetitive tasks.

AI is not allowed to make business decisions.

AI is not allowed to make irreversible changes.

AI execution follows four phases:

1. Propose
2. Explain
3. Await Approval
4. Execute

Every deployment task must be reversible.

If rollback is not possible, execution is prohibited.

## AI Configuration Rules

AI may inspect local model availability.

AI may propose configuration changes.

AI may NOT modify opencode.json without approval.

Before modifying opencode.json, AI must provide:

1. Current configuration
2. Proposed configuration
3. Exact changes (diff)
4. Benefits
5. Risks

Human approval is required before any configuration change.

## OpenCode Rules

opencode.json is infrastructure configuration only.

Allowed:

- Providers
- Model selection
- Image attachment support
- Shell configuration
- Instruction file references

Not allowed:

- Deployment rules
- Database rules
- Business rules
- Cardboard Empires project logic

Project behavior belongs in:

- AGENTS.md
- Now.md
- MorningMeeting.md
