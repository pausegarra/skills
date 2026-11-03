---
name: publish-process
description: Use when the user asks to publish repository changes through commit, push, pull request, squash merge after their OK, version bump, tag, or release-pipeline monitoring.
---

# Publish Process

Run the repository's publish flow from feature branch through release pipeline, preserving the user's approval boundary.

## Authorization and stop points

- A request to publish authorizes committing the requested changes, pushing the feature branch, and opening a pull request. Do not add generic confirmation stops between these actions.
- After opening the pull request, send its URL and current check status. Stop and wait for the user's explicit OK for that pull request.
- That OK authorizes squash-merging the approved pull request, creating and pushing its release tag, and monitoring the resulting pipeline. Do not ask for the same authorization again.
- Reviewer approval, green checks, a teammate's message, or silence do not replace the user's OK.
- If the user changes the PR, target branch, release version, or scope after approval, clarify before acting on the changed scope.
- When existing commit, merge, or tag skills impose generic confirmation prompts, treat the authorizations above as satisfying those prompts for this publish flow. Keep their repository inspection and safety checks.

## Before opening the pull request

1. Read repository instructions. Inspect `git status --short --branch`, staged and unstaged diffs, current branch, remotes, recent commits and tags, default/principal branch, release configuration, version files, and CI configuration.
2. Commit only files belonging to the requested work. Preserve unrelated changes. Do not stash, reset, clean, amend, force-push, or stage everything indiscriminately. Stop and ask if requested changes cannot be separated safely or secrets are present.
3. Confirm current branch is suitable for a pull request into the repository's principal branch. If current branch is protected or is itself the principal branch, ask before creating or switching to a feature branch.
4. Check whether this release's version bump is already included in the branch:
   - Compare version files and release configuration with the PR base and recent release tags.
   - If the branch already contains the intended bump, keep it and do not bump again.
   - If the repository automates the bump or uses the release tag itself as its only version marker, follow that established convention without adding a second bump.
   - Otherwise, add the missing bump on the feature branch before committing and opening the PR. Use the repository's version tool and update all files its release policy requires.
   - Use the repository's documented bump rule when it determines the next version. If major/minor/patch, exact version, or versioning method remains unclear, ask before committing or opening the PR. Never infer a bump from PR size or commit wording.
5. Determine the PR base and release path from repository configuration and current conventions. Resolve tag format, target, and annotated/signed/lightweight type before merge. If any required release choice is unclear, ask before starting the publish flow.
6. Stage the exact requested and required version-bump files. Create semantic commit(s) matching repository history. Push only the feature branch to its configured remote; never force-push.
7. Open the pull request using the repository's usual title, body, and base branch conventions. Capture its URL and current required-check status.
8. Send the URL and check status promptly, then stop. State that squash merge and tagging await the user's OK for this PR. Do not wait for checks before sending the link.

## After the user's OK

1. Re-open the same pull request and verify it is still open, targets the intended principal branch, and has the expected head SHA. If its head or base changed, clarify before merging.
2. Check required reviews, mergeability, merge-queue rules, and required CI checks. Watch pending checks to a terminal result. If required checks fail, are cancelled, or require an unresolved manual approval, report the PR/run link and stop. Never bypass branch protection, use admin merge, or enable auto-merge. If a required merge queue can perform the repository's squash strategy, enqueue the approved PR and wait until it is actually merged. If its strategy is unclear or incompatible with squash, stop and ask.
3. Squash-merge only the approved PR and inspected head SHA. For GitHub CLI, use the PR-specific command: `gh pr merge <PR> --squash --match-head-commit <HEAD_SHA>`. Do not delete the remote feature branch unless the user requested it.
4. Verify the merge completed. Fetch the PR's base branch and fast-forward its local branch. Do not reset, rebase, stash, clean, or overwrite local work. Stop if the branch cannot be updated safely.
5. Verify the release version and exact tag against the merged commit and repository conventions. Confirm the tag name is unused locally and remotely, create it with the established tag type, then push only that tag. Do not overwrite, move, or force-push an existing tag. Do not add another version bump after merge.

## Follow the pipeline

- Find pipelines triggered by the merged commit or release tag using the repository's CI configuration and supported provider tools. Use the relevant installed skill for providers such as Jenkins when available; use GitHub CLI for GitHub Actions runs.
- Watch every publish or release pipeline relevant to this tag until it reaches a terminal state. For GitHub Actions, `gh run watch <RUN_ID>` follows a run to completion.
- Report success only when relevant runs finish successfully. Include run links and final states.
- If a run fails, is cancelled, waits for manual approval, or no run appears, inspect available status and concise failure logs. Report the state and stop before retrying, changing release state, or claiming success. Ask before an action that could rerun a deployment or publish artifacts twice.

## Completion report

Report commit and PR links, squash-merge commit, principal branch, version bump (or why none was needed), exact tag and remote, and final pipeline links and states.
