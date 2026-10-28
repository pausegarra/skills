---
name: openfga-relation-naming
description: Use when naming parent and permission relations in OpenFGA authorization models (`.fga`).
---

# OpenFGA Relation Prefixes

Use these prefixes for relation names in OpenFGA `.fga` models:

- Parent relations use `in_<parent>` and belong to the child type, for example `in_system`, `in_application`, or `in_parent`.
- Permission relations use `can_<action>`, for example `can_access`, `can_read`, or `can_update`.

## Example

```fga
type user

type system
type application
  relations
    define in_system: [system]
    define can_access: [user]
```
