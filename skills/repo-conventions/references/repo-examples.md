# Repo Examples

Use this reference when you need concrete examples from this repo to apply naming, completion checks, and repo-wide conventions.

## Good Naming Examples

- `LoginPage`
  - Path: `src/features/Login/presentation/pages/Login.page.tsx`
  - Why: page component uses `PascalCase` and the file name is descriptive for the route-facing screen
- `useDebouncedValue`
  - Path: `src/commons/hooks/useDebouncedValue.ts`
  - Why: hook follows `camelCase` naming with the `use` prefix and has one focused responsibility
- `withLayoutProps`
  - Path: `src/commons/utils/layoutProps.ts`
  - Why: shared utility name is explicit about what it adds to the page layer

## Good Extraction Examples

- `withLayoutProps`
  - Path: `src/commons/utils/layoutProps.ts`
  - Why: generic SSR helper used by multiple route entrypoints, so `commons` is the right owner
- `useAuth`
  - Path: `src/commons/core/Auth/infrastructure/auth.hook.ts`
  - Why: authentication access is cross-feature and should not be reimplemented inside a single product slice
- `useDebouncedValue`
  - Path: `src/commons/hooks/useDebouncedValue.ts`
  - Why: reusable hook with no feature-specific vocabulary or business rules

## Test Placement and Validation Examples

- `__tests__` directories exist across shared and feature code, for example:
  - `src/commons/hooks/__tests__/`
  - `src/features/Organization/__tests__/`
  - `src/features/MobileDevice/presentation/templates/__tests__/`
- Validation commands come from `package.json`:
  - `npm run lint`
  - `npm run test`
  - `npm run typecheck`
- `pre-commit` currently points to lint:
  - `npm run pre-commit`

## Watch-Out Examples

- `ProductIndustry.page`
  - Path: `src/features/ProductIndustry/presentation/pages/ProductIndustry.page.tsx`
  - Why it is a watch-out: the file carries a large amount of UI orchestration, inline helpers, local state, and mutation coordination in one page component
  - What to avoid: adding more screen logic to already large pages when a presentation hook, template, or feature-local helper would reduce coupling
- `any` usage in feature UI flow
  - Path: `src/features/ProductIndustry/presentation/pages/ProductIndustry.page.tsx`
  - Why it is a watch-out: multiple `any`-typed helpers weaken type safety and make future refactors harder
  - What to avoid: copying loose typing into new code when a real type can be introduced
- Mixed legacy naming
  - Example areas: repo still contains a mix of names like `Login.page.tsx`, `PinInputMolecule`, and older component/file naming patterns
  - What to avoid: copying inconsistent legacy naming into new files when the repo convention for new code is already clear

## Heuristics

- Prefer the clearest name over the shortest name.
- Extract to `commons` only when the code is truly reusable and not feature-shaped.
- Treat lint, tests, and typecheck as completion gates, not optional cleanup.
- When you see a large page or template growing further, consider feature-local extraction before adding more inline logic.
