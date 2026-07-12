# exercise-03: ISO/IEC 42001 + 42005 + 42006 Shape Read

**Estimated effort:** 3 hours

## Objective

Produce a **shape read** of the ISO/IEC 42001 AIMS standard, its 42005 impact-assessment companion, and the 42006 certification-body standard — a durable set of notes and diagrams you can bring to mod-105 (AIMS architecture), mod-107 (assurance architecture), and every later audit-facing engagement.

A shape read is *not* a summary. It is a compressed representation of the standard's *architecture* — the clauses / sections that carry structural weight, the artefacts each requires, the seams where each plugs into the AIMS-of-record, and the reader-audience-per-clause. You should be able to hand the shape read to another architect and answer their questions with only the notes in your hand.

## Prerequisites

- Chapter [`03-iso-42001-family-and-vocabulary-layer.md`](../03-iso-42001-family-and-vocabulary-layer.md) read once.
- Access to ISO/IEC 42001:2023, ISO/IEC 42005, and ISO/IEC 42006. If you do not have direct access, use your organisation's ISO subscription; a *paraphrase* of the standard (blog, vendor-authored summary) is **not acceptable** as a source for this exercise — you must have the primary text in front of you.
- Skim of ISO/IEC 22989:2022 for the vocabulary you will cite in the SoA sketch.

## Deliverables

Produce a directory containing:

1. **`42001-shape-read.md`** — the ISO/IEC 42001 shape read.
2. **`42005-shape-read.md`** — the ISO/IEC 42005 shape read.
3. **`42006-shape-read.md`** — the ISO/IEC 42006 shape read.
4. **`family-seams-diagram.md`** — one diagram (Mermaid, ASCII, or committed PNG) showing how the three plug into each other and into a pre-existing ISO/IEC 27001 ISMS.
5. **`soa-sketch.md`** — a Statement of Applicability sketch for three ISO/IEC 42001 Annex A controls (chapter 03 shows the shape).

## Requirements

### `42001-shape-read.md`

- **Clause-by-clause structural map** for Clauses 4–10. For each clause, one paragraph: what it requires *of the management system*, what the architectural deliverable is, and which role at Northbrook Financial (see exercise-02) would own it.
- **Annex A treatment** — a compressed table of the Annex A control *objectives* (not individual controls), grouped by objective family, with a one-line description each. This table is a shopping list for the SoA.
- **Vocabulary alignment note** — for at least five terms Annex A uses (e.g. *AI system*, *AI risk*, *AI stakeholder*), cite the ISO/IEC 22989 definition and note where 42001 relies on it.
- **Reader-audience-per-clause table** — which stakeholder (executive, CISO, CRO, CAO, head of AI governance, auditor, engineering lead) reads which clause the most closely, and why. Chapter 03's "sibling frameworks as audiences" pattern applies here too.

### `42005-shape-read.md`

- **Process shape** — the impact-assessment lifecycle 42005 defines, in a single diagram: scope → plan → execute → document → review → update. Annotate each stage with the artefact it produces.
- **Fields your schema will need** — a candidate list of impact-assessment schema fields sourced from 42005's requirements, distinguishing *required* fields (mandated by 42005) from *conditionally required* (mandated only under specific triggers, e.g. high-risk classification) from *organisationally chosen*.
- **Seam with EU AI Act Article 27 FRIA** — a short note on how you would structure the schema so a Northbrook Financial system deployed in the EU can produce a compliant FRIA as a *view* on the same underlying assessment. Cite the Article 27 provisions you are addressing.

### `42006-shape-read.md`

- **What the certification body is required to do** — a one-page distillation of the requirements a 42006-conformant auditor must meet: competence, impartiality, evidence collection, reporting, non-conformity handling.
- **What that means the auditee must produce** — for each 42006 requirement above, a one-line corresponding *auditee expectation* (e.g. auditor requires documented evidence of internal-audit programme → auditee must maintain the internal-audit programme's documented information per 42001 Clause 9.2).
- **Two audit-shape failure modes** — from your reading, two ways an AIMS could pass 42001 on paper and still fail a 42006-conformant audit. Name each failure mode and the artefact that would prevent it.

### `family-seams-diagram.md`

A single diagram that shows:

- The four boxes: 27001 ISMS, 42001 AIMS, 42005 impact-assessment process, 42006-conformant audit / certification cycle.
- The arrows: shared machinery, control catalog reuse, impact-assessment integration, audit-body inputs.
- The seams: at least three explicit seams where the AIMS reuses ISMS machinery rather than forking.

### `soa-sketch.md`

Pick three ISO/IEC 42001 Annex A controls that read to you as high-value or high-ambiguity. For each, produce a SoA row in the shape chapter 03 shows: control ID, title, included/excluded, justification, implementing enterprise controls, applicability filter, evidence contract, crosswalk. At least one row must be an *exclusion* with a defensible justification.

## Starter guidance

- Read Clauses 4–10 of ISO/IEC 42001 *before* Annex A. Annex A is unreadable without the shape.
- The reader-audience-per-clause table is where you will earn credibility with executives later. Draft it right after Clauses 5 and 9 (Leadership and Performance Evaluation).
- For 42005, do not paraphrase the process into your own words. The process names are load-bearing; use them verbatim so your schema fields are cite-able against the standard.
- For 42006, the fastest way to build the failure-mode section is to read the standard while asking "what would a lazy auditee try to skip past?" Two examples will emerge quickly.
- The family-seams diagram is *worth* an extra hour. If it is not legible without the text, redraw it.

## Acceptance criteria

- [ ] All five artefacts present and named as specified.
- [ ] 42001 shape read covers Clauses 4–10 with the four required sub-sections filled.
- [ ] Annex A table lists control *objectives* (not individual controls) with one-line descriptions.
- [ ] Vocabulary alignment cites ISO/IEC 22989 definitions verbatim for at least five terms.
- [ ] 42005 shape read includes the process diagram + field table with required / conditional / organisational split.
- [ ] 42005 shape read includes the Article 27 FRIA seam with cited provisions.
- [ ] 42006 shape read includes distilled auditor requirements, auditee expectations, and two failure modes.
- [ ] Family-seams diagram exists, is legible without the text, and shows at least three explicit seams.
- [ ] SoA sketch has three rows with all columns filled; at least one is an exclusion with a defensible justification.
- [ ] All citations to specific standard clauses, articles, or provisions are verified against primary sources — no `<!-- needs-research: ... -->` markers on load-bearing citations.

## Stretch goals

- Add a fourth shape read for **ISO/IEC 23894** as a specialisation of ISO 31000 — one page, focused on how the AI risk-management process plugs into the AIMS Clause 6 planning + Clause 9 performance evaluation seams.
- Add a fifth shape read for **ISO/IEC 38507** focused on the *board-facing vocabulary* only, and produce a one-page executive brief on the AIMS scope you would show a Northbrook Financial audit committee.
- Extend the SoA sketch to ten rows including at least three exclusions, and derive one *organisational applicability filter* that appears on multiple rows (e.g. "US-deployed only") to test the profile-vs-control-vs-SoA placement question from chapter 04.
