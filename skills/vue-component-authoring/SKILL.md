---
name: vue-component-authoring
description: "Vue 3 component library authoring conventions. Use when building reusable components for a Vue component library (Button, Input, Dialog, Select, etc.). Covers directory layout, props/emits/slots/expose API design, callback-props emit pattern with call() helper, const-array enum governance, SlotsType typing, expose + XxxInst interface, side-effect cleanup, attrs passthrough, Vitest + @vue/test-utils testing, src/hooks + src/utils organization, and coding style (lint, format, commit conventions). Uses defineComponent + TSX. Not for one-off app components (use vue-tsx) or styling/theme decisions."
---

# Vue Component Library Style Guide

Conventions for authoring reusable Vue 3 components in a component library using `defineComponent` + `.tsx`, excluding styling and theming.

## When to Use This Skill

Use this skill when:

- Authoring a reusable component for a Vue component library (Button, Input, Dialog, Select, etc.)
- Designing the public API (props, emits, slots, expose) for a library component
- Organizing `src/hooks` and `src/utils` inside a Vue component library project
- Writing tests for library components or composables with Vitest + `@vue/test-utils`
- Reviewing a library component for convention compliance
- Setting up the directory structure for a new component in the library

## Workflow

### Step 1: Create the Component in the Standard Directory Layout

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

Component implementation file uses `PascalCase`. Directory uses `kebab-case`. Child components sit at the same level.

### Step 2: Define Public Types First

Create `types.ts` with enum constant arrays and slot/expose interfaces:

```ts
export const buttonSizes = ['tiny', 'small', 'medium', 'large'] as const
export const buttonTypes = ['default', 'primary', 'success', 'warning', 'error'] as const

export type ButtonSize = typeof buttonSizes[number]
export type ButtonType = typeof buttonTypes[number]

export interface ButtonSlots {
  default?: () => VNode[]
  icon?: () => VNode[]
}

export interface ButtonInst {
  focus: () => void
  blur: () => void
}
```

Derive union types from const arrays. Use `as const` to enable narrowing. Define both slots and instance interfaces here.

### Step 3: Declare Props as a Standalone `const` with `as const`

```ts
export const buttonProps = {
  block: Boolean,
  loading: Boolean,
  tag: { type: String as PropType<keyof HTMLElementTagNameMap>, default: 'button' },
  type: { type: String as PropType<ButtonType>, default: 'default' },
  size: { type: String as PropType<ButtonSize>, default: 'medium' },
  onClick: [Function, Array] as PropType<MaybeArray<(e: MouseEvent) => void>>,
  onUpdateValue: [Function, Array] as PropType<MaybeArray<OnUpdateValue>>,
} as const
```

Props object is reusable across tests and documentation. Always end with `as const`. v-model provides both `onUpdateValue` and `'onUpdate:value'`. Use `PropType` for complex types.

### Step 4: Implement with `defineComponent` + Setup Returning a Render Function

```tsx
export default defineComponent({
  name: 'Button',
  emits: { 'update:value': (val: any) => true },
  props: buttonProps,
  slots: Object as SlotsType<ButtonSlots>,
  setup(props, { slots, attrs, expose }) {
    // All logic in closure — no `this`
    const selfElRef = ref<HTMLElement | null>(null)
    const isDisabled = computed(() => props.disabled || false)
    // ...
    return () => {
      return <div ref={selfElRef}>{/* ... */}</div>
    }
  },
})
```

Always use `defineComponent` with Options API wrapper + Composition API setup body. The `setup` function returns a render function (functional style). No `this`.

Do not destructure props — use `props.xxx` or `toRefs`. Name DOM template refs with `Ref` suffix (`selfElRef`). Use semantic prefixes for computed/state values.

### Step 5: Wire Emits via Callback Props + `call()` Helper

Do NOT use the `emits` option. Receive callbacks through props and dispatch through the `call()` helper:

```ts
import { call } from '../../_utils/vue/call'

function doUpdateValue(val: string) {
  const { onUpdateValue, 'onUpdate:value': _onUpdateValue } = props
  if (onUpdateValue) call(onUpdateValue, val)
  if (_onUpdateValue) call(_onUpdateValue, val)
}
```

Support both single-function and array-of-functions through `MaybeArray`. The `emits` option serves only for validation; actual emit dispatch goes through props callbacks.

### Step 6: Type and Resolve Slots with `SlotsType` + Slot Helpers

```tsx
setup(props, { slots }) {
  return () => (
    <div>
      {resolveWrappedSlot(slots.default, (children) => (
        <div class="wrapper">{children}</div>
      ))}
      {resolveSlot(slots.prefix, () => <span>Prefix</span>)}
      {slots.count?.({ value: rawText.value, maxlength: props.maxlength })}
    </div>
  )
}
```

Use `resolveWrappedSlot` for wrapping default content. Use `resolveSlot` for optional slots with fallback. Call scoped slots directly with their parameters. All slots must be typed via `SlotsType<XxxSlots>`.

### Step 7: Expose Instance Methods via `expose()` + Matching `XxxInst` Interface

```ts
export interface InputInst {
  focus: () => void
  blur: () => void
  clear: () => void
}

// In setup:
const inputRef = ref<HTMLInputElement | null>(null)
function focus() { inputRef.value?.focus() }
function blur() { inputRef.value?.blur() }
expose({ focus, blur, clear })
```

Always define the instance type interface (`XxxInst`) alongside the component. Only expose ref-accessible methods through `expose()` in setup.

### Step 8: Manage Side Effects with `cleanupXxx` + `onBeforeUnmount`

```ts
let timer: ReturnType<typeof setTimeout> | null = null
function cleanupTimer() {
  if (timer) { clearTimeout(timer); timer = null }
}

onBeforeUnmount(() => { cleanupTimer() })

// Vue 3.5+: use onWatcherCleanup for watchers
const stopWatch = watch(source, () => { /* ... */ })
onBeforeUnmount(() => stopWatch())
```

Every `setInterval`, `setTimeout`, `addEventListener`, and `watch` must have a corresponding cleanup. Name cleanups `cleanupXxx`. Use `onWatcherCleanup` (Vue 3.5+) for watchers when possible.

### Step 9: Merge `{...attrs}` on the Root Element

```tsx
setup(props, { attrs, slots }) {
  return () => (
    <div
      {...attrs}
      style={[styles.root, attrs.style]}
    >
      {/* content */}
    </div>
  )
}
```

Always spread `attrs` on the root element to preserve `class`, `style`, and other fallthrough attributes. Merge `attrs.style` explicitly when using scoped styles. Vue automatically excludes declared props from attrs.

### Step 10: Write Vitest Tests

**Component tests:**

```ts
describe('n-button', () => {
  it('should work with `size` prop', () => {
    buttonSizes.forEach(size => {
      const wrapper = mount(NButton, { props: { size } })
      expect(wrapper.classes()).toContain(`n-button--${size}`)
      wrapper.unmount()
    })
  })

  it('should match snapshot', () => {
    const wrapper = mount(NButton)
    expect(wrapper.html()).toMatchSnapshot()
    wrapper.unmount()
  })
})
```

**Composable tests** — use a wrapper component:

```ts
const TestComponent = defineComponent({
  setup() {
    const result = useFormItem({ disabled: true })
    return { result }
  },
  template: `<div></div>`,
})

it('should merge disabled prop', () => {
  const wrapper = mount(TestComponent)
  expect(wrapper.vm.result.disabled.value).toBe(true)
  wrapper.unmount()
})
```

Always `unmount()` after each test case. Use `describe`/`it` blocks. Test all prop variants, emitted events, slot content, and exposed methods.

### Step 11: Follow Coding Style + Lint + Commit

- **Format:** semi: `false`, singleQuote: `true`, printWidth: `80`, trailingComma: `none`
- **Lint:** `@antfu/eslint-config`
- **Commits:** Angular-style (`feat(button): add ghost prop`)
- **Naming:** Semantic prefixes for state; `Ref` suffix only for DOM template refs
- **Utils:** Keep `src/utils/` flat (no second-level subdirectories)
- **Hooks:** Organize composables in `src/hooks/` (`useConfig`, `useFormItem`, `useLocale`, etc.)

## Reference Files

| Topic | Description | File |
|---|---|---|
| Project Layout | Directory tree, hooks/utils org, coding style | [project-layout](references/project-layout.md) |
| Component Template | Full `defineComponent` + setup + render fn | [component-template](references/component-template.md) |
| API Design | Props, types/enums, emits, slots, expose in depth | [api-design](references/api-design.md) |
| Testing & Lifecycle | Side-effect cleanup, attrs passthrough, test patterns | [testing-and-lifecycle](references/testing-and-lifecycle.md) |

## Prohibitions

- Do NOT use `this` inside `setup`
- Do NOT destructure props directly (`const { size } = props`) — use `props.xxx` or `toRefs`
- Do NOT use the `emits` option for dispatching — use callback props + `call()` helper
- Do NOT use the `Ref` suffix for anything other than DOM template refs
- Do NOT leave side effects (timers, listeners, watchers) without cleanup
- Do NOT forget `{...attrs}` on the root element
- Do NOT create second-level subdirectories inside `src/utils/` — keep it flat
- Do NOT place business logic or business types in shared/general utilities

## When Unsure

- If unsure whether to use a const array or literal union type → prefer const array with `as const` + derived union type
- If unsure where a utility function belongs → default to `src/utils/` (flat)
- If unsure about slot typing → always define the slot interface, even for `default` slot
- If unsure about composable placement → put it in `src/hooks/`
- If unsure about exposes → prefer explicit `expose()` over relying on implicit fallthrough
- If a reference file provides more detail than needed → read the reference file for the specific topic
