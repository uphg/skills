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
} as const
```

**Rules:**
- Always end with `as const` for type narrowing
- Use `PropType` for all non-primitive types

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

## Emits

Declare events using the `emits` option on the component and dispatch through `emit()` from the setup context. For `update:xxx` v-model events, add a corresponding callback prop (e.g., `onUpdateValue` for `update:value`) so parents can listen in JSX:

```ts
// Component declaration
export default defineComponent({
  name: 'Button',
  props: buttonProps,
  emits: {
    click: (e: MouseEvent) => true,
    'update:value': (val: any) => true,
  },
  slots: Object as SlotsType<ButtonSlots>,
  setup(props, { emit, slots, attrs, expose }) {
    function handleClick(e: MouseEvent) {
      emit('click', e)
    }

    function doUpdateValue(val: string) {
      emit('update:value', val)
      props.onUpdateValue?.(val)
    }

    return () => (
      <div onClick={handleClick}>
        {/* content */}
      </div>
    )
  },
})
```

The `emits` option accepts an array of event names or an object with validator functions. Use the object form for runtime validation. Callback props like `onUpdateValue` allow parents to listen in JSX via `onUpdateValue={handler}`, equivalent to `@update:value` in templates.

**Rules:**
- Always declare events in the `emits` option to document the component's event interface
- Use the object form with validators for better runtime diagnostics
- For `update:xxx` v-model events, add a matching callback prop in `props` (e.g., `onUpdateValue` for `update:value`)
- Dispatch events through the `emit` function from setup parameters, then call the corresponding callback prop
- v-model:value events use the `'update:value'` event name

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
