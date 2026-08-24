# Changelog

## 2026-04-30 — Modular split

Split the main document into a main file + `references/` pattern, consistent with the vue-tsx skill.

### Changes

### 1. `SKILL.md` → trimmed (~158 lines)

Kept the core content: when to use, workflow, architecture layer overview table, core constraints, architecture concepts, decision flow, reference file index, prohibitions, and fallback behavior.

### 2. `references/layer-details.md` → new

Moved the detailed descriptions of the six layers (shared / entities / features / widgets / pages / app) out of the main document, including each layer's directory structure, positioning, and characteristics.

### 3. `references/ecommerce-example.md` → new

Moved the full e-commerce project directory tree example out of the main document.

### 4. `SKILL.zh.md` → synced trim

The Chinese version was adjusted to the same structure and is fully aligned with the English version's sections (158 lines).

### 5. `references/layer-details.zh.md` → new

Chinese version of the layer details.

### 6. `references/ecommerce-example.zh.md` → new

Chinese version of the e-commerce project example.

---

## 2026-04-28 — Initial version

Created the FSD (Feature-Sliced Design) frontend architecture skill document.

### Changes

### 1. `SKILL.zh.md` → new

Chinese skill document covering:

- Core constraints (unidirectional dependencies, public APIs, grouping by business, shared purity)
- Six-layer architecture in detail (app / pages / widgets / features / entities / shared)
- The concepts of slices and segments
- E-commerce project example
- Decision tree: where does this code go?
- Prohibitions

### 2. `SKILL.md` → new

English skill document, translated from SKILL.zh.md.
