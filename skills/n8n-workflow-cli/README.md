# n8n-workflow-cli

`n8n-workflow-cli` is a skill for managing n8n workflows through CLI-first operations.

## What It Does

- Applies whenever users ask to create, update, inspect, activate, deactivate, tag, transfer, debug, fix, or delete n8n workflows.
- Prioritizes real `n8n-cli workflow` commands over raw JSON-only responses.
- Standardizes command patterns (`list`, `get`, `create --stdin`, `update --stdin`, `activate`, `deactivate`, `delete`).
- Keeps workflows inactive by default unless activation is explicitly requested.

## Safety and Credential Policy

- Never auto-assigns or infers credentials.
- Never reuses credentials from existing workflows.
- Redacts sensitive fields when inspecting workflow data.
- Requires explicit confirmation before mutating or destructive operations.

## Files

- `SKILL.md`: Full operational instructions, command patterns, and safety rules.
