# Test Examples

Use this reference when you need concrete examples from this repo to decide what to test first and where the test should live.

## Domain Test Examples

Domain tests are useful when behavior lives in use cases, models, controllers, or repository contracts.

- `src/features/ProductIndustry/__tests__/domain/product.usecase.test.ts`
  - Why: tests use case behavior and result shaping directly
- `src/features/ProductIndustry/__tests__/domain/product.controller.test.ts`
  - Why: tests controller orchestration and delegation between dependencies
- `src/features/ProductIndustry/__tests__/domain/model/product.model.test.ts`
  - Why: tests model behavior and edge-case construction rules

## Hook Test Examples

Hook tests are useful when behavior is primarily stateful or time-based and does not need a full page render.

- `src/commons/hooks/__tests__/useDebouncedValue.test.ts`
  - Why: verifies delayed state behavior directly at the hook level
- `src/commons/hooks/__tests__/useCountdownTimer.test.tsx`
  - Why: verifies time-based hook behavior and expiration flow
- `src/commons/hooks/__tests__/useInfiniteScroll.test.ts`
  - Why: verifies observable hook behavior without promoting the test to a page-level concern

## Presentation and Page Test Examples

Page and component tests are useful when the behavior is user-facing and best observed through rendering and interaction.

- `src/features/Login/__tests__/presentation/pages/Login.page.test.tsx`
  - Why: tests rendered page behavior with focused mocks around dependencies
- `src/features/Login/__tests__/pages/login.page.test.tsx`
  - Why: shows another route-facing test path in the repo, including interaction flow
- `src/features/Notification/__tests__/presentation/pages/Notification.page.test.tsx`
  - Why: page-level UI behavior can be asserted through rendering instead of lower-layer internals

## Test Placement Examples

- Shared hook tests live in `src/commons/hooks/__tests__/`
- Feature domain tests live in feature-local `__tests__/domain/`
- Presentation tests often live in:
  - `src/features/<Feature>/__tests__/presentation/...`
  - `src/features/<Feature>/presentation/<area>/__tests__/...`
- For new tests, prefer `__tests__/` and do not introduce new `__test__/` folders

## Choosing the Smallest Useful Test

- If the behavior is domain orchestration or data mapping, start with a domain test.
- If the behavior is hook state, timers, or internal UI flow without needing a full screen, start with a hook test.
- If the behavior is user-visible rendering or interaction, start with a page or component test.
- Do not jump to a page-level test if a smaller test would cover the behavior more directly.

## Validation Commands

- `npm run test`
- `npm run lint`
- `npm run typecheck`
- `npm run pre-commit`

## Watch-Outs

- Do not over-mock when the behavior can be asserted more directly.
- Do not write assertions tied too tightly to implementation details.
- Do not place new tests outside `__tests__/`.
- Be careful with large or redundant snapshot-style coverage when a behavior assertion is clearer.
