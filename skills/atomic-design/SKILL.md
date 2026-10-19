---
name: atomic-design
description: Use when building or refactoring UI that must follow the repo's Atomic Design conventions in commons-ui and feature presentation layers.
---

# Atomic Design

Use this skill when creating UI components or reorganizing feature UI.

## Core rules

- Keep the component hierarchy in order: `atoms` → `molecules` → `organisms` → `templates` → `pages`.
- Put reusable UI primitives in `src/commons-ui/*`.
- Keep feature-specific UI in the feature's `presentation` layer.
- Keep business logic out of reusable UI components.
- Use Tailwind plus `module.scss` where the repo expects both.
- Add relevant `__tests__/` for UI components that carry behavior.
- `*.stories.tsx` is only expected for components that need Storybook coverage.
- Do not default to native JSX tags for UI when a repo atom, molecule, organism, system, or template already exists.
- Use repo layout primitives such as `BoxAtom`, `FlexAtom`, and `ContainerAtom` instead of freeform `div` wrappers whenever they can express the same layout.
- Use repo interactive primitives such as `ButtonAtom`, input systems, menu systems, table systems, image systems, and icon atoms instead of raw `button`, `input`, `select`, `table`, or `img`.
- If the required UI primitive does not exist yet, create or extend the correct reusable component first instead of falling back to native HTML in feature code.
- Reuse components without changing the design's screen anatomy. Shared primitives should help reproduce the intended surface, not justify replacing it with another layout style.
- When translating Figma or screenshots, preserve the major UI structure first: title block, toolbar, content section, footer, navigation state, and page type.
- Do not let `presentation/pages/*` become the place where the full screen JSX lives. Put the screen composition in `templates` and let the page delegate to it.

## Layer guidance

- `atoms`: smallest reusable UI pieces with no business logic.
- `molecules`: small combinations of atoms with limited UI behavior.
- `organisms`: larger composed sections with feature-local UI concerns.
- `templates`: layout/orchestration components that assemble organisms into screens.
- `templates` may own layout orchestration or UI-level global state that does not belong in `pages`.
- `pages`: Next.js routing only.
- For management screens and other substantial surfaces, `pages` should typically pass props into one template, not render the full anatomy inline.

## Decision guide

- If a component is reusable across features, prefer `commons-ui`.
- If a component depends on feature data or feature state, keep it inside that feature.
- If a component only arranges layout and composition, prefer `template`.
- If a screenshot or Figma frame describes a full screen, map shell, title block, filter bar, content section, and footer controls into a `template` first before deciding whether smaller parts need organisms or molecules.
- Keep data handling in `pages` or `templates`, not in reusable atoms or molecules.
- If a component contains business rules, move those rules out before shipping the UI.
- If a layout can be expressed with existing layout atoms, do not introduce a raw wrapper tag.
- If an interaction can be expressed with an existing system component, do not rebuild it with native JSX.
- If no shared component exists, decide whether the missing piece belongs in `commons-ui` or the feature presentation layer, add it there, then compose from it.
- If shared components can only approximate the design by changing the page type or major hierarchy, build the missing abstraction instead of forcing the wrong composition.
- If the page file is accumulating large blocks of JSX for a real screen, that is a sign the template layer was skipped.

## References

- For repo-specific examples of `atoms`, `molecules`, `organisms`, `templates`, and `pages`, read `references/component-level-examples.md`.
