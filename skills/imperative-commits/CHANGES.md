# Changelog

## 2026-09-02 — Added expectations to evals.json

Each eval in the existing evals.json now carries an `expectations` array (verifiable statements) per the skill-creator schema, enabling automated grading and benchmark runs. Prompts and `expected_output` unchanged.

## 2026-08-24 — Restore enforcement semantics

Per maintainer feedback: the skill's purpose is to enforce imperative mood uniformly, not to defer to repository conventions. Reverted the conflict-resolution behavior added earlier today while keeping the structural improvements.

### Changes

### 1. `SKILL.md` → revised

- Description restored to "Enforces …", now stating rules apply uniformly regardless of repository history; removed the deference clause.
- Removed Workflow Step 1 (defer to existing repo convention); rules now apply to every commit title unconditionally.
- "Not to be confused with" reworded: this skill selects the imperative style over Conventional Commits (`Add login`, not `feat: add login`).
- Prohibitions inverted: never write or accept a non-imperative title, even to match surrounding history.
- When Unsure changed: default to the rules; deviate only on explicit user instruction for a specific commit.

### 2. `SKILL.zh.md` → synced

Structurally identical Chinese version updated with the same changes.

### 3. `evals.json` → rewritten

Eval 1 no longer expects a repo-convention check; eval 2 now expects Conventional-style titles to be flagged rather than accepted; eval 3 flipped from "defer to CONTRIBUTING.md" to "enforce imperative style despite Conventional history".

### 4. `README.md` / `README.zh.md` → updated

Description lines re-synced (deferral clause removed).

## 2026-08-24 — Review revision

Applied the skill-dev review checklist: restructured into the standard format, added convention-conflict resolution, and added evaluations.

### Changes

### 1. `SKILL.md` → revised

- Description rewritten: removed the overclaiming "Enforces"; added trigger scenarios (creating a commit, reviewing/fixing messages, validating quality, Q&A) and a note that repo conventions take precedence.
- Added `When to Use This Skill`, `Workflow` (check existing convention → apply rules → test and fix), `Prohibitions`, and `When Unsure` sections per the repo's skill format.
- Merged the overlapping rules ("imperative mood" + "base verb form") into one reasoned rule: four rules → three.
- Added reasoning: Git's own generated messages are imperative (`Merge branch …`, `Revert …`).
- Clarified scope: subject line only; capitalization applies to Latin-script titles.
- Replaced the redundant "Quick validation" checklist (it repeated the four rules) with the Step 3 test-and-fix flow.
- New decision rule: inspect `git log` / `CONTRIBUTING.md` first and defer to established conventions such as Conventional Commits, so valid history is never flagged or rewritten.

### 2. `SKILL.zh.md` → synced

Structurally identical Chinese version updated with the same changes (section count and order match).

### 3. `evals.json` → new

Three test prompts covering: writing a commit from scratch, reviewing mixed-convention messages, and deferring to `CONTRIBUTING.md`.

### 4. `README.md` / `README.zh.md` → updated

Description lines synced with the new frontmatter description.

## 2026-05-20 — Initial version

Created the imperative-commits skill document for imperative-mood commit message conventions.

### Changes

### 1. `SKILL.md` → new

English skill document covering:

- Four rules (imperative mood, capitalized first letter, base verb form, no trailing period)
- Verification method ("If applied, this commit will _____")
- Correct vs incorrect examples
- Difference from Conventional Commits
- Quick checklist

### 2. `SKILL.zh.md` → new

Chinese skill document, structurally identical to the English version.
