# Changelog

Initial creation: generated the JavaScript coding conventions skill from `src/javascript/AGENT.md`.

## Changes

### 1. `SKILL.md` → new

Summarized from `src/javascript/AGENT.md` into SKILL format, including:

- Function declaration style: always use `function` instead of arrow functions
- Declaration and call ordering: entry functions on top, definitions ordered by call sequence
- Event handler naming: the `on[Event]` pattern
- Data transformation naming: `to` / `as` / `parse` / `convert` prefix conventions
- Names to avoid: `change`, `process`, `handle`, `doConvert`
