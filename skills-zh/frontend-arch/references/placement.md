# 放置指南 —— 这段代码放哪？

按产物类型划分的完整决策表。所有条目遵循同一条原则：与使用处共置，出现真实复用再提升。放置由**作用域和职责**决定，与结构复杂度无关。

## 目录
- [API 请求](#api-请求)
- [Auth token 与会话](#auth-token-与会话)
- [类型](#类型)
- [静态资源](#静态资源)
- [布局](#布局)
- [状态](#状态)

## API 请求

| 请求服务于 | 归属 |
|---|---|
| 应用的多个部分 | `shared/api` —— `client.ts`（base URL、headers、序列化）、`endpoints/<resource>.ts`、公开的 `index.ts` |
| 恰好一个页面/模块 | 该模块的 `api/`；无需经过它的 public API 导出 |

规则：
- 请求函数 + DTO 接口 + mapper 保持为一个内聚单元——它们总是一起变更。
- 不要过早把 API 调用挪进业务逻辑层：后端响应形状通常与前端需要的不一致；在 `shared/api` 或模块的 `api/` 里做转换。
- 校验 schema 与触发点共置：后端数据 schema 放请求旁边；表单输入 schema 放表单旁边。

## Auth token 与会话

token 是被多个认证流程使用的应用级状态——如果两个不同模块都需要它，它就不属于其中任何一方。

| 方案 | 归属 | 权衡 |
|---|---|---|
| Cookie 会话 | 仅服务端基础设施 | 客户端无需架构——可能时优先选择 |
| api 模块内的客户端状态 | `shared/api` 内的响应式 store | 最简单；加自动刷新中间件（401 过期时：刷新 → 重试原请求） |
| 独立 auth 模块 | `shared/api` + `shared/auth` | 复杂的刷新/token 轮换逻辑首选 |
| 会话实体 | features 下方的业务层（`model` store） | 承载丰富的当前用户逻辑；需要一个暴露模式触达 `shared/api` |

把更高层的 token 暴露给更低层 api 客户端的三种标准模式：
1. 每次请求手动传入——最简单，繁琐。
2. Context / 全局 store"拉取"——声明式；key 保存在 `shared/api`。
3. 响应式"推送"——订阅会话 store，变化时更新客户端。

只要 refresh token 被拒就触发自动登出——即使服务端登出调用失败，也一律重置本地 token/用户状态。

## 类型

类型没有任何特殊地位：每个类型都按使用位置和目的放置，绝不进 `types` 大杂烩。

| 类型 | 归属 |
|---|---|
| 工具类型 | `shared/lib/utility-types`（+ README 说明范围）或唯一消费者旁边 |
| DTO 与 mapper | 紧挨使用它们的请求函数 |
| 枚举 | 靠近使用处；文件夹 = 它代表的东西（设计 token/toast 位置 → `ui`，状态码 → `api`） |
| Props / context 接口 | 与组件同一文件，或同目录的兄弟文件 |
| 环境声明 `*.d.ts` | `app/ambient/`；无类型包的补丁 → `shared/lib/untyped-packages/<lib>.d.ts` |
| 生成的类型（OpenAPI） | 专用目录如 `shared/api/openapi` + 再生成说明 README |
| 应用级类型（Redux `RootState`） | 在顶层全局声明，让低层的 typed hooks 无需向上导入即可使用 |

两个业务类型跨模块互相引用时（Song → Artist）：能干净分离就用泛型参数化（`interface Song<A extends { id: string }>`）；否则通过显式的跨模块 public API 文件 re-export——绝不用隐藏的 middleware 中转。

## 静态资源

按用例分组，紧贴消费代码。一刀切的 per-slice 或全局 `assets/` 文件夹是按种类分组，破坏内聚。

| 资源 | 归属 |
|---|---|
| 模块专属图片/图标 | 模块内部 |
| 可复用图标/图片 | `shared/ui/` |
| 全局样式、字体 | 顶层（`app/styles`、`app/fonts`） |
| favicon、未处理的静态文件 | 打包器 `public/` 根目录 |

## 布局

按作用域和职责决定放置：

| 作用域 | 归属 |
|---|---|
| 整个应用 / 路由外壳 | 顶层（`app`） |
| 特定页面或路由组 | 该页面模块 |
| 无语境的可复用结构 | `shared/ui` |
| 跨页复用的流程型布局 | 对应的 feature 模块 |

共享布局不得向上导入动态内容——用 render props/slot 注入，或在上方配置路由嵌套，或直接定义在页面里。

## 状态

| 种类 | 归属 |
|---|---|
| 服务端缓存 | query 层（`shared/api` 工厂或所属模块）——query-factory 模式：层级 key `all → lists → list`，叶子返回 `queryOptions({queryKey, queryFn})` |
| URL 可表达的视图状态（筛选、tab、分页） | URL search params——浏览器负责持久化；往往可以完全省掉客户端状态 |
| 单页 UI 状态 | 该页面组件内部 |
| 跨 feature 应用状态（auth、主题） | 低层的专用模块 |
| Mutations | 与触发的 feature/页面共置；在 `onSuccess` 里更新缓存 |
