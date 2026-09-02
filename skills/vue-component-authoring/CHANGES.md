# Changelog

## 2026-09-02 — Description: added missing trigger anchors (fallback trigger eval)

Ran a description-only trigger evaluation (upstream run_loop.py requires `claude -p`, which hangs in this environment; used a fresh-context judge simulating the available_skills decision against 20 queries per skill — see `trigger-evals.json`).

- vue-tsx: 20/20 correct — no change.
- vue-component-authoring: 17/20; the 3 false negatives (side-effect cleanup, attrs passthrough, SSR compatibility) were capabilities covered only in the body, invisible at trigger time. Added `cleanupXxx + onBeforeUnmount` cleanup, `inheritAttrs`/root-element attrs merging, and SSR compatibility anchors to the description.

## 2026-09-02 — Added evals.json

Added 2 test prompts with `expected_output` and `expectations` per skill-dev Step 5 and the skill-creator schema. Coverage: full Dialog component API (props/emits/callbacks/slots/expose/tests), side-effect cleanup convention. No SKILL.md content changed.

## 2026-08-25 — Description rewrite

Compressed the jargon-heavy coverage enumeration into representative anchors and switched to a verb-led opening, keeping the matching value while making the description easier to scan. No skill content changed — topics dropped from the description (const-array enum governance, side-effect cleanup, attrs passthrough, hooks/utils organization, coding style) remain covered in the skill body.

### Changes

### 1. `SKILL.md` → description revised

- Opening now verb-led: "Author reusable Vue 3 components for a component library using defineComponent + TSX".
- "Use when" broadened: building, reviewing, or refactoring library components.
- Coverage enumeration compressed to API-design anchors (emits option + emit() with callback props, SlotsType typing, expose + XxxInst interfaces) plus directory/test organization (Vitest + @vue/test-utils).
- Negative boundary unchanged in substance; wording tightened to "one-off application components".

### 2. `SKILL.zh.md` → synced

Structurally identical Chinese version updated with the same changes.

## 2026-07-18 — Initial version

Created the Vue component library authoring conventions skill (from the original `src/vue-component-authoring/README.md` document).
Originally named `vue-style-guide`, later renamed to `vue-component-authoring`.

### Modified files

- `SKILL.md` / `SKILL.zh.md` — main document
- `references/component-template.md` / `.zh.md` — component template
- `references/api-design.md` / `.zh.md` — API design
- `references/testing-and-lifecycle.md` / `.zh.md` — testing and lifecycle
- `README.md` — source document

---

## 2026-07-19 — Emit pattern correction

Changed the emit pattern from callback props + a `call()` helper function to Vue 3's official `emits` option + `emit()`.

### Modified files

- `SKILL.md` — English skill main document (canonical)
- `SKILL.zh.md` — Chinese skill main document
- `GENERATION.md` — generation metadata
- `references/project-layout.md` / `.zh.md` — project layout + hooks/utils + coding style
- `references/component-template.md` / `.zh.md` — defineComponent implementation template
- `references/api-design.md` / `.zh.md` — Props / types / Emits / Slots / Expose
- `references/testing-and-lifecycle.md` / `.zh.md` — side-effect cleanup + attrs + testing

---

## 2026-07-19 — Emit pattern addition: callback props

Added a matching callback prop for each `update:xxx` v-model event (e.g. `onUpdateValue` for `update:value`) so parent components can use them easily in JSX. Removed the Prohibitions entry banning `onXxx`.

### Modified files

- `SKILL.md` — English skill main document
- `SKILL.zh.md` — Chinese skill main document
- `references/api-design.md` / `.zh.md` — Emits section
- `references/component-template.md` / `.zh.md` — base template
- `CHANGES.md` — this file

---

## 2026-07-19 — Simplified rules: removed overly rigid restrictions

Relaxed the directory structure into a reference layout (not mandatory), removed lint/format/commit and other coding-style entries unrelated to component authoring, changed SSR testing to a conditional requirement, and dropped the mandatory `src/utils/` flattening constraint.

### Modified files

- `SKILL.md` / `SKILL.zh.md` — main document Step 1, Step 11, Prohibitions, When Unsure
- `references/project-layout.md` / `.zh.md` — removed concrete file lists and coding style tables
- `references/testing-and-lifecycle.md` / `.zh.md` — SSR testing changed to a conditional requirement
- `CHANGES.md` — this file
