# 修改记录

## 2026-08-25 — description 措辞优化

重写 front-matter description：开头改为直接陈述规则的动词句，保留具体命名锚点，新增引号触发语示例与明确的负面边界。技能内容无改动。

### 变更清单

### 1. `SKILL.zh.md` → 修订 description

- 开头改为祈使式（“套用 JavaScript 编码规范：…”），替代原来的主题罗列名词短语。
- “使用时机”扩展：明确代码审查与变量命名提问场景，并给出示例“这个处理函数该叫什么”。
- 新增负面边界：不适用于 TypeScript 类型层设计或特定框架的组件规范。

### 2. `SKILL.md` → 同步

英文版同步以上修改，结构与中文版一致。

初始创建，基于 `src/javascript/AGENT.md` 生成 JavaScript 编码规范 skill。

## 变更清单

### 1. `SKILL.md` → 新建

从 `src/javascript/AGENT.md` 总结为 SKILL 格式，包含：
- 函数声明方式：始终使用 `function` 而非箭头函数
- 函数声明与调用顺序：入口函数在顶部，定义按调用顺序排列
- 事件处理函数命名：`on[Event]` 模式
- 数据转换函数命名：`to` / `as` / `parse` / `convert` 前缀约定
- 应避免的命名：`change`、`process`、`handle`、`doConvert`
