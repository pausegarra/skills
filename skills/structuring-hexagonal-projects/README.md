# Structuring Hexagonal Projects

`structuring-hexagonal-projects` keeps project code organized in the same language-independent hexagonal layout across software projects.

## What It Does

- Groups shared capabilities under `common/` and business areas under `context/<bounded_context>/`.
- Uses consistent `domain/`, `application/`, and `infrastructure/` layers and responsibility-based subdirectories.
- Separates domain entities and repository ports from infrastructure models and concrete adapters.
- Maps the logical layout to each language's existing source and test roots.
- Leaves existing files in place; applies the structure to new work without triggering repository-wide migrations.

## When to Use

Use when implementing features, creating modules, reviewing file placement, or deciding where entities, services, use cases, models, repositories, API adapters, and tests belong in any software project.

## Files

- `SKILL.md`: Full architecture rules, directory layout, and application guidance.
