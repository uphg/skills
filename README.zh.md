# skills

[English](./README.md) | 中文

一个为 AI 编程助手量身定制的技能包集合，提供各种框架和工具的项目特定知识。

## 可用技能

### vue-tsx

使用 `defineComponent()` + `.tsx`（Composition API + TSX 渲染函数）的 Vue 3 开发。

```bash
npx skills add https://github.com/uphg/skills --skill vue-tsx
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
    └── references/                       # 参考文档
        └── ...
```

每个技能都放在 `skills/` 下各自的目录中，遵循相同的结构。

## 贡献

欢迎提交 issue 或 pull request 来添加新技能或改进现有技能。

**作者：** LvHeng
