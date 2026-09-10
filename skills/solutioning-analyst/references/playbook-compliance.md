# System Analyst Playbook v1.6 Compliance

Use this checklist to determine required and optional content. Apply only sections
relevant to requested deliverable, but evaluate every item before omission.

## Operating principles

- Consistent: reuse approved formats, legends, symbols, colors, naming, and prior
  documents as baseline.
- Objective: state facts supported by source evidence. Cite external references.
  Exclude unresolved opinion.
- Definitive: explain terms and abbreviations, use assertive present-tense wording,
  describe end state, and avoid ambiguity.
- Effective: group related content, remove repetition, and write concise prose.

System Analyst bridges business and implementation through documentation, analysis,
design, facilitation, requirement refinement, validation/testing support, and
continuous improvement.

## Way of working

Follow Grasp, Analyze, Design, Propose, Document.

- Requirement gathering: understand goals, existing capabilities, constraints, and
  decisions; ask probing questions and recap conclusions. Do not design prematurely.
- Backlog creation: help define scope, high-level system flow, technical acceptance
  criteria, dependencies, and rough effort.
- Solutioning: design options, consult relevant experts, review with technical teams,
  resolve ambiguity, evaluate impacts, and document agreed LLD/IFA.
- Development: answer implementation disputes using approved documents; avoid
  unnecessary solution changes and code micromanagement.
- Testing: advise UAT alignment; prepare NFT and VA/Pentest API scopes.
- Deployment: provide migration strategy for major architecture, database, queue,
  module, or platform transformation.

## LLD coverage

Formal LLD sections:

1. Approvals
2. Version/revision history
3. Table of contents
4. High-level design reference
5. Glossary
6. One solution section per epic/high-level requirement
7. References

Each solution section evaluates:

- Objective Overview: objective, scope, and constraints in 2-5 sentences.
- Integration Landscape: mandatory when adding a surrounding system.
- Impacted Objects: all New, Enhancement, and relevant Existing modules, services,
  databases, queues, caches, storage, APIs, and jobs with concise function.
- Domain Model: tactical DDD class model when useful; may be separate for
  non-business or fast-changing services.
- Sequence Diagram: detailed component interactions.
- State Diagram: mandatory for transactional-module enhancement.
- Application Interfaces: impacted API list only; detailed contract stays in IFA.
- Performance/SLA: success rate, VUsers, CPU/memory, response time, and target TPS
  where applicable.
- Security Measures: IDOR, secure design, ACID/idempotency, broken access, and API
  protection.
- Data Design: relational or NoSQL/storage final state.
- Migration Strategy: required for legacy-to-modern switchover.

Administrative sections are optional for informal Markdown. Formal Word/PDF requires
approvals and revision history. Version documents semantically: draft starts at 0.x;
approved/significant resharing increments major; draft corrections and information
additions increment minor.

## Impacted objects

- New: service/object did not exist.
- Enhancement: existing BAU object changes.
- Existing: unchanged dependency included only for context.

## Sequence diagram completeness

Include:

- external actors
- internal/external participant grouping
- service/module participants
- queue for brokers and database for database/cache
- lifelines and activation where syntax supports them
- synchronous, asynchronous, and return messages
- conditions, alternatives, and loops
- minimal notes for request/response detail
- grouping for New and Enhancement
- named connector to child diagram when flow is too large

## State diagram completeness

Model meaningful system states and lifecycle permutations. State labels are stative
or past participle, such as SUBMITTED, DELIVERED, APPROVED, REJECTED. Never use
command forms such as SUBMIT or APPROVE.

## Performance guidance

Expected success rate:

(all traffic - technical errors) / all traffic

- non-critical service: 99%
- critical service: 99.9%
- highly critical financial, healthcare, or telecom service: 99.99%

VUser target comes from maximum expected concurrent production users.
CPU/memory target considers expected traffic, correlated API usage, and NFT results.

Response-time baseline:

| Service type | Ideal | Acceptable | Max tolerable |
| --- | ---: | ---: | ---: |
| Web/GUI, less critical | 200 ms | 1 s | 2 s |
| E-commerce/transactional | 100 ms | 500 ms | 1-2 s |
| Financial | 50 ms | 200 ms | 500 ms |
| Real-time/streaming | 50 ms | 100 ms | 200 ms |

Do not invent a target when traffic or business criticality is unknown. Mark it as
an assumption or required confirmation.

## Security measures

- IDOR: authenticate and authorize object access; apply throttling against brute force.
- Secure design: minimize PII exposure; apply masking/encryption; cover high-impact
  permutations.
- Transaction integrity: apply ACID where relevant and idempotency for transactional APIs.
- Broken access: protect personal/private data and administrative functions with
  authentication, authorization, and ACL/RBAC.
- API protection: separate public and private traffic when applicable; validate
  secret/signature mechanisms; strictly limit administrative APIs.
- Never place production secrets or actual customer PII in documentation.

## Data design

Relational design includes table names, relations/cardinality, columns, types,
constraints, PK/FK, indexes, and detailed ER relationships.

NoSQL/search/cache design includes schema/document name, key/index, structure or JSON
hierarchy, and synthetic samples.

Describe files, caches, object stores, or other storage structures when affected.

## Migration and references

Include cutover/migration strategy when final delivery switches legacy components to
modern replacements. List source BRS, FRS, SRS, HLD, existing LLD/IFA, decisions,
diagrams, and other external references.

## IFA ownership and scope

- Internal usage: one IFA may represent all interfaces in one service.
- External party: one IFA represents one purpose/use case for one partner/consumer.
- Detailed headers, request, response, HTTP status, and service status codes belong
  in IFA, not LLD.
- Formal IFA includes contributors, approvals, version history, and table of contents.
- Group multiple APIs by parent capability.

## API exposure and testing scope

Exposure:

- Public: internet-accessible, including customer web/mobile backend APIs.
- Enterprise Intranet: shared with surrounding enterprise systems.
- Internal Only: used only by the system or neighboring internal services.

NFT applies to customer-facing APIs, high-load/spike-prone flows, new qualifying
APIs, and legacy enhancements affecting core flow. Pentest applies to new Public or
Enterprise Intranet APIs and security-impacting enhancements. Internal Only APIs are
not listed for Pentest.

NFT list requires service/module, endpoint, sample parameter, sample response, target
CPU/memory, and target TPS. VA/Pentest list requires service/module, endpoint,
sample parameter, and sample response.
