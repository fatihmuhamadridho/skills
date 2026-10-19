---
name: test-driven-development
description: Use when adding or changing behavior and tests must be written or updated in a red-green-refactor workflow.
---

# TDD

Use this skill only when the user explicitly asks for tests, triggers `$test-driven-development`, or wants to work on unit testing and regressions directly.

## Core rules

- This skill is opt-in. Do not apply it automatically during normal coding work.
- Start from observable behavior, not implementation details.
- Write or update the smallest relevant test first.
- Make the test fail for the right reason, then implement the minimum fix.
- Refactor only after behavior is covered.
- Put new and updated tests in `__tests__/`.
- Do not create new tests under `__test__`.
- Prefer stable assertions over brittle snapshots or deeply mocked internals.
- Test critical paths, edge cases, and regressions that matter to the feature.
- Add tests for new features, behavior changes, and bug fixes.

## Repo-specific guidance

- For domain logic, test pure functions, use cases, and repository contracts directly.
- For presentation logic, test user-visible behavior and component interactions.
- Avoid over-mocking unless the dependency is external, slow, or unstable.
- Keep tests close to the touched feature and aligned with the current file layout.
- Prefer behavior-level assertions over implementation-coupled test setups.

## Decision guide

- If the user has not explicitly asked for tests, do not add or update unit tests yet.
- If the user explicitly asks for tests or invokes `$test-driven-development`, add or update a test before the implementation.
- If the change is purely mechanical and cannot affect behavior, explain why a test is unnecessary.
- If a test is fragile, prefer a simpler assertion over a clever one.

## References

- For repo-specific examples of domain tests, hook tests, page tests, and `__tests__` placement, read `references/test-examples.md`.
