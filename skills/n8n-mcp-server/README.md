# n8n-mcp-server

`n8n-mcp-server` is a skill for managing and debugging n8n workflows through MCP server tools.

## What It Does

- Applies when users ask to list, inspect, create, update, activate, deactivate, delete, or troubleshoot n8n workflows and executions.
- Uses MCP workflow and execution tools instead of shell-based n8n management.
- Encourages safe workflow discovery first, then targeted inspection or mutation by workflow ID.
- Supports execution-level debugging for failed, running, or recent workflow runs.

## Safety and Usage Rules

- Requires `N8N_BASE_URL` and `N8N_API_KEY`; `N8N_TIMEOUT_MS` is optional.
- Does not load `.env` files automatically; MCP client must provide environment variables.
- Prefers `list_workflows` before mutations when exact workflow ID is not known.
- Recommends minimal updates instead of rewriting unrelated workflow fields.
- Avoids destructive actions or wrong activation/deactivation when workflow names are ambiguous.
- Uses execution payload data sparingly because responses can become large and noisy.

## Files

- `SKILL.md`: Full operational instructions, tool-selection rules, response handling, and safety notes.
