---
name: exclude-docs-from-git
description: Use when Git operations may include paths under `docs/` that contain local-only Superpowers documentation.
---

# Exclude Local Documentation from Git

Treat every path under `docs/` as local-only Superpowers documentation.

- Never stage, commit, push, or otherwise add any `docs/**` path to Git.
- This includes forced operations such as `git add -f`, `git add --force`, `git update-index --add --force`, and equivalent commands.
- Keep `docs/**` files local and ignored. Do not delete them to satisfy this rule.
- Include a `docs/**` path only when the user explicitly instructs in the current request to add or commit it.
- Before any commit or push, inspect staged paths and remove every `docs/**` entry unless that explicit authorization exists.
