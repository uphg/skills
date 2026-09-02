# Changelog

## 2026-09-02 — Added expectations to evals.json

Each eval in the existing evals.json now carries an `expectations` array (verifiable statements) per the skill-creator schema, enabling automated grading and benchmark runs. Prompts and `expected_output` unchanged.

## 2026-08-26

### Renamed to `frontend-arch`

- Renamed the skill from `frontend-boundaries` to `frontend-arch`, repositioning it as a general frontend architecture methodology skill.
- Rewrote frontmatter `description` to cover methodology-level triggers (designing layer/module structure from scratch, restructuring or migrating legacy codebases) alongside the existing placement and review scenarios.
- Normalized the Chinese directory to repo conventions: `SKILL.md` → `SKILL.zh.md`, added `CHANGES.zh.md`.

## 2026-08-25

### Initial release

- Created `frontend-boundaries`: frontend code organization built on four principles — downward-only dependencies, explicit public APIs, colocate-first-promote-on-second-use, domain-grouped purpose-named folders.
- Body keeps the decision tables and coupling playbook lean; per-artifact placement detail (`references/placement.md`), incremental restructuring stages (`references/restructuring.md`), and mechanical violation detection (`references/audit.md`) live in references.
- Added three test prompts in `evals.json` covering restructuring, token placement, and cross-import resolution.
