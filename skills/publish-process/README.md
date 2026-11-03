# publish-process

publish-process guides repository publishing from feature-branch commit through pull request approval, squash merge, version bump and tag, then monitors the release pipeline to completion.

## What It Does

- Commits, pushes, and opens the requested pull request, then pauses with its link for the user's OK.
- Checks whether the version bump is already included and follows repository version and tag conventions.
- Squash-merges only after the user's OK, creates and pushes the release tag, and follows relevant pipelines to a terminal result.

## Files

- SKILL.md: Publish workflow, authorization boundary, version and tag handling, and pipeline monitoring.
