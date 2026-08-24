# Changelog

## 2026-07-31

### Created skills/specs-workflow

Distilled `src/specs-workflow/specs-workflow.md` (the `.specs` directory convention) into a distributable skill pack:

- `SKILL.md`: English definition, including Workflow, directory structure, mandatory rules, naming conventions, Prohibitions, and When Unsure.
- `references/file-templates.md`: full skeleton templates for `.specs/README.md`, `requirements.md`, `design.md`, `tasks.md`, and `CHANGELOG.md`.
- `references/example.md`: complete example document (split-out recorded below).
- `GENERATION.md`: source and generation metadata.

Also created the Chinese version `src/specs-workflow/SKILL.zh.md`, structurally identical to the English version.

### Rewrote references/example.md as a complete example

- Rewrote from "condensed example (excerpted fragments + traceability chain)" into **three complete example documents in one file** (English, trimmed from `example/.specs/vue-headless-tabs/`):
  - requirements.md example: kept Requirements 1–3, each with a User Story + full acceptance criteria
  - design.md example: kept Introduction / Architecture / Key Design Decisions / Data Models & Types / Components / Error Handling / Correctness Properties (3 items, `Validates: Requirements 2.2/2.3/2.5`)
  - tasks.md example: kept phases 1–3 and Checkpoint, with `_Requirements:` references and the Task Dependency Graph
  - Internal references kept self-consistent, pointing only at retained items
- `SKILL.md` / `SKILL.zh.md`: Step 2 now links `references/example.md` alongside the file-templates link

### Split the example into standalone files

- Deleted `references/example.md`; the three complete examples were split into standalone files under `references/examples/`:
  - `references/examples/requirements.md` / `design.md` / `tasks.md`
  - Content taken out of the fences, ready to use as real reference documents
- `SKILL.md` / `SKILL.zh.md`: Step 2 links updated to point to the three files under `references/examples/`

### Switched example diagrams to Mermaid

- `references/examples/design.md`: replaced the ASCII box diagram in `## Architecture` with a Mermaid `flowchart TB` diagram (keeping the three-layer structure: Consumer → API Layer → TabsContext, with `uses` / `provide / inject` edge labels)
- `references/file-templates.md`: added a convention to the Architecture section of the design.md skeleton: "Use Mermaid for diagrams (e.g. architecture layers, data flow).", keeping AI-generated diagrams consistent

### Examples changed to "prompt-first + condensed traceability example"

- Added `references/prompt-templates.md`: fill-in-the-blank prompts for planning ahead (paired with the skeletons; they fix depth, not structure) —
  - requirements.md: Introduction names the pain point, Glossary defines each term, User Story has three elements, AC must be testable and cover happy path + edges + exceptions
  - design.md: Overview names the trade-offs, Architecture uses Mermaid and explains "why layered this way", Components explain "why", each Property uses the `*For any*` form and points back to a requirement
  - tasks.md: phases divided by dependency, tasks reference requirement clauses, Checkpoints must be verifiable, dependency graph waves
  - Cross-cutting traceability conventions (`Validates:` / `_Requirements:`)
- Deleted the three complete examples `references/examples/requirements.md` / `design.md` / `tasks.md`; added `references/examples/traceability.md`: a condensed traceability chain example (one full requirement → one full Property → one full task + dependency graph fragment + traceability chain diagram)
- `SKILL.md` / `SKILL.zh.md`: Step 2 links updated to `file-templates.md` + `prompt-templates.md` + `examples/traceability.md`

### Switched the traceability chain diagram to Mermaid

- `references/examples/traceability.md`: replaced the ASCII diagram in `## The traceability chain` with a Mermaid `flowchart TD` (Property and Task each point to Requirement with labeled edges, preserving the two-way `Validates:` / `_Requirements:` semantics)
- `references/file-templates.md`: widened the Mermaid convention wording to "e.g. architecture layers, data flow, traceability", explicitly covering relationship/traceability diagrams
