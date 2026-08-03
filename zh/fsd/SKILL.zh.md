---
name: fsd
description: "当用户需要按照 Feature-Sliced Design (FSD) 方法论来设计、组织或重构前端项目架构时使用此技能。具体场景包括：创建符合 FSD 规范的目录结构、决定某段代码应该放在哪个层（app/pages/widgets/features/entities/shared）、检查代码是否违反单向依赖或公共 API 规则、将现有项目重构为 FSD 架构。不适用于：编写独立组件、讨论非 FSD 的架构方案、或与前端架构无关的问题。"
---

# 前端 FSD 架构专家

此技能帮助您按照 Feature-Sliced Design (FSD) 方法论设计、组织或重构前端项目架构。

## 何时使用此技能

当用户出现以下情况时使用此技能：

- 要求创建或规划符合 FSD 规范的前端项目目录结构
- 询问某段代码应该放在 FSD 架构的哪个层（app/pages/widgets/features/entities/shared）
- 需要审查现有代码是否违反 FSD 的单向依赖或公共 API 规则
- 希望将现有项目从非 FSD 架构逐步迁移到 FSD
- 对 FSD 的层、切片、段等概念有疑问
- 需要判断某个业务模块应属于 feature 还是 entity
- 设计跨页面复用的 UI 大块时，不确定应放在 widget 还是 feature

## 工作流程

### 步骤 1：了解用户项目上下文

确定以下关键信息：

1. **项目类型**：单页应用 / 多页应用 / 微前端？（React / Vue / Next.js / 其他框架？）
2. **当前状态**：从零开始？还是已有代码需要重构？
3. **业务领域**：电商？内容管理？社交？企业后台？
4. **技术栈**：状态管理方案、路由方案、构建工具

### 步骤 2：识别业务实体

在输出目录结构之前，先梳理项目的核心业务概念：

- 找出项目中的关键"名词" → 这些就是 entity 候选（如：用户、商品、订单、文章）
- 列出每个实体的核心数据模型字段
- 判断实体之间的关系（关联、聚合等）

### 步骤 3：分析用户交互场景

梳理用户关心的交互功能：

- 找出项目中的关键"动词/场景" → 这些就是 feature 候选（如：登录注册、搜索、加入购物车、评论）
- 判断各 feature 复用了哪些 entity
- 评估 feature 的复用价值（跨页面使用 → 独立 feature；单页面一步操作 → 可合并到 entity）

### 步骤 4：设计目录结构

按照 FSD 层次从下往上设计目录结构：

1. **shared** — 先确定通用基础设施（UI 组件库、API 客户端、工具函数、环境配置）
2. **entities** — 按业务实体创建 slice，每个 entity 下划分 ui/model/api/lib 段
3. **features** — 按用户场景创建 slice，确保通过 entity 的公共 API 访问数据
4. **widgets** — 识别跨页面复用的 UI 大块，组合 features 和 entities
5. **pages** — 为每个路由创建页面，组合 widgets
6. **app** — 配置全局路由、store、样式、providers

输出时使用完整的目录树，让用户清晰看到每个文件和文件夹的位置。

### 步骤 5：检查架构约束

设计完成后，检查是否违反 FSD 核心约束：

1. **单向依赖检查**：逐层确认 app → pages → widgets → features → entities → shared，下层不能依赖上层
2. **公共 API 检查**：每个 slice 是否通过 `index.ts` 暴露接口？是否存在直接导入 slice 内部文件的路径？
3. **shared 纯净性检查**：shared 层是否包含任何业务逻辑或对其他层的依赖？
4. **循环依赖检查**：是否存在跨 slice 的循环引用？

如发现问题，给出具体的修正方案。

### 步骤 6：提供决策指导

当用户有具体代码需要归档时，按照决策树帮助用户判断代码归属。

## 架构层次总览

| 层 | 定位 | 典型内容 | 依赖方向 |
|---|---|---|---|
| app | 应用运行所需的基础设施 | 路由配置、全局 store、全局样式、providers | → 可依赖所有下层 |
| pages | 用户访问的完整页面 | 页面组件、页面级布局 | → 依赖 widgets、features、entities |
| widgets | 自包含的大型 UI 块 | 顶部导航、侧边栏、商品网格 | → 依赖 features、entities |
| features | 用户关心的完整交互场景 | 登录表单、搜索功能、加入购物车 | → 依赖 entities |
| entities | 核心业务概念模型 | 用户数据模型、商品 API、订单类型 | → 依赖 shared |
| shared | 与业务无关的通用基础设施 | UI 组件库、工具函数、API 客户端 | → 不依赖任何上层 |

## 核心约束

1. **单向依赖**：app → pages → widgets → features → entities → shared，下层绝不能依赖上层
2. **公共 API**：每个 slice 通过 `index.ts` 暴露接口，禁止直接从 slice 内部导入
3. **按业务分组**：按业务领域（auth/cart/user）组织，而非按技术类型（components/hooks/utils）
4. **shared 纯净**：只放与业务无关的通用代码，不依赖其他层

## 架构概念

### slice（切片）

每个业务层内部按业务模块划分：

```
pages/            → pages/home、pages/profile
widgets/          → widgets/header、widgets/sidebar
features/         → features/auth、features/comment-form
entities/         → entities/user、entities/post
```

### segment（段）

每个 slice 内部按技术角色划分：

| 段 | 职责 | 示例 |
|---|---|---|
| `ui/` | UI 组件 | 按钮、表单、卡片 |
| `model/` | 状态管理、类型定义 | Redux store、TypeScript 接口 |
| `api/` | API 请求封装 | REST 调用、GraphQL query |
| `lib/` | 辅助函数、工具逻辑 | 格式化、校验、计算 |
| `config/` | 配置项 | 常量、环境变量映射 |

## 决策流程：代码放哪里？

判断某段代码应该放在哪个层时，按以下顺序进行决策：

1. 是全局配置/路由/store？ → `app/`
2. 是完整页面（对应一个路由）？ → `pages/{slice}/ui/`
3. 是独立的大型 UI 块（跨页面复用，组合多个 feature）？ → `widgets/{slice}/ui/`
4. 是完整的用户交互场景（有独立业务价值）？ → `features/{slice}/ui/`
5. 是核心业务概念的数据模型？ → `entities/{slice}/model/`
6. 是对核心业务概念的 API 请求？ → `entities/{slice}/api/`
7. 是核心业务概念的展示组件？ → `entities/{slice}/ui/`
8. 以上都不是？ → `shared/{ui|lib|api|config}/`

## 参考文件

需要详细了解时可阅读以下参考文件：

| 主题 | 说明 | 文件 |
|---|---|---|
| 各层详解 | 六个层的详细说明，含目录结构、定位和特点 | [layer-details](references/layer-details.zh.md) |
| 电商案例 | 符合 FSD 规范的电商项目完整目录树 | [ecommerce-example](references/ecommerce-example.zh.md) |

## 禁止事项

- 不要违反单向依赖（下层导入上层）
- 不要绕过公共 API 直接从 slice 内部导入文件
- 不要在 shared 层放置业务逻辑或业务类型
- 不要创建跨 slice 的循环依赖
- 不要按技术类型（components/hooks/utils）组织目录结构

## 不确定时的处理策略

如果无法确定代码应该放在哪个层：

1. 优先放在更具体的层（entities 优于 shared，features 优于 widgets）
2. 如果某段代码只在一个 feature 中使用且没有复用价值，可以保留在 feature 内部
3. 如果某段代码被多个 feature 使用但包含业务语义，应提取到 entity
4. 如果代码完全不包含业务语义且被广泛使用，才放入 shared
5. 给出两个候选方案并说明各自优劣，让用户做出最终决定
