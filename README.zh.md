# skills

[English](./README.md) | 中文

一个为 AI 编程助手量身定制的技能包集合，提供各种框架和工具的项目特定知识。

## 可用 skills

### skill-dev

技能包的创建、审查与改进方法论：意图捕获、包结构设计（references/、scripts/、assets/）、description 触发优化、测试提示词、质量审查清单与写作风格指导。

```bash
npx skills add https://github.com/xypur/skills --skill skill-dev
```

### fsd

遵循 Feature-Sliced Design (FSD) 方法论的前端架构设计。

```bash
npx skills add https://github.com/xypur/skills --skill fsd
```

### frontend-arch

前端架构方法论，凝练为四条原则：依赖只向下、模块显式公共 API、先同位置后提升、按领域分组命名。附增量式旧项目重构指南与架构审查清单。

```bash
npx skills add https://github.com/xypur/skills --skill frontend-arch
```

### vue-tsx

使用 `defineComponent()` + `.tsx`（Composition API + TSX 渲染函数）的 Vue 3 开发。

```bash
npx skills add https://github.com/xypur/skills --skill vue-tsx
```

### imperative-commits

强制 Git 提交标题使用祈使语气：首字母大写、动词原形、不以句号结尾——无论仓库历史如何，对所有提交统一执行。

```bash
npx skills add https://github.com/xypur/skills --skill imperative-commits
```

### js-coding-style

JavaScript 编码风格约定：函数声明方式、调用顺序、事件处理命名，以及数据转换、异步函数、常量和文件的命名模式。

```bash
npx skills add https://github.com/xypur/skills --skill js-coding-style
```

### vue-component-authoring

Vue 3 组件库组件书写规范：目录结构、props/emits/slots/expose API 设计、emits + callback props for JSX、const 数组枚举治理、副作用清理、attrs 透传、测试以及 hooks/utils 组织。

```bash
npx skills add https://github.com/xypur/skills --skill vue-component-authoring
```

### markdown-style

Markdown 文档格式化与风格优化，支持中文、英文及中英混合文档。涵盖代码/术语格式化、CJK 空格规范、排版细节、语言润色以及专业排版标准。

```bash
npx skills add https://github.com/xypur/skills --skill markdown-style
```

## 使用方法

本仓库专为支持"技能包"系统的 AI 编程工具（如 Cursor、Windsurf 等）设计。将 `skills/` 目录指向你的 AI 助手，即可为其提供代码生成的相关语境指导。

## 目录结构

```
skills/
└── <skill-name>/
    ├── SKILL.md                          # 主要技能定义与编码偏好
    ├── GENERATION.md                     # 来源与生成元数据
    ├── CHANGES.md                        # 修改日志
    ├── evals.json                        # 可选：测试提示词与预期结果
    └── references/                       # 参考文档
        └── ...
```

每个技能都放在 `skills/` 下各自的目录中，遵循相同的结构。

## 贡献

欢迎提交 issue 或 pull request 来改进现有技能。

## 许可证

[MIT](./LICENSE)

**作者：** LvHeng
