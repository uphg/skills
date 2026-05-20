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

**Conventional Commits** and imperative commits are distinct conventions — pick one: `feat: add login` (Conventional Commits) or `Add login` (imperative), not both.

## Quick validation

- Capitalized? ✅
- Verb base form? ✅
- No period? ✅
- Passes "If applied..."? ✅