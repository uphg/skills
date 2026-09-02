# 修改记录

## 2026-09-02 — 新增 evals.json

按 skill-dev Step 5 与 skill-creator schema 新增 3 条测试提示词(含 expected_output 与 expectations)。覆盖:全局状态(token)的 shared 归属、entity/widget/feature 分层决策、跨层 import 违规审查。SKILL.zh.md 内容无改动。

## 2026-04-30 — 模块化拆分

将主文档拆分为主文件 + references/ 模式，与 vue-tsx 技能保持一致。

### 变更清单

### 1. `SKILL.md` → 精简 (~158 行)

保留核心内容：何时使用、工作流程、架构层次总览表、核心约束、架构概念、决策流程、参考文件索引、禁止事项、不确定时的处理。

### 2. `references/layer-details.md` → 新建

从主文档中移出六层详细说明（shared / entities / features / widgets / pages / app），包含各层的目录结构、定位和特点。

### 3. `references/ecommerce-example.md` → 新建

从主文档中移出电商项目完整目录树案例。

### 4. `SKILL.zh.md` → 同步精简

中文版同步调整为相同结构，与英文版 section 完全对齐（158 行）。

### 5. `references/layer-details.zh.md` → 新建

各层详解的中文版。

### 6. `references/ecommerce-example.zh.md` → 新建

电商项目案例的中文版。

---

## 2026-04-28 — 初始版本

创建 FSD (Feature-Sliced Design) 前端架构设计 skill 文档。

### 变更清单

### 1. `SKILL.zh.md` → 新建

中文版 skill 文档，覆盖以下主题：
- 核心约束（单向依赖、公共 API、按业务分组、shared 纯净）
- 六层架构详解（app / pages / widgets / features / entities / shared）
- 切片与段的概念
- 电商项目案例
- 决策树：代码放哪里？
- 禁止事项

### 2. `SKILL.md` → 新建

英文版 skill 文档，由 SKILL.zh.md 翻译而来。
