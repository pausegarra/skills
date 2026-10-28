---
name: git-branches-worktrees
description: Use when starting or continuing repository work that needs Git branch isolation or could involve worktrees.
---

# Git Branches and Worktrees

- Do not create or use Git worktrees unless the user explicitly asks for one.
- Work in the current checkout and use branches for isolation.
- Before changing files in a Git repository, inspect the current branch and working-tree status.
- If the current branch is shared or a base branch, such as `main`, `master`, `develop`, `release`, or bare `hotfix`, create and switch to a task branch before editing. Never make task changes directly on a shared branch.
- Name task branches with the matching prefix, such as `feat/`, `fix/`, `refactor/`, `chore/`, `docs/`, `test/`, or `hotfix/`, followed by a short descriptive slug.
- If already on a task branch, keep using it unless the user asks for a new branch.
