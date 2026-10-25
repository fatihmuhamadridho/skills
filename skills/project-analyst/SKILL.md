---
name: project-analyst
description: Analyze product and business requirements, map user and system flows, write business-facing acceptance criteria, and break scope into actionable backlog items or story-sizing sheets.
metadata:
  short-description: Analyze requirements and produce acceptance criteria or story sizing
---

# Project Analyst

Turn business goals and source artifacts into clear, traceable, implementation-ready project analysis. Use for product discovery, requirement analysis, process mapping, acceptance criteria, backlog breakdown, and story sizing.

## Choose the deliverable

- **Project analysis:** clarify objective, users, outcomes, scope, process, business rules, dependencies, risks, decisions, and open questions.
- **Acceptance-criteria document:** produce a Markdown document in the exact structure and presentation defined by [references/acceptance-criteria-document.md](references/acceptance-criteria-document.md). Do not omit its sections; use `TBD` or `N/A` when source material does not support a value.
- **Story sizing sheet:** when the user explicitly requests a Google Sheet, write detailed positive and negative acceptance-criteria slices into the requested workbook and follow its specified template. Read [references/example-layout.md](references/example-layout.md) when using the Example layout.

Honor an explicitly requested output format or target artifact. For Google Sheets work, use the available Google Sheets skill and connector workflow before reading or editing a live spreadsheet. Never write to an unrelated spreadsheet or tab.

For session-based project-analysis tracking when project instructions require it, use `<codex-home>/artifacts/templates/project-analysis-template.csv`: make a dated working copy and record one row per analyzed feature or item. Resolve `<codex-home>` from `CODEX_HOME`, falling back to `.codex` in the current user's home directory.

## Analysis principles

- Start from the business problem and desired outcomes; do not assume a proposed solution is the underlying need.
- Review the named requirements, BRD, solutioning, feature documents, and existing terminology before drafting.
- Keep confirmed facts, decisions, assumptions, recommendations, dependencies, risks, and open questions distinct.
- Trace requirements to business outcomes and affected actors. Identify conflicts and gaps instead of silently resolving material decisions.
- Do not invent policy, thresholds, permissions, or system behavior. Make the smallest safe assumption explicit when a useful draft can proceed without a decision.
- Keep analysis proportionate. Do not turn business analysis into technical architecture or API contract design unless requested.

## Acceptance criteria

- For every Markdown acceptance-criteria document, follow [references/acceptance-criteria-document.md](references/acceptance-criteria-document.md) exactly: title and metadata, Story, Acceptance Criteria, Technical Flow, Parameters, Subtask with Frontend and Backend, Summary, Sizing Reference, and Notes.
- Use the source artifacts and current feature scope as authority; do not expand scope based on adjacent features.
- Center each criterion on an actor, action or event, and observable outcome. Keep implementation details out unless the user requests technical criteria or the source requires them.
- Cover the happy path and relevant alternate, validation, blocked, failure, permission, and boundary flows implied by the requirements.
- Write separate `AC-###` criteria with `Given`, `When`, and numbered `Then` outcomes. For explicit table or spreadsheet output, follow the target's columns and layout instead.
- Make each row independently understandable and testable. Split rows when actors, preconditions, or observable outcomes differ; combine near-identical cases when the business outcome is the same.
- Re-check every criterion against its source and remove invented behavior.

## Backlog breakdown

- Group work by user or business outcome, then split into independently understandable epics, features, or stories as requested.
- Include dependencies and sequencing where they affect delivery.
- Make stories small enough to estimate and verify; retain links to the outcomes and criteria they serve.
- Surface unresolved decisions that could change scope or estimates.

## Story sizing in Google Sheets

- Use the user-provided spreadsheet and tab when specified. If the user asks for a sheet but does not identify one, locate a relevant existing template or spreadsheet through the connected Google Sheets workflow; ask only when multiple plausible targets remain.
- Inspect the target workbook and tab before editing. Preserve its layout and write only to the requested acceptance-criteria area.
- If using the `Example` template, follow [references/example-layout.md](references/example-layout.md): preserve its visual structure, populate only the feature statement and numbered `No`, `Given`, `When`, `Then` rows, put positive flow before negative or validation flow, and leave estimation and technical sections blank.
- Use the target sheet's acceptance-criteria structure. In the `Example` layout, `Acceptance Criteria:` is a merged group header above `Given`, `When`, and `Then`, not a separate content column.
- Break broad journeys into separate rows whenever the user observes distinct steps, states, validations, or outcomes. Do not compress the whole feature into one row.
- Re-read the written range and verify tab, feature statement, numbering, headers, flow coverage, and empty non-AC sections before reporting completion.
- Do not overwrite unrelated tabs or replace the whole workbook when a targeted tab edit is sufficient.

## Default analysis outline

Adapt to the request; omit sections that add no value.

- Objective and expected outcomes
- Stakeholders and users
- Scope in and out
- Current and target process or user flow
- Business rules and requirements
- Acceptance criteria
- Assumptions, dependencies, constraints, and risks
- Decisions and open questions
- Suggested backlog breakdown and priorities

Lead with the requested deliverable. Keep language plain, precise, and specific enough for stakeholders and delivery teams to review.
