---
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
- After showing the commit plan, STOP and ask for confirmation using selectable options.
- Do not create commits until the user explicitly approves the plan.
- Use `ask_user` for both confirmations. Do not ask for typed commands like `continue`, `push`, `yes`, or free-text confirmations.
- Preferred confirmation style:
  - ✅ Continue with commits
  - ✏️ Modify grouping
  - ❌ Cancel
- After all commits are created, STOP again before pushing.
- Ask separately for push confirmation using selectable options:
  - 🚀 Push branch
  - ❌ Cancel push
- If `ask_user` is unavailable in current mode, stop and tell user to rerun this skill in a mode that supports selectable prompts; do not continue with text-input fallback.

Flow:
1. Inspect repository state.
2. Check for sensitive files.
3. Generate and display the commit plan.
4. Pause for approval.
5. If approved, create commits.
6. Pause before `git push`.
7. If approved, run `git push`.
8. Summarize created commits and pushed branch.
