# Secret Locations

Purpose:

Store locations of secrets, never the secrets themselves.

## SQL Credentials

Location:

`D:\MY AI\Repos\AI-Operating-System\.secrets\vault.json`

Key:

- `database.server` — 192.168.4.35
- `database.database` — CardboardEmpiresDev
- `database.user` — cardboard_ai
- `database.password` — stored locally only

## CARDBOARD_EMP Server Access

Location:

`D:\MY AI\Repos\AI-Operating-System\.secrets\vault.json`

Variables:

- cardboardEmp.host — 192.168.4.35
- cardboardEmp.computerName — CARDBOARD_EMP
- cardboardEmp.adminUser — Windows admin account
- cardboardEmp.adminPassword — admin password
- cardboardEmp.winrmPort — 5985
- cardboardEmp.rdpPort — 3389
- cardboardEmp.iisSiteName — CardboardEmpires
- cardboardEmp.targetPath — C:\inetpub\CardboardEmpires
- cardboardEmp.backupRoot — C:\inetpub\CardboardEmpires_Backups
- cardboardEmp.deployRoot — C:\Deploy\CardboardEmpires

Connection script:

`D:\MY AI\Scripts\Connect-CardboardEmp.ps1`

## Cloudflare

Location:

`D:\MY AI\Repos\AI-Operating-System\.secrets\vault.json`

Variables:

- cloudflare.accountId — `ea5ea26571d70422a002b6ce66d0df5f`
- cloudflare.r2AccessKeyId — S3-compatible access key
- cloudflare.r2SecretAccessKey — S3-compatible secret key
- cloudflare.r2Token — Legacy API token (not needed for R2)

## MCP Servers

All MCP servers live under `D:\MY AI\MCP\` (separate from any git repo). Registered in `D:\MY AI\opencode.json`.

| Server | Type | Source |
|--------|------|--------|
| `cf-r2` | Local Node.js | `D:\MY AI\MCP\cf-r2\server.js` |
| `mssql-reader` | npx package | `@connorbritain/mssql-mcp-reader@latest` |
| `playwright` | npx package | `@playwright/mcp` |
| `comfyui` | Local Node.js | `D:\MY AI\MCP\comfyui\server.js` |

### cf-r2 — Cloudflare R2 Storage

Source: `D:\MY AI\MCP\cf-r2\server.js`

Credentials from `D:\MY AI\Repos\AI-Operating-System\.secrets\vault.json` → `cloudflare` section (accountId, r2AccessKeyId, r2SecretAccessKey).

| Tool | Purpose |
|------|---------|
| `r2_bucket_info` | Get bucket metadata |
| `r2_list_objects` | List objects (prefix filter) |
| `r2_upload_object` | Upload file to R2 |
| `r2_download_object` | Download object from R2 |
| `r2_delete_object` | Delete object from R2 |

Bucket: `ce-images`.

### mssql-reader — SQL Server Read-Only

Invoked via `npx @connorbritain/mssql-mcp-reader@latest`. Connects to `CardboardEmpiresDev` on `192.168.4.35` using the `cardboard_ai` SQL login (passwords stored in opencode.json `env` block, which is outside any git repo).

| Tool | Purpose |
|------|---------|
| `read_data` | Execute SELECT with auto-limit, blocks writes |
| `list_tables` | List tables, filterable by schema |
| `list_databases` | List databases on the instance |
| `list_environments` | List configured DB environments |
| `describe_table` | Show column names, types, nullability |
| `search_schema` | Wildcard search across tables/columns |
| `profile_table` | Column stats, data distributions, samples |
| `inspect_relationships` | FK relationships for a table |
| `inspect_dependencies` | Objects that depend on a table |
| `explain_query` | Estimated execution plan (no execution) |
| `test_connection` | Test connectivity + latency |
| `validate_environment_config` | Validate JSON config |
| `list_scripts` / `run_script` | Pre-approved SQL scripts |

Guarded by `READONLY: true` — no mutation tools exposed.

### playwright — Browser Automation

Invoked via `npx @playwright/mcp`. Chromium installed at `C:\Users\Micha\AppData\Local\ms-playwright\chromium_headless_shell-1228`.

Flags: `--headless --browser chromium --caps vision --allowed-hosts *`

| Tool | Purpose |
|------|---------|
| `browser_navigate` | Navigate to a URL |
| `browser_snapshot` | Get accessibility tree of the page |
| `browser_take_screenshot` | Take a screenshot |
| `browser_click` | Click an element |
| `browser_type` | Type text into an element |
| `browser_fill_form` | Fill multiple form fields |
| `browser_select_option` | Select dropdown option |
| `browser_hover` | Hover over element |
| `browser_tabs` | Manage browser tabs |
| `browser_evaluate` | Run JS on page |
| *(+16 more for dialogs, keys, mouse, drag/drop, network)* |

### comfyui — Local Image Generation

Source: `D:\MY AI\MCP\comfyui\server.js`

ComfyUI service endpoint: `http://127.0.0.1:8188`

Docker/workspace paths used by ComfyUI:

| Path | Purpose |
|------|---------|
| `/workspace/Lobby/Inbox` | ComfyUI input directory |
| `/workspace/Lobby/Comfy_Out` | ComfyUI output directory |

Windows workspace paths:

| Path | Purpose |
|------|---------|
| `D:\MY AI\Lobby\Inbox` | Input files for ComfyUI |
| `D:\MY AI\Lobby\Comfy_Out` | Generated image output |

Default checkpoint currently exposed by the MCP server:

`juggernautXL_ragnarokBy.safetensors`

| Tool | Purpose |
|------|---------|
| `generate_image` | Generate an image from prompt, negative prompt, width, height, steps, and optional seed |

Local AI note: if the MCP tool is not attached in the active runtime, ComfyUI can still be called directly through the local HTTP API at `http://127.0.0.1:8188` when the service is running.

## Rules

- Never store passwords in repositories.
- Never store passwords in Morning Meetings.
- Never store passwords in deployment ZIPs.
- Never store passwords in appsettings files.
- Never commit `.env` files to source control.
- AI may use secrets only when explicitly approved and only for the approved task.
