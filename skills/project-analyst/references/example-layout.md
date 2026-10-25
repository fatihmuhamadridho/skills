# Example Story Sizing Layout

Use this layout when the target story-sizing sheet uses the `Example` template. Match the template's visual structure while actively populating only the feature statement and acceptance-criteria table.

## Sheet structure

- Row 1 is blank.
- Row 2 contains the feature statement in natural language.
- Row 3 is blank.
- Row 4 contains the main section headers.
- Row 5 contains the acceptance-criteria subheaders.
- Rows 6 onward contain numbered acceptance-criteria slices.
- Put positive-flow rows first, followed by negative, validation, and failure flows.
- Preserve the shape, merges, borders, and visual grouping of the `Example` tab. Keep non-AC sections empty.

## Acceptance-criteria columns

- `No`: sequential numbering starting from `1`.
- `Given`: optional precondition.
- `When`: user or system action.
- `Then`: observable outcome.

`Acceptance Criteria:` is a merged group header above `Given`, `When`, and `Then`; it is not a content column. Do not add duplicate `When` or `Then` columns or other AC subheaders.

## Flow depth

Do not compress an end-to-end flow into one row when users observe multiple steps or states. Add rows for successful progression, validation failures, blocked actions, and failed submission or processing when implied by the source. Use the criterion wording to express the flow type; do not add a separate label column.

## Keep other sections blank

Preserve but do not populate technical or estimation sections such as `New Technology`, `Surrounding`, `Technical Flow`, `UI Slicing`, `Task`, `Mandays`, `Total`, or `Size`. Keep spacer rows where needed. Ensure the final AC row has the same borders as the rest of the table.
