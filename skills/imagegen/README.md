# imagegen

`imagegen` is a skill that enables image generation workflows in a controlled and explicit way.

## What It Does

- Allows image generation only when the user explicitly includes `#imagegen`.
- Helps plan assets before generation (filename, purpose, size, prompt summary, output path).
- Uses one generation call per asset to keep output intentional and minimal.
- Enforces integration: generated images must be wired into the codebase and used in the UI.

## Safety and Quality Rules

- Prevents accidental image generation when authorization is missing.
- Avoids excessive asset creation.
- Applies default constraints such as no watermark and no unintended logos/text.
- Encourages clean, reusable assets and graceful fallback behavior on failures.

## Files

- `SKILL.md`: Full operational instructions and mandatory rules.
