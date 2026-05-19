# commit-all

`commit-all` is a skill for grouping repository changes into meaningful semantic commits, then pushing the current branch.

## What It Does

- Inspects repository state before proposing any commit.
- Groups changed files by intent such as feature, fix, refactor, docs, tests, chore, or config.
- Produces a concise commit plan with exact files and final commit messages.
- Uses optional user context to refine commit messages when that context matches the actual changes.

## Safety and Commit Rules

- Checks for sensitive files before committing.
- Avoids reverting existing work, amending commits, force-pushing, or bypassing hooks.
- Requires explicit approval before creating commits.
- Requires a separate approval step before pushing the branch.
- Prefers multiple commits when changes are independent.

## Files

- `commit-all.md`: Full operational instructions for repository inspection, commit planning, approval flow, and push.
