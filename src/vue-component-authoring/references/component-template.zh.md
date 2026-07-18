---
name: component-template
description: Vue 3 组件库组件的完整 defineComponent + setup 模板及渲染函数
---

# 组件模板

## 基础模板

```tsx
// src/button/src/Button.tsx
import { defineComponent, computed, ref, toRefs } from 'vue'
import { useConfig, useFormItem } from '../../_mixins'
import { call } from '../../_utils/vue/call'
import { buttonProps } from './props'

export default defineComponent({
  name: 'Button',
  emits: {
    'update:value': (val: any) => true,
  },
  props: buttonProps,
  slots: Object as SlotsType<ButtonSlots>,

  setup(props, { slots, attrs, expose }) {
    // ===== DOM 引用（唯一带 Ref 后缀） =====
    const selfElRef = ref<HTMLElement | null>(null)

    // ===== Config / FormItem =====
    const formItem = useFormItem(props)
    const { disabled: formDisabled } = toRefs(formItem || {})

    // ===== 响应式状态（语义前缀） =====
    const isDisabled = computed(() => props.disabled || formDisabled?.value || false)
    const currentSize = computed(() => props.size || 'medium')

    // ===== 副作用执行 =====
    function doUpdateValue(val: string) {
      const { onUpdateValue } = props
      emit('update:value', val)
      if (onUpdateValue) call(onUpdateValue, val)
    }

    // ===== 暴露实例方法 =====
    function focus() { selfElRef.value?.focus() }
    expose({ focus })

    // ===== 渲染函数 =====
    return () => {
      const Component = props.tag || 'button'

      return (
        <Component
          ref={selfElRef}
          {...attrs}
          disabled={isDisabled.value}
        >
          {resolveWrappedSlot(slots.default, (children) => (
            <span>{children}</span>
          ))}
        </Component>
      )
    }
  },
})
```

## Setup 规则

1. **禁止 `this`** — 所有访问通过 setup 参数的 `props`、`slots`、`attrs`、`expose`
2. **禁止解构 props** — `const { size } = props` 会破坏响应式。使用 `props.size` 或 `toRefs` 包裹
3. **DOM 引用命名** — 始终使用 `XxxRef` 后缀（`selfElRef`、`inputRef`、`popoverRef`）
4. **计算属性命名** — 语义前缀（`isDisabled`、`currentSize`、`mergedClsPrefix`）
5. **代码组织** — 按以下分组：DOM 引用 → config/form → 状态 → 副作用 → expose → 渲染

## 渲染函数

- 始终从 setup 闭包返回 JSX/TSX 元素（或 fragment）
- 使用 `()` JSX 语法，不使用 `h()` 调用
- 条件渲染：`{condition && <Element />}`
- 列表渲染：`{items.map(item => <Element key={item.id} />)}`
- 在根元素展开 `{...attrs}`（参见测试与生命周期参考文档）
