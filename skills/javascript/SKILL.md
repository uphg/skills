---
name: javascript
description: JavaScript coding conventions covering function declaration style (function vs arrow), function ordering, event handler naming (on[Event]), data transformation naming patterns (to/parse/convert), and variable naming patterns (is/has prefix, descriptive booleans, plural/singular nouns). Use when writing JavaScript, naming functions, or setting coding standards.
metadata:
  author: LvHeng
  version: "2026.5.20"
  source: Generated from src/javascript/AGENT.md
---

# JavaScript Coding Conventions

Always use `function` declarations over arrow functions assigned to `const`. Organize code by calling entry functions at the top, then defining functions in call order below. Follow consistent naming patterns for event handlers and data transformations.

## When to Use This Skill

Use this skill when:

- Writing new JavaScript code
- Naming functions or variables in JavaScript projects
- Reviewing code for naming consistency
- Setting or enforcing JavaScript coding standards
- Refactoring to improve function naming clarity

## Preferences

- Prefer `function` declarations over `const` arrow function assignments
- Entry functions (mount/load/init/main/setup) at the top, definitions below in call order
- Event handlers always use `on[Event]` naming pattern

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

```javascript
mount()  // Entry function (mount / load / init / main / setup)

function mount() {
  readConfig()
  fetchData()
}

function readConfig() { }
function fetchData() { }
```

## Naming Conventions

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

### Variable Naming

Use prefixes and naming patterns to make variable types and intent clear:

| Category | Convention | Examples | Notes |
|----------|-----------|----------|-------|
| Boolean | `is`/`has` prefix or descriptive adjective | `isLoading`, `hasError`, `modalVisible` | Prefix or descriptive name that naturally implies true/false |
| Number/String | `current`/`raw` prefix | `currentPage`, `rawText` | `current` for stateful values, `raw` for unprocessed data |
| Array | Plural noun | `users`, `items`, `configs` | Plural form signals a collection |
| Single Object | Singular noun | `user`, `item`, `config` | Singular form signals a single entity |

### Names to Avoid

- ❌ `change()` — too vague
- ❌ `process()` — unclear what is done
- ❌ `handle()` — not specific enough
- ❌ `doConvert()` — redundant prefix

## Core Principle

> **Naming should let the caller know at a glance what the input is and what the output is.**
