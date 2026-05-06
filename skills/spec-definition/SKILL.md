---
name: spec-definition
description: >
  Use when the user wants to define, draft, complete, or refine a product
  specification from a user story. The skill fills gaps in the story, lists
  assumed non-technical and functional decisions, then guides the user through
  rejected assumptions one by one with progress, four alternative assumptions,
  and a fifth "otra" option.
---

# User Story Spec Skill

Use this skill when the user gives a user story and asks to create, define,
complete, or refine a spec.

The goal is to transform a user story into a ready-to-write specification while making every non-technical or functional assumption explicit and reviewable.

## Core Flow

1. Receive the user story.
2. Draft a preliminary specification by filling missing product, business, UX, and functional details.
3. List every assumption made that is not purely technical.
4. Ask the user which numbered assumptions they dislike.
5. For each rejected assumption, ask exactly one question at a time.
6. In every question, show progress as `Pregunta X/Y`.
7. In every question, provide four concrete replacement assumptions plus a fifth option named `Otra`.
8. If the user selects `Otra`, ask them to specify their preferred definition.
9. Update the working definition after each answer.
10. When all rejected assumptions are resolved, say: `Ya me encuentro listo para crear la especificacion.`

## Initial Response Format

When the user provides a story, answer in Spanish and use this structure:

```markdown
**Spec Preliminar**

<filled specification>

**Asunciones**

1. <assumption>
2. <assumption>
3. <assumption>

Indica los numeros de las asunciones que no te gustaron. Si todas estan bien, dime `ninguna`.
```

Keep the preliminary spec concise but complete enough to expose the assumptions.

## Specification Contents

Include the most relevant sections for the story. Prefer these sections unless the context suggests otherwise:

- Objetivo
- Usuario principal
- Problema
- Alcance
- Flujo principal
- Reglas funcionales
- Estados y errores
- Criterios de aceptacion
- Fuera de alcance

Do not over-engineer. If a section is not relevant, omit it.

## Assumption Rules

List only assumptions that are product, business, UX, content, workflow, permission, validation, data retention, notification, or functional behavior decisions.

Include assumptions such as:

- Who the target user is.
- What triggers the feature.
- What the happy path is.
- What validation rules apply.
- What happens in empty, error, or edge states.
- What data is shown, hidden, stored, or sent.
- What permissions or roles can perform actions.
- What notifications or confirmations exist.
- What is intentionally out of scope.

Do not list purely technical assumptions such as:

- Frameworks, libraries, APIs, databases, queues, caches, hosting, or architecture.
- Internal implementation details.
- File names, components, routes, schemas, or code structure.

If a technical decision affects user-visible behavior, express it as the user-visible behavior only.

## Rejected Assumption Handling

When the user provides assumption numbers they dislike:

1. Parse the numbers.
2. Preserve their order.
3. Ask about the first unresolved rejected assumption only.
4. Do not ask multiple questions in a single response.

Question format:

```markdown
**Pregunta X/Y**

Sobre la asuncion <N>: "<original assumption>"

Que definicion prefieres?

1. <replacement assumption A>
2. <replacement assumption B>
3. <replacement assumption C>
4. <replacement assumption D>
5. Otra
```

The four alternatives must be meaningful and mutually different. Do not include weak options like "mantener igual" unless that is a realistic answer and the user explicitly appears uncertain.

## Progress Bar

Every rejected-assumption question must include a visual progress bar immediately after the progress label.

Use this format:

```markdown
**Pregunta X/Y**
`[##---] X/Y`
```

Use five segments total. Fill segments proportionally to completed/current progress. Examples:

- `[#----] 1/5`
- `[##---] 2/5`
- `[###--] 3/5`
- `[####-] 4/5`
- `[#####] 5/5`

## User Answers

If the user answers with option 1-4:

- Replace the rejected assumption with that option.
- Confirm briefly.
- Move to the next rejected assumption, if any.

If the user answers `5`, `otra`, or custom text indicating another answer:

- Ask them to specify the desired definition for that assumption.
- Keep the same progress count; do not advance until they provide the definition.

If the user provides a custom definition:

- Use their definition exactly in meaning, cleaning wording only for clarity.
- Confirm briefly.
- Move to the next rejected assumption, if any.

If the user says `ninguna` or equivalent after seeing the assumptions:

- Say: `Ya me encuentro listo para crear la especificacion.`

## Completion

After all rejected assumptions are resolved, do not create the final spec unless the user asks for it. End with exactly:

```text
Ya me encuentro listo para crear la especificacion.
```

If the user then asks to create the specification, produce the final spec using the updated assumptions.

## Language And Tone

Use Spanish by default.

Be direct, structured, and concise.

Avoid technical implementation details unless the user explicitly asks for them.

Use ASCII punctuation in files and generated examples unless the user's requested output needs Spanish punctuation.
