# Test Case Document Format

Use this reference whenever generating manual test case documentation. Match the
heading names, order, metadata style, and table shapes. Preserve the example's English
headings and field labels; write story and test content in the user's requested
language or the source language.

```markdown
# <Feature Name> - Test Cases

> **Document Type:** Test Case
> **Status:** <Draft | Review | Approved | Example>
> **Story Link:** [<Story title>](<relative path or URL>)
> **Acceptance Criteria Link:** [<Acceptance criteria title>](<relative path or URL>)

## Story

> <Story text copied faithfully from the source.>

## Test Cases

### TC-001 - <Scenario title>

> **Related AC:** <AC ID or source requirement ID>
> **Role:** <Actor or role>
> **Behaviour:** <Positive | Negative | Boundary>
> **Priority:** <Priority from the project convention>
> **Type:** <Test type from the project convention>

#### Preconditions

- <State needed before executing the case>

#### Test Data

| Data        | Value / Condition              |
| ----------- | ------------------------------ |
| <Data name> | <Synthetic value or condition> |

#### Steps and Expected Results

| #   | Action       | Expected Result                     |
| --- | ------------ | ----------------------------------- |
| 1   | <One action> | <Observable result for that action> |

#### Cleanup / Postconditions

- <Cleanup or resulting state; use `None.` when no cleanup is needed.>
```

## Authoring rules

- Include the Story and Test Cases sections. Repeat the complete TC block for every
  case, in sequential ID order. Keep the title pattern `TC-### - <Scenario title>`.
- Include both Story Link and Acceptance Criteria Link when those artifacts exist.
  If a source link is unavailable, use `TBD` rather than a fabricated path. For tests
  derived directly from requirements without AC, preserve the requirement ID in
  `Related AC` and identify it accurately in the document context.
- Create enough cases to cover applicable acceptance criteria. Split materially
  different roles, outcomes, and boundary conditions into separate cases.
- Keep `Behaviour` values to `Positive`, `Negative`, or `Boundary`. Use the project's
  defined priority and test type values; if none are defined, use `TBD` rather than
  inventing an enum. Use the sample's `Functional` type when it is clearly applicable.
- Preconditions describe the starting state; Test Data lists values/conditions used;
  each numbered action pairs with a specific observable expected result.
- Keep steps reproducible and concise. Do not combine unrelated actions or assert
  implementation details unless a requirement explicitly requires them.
- Add Cleanup / Postconditions to every test case. State `None.` when no cleanup or
  special resulting state applies.
- Use safe synthetic identifiers and dates in examples. Clearly mark illustrative
  data when it is not supplied by the source.
- Do not add sections absent from the format unless the user or project template
  explicitly requires them. Do not include execution status, actual results, or pass
  claims in a design-only test case document.
