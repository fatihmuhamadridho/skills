# Feature Slice Examples

Use this reference when you need concrete examples from this repo to decide how to slice a feature, what should stay feature-local, and what should move to shared code.

## Vertical Slice Examples

These features show what an end-to-end slice looks like in practice.

- `Campaign`
  - Path: `src/features/Campaign/`
  - Why: contains `domain`, `infrastructure`, and `presentation`, which makes it a clear example of a full feature slice
- `AuditTrail`
  - Path: `src/features/AuditTrail/`
  - Why: also shows a feature with all three layers and a route-facing page/template flow
- `MobileDevice`
  - Path: `src/features/MobileDevice/`
  - Why: heavily presentation-focused slice with hooks, templates, pages, and feature-specific UI orchestration

## Routing-Only Page Examples

Pages in `src/pages/*` should stay thin and delegate to the feature entrypoint.

- `src/pages/campaign/index.tsx`
  - Why: exports the feature page and SSR wrapper without owning business logic
- `src/pages/mobile/device/index.tsx`
  - Why: forwards directly into the `MobileDevice` feature
- `src/pages/auth/login/index.tsx`
  - Why: route entrypoint that only wires layout props and feature page export

## Shared Extraction Examples

These examples belong in `src/commons/*` because they are generic or shared across multiple features.

- `withLayoutProps`
  - Path: `src/commons/utils/layoutProps.ts`
  - Why: route-level SSR layout helper reused across page entrypoints
- `useAuth`
  - Path: `src/commons/core/Auth/infrastructure/auth.hook.ts`
  - Why: auth access is cross-feature and not owned by one product slice

## Feature-Local Logic Examples

These examples should stay inside their feature because they encode one feature's flow or UI behavior.

- `useTableFilters`
  - Path: `src/features/Organization/presentation/hooks/useTableFilters.ts`
  - Why: even though it looks reusable, it is still tied to the organization table flow and filter behavior
- `useCampaignFormHeaderLogic`
  - Path: `src/features/Campaign/presentation/logics/campaignFormHeader.logic.ts`
  - Why: campaign-specific form orchestration should not be extracted to shared code
- `useMobileDevicePageForm`
  - Path: `src/features/MobileDevice/presentation/hooks/useMobileDevicePageForm.ts`
  - Why: packages the Mobile Device form flow, router handling, and template binding for one feature

## Keep It in the Feature vs Move It to Commons

- Keep it in the feature if the logic serves one business flow, one feature's screen behavior, or one feature's vocabulary.
- Move it to `src/commons/*` only when the code is generic, reusable, and free from feature-specific assumptions.
- Do not extract to shared code just because two files look similar once; wait until the abstraction is stable and clearly reusable.

## Feature Slice Heuristics

- Start from the user-visible slice, not from the folder structure.
- Keep ownership clear: each feature should be understandable as one product slice.
- Let `src/pages/*` route into features instead of rebuilding feature logic there.
- If two features must interact, prefer a shared abstraction instead of direct imports between them.
