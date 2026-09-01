# Changelog

## 2026-09-01 — Renamed to `js-coding-style` and expanded coverage

Renamed the skill from `javascript` to `js-coding-style` to avoid name collisions in the skills ecosystem, and filled the coverage gaps found in review: async function naming, constants, and file naming. Removed the redundant Preferences section and clarified the intent behind the entry-function-top convention.

### Changes

### 1. Directory & name → `js-coding-style`

- `skills/javascript/` → `skills/js-coding-style/`; `skills-zh/javascript/` → `skills-zh/js-coding-style/`.
- Frontmatter `name` updated to `js-coding-style`; README install commands updated accordingly.

### 2. `SKILL.md` / `SKILL.zh.md` → content updates (synced)

- Removed the `Preferences` section — its three bullets fully duplicated later sections.
- Added an explicit note that entry-function-top ordering is an intentional reading-order convention of this codebase, not a universal best practice.
- Retitled "Naming Conventions" to "Function Naming Conventions"; promoted Variable Naming and File Naming to top-level sections.
- Reworked async function naming: `Async` suffix for Promise-returning functions (`initSettingsAsync`); network/API requests use `api` + verb (`apiGetUser`, `apiDeleteOrder`) without the `Async` suffix — the two never combine.
- New section: storage & cache access verbs — `read`/`write` for persistent storage (`readSettingsFromStorage`, `writeSettingsToStorage`), `get`/`set` for in-memory cache and app state (`getCachedUser`, `setCachedUser`).
- New section: constants (`UPPER_SNAKE_CASE` at module level, lowerCamelCase in function scope).
- New section: file naming (`kebab-case`).
- Description updated with the new naming anchors (Async suffix, api+verb, read/write, get/set, constants, files).

## 2026-08-25 — Description rewrite

Reworked the front-matter description: verb-led opening that states the rules themselves, concrete naming anchors kept, added a quoting-style trigger example and an explicit negative boundary. No skill content changed.

### Changes

### 1. `SKILL.md` → description revised

- Opening now imperative-style ("Apply JavaScript coding conventions: …") instead of a topic-list noun phrase.
- "Use when" expanded: code review and variable-naming questions made explicit, with the example "what should I call this handler?".
- Added negative boundary: not for TypeScript type-level design or framework-specific component conventions.

### 2. `SKILL.zh.md` → synced

Structurally identical Chinese version updated with the same changes.

Initial creation: generated the JavaScript coding conventions skill from `src/javascript/AGENT.md`.

## Changes

### 1. `SKILL.md` → new

Summarized from `src/javascript/AGENT.md` into SKILL format, including:

- Function declaration style: always use `function` instead of arrow functions
- Declaration and call ordering: entry functions on top, definitions ordered by call sequence
- Event handler naming: the `on[Event]` pattern
- Data transformation naming: `to` / `as` / `parse` / `convert` prefix conventions
- Names to avoid: `change`, `process`, `handle`, `doConvert`
