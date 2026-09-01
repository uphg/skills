# JavaScript 编码规范

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

## 事件处理函数命名

始终使用 `on[事件]` 格式命名事件处理函数。

```javascript
function onClick() {}
function onSubmit() {}
function onButtonClick() {}
```

## 数据转换函数命名

| 场景 | 命名格式 | 示例 |
|------|----------|------|
| 类型转换 | `to` + 类型 | `toString()`, `toNumber()` |
| 格式转换 | `as` + 类型 | `asInt()`, `asPercentage()` |
| 解析字符串 | `parse` + 类型 | `parseInt()`, `parseDate()` |
| 通用数据格式转换 | `convert` | `convertToPascalCase(str)`, `convertUnits(value, from, to)` |

## 变量命名

使用前缀和命名模式来明确变量类型与意图：

| 类别 | 约定 | 示例 | 说明 |
|------|------|------|------|
| 布尔值 | `is`/`has` 前缀或描述性形容词 | `isLoading`、`hasError`、`modalVisible` | 使用前缀或能自然暗示 true/false 的描述性名称 |
| 数值/字符串 | `current`/`raw` 前缀 | `currentPage`、`rawText` | `current` 表示状态值，`raw` 表示原始数据 |
| 数组 | 复数名词 | `users`、`items`、`configs` | 复数形式表示集合 |
| 单个对象 | 单数名词 | `user`、`item`、`config` | 单数形式表示单个实体 |

## 避免的命名
- ❌ `change()` - 太模糊
- ❌ `process()` - 不明确做了什么
- ❌ `handle()` - 不清晰
- ❌ `doConvert()` - 冗余

## 核心原则

> **命名要让调用者一眼知道输入是什么、输出是什么。**