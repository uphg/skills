---
name: vue-tsx
description: Vue 3 Composition API、defineComponent + TSX、响应式系统及内置组件。当使用 defineComponent/TSX 编写 Vue 组件、使用侦听器（watchers），或使用 Transition/Teleport/Suspense/KeepAlive 时使用此技能。
metadata:
  author: LvHeng
  version: "2026.4.27"
  source: Generated from https://github.com/vuejs/docs, scripts at https://github.com/antfu/skills
---

# Vue

> 基于 Vue 3.5+。始终使用 `defineComponent()` + `.tsx` 配合 Composition API。

## 偏好

- 优先使用 TypeScript 而非 JavaScript
- 优先使用 `defineComponent()` + `.tsx` 而非 `<script setup lang="ts">` + `.vue`
- 性能方面：如果不需要深度响应式，优先使用 `shallowRef` 而非 `ref`
- 始终使用 Composition API 而非 Options API
- 不建议使用响应式 Props 解构

## 核心

| 主题 | 描述 | 参考资料 |
|------|------|----------|
| defineComponent + TSX | `defineComponent`、props/emits 声明、渲染函数 / JSX、泛型 | [define-component-tsx](references/define-component-tsx.md) |
| 响应式与生命周期 | ref、shallowRef、computed、watch、watchEffect、effectScope、生命周期钩子、composables | [core-new-apis](references/core-new-apis.md) |

## 功能特性

| 主题 | 描述 | 参考资料 |
|------|------|----------|
| 内置组件与指令 | Transition、Teleport、Suspense、KeepAlive、v-memo、自定义指令 | [advanced-patterns](references/advanced-patterns.md) |

## 快速参考

### 组件模板

```tsx
import { ref, computed, watch, onMounted, defineComponent } from 'vue'

export default defineComponent(
  (props: { title: string; count?: number }) => {
    const doubled = computed(() => (props.count ?? 0) * 2)

    watch(() => props.title, (newVal) => {
      console.log('标题已更改:', newVal)
    })

    onMounted(() => {
      console.log('组件已挂载')
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

### 主要导入

```ts
// 响应式
import { ref, shallowRef, computed, reactive, readonly, toRef, toRefs, toValue } from 'vue'

// 侦听器
import { watch, watchEffect, watchPostEffect, onWatcherCleanup } from 'vue'

// 生命周期
import { onMounted, onUpdated, onUnmounted, onBeforeMount, onBeforeUpdate, onBeforeUnmount } from 'vue'

// 工具函数
import { nextTick, defineComponent, defineAsyncComponent } from 'vue'
```