# 修改记录

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
