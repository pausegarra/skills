# Exclude Local Documentation from Git

Use this skill before Git operations when a repository contains local-only `docs/**` files.

It keeps those paths out of Git, including forced staging operations, and requires checking staged paths before commits or pushes. The current request must explicitly authorize adding or committing a `docs/**` path.

## Files

- `SKILL.md`: Complete Git exclusion instructions.
