# 修改记录

## 2026-08-24

### 泛化为公共技能

- 移除仓库专属约定：删除双语同步相关内容（触发场景、Step 7 步骤、快速参考行、Prohibitions 条目及审查清单第 6 节），双语同步为当前仓库独有约定。
- 包路径泛化：技能目录不再限定 `skills/<skill-name>/`，改为通用的 `<skill-name>/` 自包含目录，父目录取决于宿主环境；`GENERATION.md` / `CHANGES.md` 改为“宿主有此约定时遵循”。
- description 与“何时使用”相应更新，中英文版及两份审查清单同步修改。

### 补充中文版 references 文档

- 新增 `zh/skill-dev/references/review-checklist.zh.md` 与 `zh/skill-dev/references/writing-guide.zh.md`，与英文版 `references/` 内容对应。
- `SKILL.zh.md` Step 6 中的参考文件链接改为指向中文版参考文件。

### 新建 skills/skill-dev

创建技能开发方法论技能包，提炼出一套通用的技能编写规范：

- `SKILL.md`：英文规范定义。核心内容包括：
  - 四条核心原则（渐进披露三层模型、"解释 why 优于堆砌 MUST"、泛化而非过拟合、确定性步骤脚本化）
  - 七步 Workflow：意图捕获 → 包结构设计（含 `scripts/`、`assets/` 资源分层）→ frontmatter/description 写法 → 正文写作 → 测试提示词（evals.json 格式）→ 审查清单 → 集成收尾
  - Prohibitions / When Unsure 段落按需保留，不作为强制骨架
- `references/review-checklist.md`：发布前质量审查清单（触发、渐进披露、写作风格、结构、评估、双语同步、仓库集成七组检查项）
- `references/writing-guide.md`：description 触发优化方法论（should/should-not-trigger 查询设计、near-miss 反例）与正文写作风格指南
- `GENERATION.md`：来源与生成元数据

同时创建中文版 `zh/skill-dev/SKILL.zh.md`，结构内容与英文版保持一致。
