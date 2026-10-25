# Acceptance Criteria Document Format

Use this Markdown structure for every acceptance-criteria document. Preserve the heading order, metadata labels, blockquote style, AC numbering, tables, and notes layout shown here. Replace example content with feature-specific content; never copy the example story, criteria, or estimates into another feature.

## Document structure

1. **Title:** feature or story name as the level-one heading.
2. **Metadata:** blockquoted `Document Type :` set to `Acceptance Criteria` and `Status :` set to `Draft`, `Final`, or `Example` as appropriate.
3. **Story:** one concise, business-facing user story.
4. **Acceptance Criteria:** numbered `AC-001`, `AC-002`, etc. Each criterion has `Given`, `When`, and numbered `Then` outcomes.
5. **Technical Flow:** ordered actor or system steps describing the flow needed to realize the story. Include branches and failure outcomes that affect acceptance criteria; avoid API-contract detail unless requested.
6. **Parameters:** `Weight per Parameter`, `New Technology`, `Surrounding Involvement`, `Technical Flow`, `Acceptance Criteria`, and `Size`, including scores and calculation when the project rubric provides them.
7. **Subtask:** separate `Frontend` and `Backend` tables with `Task`, `Mandays`, `Story Point`, and a total row; use `N/A` when a discipline has no work.
8. **Summary:** `FE`, `BE`, and `Size Task`.
9. **Sizing Reference:** a table with `SIZE`, `SCORE RANGE`, `ESTIMATE COMPLETION`, and `STORY POINT`, populated from the project's agreed size bands.
10. **Notes:** numbered clarifications needed to interpret requirements, estimates, or sizing.

## Story and criteria

Use this story shape:

> As a [user or role], I want [capability] so that [benefit or outcome].

Use this structure for each criterion:

### AC-001

> **Given :** [Relevant starting context or precondition]
> **When :** [User action or event]
> **Then :**
>
> 1. [Observable expected result]
> 2. [Additional observable result, if needed]

Keep criteria business-facing and independently verifiable. Include only scenarios supported or implied by the source, such as successful, alternate, validation, access, blocked, or failure flows. Mark unresolved behavior `TBD` and surface its impact in Notes or the project's question document.

Keep each `AC-###` heading and its Given/When/Then content in the same blockquote layout shown above. Put each observable Then result on its own numbered line. Start numbering at `AC-001` and increment sequentially.

## Technical Flow

Use a numbered list. Start each step with the responsible actor or system, followed by a colon and the action or result, for example `1. Customer portal: Request the signed-in customer's requests.` Include the main success path and relevant branches without turning this section into an API contract.

## Parameters and task tables

Use the scoring rubric from the provided template unless the target project has an agreed replacement. Preserve unknown feature facts instead of guessing:

> **Weight per Parameter :** 15
> **New Technology :** [Experienced in project (1) / Ever implement (2) / Aware but never implement (3) / Never implement and not aware (4)]
> **Surrounding Involvement :** [0 (1) / 1 (2) / 2 (3) / ≧ 3 (4)]
> **Technical Flow :** [1-4 steps (1) / 5-7 steps (2) / 8-10 steps (3) / > 10 steps (4)]
> **Acceptance Criteria :** [1-4 (1) / 5-8 (2) / 9-12 (3) / > 12 (4)]
> **Size :** [Sum of four parameter scores x 15; map score to Sizing Reference]

Assess the four parameter scores from the supplied feature and project context. Mark the Parameters section `Illustrative estimate only. Replace with estimates assessed by the delivery team.` when the scores are your analysis rather than team-approved values. Use `TBD` for a parameter that cannot be assessed from available context, explain the missing input in Notes, and do not calculate a final size from incomplete scores.

Derive distinct frontend and backend deliverables from the story and criteria. Use the `Task | Mandays | Story Point` table under both `### Frontend` and `### Backend`, with a final `Total` row. Provide provisional manday and story-point estimates when the scope supports a reasonable estimate; label them `Illustrative task estimates only.` Use `TBD` when an estimate cannot be grounded, and `N/A` when a discipline has no work.

Use this table shape for each discipline:

| Task          |            Mandays |        Story Point |
| ------------- | -----------------: | -----------------: |
| [Deliverable] |  [Estimate or TBD] |  [Estimate or TBD] |
| **Total**     | **[Total or TBD]** | **[Total or TBD]** |

Under `### Summary`, use blockquotes labeled `FE :`, `BE :`, and `Size Task :`. Report confirmed totals, or `TBD`/`N/A` as applicable.

Use the target team's story-point conversion if available. Otherwise, for illustrative sizing only, use the example's `Story Point = Mandays x 2` assumption and state in Notes that the team must confirm it. Derive `Size Task` from the total frontend and backend story points using the Sizing Reference; mark it `TBD` if its inputs or conversion are incomplete.

For `Sizing Reference`, use this table and its score bands by default. If the target project has a different agreed rubric, use that instead.

| SIZE | SCORE RANGE | ESTIMATE COMPLETION | STORY POINT |
| ---- | ----------- | ------------------- | ----------- |
| S    | ≦ 100       | < 2 days            | 1-3         |
| M    | 101-200     | 2–3 days            | 4-6         |
| L    | 201-250     | > 3–5 days          | 7-10        |
| XL   | > 250       | > 5 days            | > 10        |

Do not present illustrative parameter or task estimates as assessed values or commitments. When parameter scores are illustrative, include `Illustrative estimate only. Replace with estimates assessed by the delivery team.` under `## Parameters`. When task values are illustrative, include `Illustrative task estimates only.` under `## Subtask`. If evidence is insufficient to form a useful estimate, use `TBD` and explain what input is missing.

## Notes

Use a numbered list. Keep unresolved inputs, estimation caveats, and sizing assumptions explicit. Distinguish confirmed team rules from illustrative examples and identify the source or owner when known. Include only notes that help interpret this feature, such as sizing assumptions, the story-point conversion, or the relationship between task estimates and story size.

## Estimation rules

- Use the rubric and score bands above unless the project supplies an agreed replacement; mark agent-assessed parameter scores and task estimates as illustrative.
- Do not reuse illustrative values from an example as assessed values or commitments.
- If a parameter or estimate cannot be grounded in the available scope, retain the section and mark the value `TBD`; explain the missing input in Notes. Do not calculate a parameter size from incomplete inputs.
- Use `N/A` when frontend or backend work does not apply. Do not invent tasks or duplicate work to fill a table.
- Use the score bands shown above unless the target project provides a different agreed sizing reference.
- Treat `Story Point = Mandays x 2` as an illustrative conversion only when no team conversion is available, and flag it for confirmation in Notes.
- Keep the document `Draft` while material requirements or sizing decisions remain unresolved. Use `Final` only when the source or user confirms finalization.

## Adaptation

- Keep the section order and headings above for Markdown acceptance-criteria documents.
- If the user explicitly requests another format or target artifact, honor that format. For Google Sheets story sizing, follow the target sheet and [example-layout.md](example-layout.md) when applicable.
- Do not create or edit a file in a project repository unless the user requested file output or the active task clearly calls for it.
