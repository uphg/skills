---
name: component-template
description: Full defineComponent + setup template with render function for Vue 3 component library components
---

# Component Template

## Base Template

```tsx
// src/button/src/Button.tsx
import { defineComponent, computed, ref, toRefs } from 'vue'
import { useConfig, useFormItem } from '../../_mixins'
import { buttonProps } from './props'

export default defineComponent({
  name: 'Button',
  props: buttonProps,
  emits: {
    click: (e: MouseEvent) => true,
    'update:value': (val: any) => true,
  },
  slots: Object as SlotsType<ButtonSlots>,

  setup(props, { emit, slots, attrs, expose }) {
    // ===== DOM ref (only one with Ref suffix) =====
    const selfElRef = ref<HTMLElement | null>(null)

    // ===== Config / FormItem =====
    const formItem = useFormItem(props)
    const { disabled: formDisabled } = toRefs(formItem || {})

    // ===== Reactive state (semantic prefixes) =====
    const isDisabled = computed(() => props.disabled || formDisabled?.value || false)
    const currentSize = computed(() => props.size || 'medium')

    // ===== Side effect execution =====
    function handleClick(e: MouseEvent) {
      emit('click', e)
    }

    function doUpdateValue(val: string) {
      emit('update:value', val)
      props.onUpdateValue?.(val)
    }

    // ===== Expose instance methods =====
    function focus() { selfElRef.value?.focus() }
    expose({ focus })

    // ===== Render function =====
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

## Setup Rules

1. **No `this`** — all access through `props`, `slots`, `attrs`, `expose` from setup parameters
2. **No props destructuring** — `const { size } = props` breaks reactivity. Use `props.size` or wrap with `toRefs`
3. **DOM ref naming** — always use `XxxRef` suffix (`selfElRef`, `inputRef`, `popoverRef`)
4. **Computed naming** — semantic prefix (`isDisabled`, `currentSize`, `mergedClsPrefix`)
5. **Code organization** — group by: DOM refs → config/form → state → effects → expose → render

## Render Function

- Always return a JSX/TSX element (or fragment) from the setup closure
- Use `()` JSX syntax, not `h()` calls
- For conditional rendering: `{condition && <Element />}`
- For lists: `{items.map(item => <Element key={item.id} />)}`
- Spread `{...attrs}` on the root element (see testing-and-lifecycle reference)
