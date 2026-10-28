# GitHub Authentication Fallback

Use this skill when a GitHub CLI or Git operation fails because the current account lacks permission or authentication.

It checks existing GitHub CLI sessions, switches to another already authenticated account when available, and retries the failed operation once. It does not switch accounts for unrelated errors.

## Files

- `SKILL.md`: Complete fallback instructions.
