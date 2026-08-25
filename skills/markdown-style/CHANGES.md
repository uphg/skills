# Changelog

## 2026-08-25 — Remove heading-hierarchy rules

Maintainer decision: heading conventions do not need skill-level constraints. Removed the entire workflow step — superseding the bare-headings-default revision made earlier today — so the skill no longer prescribes anything about heading hierarchy or numbering.

### Changes

### 1. `SKILL.md` → revised

- Removed "Step 2: Apply Heading Hierarchy" (bare-heading default, formal-document numbering exception, 总结 final-section convention).
- Renumbered workflow steps 3–7 → 2–6.
- Description: dropped the "heading hierarchy" coverage claim; scope is now code formatting, CJK spacing, list/table structure, and language polishing.
- When-to-use: trimmed the inconsistent-headings bullet (spacing rules still cover blank lines around headings).
- Prohibitions: removed the mixed-numbering-systems rule — meaningless without numbering guidance.

### 2. `SKILL.zh.md` → synced

Structurally identical Chinese version updated with the same changes.

### 3. `README.md` / `README.zh.md` → updated

One-liner descriptions no longer mention title hierarchy.

## 2026-08-25 — Description rewrite

Reworked the front-matter description: verb-led opening, quoted trigger phrasings users actually type, list/table structure added to coverage, explicit negative boundary. No skill content changed.

### Changes

### 1. `SKILL.md` → description revised

- Opening now verb-led: "Format, polish, and standardize …" instead of "Markdown document authoring, formatting …".
- Added concrete trigger phrasings: "clean up this README", "polish this doc", pasted draft text destined for a .md file.
- Coverage now mentions list/table structure alongside heading hierarchy, code formatting, CJK spacing.
- Added negative boundary: not for rendering markdown to HTML/PDF or static site builds.

### 2. `SKILL.zh.md` → synced

Structurally identical Chinese version updated with the same changes.

Initial creation: generated the Markdown document style optimization skill from `src/markdown/README.md`.

## Changes

### 1. `SKILL.md` → new

Distilled from `src/markdown/README.md` into SKILL format, including:

- Heading hierarchy: dual mode with Chinese numbered headings (for Chinese documents) and conventional headings (for English documents)
- Code and technical term formatting: backticks, fenced code block language tags
- CJK spacing and punctuation rules: CJK-Latin spacing, full-width/half-width punctuation rules
- Structural spacing rules: blank-line conventions for headings, paragraphs, code blocks, and lists
- Language polish rules: disambiguation, splitting long sentences, active voice, 的/地/得 differentiation, terminology consistency
- Quick reference: text styles, list formats, tables, quotes and footnotes
- Prohibitions: do not add new content, do not change data, do not use emoji
- Edge cases: English translation, emoji removal, fragmented reorganization, handling contradictory information

### 2. `SKILL.zh.md` → new

Chinese version, structurally identical to `SKILL.md`; only the language differs.
