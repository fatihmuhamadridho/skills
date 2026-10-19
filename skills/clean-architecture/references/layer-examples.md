# Layer Examples

Use this reference when you need concrete examples to decide whether code belongs in `domain`, `application`, `infrastructure`, or `presentation`.

## Domain

Domain holds business-oriented types, models, repository contracts, and orchestration that should stay framework-light and API-agnostic.

- **Repository contract**
  - Example path: `src/features/<Feature>/domain/<feature>.repository.ts`
  - Why: defines the repository contract that upper layers depend on without knowing HTTP or transport details

- **Result and response type definitions**
  - Example path: `src/features/<Feature>/domain/<feature>.type.ts`
  - Why: separates domain-facing result shapes from raw response DTO shapes

- **Use case (only when real orchestration is needed)**
  - Example path: `src/features/<Feature>/domain/<feature>.usecase.ts`
  - Why: orchestrates repository access and maps raw response data into domain models and result objects; do not create if the only job is forwarding calls

- **Domain models / entities**
  - Example path: `src/features/<Feature>/domain/models/*`
  - Why: represent business entities instead of UI-only or transport-only objects

## Application

Application holds DTOs, request/result contracts, and use cases that coordinate domain and infrastructure.

- **Feature-level public contracts**
  - Example path: `src/features/<Feature>/application/<feature>.interface.ts`
  - Why: central place for request types, result types, and repository error types exposed upward; infrastructure implements them, not the other way around

- **Application-specific DTOs**
  - Example path: `src/features/<Feature>/application/<feature>.dto.ts`
  - Why: lightweight shapes for flows that do not need full domain invariants, such as list summaries or delete acknowledgements

## Infrastructure

Infrastructure talks to external systems and returns data in shapes that domain or upper layers can consume.

- **Repository implementation**
  - Example path: `src/features/<Feature>/infrastructure/<feature>.repository.impl.ts`
  - Why: performs HTTP requests, gates on documented internal status codes before returning normalized domain data, and exposes only sanitized error contracts to presentation

- **Infrastructure mapper**
  - Example path: `src/features/<Feature>/infrastructure/<feature>.mapper.ts`
  - Why: keeps pure response-to-model transformation in one place instead of scattering it across repository methods

- **Infrastructure constants**
  - Example path: `src/features/<Feature>/infrastructure/<feature>.constants.ts`
  - Why: holds backend status code maps, transport error code maps, and endpoint-specific constants so the repository implementation stays orchestration-focused

- **Transport response types**
  - Example path: `src/features/<Feature>/infrastructure/<feature>.response.ts`
  - Why: raw BE response or transport-facing types only; application request/result contracts do not belong here

- **Infrastructure hook (for UI frameworks with query libraries)**
  - Example path: `src/features/<Feature>/infrastructure/<feature>.hook.ts`
  - Why: bridges domain or use case methods into React Query, SWR, or mutation wiring for the UI layer; for migrated features, may instantiate repositories and use cases directly when a domain controller would only act as a thin proxy

## Presentation

Presentation owns UI orchestration, UI state, route-facing screens, and feature-level bindings needed by templates and pages.

- **Page / screen component**
  - Example path: `src/features/<Feature>/presentation/pages/<Feature>.page.tsx`
  - Why: orchestrates screen behavior, disclosure state, queries, mutation calls, and user interaction handling; entry point is routing-only

- **Presentation hook**
  - Example path: `src/features/<Feature>/presentation/hooks/use<Feature>PageForm.ts`
  - Why: packages UI-specific form behavior, router actions, and feature-level UI state for a flow

- **Feature template**
  - Example path: `src/features/<Feature>/presentation/templates/<Feature>Form.template.tsx`
  - Why: renders the screen structure and binds prepared handlers/state to feature UI components

## Shared Code

- Generic reusable utilities belong in a shared commons directory, not in feature folders.
- UI-only building blocks belong in a separate shared UI directory.
- Do not let shared packages import feature-specific code or mix domain rules with transport helpers.

## Common Boundary Decisions

- If the code transforms API payloads into domain models or normalizes result shapes, it belongs in `domain` orchestration or an infrastructure mapper, not in a page component.
- If the API contract is partial or nullable, keep that uncertainty in DTOs and normalize it before constructing domain entities.
- If the code performs HTTP calls, token persistence, cookie writes, or datasource coordination, it belongs in `infrastructure`.
- If the code wires query libraries, form bindings, modal state, router navigation, or screen interactions, it belongs in `presentation`.
- If a page grows large because it owns too much UI behavior, move that behavior into a presentation hook or feature template.
- If both a controller and a presentation hook could solve the problem, prefer the hook for new or migrated feature work unless the logic is truly domain orchestration.
- If the only job of a controller is forwarding calls from a hook to use cases, remove it and keep that wiring in `infrastructure`.
- If the only job of a use case is mapping transport data and forwarding repository calls, remove it and keep that mapping in `infrastructure`.
- If the API docs provide internal status codes, enforce those codes in the repository before returning any mapped result object.
- If a presentation hook needs to branch on a business failure, branch on a sanitized repository error field like `type`, not on raw transport status codes.

## Quick Heuristics

- Ask "does this know transport details?" If yes, it is not `domain`.
- Ask "does this know UI framework state, router, or UI events?" If yes, it is `presentation`.
- Ask "would another UI surface still need this rule?" If yes, it may belong in `domain`.
- Ask "is this touching external IO, persistence, cookies, or network?" If yes, it belongs in `infrastructure`.
