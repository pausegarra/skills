# tag

`tag` is a skill for running a guarded release-tag workflow based on semantic versioning.

## What It Does

- Applies when the user wants to bump a repository version and create a release tag.
- Accepts one required bump type: `major`, `minor`, or `patch`.
- Detects the latest semantic version tag, calculates the next version, and updates common version files when present.
- Separates confirmation steps for release changes and for pushing the tag or branch.

## Safety and Release Rules

- Validates the requested bump type before doing release work.
- Checks whether the working tree is clean before continuing.
- Stops for user input if uncommitted changes exist.
- Avoids stashing, resetting, or cleaning unrelated changes.
- Creates an annotated tag and asks before any push.

## Files

- `tag.md`: Full operational instructions for version bumping, tagging, and push flow.
