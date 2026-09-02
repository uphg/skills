# AGENTS.md

本文件为 AI 编程助手（Claude Code、Cursor、Copilot 等）在使用本仓库代码时提供指导。

## 仓库概述

为 AI 编程助手精心策划的技能包集合，提供特定框架的知识和架构指导。这些技能遵循 Skills CLI 生态系统约定（参见 [skills.sh](https://skills.sh/)）。

## 项目结构

```
skills/
  <skill-name>/           # kebab-case 命名，可分发的技能包
    SKILL.md              # 英文技能定义（必需）
    GENERATION.md         # 来源与生成元数据
    CHANGES.md            # 修改变更日志
    evals.json            # 可选：2-3 条测试提示词与预期结果（见 skill-dev Step 5）
    references/           # 可选：详细参考文档
      ...
skills-zh/
  <skill-name>/           # 技能文档的中文版本
    SKILL.zh.md           # 中文技能定义，内容与 skills/<skill-name>/SKILL.md 相同
example/                  # 参考示例（已 gitignore），不属于本项目
```

## 中英文同步规则

- **英文版本**：`skills/<skill-name>/SKILL.md` — 可分发的规范技能文件。
- **中文版本**：`skills-zh/<skill-name>/SKILL.zh.md` — 面向中文用户，单独存放以保持 `skills/` 目录整洁。
- **修改时**：必须同时更新两个文件。内容必须在结构和语义上完全一致 — 相同的章节、相同的表格、相同的代码块、相同的示例。仅语言不同。

## 技能文档格式

`SKILL.md` 文件必须遵循以下格式：

```markdown
---
name: <skill-name>
description: "<一句话描述何时使用该技能，包括触发场景>"
---

# <技能标题>

<技能功能的单行概述。>

## 何时使用本技能

<触发本技能的具体场景的列表。>

## 工作流程

<描述代理应如何完成任务的分步说明：>

### 步骤 1：...

### 步骤 2：...

...

## <附加章节>

<快速参考表格、层级详情、代码示例、决策流程等。>

## 禁止事项

<代理绝不能做的事情。>

## 不确定时

<当代理无法确定答案时的回退行为。>
```

关键格式约定：
- 使用**表格**提供快速参考数据（概览、命名、映射）。
- 在工作流程章节使用**编号步骤**提供流程指导。
- 使用**代码块**展示目录树和命令。
- 使用**列表**描述场景和约束。
- 保持 front matter 中的 `description` 简洁 — 它决定技能何时被激活。

**参考型技能**（如 `vue-tsx` 这类速查型技能）可用主题表格 + `references/` 指针替代 Workflow 骨架，但触发信息必须保留在 frontmatter `description` 中。

## 命名约定

- **技能目录**：`kebab-case`（例如 `vue-tsx`、`find-skills`）
- **SKILL.md**：始终大写，始终使用此确切文件名
- **SKILL.zh.md**：始终大写前缀，始终使用此确切文件名
- **GENERATION.md**：始终大写，始终使用此确切文件名
- **CHANGES.md**：始终大写，始终使用此确切文件名
- **参考文件**：位于 `references/` 下，使用 `kebab-case` 或描述性名称

## 上下文效率指南

技能按需加载 — 代理在启动时只能看到技能名称和描述。只有当代理判定技能相关时，完整的 `SKILL.md` 才会被读入上下文。为最小化上下文：

- **保持 SKILL.md 在 500 行以内** — 将详细的参考材料放在 `references/` 中
- **编写具体的描述** — 帮助代理准确判断何时激活
- **渐进式披露** — 链接到仅在需要时读取的参考文件
- **关键信息前置** — 将最重要的指导放在前面

## 在本仓库中使用技能

### 添加新技能

1. 创建 `skills/<skill-name>/`，包含：
   - `SKILL.md`（英文，遵循上述格式）
   - `GENERATION.md`（元数据：来源、git SHA、生成日期）
   - `CHANGES.md`（中文变更日志）
   - `references/`（可选，用于补充文档）
   - `evals.json`（可选但推荐：2–3 条真实测试提示词，含 `expected_output` 与 `expectations`，见 skill-dev Step 5）
2. 创建 `skills-zh/<skill-name>/`，包含：
   - `SKILL.zh.md`（中文，与英文版本结构一致）
3. 更新 `README.md` 和 `README.zh.md`，添加新技能条目和安装命令。

### 修改现有技能

1. 首先编辑 `skills/<skill-name>/SKILL.md`（英文）。
2. 将所有更改同步到 `skills-zh/<skill-name>/SKILL.zh.md`（中文）。
3. 更新 `skills/<skill-name>/CHANGES.md`，记录修改。
4. 如果技能的范围、描述或安装指令发生变化，更新 `README.md` 和 `README.zh.md`。

### 验证一致性

修改技能后，比较章节标题以确保两个语言版本对齐：

```bash
grep -n '^##' skills/<skill-name>/SKILL.md
grep -n '^##' skills-zh/<skill-name>/SKILL.zh.md
```

行数和章节编号应该匹配。

## 当前技能

| 技能 | 描述 | 目录 |
|---|---|---|
| skill-dev | 技能包创作方法论（结构、description 优化、evals、审查清单） | `skills/skill-dev/` |
| fsd | Feature-Sliced Design 前端架构 | `skills/fsd/` |
| frontend-arch | 前端架构原则 + 增量重构指南 | `skills/frontend-arch/` |
| imperative-commits | Git 提交标题祈使语气，统一执行 | `skills/imperative-commits/` |
| js-coding-style | JavaScript 编码风格：声明方式、调用顺序、命名模式 | `skills/js-coding-style/` |
| markdown-style | Markdown 排版与格式化（支持 CJK） | `skills/markdown-style/` |
| vue-component-authoring | Vue 组件库组件书写规范 | `skills/vue-component-authoring/` |
| vue-tsx | Vue 3 Composition API + TSX 开发 | `skills/vue-tsx/` |

## 构建 / 测试 / 代码检查

本仓库没有构建流程、测试套件或代码检查配置。它是一个纯文档/技能仓库。验证是手动的 — 检查渲染后的 markdown 并确认各语言版本之间的结构一致性。