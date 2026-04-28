# skills

English | [中文](./README.zh.md)

A curated collection of skill packs for AI coding assistants, providing project-specific knowledge for various frameworks and tools.

## Available Skills

### vue-tsx

Vue 3 development using `defineComponent()` + `.tsx` (Composition API with TSX render functions).

```bash
npx skills add https://github.com/uphg/skills --skill vue-tsx
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
