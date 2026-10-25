# Glossary Document Format

Use this structure for every glossary deliverable. Match the supplied Sprint 1 glossary example: one scope-specific title, a short scope note, and a two-column `Istilah` / `Definisi` table. Do not add metadata, extra sections, or a separate unresolved-terms table.

## Required structure

```markdown
# Glosarium [Sprint, Project, or Feature Scope]

Glosarium ini menjelaskan istilah yang digunakan dalam [documents in scope]. Istilah khusus endpoint atau fitur dijelaskan lebih rinci pada dokumen solutioning terkait.

| Istilah                                | Definisi                                        |
| -------------------------------------- | ----------------------------------------------- |
| [Term or abbreviation (expanded form)] | [Concise definition in this project's context.] |
```

## Content rules

- Build the glossary from the BRD, HLD, ERD, solutioning/IFA, and other named documents in scope. Include shared business, system, security, data, API, and testing terminology that readers need to interpret those documents.
- Define terms in the current project's context; a generic dictionary definition is insufficient when the project gives the term a specific meaning.
- Expand abbreviations in the term cell on first use, for example `API (Application Programming Interface)`. Preserve exact spelling and casing for code identifiers, JSON fields, statuses, role names, and enum values by formatting them as inline code.
- Keep each definition concise, precise, and consistent with its source. Reuse one agreed definition for a term across the glossary; distinguish meanings only when the source establishes different contexts.
- Leave endpoint- or feature-specific detail in the relevant solutioning document. Include a short pointer in the definition only when needed to explain where the detailed meaning lives.
- Do not invent meanings. If a term is present but not defined by available sources, use `TBD (To Be Determined)` or a concise definition ending with `TBD` and the needed confirmation.
- Exclude terms that do not appear in or materially help interpret the documents in scope. Do not copy terms from the example glossary into an unrelated project.
- Preserve the source document's terminology and meaning. Where sources conflict, apply the Solutioning Analyst source-precedence rule and record the unresolved term as `TBD` rather than silently choosing a definition.
- Preserve an existing glossary's established term order. For a new glossary, group related terms by document or functional area and keep related names together, as in the supplied example.

## Validation

- Confirm the title and opening note accurately identify the document scope.
- Confirm each row has one term and one definition, abbreviations are expanded, and code literals retain their exact spelling.
- Cross-check definitions against source documents and remove duplicates, unsupported claims, and feature-level detail that belongs in the related solutioning document.
