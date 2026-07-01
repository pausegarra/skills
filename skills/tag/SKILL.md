---
name: tag
description: Bump repo version, create git tag, and push
argument-hint: <major|minor|patch>
---

Bump the repository version using semantic versioning based on: `$ARGUMENTS`

Rules:

- Validate `$ARGUMENTS`
  - Allowed values: `major`, `minor`, `patch`
  - If invalid, stop with error

- Check working tree:
  - Run: `git status --short`

- If working tree is NOT clean:
  - Show uncommitted changes
  - Ask user what to do using `request_user_input` selectable options:
    - Continue without touching current uncommitted changes
    - Abort release process
  - Stop until user selects option
  - Do not ask for typed commands like `continue`, `yes`, `push`, or free-text confirmations

- Fetch latest tags:
  - `git fetch --tags`

- Detect current version from latest semantic version tag:
  - Match format: `X.Y.Z`
  - If none exists:
    - use `0.0.0`

- Calculate next version:
  - major -> increment major, reset minor and patch
  - minor -> increment minor, reset patch
  - patch -> increment patch

- Update repository version files when present:
  - `package.json`
  - `package-lock.json`
  - `npm-shrinkwrap.json`
  - `pnpm-lock.yaml`
  - `yarn.lock`
  - `Cargo.toml`
  - `Cargo.lock`
  - `pyproject.toml`
  - `VERSION`
  - `.version`
  - `tauri.conf.json`
  - `src-tauri/tauri.conf.json`
  - `pom.xml`
  - `gradle.properties`
  - `build.gradle`
  - `build.gradle.kts`

- For Node projects:
  - Prefer package manager tooling:
    - npm: `npm version <new-version> --no-git-tag-version`
    - pnpm: `pnpm version <new-version> --no-git-tag-version`
    - yarn: `yarn version --new-version <new-version> --no-git-tag-version`

- For Rust projects:
  - Update all `[package].version` entries in `Cargo.toml`

- For Tauri projects:
  - Update app version in config files

- For Java projects:
  - Maven:
    - Update `<version>` in the root `pom.xml`
    - Update module versions when it is a multi-module project
    - Prefer:
      - `mvn versions:set -DnewVersion=<new-version> -DgenerateBackupPoms=false`
  - Do not update dependency versions unless they are explicitly the current project version

- Show ONLY release-related diff if possible

- Ask confirmation before continuing
  - Use `request_user_input` selectable options:
    - ✅ Continue release
    - ❌ Cancel release
  - Do not continue until user selects an option
  - Do not use text-input fallback

- If working tree was clean:
  - Commit release version bump:
    - `git add .`
    - `git commit -m "chore(release): <new-version>"`

- If working tree was NOT clean and user chose continue:
  - Do NOT stash
  - Do NOT reset
  - Do NOT clean working tree
  - Do NOT modify unrelated files
  - Create tag from current HEAD only

- Create annotated tag:
  - `git tag -a <new-version> -m "<new-version>"`

- Ask confirmation before push
  - Use `request_user_input` selectable options:
    - 🚀 Push tag (and branch if applicable)
    - ❌ Cancel push
  - Do not push until user selects an option
  - Do not use text-input fallback

- If `request_user_input` is unavailable in current mode, stop and tell user to rerun this skill in a mode that supports selectable prompts; do not continue with text-input fallback

- Push tag:
  - `git push origin <new-version>`

- If a release commit was created:
  - Also push current branch:
    - `git push origin HEAD`

- Final output:
  - previous version
  - new version
  - tag created
  - push status
