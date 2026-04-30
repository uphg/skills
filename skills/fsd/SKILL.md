---
name: fsd
description: "Use this skill when the user needs to design, organize, or refactor a frontend project architecture following the Feature-Sliced Design (FSD) methodology. Specific scenarios include: creating an FSD-compliant directory structure, deciding which layer (app/pages/widgets/features/entities/shared) a piece of code belongs to, checking whether code violates unidirectional dependency or public API rules, and refactoring an existing project to FSD architecture. Not suitable for: writing standalone components, discussing non-FSD architecture approaches, or problems unrelated to frontend architecture."
---

# Frontend FSD Architecture Expert

This skill helps you design, organize, or refactor a frontend project architecture following the Feature-Sliced Design (FSD) methodology.

## When to Use This Skill

Use this skill when the user:

- Asks to create or plan an FSD-compliant frontend project directory structure
- Wants to know which FSD layer (app/pages/widgets/features/entities/shared) a piece of code belongs to
- Needs to review existing code for FSD unidirectional dependency or public API violations
- Wants to gradually migrate an existing non-FSD project to FSD architecture
- Has questions about FSD concepts like layers, slices, and segments
- Needs to determine whether a business module belongs in feature or entity
- Is designing a large reusable UI block and is unsure whether to place it in widget or feature

## Workflow

### Step 1: Understand the User's Project Context

Determine the following key information:

1. **Project type**: SPA / MPA / Micro-frontend? (React / Vue / Next.js / other framework?)
2. **Current state**: Starting from scratch? Or refactoring existing code?
3. **Business domain**: E-commerce? CMS? Social? Enterprise admin?
4. **Tech stack**: State management solution, routing solution, build tools

### Step 2: Identify Business Entities

Before outputting a directory structure, identify the project's core business concepts:

- Find the key "nouns" in the project → these are entity candidates (e.g., User, Product, Order, Article)
- List the core data model fields for each entity
- Determine relationships between entities (association, aggregation, etc.)

### Step 3: Analyze User Interaction Scenarios

Identify the interaction features users care about:

- Find the key "verbs/scenarios" in the project → these are feature candidates (e.g., login/register, search, add to cart, comment)
- Determine which entities each feature reuses
- Evaluate feature reusability (used across pages → standalone feature; single-page one-step operation → can merge into entity)

### Step 4: Design the Directory Structure

Design the directory structure bottom-up according to FSD layers:

1. **shared** — First determine the general infrastructure (UI component library, API client, utility functions, environment config)
2. **entities** — Create slices by business entity, dividing each entity into ui/model/api/lib segments
3. **features** — Create slices by user scenario, ensuring data access goes through entity public APIs
4. **widgets** — Identify large UI blocks reused across pages, composing features and entities
5. **pages** — Create a page for each route, composing widgets
6. **app** — Configure global routes, store, styles, providers

When outputting, use a complete directory tree so the user can clearly see where each file and folder goes.

### Step 5: Verify Architecture Constraints

After design, check for FSD constraint violations:

1. **Unidirectional dependency check**: Verify layer by layer that app → pages → widgets → features → entities → shared, and lower layers never depend on upper layers
2. **Public API check**: Does each slice expose its interface through `index.ts`? Are there any direct imports from within a slice?
3. **Shared purity check**: Does the shared layer contain any business logic or dependencies on other layers?
4. **Circular dependency check**: Are there any cross-slice circular references?

If issues are found, provide specific remediation steps.

### Step 6: Provide Decision Guidance

When users have specific code to place, use the decision tree to help them determine where it belongs.

## Architecture Layer Overview

| Layer | Purpose | Typical Contents | Dependency Direction |
|---|---|---|---|
| app | Infrastructure for the application to run | Route config, global store, global styles, providers | → Can depend on all lower layers |
| pages | Complete pages users access | Page components, page-level layouts | → Depends on widgets, features, entities |
| widgets | Self-contained large UI blocks | Header nav, sidebar, product grid | → Depends on features, entities |
| features | Complete user interaction scenarios | Login form, search, add to cart | → Depends on entities |
| entities | Core business concept models | User data model, product API, order types | → Depends on shared |
| shared | Business-agnostic general infrastructure | UI component library, utility functions, API client | → Depends on nothing above |

## Core Constraints

1. **Unidirectional dependency**: app → pages → widgets → features → entities → shared. Lower layers must never depend on upper layers.
2. **Public API**: Each slice exposes its interface through `index.ts`. Direct imports from within a slice are forbidden.
3. **Business grouping**: Organize by business domain (auth/cart/user), not by technical type (components/hooks/utils).
4. **Pure shared**: Only business-agnostic general-purpose code. Must not depend on other layers.

## Architecture Concepts

### Slices

Each business layer is internally divided by business module:

```
pages/            → pages/home, pages/profile
widgets/          → widgets/header, widgets/sidebar
features/         → features/auth, features/comment-form
entities/         → entities/user, entities/post
```

### Segments

Each slice is internally divided by technical role:

| Segment | Responsibility | Example |
|---|---|---|
| `ui/` | UI components | Button, form, card |
| `model/` | State management, type definitions | Redux store, TypeScript interfaces |
| `api/` | API request wrappers | REST calls, GraphQL queries |
| `lib/` | Helper functions, utility logic | Formatting, validation, calculation |
| `config/` | Configuration items | Constants, environment variable mappings |

## Decision Flow: Where Does Code Go?

When determining which layer a piece of code belongs to, follow this decision order:

1. Is it global config/routes/store? → `app/`
2. Is it a complete page (mapped to a route)? → `pages/{slice}/ui/`
3. Is it a large standalone UI block (reused across pages, composing multiple features)? → `widgets/{slice}/ui/`
4. Is it a complete user interaction scenario (with independent business value)? → `features/{slice}/ui/`
5. Is it a data model for a core business concept? → `entities/{slice}/model/`
6. Is it an API request for a core business concept? → `entities/{slice}/api/`
7. Is it a display component for a core business concept? → `entities/{slice}/ui/`
8. None of the above? → `shared/{ui|lib|api|config}/`

## Reference Files

For detailed information, read the reference files when needed:

| Topic | Description | Reference |
|---|---|---|
| Layer Details | In-depth explanation of all 6 layers with directory structure, purpose, and characteristics | [layer-details](references/layer-details.md) |
| E-commerce Example | Complete FSD directory tree of a real-world e-commerce project | [ecommerce-example](references/ecommerce-example.md) |

## Prohibitions

- Do not violate unidirectional dependency (lower layer importing from upper layer)
- Do not bypass the public API by directly importing from within a slice
- Do not place business logic or business types in the shared layer
- Do not create cross-slice circular dependencies
- Do not organize directory structure by technical type (components/hooks/utils)

## When Unsure

If you cannot determine which layer a piece of code belongs to:

1. Prefer more specific layers (entities over shared, features over widgets)
2. If code is used only by one feature and has no reuse value, keep it inside the feature
3. If code is used by multiple features but carries business semantics, extract it to an entity
4. Only place code in shared if it is completely business-agnostic and widely used
5. Present two candidate solutions with pros and cons, and let the user make the final decision
