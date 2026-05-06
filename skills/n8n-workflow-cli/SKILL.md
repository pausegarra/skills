---
name: n8n-workflow-cli
description: >
  MUST use this skill whenever the user asks to create, update,
  modify, inspect, activate, deactivate, tag, transfer, delete,
  debug, fix, or manage an n8n workflow.

  This includes:
  - "create an n8n workflow"
  - "build an automation"
  - "make a workflow"
  - "update this workflow"
  - "fix my n8n flow"
  - "generate a workflow"
  - "add a node"
  - "connect nodes"
  - "create an n8n automation"

  Do not only generate JSON.
  Prefer using n8n-cli workflow commands whenever possible.
  Never assign, copy, infer, reuse, or set credentials in workflow JSON.
---

# n8n Workflow CLI Skill

Use `n8n-cli workflow` for workflow management.

This skill is for workflow operations only.

## Command Model

`n8n-cli workflow` uses subcommands.

General form:

```bash
n8n-cli workflow COMMAND [ARGUMENTS] [FLAGS]
```

Important CLI patterns:

```bash
n8n-cli workflow list --json
n8n-cli workflow get WORKFLOW_ID --json
n8n-cli workflow create --stdin --json
n8n-cli workflow update WORKFLOW_ID --stdin --json
n8n-cli workflow activate WORKFLOW_ID --json
n8n-cli workflow deactivate WORKFLOW_ID --json
n8n-cli workflow delete WORKFLOW_ID --json
```

Do not invent flags.

Known wrong patterns:

```bash
n8n-cli workflow create --input -
n8n-cli workflow update --id "$WORKFLOW_ID" --input -
n8n-cli workflow get --id "$WORKFLOW_ID"
```

Known correct patterns:

```bash
cat <<'JSON' | n8n-cli workflow create --stdin --json
{
  "name": "Example workflow",
  "nodes": [],
  "connections": {}
}
JSON
```

```bash
cat <<'JSON' | n8n-cli workflow update "$WORKFLOW_ID" --stdin --json
{
  "name": "Updated workflow",
  "nodes": [],
  "connections": {}
}
JSON
```

```bash
n8n-cli workflow get "$WORKFLOW_ID" --json
```

## Mandatory Behavior

If the user asks for an n8n workflow:

1. Use this skill.
2. Prefer interacting through `n8n-cli workflow`.
3. Do not default to raw JSON output.
4. Only output standalone JSON if the user explicitly asks for raw JSON.
5. If connected to a real n8n instance, create or update the workflow directly through the CLI.
6. Do not create local workflow files unless the user explicitly asks.
7. Do not set credentials in workflow JSON.
8. Do not copy credentials from existing workflows.
9. Do not infer which existing credentials should be used.
10. Create workflows inactive by default unless the user explicitly asks to activate them.

## Credential Policy

Never include a `credentials` object in generated workflow JSON.

Forbidden:

```json
{
  "credentials": {
    "telegramApi": {
      "id": "abc123",
      "name": "telegram-personal-bot"
    }
  }
}
```

Forbidden behavior:

- Do not reuse credentials found in `workflow list`.
- Do not reuse credentials found in `workflow get`.
- Do not guess credential names.
- Do not choose credentials based on similar node types.
- Do not attach credentials to nodes automatically.
- Do not print credential IDs or credential names unless the user explicitly asks.
- Do not expose existing workflow credentials in summaries.

Allowed behavior:

- Create nodes without credentials.
- Tell the user that credentials must be selected manually in the n8n UI after creation.
- If the user explicitly provides a credential ID/name and explicitly asks to use it, ask for confirmation before applying it.
- If a workflow requires credentials to run, create it inactive and mention that credentials still need to be attached.

When inspecting existing workflows, redact credentials.

Redact fields named:

```txt
credentials
authentication
apiKey
accessToken
refreshToken
clientSecret
password
secret
token
cookie
authorization
```

## Stdin / In-Memory Rule

Do not create local workflow files unless the user explicitly asks.

Prefer stdin:

```bash
cat <<'JSON' | n8n-cli workflow create --stdin --json
{
  "name": "Example workflow",
  "nodes": [],
  "connections": {}
}
JSON
```

```bash
cat <<'JSON' | n8n-cli workflow update "$WORKFLOW_ID" --stdin --json
{
  "name": "Updated workflow",
  "nodes": [],
  "connections": {}
}
JSON
```

Avoid:

```bash
echo '...' > workflow.json
n8n-cli workflow update "$WORKFLOW_ID" --file workflow.json
```

Avoid:

```bash
n8n-cli workflow get "$WORKFLOW_ID" --json > workflow.json
```

If stdin is not supported for a command, stop and tell the user instead of silently creating temporary files.

## Safety Rules

Before destructive or mutating operations, show the exact command and ask for confirmation.

Mutating operations:

```bash
n8n-cli workflow create --stdin --json
n8n-cli workflow update WORKFLOW_ID --stdin --json
n8n-cli workflow delete WORKFLOW_ID --json
n8n-cli workflow activate WORKFLOW_ID --json
n8n-cli workflow deactivate WORKFLOW_ID --json
n8n-cli workflow transfer ...
n8n-cli workflow tags ...
```

Read-only operations do not need confirmation:

```bash
n8n-cli workflow list --json
n8n-cli workflow get WORKFLOW_ID --json
```

Do not run the same mutating command twice.

After a successful create/update/delete/activate/deactivate command:

1. Capture the returned workflow ID or status from the command output.
2. Run at most one verification command.
3. Do not retry with alternative flags unless the previous command failed.
4. If a command fails because of flags, run `n8n-cli workflow COMMAND --help` before trying again.

## Recommended Workflow

For listing workflows:

```bash
n8n-cli workflow list --json
```

For getting a workflow:

```bash
n8n-cli workflow get "$WORKFLOW_ID" --json
```

For creating a workflow:

1. Build valid workflow JSON in memory.
2. Ensure the JSON contains no `credentials` objects.
3. Show a concise summary of what will be created.
4. Ask for confirmation.
5. Create using stdin.
6. Verify once using the ID returned by the create command.

```bash
cat <<'JSON' | n8n-cli workflow create --stdin --json
{
  "name": "New workflow",
  "nodes": [],
  "connections": {},
  "settings": {
    "executionOrder": "v1"
  },
  "active": false
}
JSON
```

Then verify once:

```bash
n8n-cli workflow get "$CREATED_WORKFLOW_ID" --json
```

For updating a workflow:

1. Get the current workflow.
2. Redact credentials before showing anything.
3. Explain the intended change.
4. Produce the updated JSON in memory.
5. Ensure the updated JSON contains no new credentials.
6. Ask for confirmation.
7. Apply using stdin.
8. Verify once.

```bash
n8n-cli workflow get "$WORKFLOW_ID" --json
```

Then, after confirmation:

```bash
cat <<'JSON' | n8n-cli workflow update "$WORKFLOW_ID" --stdin --json
{
  "name": "Updated workflow",
  "nodes": [],
  "connections": {},
  "settings": {
    "executionOrder": "v1"
  }
}
JSON
```

Then verify once:

```bash
n8n-cli workflow get "$WORKFLOW_ID" --json
```

## Output Handling

Prefer concise summaries over dumping huge JSON.

When JSON is useful, show only the relevant parts unless the user asks for the full workflow.

Use `jq` for read-only inspection when available:

```bash
n8n-cli workflow get "$WORKFLOW_ID" --json | jq '.name, .active, [.nodes[].name]'
```

Do not use `jq` to write files.

## Permission Expectation

Safe read commands may be allowed:

```txt
n8n-cli workflow list
n8n-cli workflow get
```

Mutating commands should require confirmation:

```txt
n8n-cli workflow create
n8n-cli workflow update
n8n-cli workflow delete
n8n-cli workflow activate
n8n-cli workflow deactivate
n8n-cli workflow transfer
n8n-cli workflow tags
```

## Behavior

Be careful. Workflows can trigger production automations.

Prefer:

- inspect first
- no local files
- no credential assignment
- no credential reuse
- no secret leaks
- create inactive by default
- summarize changes
- confirm before mutation
- verify once after mutation
- never run the same create/update command twice
