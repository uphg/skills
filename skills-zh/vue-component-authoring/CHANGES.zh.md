# 修改记录

## 2026-08-25 — description 措辞优化

将术语堆叠的覆盖清单压缩为代表性锚点，开头改为动词引导，在保持触发匹配价值的同时让 description 更易扫读。技能内容无改动——从 description 中移除的主题（const 数组枚举治理、副作用清理、attrs 透传、hooks/utils 组织、编码风格）仍在正文完整覆盖。

### 变更清单

### 1. `SKILL.zh.md` → 修订 description

- 开头改为动词引导：“使用 defineComponent + TSX 为组件库编写可复用的 Vue 3 组件”。
- “使用时机”扩展：构建、审查或重构组件库组件。
- 覆盖清单压缩为 API 设计锚点（emits 选项 + emit() 分发配合 callback props、SlotsType 类型声明、expose + XxxInst 接口）以及目录/测试组织（Vitest + @vue/test-utils）。
- 负面边界实质不变，措辞收紧为“一次性应用组件”。

### 2. `SKILL.md` → 同步

英文版同步以上修改，结构与中文版一致。

## 2026-07-18 — 初始版本

创建 Vue 组件库组件编写规范 skill（根据 `src/vue-component-authoring/README.md` 原始文档）。
原名为 `vue-style-guide`，后更名为 `vue-component-authoring`。

### 修改文件清单

- `SKILL.md` / `SKILL.zh.md` — 主文档
- `references/component-template.md` / `.zh.md` — 组件模板
- `references/api-design.md` / `.zh.md` — API 设计
- `references/testing-and-lifecycle.md` / `.zh.md` — 测试与生命周期
- `README.md` — 源文档

---

## 2026-07-19 — Emit 模式修正

将 Emit 模式从 callback-props + `call()` 辅助函数改为 Vue 3 官方 `emits` 选项 + `emit()` 分发。

### 修改文件清单

- `SKILL.md` — 英文技能主文档（canonical）
- `SKILL.zh.md` — 中文技能主文档
- `GENERATION.md` — 生成元数据
- `references/project-layout.md` / `.zh.md` — 项目结构 + hooks/utils + 编码风格
- `references/component-template.md` / `.zh.md` — defineComponent 实现模板
- `references/api-design.md` / `.zh.md` — Props / 类型 / Emits / Slots / Expose
- `references/testing-and-lifecycle.md` / `.zh.md` — 副作用清理 + attrs + 测试

---

## 2026-07-19 — Emit 模式补充：Callback Props

每个 `update:xxx` v-model 事件添加对应的 callback prop（如 `onUpdateValue` 对应 `update:value`），方便父组件在 JSX 中使用。移除 Prohibitions 中关于 `onXxx` 的禁止条目。

### 修改文件清单

- `SKILL.md` — 英文技能主文档
- `SKILL.zh.md` — 中文技能主文档
- `references/api-design.md` / `.zh.md` — Emits 章节
- `references/component-template.md` / `.zh.md` — 基础模板
- `CHANGES.md` — 本文件

---

## 2026-07-19 — 精简规范：移除过于死板的限制

放宽目录结构为参考布局（非强制），移除 lint/format/commit 等与组件编写无关的编码风格条目，SSR 测试改为条件性要求，去掉 `src/utils/` 扁平化强制约束。

### 修改文件清单

- `SKILL.md` / `SKILL.zh.md` — 主文档 Step 1、Step 11、Prohibitions、When Unsure
- `references/project-layout.md` / `.zh.md` — 移除具体文件列表和编码风格表格
- `references/testing-and-lifecycle.md` / `.zh.md` — SSR 测试改为条件性要求
- `CHANGES.md` — 本文件
