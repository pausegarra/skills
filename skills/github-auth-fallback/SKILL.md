---
name: github-auth-fallback
description: Use when a GitHub CLI or Git operation fails because the active account lacks permission or authentication.
---

# GitHub Authentication Fallback

When a GitHub CLI or Git operation fails because the active account lacks permission or authentication:

1. Inspect the active accounts with `gh auth status`.
2. If another account is already authenticated, switch to it with `gh auth switch --hostname github.com --user <account>`.
3. Retry the same operation once before reporting failure.

Do not switch accounts for unrelated errors, such as conflicts, validation failures, or network outages.
