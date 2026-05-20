---
name: javascript
description: JavaScript 编码规范，涵盖函数声明方式（function vs 箭头函数）、函数调用顺序、事件处理函数命名（on[事件]）以及数据转换函数命名模式（to/parse/convert）。在编写 JavaScript 代码、命名函数或设定编码规范时使用。
metadata:
  author: LvHeng
  version: "2026.5.20"
  source: Generated from src/javascript/AGENT.md
---

# JavaScript 编码规范

始终使用 `function` 声明而非 `const` 箭头函数赋值。代码组织上，入口函数在顶部调用，定义函数按调用顺序依次排列。事件处理和数据转换遵循一致的命名模式。

## 何时使用此 Skill

在以下场景使用此 skill：

- 编写新的 JavaScript 代码
- 命名 JavaScript 项目中的函数或变量
- 代码审查时检查命名一致性
- 设定或执行 JavaScript 编码规范
- 重构代码以提升函数命名清晰度

## 偏好设置

- 优先使用 `function` 声明，而非 `const` 箭头函数赋值
- 入口函数（mount/load/init/main/setup）在顶部，定义按调用顺序排列
- 事件处理函数始终使用 `on[事件]` 命名模式

## 函数声明方式

非必要情况下，始终使用 `function` 声明，不使用 `const` 箭头函数赋值。

```javascript
// ✅ 正确
function fn() {}

// ❌ 错误
const fn = () => {}
```

## 函数声明与调用顺序

先在顶部调用入口函数，再按调用顺序从上到下定义函数。

```javascript
mount()  // 入口函数（mount / load / init / main / setup 等）

function mount() {
  readConfig()
  fetchData()
}

function readConfig() { }
function fetchData() { }
```

## 命名约定

### 事件处理函数

始终使用 `on[事件]` 格式命名事件处理函数。

```javascript
function onClick() {}
function onSubmit() {}
function onButtonClick() {}
```

### 数据转换函数

- 类型转换 — `to` + 类型：`toNumber()`, `toInt()`, `toPercentage()`
- 从字符串解析 — `parse` + 类型：`parseInt()`, `parseDate()`
- 复杂格式转换 — `convert` + 模式：`convertToPascalCase(str)`, `convertUnits(value, from, to)`

### 避免的命名

- ❌ `change()` — 太模糊
- ❌ `process()` — 不明确做了什么
- ❌ `handle()` — 不清晰
- ❌ `doConvert()` — 冗余

## 核心原则

> **命名要让调用者一眼知道输入是什么、输出是什么。**
