---
name: js-coding-style
description: "应用 JavaScript 编码风格约定:优先使用 function 声明而非 const 箭头函数,按调用顺序组织函数,事件处理器用 on[Event] 命名,数据转换用 to/parse/convert,异步函数加 Async 后缀,API 请求用 api+动词,持久化存储用 read/write,缓存访问用 get/set,常量用 UPPER_SNAKE_CASE,布尔值用 is/has 前缀。在编写或审查 JavaScript 代码、为函数/变量/常量/文件命名(\"这个 handler 该叫什么?\"),或制定团队编码规范时使用。不适用于 TypeScript 类型层设计或框架特定的组件约定。"
metadata:
  author: LvHeng
  version: "2026.9.2"
  source: Generated from src/javascript/AGENT.md
---

# JavaScript 编码风格

始终使用 `function` 声明,不使用赋值给 `const` 的箭头函数。在顶部调用入口函数,再按调用顺序从上到下定义函数。事件处理器、异步函数、常量和数据转换遵循统一的命名模式。

## 何时使用此技能

在以下场景使用:

- 编写新的 JavaScript 代码
- 为 JavaScript 项目中的函数、变量、常量或文件命名
- 审查代码的命名与风格一致性
- 制定或推行 JavaScript 编码规范
- 重构以提升命名清晰度

## 函数结构

### 声明方式

始终使用 `function` 关键字声明。不要使用赋值给 `const` 的箭头函数。

```javascript
// ✅ 正确
function fn() {}

// ❌ 错误
const fn = () => {}
```

### 调用顺序

先在顶部调用入口函数,再按调用顺序从上到下定义函数。

这是一条刻意的阅读顺序约定:读者应先看到"程序做什么",再看到"每一步怎么做"。它是本代码库的风格选择,而非通用最佳实践——在新代码和重构中应用即可,不要机械地改写第三方代码或已有既定布局的文件。

```javascript
mount()  // 入口函数(mount / load / init / main / setup 等)

function mount() {
  readConfig()
  fetchData()
}

function readConfig() { }
function fetchData() { }
```

## 命名约定

### 函数

按函数的用途选择命名模式:

| 场景 | 模式 | 示例 |
|---|---|---|
| 事件处理器 | `on[Event]` | `onClick()`、`onSubmit()`、`onButtonClick()` |
| 类型转换 | `to` + 类型 | `toNumber()`、`toInt()`、`toPercentage()` |
| 字符串解析 | `parse` + 类型 | `parseInt()`、`parseDate()` |
| 复杂转换 | `convert` + 模式 | `convertToPascalCase(str)`、`convertUnits(value, from, to)` |
| 异步函数 | 动词 + `Async` | `initSettingsAsync()`、`loadLocaleAsync()` |
| API 请求 | `api` + 动词 | `apiGetUser()`、`apiDeleteOrder()` |
| 持久化存储 | `read` / `write` | `readSettingsFromStorage()`、`writeSettingsToStorage()` |
| 内存缓存、应用状态 | `get` / `set` | `getCachedUser()`、`setCachedUser()` |

消歧规则:

- `Async` 标记任何返回 Promise 的函数——动词说明"做什么",后缀提醒调用方需要 `await`。
- `api` 本身已隐含网络请求,API 请求用 `api` + 动词,且永不加 `Async` 后缀——不用 `apiGetUserAsync`,也不用泛用前缀替代后缀(`asyncFetch`、`doRequest`)。
- `read`/`write` 与 `get`/`set` 按数据所在位置选择:持久化存储(localStorage、文件、DB)用 `read`/`write`,内存缓存与应用状态用 `get`/`set`。

### 变量与常量

使用前缀和命名模式,让变量的类型和意图一目了然:

| 类别 | 约定 | 示例 | 说明 |
|------|------|------|------|
| 布尔值 | `is`/`has` 前缀或描述性形容词 | `isLoading`、`hasError`、`modalVisible` | 前缀或自然蕴含真/假的描述性名称 |
| 数字/字符串 | `current`/`raw` 前缀 | `currentPage`、`rawText` | `current` 表示有状态值,`raw` 表示未加工数据 |
| 数组 | 复数名词 | `users`、`items`、`configs` | 复数形式表示集合 |
| 单个对象 | 单数名词 | `user`、`item`、`config` | 单数形式表示单个实体 |

常量命名:

| 形式 | 命名 | 示例 |
|---|---|---|
| 模块级不可变值 | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT`、`DEFAULT_TIMEOUT` |
| 函数内常量及所有变量 | 小驼峰 | `maxRetries` |

### 文件

文件和目录名使用 `kebab-case`:`api-user.js`、`date-utils.js`。

### 应避免的命名

- ❌ `change()` — 太模糊
- ❌ `process()` — 不清楚做了什么
- ❌ `handle()` — 不够具体
- ❌ `doConvert()` — 冗余前缀

## 核心原则

> **命名要让调用者一眼看出输入是什么、输出是什么。**
