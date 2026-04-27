[中文](./README.zh.md)

# vue-skills

A curated skill pack for AI coding assistants, providing project-specific knowledge for **Vue 3** development using **`defineComponent()` + `.tsx`** (Composition API with TSX render functions).

## Installation

```bash
npx skills add https://github.com/uphg/vue-skills --skill vue-skills
```

## Usage

This repository is designed to be consumed by AI-powered coding tools (such as Cursor, Windsurf, or similar) that support a "skills" system. Point your AI assistant to the `skills/` directory to give it context-aware guidance on Vue 3 + TSX code generation.

## Structure

```
skills/vue-tsx/
├── SKILL.md                          # Main skill definition & coding preferences
├── GENERATION.md                     # Provenance & generation metadata
├── CHANGES.md                        # Modification changelog
└── references/
    ├── define-component-tsx.md       # defineComponent() + TSX patterns
    ├── core-new-apis.md              # Reactivity, lifecycle, composables
    └── advanced-patterns.md          # Built-in components & directives in TSX
```

## Content

- **SKILL.md** — Declares the preferred coding style: TypeScript, `defineComponent()`, `shallowRef`, Composition API, and provides a canonical component template.
- **define-component-tsx.md** — Covers both function and options signatures, props/emits declarations, generic components, `defineAsyncComponent`, and custom directives in TSX.
- **core-new-apis.md** — Deep reference on `ref`, `shallowRef`, `computed`, `reactive`, `watch`, `watchEffect`, lifecycle hooks, `effectScope`, and composable patterns.
- **advanced-patterns.md** — TSX equivalents of `Transition`, `Teleport`, `Suspense`, `KeepAlive`, and custom directives.

## Origin

Generated from the [official Vue.js documentation](https://github.com/vuejs/docs) using the [skills](https://github.com/antfu/skills) tool, then customized for the `defineComponent()` + `.tsx` workflow.

**Author:** LvHeng
**Version:** 2026.4.27
