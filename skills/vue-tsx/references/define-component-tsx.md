---
name: define-component-tsx
description: Vue 3 defineComponent usage with TypeScript and TSX for type-safe component authoring
---

# defineComponent + TSX

`defineComponent()` is the recommended way to author Vue components in `.tsx` files. It provides full type inference for both Options API and Composition API styles.

## Basic Usage

### Options Signature

```tsx
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    title: { type: String, required: true },
    count: { type: Number, default: 0 }
  },
  emits: {
    'update': (value: string) => true
  },
  setup(props, { emit }) {
    return () => (
      <div>{props.title} - {props.count}</div>
    )
  }
})
```

### Function Signature (Vue 3.3+)

Use for Composition API with render functions or JSX. Supports generics.

```tsx
import { ref, defineComponent } from 'vue'

export default defineComponent(
  (props: { msg: string; count?: number }) => {
    const doubled = computed(() => (props.count ?? 0) * 2)

    return () => (
      <div>{props.msg} - {doubled.value}</div>
    )
  },
  {
    props: ['msg', 'count']
  }
)
```

## Props

### Runtime Declaration

```tsx
import { defineComponent, PropType } from 'vue'

export default defineComponent({
  props: {
    title: { type: String, required: true },
    count: { type: Number, default: 0 },
    items: { type: Array as PropType<string[]>, default: () => [] }
  },
  setup(props) {
    return () => <div>{props.title}</div>
  }
})
```

### With Default Values

Always use factory functions for objects/arrays:

```tsx
props: {
  tags: { type: Array as PropType<string[]>, default: () => [] },
  config: { type: Object as PropType<Config>, default: () => ({}) }
}
```

## Emits

Declare emits with type validation:

```tsx
export default defineComponent({
  emits: {
    'update': (value: string) => true,
    'change': (id: number, name: string) => true,
    'close': () => true
  },
  setup(props, { emit }) {
    return () => (
      <button onClick={() => emit('update', 'new value')}>Update</button>
    )
  }
})
```

## Generics (Function Signature)

The function signature supports generic type parameters for more flexible components:

```tsx
const List = defineComponent(
  <T extends string | number>(props: { items: T[]; selected: T }) => {
    return () => (
      <ul>
        {props.items.map(item => (
          <li class={item === props.selected ? 'active' : ''}>{item}</li>
        ))}
      </ul>
    )
  },
  {
    props: ['items', 'selected']
  }
)
```

## Expose

Use the `expose` option or `expose()` from setup context:

```tsx
export default defineComponent({
  expose: ['count', 'reset'],
  setup(props, { expose }) {
    const count = ref(0)
    const reset = () => { count.value = 0 }

    expose({ count, reset })

    return () => <div>{count.value}</div>
  }
})
```

Parent access via template refs:

```tsx
const childRef = ref<{ count: number; reset: () => void }>()
childRef.value?.reset()
```

## defineAsyncComponent

Lazy-load components:

```tsx
import { defineAsyncComponent } from 'vue'

// Simple usage
const AsyncComp = defineAsyncComponent(() => import('./MyComponent.vue'))

// With loading/error states
const AsyncComp = defineAsyncComponent({
  loader: () => import('./MyComponent.vue'),
  loadingComponent: LoadingSpinner,
  errorComponent: ErrorState,
  delay: 200,
  timeout: 3000
})
```

## Custom Directives in TSX

Use `vNameOfDirective` convention in render function:

```tsx
import { defineComponent, type Directive } from 'vue'

export default defineComponent({
  setup() {
    const vFocus: Directive<HTMLElement> = {
      mounted: (el) => el.focus()
    }

    return () => (
      <input v-focus placeholder="Auto focused" />
    )
  }
})
```

## Note on Webpack Treeshaking

For webpack compatibility, add `/*#__PURE__*/`:

```tsx
export default /*#__PURE__*/ defineComponent({ ... })
```

Vite/Rollup handle this automatically without annotations.

<!--
Source references:
- https://vuejs.org/api/general.html#definecomponent
- https://vuejs.org/guide/typescript/overview
- https://vuejs.org/guide/extras/render-function.html
-->
