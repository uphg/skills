[English](./README.md)

# vue-skills

一个为 AI 编程助手量身定制的技能包，提供 **Vue 3** 使用 **`defineComponent()` + `.tsx`**（Composition API + TSX 渲染函数）开发的项目特定知识。

## 安装

```bash
npx skills add https://github.com/uphg/vue-skills --skill vue-skills
```

## 使用方法

本仓库专为支持"技能包"系统的 AI 编程工具（如 Cursor、Windsurf 等）设计。将 `skills/` 目录指向你的 AI 助手，即可为其提供 Vue 3 + TSX 代码生成的相关语境指导。

## 目录结构

```
skills/vue-tsx/
├── SKILL.md                          # 主要技能定义与编码偏好
├── GENERATION.md                     # 来源与生成元数据
├── CHANGES.md                        # 修改日志
└── references/
    ├── define-component-tsx.md       # defineComponent() + TSX 模式
    ├── core-new-apis.md              # 响应式、生命周期、组合式函数
    └── advanced-patterns.md          # 内置组件与指令在 TSX 中的使用
```

## 内容

- **SKILL.md** — 声明首选编码风格：TypeScript、`defineComponent()`、`shallowRef`、Composition API，并提供规范的组件模板。
- **define-component-tsx.md** — 涵盖函数签名与选项签名、props/emits 声明、泛型组件、`defineAsyncComponent` 以及 TSX 中的自定义指令。
- **core-new-apis.md** — `ref`、`shallowRef`、`computed`、`reactive`、`watch`、`watchEffect`、生命周期钩子、`effectScope` 和组合式函数模式的深度参考。
- **advanced-patterns.md** — `Transition`、`Teleport`、`Suspense`、`KeepAlive` 以及自定义指令的 TSX 等价写法。

## 来源

从 [官方 Vue.js 文档](https://github.com/vuejs/docs) 使用 [skills](https://github.com/antfu/skills) 工具生成，随后针对 `defineComponent()` + `.tsx` 工作流进行了定制。

**作者：** LvHeng
**版本：** 2026.4.27
