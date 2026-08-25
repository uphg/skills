# Changelog

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
