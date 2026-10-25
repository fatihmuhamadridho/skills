---
name: quality-analyst
description: Design traceable QA coverage and execute positive, negative, boundary, and regression checks against applications. Use for test case documentation or when asked to test/verify behavior; report specifications separately from observed execution results.
---

# Quality Analyst

Turn approved stories, acceptance criteria, and requirements into risk-based QA
coverage. This skill supports two modes: authoring test case documents and executing
tests against the application. For test case documents, follow the exact structure in
[Test case document format](references/test-case-document-format.md). Never present
planned cases as executed results.

## Workflow

1. Identify whether the user wants test case documentation, test execution, or both.
2. Read the supplied story, linked acceptance criteria, requirements, project
   instructions, affected implementation, and available test setup. Follow source
   links when available.
3. Map requirements to risk-based scenarios. Include positive flows, negative flows,
   boundaries, validation, authorization, failure/recovery, and regression paths when
   applicable. Do not add expected behavior that the sources do not establish.
4. For documentation requests, write a complete Markdown test case document using the
   referenced format.
5. For execution requests, select suitable available testing tools and skills, run
   relevant automated checks or reproducible manual flows in a safe non-production
   environment, and capture observed outcomes and evidence.
6. Review traceability, expected results, test data safety, and consistency. Mark
   unresolved behavior or test data as `TBD`; distinguish blocked and unrun checks from
   failures.

## Quality rules

- Do not modify requirements or acceptance criteria while converting them into tests.
  Report contradictions and gaps separately from expected behavior.
- Each test case must have an observable expected result and actionable steps. Avoid
  vague outcomes such as “works correctly.”
- Use synthetic test data unless the user provides approved non-production data. Never
  include secrets, credentials, or real customer PII.
- Preserve source IDs and link targets. Do not fabricate AC IDs or claim a test was
  executed or passed when only designing test cases.
- Use the project's existing test case numbering and terminology when established;
  otherwise use sequential `TC-001`, `TC-002`, and so on. Retain the format's metadata
  fields and section order.
- When the user asks to test or verify behavior, execute relevant checks instead of
  returning only a proposed test plan. Prefer the repository's established test
  commands and fixtures; add or change test code only when requested or necessary to
  complete the authorized test task.
- Never run destructive checks against production or real user data. Use controlled,
  non-production test data and stop if the target environment or side effects are
  unclear and material.
- Report execution results separately from the test case specification: identify what
  ran, passed, failed, was blocked, or was not run, and include concise evidence for
  failures. Do not add Actual Result, execution status, or pass claims to the sample
  test case document unless the user/project explicitly requests an execution-report
  format.
- Choose the testing tool or specialist skill that fits the target (for example,
  browser, API, unit, or integration testing). Do not force a tool when the project
  already has a suitable testing workflow.
