---
name: frontend-arch
description: "前端架构方法论，提炼为四条久经考验的原则：层级间依赖只能向下、每个模块暴露显式 public API、先共置第二次用到才下沉、按业务域分组按目的命名。适用于从零设计前端项目的分层/模块结构、不停业务重构或迁移混乱的代码库、决定代码归属（API 请求、auth/token、类型、共享 UI、业务逻辑）、审查架构违规（循环依赖、深路径导入、utils 垃圾场），或回答‘这段代码该放哪个文件夹’、‘这该做成共享吗’类问题。"
---

# Frontend Arch（前端架构方法论）

一份前端架构方法论指南：让功能可被找到、模块可被替换、新人能预测代码的位置。提炼自分层架构实践--刻意保持最小化:约 30% 的结构成本,换取约 80% 的收益。

## 何时使用此技能

- 搭建新前端项目的目录结构
- 决定一段代码放哪：请求、token、类型、共享 UI、业务逻辑、布局
- 审查代码库的架构违规（循环依赖、深路径导入、大杂烩文件）
- 在不停业务的前提下增量重构混乱的代码库
- 回答"这该做成共享吗？" / "这是不是过度设计？"类问题

## 四条原则

### 1. 依赖只能向下
按职责范围把代码分成层级——`app`（装配：路由、Provider、全局样式）→ `pages`（页面组合）→ `features`（跨页复用的用户操作）→ `shared`（无业务语境的基础设施）。模块只能从严格更低的层导入，不能横向导入，更不能向上。

为什么：横向导入制造循环和隐性耦合；向上导入让模块无法独立抽取或测试。单向流动给你一张可以 lint、可以推理的有向无环图。

### 2. 每个模块暴露显式 public API
消费者只通过模块根部导入（带具名 re-export 的 `index.ts`），绝不深路径导入如 `features/cart/model/pricing.ts`。禁止通配符 re-export（`export *`）——它隐藏真实接口，还会意外泄漏内部实现。

为什么：有了 public API，内部文件可以随意重命名重构；每个导出都是一份兼容性承诺，所以只暴露消费者真正需要的东西。模块内部使用完整相对路径——通过自己的 index 导入自己会制造循环。

```js
// features/comments/index.ts
export { CommentCard } from "./ui/CommentCard";
export { fetchComments } from "./api/fetchComments";
```

### 3. 先共置，第二次用到才下沉
新逻辑放在唯一消费者的旁边——哪怕它"看起来可复用"。只有当第二个真实消费者出现时才提取到更低层。

为什么：预测中的复用通常是错的。过早下沉的单用途代码变成所有人都要维护的全局面积；之后再下沉比之后提升更难。越晚下沉，重构越安全。

### 4. 按业务域分组，按目的命名
服务同一业务领域的文件放在一起：`model/delivery.ts`，而不是混装 delivery 和 user 的 `types.ts`。文件夹回答的是"这是干什么的"，永远不是"这是什么类型的文件"。

为什么：内聚意味着每个模块只有一个业务理由变更。按本质命名的文件夹（`components/`、`hooks/`、`utils/`、`assets/`）把相关代码撒满目录树、膨胀搜索范围、还让不相关的领域混进同一个文件。

## 工作流

### 第一步：划分纵向切口
先列出应用的所有页面/屏幕——每一个成为一个模块文件夹（`pages/feed`、`pages/sign-in`）。屏幕是最自然的分解依据；结构在写任何代码之前就从屏幕里长出来。

### 第二步：确定层级下限
只从三层开始：`app`、`pages`、`shared`。当一个用户操作（连同其 UI）确实被多个页面复用时，再加 `features` 层。仅当客户端承载大量业务规则时，才在 features 下加业务逻辑层——薄客户端（后端承担逻辑）完全可以没有这一层。不要预建空层。

### 第三步：按表格放置代码

| 代码 | 归属 |
|---|---|
| CRUD 接口调用 | `shared/api/endpoints/<resource>` —— 仅单一消费者 → 放所属模块的 `api/` |
| 被 ≥2 处复用的业务规则 | 两个消费者的下一层 |
| Auth token / 会话数据 | `shared/api` 或独立的 `shared/auth` —— 绝不放进某个页面或单个 feature |
| 无业务语境的可复用 UI | `shared/ui`（内部不含业务逻辑） |
| 屏幕级组合 | `pages/<name>` |
| 新的 / 不稳定的逻辑 | 当前页面模块；稳定后再提升 |

请求、token、类型、资源、布局、状态的完整放置表 → 阅读 [references/placement.md](references/placement.md)。

### 第四步：接好 public API
每个模块一个 `index.ts`，只用具名 re-export。同模块导入：完整相对路径。跨模块导入：绝对别名 + 走 public API。`shared/ui` 出现 tree-shaking 问题时改为按组件建 index。

### 第五步：机械化执行
用 ESLint `no-restricted-imports` 规则、dependency-cruiser 或打包器边界规则把方向规则固化为 lint。没有 lint 兜底的约定在 deadline 面前必然溃败。CI 里跑 `npx madge --circular src` 抓循环依赖。

### 第六步：按触发条件生长
写代码时检查提升触发器：出现了第二个真实消费者 → 向下提取。两个模块互相导入 → 合并或下沉。其余情况什么都不动。

## 耦合处理手册

当模块 A 在同一层需要模块 B 时，按此顺序解决：

1. **总是同时变更？** → 合并为一个模块。
2. **共享部分是纯逻辑？** → 下沉一层；双方从下方合法导入。
3. **需要 B 的内容/UI？** → 在更高层用 render props / slot / DI 组合；A 与 B 保持互不相识。
4. **实在无法避免？** → 严格通过 B 的 public API 导入，并记录这个例外。

策略 1–3 消除耦合；策略 4 只是圈住它——保留的例外需要文档化和定期复审，因为不受管理的同层导入会漂移成双向依赖。

## Smell 与修复

| Smell | 修复 |
|---|---|
| 大杂烩文件（`utils.ts`、`helpers.ts`、`types.ts` 混装多领域） | 按领域拆分：`model/delivery.ts`、`model/user.ts` |
| 按本质命名的文件夹（`components`、`hooks`、`utils`、`assets`） | 按目的改名（`ui`、`model`、`api`） |
| index 里的 `export *` | 只用具名导出 |
| 深路径导入其他模块内部 | 改走它的 public API |
| 两个模块互相导入 | 合并，或把共享部分下沉 |
| 只有一个消费者的代码躺在 shared | 移回消费者旁边 |
| feature 按 UI 位置命名（`header`） | 按用户操作命名（`logout`） |

## References

- [references/placement.md](references/placement.md) —— 放置具体产物时阅读：API 请求、token/会话存储、类型共置、静态资源、布局、状态
- [references/restructuring.md](references/restructuring.md) —— 迁移既有混乱代码库时阅读：门控问题、增量阶段、遗留目录映射
- [references/audit.md](references/audit.md) —— 审查既有项目时阅读：检测违规的 grep 命令与修复优先级
