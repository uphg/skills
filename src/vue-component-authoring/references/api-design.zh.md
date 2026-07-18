---
name: api-design
description: Vue 组件库组件的 Props、类型/枚举、Emits、Slots 和 Expose 规范
---

# API 设计

## Props 声明

Props 定义为独立的 `const` 对象，以 `as const` 结尾：

```ts
export const buttonProps = {
  // 简单布尔
  block: Boolean,
  loading: Boolean,

  // 带默认值
  tag: { type: String as PropType<keyof HTMLElementTagNameMap>, default: 'button' },
  type: { type: String as PropType<ButtonType>, default: 'default' },
  size: { type: String as PropType<ButtonSize>, default: 'medium' },

  // 函数 / 多回调
  onClick: [Function, Array] as PropType<MaybeArray<(e: MouseEvent) => void>>,

  // v-model:value
  onUpdateValue: [Function, Array] as PropType<MaybeArray<OnUpdateValue>>,
} as const
```

**规则：**
- 始终以 `as const` 结尾以启用类型收窄
- v-model 同时提供 `onUpdateValue` 和 `'onUpdate:value'`（kebab-case）
- 对所有非原始类型使用 `PropType`
- 使用 `MaybeArray<T>` 接受单个函数或函数数组

## 类型与枚举治理

推荐使用 const 数组而非 TypeScript 枚举。从 const 数组衍生联合类型：

```ts
// src/button/src/types.ts
export const buttonSizes = ['tiny', 'small', 'medium', 'large'] as const
export const buttonTypes = ['default', 'primary', 'success', 'warning', 'error'] as const
export const buttonVariants = ['solid', 'outline', 'ghost'] as const

export type ButtonSize = typeof buttonSizes[number]
export type ButtonType = typeof buttonTypes[number]
export type ButtonVariant = typeof buttonVariants[number]
```

**相比枚举的优势：**
- 数组可迭代——可在测试和文档中使用
- 无运行时开销（纯字符串）
- `as const` 提供完全的类型收窄
- 可用于 `v-for`、筛选、校验

## Emits（Callback-Props 模式）

**不使用 `emits` 选项**进行分发。所有回调通过 props 传递：

```ts
// 声明（在 props 中）
onUpdateValue: [Function, Array] as PropType<MaybeArray<OnUpdateValue>>,
'onUpdate:value': [Function, Array] as PropType<MaybeArray<OnUpdateValue>>,

// 调用
function doUpdateValue(val: string) {
  const { onUpdateValue, 'onUpdate:value': _onUpdateValue } = props
  if (onUpdateValue) call(onUpdateValue, val)
  if (_onUpdateValue) call(_onUpdateValue, val)
}
```

组件上的 `emits` 选项**仅用于运行时验证**——实际分发通过 props 回调。使用 `call()` 工具函数安全调用单个函数或函数数组：

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

Slots 通过 `SlotsType` 进行类型化，并使用专用辅助函数解析：

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

// 组件声明
slots: Object as SlotsType<ButtonSlots>,

// 使用
setup(props, { slots }) {
  return () => (
    <div>
      {resolveWrappedSlot(slots.default, (children) => (
        <div class="wrapper">{children}</div>
      ))}
      {resolveSlot(slots.prefix, () => <span>Prefix</span>)}
      {isSlotEmpty(slots.suffix) || <span>有内容</span>}
      {slots.count?.({ value: rawText.value, maxlength: props.maxlength })}
    </div>
  )
}
```

| 辅助函数 | 用途 |
|---|---|
| `resolveWrappedSlot(slot, render)` | 用容器包裹 slot 内容（如 `<span>`） |
| `resolveSlot(slot, fallback)` | 渲染 slot 或后备内容 |
| `isSlotEmpty(slot)` | 检查 slot 是否有内容 |

所有 slot 都必须类型化——即使是 `default`。作用域插槽参数必须包含完整的参数类型。

## Expose

当组件需要暴露方法（focus、clear、validate）时，定义单独的实例类型接口：

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

**规则：**
- 始终在 `types.ts` 中定义 `XxxInst` 接口
- 仅暴露可通过 ref 访问的实例方法（不暴露内部状态）
- `XxxInst` 类型使父组件能够正确类型化其 ref
