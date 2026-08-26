# Placement Guide — Where Does This Code Go?

Full decision tables per artifact kind. All of it follows one principle: colocate with usage, promote on demonstrated reuse. Placement is decided by **scope and responsibility**, never by structural complexity.

## Table of Contents
- [API requests](#api-requests)
- [Auth tokens & session](#auth-tokens--session)
- [Types](#types)
- [Assets](#assets)
- [Layouts](#layouts)
- [State](#state)

## API requests

| Request serves | Home |
|---|---|
| Many parts of the app | `shared/api` — `client.ts` (base URL, headers, serialization), `endpoints/<resource>.ts`, public `index.ts` |
| Exactly one page/module | that module's `api/` segment; no need to export through its public API |

Rules:
- Keep the request function + DTO interface + mapper as ONE cohesive unit — they change together.
- Do not move API calls into a business-logic tier prematurely: backend response shapes usually differ from what the frontend needs; transform in `shared/api` or the module's `api/`.
- Validation schemas colocate with their trigger: backend-data schemas next to the request; form-input schemas next to the form.

## Auth tokens & session

The token is application-wide state used by multiple authenticated flows — if two different modules would need it, it belongs to neither of them.

| Option | Where | Trade-off |
|---|---|---|
| Cookie sessions | server-side infra only | no client architecture needed — prefer when possible |
| Client state in api module | reactive store inside `shared/api` | simplest; add auto-refresh middleware (on 401-expiry: refresh → retry original request) |
| Dedicated auth module | `shared/api` + `shared/auth` | best for complex refresh/token-rotation logic |
| Session entity | business tier below features (`model` store) | rich current-user logic; needs an exposure pattern to reach `shared/api` |

Exposing a higher-tier token to the lower-tier api client (three standard patterns):
1. Pass manually per request — simplest, cumbersome.
2. Context/global-store "pull" — declarative; key kept in `shared/api`.
3. Reactive "push" — subscribe the session store, update the client on change.

Auto-logout fires whenever the refresh token is rejected — even if the server-side logout call failed, reset local token/user state regardless.

## Types

Types have no special status: place every type by its usage location and purpose, never in a `types` bucket.

| Type | Location |
|---|---|
| Utility types | `shared/lib/utility-types` (+ README stating scope) or next to sole consumer |
| DTOs & mappers | next to the request functions using them |
| Enums | near usage; folder = what it represents (`ui` for design tokens/toast positions, `api` for statuses) |
| Props / context interfaces | same file as component, or sibling file |
| Ambient `*.d.ts` | `app/ambient/`; typings for untyped packages → `shared/lib/untyped-packages/<lib>.d.ts` |
| Generated (OpenAPI) | dedicated folder e.g. `shared/api/openapi` + regeneration README |
| Whole-app types (Redux `RootState`) | declared globally at the top tier so lower tiers' typed hooks can consume without importing up |

When two business types reference each other across modules (Song → Artist): parametrize with generics if clean (`interface Song<A extends { id: string }>`), else re-export through an explicit cross-module public API file — never hidden middleware indirection.

## Assets

Group by use case, keep close to consuming code. A blanket per-slice or global `assets/` segment groups by kind and breaks cohesion.

| Asset | Location |
|---|---|
| Module-specific images/icons | inside the module |
| Reusable icons/images | `shared/ui/` |
| Global styles, fonts | top tier (`app/styles`, `app/fonts`) |
| Favicon, unprocessed static files | bundler's `public/` root |

## Layouts

Placement by scope and responsibility:

| Scope | Location |
|---|---|
| Entire app / routing shell | top tier (`app`) |
| Specific page or route group | that page module |
| Context-free reusable structure | `shared/ui` |
| Flow-centric layout reused across pages | matching feature module |

A shared layout must not import upward for dynamic content — inject via render props/slots, configure route nesting above, or define it in the page instead.

## State

| Kind | Home |
|---|---|
| Server cache | query layer (`shared/api` factories, or owning module) — see query-factory pattern: hierarchical keys `all → lists → list`, leaves return `queryOptions({queryKey, queryFn})` |
| URL-representable view state (filters, tabs, pagination) | URL search params — browser persists; often eliminates client state entirely |
| Single-page UI state | local to that page's components |
| Cross-feature app state (auth, theme) | dedicated module at a low tier |
| Mutations | colocated with the triggering feature/page; update caches in `onSuccess` |
