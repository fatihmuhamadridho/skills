---
name: repo-conventions
description: Use when applying Patricia Dashboard coding conventions, completion checks, and repo-wide anti-pattern rules that are not specific to one architecture skill.
---

# Repo Conventions

Use this skill when checking whether a change follows this repo's coding standards and done criteria.

## Coding rules

- Use TypeScript strict mode assumptions.
- Avoid `any`.
- Use clear, descriptive naming.
- Avoid uncommon abbreviations.
- Use `kebab-case` for files.
- Use `PascalCase` for components.
- Use `camelCase` for functions and variables.
- Keep one function focused on one responsibility.
- Avoid deeply nested logic when a simpler structure is available.
- Follow existing patterns before introducing a new variation.

## Done criteria

- The change follows the existing feature structure.
- Relevant tests are added or updated only when the user explicitly asks for testing work or invokes `$tdd`.
- `npm run lint` should pass.
- `npm run test` should pass for the touched area only when testing is explicitly in scope.
- `npm run typecheck` should pass when the change touches shared typing or cross-layer interfaces.
- When adding a new page route, treat route integration as incomplete until `src/middleware.ts` and `src/commons/configs/permission.ts` are checked.
- When adding a new page route, verify whether `ROUTE_PERMISSIONS` needs a new entry, whether an existing permission can be reused, and whether `PERMISSION_LIST` needs a new permission slug.
- Do not mark a new route as complete unless it is reachable after login through the repo's actual middleware and permission flow.
- When adding or changing UI, verify that existing shared components were searched and reused before introducing new JSX structure.
- When adding a new page route that should appear in navigation, also check the sidebar or menu data source instead of stopping at the page export.
- Distinguish between repo-controlled navigation and backend-controlled navigation. State clearly which one is the source of truth before claiming the page is fully reachable from the menu.
- If a UI request comes with strong visual evidence such as Figma screenshots or frame exports, do not change the screen type during implementation just because a nearby existing page is easier to reuse.
- For substantive feature screens, keep `presentation/pages/*` orchestration-only and move screen composition into a `presentation/templates/*` component.

## Anti-patterns

- Business logic in reusable UI.
- New folders without a clear reason.
- Adding or changing unit tests without the user explicitly putting testing in scope.
- Violating architecture boundaries without justification.
- Adding a page route without checking `src/middleware.ts`, `ROUTE_PERMISSIONS`, or the underlying permission slug coverage.
- Claiming a new page is done when the route exists in `src/pages/*` but the middleware or permission map still blocks it.
- Treating a backend-driven sidebar as if it were fully controllable from the frontend repo.
- Reusing shared UI in a way that changes the intended surface type from the design, such as turning a list-management page into a dashboard.
- Writing raw UI with native `div`, `button`, `input`, `select`, `table`, `img`, or similar tags when an existing repo component can express the same behavior or structure.
- Treating native HTML as the default path for Figma or UI implementation instead of reusing or extracting repo components first.
- Rendering an entire feature screen directly inside `presentation/pages/*` when the page should delegate to a template.

## Decision guide

- If the task is mainly about architecture, UI composition, testing workflow, or feature slicing, use the dedicated skill for that topic alongside this one if needed.
- If the task only needs repo-wide style and completion rules, this skill is enough on its own.

## References

- For repo-specific examples of naming, shared extraction, done criteria, and watch-outs, read `references/repo-examples.md`.
