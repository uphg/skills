---
name: vue-component-authoring
description: "Vue 3 组件库组件书写规范。适用于构建 Vue 组件库的可复用组件（Button、Input、Dialog、Select 等）。涵盖目录结构、props/emits/slots/expose API 设计、emits 选项 + emit() 分发 + 用于 JSX 的 callback props、const 数组枚举治理、SlotsType 类型声明、expose + XxxInst 接口、副作用清理、attrs 透传、Vitest + @vue/test-utils 测试、src/hooks + src/utils 组织方式以及编码风格（lint、format、commit 规范）。使用 defineComponent + TSX。不适用于一次性应用组件（使用 vue-tsx）或样式/主题决策。"
---

# Vue 组件书写风格规范

适用于构建 Vue 3 组件库的可复用组件，使用 `defineComponent` + `.tsx`，不涉及样式与主题相关规范。

## 何时使用本技能

本技能适用于以下场景：

- 编写组件库中的可复用组件（Button、Input、Dialog、Select 等）
- 设计组件公开 API（props、emits、slots、expose）
- 组织 Vue 组件库项目中的 `src/hooks` 和 `src/utils`
- 使用 Vitest + `@vue/test-utils` 编写组件库或组合式函数的测试
- 审查组件库组件是否符合规范约定
- 为组件库中的新组件搭建目录结构

## 工作流程

### 步骤 1：按标准目录结构创建组件

```
src/component/<component>/
├── index.ts                   # 对外导出（component、props、types）
├── <Component>.tsx            # defineComponent 组件实现（函数式）
├── inner-types.ts             # 内部类型/接口（可选）
├── types.ts                   # 公开类型、枚举常量数组
├── utils.ts                   # 组件专用工具函数（可选）
└── tests/
    ├── <Component>.spec.tsx   # Vitest 测试
    └── __snapshots__/         # 快照产物
```

组件实现文件使用 `PascalCase`，目录使用 `kebab-case`，子组件同级放置。此布局仅供参考——可根据项目惯例调整。

### 步骤 2：先定义公开类型

创建 `types.ts`，包含枚举常量数组和 slot/expose 接口：

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

从 const 数组衍生联合类型。使用 `as const` 启用类型收窄。在此处同时定义 slots 和实例接口。

### 步骤 3：将 Props 声明为独立的 `const` 对象，以 `as const` 结尾

```ts
export const buttonProps = {
  block: Boolean,
  loading: Boolean,
  tag: { type: String as PropType<keyof HTMLElementTagNameMap>, default: 'button' },
  type: { type: String as PropType<ButtonType>, default: 'default' },
  size: { type: String as PropType<ButtonSize>, default: 'medium' },
  onUpdateValue: Function as PropType<(value: string) => void>,
} as const
```

Props 对象可在测试和文档中复用。始终以 `as const` 结尾。对复杂类型使用 `PropType`。

### 步骤 4：使用 `defineComponent` + setup 返回渲染函数实现组件

```tsx
export default defineComponent({
  name: 'Button',
  emits: { 'update:value': (val: any) => true },
  props: buttonProps,
  slots: Object as SlotsType<ButtonSlots>,
  setup(props, { emit, slots, attrs, expose }) {
    // 所有逻辑在闭包内完成 — 禁止 this
    const selfElRef = ref<HTMLElement | null>(null)
    const isDisabled = computed(() => props.disabled || false)

    function doUpdateValue(value: string) {
      emit('update:value', value)
      props.onUpdateValue?.(value)
    }
    // ...
    return () => {
      return <div ref={selfElRef}>{/* ... */}</div>
    }
  },
})
```

始终使用 `defineComponent` 的 Options API 包裹 + Composition API setup 体。`setup` 函数返回渲染函数（函数式风格）。禁止使用 `this`。

禁止解构 Props——使用 `props.xxx` 或 `toRefs`。DOM 模板引用命名使用 `Ref` 后缀（`selfElRef`）。计算属性/状态值使用语义前缀。

### 步骤 5：通过 `emits` 声明事件 + `emit()` 分发 + Callback Props

使用 `emits` 选项声明事件，通过 setup 上下文中的 `emit()` 进行分发。对于 `update:xxx` v-model 事件添加对应的 callback prop（如 `onUpdateValue` 对应 `update:value`），方便父组件在 JSX 中监听：

```ts
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
        {/* 内容 */}
      </div>
    )
  },
})
```

`emits` 选项接受事件名称数组或带校验函数的对象。使用对象形式进行运行时校验。`onUpdateValue` 等 callback prop 允许父组件在 JSX 中通过 `onUpdateValue={handler}` 监听，等价于模板中的 `@update:value`。

### 步骤 6：使用 `SlotsType` + Slot 辅助函数进行类型化和解析

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

使用 `resolveWrappedSlot` 包裹默认内容。使用 `resolveSlot` 处理可选 slot 及其后备内容。直接调用作用域插槽并传入参数。所有 slot 必须通过 `SlotsType<XxxSlots>` 进行类型化。

### 步骤 7：通过 `expose()` + 匹配的 `XxxInst` 接口暴露实例方法

```ts
export interface InputInst {
  focus: () => void
  blur: () => void
  clear: () => void
}

// 在 setup 中：
const inputRef = ref<HTMLInputElement | null>(null)
function focus() { inputRef.value?.focus() }
function blur() { inputRef.value?.blur() }
expose({ focus, blur, clear })
```

始终在组件旁定义实例类型接口（`XxxInst`）。仅将可通过 ref 访问的方法通过 `expose()` 在 setup 中暴露。

### 步骤 8：使用 `cleanupXxx` + `onBeforeUnmount` 管理副作用

```ts
let timer: ReturnType<typeof setTimeout> | null = null
function cleanupTimer() {
  if (timer) { clearTimeout(timer); timer = null }
}

onBeforeUnmount(() => { cleanupTimer() })

// Vue 3.5+：对 watch 使用 onWatcherCleanup
const stopWatch = watch(source, () => { /* ... */ })
onBeforeUnmount(() => stopWatch())
```

每个 `setInterval`、`setTimeout`、`addEventListener` 和 `watch` 必须有对应的清理。命名清理函数为 `cleanupXxx`。Vue 3.5+ 中对 watch 优先使用 `onWatcherCleanup`。

### 步骤 9：在根元素上合并 `{...attrs}`

```tsx
setup(props, { attrs, slots }) {
  return () => (
    <div
      {...attrs}
      style={[styles.root, attrs.style]}
    >
      {/* 内容 */}
    </div>
  )
}
```

始终在根元素上展开 `attrs`，以保留 `class`、`style` 和其他透传属性。使用组合样式时需显式合并 `attrs.style`。Vue 会自动从 attrs 中排除已声明的 props。

### 步骤 10：编写 Vitest 测试

**组件测试：**

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

**组合式函数测试** — 使用包裹组件：

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

每个测试用例后必须 `unmount()`。使用 `describe`/`it` 块。测试所有 prop 变体、触发事件、slot 内容和暴露方法。

### 步骤 11：遵循命名约定 + 项目组织

- **命名：** 状态值使用语义前缀；`Ref` 后缀仅用于 DOM 模板引用
- **组合式函数：** 组织在 `src/hooks/` 中，使用 `useXxx` 命名
- **工具函数：** 放置在 `src/utils/` 中

## 参考文件

| 主题 | 说明 | 文件 |
|---|---|---|
| 项目结构 | 目录树、hooks/utils 组织 | [project-layout](references/project-layout.zh.md) |
| 组件模板 | 完整的 defineComponent + setup + 渲染函数 | [component-template](references/component-template.zh.md) |
| API 设计 | Props、类型/枚举、Emits、Slots、Expose 详解 | [api-design](references/api-design.zh.md) |
| 测试与生命周期 | 副作用清理、attrs 透传、测试模式 | [testing-and-lifecycle](references/testing-and-lifecycle.zh.md) |

## 禁止事项

- 禁止在 `setup` 中使用 `this`
- 禁止直接解构 props（`const { size } = props`）——应使用 `props.xxx` 或 `toRefs`
- 禁止在非 DOM 模板引用上使用 `Ref` 后缀
- 禁止遗留未清理的副作用（定时器、监听器、watch）
- 禁止忘记在根元素上使用 `{...attrs}`
- 禁止在通用工具函数中放置业务逻辑或业务类型

## 不确定时

- 不确定使用 const 数组还是字面量联合类型 → 优先使用带有 `as const` 的 const 数组 + 衍生联合类型
- 不确定工具函数归属 → 默认放入 `src/utils/`
- 不确定 slot 类型 → 始终定义 slot 接口，即使是 `default` slot
- 不确定组合式函数放置位置 → 放入 `src/hooks/`
- 不确定 expose 内容 → 优先显式 `expose()`，而不是依赖隐式透传
- 如果参考文件提供了比所需更详细的信息 → 按需阅读对应主题的参考文件
