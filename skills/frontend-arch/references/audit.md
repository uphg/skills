# Audit Guide — Reviewing an Existing Project

How to detect architectural violations mechanically, ranked by what to fix first. Run the greps before reading code — they locate the disease; then judge severity in context.

## Table of Contents
- [Detection commands](#detection-commands)
- [Severity ranking](#severity-ranking)

## Detection commands

```bash
# Wildcard re-exports hiding module interfaces
grep -rn "export \*" src --include="*.ts" --include="*.tsx" --include="*.js"

# Deep imports crossing module boundaries (adjust pattern to your layout)
grep -rnE "from ['\"]@/(features|entities)/[^'\"]+/(model|api|ui)/" src

# Modules importing through their own index (cycle risk)
# look for `from "../"` or `from "./index"` inside module folders

# Catch-all files mixing domains
find src -name "utils.ts" -o -name "helpers.ts" -o -name "types.ts" -o -name "constants.ts" | xargs wc -l | sort -rn | head

# Circular dependencies (install madge)
npx madge --circular --extensions ts,tsx src

# Essence-named folders inside modules (desegmentation smell)
find src -type d \( -name components -o -name hooks -o -name types \) -not -path "*/node_modules/*"
```

Also check manually:
- Single-consumer code sitting in shared (open `shared/`, ask "who else imports this?" per file).
- Features named after UI location (`features/header`) instead of user actions.
- Token/session state stored inside a page or single feature.
- Business logic embedded in context-free shared UI components.

## Severity ranking

Fix in this order — earlier items cause the others:

1. **Circular dependencies** — break via coupling playbook: merge modules or push the shared part down a tier.
2. **Business logic duplicated across pages** — promote once to a lower tier; divergence multiplies bug-fix sites.
3. **Deep imports across module boundaries** — route through public APIs; this unblocks safe refactoring everywhere else.
4. **Single-consumer code in shared** — move back to its consumer; shrinks the riskiest tier.
5. **Catch-all / essence-named files and folders** — rename and split by domain; mechanical but touches many files, do it opportunistically per team ownership areas.

Don't try to fix everything in one pass: pick one violation class, sweep it, encode the fix as a lint rule so it can't return, then move to the next class.
