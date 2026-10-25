# BRD Document Format

Use this format for Business Requirements Documents (BRDs). Match the structure,
heading names, order, metadata style, and tables below. Write in Bahasa Indonesia by
default. Adapt the title, sprint label, and content to the user's project; never carry
JokiHub-specific requirements into another project.

```markdown
# <Nama Proyek> — Dokumen Kebutuhan Bisnis <Sprint/Release>: <Nama Cakupan>

> **Status:** <Draft | Review | Approved | Example>
> **Sprint:** <Nama atau nomor sprint>
> **ID kebutuhan:** <Prefix dan rentang ID>

## 1. Gambaran Proyek

<Ringkasan masalah/konteks bisnis, pengguna atau pemangku kepentingan, serta hasil
yang ingin dicapai. Jaga tetap pada tingkat kebutuhan bisnis.>

## 2. Tujuan

- <Tujuan bisnis yang dapat dipahami dan diverifikasi>

## 3. Cakupan dan Urutan

### Prasyarat

- <Prasyarat yang bersumber atau TBD bila belum diketahui>

### Urutan Pelaksanaan

1. <Urutan atau dependensi pelaksanaan yang terkonfirmasi>

### Di Luar Cakupan <Sprint/Release>

- <Item eksplisit di luar cakupan>

## 4. Kebutuhan Bisnis

| ID     | Kebutuhan                                                          |
| ------ | ------------------------------------------------------------------ |
| BR-001 | <Satu kebutuhan bisnis yang jelas, bernilai, dan dapat ditelusuri> |

## 5. Aturan Bisnis dan Dependensi

- <Aturan bisnis atau dependensi yang bersumber; tandai usulan dan hal yang belum
  dikonfirmasi secara eksplisit.>

## 6. SLA Awal

<Jelaskan cakupan layanan dan apakah target bersumber, usulan, atau asumsi. Target
yang belum disepakati harus ditandai sebagai usulan/TBD, bukan komitmen.>

| Metrik           | Target                                                 |
| ---------------- | ------------------------------------------------------ |
| <Metrik layanan> | <Target bersumber, usulan yang diberi label, atau TBD> |
```

## Authoring rules

- Keep all six numbered sections and the three subsection headings under section 3,
  in the shown order. Do not add generic template sections such as assumptions,
  timeline, or open questions; include relevant confirmed content under the matching
  section above.
- Use `BR-001`, `BR-002`, and sequential IDs unless the source defines another
  requirement-ID convention. Keep each row focused on one business need and retain
  traceability to source material.
- State business outcomes, actors, scope, sequencing, rules, and dependencies. Keep
  endpoint contracts, schemas, implementation choices, and detailed technical flows
  in the appropriate solutioning documents and link them when useful.
- Do not invent project context, approvals, prerequisites, exclusions, dependencies,
  metrics, or SLA targets. Use `TBD` for unresolved facts and clearly label proposals
  or assumptions.
- Include a prerequisite, out-of-scope item, rule, dependency, or SLA metric only when
  relevant or established by source material. If no SLA evidence exists, retain the
  section and state that targets are `TBD` pending confirmation.
- Distinguish approved business requirements from illustrative examples. Estimates,
  capacity figures, and proposed targets are not commitments unless an authoritative
  source confirms them.
- Follow an existing project's BRD location and naming convention. Update an existing
  BRD when requested; do not overwrite templates or examples.
