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
- `skills/<skill-name>/SKILL.md`: Skill definition and execution rules.
- `skills/<skill-name>/README.md`: Human-friendly skill overview.

## Available Skills

- [`imagegen`](skills/imagegen/README.md): Enables controlled image generation only when explicitly authorized with `#imagegen`.
- [`n8n-workflow-cli`](skills/n8n-workflow-cli/README.md): Standardizes safe n8n workflow management through `n8n-cli workflow` commands.

## How to Add a New Skill

1. Create a new folder under `skills/`.
2. Add a `SKILL.md` file with the skill description, behavior, and usage.
3. Add a `README.md` file in English that explains what the skill does.
4. Add the skill to the **Available Skills** list in this README.
5. Keep naming and structure consistent with existing skills.

## Contributing

Contributions are welcome. Please keep skill documentation clear, concise, and testable.
