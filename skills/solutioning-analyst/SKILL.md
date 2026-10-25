---
name: solutioning-analyst
description: Analyze business and backend requirements and produce BRDs, HLDs, ERDs, glossaries, and IFA/API contracts in their configured formats, from requirements, Figma, screenshots, existing documents, or feature descriptions.
---

# Solutioning Analyst

Turn business or UI evidence into objective, definitive, implementation-ready
technical solutioning. Apply System Analyst Playbook v1.6. Use configured IFA format
for API-focused output, configured ERD format for data-model output, and configured
HLD format for high-level architecture output. Use the configured glossary format for
cross-document terminology and configured BRD format for business-requirements output.

## Required references

Read before producing deliverables:

1. [Playbook compliance](references/playbook-compliance.md) for Playbook requirements.
2. [IFA template v4](references/ifa-template-v4.md) for every API contract or IFA.
3. [ERD document format](references/erd-document-format.md) for every ERD deliverable.
4. [HLD document format](references/hld-document-format.md) for every HLD deliverable.
5. [Glossary document format](references/glossary-document-format.md) for every glossary deliverable.
6. [BRD document format](references/brd-document-format.md) for every BRD deliverable.
7. [Feature inference rules](references/feature-inference-rules.md) only when scope is unclear.

Use [API contract shape](references/api-contract-shape.md) only as supplementary
guidance. IFA template wins on conflict.

## Sources and baseline

Accept requirements, BRS/FRS/SRS, Figma, screenshots, diagrams, existing LLD/IFA,
meeting conclusions, and user constraints. A BRD may also be the requested output when
the available source material defines business context and scope.

Before writing:

1. Identify the feature or API purpose, service/domain, actors or consumers, and scope.
2. Find the latest relevant HLD, solutioning, interface, and architecture-diagram baseline.
3. Carry forward still-valid content.
4. Classify each impact as New, Enhancement, or Existing.
5. Separate sourced facts from assumptions.
6. For ERD requests, identify the in-scope entities or collections, attributes,
   constraints, indexes, relationships, lifecycle rules, and source SLA.
7. For HLD requests, identify the application stack, deployment topology, integrations,
   important user/system flows, SLA sources, and related IFA/ERD documents.
8. For glossary requests, identify the documents in scope and terms used consistently
   across them, distinguishing shared terminology from feature-specific definitions.
9. For BRD requests, identify business goals, stakeholders, scope and exclusions,
   prerequisites, delivery sequence, business rules, dependencies, and any sourced or
   proposed SLA targets. Preserve unknowns as `TBD`.
10. Resolve conflict by user instruction, newest approved source, then newest baseline.

Never expose production secrets, credentials, tokens, keys, private endpoints, or
real customer PII. Use placeholders and synthetic examples.

## Output decision

- Glossary request: produce a concise, scoped glossary in the exact structure from
  [Glossary document format](references/glossary-document-format.md).
- BRD request: produce a business-level requirements document with the exact section
  order and layout from [BRD document format](references/brd-document-format.md).
  Keep technical contracts in their dedicated solutioning deliverables.
- HLD or high-level architecture request: produce the Markdown document and linked
  architecture SVG in the exact structure from
  [HLD document format](references/hld-document-format.md).
- ERD or data-model request: produce an ERD in the exact structure from
  [ERD document format](references/erd-document-format.md). Use the database engine
  and scope established by source material; do not assume MongoDB when unspecified.
- API/interface request: produce IFA in exact order and shape from IFA template v4.
- Broader solution request: produce LLD coverage from Playbook compliance; keep
  detailed request/response/status specifications in an IFA or link.
- Major legacy-to-modern transformation: include migration strategy.
- Formal Word/PDF: include required administrative sections.
- Informal Markdown: approvals, revisions, and contents may be omitted.

Default language is Bahasa Indonesia. Keep identifiers, JSON fields, paths,
protocols, diagram syntax, and status codes unchanged. Request Validation must use
Bahasa Indonesia unless user explicitly requests another language.

## Workflow

1. Grasp context, current state, target state, constraints, and evidence.
2. Analyze gaps, impacted systems, interfaces, data, security, lifecycle, and testing.
3. Design alternatives when meaningful tradeoffs exist.
4. Conclude design using evidence.
5. Document final state declaratively and concisely.
6. Verify consistency across diagrams, tables, examples, and codes.

Cover rare cases when security, money, data integrity, or irreversible state is at risk.

## Diagram rules

- Sequence diagrams show actors, internal/external participants, calls, returns,
  conditions, loops, async interactions, and useful request/response notes.
- Distinguish new and enhanced flow where relevant.
- Keep notes short; split large flows into named child diagrams.
- State diagrams are mandatory for transactional-module enhancements.
- State names use completed or stative forms: SUBMITTED, APPROVED, REJECTED.
- Do not use imperative state names such as SUBMIT, APPROVE, or REJECT.
- Prefer readable Mermaid flowcharts when clearer than stateDiagram.

## API rules

- Internal IFA may cover all interfaces in one service.
- External-party IFA covers one purpose/use case for one consumer or partner.
- Preserve this exact top-level order from IFA template v4:
  Design, Sequence Diagram, Performance / SLA Targets, General Information,
  Business Rules, Request Headers, Request Parameters, Request Body, Response Body,
  HTTP Status Codes, and Service Status Codes.
- Each API includes general information, endpoint composition, applicable headers,
  parameters/body, validation, response, HTTP statuses, service codes, versioning,
  exposure classification, and testing scope.
- Every IFA includes Performance / SLA Targets directly after Sequence Diagram.
  Use columns Success Rate, VUser, CPU / Memory, and Response Time.
- Derive SLA values from approved requirements, expected traffic, existing correlated
  API usage, or NFT results. Never present invented values as agreed targets. When
  evidence is unavailable, use explicit TBD values and list required confirmation.
- Apply Playbook baselines only as a stated proposal: 99% non-critical, 99.9%
  critical, and 99.99% highly critical. Response-time baseline depends on service
  type; VUser and CPU/memory targets require workload evidence.
- Request fields use M, O, or C. Preserve hierarchy. Define type, maximum length,
  description, and synthetic example.
- Every success and error uses this envelope:

    {
      "status": {
        "code": "ORDS00000",
        "message": "Order created successfully"
      },
      "data": {}
    }

- Response table uses status, status.code, status.message, data, and dotted nested
  fields such as data.orderId.
- Default HTTP mapping: 200 positive, 400 business/validation, 500 technical.
- Service code format: Service Prefix + Status Type + Category + Specific Error Code.
- Prefix normally has three uppercase letters. Status type is S, B, or T.
- Category has two digits; 00 is default. Common categories:

| Category | Business | Technical |
| --- | --- | --- |
| 00 | General/default | Generic exception |
| 01 | Authentication | Data unmarshalling |
| 02 | User validation | External service |
| 03 | Request payment | Database |
| 04 | Request fulfilment | Object storage |

- Specific error code has three digits. Use 000 when no granular distinction is
  needed; otherwise allocate consistently within service/domain.
- Sequence outcomes, examples, HTTP statuses, and service-code tables must agree.

## Versioning, exposure, and testing

- Enhance existing API: optional change without consumer impact; preserve path/version.
- New major version: breaking request/response change; keep older version supported.
- New endpoint: materially different business flow; start at v1.

Classify API as Public, Enterprise Intranet, or Internal Only.

- NFT: customer-facing, high-load/spike-prone, new API, or legacy enhancement
  affecting core logic.
- VA/Pentest: new Public or Enterprise Intranet API, or security-impacting enhancement.
- Internal-only API is excluded from Pentest.
- NFT handoff includes service/module, endpoint, sample parameter, sample response,
  target CPU/memory, and target TPS.
- VA/Pentest handoff includes service/module, endpoint, sample parameter, and response.

## Delivery

For feature solutioning, default to one Markdown file per feature:
docs/solutioning/<feature-slug>/<yyyy-mm-dd>-<feature-slug>.md

For HLD requests, also produce the paired architecture SVG required by
`references/hld-document-format.md`. Follow the existing repository location and
update existing HLDs/diagrams when requested; do not create duplicates.

For glossary requests, use the existing project- or sprint-level glossary location
and naming convention. Update the current glossary when requested; do not edit its
template/example files or create a duplicate.

For BRD requests, use the existing project- or sprint-level BRD location and naming
convention. Update the current BRD when requested; do not edit its template/example
files or create a duplicate.

## Validation

Confirm IFA template compliance, baseline use, facts versus assumptions, all relevant
Playbook points, Indonesian request validation, nested status envelope in every
response, no secrets/PII, and cross-section consistency. Remove placeholders unless
source information is unavailable and the placeholder is explicit.

Also confirm Performance / SLA Targets exists immediately after Sequence Diagram and
contains all four required columns with sourced values or explicit TBD markers.

For ERD deliverables, confirm exact section order and table shapes in
`references/erd-document-format.md`, Mermaid/data dictionary consistency, documented
indexes and relationships, sourced data rules and SLA, and linked references. Mark unknowns
`TBD`; do not invent schema, constraints, index behavior, retention, or SLA.

For HLD deliverables, confirm exact section order from
`references/hld-document-format.md`, that the architecture SVG exists and is linked
with a relative path, and that the technology table, SVG, Mermaid flowchart,
explanations, sourced SLA, and linked IFA/ERD references agree.

For glossary deliverables, confirm the scope introduction and two-column term table
follow `references/glossary-document-format.md`, definitions match the source
documents, abbreviations are expanded, identifiers retain exact spelling, and
feature-specific terms are left to their detailed solutioning documents.

For BRD deliverables, confirm the six numbered sections and section 3 subsections
follow `references/brd-document-format.md`, requirements use sequential traceable IDs,
business facts are distinguished from proposals and unknowns, and SLA targets are
sourced or explicitly marked as proposed/TBD.
