---
name: feature-driven-development
description: Use when organizing work as feature-driven development so each feature stays isolated, end-to-end, and aligned with this repo's feature structure.
---

# FDD

Use this skill when planning or implementing work as a feature slice in this repo.

## Core rules

- Treat each feature as an end-to-end vertical slice.
- Keep feature code isolated from other features; do not create direct feature-to-feature dependencies.
- Use abstractions when features must collaborate.
- Keep feature structure aligned with:
  - `domain` for models, types, use cases, repository contracts, controllers
  - `infrastructure` for repository implementations and API adapters
  - `presentation` for UI, hooks, page templates, and feature-local UI state
- Keep `src/pages/*` routing-only; feature SSR/ISR and main data fetching should come from the feature.
- Put shared logic in `src/commons/*` only when it is generic and reusable.
- Keep UI primitives in `src/commons-ui/*` and feature-specific UI inside the feature.
- Keep `presentation/pages/*` orchestration-only. For substantive screens, the page should ideally import one feature `Template` and wire data or handlers into it.
- Keep major screen anatomy in `presentation/templates/*`, then split sections into `organisms` or `molecules` when the screen becomes large or stateful.

## Planning guidance

- Start from the feature outcome, not from the file tree.
- Group work by user-visible slice, then decide which layer owns each responsibility.
- If a change touches more than one feature, extract a shared abstraction instead of coupling features directly.
- If a page contains feature behavior, move that behavior into the feature before finishing.
- Keep SSR, ISR, and primary fetch orchestration with the feature instead of re-implementing it in `src/pages/*`.
- If design evidence shows a new user-facing surface that does not already exist in the repo, create a new feature slice by default instead of attaching it to the closest existing feature.
- Do not merge a new Figma surface into an existing feature unless the repo already proves that both screens belong to the same feature boundary.
- If a page file starts rendering most of the screen JSX itself, stop and extract a template before considering the work complete.

## Decision guide

- If logic is reusable across features and has no feature-specific rules, move it to `src/commons/*`.
- If logic belongs to one user flow, keep it inside that feature.
- If logic only routes or composes feature entry points, keep it in `src/pages/*` or feature templates.
- If a feature page represents a management page, detail page, form flow, or any other substantial screen, create a `Template` and keep the page focused on orchestration.
- Only skip a dedicated template when the screen is genuinely trivial and there is no meaningful composition to extract.
- If the request is a new page from design, decide the feature boundary first, then create the route wrapper after the feature target is clear.

## References

- For repo-specific examples of feature slices, routing-only pages, and shared extraction decisions, read `references/feature-slice-examples.md`.
