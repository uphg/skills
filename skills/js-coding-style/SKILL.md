---
name: js-coding-style
description: "Apply JavaScript coding style conventions: prefer function declarations over const arrow functions, order functions by call sequence, name event handlers on[Event], data transformers to/parse/convert, async functions with an Async suffix, API requests as api+verb, persistent storage read/write, cache access get/set, constants UPPER_SNAKE_CASE, and booleans with is/has prefixes. Use when writing or reviewing JavaScript code, naming a function, variable, constant, or file (\"what should I call this handler?\"), or setting team coding standards. Not for TypeScript type-level design or framework-specific component conventions."
metadata:
  author: LvHeng
  version: "2026.9.1"
  source: Generated from src/javascript/AGENT.md
---

# JavaScript Coding Style

Always use `function` declarations over arrow functions assigned to `const`. Organize code by calling entry functions at the top, then defining functions in call order below. Follow consistent naming patterns for event handlers, async functions, constants, and data transformations.

## When to Use This Skill

Use this skill when:

- Writing new JavaScript code
- Naming functions, variables, constants, or files in JavaScript projects
- Reviewing code for naming and style consistency
- Setting or enforcing JavaScript coding standards
- Refactoring to improve naming clarity

## Function Declaration Style

Always use `function` keyword declarations. Do not use arrow functions assigned to `const`.

```javascript
// ✅ Correct
function fn() {}

// ❌ Incorrect
const fn = () => {}
```

## Function Declaration Order

Call the entry function at the top, then define functions in the same order they are called.

This is an intentional reading-order convention: a reader should see *what the program does* before *how each step is implemented*. It is a style choice for this codebase, not a universal best practice — apply it in new code and refactors, but do not mechanically rewrite third-party code or files that follow a different established layout.

```javascript
mount()  // Entry function (mount / load / init / main / setup)

function mount() {
  readConfig()
  fetchData()
}

function readConfig() { }
function fetchData() { }
```

## Function Naming Conventions

### Event Handlers

Always use `on[Event]` format for event handler functions.

```javascript
function onClick() {}
function onSubmit() {}
function onButtonClick() {}
```

### Data Transformation Functions

- Type conversion — `to` + Type: `toNumber()`, `toInt()`, `toPercentage()`
- String parsing — `parse` + Type: `parseInt()`, `parseDate()`
- Complex conversion — `convert` + Pattern: `convertToPascalCase(str)`, `convertUnits(value, from, to)`

### Async Functions

Append `Async` to any function that returns a Promise. The verb states what is done; the suffix tells the caller to `await` it.

```javascript
function initSettingsAsync() {}
function loadLocaleAsync() {}
```

Network / API requests use the `api` prefix + verb instead, and do not take the `Async` suffix — `api` already implies a request:

```javascript
function apiGetUser() {}
function apiDeleteOrder() {}
```

- Do not mix the two — no `apiGetUserAsync`
- Do not use a prefix in place of the suffix — no `asyncFetch`, `doRequest`

### Storage & Cache Access

Choose the verb by where the data lives:

| Data location | Verb | Examples |
|---|---|---|
| Persistent storage (localStorage, file, DB) | `read` / `write` | `readSettingsFromStorage()`, `writeSettingsToStorage()` |
| In-memory cache, app state | `get` / `set` | `getCachedUser()`, `setCachedUser()` |

### Constants

- Module-level immutable values — `UPPER_SNAKE_CASE`: `MAX_RETRY_COUNT`, `DEFAULT_TIMEOUT`
- Function-scoped constants and all variables — lowerCamelCase: `maxRetries`

## Variable Naming

Use prefixes and naming patterns to make variable types and intent clear:

| Category | Convention | Examples | Notes |
|----------|-----------|----------|-------|
| Boolean | `is`/`has` prefix or descriptive adjective | `isLoading`, `hasError`, `modalVisible` | Prefix or descriptive name that naturally implies true/false |
| Number/String | `current`/`raw` prefix | `currentPage`, `rawText` | `current` for stateful values, `raw` for unprocessed data |
| Array | Plural noun | `users`, `items`, `configs` | Plural form signals a collection |
| Single Object | Singular noun | `user`, `item`, `config` | Singular form signals a single entity |

## File Naming

Use `kebab-case` for file and directory names: `api-user.js`, `date-utils.js`.

## Names to Avoid

- ❌ `change()` — too vague
- ❌ `process()` — unclear what is done
- ❌ `handle()` — not specific enough
- ❌ `doConvert()` — redundant prefix

## Core Principle

> **Naming should let the caller know at a glance what the input is and what the output is.**
