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

- [`commit-all`](skills/commit-all/README.md): Groups current repository changes into semantic commits with an approval step before commit and push.
- [`imagegen`](skills/imagegen/README.md): Enables controlled image generation only when explicitly authorized with `#imagegen`.
- [`n8n-mcp-server`](skills/n8n-mcp-server/README.md): Standardizes safe n8n workflow and execution management through MCP server tools.
- [`n8n-workflow-cli`](skills/n8n-workflow-cli/README.md): Standardizes safe n8n workflow management through `n8n-cli workflow` commands.
- [`spec-definition`](skills/spec-definition/README.md): Turns user stories into reviewable product specifications by making assumptions explicit.
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
