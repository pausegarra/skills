---
name: commit-all
description: Group changes into semantic commits and push
---
Group all current changes into meaningful semantic commits and push the current branch.
Optional context for commit messages: `$ARGUMENTS`
Rules:

- First inspect the full repository state without asking for permission:
  - `git status --short`
  - `git diff --stat`
  - `git diff`
  - `git log --oneline -10`
- Identify related file groups by intent: feature, fix, refactor, tests, docs, chore, release, ci, or config.
- Create multiple commits when there are independent changes. Do not mix unrelated changes in the same commit.
- If `$ARGUMENTS` is not empty, use it as context to adjust commit messages, but do not force that text if it does not accurately describe the changes.
- Use clear, semantic, concise commit messages that follow the repo's recent style.
- Before committing, check for sensitive or suspicious files (`.env`, tokens, credentials, keys, secrets). If any appear, stop and ask.
- Before any commit, detect the current branch name.
- If the branch is protected or trunk-like (`main`, `master`, `develop`, `dev`, `staging`, `release`, `release/*`, or similar), do not commit yet.
- First ask whether to create a new branch for the commit set, and include a proposed semantic branch name in the prompt using a standard prefix such as `feat/`, `fix/`, `refactor/`, `chore/`, `docs/`, `test/`, `ci/`, or `release/`.
- Derive the suggested branch name from the dominant change type and a short slug, for example `feat/split-commit-groups` or `fix/safe-commit-flow`.
- If the user approves, create the branch before generating the commit plan. If the user declines, continue on the current branch.
- Include new, modified, and deleted files that belong to each group.
- Do not revert existing changes.
- Do not use `--no-verify`.
- Do not amend commits.
- Do not force push.

Commit plan:

- Before creating commits, show a concise proposed commit plan

- The plan must include:
  - Commit order.
  - Exact files included in each commit.
  - Final semantic commit message.

Interaction rules:
- If the current branch is protected, use `request_user_input` to ask whether to create the suggested branch before any commit work.
- After showing the commit plan, STOP and ask for confirmation using selectable options.
- Do not create commits until the user explicitly approves the plan.
- Use the native agent prompt tool for every user question and confirmation. In Codex app/CLI, that is `request_user_input`; in OpenCode, IntelliJ, or other agent surfaces, use their equivalent prompt instead of typing free-text questions.
- Do not ask for typed commands like `continue`, `push`, `yes`, or free-text confirmations.
- Preferred confirmation style:
  - ✅ Continue with commits
  - ✏️ Modify grouping
  - ❌ Cancel
- After all commits are created, STOP again before pushing.
- Ask separately for push confirmation using selectable options:
  - 🚀 Push branch
  - ❌ Cancel push
- After push, ask whether to create a PR using selectable options:
  - 📣 Create PR
  - ❌ No PR
- If the user selects `No PR`, do nothing else and stop.
- If the user selects `Create PR`, use this PR body structure unless the user asked for a different format:

  ```md
  ## What changed
  - ...

  ## Impact
  - ...

  ## Validation
  - ...
  ```
- If `request_user_input` is unavailable in current mode, stop and tell user to rerun this skill in a mode that supports selectable prompts; do not continue with text-input fallback.

Flow:
1. Inspect repository state.
2. Check for sensitive files.
3. Detect the current branch.
4. If the branch is protected, ask whether to create a new branch with a suggested semantic name before any commit.
5. Generate and display the commit plan.
6. Pause for approval.
7. If approved, create commits.
8. Pause before `git push`.
9. If approved, run `git push`.
10. Ask whether to create a PR.
11. If approved, create the PR with the standard body structure.
12. If the user declines PR creation, stop immediately.
13. Summarize created commits, pushed branch, and PR if created.
