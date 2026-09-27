---
name: structuring-hexagonal-projects
description: Use when working on any software project involving source code, including new projects, features, bug fixes, refactors, or reviews, where layers, modules, directories, or file placement matter.
---

# Structuring Hexagonal Projects

Use this language- and framework-independent layout as the default for all software-project source work. Language conventions determine the source and test roots; they do not change the logical layers or their responsibilities.

## Preferred project shape

```text
<source-root>/
  common/
    domain/
      criteria/
      entities/
      enums/
      exceptions/
      repositories/       # domain ports
      services/
      value_objects/
    application/
      dto/
      interfaces/
      services/
      use_cases/
        <use_case>/
    infrastructure/
      audit/
      config/
      cron/
      exception_mappers/
      models/
      presentations/
      repositories/       # concrete adapters
      requests/
      rest/
      rest_clients/
      security/
      spec/
  context/
    <bounded_context>/
      domain/
        criteria/
        entities/
        enums/
        exceptions/
        repositories/
        services/
        value_objects/
      application/
        dto/
        interfaces/
        services/
        use_cases/
          <use_case>/
      infrastructure/
        audit/
        config/
        cron/
        exception_mappers/
        models/
        presentations/
        repositories/
        requests/
        rest/
        rest_clients/
        security/
        spec/
```

Use `common/` only for capabilities shared by multiple contexts. Put each business capability under `context/<bounded_context>/`. For a project with one business context, use one context named for that product area. Keep the same layer names, category names, and responsibilities across languages and contexts. Create a category directory when it has content; do not add empty placeholders. Add a responsibility-specific category only when the project needs it, then use that name consistently.

## Layer boundaries

- `domain/` owns business rules and framework-independent entities, value objects, criteria, enums, exceptions, domain services, and repository ports.
- `application/` owns use cases, application services, orchestration, policies, and application DTOs. Group each use case under `use_cases/<use_case>/` with its related inputs and outputs.
- `infrastructure/` owns concrete integrations and framework-bound code: persistence models, repository implementations, HTTP resources and clients, request/response representations, mappings, configuration, security, and scheduled jobs.
- Keep dependencies pointing inward: domain stays independent; application depends on domain; infrastructure depends on the inner layers and implements their ports. Entry adapters call application use cases.

Keep `domain/entities/` separate from `infrastructure/models/`. Keep repository interfaces in `domain/repositories/` and their implementations in `infrastructure/repositories/`. Put HTTP entry points in `infrastructure/rest/`, outgoing HTTP integrations in `infrastructure/rest_clients/`, and application orchestration in `application/services/` or `application/use_cases/` according to its role.

## Applying the layout

1. Inspect the repository's source root, modules, and nearby feature before adding code. Map its language-specific source and test roots to `<source-root>` and `<test-root>`; do not impose another language's physical layout.
2. In a monorepo, apply the shape under each independently buildable application or service source root.
3. For new features and modules, use the preferred shape above. Keep files grouped by responsibility instead of placing them directly in a layer root.
4. Mirror the logical grouping in tests under `<test-root>/common/` and `<test-root>/context/<bounded_context>/`. Keep integration and architecture checks in distinct test categories when the repository supports them.
5. In existing repositories, leave existing files and folders in place. Do not migrate, rename, or reorganize code solely to adopt this layout. Place new code consistently and make only the moves required by the user's requested change.
6. Preserve local build, test, naming, and public-contract conventions when they do not conflict with these layer boundaries.

Conceptual test layout:

```text
<test-root>/
  common/
    domain/
    application/
    infrastructure/
  context/
    <bounded_context>/
      domain/
      application/
      infrastructure/
  integration/
  architecture/
```

Use the language's conventional test root and keep tests close to the production responsibility they cover. Add `integration/` or `architecture/` only when those test types exist.

## Common placement errors

- Domain entity placed in infrastructure model folder → keep business entity in `domain/entities/`; use infrastructure model for persistence/framework representation.
- Repository implementation placed beside its domain interface → keep the port under `domain/repositories/` and implementation under `infrastructure/repositories/`.
- Business rules placed in REST handlers, ORM models, or external clients → move new behavior into domain or application layer and keep adapters focused on translation and integration.
- Context-specific code placed in `common/` → keep it inside its bounded context until multiple contexts genuinely share it.
- Existing project rewritten to match the template → preserve its current files; apply the layout to new work unless relocation is part of the user's request.
