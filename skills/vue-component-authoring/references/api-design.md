---
name: api-design
description: Props, types/enums, emits, slots, and expose conventions for Vue component library components
---

# API Design

## Props Declaration

Props are defined as standalone `const` objects ending with `as const`:

```ts
export const buttonProps = {
  // Simple boolean
  block: Boolean,
  loading: Boolean,

  // With default value
  tag: { type: String as PropType<keyof HTMLElementTagNameMap>, default: 'button' },
  type: { type: String as PropType<ButtonType>, default: 'default' },
  size: { type: String as PropType<ButtonSize>, default: 'medium' },

  // Function / multiple callbacks
  onClick: [Function, Array] as PropType<MaybeArray<(e: MouseEvent) => void>>,

  // v-model:value
  onUpdateValue: [Function, Array] as PropType<MaybeArray<OnUpdateValue>>,
} as const
```

**Rules:**
- Always end with `as const` for type narrowing
- v-model provides both `onUpdateValue` and `'onUpdate:value'` (kebab-case)
- Use `PropType` for all non-primitive types
- Use `MaybeArray<T>` to accept single function or array of functions

## Types & Enums

Prefer const arrays over TypeScript enums. Derive union types from const arrays:

```ts
// src/button/src/types.ts
export const buttonSizes = ['tiny', 'small', 'medium', 'large'] as const
export const buttonTypes = ['default', 'primary', 'success', 'warning', 'error'] as const
export const buttonVariants = ['solid', 'outline', 'ghost'] as const

export type ButtonSize = typeof buttonSizes[number]
export type ButtonType = typeof buttonTypes[number]
export type ButtonVariant = typeof buttonVariants[number]
```

**Advantages over enums:**
- Array is iterable — usable in tests and docs
- No runtime overhead (plain strings)
- `as const` gives full type narrowing
- Can be used in `v-for`, filtering, validation

## Emits (Callback-Props Pattern)

**Do NOT use the `emits` option** for dispatching. All callbacks come through props:

```ts
// Declaration (in props)
onUpdateValue: [Function, Array] as PropType<MaybeArray<OnUpdateValue>>,
'onUpdate:value': [Function, Array] as PropType<MaybeArray<OnUpdateValue>>,

// Invocation
function doUpdateValue(val: string) {
  const { onUpdateValue, 'onUpdate:value': _onUpdateValue } = props
  if (onUpdateValue) call(onUpdateValue, val)
  if (_onUpdateValue) call(_onUpdateValue, val)
}
```

The `emits` option on the component serves **only for runtime validation** — actual dispatch goes through props callbacks. Use the `call()` utility to safely invoke single-function or array-of-functions:

```ts
// src/utils/call.ts
export function call<T extends (...args: any[]) => any>(
  fns: T | T[],
  ...args: Parameters<T>
): void {
  if (Array.isArray(fns)) {
    fns.forEach(fn => fn(...args))
  } else {
    fns(...args)
  }
}
```

## Slots

Slots are typed via `SlotsType` and resolved with dedicated helpers:

```ts
// types.ts
export interface ButtonSlots {
  default?: () => VNode[]
  icon?: () => VNode[]
}

export interface InputSlots {
  count?: (props: { value: string; maxlength: number }) => VNode[]
  'clear-icon'?: () => VNode[]
}

// Component declaration
slots: Object as SlotsType<ButtonSlots>,

// Usage
setup(props, { slots }) {
  return () => (
    <div>
      {resolveWrappedSlot(slots.default, (children) => (
        <div class="wrapper">{children}</div>
      ))}
      {resolveSlot(slots.prefix, () => <span>Prefix</span>)}
      {isSlotEmpty(slots.suffix) || <span>has content</span>}
      {slots.count?.({ value: rawText.value, maxlength: props.maxlength })}
    </div>
  )
}
```

| Helper | Purpose |
|---|---|
| `resolveWrappedSlot(slot, render)` | Wrap slot content with a container (e.g., `<span>`) |
| `resolveSlot(slot, fallback)` | Render slot or fallback content |
| `isSlotEmpty(slot)` | Check if slot has content |

All slots must be typed — even `default`. Scoped slot parameters must include full parameter types.

## Expose

When a component needs to expose methods (focus, clear, validate), define a separate instance type interface:

```ts
// src/input/src/types.ts
export interface InputInst {
  focus: () => void
  blur: () => void
  clear: () => void
}

// src/input/src/Input.tsx
setup(props, { slots, expose }) {
  const inputRef = ref<HTMLInputElement | null>(null)

  function focus() { inputRef.value?.focus() }
  function blur() { inputRef.value?.blur() }
  function clear() { /* ... */ }

  expose({ focus, blur, clear })

  return () => <input ref={inputRef} ... />
}
```

**Rules:**
- Always define `XxxInst` interface in `types.ts`
- Only expose ref-accessible instance methods (not internal state)
- The `XxxInst` type allows parent components to type their refs correctly
