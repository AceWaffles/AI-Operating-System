
# Current State

**Date:** 2026-06-26

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

## Avoid

* New businesses
* New debt
* New side quests

---

## Core Rule

**Build systems before growth.**

Do not scale projects until repeatable systems exist.

---

# Current Focus (2026-06-26)

Goal:

Build CardVoyager into a standalone exploration RPG inside Cardboard Empires while keeping deployment safety and AIOS memory intact.

Active Build:

CardVoyager Phase 2 exploration prototype: Cardlandia launch flow, Pullaverse flight, Star Map catalog, and Sol as the first static destination.

Success Metric:

**A repeatable CardVoyager exploration loop where the Captain can launch from Cardlandia, manually navigate the Pullaverse, use the Star Map, discover a static system, enter system exploration, evaluate planets from orbit, land, and return home.**

---

# Current System State

Date: 2026-06-26

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

## AI Integration Status

✅ AI can query SQL read-only through `cardboard_ai` using local `.env`

❌ AI cannot browse the website automatically yet

## Cloudflare Status

❌ DNS not configured

❌ Tunnel not configured

❌ R2 integration not connected

## Deployment Status

✅ ZIP deployment package operational

⚠️ One-button deployment not built yet

## Next Milestone

M3 - AI Integration

Goals:

Local AI

↓

Read SQL

↓

Read Website

↓

Cloudflare

↓

R2 Uploads

↓

One-button Deployments

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

Current AI SQL connection details are stored locally in `D:\MY AI\.env` and documented in `Index/infrastructure.md`. Do not print or store the password in AIOS, repositories, appsettings files, scripts, or deployment packages.

Latest deployment session summary:

`MorningMeetings/2026/2026-06-16-cardboard-deployment-session.md`

Latest product/UI session summary:

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

* Future public Cardboard Empires hosting environment
* **Status:** INACTIVE — not deployable
* `appsettings.Production.json` contains placeholder values only
* A real connection string and hosting infrastructure must be configured before any Production deployment

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
