---
name: project-layout
description: Directory structure, hooks/utils organization for a Vue component library
---

# Project Layout

## Directory Structure

```
src/component/<component>/
├── index.ts                   # Public exports (component, props, types)
├── <Component>.tsx            # defineComponent implementation (functional)
├── inner-types.ts             # Internal types/interfaces (optional)
├── types.ts                   # Public types, enum constant arrays
├── utils.ts                   # Component-specific utilities (optional)
└── tests/
    ├── <Component>.spec.tsx   # Vitest tests
    ├── ssr.spec.tsx           # SSR compatibility test
    └── __snapshots__/         # Snapshot artifacts
```

**Suggested conventions:**
- Component implementation file: `PascalCase` (`Button.tsx`, `Input.tsx`)
- Directory: `kebab-case` (`button/`, `input/`, `date-picker/`)
- Child components sit at the same level, not nested
- `index.ts` re-exports the component, its props, and public types

The directory layout above is a reference — adapt it to your project's conventions.

## Shared Hooks (`src/hooks/`)

All composables live in `src/hooks/`. Each composable follows the `useXxx` naming convention.

## Shared Utilities (`src/utils/`)

Keep shared utility functions in `src/utils/`. Organize by concern as needed.
