---
name: project-layout
description: Directory structure, hooks/utils organization, and coding style conventions for a Vue component library
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

**Key conventions:**
- Component implementation file: `PascalCase` (`Button.tsx`, `Input.tsx`)
- Directory: `kebab-case` (`button/`, `input/`, `date-picker/`)
- Child components sit at the same level, not nested
- `index.ts` re-exports the component, its props, and public types

## Shared Hooks (`src/hooks/`)

All composables live in `src/hooks/`:

```
src/hooks/
├── useConfig.ts         # ConfigProvider injection
├── useFormItem.ts       # FormItem injection (size, disabled, status)
├── useLocale.ts         # Internationalization
├── useRtl.ts            # RTL direction
├── useDeferredTrue.ts   # Custom helper
├── useResize.ts         # Custom helper
└── ...
```

Each composable follows the `useXxx` naming convention.

## Shared Utilities (`src/utils/`)

Keep `src/utils/` **flat** — no second-level subdirectories:

```
src/utils/
├── call.ts              # Safe callback invocation
├── createInjectionKey.ts # Injection key creation
├── resolveSlot.ts       # Slot resolution helper
├── resolveWrappedSlot.ts # Slot wrapping helper
├── isSlotEmpty.ts       # Slot emptiness check
├── omit.ts / keep.ts    # Object manipulation
├── warn.ts / warnOnce.ts # Dev warnings
└── types.ts             # Common type utilities
```

Flat layout avoids deep nesting and makes imports predictable.

## Coding Style

| Category | Rule |
|---|---|
| **Semicolons** | `false` (no semicolons) |
| **Quotes** | `singleQuote: true` |
| **Print width** | `80` |
| **Trailing commas** | `none` |
| **Linter** | `@antfu/eslint-config` |
| **Commit style** | Angular-style (`feat(button): add ghost prop`) |
| **Reactivity naming** | Semantic prefixes (`isDisabled`, `currentSize`) |
| **Ref suffix** | Only on DOM template refs (`selfElRef`, `inputRef`) |
| **Props destructuring** | Forbidden — use `props.xxx` or `toRefs` |
