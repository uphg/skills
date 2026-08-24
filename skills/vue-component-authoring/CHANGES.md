# Changelog

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
