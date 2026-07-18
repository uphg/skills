---
name: project-layout
description: Vue 组件库的项目结构、hooks/utils 组织和编码风格规范
---

# 项目结构

## 目录结构

```
src/component/<component>/
├── index.ts                   # 对外导出（component、props、types）
├── <Component>.tsx            # defineComponent 组件实现（函数式）
├── inner-types.ts             # 内部类型/接口（可选）
├── types.ts                   # 公开类型、枚举常量数组
├── utils.ts                   # 组件专用工具函数（可选）
└── tests/
    ├── <Component>.spec.tsx   # Vitest 测试
    ├── ssr.spec.tsx           # SSR 兼容测试
    └── __snapshots__/         # 快照产物
```

**关键约定：**
- 组件实现文件：`PascalCase`（`Button.tsx`、`Input.tsx`）
- 目录：`kebab-case`（`button/`、`input/`、`date-picker/`）
- 子组件同级放置，不嵌套
- `index.ts` 重新导出组件、其 props 和公开类型

## 共享 Hooks（`src/hooks/`）

所有组合式函数位于 `src/hooks/`：

```
src/hooks/
├── useConfig.ts         # ConfigProvider 注入
├── useFormItem.ts       # FormItem 注入（size, disabled, status）
├── useLocale.ts         # 国际化
├── useRtl.ts            # RTL 方向
├── useDeferredTrue.ts   # 自定义辅助
├── useResize.ts         # 自定义辅助
└── ...
```

每个组合式函数遵循 `useXxx` 命名惯例。

## 共享工具函数（`src/utils/`）

保持 `src/utils/` **扁平化**——无二级子目录：

```
src/utils/
├── call.ts              # 安全调用回调
├── createInjectionKey.ts # 创建注入键
├── resolveSlot.ts       # Slot 解析辅助
├── resolveWrappedSlot.ts # Slot 包裹辅助
├── isSlotEmpty.ts       # Slot 为空检查
├── omit.ts / keep.ts    # 对象操作
├── warn.ts / warnOnce.ts # 开发警告
└── types.ts             # 通用类型工具
```

扁平布局避免了深层嵌套，使导入路径可预测。

## 编码风格

| 类别 | 规则 |
|---|---|
| **分号** | `false`（无分号） |
| **引号** | `singleQuote: true`（单引号） |
| **行宽** | `80` |
| **尾逗号** | `none`（无） |
| **Linter** | `@antfu/eslint-config` |
| **提交风格** | Angular 风格（`feat(button): add ghost prop`） |
| **响应式命名** | 语义前缀（`isDisabled`、`currentSize`） |
| **Ref 后缀** | 仅用于 DOM 模板引用（`selfElRef`、`inputRef`） |
| **Props 解构** | 禁止——使用 `props.xxx` 或 `toRefs` |
