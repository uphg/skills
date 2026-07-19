---
name: project-layout
description: Vue 组件库的项目结构、hooks/utils 组织规范
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

**建议约定：**
- 组件实现文件：`PascalCase`（`Button.tsx`、`Input.tsx`）
- 目录：`kebab-case`（`button/`、`input/`、`date-picker/`）
- 子组件同级放置，不嵌套
- `index.ts` 重新导出组件、其 props 和公开类型

以上目录布局仅供参考——可根据项目惯例调整。

## 共享 Hooks（`src/hooks/`）

所有组合式函数位于 `src/hooks/`。每个组合式函数遵循 `useXxx` 命名惯例。

## 共享工具函数（`src/utils/`）

将共享工具函数放在 `src/utils/` 中。按需组织分类。
