---
name: imperative-commits
description: "Enforces Git commit messages in imperative mood: capitalized first letter, base verb form, no trailing period. Use when users ask about commit message conventions or want to validate commit quality."
---

# Imperative Commits

Write Git commit titles that read like commands: `Add feature` not `Added feature` or `adds feature`.

## The Four Rules

1. **Imperative mood** — Reads as a command
2. **Capitalized** — First letter uppercase
3. **Base verb** — Use `Add`, `Fix`, `Update` (not `Added`, `Fixes`, `Updating`)
4. **No period** at the end

## The Test

Does your title complete this sentence? *"If applied, this commit will _____"*

## Correct vs Incorrect

| ✅ Correct | ❌ Incorrect |
|------------|--------------|
| `Add login validation` | `added login validation` |
| `Fix memory leak` | `Fixes memory leak.` |
| `Update README` | `Updating README` |

## Not to be confused with

**Conventional Commits** (`feat: add login` — lowercase, has prefix). These can be combined: `feat: Add login` (prefix lowercase, title imperative).

## Quick validation

- Capitalized? ✅
- Verb base form? ✅
- No period? ✅
- Passes "If applied..."? ✅