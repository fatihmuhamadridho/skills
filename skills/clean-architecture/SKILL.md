---
name: clean-architecture
description: Use for reviewing or refactoring codebases with clean architecture, layer boundaries, dependency direction, and business-rule isolation.
---

# Clean Architecture

## Overview

Use this skill to review an existing codebase through boundary ownership and dependency direction, then propose the smallest structural changes that make responsibilities clearer.

Default to incremental cleanup. Do not recommend a full rewrite when adapters, seams, or boundary extraction can fix the problem with less risk.

## Core Workflow

1. Identify the concrete surface being reviewed: feature, module, service, route, screen, or job.
2. List the current responsibilities before trusting folder names.
3. Classify code into `domain`, `application`, `interface/adapters`, or `infrastructure/presentation` based on what it does.
4. Trace dependencies inward and note every place where outer concerns leak into inner layers.
5. Recommend the minimum refactor that restores dependency direction and reduces coupling.

## Default Layer Model

Use these terms by default:

- `domain`
- `application` or `use case`
- `interface/adapters`
- `infrastructure` or `presentation`

Map repo-specific terms into this model instead of forcing a rename immediately.

Examples:

- `core`, `model`, or `entities` often map to `domain`
- `services`, `actions`, or `usecases` often map to `application`
- `controllers`, `presenters`, `mappers`, or `gateways` often map to `interface/adapters`
- `http`, `db`, `orm`, `ui`, `framework`, or `sdk` often map to `infrastructure/presentation`

## Classify Responsibilities

### Domain

Treat code as `domain` when it represents business rules, invariants, policies, or concepts that should survive framework changes.

Common examples:

- Entities
- Value objects
- Domain services
- Validation rules tied to business meaning
- Policy calculations

`domain` may know:

- Domain concepts
- Business rules
- Stable abstractions that do not expose technical details

`domain` must not know:

- HTTP
- Database drivers or ORM models
- UI framework state
- Queue clients
- File system details

Keep `domain` pure: no side effects, no framework coupling, no external API calls.

Do not mirror transport-level optionality in domain entities. API DTOs may be partial, but domain models should represent normalized business invariants.

Remove example-only aliases or enums from domain models, but keep real business vocabularies as enums when they protect the domain from invalid string values.

### Application / Use Case

Treat code as `application` when it coordinates work to fulfill a user or system action.

Common examples:

- Create order
- Sync account
- Approve invoice
- Load dashboard use case
- Export report workflow

`application` may know:

- Domain types and rules
- Ports or repository interfaces
- Transaction or orchestration policy
- Input and output models for use cases

`application` must not know:

- Concrete transport details
- Controller objects
- ORM-specific query APIs
- UI widget trees

Keep repository contracts in `application`, not in `domain`, when the repository mainly describes application-facing data access and orchestration boundaries.

Prefer feature-level public contract files in `application` such as `<feature>.interface.ts` for request types, result types, repository error types, and other application-facing interfaces.

Do not import repository request types, result types, or repository error contracts from `infrastructure` into `application`. Those public contracts belong to `application`, and `infrastructure` may implement or consume them.

Application DTOs may be partial and use-case-specific. They are allowed to model lightweight flows such as list, detail summary, or delete responses that do not require full domain invariants.

Keep DTO validation minimal at the application boundary. Validate only the fields required for that DTO contract and do not move rich business validation out of the domain just to satisfy DTO construction.

Do not let DTOs replace domain entities in flows that require business rules, invariants, or domain behavior.

Treat use cases as orchestrators. Do not create one unless new orchestration is actually needed. If a use case only maps API payloads/results and forwards repository calls, remove it and move that mapping into the repository implementation instead.

### Interface / Adapters

Treat code as `interface/adapters` when it translates between external shapes and inner use-case shapes.

Common examples:

- Controllers
- Presenters
- View models
- Repository adapters
- DTO mappers
- Serializer/deserializer boundaries

Use adapters for:

- Translating request and response formats
- Converting persistence models to domain models
- Preparing UI-facing state from use-case output

Adapters should remain thin. If business policy starts living here, move it inward.

### Infrastructure / Presentation

Treat code as `infrastructure/presentation` when it depends directly on frameworks, runtimes, or external systems.

Common examples:

- Database implementations
- HTTP framework handlers
- SDK clients
- Filesystem access
- Message broker producers
- UI framework components and screens

Outer layers may depend inward. Inner layers must not depend outward.

Put API clients, response shaping, and status validation in `infrastructure`.

Keep backend transport contracts in `infrastructure`. Files such as `<feature>.response.ts` or `<feature>.interface.impl.ts` should contain only raw transport-facing types, not application request/result contracts.

Browser or client storage concerns such as localStorage/sessionStorage caching and offline fallback belong in infrastructure datasources, and repository methods should delegate those reads and writes instead of handling storage directly.

Keep pure response-to-model transformation in dedicated infrastructure mappers such as `<feature>.mapper.ts` instead of scattering mapping logic across repository methods.

Keep backend status code maps, transport error code maps, endpoint-specific constants, and similar external-service constants in dedicated infrastructure constants files such as `<feature>.constants.ts`.

Map API response to domain types before data reaches presentation.

Do not pass raw transport status objects to presentation. On success, return only the domain payload plus a user-facing `message`. On failure, throw a sanitized repository error shape such as `message`, `type`, and optional `details`.

Keep hooks for feature behavior in `presentation`, not in `domain` or `infrastructure`.

Prefer hooks over legacy controllers when the feature is in the migrated path. Do not keep domain controllers that only proxy use case calls into React Query hooks.

Keep page or route entry points routing-only. Do not let pages accumulate business logic or feature orchestration.

## Repository Rules

Repository method params must match the actual use case of that method. Do not accept a broad domain entity when the method only needs `id`, filters, or a transport-shaped payload subset.

Repository method return types must match the contract exposed upward. Return the specific result model or domain model required by the use case instead of a vague or over-broad type.

Do not keep a datasource layer by default. If a repository implementation can call an HTTP client directly without losing clarity or testability, prefer the simpler direct call and add a datasource only when there is a concrete need such as storage orchestration, shared transport behavior, or multiple external sources.

Repository implementations must validate response status codes before returning data upward. When an API contract documents internal success codes, repository implementations must gate on that code before returning normalized domain data.

When the same documented response codes are reused across repository methods, extract them into named constants instead of repeating magic strings.

Do not rely on raw transport error fields in hooks or pages. Presentation must only consume the sanitized repository error contract.

If a repository is only used inside one feature flow, prefer direct instantiation over injecting HTTP clients through constructor arguments unless there is a concrete testing or composition need.

## Dependency Direction Rules

Prefer this direction:

- `infrastructure/presentation` -> `interface/adapters` -> `application` -> `domain`

Allow direct calls into inner layers only when they preserve this direction.

Reject these patterns:

- `domain` importing framework or transport code
- `application` importing controllers, SDK clients, or ORM models
- Adapters reaching across layers to execute business decisions on their own
- Shared utility packages becoming a hidden backdoor for outer-layer concerns

If a dependency is necessary but points outward, introduce a port, interface, or mapper at the inner boundary and implement it outside.

Do not create direct dependencies between features; communicate through abstractions or shared contracts only.

## Review Heuristics

When reviewing a file or module, ask:

- Is this code making a business decision or only translating data?
- Would this code still make sense if the framework changed?
- Does this dependency point inward or outward?
- Is this module named after technical shape instead of responsibility?
- Is one component mixing orchestration, transport, and persistence at once?

Use responsibility over location. A file in `domain/` is not domain code if it imports controllers or SQL models.

## Common Violations

Watch for these failures:

- Domain models annotated with persistence or transport concerns
- Use cases returning framework-specific response types
- Controllers performing business branching instead of delegating to use cases
- Repositories exposing ORM entities directly to the application or domain layer
- UI or API handlers reaching into database code without an application boundary
- "Shared" helpers importing everything and creating implicit coupling
- Validation split across controller, adapter, and domain with no clear ownership
- Presentation branching on raw transport error fields instead of sanitized error types
- A use case that only forwards repository calls with no real orchestration
- A controller that only proxies use case calls into a hook with no extra business logic

## Minimal Refactor Strategy

When the architecture is messy:

1. Keep the current behavior stable.
2. Extract one clear use-case boundary around a real workflow.
3. Move business rules inward into `domain` or `application`.
4. Add adapters to translate outer models into inner models.
5. Hide concrete DB, HTTP, UI, or SDK calls behind outward implementations of inward-facing interfaces.
6. Rename and relocate only after the responsibilities are actually separated.

Prefer transitional seams over broad restructuring:

- Add a mapper before replacing an entire module.
- Introduce a repository interface before changing storage.
- Wrap framework objects at the edge before rewriting business logic.

## Decision Guide

- If logic changes data shape, validation, or orchestration, move it down to `domain` or `infrastructure`.
- If API fields can be missing, normalize them at the mapper or use case boundary instead of spreading `?` through domain models and presentation.
- If a screen only needs a lightweight subset of data, prefer an application DTO instead of weakening the domain entity to match the transport payload.
- If a repository contract returns both lightweight and rich data, use DTOs for simple use cases such as list or delete acknowledgements and return domain entities for flows that need business behavior.
- If a repository method only needs request filters, ids, or a command payload, define a dedicated param type for that method instead of reusing the full domain entity.
- If a repository method returns `message`, `meta`, or other response-level data, expose a result type that models that full contract instead of returning only the inner entity.
- If a method return type would otherwise expose inline transport-neutral shapes, promote them into named application types.
- If a domain field has a stable business vocabulary such as status or type, prefer an enum plus mapper normalization over passing raw strings through the domain.
- If the API docs define a success code, only map and return data after that exact code matches; otherwise throw with the API message and let infrastructure error handling map failure codes.
- If presentation needs to branch on an error, expose a repository-level error `type` instead of leaking API status codes into hooks or pages.
- If a repository method succeeds, expose only `message` and normalized data. Do not expose raw transport status codes or messages as nested transport objects in result contracts.
- If repository code starts filling up with transport mapping or code tables, extract those concerns to dedicated mapper and constants files so the repository implementation stays orchestration-focused.
- If logic only coordinates UI state or side effects for a feature, keep it in `presentation`.
- If a controller and a presentation hook both fit, prefer the hook for new or migrated feature work.
- If a hook needs feature data access, wire the repository and use case there instead of adding a domain-layer controller wrapper with no extra business logic.
- If the remaining use case work is just request/response mapping, let the repository return normalized domain data directly.
- If a page is doing more than routing, move the work into the feature layer.

## Naming and Ownership

Name modules by responsibility, not mechanism.

Prefer:

- `ApproveInvoice`
- `OrderRepository`
- `ProfilePresenter`
- `SyncAccountUseCase`

Avoid:

- `Utils`
- `Helpers`
- `Manager`
- `CommonService`

Let each layer own what naturally belongs there:

- `domain` owns business meaning and invariants
- `application` owns workflow coordination
- `interface/adapters` own translation at boundaries
- `infrastructure/presentation` own framework and runtime details

## Testing Guidance

Use the architecture to decide test focus:

- Test `domain` with fast rule-focused unit tests
- Test `application` with use-case tests over ports/fakes
- Test adapters with mapping and translation tests
- Test infrastructure with integration tests against real frameworks or systems when needed

Do not force heavy end-to-end tests to validate logic that belongs in inner layers.

## Anti-Patterns

Reject these moves:

- Putting every class behind an interface without a boundary need
- Creating layers as folders only, while dependencies still point the wrong way
- Calling a module `clean` while business rules still live in controllers or UI
- Building a "shared" package that mixes domain rules, transport helpers, and persistence code
- Splitting a simple feature into too many layers when there is no meaningful boundary

If a proposed architecture creates more ceremony than clarity, simplify it.

## Review Checklist

Before finishing, verify:

- Business rules are isolated from framework details
- Dependencies point inward
- Ports and interfaces exist only where they protect a real boundary
- Application workflows are visible and not buried in controllers or infrastructure
- Adapters translate instead of owning policy
- Repository implementations gate on documented success codes before returning data
- Presentation consumes only sanitized error contracts, not raw transport fields
- The suggested refactor is smaller and safer than a rewrite

## References

- For concrete examples of layer boundaries, read `references/layer-examples.md`.

## Trigger Examples

Use this skill for requests like:

- "Review service ini, ada pelanggaran clean architecture nggak?"
- "Cari dependency yang salah arah antara domain dan infra."
- "Bantu rapihin boundary use case, adapter, dan repository."
- "Audit codebase ini, business rule bocor ke controller atau framework nggak?"
- "Pisahin orchestration, adapter, dan persistence tanpa rewrite besar."
