# IFA Template v4

Use this structure for API-focused solutioning. Preserve section order. Repeat API
specification blocks when one IFA contains multiple related internal APIs.

# [Service / API Purpose] - Interface Agreement

Metadata:

- Document Type: IFA / API Contract
- Status
- Service / Domain
- Consumer / Partner

## Design

Embed or link relevant UI/HLD image. Omit only when unavailable or irrelevant.

## Sequence Diagram

Use Mermaid. Show actor, consumer, API provider, database/cache/queue, validation,
persistence, downstream calls, success, business errors, and technical errors.
Response labels use the same service codes as Service Status Codes.

## Performance / SLA Targets

This section is required and appears immediately after Sequence Diagram.

| Success Rate | VUser | CPU / Memory | Response Time |
| --- | --- | --- | --- |
| Sourced target or TBD | Concurrent-user target or TBD | CPU and memory targets or TBD | Sourced target or TBD |

Rules:

- Use approved requirements, projected traffic, existing correlated API usage, or NFT
  results as evidence.
- Never convert an assumption into an agreed target.
- If evidence is unavailable, write TBD and identify what must be confirmed.
- Playbook success-rate baselines may be proposed explicitly: 99% for non-critical,
  99.9% for critical, and 99.99% for highly critical services.
- VUser means expected maximum concurrent users.
- CPU and memory targets describe tolerable projected production utilization.
- Select response-time proposal by service type:

| Service type | Ideal | Acceptable | Max tolerable |
| --- | ---: | ---: | ---: |
| Web/GUI, less critical | 200 ms | 1 s | 2 s |
| E-commerce/transactional | 100 ms | 500 ms | 1-2 s |
| Financial | 50 ms | 200 ms | 500 ms |
| Real-time/streaming | 50 ms | 100 ms | 200 ms |

## General Information

| Component | Value |
| --- | --- |
| Description | API purpose |
| Resource URL | Full URL or safe environment placeholder |
| Transport Protocol | HTTPS, REST, SOAP, queue, and so on |
| Request Verb | GET, POST, PUT, PATCH, DELETE |
| Content Type | application/json or applicable type |
| Interaction Type | Synchronous, Asynchronous, Batch, RPC, Queue |
| Security Policy | Access token, API/signature key, or applicable control |

### Path / Endpoint

| Section | Value |
| --- | --- |
| Host / Domain | Safe host placeholder |
| Service Name | Singular service name |
| Version | Major version prefixed with v |
| Resource | Resource/function path |
| Final Endpoint | Final composed path |

### API Versioning

- Decision: Enhance Existing API, New Major Version, or New Endpoint
- Previous Endpoint
- Final Endpoint
- Backward Compatibility
- Reason

## Business Rules

Numbered rules in Bahasa Indonesia. Cover permissions, ownership, uniqueness,
valid state, dependencies, idempotency, side effects, and relevant edge cases.

## Request Headers

| Header | Type | M/O/C | Max Length | Description | Example |
| --- | --- | --- | ---: | --- | --- |

Omit only when no request header needs documentation.

## Request Parameters

| Parameter | Type | M/O/C | Max Length | Description | Example |
| --- | --- | --- | ---: | --- | --- |

Omit when no path/query parameter exists.

## Request Body

| Attribute | Type | M/O/C | Max Length | Description | Example |
| --- | --- | --- | ---: | --- | --- |

Use dotted paths for nested attributes. Preserve parent hierarchy.

### Example Request Body

Provide valid synthetic JSON. Never use production secrets or real PII.

### Request Validation

Write numbered validation rules in Bahasa Indonesia. Preserve field names and
technical values. Include mandatory, format, range, ownership, uniqueness,
conditional, idempotency, and cross-field rules as applicable.

## Response Body

The contract contains status and data. Status is an object containing code and message.

| Attribute | Type | M/O/C | Max Length | Description | Example |
| --- | --- | --- | ---: | --- | --- |
| status | Object | M | - | Response status object | {} |
| status.code | String | M | 20 | Service status code | ORDS00000 |
| status.message | String | O | 200 | Human-readable response message | Order created successfully |
| data | Object/null | M | - | Service-specific payload; null for errors | {} |

Add all data descendants using dotted paths.

### Success Example

    {
      "status": {
        "code": "ORDS00000",
        "message": "Order created successfully"
      },
      "data": {}
    }

### Business Error Example

    {
      "status": {
        "code": "ORDB02001",
        "message": "Business validation failed"
      },
      "data": null
    }

### Technical Error Example

    {
      "status": {
        "code": "ORDT03001",
        "message": "Database is temporarily unavailable"
      },
      "data": null
    }

Every success and error example must use the same nested status structure.

## HTTP Status Codes

| HTTP Status | Usage |
| --- | --- |
| 200 | Positive response or process may continue |
| 400 | Business validation or incompatibility failure |
| 500 | Technical failure in service or dependency |

Add another HTTP status only when required by approved contract/source.

## Service Status Codes

Format:

    [Service Prefix][Status Type][Category][Specific Error Code]

| Part | Rule | Example |
| --- | --- | --- |
| Service Prefix | Normally 3 uppercase service/domain letters | ORD |
| Status Type | S success, B business, T technical | S, B, T |
| Category | 2 digits, zero-padded; 00 default | 00, 02, 03 |
| Specific Error Code | 3 digits, zero-padded; 000 default | 000, 001 |

Common category reference:

| Category | Business category | Technical category |
| --- | --- | --- |
| 00 | General / default | Generic exception |
| 01 | Authentication | Data unmarshalling |
| 02 | User validation | External service |
| 03 | Request payment | Database |
| 04 | Request fulfilment | Object storage |

Use only categories required by the API.

| Code | Type | Category | Meaning |
| --- | --- | --- | --- |

List every code used by sequence diagram and response examples.

## API Exposure Classification

| Classification | Rationale |
| --- | --- |
| Public / Enterprise Intranet / Internal Only | Explain actual consumer and boundary |

## Testing & Documentation Scope

| Scope | Required | Rationale / Handoff |
| --- | --- | --- |
| NFT | Yes/No | Explain traffic/customer-facing/core-flow criteria and required test data |
| VA/Pentest | Yes/No | Explain exposure/security-change criteria and required test data |

Reference Performance / SLA Targets here when NFT applies. NFT handoff also includes
target TPS when evidence exists.

## Optional sections

Add only when relevant:

- Assumptions
- References
- Security Measures
- Data Design
- Migration Strategy
- Contributors, Approvals, Revision History, Table of Contents for formal output
