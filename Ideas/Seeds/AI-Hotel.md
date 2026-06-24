# AI Hotel

Status:

Seed

Purpose:

Docker-based environment for hosting and testing different AI models, agents, and MCP servers without losing control of tools, permissions, or context.

Concept:

AIOS acts as the front desk and operating manual.

Each model or agent gets a defined room with known capabilities, tools, permissions, and startup tests.

Potential Rooms:

- Enggy / OpenCode / GPT-5.5 deployment engineer
- Number1 / CloudGPT strategist and proofreader
- Big Pickle stable fallback operator
- Gemma lab
- Qwen lab
- Future local model experiments

Potential MCP Wing:

- SQL read-only MCP
- Filesystem MCP
- Browser MCP
- GitHub MCP
- Cloudflare MCP
- R2 image storage MCP

Operating Rules:

- Every AI resident must report active model and provider.
- Installed model does not mean active model.
- Provider installed or running does not mean active provider.
- Tool access must be explicit.
- Database writes require Michael approval.
- Deployment execution requires Michael approval.
- Secrets must stay outside repositories.

Why It Matters:

Allows experimentation without turning every model test into a side quest.

Preserves ideas while keeping current work focused.

Priority:

Later phase.

Do not activate until AIOS and Cardboard Empires staging deployment are stable and repeatable.
