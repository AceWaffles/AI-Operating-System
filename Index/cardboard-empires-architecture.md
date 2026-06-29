# Cardboard Empires Architecture Snapshot

## Current Architecture

Developer

↓

Visual Studio

↓

Build

↓

Deploy ZIP

↓

IIS Website

↓

CardboardEmpiresDev

↓

CardboardEmpiresWorking

↓

Local AI

Notes:

- Current deployment is RDP-assisted ZIP deployment.
- `CardboardEmpiresDev` is the IIS staging website database.
- `CardboardEmpiresWorking` is the AI/testing sandbox database.
- Local AI uses `cardboard_ai` read-only credentials stored outside repositories.
- MCP servers at `D:\MY AI\MCP\` provide tool access: `cf-r2` (R2 storage), `mssql-reader` (DB queries), `playwright` (browser).

## Future Architecture

Developer

↓

AIOS

↓

Local AI

↓

Cardboard Empires Website

↓

Cloudflare

↓

R2 Storage

↓

Public Internet

Notes:

- Cloudflare DNS is not configured yet.
- Cloudflare Tunnel is not configured yet.
- R2 integration is not connected yet.
- One-button deployment is not built yet.
