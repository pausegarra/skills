# merge-pr-squash-tag

`merge-pr-squash-tag` guides a guarded GitHub release flow: squash-merge a pull request, fast-forward its principal branch locally, then create and push a tag using the repository's Makefile and existing tag conventions.

## What It Does

- Resolves a pull request by number or URL and checks its base branch, mergeability, and required checks.
- Inspects existing tags and relevant Makefile targets and scripts before choosing a release command.
- Pauses for confirmation before the squash merge and again before creating and publishing the tag.
- Updates the local principal branch with a fast-forward and pushes only the new tag.

## Files

- `SKILL.md`: Full workflow, preflight checks, confirmation gates, and recovery boundaries.
