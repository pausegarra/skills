---
name: imagegen
description: Use OpenAI image generation only when the user explicitly includes #imagegen
---

Only use image generation when the user explicitly includes `#imagegen`.

`#imagegen` is an authorization marker, not necessarily the image prompt itself.

When a user asks for a larger task and includes `#imagegen`:

1. Analyze the task and decide which image assets are needed.

2. Create a short asset plan before generating:
   - asset filename
   - purpose
   - size/aspect ratio
   - prompt summary
   - output directory

3. Generate only the minimum useful number of images.

4. For each asset, create a production-ready structured prompt.

5. Call the `imagegen` tool once per asset.

6. Save generated images in the directory requested by the user:
   - If no directory is specified, use `src/assets/generated/` for frontend projects
   - Otherwise use `.opencode/generated_images/`

---

## 🔧 MANDATORY WIRING (CRITICAL)

After generating images:

1. Immediately integrate them into the codebase:
   - Import or reference them using correct relative paths
   - Replace any placeholder images or temporary URLs

2. Ensure correct usage in UI:
   - The generated assets MUST be used in the actual components/layout
   - Do not leave them unused

3. Verify integration:
   - Files exist at expected paths
   - Imports resolve correctly
   - No broken references

4. Framework-specific usage:
   - For Svelte / SvelteKit:
     - Use `src/assets/generated/`
     - Import via relative path or `$lib` if configured
   - For other frontend frameworks:
     - Follow project conventions for static assets

5. Run the project:
   - Start dev server or build
   - Fix any errors related to paths, imports, or missing files

6. If images are generated but not used in the UI, the task is considered incomplete.

---

## 📦 General Rules

- Do not ask the user to provide exact image prompts unless the task is ambiguous
- Do not generate images without `#imagegen`
- Do not generate excessive assets
- Do not create text, logos, or watermarks unless explicitly requested
- Prefer reusable web assets: hero backgrounds, illustrations, mockups, icons
- For landing pages, generate 2–4 assets maximum
- Use descriptive filename hints:
  - `hero-background`
  - `dashboard-illustration`
  - `feature-visual`
  - `testimonial-illustration`
  - `app-icon`
- Use project-relative paths unless explicitly told otherwise
- Never expose API keys or env variables
- If generation fails, continue gracefully and explain briefly

---

## 🧠 Prompt Structure

Use structured prompts like:

Use case:
Asset type:
Primary request:
Scene/backdrop:
Subject:
Style/medium:
Composition/framing:
Lighting/mood:
Color palette:
Materials/textures:
Constraints:

---

## 🚫 Default Constraints

Always include:

- no watermark
- no brand logos unless explicitly requested
- no readable text unless explicitly requested
