---
name: vue-tsx
description: Develop Vue 3 components with the Composition API using defineComponent() + .tsx. Use when writing, refactoring, or reviewing Vue components in TSX — props/emits declaration, reactivity (ref, shallowRef, computed), watchers, composables, generic components, and built-ins such as Transition/Teleport/Suspense/KeepAlive. Also trigger on converting an SFC (`<script setup>`) to defineComponent/TSX. Not for component library conventions (use vue-component-authoring) or styling/theme decisions.
metadata:
  author: LvHeng
  version: "2026.9.2"
  source: Generated from https://github.com/vuejs/docs, scripts at https://github.com/antfu/skills
---

# Vue

> Based on Vue 3.5+. Always use Composition API with `defineComponent()` + `.tsx`. Prefer `shallowRef` over `ref` when deep reactivity is not needed. Avoid Reactive Props Destructure.

## Core

| Topic | Description | Reference |
|-------|-------------|-----------|
| defineComponent + TSX | `defineComponent`, props/emits declaration, render function / JSX, generics | [define-component-tsx](references/define-component-tsx.md) |
| Reactivity & Lifecycle | ref, shallowRef, computed, watch, watchEffect, effectScope, lifecycle hooks, composables | [core-new-apis](references/core-new-apis.md) |

## Features

| Topic | Description | Reference |
|-------|-------------|-----------|
| Built-in Components & Directives | Transition, Teleport, Suspense, KeepAlive, v-memo, custom directives | [advanced-patterns](references/advanced-patterns.md) |

## Quick Reference

### Component Template

```tsx
import { ref, computed, watch, onMounted, defineComponent } from 'vue'

export default defineComponent(
  (props: { title: string; count?: number }) => {
    const doubled = computed(() => (props.count ?? 0) * 2)

    watch(() => props.title, (newVal) => {
      console.log('Title changed:', newVal)
    })

    onMounted(() => {
      console.log('Component mounted')
    })

    return () => (
      <div>{props.title} - {doubled.value}</div>
    )
  },
  {
    props: ['title', 'count']
  }
)
```

### Key Imports

```ts
// Reactivity
import { ref, shallowRef, computed, reactive, readonly, toRef, toRefs, toValue } from 'vue'

// Watchers
import { watch, watchEffect, watchPostEffect, onWatcherCleanup } from 'vue'

// Lifecycle
import { onMounted, onUpdated, onUnmounted, onBeforeMount, onBeforeUpdate, onBeforeUnmount } from 'vue'

// Utilities
import { nextTick, defineComponent, defineAsyncComponent } from 'vue'
```
