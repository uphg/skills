# Restructuring Guide — Incremental Migration

Migrate incrementally while shipping features. Big-bang rewrites halt development and lose stakeholder support; forced adoption without team consensus produces a formal structure nobody follows.

## Table of Contents
- [Gate question](#gate-question)
- [The five stages](#the-five-stages)
- [Legacy folder mapping](#legacy-folder-mapping)
- [Migration anti-patterns](#migration-anti-patterns)

## Gate question

Migrate only if at least one holds — otherwise the current architecture works and shouldn't be touched:

1. New team members complain it's hard to reach productivity.
2. Modifying one part often breaks an unrelated part.
3. Adding functionality is hard due to the sheer number of things to track.

Get the team convinced before starting (never impose as lead), and management on board with: incremental migration doesn't halt features, a documented standard speeds onboarding, the team stops maintaining private architecture docs.

## The five stages

Setup once: add a `@` → `./src` alias so top-level moves don't rewrite every import.

### Stage 1: Divide by pages
Create `pages/`; move component code out of route files into page folders with an `index.js`. Routes become thin adapters (`export { ProductPage as default } from "@/pages/product"`). Inter-page imports are tolerated **at this stage only**.

### Stage 2: Separate everything else from pages
Binary classification: code that imports pages/routes → top tier (`app`); everything else → `shared`.

### Stage 3: Kill page-to-page imports
For each remaining page→page import: copy-paste if it's presentational/config code likely to diverge; promote to `shared` if it's genuinely common (UI kit → `shared/ui`, constants → `shared/config`, backend calls → `shared/api`). Never duplicate business logic — bug fixes must land once.

### Stage 4: Unpack shared again
Find objects used by only ONE page and move them into that page's module — including actions, reducers, selectors. Colocation beats grouping "all actions together". Shared is a dependency of every other tier, so it must stay small and truly reused.

### Stage 5: Organize by technical purpose
Split each module into purpose folders: `ui/`, `api/`, `model/` (stores, schemas, business logic), `lib/`, `config/`. Dissolve legacy folders per the mapping table below.

Optional later stages, on triggers only:
- Extract business-logic modules reused across several pages into a lower business tier.
- Build a clean context-free `shared/ui`: no business logic encoded in it; extract logic upward or copy-paste where usage is sparse.

## Legacy folder mapping

| Legacy | Destination |
|---|---|
| `routes/`, `screens/`, `views/` | thin files stay in routing; bodies → `pages/<name>/ui` |
| `components/`, `containers/` | mostly `shared/ui` after extracting business logic upward |
| `helpers/`, `utils/` | group by function → `shared/lib` |
| `constants/` | group by function → `shared/config` |
| `actions/`, `reducers/`, `selectors/` (single page) | that page's `model/` |
| same, but reused across pages | business tier / feature modules |
| `modules/` (business logic) | feature-tier modules; large UI chunks stay near their pages |
| `services/`, `api/` | `shared/api` or owning module's `api/` |

## Migration anti-patterns

- **Big-bang rewrite** — halts features, loses support; always incremental.
- **Purpose folders named by artifact type** — creating `components/`, `types/`, `utils/` inside modules just relocates the smell.
- **Bloated shared** — dumping single-use state/actions there makes the highest-risk dependency riskier.
- **Copy-pasting business logic** — acceptable for presentational/config code only.
- **Fine-grained extraction carried forward** — single-consumer modules are usually noise; merge them into their consumer first.
