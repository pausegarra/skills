---
name: n8n-mcp
description: Use this skill when the user wants to inspect, create, update, activate, deactivate, delete, or troubleshoot n8n workflows and executions through the n8n MCP server.
---

Use this skill when user wants to work with n8n through MCP tools exposed by this server.

## Prerequisite

MCP client must start server with these environment variables:

- `N8N_BASE_URL`
- `N8N_API_KEY`
- Optional: `N8N_TIMEOUT_MS`

Server does not load `.env` files by itself.

## Available Tools

- `list_workflows`
- `get_workflow`
- `create_workflow`
- `update_workflow`
- `activate_workflow`
- `deactivate_workflow`
- `delete_workflow`
- `list_executions`
- `get_execution`

## When to Use This Skill

Activate this skill when user asks to:

- List or search n8n workflows
- Inspect workflow structure or settings
- Create new workflow
- Update existing workflow fields
- Turn workflow on or off
- Delete workflow
- Inspect workflow runs or failed executions
- Diagnose n8n automation issues using workflow or execution data

## Tool Selection Guide

### Discover workflows

Use `list_workflows` first when user does not provide exact workflow ID.

Guidelines:

- Use `limit: 250` unless user asks for fewer
- Omit `active` unless user explicitly wants only active or only inactive workflows
- Use `name`, `tags`, or `projectId` filters when user gives them

### Inspect workflow details

Use `get_workflow` when user provides workflow ID or after finding candidate workflow from `list_workflows`.

Use `excludePinnedData: true` when pinned data not needed and smaller payload helps.

### Create workflow

Use `create_workflow` with full workflow payload:

- `name`
- `nodes`
- `connections`
- `settings`
- Optional `staticData`
- Optional `shared`

If user asks for new workflow but does not give enough structure, first gather:

- Trigger type
- Main nodes and purpose
- Required credentials or external systems
- Expected success or failure behavior

### Update workflow

Use `update_workflow` for partial changes to existing workflow.

Preferred approach:

1. Fetch current workflow with `get_workflow`
2. Change only fields needed
3. Send minimal safe update with `workflowId` and `workflow`

Avoid rewriting unrelated fields unless user explicitly wants full replacement.

### Activate or deactivate workflow

- Use `activate_workflow` to enable workflow
- Use `deactivate_workflow` to disable workflow

If activation requires metadata already known by tool caller, pass optional:

- `versionId`
- `name`
- `description`

### Delete workflow

Use `delete_workflow` only when user explicitly asks to delete workflow.

Before deletion:

- Confirm exact workflow ID if ambiguity exists
- Prefer showing workflow name first when possible

### Inspect executions

Use `list_executions` to review runs.

Guidelines:

- Use `workflowId` when user targets one workflow
- Use `status` filter for errors or running jobs
- Use `includeData: true` only when payload details matter
- Use `limit: 250` unless user asks for fewer

Use `get_execution` for one execution when deeper debugging needed.

## Response Handling

Each tool returns JSON text with stable shape:

```json
{
  "ok": true,
  "data": {},
  "error": null,
  "meta": {
    "requestId": "uuid",
    "pagination": {
      "nextCursor": null
    }
  }
}
```

Rules:

- Check `ok` first
- If `ok` is `false`, use `error.code`, `error.message`, and `error.details`
- For paginated results, inspect `meta.pagination.nextCursor`
- Surface `requestId` when reporting API or upstream problems

## Suggested Workflows

### User wants workflow by name

1. Call `list_workflows` with `name`
2. If one match, call `get_workflow`
3. If many matches, ask user which one

### User wants failed runs

1. Call `list_executions` with `status: "error"`
2. If workflow known, include `workflowId`
3. Call `get_execution` for execution needing deep inspection

### User wants safe edit

1. Call `get_workflow`
2. Modify only requested fields
3. Call `update_workflow`
4. If requested, verify with `get_workflow`

## Safety Notes

- Do not delete workflows unless user explicitly asks
- Do not activate or deactivate wrong workflow when multiple names look similar
- Prefer IDs over names for final mutations
- Large execution payloads can be noisy; use `includeData` sparingly

## Example User Requests

- "List inactive workflows tagged production"
- "Show workflow 12345"
- "Create a workflow that runs every hour and posts to Slack"
- "Rename workflow 12345 to Invoice Sync"
- "Disable workflow 12345"
- "Show last failed executions for workflow 12345"
