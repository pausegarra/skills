# Skills Repository

This repository contains reusable skills for agent workflows.

## Installation

Use the full Vercel Skills install command:

```bash
npx skills@latest install https://github.com/pausegarra/skills.git
```

## Repository Structure

- `skills/`: Main folder containing all skills.
- `skills/<skill-name>/`: One directory per skill.
- `skills/<skill-name>/SKILL.md`: Skill definition and execution rules for standard skills.
- `skills/<skill-name>/<skill-name>.md`: Skill definition file used by command-style skills in this repo.
- `skills/<skill-name>/README.md`: Human-friendly skill overview.

## Available Skills

- [`cli-first-tools`](skills/cli-first-tools/README.md): Prefers supported CLI tools for operations across platforms such as GitHub, Azure, and Jira.
- [`commit-all`](skills/commit-all/README.md): Groups current repository changes into semantic commits with an approval step before commit and push.
- [`exclude-docs-from-git`](skills/exclude-docs-from-git/README.md): Keeps `docs/**` paths local and out of Git operations unless the current request explicitly authorizes them.
- [`github-auth-fallback`](skills/github-auth-fallback/README.md): Retries GitHub CLI or Git operations once with another authenticated account after an authentication or permission failure.
- [`git-branches-worktrees`](skills/git-branches-worktrees/README.md): Uses task branches for isolation and avoids worktrees unless explicitly requested.
- [`inherit-subagent-model`](skills/inherit-subagent-model/README.md): Keeps every dispatched subagent on the parent's exact model and reasoning configuration.
- [`imagegen`](skills/imagegen/README.md): Enables controlled image generation only when explicitly authorized with `#imagegen`.
- [`merge-pr-squash-tag`](skills/merge-pr-squash-tag/README.md): Squash-merges a pull request, updates its principal branch locally, and creates and pushes a tag using repository conventions.
- [`n8n-mcp-server`](skills/n8n-mcp-server/README.md): Standardizes safe n8n workflow and execution management through MCP server tools.
- [`n8n-workflow-cli`](skills/n8n-workflow-cli/README.md): Standardizes safe n8n workflow management through `n8n-cli workflow` commands.
- [`service-token-credentials`](skills/service-token-credentials/README.md): Finds service and environment credentials and builds token requests without exposing secrets.
- [`spec-definition`](skills/spec-definition/README.md): Turns user stories into reviewable product specifications by making assumptions explicit.
- [`structuring-hexagonal-projects`](skills/structuring-hexagonal-projects/README.md): Applies a consistent language-independent hexagonal structure across software projects.
- [`tag`](skills/tag/README.md): Runs a guarded semantic-version release flow with version bump, annotated tag, and push confirmations.
- [`xpdf`](skills/xpdf/README.md): Creates, inspects, renders, and visually verifies PDF artifacts.

## How to Add a New Skill

1. Create a new folder under `skills/`.
2. Add the skill definition file (`SKILL.md` or `<skill-name>.md`, matching the skill format you use) with the skill description, behavior, and usage.
3. Add a `README.md` file in English that explains what the skill does.
4. Add the skill to the **Available Skills** list in this README.
5. Keep naming and structure consistent with existing skills.

## Contributing

Contributions are welcome. Please keep skill documentation clear, concise, and testable.
