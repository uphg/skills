# Changelog

## 2026-08-25 — Description rewrite

Reworked the front-matter description following the example-skills benchmark: verb-led opening, concrete trigger scenarios, explicit negative boundary, and cross-reference to the sibling skill. No skill content changed.

### Changes

### 1. `SKILL.md` → description revised

- Opening now verb-led: "Develop Vue 3 components …" instead of the noun list "Vue 3 Composition API, defineComponent + TSX …".
- Added explicit coverage anchors: reactivity primitives (ref, shallowRef, computed) and generic components.
- Added a trigger scenario: converting an SFC (`<script setup>`) to defineComponent/TSX.
- Added negative boundary: not for component library conventions (use vue-component-authoring) or styling/theme decisions.

### 2. `SKILL.zh.md` → synced

Structurally identical Chinese version updated with the same changes.

Changed the Vue 3 documentation preference from `.vue` `<script setup lang="ts">` syntax first to `defineComponent()` + `.tsx`.

## Changes

### 1. `SKILL.md`

| Location | Before | After |
|------|--------|--------|
| Tagline (L12) | `Always use Composition API with <script setup lang="ts">` | `Always use Composition API with defineComponent() + .tsx` |
| Preference (L17) | `Prefer <script setup lang="ts"> over <script>` | `Prefer defineComponent() + .tsx over <script setup lang="ts"> + .vue` |
| Reference table (L26) | `Script Setup & Macros` → `script-setup-macros.md` | `defineComponent + TSX` → `define-component-tsx.md` |
| Component Template (L37-62) | `.vue` SFC with `<script setup lang="ts">` | `.tsx` with `defineComponent()` function signature |
| description (frontmatter) | `script setup macros, defineProps/defineEmits/defineModel` | `defineComponent + TSX` |

### 2. `references/script-setup-macros.md` → deleted

Removed. SFC macros (`defineProps`, `defineEmits`, `defineModel`, `defineOptions`, `defineSlots`) do not apply to the `.tsx` approach.

### 3. `references/define-component-tsx.md` → new

Covers:

- `defineComponent()` Options Signature
- `defineComponent()` Function Signature (3.3+)
- Props declaration (runtime declaration + `PropType`)
- Emits declaration
- Generic components
- Expose
- `defineAsyncComponent`
- Custom directives in TSX
- Webpack treeshaking notes

### 4. `references/advanced-patterns.md`

| Location | Change |
|------|----------|
| Transition | template syntax → TSX/JSX, `v-if` changed to `{condition && element}` |
| TransitionGroup | `v-for` changed to `.map()` |
| Teleport | template syntax → TSX/JSX |
| Suspense | template syntax → TSX/JSX, named slots use object syntax `{{ default: () => ..., fallback: () => ... }}` |
| KeepAlive | template syntax → TSX/JSX, dynamic components via `h(component)` |
| v-memo / v-once | marked as template-only, no TSX equivalent |
| Custom Directives | `<script setup lang="ts">` → defined inside a `defineComponent` setup function |

### 5. `references/core-new-apis.md`

**No changes.** This file only covers the Composition API (reactivity, watchers, lifecycle, composables) and contains no SFC-specific syntax; it applies to both `.tsx` and `.vue`.
