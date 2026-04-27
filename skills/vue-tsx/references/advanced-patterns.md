---
name: advanced-patterns
description: Vue 3 built-in components (Transition, Teleport, Suspense, KeepAlive) and advanced directives usage with defineComponent + TSX
---

# Built-in Components & Directives

## Transition

Animate enter/leave of a single element or component.

```tsx
import { defineComponent, ref } from 'vue'

export default defineComponent({
  setup() {
    const show = ref(true)

    return () => (
      <Transition name="fade">
        {show.value && <div>Content</div>}
      </Transition>
    )
  }
})
```

CSS classes are defined separately (e.g. in a `.css` file or `<style>` block):

| Class | When |
|-------|------|
| `{name}-enter-from` | Start state for enter |
| `{name}-enter-active` | Active state for enter (add transitions here) |
| `{name}-enter-to` | End state for enter |
| `{name}-leave-from` | Start state for leave |
| `{name}-leave-active` | Active state for leave |
| `{name}-leave-to` | End state for leave |

### Transition Modes

```tsx
<Transition name="fade" mode="out-in">
  {h(currentView.value)}
</Transition>
```

### JavaScript Hooks

```tsx
import { defineComponent, ref, h } from 'vue'

export default defineComponent({
  setup() {
    const show = ref(true)

    const onBeforeEnter = (el: Element) => { /* ... */ }
    const onEnter = (el: Element, done: () => void) => {
      gsap.to(el, { opacity: 1, onComplete: done })
    }
    const onLeave = (el: Element, done: () => void) => {
      gsap.to(el, { opacity: 0, onComplete: done })
    }

    return () => (
      <Transition
        onBeforeEnter={onBeforeEnter}
        onEnter={onEnter}
        onLeave={onLeave}
        css={false}
      >
        {show.value && <div>Content</div>}
      </Transition>
    )
  }
})
```

### Appear on Initial Render

```tsx
<Transition appear name="fade">
  <div>Shows with animation on mount</div>
</Transition>
```

## TransitionGroup

Animate list items. Each child must have a unique `key`.

```tsx
import { defineComponent, ref } from 'vue'

export default defineComponent({
  setup() {
    const items = ref([/* ... */])

    return () => (
      <TransitionGroup name="list" tag="ul">
        {items.value.map(item => (
          <li key={item.id}>{item.text}</li>
        ))}
      </TransitionGroup>
    )
  }
})
```

## Teleport

Render content to a different DOM location.

```tsx
import { defineComponent, ref } from 'vue'

export default defineComponent({
  setup() {
    const open = ref(false)

    return () => (
      <>
        <button onClick={() => open.value = true}>Open Modal</button>
        <Teleport to="body">
          {open.value && <div class="modal">Modal content rendered at body</div>}
        </Teleport>
      </>
    )
  }
})
```

### Props

```tsx
<Teleport to="#modal-container">
<Teleport to={targetElement}>
<Teleport to="body" disabled={isMobile}>
<Teleport defer to="#late-rendered-target">
```

## Suspense

Handle async dependencies with loading states. **Experimental feature.**

```tsx
import { defineComponent, defineAsyncComponent } from 'vue'

const AsyncComponent = defineAsyncComponent(() => import('./MyComponent.vue'))

export default defineComponent({
  setup() {
    return () => (
      <Suspense>
        {{
          default: () => <AsyncComponent />,
          fallback: () => <div>Loading...</div>
        }}
      </Suspense>
    )
  }
})
```

### Events

```tsx
<Suspense onPending={onPending} onResolve={onResolve} onFallback={onFallback}>
```

## KeepAlive

Cache component instances when toggled.

```tsx
import { defineComponent, h, ref } from 'vue'
import ComponentA from './ComponentA'
import ComponentB from './ComponentB'

export default defineComponent({
  setup() {
    const currentTab = ref(ComponentA)

    return () => (
      <KeepAlive>
        {h(currentTab.value)}
      </KeepAlive>
    )
  }
})
```

### Include/Exclude

```tsx
<KeepAlive include="ComponentA,ComponentB">
<KeepAlive include={/^Tab/}>
<KeepAlive include={['TabA', 'TabB']}>

<KeepAlive exclude="ModalComponent">

<KeepAlive max={10}>
```

### Lifecycle Hooks

```ts
import { onActivated, onDeactivated } from 'vue'

onActivated(() => {
  fetchLatestData()
})

onDeactivated(() => {
  pauseTimers()
})
```

## v-memo (Template Only)

`v-memo` is a template-only directive without a direct TSX equivalent. For performance optimization in TSX, use computed values or memoization patterns.

## v-once (Template Only)

`v-once` is a template-only directive. In TSX, render static content directly without reactive bindings.

## Custom Directives

Create reusable DOM manipulations using the `vName` JSX convention.

```tsx
import { defineComponent, type Directive } from 'vue'

export default defineComponent({
  setup() {
    const vFocus: Directive<HTMLElement> = {
      mounted: (el) => el.focus()
    }

    return () => <input v-focus />
  }
})
```

### Full Lifecycle Hooks

```tsx
import { defineComponent, type Directive } from 'vue'

export default defineComponent({
  setup() {
    const vColor: Directive<HTMLElement, string> = {
      created(el, binding, vnode, prevVnode) {},
      beforeMount(el, binding) {},
      mounted(el, binding) {
        el.style.color = binding.value
      },
      beforeUpdate(el, binding) {},
      updated(el, binding) {
        el.style.color = binding.value
      },
      beforeUnmount(el, binding) {},
      unmounted(el, binding) {}
    }

    return () => (
      <div v-color:background.bold="'red'">Content</div>
    )
  }
})
```

### Directive Arguments & Modifiers

```tsx
const vColor: Directive<HTMLElement, string> = {
  mounted(el, binding) {
    // binding.arg = 'background'
    // binding.modifiers = { bold: true }
    // binding.value = 'red'
    el.style[binding.arg || 'color'] = binding.value
    if (binding.modifiers.bold) {
      el.style.fontWeight = 'bold'
    }
  }
}
```

### Global Registration

```ts
// main.ts
app.directive('focus', {
  mounted: (el) => el.focus()
})
```

<!--
Source references:
- https://vuejs.org/api/built-in-components.html
- https://vuejs.org/guide/built-ins/transition.html
- https://vuejs.org/guide/built-ins/teleport.html
- https://vuejs.org/guide/built-ins/suspense.html
- https://vuejs.org/guide/built-ins/keep-alive.html
- https://vuejs.org/api/built-in-directives.html
- https://vuejs.org/guide/reusability/custom-directives.html
-->
