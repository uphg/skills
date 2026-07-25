# skills

English | [中文](./README.zh.md)

A curated collection of skill packs for AI coding assistants, providing project-specific knowledge for various frameworks and tools.

## Available Skills

### vue-tsx

Vue 3 development using `defineComponent()` + `.tsx` (Composition API with TSX render functions).

```bash
npx skills add https://github.com/uphg/skills --skill vue-tsx
```

### fsd

Frontend architecture design following Feature-Sliced Design (FSD) methodology.

```bash
npx skills add https://github.com/uphg/skills --skill fsd
```

### imperative-commits

Enforces Git commit messages in imperative mood: capitalized first letter, base verb form, no trailing period.

```bash
npx skills add https://github.com/uphg/skills --skill imperative-commits
```

### javascript

JavaScript coding conventions: function declaration style, call ordering, event handler naming, and data transformation naming patterns.

```bash
npx skills add https://github.com/uphg/skills --skill javascript
```

### vue-component-authoring

Vue 3 component library authoring conventions: directory layout, props/emits/slots/expose API design, emits + callback props for JSX, const-array enum governance, side-effect cleanup, attrs passthrough, testing, and hooks/utils organization.

```bash
npx skills add https://github.com/uphg/skills --skill vue-component-authoring
```

### markdown-style

Markdown document formatting and style optimization for Chinese, English, and CJK-mixed documents. Covers title hierarchy, code/term formatting, CJK spacing, typography, language polishing, and professional layout standards.

```bash
npx skills add https://github.com/uphg/skills --skill markdown-style
```

## Usage

This repository is designed to be consumed by AI-powered coding tools (such as Cursor, Windsurf, or similar) that support a "skills" system. Point your AI assistant to the `skills/` directory to give it context-aware guidance on code generation.

## Structure

```
skills/
└── <skill-name>/
    ├── SKILL.md                          # Main skill definition & coding preferences
    ├── GENERATION.md                     # Provenance & generation metadata
    ├── CHANGES.md                        # Modification changelog
    └── references/                       # Reference documents
        └── ...
```

Each skill resides in its own directory under `skills/`, following the same structure.

## Contributing

Feel free to open issues or pull requests to improve existing skills.

**Author:** LvHeng
