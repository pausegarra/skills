---
name: merge-pr-squash-tag
description: Use when a pull request must be squash-merged, its principal branch fast-forwarded locally, and a release tag created and pushed.
---

# Merge a Pull Request and Publish Its Tag

Use after a user asks to squash-merge a pull request, sync its base locally, and publish a tag.

Resolve PR from `$ARGUMENTS`; without it, use a unique PR for the current branch or ask. Treat an exact tag or bump argument as user input and validate it against repository conventions. Never infer a bump from PR size or commit text.

## Preflight

1. Inspect `git status --short --branch`, remotes, and `gh pr view <PR>`. Stop on any worktree changes; never stash, reset, clean, or overwrite them.
2. Confirm PR is open and mergeable, targets the default branch, has required reviews/checks satisfied (`gh pr checks <PR> --required`), and can merge directly without a queue. Otherwise stop. Never bypass branch protection or enable auto-merge.
3. Record PR head SHA, base, and remote. Confirm local base can fast-forward; stop on divergence.
4. Inspect tag history, remote tags, relevant Makefile recipes, included files, and called scripts. Determine exact target, tag name/type, and side effects; never trust target name alone.
5. Run a Makefile target only if it creates the requested tag without version changes, commits, branch pushes, unrelated tags, or other unrequested effects. If tag format, bump, type, or target is unclear or conflicting, ask before mutation.

Show PR, SHA, base, checks, merge and sync commands, exact tag command, and push effect. Ask with `request_user_input` before merging. If selectable prompts are unavailable, stop.

## Merge and Sync

Merge the inspected PR and head SHA only:

```bash
gh pr merge <PR> --squash --match-head-commit <HEAD_SHA>
```

Do not add `--admin`, `--auto`, or `--delete-branch`. Verify PR merged; if merge fails or queues, stop before tagging.

Fetch the base remote, switch to the local base branch, then fast-forward it:

```bash
git fetch <REMOTE>
git switch <BASE_BRANCH>
git pull --ff-only <REMOTE> <BASE_BRANCH>
```

If local base is absent, create it tracking `<REMOTE>/<BASE_BRANCH>`. If checkout or fast-forward fails, stop; never reset, rebase, stash, or clean. Verify `HEAD` contains squash commit.

## Tag and Push

Recheck tag is unused locally/remotely and points to intended release commit. Show exact command and push effect; ask separately with `request_user_input` before creating and publishing.

- Run an inspected Makefile target once only if it creates the requested tag and has no unrequested side effects.
- If no suitable target exists, use `git tag` with the tag format and annotated/lightweight type established by the repository. Ask if type is unclear.
- Push only the new tag with `git push <REMOTE> <TAG>`, unless the approved Makefile target already pushes that tag.
- Never use `git push --follow-tags`, force-push, overwrite an existing tag, or push the base branch.
- On tag or push failure, preserve existing state; never delete or replace a tag to recover.

Report PR/squash commit, local branch/SHA, tag/target commit, command, and remote verification.
