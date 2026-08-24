---
name: imperative-commits
description: "Enforces Git commit titles in imperative mood: capitalized first letter, base verb form, no trailing period — applied uniformly to every commit regardless of repository history. Use when creating a git commit or writing a commit message, reviewing or fixing an existing commit message, validating commit quality, or answering questions about commit conventions."
---

# Imperative Commits

Write Git commit titles (the subject line) that read like commands: `Add feature` not `Added feature` or `adds feature`. Body-text conventions are out of scope.

## When to Use This Skill

- Before running `git commit` — composing the commit title
- Reviewing or validating existing commit messages
- Fixing a commit message rejected in review or CI
- Answering questions about commit message conventions

## Workflow

### Step 1: Apply the three rules

These rules apply to **every** commit title, even when repository history uses a different style — uniformity is the purpose of this skill.

1. **Imperative mood, base verb form** — use `Add`, `Fix`, `Update`; never `Added`, `Fixes`, `Updating`. A title must complete the sentence *"If applied, this commit will _____"*: `Add login validation` works; `Added login validation` doesn't. Git itself writes this way — `git merge` generates `Merge branch 'x'`, `git revert` generates `Revert "..."`.
2. **Capitalized first letter** — applies to Latin-script titles; other languages have no letter case.
3. **No trailing period** at the end of the title.

### Step 2: Test and fix

Fill the final title into *"If applied, this commit will _____"*. If it reads naturally, proceed; if not, rewrite it and say which titles you changed and why.

## Correct vs Incorrect

| ✅ Correct | ❌ Incorrect |
|------------|--------------|
| `Add login validation` | `added login validation` |
| `Fix memory leak` | `Fixes memory leak.` |
| `Update README` | `Updating README` |

## Not to be confused with

**Conventional Commits** and imperative commits are distinct conventions. This skill enforces the imperative style: write `Add login`, not `feat: add login`, and never combine both in one title.

## Prohibitions

- Do not write or accept a non-imperative title — including to match surrounding history; this skill defines the required convention.
- Do not mix the two conventions in one title: `feat: Added login` satisfies neither.

## When Unsure

Default to the three rules; the *"If applied, this commit will _____"* test settles borderline wording. Deviate only when the user explicitly asks for a different style for a specific commit — an explicit instruction beats this skill.
