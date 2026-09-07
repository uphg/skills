# Changelog

## 2026-09-07 — Restructured function naming into a grouped list

Replaced the flat Functions quick-reference table plus separate disambiguation bullets with a single nested unordered list. The `to` / `from` / `as` prefixes now sit under one "Type conversion" parent item, making the grouping explicit; each sibling pattern carries its own disambiguation rule inline, removing the duplication between the table and the rules list. Information-architecture change only — all rules, examples, and prohibitions preserved.

### Changes

### 1. `SKILL.md` → Functions section restructured

- Replaced the `Scenario | Pattern | Examples` table and the four disambiguation bullets with one nested unordered list.
- `to` / `from` / `as` grouped under a single **Type conversion** parent item with three sub-bullets, each carrying pattern + return-type semantics + examples; the `parse`/`convert` prohibition moved into the parent item.
- Remaining patterns (event handler, Async, api, read/write, get/set) became sibling bullets in the original order; their disambiguation rules (Async semantics, api never + Async, storage-location discriminator) merged inline into each bullet.
- Frontmatter `metadata.version` → `2026.9.4`.

### 2. `SKILL.zh.md` → synced, structurally identical

## 2026-09-06 — Reworked data-conversion naming: `to` / `from` / `as` only

Replaced the data-conversion naming module with three direction rules chosen by what happens to the return type. Removed `parse` and `convert` as conversion naming patterns.

### Changes

### 1. `SKILL.md` → rules replaced

- Type conversion is now three rows driven by return-type semantics:
  - `to` + Target — becomes the target type/entity (return type changes): `toNumber()`, `toInt()`, `toUserVO()`.
  - `from` + Source — builds data from a source format (reverse-deserialization): `fromUnixTime()`, `fromBase64()`.
  - `as` + Format — changes the display format (return type unchanged): `asCamelCase()`, `asPercent()`.
- Removed the `parse` + Type and `convert` + Pattern rows — data transformation naming uses only `to` / `from` / `as`.
- New disambiguation bullet spelling out the return-type discriminator and forbidding `parse`/`convert` for transformation names.
- Frontmatter description anchor updated: data transformers `to/parse/convert` → `to/from/as`; `metadata.version` → `2026.9.3`.

### 2. `SKILL.zh.md` → synced with identical structure

### 3. `evals.json` → eval 2 updated

The string→number conversion expectation now requires a `to`/`from` name (e.g., `toNumber` / `fromPercent`) instead of a `to`/`parse`/`convert` pattern.

## 2026-09-02 — Added evals.json

Added 3 test prompts with `expected_output` and `expectations` per skill-dev Step 5 and the skill-creator schema (repo audit: eval coverage). Coverage: declaration style + call order, naming review, api-vs-Async disambiguation. No SKILL.md content changed.

## 2026-09-02 — Restructured: separated code structure from naming

Reorganized the section layout around the document's two underlying concerns — how code is structured and how things are named — after review found misplaced and overlapping sections. All rules, examples, and caveats are preserved; this is an information-architecture change only.

### Changes

### 1. `SKILL.md` → section restructure

- Merged the two near-identical top-level headings `Function Declaration Style` and `Function Declaration Order` into one `Function Structure` section with `Declaration` and `Call Order` subsections.
- Introduced a single `Naming Conventions` umbrella:
  - Collapsed the four function-naming subsections (event handlers, data transformation, async functions, storage & cache access) into one quick-reference table (`Scenario | Pattern | Examples`) plus three disambiguation bullets (`Async` suffix semantics; `api` + verb never combined with `Async`, no `asyncFetch`/`doRequest`; `read`/`write` vs `get`/`set` chosen by data location).
  - Moved `Constants` out of "Function Naming Conventions" — constants are not functions — and merged it with `Variable Naming` into `Variables & Constants`.
  - Moved top-level `File Naming` and `Names to Avoid` under `Naming Conventions`.
- Top-level sections reduced from 9 to 4: When to Use This Skill → Function Structure → Naming Conventions → Core Principle.
- Kept `to` / `parse` / `convert` as three distinct table rows to preserve the type-conversion / string-parsing / complex-conversion distinction.
- Frontmatter `metadata.version` → `2026.9.2`.

### 2. `SKILL.zh.md` → synced

Structurally identical Chinese version with the same restructure.

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
