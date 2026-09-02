# 更新日志

## 2026-09-02 — 为 evals.json 补充 expectations

现有 evals.json 的每条 eval 均补充 `expectations` 数组(可验证陈述),符合 skill-creator schema,可支持自动评分与基准运行。提示词与 expected_output 未改动。

## 2026-08-26

### 重命名为 `frontend-arch`

- 技能由 `frontend-boundaries` 重命名为 `frontend-arch`，定位升级为通用的前端架构方法论技能。
- 重写 frontmatter `description`：在原有放置与审查场景外，补充方法论级触发场景（从零设计分层/模块结构、不停业务重构或迁移遗留代码库）。
- 中文目录按仓库规范调整：`SKILL.md` → `SKILL.zh.md`，新增 `CHANGES.zh.md`。

## 2026-08-25

### 首次发布

- 创建 `frontend-boundaries`：基于四条原则的前端代码组织方法——依赖只能向下、显式 public API、先共置第二次用到才下沉、按业务域分组按目的命名。
- 正文保持决策表与耦合处理手册精简；具体产物放置细节（`references/placement.md`）、增量重构阶段（`references/restructuring.md`）、机械化违规检测（`references/audit.md`）放在 references 中。
- `evals.json` 新增三条测试提示词，覆盖结构重组、token 放置、交叉导入解决。
