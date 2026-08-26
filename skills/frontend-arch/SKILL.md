---
name: frontend-arch
description: "Frontend architecture methodology distilled into four battle-tested principles: downward-only dependencies between tiers, explicit public APIs per module, colocate-first-promote-on-second-use, and domain-grouped purpose-named folders. Use when designing a frontend project's layer/module structure from scratch, restructuring or migrating a messy codebase without freezing feature work, deciding where code belongs (API calls, auth/tokens, types, shared UI, business logic), reviewing architectural violations like circular imports or utils dumps, or answering 'which folder should this go in' / 'should this be shared' questions."
---

# Frontend Arch

A methodology guide for frontend architecture: organizing code so that features stay findable, modules stay replaceable, and new team members can predict where things live. Distilled from layered architecture practice — deliberately minimal: ~30% of the structure cost, ~80% of the benefit.

## When to Use This Skill

- Scaffolding a new frontend project's directory structure
- Deciding where a piece of code belongs: requests, tokens, types, shared UI, business logic, layouts
- Reviewing a codebase for architectural violations (cycles, deep imports, catch-all files)
- Incrementally restructuring a messy codebase without freezing feature work
- Answering "should this be shared?" / "is this over-engineered?" questions

## The Four Principles

### 1. Dependencies point downward only
Arrange code in tiers by scope of responsibility — `app` (wiring: routing, providers, global styles) → `pages` (screen composition) → `features` (reused user actions) → `shared` (context-free infrastructure). A module may import from tiers strictly below it, never sideways and never up.

Why: sideways imports create cycles and hidden coupling; upward imports make modules impossible to extract or test in isolation. One-directional flow gives you a DAG you can lint and reason about.

### 2. Every module exposes an explicit public API
Consumers import only through the module's root (`index.ts` with named re-exports), never deep paths like `features/cart/model/pricing.ts`. No wildcard re-exports (`export *`) — they hide the real interface and leak internals by accident.

Why: with a public API, internals can be renamed and refactored freely; every export is a compatibility promise, so expose only what consumers actually need. Inside your own module, use full relative paths — importing through your own index creates cycles.

```js
// features/comments/index.ts
export { CommentCard } from "./ui/CommentCard";
export { fetchComments } from "./api/fetchComments";
```

### 3. Colocate first, promote on second use
New logic lives next to its only consumer — even if it "looks reusable". Extract to a lower tier only when a second real consumer appears.

Why: predicted reuse is usually wrong. Single-use code promoted early becomes global surface area everyone must maintain; demoting it later is harder than promoting it later. The later you promote, the safer the refactor.

### 4. Group by domain, name by purpose
Files serving one business area live together: `model/delivery.ts`, not `types.ts` mixing delivery + user shapes. Folders answer "what is this for", never "what kind of file is this".

Why: cohesion means one business reason to change per module. Essence-named folders (`components/`, `hooks/`, `utils/`, `assets/`) scatter related code across the tree, bloat searches, and mix unrelated domains in one file.

## Workflow

### Step 1: Map the vertical cuts
List the app's pages/screens first — each becomes a module folder (`pages/feed`, `pages/sign-in`). Screens are the natural decomposition; structure falls out of them before any code exists.

### Step 2: Set the tier floor
Start with three tiers only: `app`, `pages`, `shared`. Add a `features` tier when a user action (+ its UI) is genuinely reused across pages. Add a business-logic tier below features only if the client carries significant business rules — thin clients (backend does the logic) legitimately skip it. Do not pre-create empty tiers.

### Step 3: Place code by the table

| Code | Home |
|---|---|
| CRUD endpoint calls | `shared/api/endpoints/<resource>` — single consumer → owning module's `api/` instead |
| Business rules reused in ≥2 places | one tier below both consumers |
| Auth tokens / session data | `shared/api` or dedicated `shared/auth` — never inside a page or single feature |
| Context-free reusable UI | `shared/ui` (no business logic inside) |
| Screen-specific composition | `pages/<name>` |
| New / unstable logic | current page module; promote when stable |

Full placement tables for requests, tokens, types, assets, layouts, and state → read [references/placement.md](references/placement.md).

### Step 4: Wire the public APIs
One `index.ts` with named re-exports per module. Same-module imports: relative full paths. Cross-module imports: absolute alias through the public API. Per-component indexes under `shared/ui` when tree-shaking suffers.

### Step 5: Enforce mechanically
Encode the direction rule with ESLint `no-restricted-imports` patterns, dependency-cruiser, or bundler boundary rules. Convention without linting erodes under deadline pressure. Run `npx madge --circular src` in CI to catch cycles.

### Step 6: Grow on triggers
When writing code, check promotion triggers: a second real consumer appeared → extract downward. Two modules keep importing each other → merge or push down. Nothing else moves until a trigger fires.

## Coupling Playbook

When module A needs module B on the same tier, resolve in this order:

1. **Always change together?** → merge into one module.
2. **Shared part is pure logic?** → push it down a tier; both import legally from below.
3. **Needs B's content/UI?** → compose at a higher tier via render props / slots / DI; A and B stay mutually ignorant.
4. **Truly unavoidable?** → import strictly through B's public API and document the exception.

Strategies 1–3 eliminate the coupling; strategy 4 only bounds it — kept exceptions need documentation and periodic review, because unmanaged same-tier imports drift into bidirectional dependencies.

## Smells & Fixes

| Smell | Fix |
|---|---|
| Catch-all files (`utils.ts`, `helpers.ts`, `types.ts` mixing domains) | split per domain: `model/delivery.ts`, `model/user.ts` |
| Folder named by essence (`components`, `hooks`, `utils`, `assets`) | rename by purpose (`ui`, `model`, `api`) |
| `export *` in an index | named exports only |
| Deep import into another module's internals | route through its public API |
| Two modules importing each other | merge, or push shared part down |
| Single-consumer code sitting in shared | move back to its consumer |
| Feature named after UI location (`header`) | name after the user action (`logout`) |

## References

- [references/placement.md](references/placement.md) — read when placing specific artifact kinds: API requests, token/session storage, type colocation, assets, layouts, state
- [references/restructuring.md](references/restructuring.md) — read when migrating an existing messy codebase: gate question, incremental stages, legacy-folder mapping
- [references/audit.md](references/audit.md) — read when reviewing an existing project: grep commands to detect violations and what to fix first
