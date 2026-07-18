---
name: testing-and-lifecycle
description: Side-effect cleanup, attrs passthrough, and Vitest testing patterns for Vue component library components
---

# Testing & Lifecycle

## Side Effects & Lifecycle Cleanup

All side effects must have corresponding cleanup:

```ts
import { onBeforeUnmount, watch, onWatcherCleanup } from 'vue'

// Timer cleanup
let timer: ReturnType<typeof setTimeout> | null = null
function startTimer() { /* ... */ }

function cleanupTimer() {
  if (timer) { clearTimeout(timer); timer = null }
}

onBeforeUnmount(() => {
  cleanupTimer()
  // additional cleanups
})

// Watcher cleanup
// Option A: store stop handle
const stopWatch = watch(source, () => { /* ... */ })
onBeforeUnmount(() => stopWatch())

// Option B: Vue 3.5+ onWatcherCleanup
watch(source, (newVal) => {
  const cleanup = /* ... */
  onWatcherCleanup(() => cleanup())
})
```

**Pattern:**
- Name cleanup functions `cleanupXxx` for consistency
- Group all cleanup calls in a single `onBeforeUnmount`
- For `addEventListener`, store the handler reference for removal
- For observers (`ResizeObserver`, `MutationObserver`), call `.disconnect()`

## Attrs Passthrough

When using `attrs` from setup, the root element must explicitly receive fallthrough attributes:

```tsx
setup(props, { attrs, slots }) {
  return () => {
    return (
      <div
        {...attrs}
        style={[styles.root, attrs.style]}
      >
        {/* content */}
      </div>
    )
  }
}
```

**Important:**
- Always spread `{...attrs}` on the root element
- Explicitly merge `attrs.style` when combining with component styles
- Vue automatically excludes declared props from `attrs`, so only undeclared attributes pass through
- Without `{...attrs}`, `class`, `style`, and custom attributes from consumers will be lost

## Testing

### Component Tests

Use `vitest` + `@vue/test-utils`. Always `unmount()` after each test case.

```ts
import { mount } from '@vue/test-utils'
import { NButton } from '../src'
import { buttonSizes } from '../src/types'

describe('n-button', () => {
  it('should work with `size` prop', () => {
    buttonSizes.forEach(size => {
      const wrapper = mount(NButton, { props: { size } })
      expect(wrapper.classes()).toContain(`n-button--${size}`)
      wrapper.unmount()
    })
  })

  it('should emit click event', () => {
    const wrapper = mount(NButton, { props: { onClick: vi.fn() } })
    wrapper.trigger('click')
    expect(wrapper.props().onClick).toHaveBeenCalled()
    wrapper.unmount()
  })

  it('should match snapshot', () => {
    const wrapper = mount(NButton)
    expect(wrapper.html()).toMatchSnapshot()
    wrapper.unmount()
  })
})
```

**SSR compatibility:**

```ts
// tests/ssr.spec.tsx
describe('n-button SSR', () => {
  it('should render without error', () => {
    const { html } = renderToString(NButton)
    expect(html).toContain('n-button')
  })
})
```

### Composable Tests

Test composables by mounting a wrapper component or using `withSetup`:

```ts
// Method 1: Wrapper component
import { mount } from '@vue/test-utils'
import { defineComponent } from 'vue'
import { useFormItem } from '../use-form-item'

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

// Method 2: withSetup helper
function withSetup<T>(composable: () => T): { result: T; app: App } {
  let result!: T
  const app = createApp({
    setup() { result = composable(); return () => null }
  })
  app.mount(document.createElement('div'))
  return { result, app }
}

it('useFormItem should return disabled from injection', () => {
  const { result, app } = withSetup(() => useFormItem({ disabled: true }))
  expect(result.disabled.value).toBe(true)
  app.unmount()
})
```

**Coverage:**
- Test all prop variants (especially enum-based props — iterate the const array)
- Test emitted events / callback invocation
- Test slot rendering (default slots, named slots, scoped slots)
- Test exposed instance methods
- Test edge cases (empty/null/undefined props, boundary values)
- For composables: test all input combinations and their effect on output
