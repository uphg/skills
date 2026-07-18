---
name: testing-and-lifecycle
description: Vue 组件库组件的副作用清理、attrs 透传和 Vitest 测试模式
---

# 测试与生命周期

## 副作用与生命周期清理

所有副作用必须有对应的清理：

```ts
import { onBeforeUnmount, watch, onWatcherCleanup } from 'vue'

// 定时器清理
let timer: ReturnType<typeof setTimeout> | null = null
function startTimer() { /* ... */ }

function cleanupTimer() {
  if (timer) { clearTimeout(timer); timer = null }
}

onBeforeUnmount(() => {
  cleanupTimer()
  // 其他清理
})

// Watch 清理
// 方式 A：存储 stop 句柄
const stopWatch = watch(source, () => { /* ... */ })
onBeforeUnmount(() => stopWatch())

// 方式 B：Vue 3.5+ onWatcherCleanup
watch(source, (newVal) => {
  const cleanup = /* ... */
  onWatcherCleanup(() => cleanup())
})
```

**模式：**
- 命名清理函数为 `cleanupXxx` 以保证一致性
- 将所有清理调用集中在一个 `onBeforeUnmount` 中
- 对于 `addEventListener`，存储处理程序引用以便移除
- 对于 observers（`ResizeObserver`、`MutationObserver`），调用 `.disconnect()`

## Attrs 透传

使用 setup 中的 `attrs` 时，根元素必须显式接收透传属性：

```tsx
setup(props, { attrs, slots }) {
  return () => {
    return (
      <div
        {...attrs}
        style={[styles.root, attrs.style]}
      >
        {/* 内容 */}
      </div>
    )
  }
}
```

**重要：**
- 始终在根元素上展开 `{...attrs}`
- 组合组件样式时需显式合并 `attrs.style`
- Vue 会自动从 `attrs` 中排除已声明的 props，因此仅未声明的属性会透传
- 缺少 `{...attrs}` 会导致 `class`、`style` 和消费者的自定义属性丢失

## 测试

### 组件测试

使用 `vitest` + `@vue/test-utils`。每个测试用例后必须 `unmount()`。

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

**SSR 兼容：**

```ts
// tests/ssr.spec.tsx
describe('n-button SSR', () => {
  it('should render without error', () => {
    const { html } = renderToString(NButton)
    expect(html).toContain('n-button')
  })
})
```

### 组合式函数测试

通过挂载包裹组件或使用 `withSetup` 测试组合式函数：

```ts
// 方式 1：包裹组件
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

// 方式 2：withSetup 辅助函数
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

**覆盖范围：**
- 测试所有 prop 变体（尤其是基于枚举的 props——遍历 const 数组）
- 测试触发事件 / 回调调用
- 测试 slot 渲染（default slots、命名 slots、作用域 slots）
- 测试暴露的实例方法
- 测试边界情况（空/null/undefined props、边界值）
- 对于组合式函数：测试所有输入组合及其对输出的影响
