# exercise-02: Principle-to-Policy-to-Control Crosswalk

**Estimated effort:** 3 hours

## Objective

Author the **traceability mapping matrix** for five of Northbrook's Responsible AI principles — from principle to binding policy to standard to control to evidence artefact — and prove, with two standing queries the ai-governance-analyst can run, that none of the five is decorative. This is where the *principle wall* from chapter [`02-principle-to-policy-to-standard-to-control-traceability.md`](../02-principle-to-policy-to-standard-to-control-traceability.md) either breaks or holds; the exercise is the discipline that converts a slide of principles into a walkable graph terminating in a file an auditor is served.

## Prerequisites

- Chapter [`02-principle-to-policy-to-standard-to-control-traceability.md`](../02-principle-to-policy-to-standard-to-control-traceability.md) read once, including the fairness worked example at the end.
- Chapter [`01-ai-policy-hierarchy-authority-and-cadence.md`](../01-ai-policy-hierarchy-authority-and-cadence.md) — the taxonomy shape you are threading the matrix through.
- Skim of the mod-102 catalog anatomy in [`../../mod-102-ai-control-library-architecture/01-anatomy-of-a-control-library-entry.md`](../../mod-102-ai-control-library-architecture/01-anatomy-of-a-control-library-entry.md) for the crosswalk field shape you will extend with `principle_ids`.
- The Northbrook Financial scenario from mod-101 exercise-02 and mod-102 exercise-01.

## Scenario

Northbrook's board ratified a set of Responsible AI principles at the end of the last quarter. External counsel has been briefing the audit committee on regulator posture; the committee has asked the head-of-AI-governance a plain question at the next meeting: *"for each of our published principles, show me the policy clause it produced, the standard, the control that tests it, and the last evidence artefact that control emitted."* The head-of-AI-governance has asked you, the level-50 architect, to have a defensible matrix in place for the *five most exposed* principles before the meeting.

You pick the five. Choose principles that stress the matrix differently rather than five variants of the same shape — the matrix is being tested for range, not neatness:

- A governance-shaped principle (accountability, oversight).
- A data-shaped principle (privacy, data governance).
- A model-behaviour-shaped principle (fairness / non-discrimination, robustness).
- A transparency-shaped principle (explainability, disclosure).
- A supply-chain-shaped principle (third-party integrity, provenance).

## Deliverables

Author four artefacts in a working directory of your choice:

1. **`rai-principle-register.yaml`** — the five principles you are exercising.
2. **`traceability-matrix.yaml`** (or `.csv`) — the mapping-matrix file.
3. **`detector-queries.md`** — the orphan and widow detector queries plus a plain-English interpretation.
4. **`crosswalk-defensibility-note.md`** — one page for the audit committee meeting.

## Requirements

### `rai-principle-register.yaml`

Five entries. Each entry: stable ID (`RAI-01` … `RAI-08` — pick from an eight-principle register or a plausible sub-set you invent for Northbrook and mark as such), the principle statement (declarative present tense; layer 1 shape per chapter 01), and the anchor-source list mapping to at least one characteristic in each of OECD AI Principles, UNESCO Recommendation 2021, and NIST AI RMF trustworthy characteristics. If Northbrook's register does not currently exist, state that assumption explicitly at the top of the file.

### `traceability-matrix.yaml`

One row per edge assertion. Six mandatory columns per chapter 02 — `principle_id`, `policy_clause_id`, `standard_clause_id`, `control_id`, `evidence_artefact_ref`, `verified_date` — plus the optional `edge_kind` (`derives_from` or `assists_with`) and optional `owning_role`. Constraints:

- Every one of the five principles is covered by *at least one* row with all six mandatory columns non-null.
- At least one principle carries *multiple rows* (the N:M shape from chapter 02 — a single principle producing more than one policy-clause / standard-clause / control triple, or two different controls satisfying the same triple).
- At least one row is `edge_kind: derives_from` and at least one is `edge_kind: assists_with`. The distinction is load-bearing for change propagation and must be visible.
- Every control ID cited (`AIC-<family>-<seq>`) either exists in the running mod-102 catalog or is marked `<!-- planned: AIC-2026.3 -->` per the mod-102 chapter 06 versioning.
- Every external-framework citation (NIST AI RMF sub-category, ISO/IEC 42001 Annex A control, EU AI Act Article) is either verified against the primary source or carries a `<!-- needs-research: ... -->` marker.
- `verified_date` is populated on every row with a plausible ISO date within the last two review cycles.

### `detector-queries.md`

Two standing queries the ai-governance-analyst will run each quarter, expressed as SQL-ish or `jq`-ish pseudocode plus a plain-English interpretation of what a hit means and what the analyst does next:

- **Orphan detector.** "List every `principle_id` in `rai-principle-register.yaml` that has no row in `traceability-matrix.yaml` with a non-null `control_id`." A hit is a rule-1 violation (zero-degree principle — decorative). Include the follow-up action: file a finding, escalate to the head-of-AI-governance, and route the principle for either downstream coverage or amendment.
- **Widow detector.** "List every `control_id` in the mod-102 control library whose `crosswalk.principle_ids` list is empty or missing." A hit is a rule-4 violation (control without upstream principle warrant — often an engineering-side control drafted by ai-risk-engineer per mod-102 chapter 08 without an architect-side upstream pass). Include the follow-up action.

Cover the third failure mode from chapter 02 (control retired without back-reference cleanup) explicitly in the prose — either as a third query or as a data-hygiene note attached to the widow detector.

### `crosswalk-defensibility-note.md`

One page for the audit committee. For each of the five principles, name in one line: the evidence artefact an auditor would be handed (`<artefact-name>@<schema-ref>` per chapter 02), the role that produces it, and the cadence. If any of the five principles still resolves to a slide deck rather than to a file, say so — do not paper over the gap; note it and describe what needs to change (either the principle is decorative and should be retired, or the missing standard/control needs to be authored before the audit-committee meeting).

Close the note with the *two shape mistakes* from chapter 02 (complete-bipartite "everything traces to everything"; conflating `derives_from` with `assists_with`) and a one-sentence-each statement of how the matrix avoided each.

## Starter guidance

- Start with the fairness example at the end of chapter 02. Copy its two rows for `RAI-04` verbatim into your matrix as row 1 and row 2; then extend the matrix to cover the other four principles you picked. This forces you to feel where the fairness pattern fits and where it does not.
- If a principle "traces to five standards and eleven controls", it probably has one edge that is `derives_from` and the rest that are `assists_with`. Draw the distinction; do not flatten.
- Do not populate the matrix with rows whose only purpose is to satisfy the orphan detector. A row that traces `RAI-05 -> POL-??? -> STD-??? -> AIC-???` with three question marks is worse than an empty entry — it is a lie that will be caught at the next review.
- The `owning_role` column is optional; consider populating it for the rows where the assertion is contested. The signature is what the audit trail reads.

## Acceptance criteria

- [ ] Five principles in the register, each with a stable ID, a layer-1-shaped statement, and anchors in OECD + UNESCO + NIST AI RMF.
- [ ] Traceability matrix contains at least one non-null row per principle covering all six mandatory columns.
- [ ] At least one principle has multiple rows (N:M shape).
- [ ] At least one `edge_kind: derives_from` and one `edge_kind: assists_with` row are present, with the distinction explained in the defensibility note.
- [ ] Every control ID resolves to a mod-102 entry or is marked `<!-- planned: ... -->`; every external-framework citation is verified or marked `<!-- needs-research: ... -->`.
- [ ] `detector-queries.md` contains runnable-in-principle pseudocode for the orphan and widow detectors plus the plain-English interpretation and the follow-up action.
- [ ] `crosswalk-defensibility-note.md` fits on one page, names the artefact + role + cadence per principle, and flags any principle still resolving to a slide.
- [ ] `crosswalk-defensibility-note.md` calls out both chapter 02 shape mistakes (complete-bipartite; `derives_from`/`assists_with` conflation) and states how the matrix avoided each.
- [ ] No matrix row exists purely to satisfy a detector; if you had to add one, remove it and file the underlying gap as a finding in the note.

## Stretch goals

- Prototype an OSCAL projection of the same matrix (chapter 02 "Option B") — one YAML file expressing principles as a bespoke catalog, controls as OSCAL controls, and the matrix rows as `link` elements. Half a page of prose on why you did (or did not) recommend adoption.
- Author two adversarial test cases for the orphan detector — one where a principle is amended without walking the matrix (rows silently stale), and one where a control is retired without cleaning the back-reference (dangling `control_id`). Show what each hit looks like and how the analyst distinguishes them.
- Propose a lightweight cadence for the quarterly traceability review — the concrete half-day the ai-governance-analyst spends walking the matrix, running the two detectors, and producing the report the architect signs. Name the checkpoints inside the half-day.
- Draft the escalation shape for a `derives_from`-edge violation vs. an `assists_with`-edge violation. The two should not route to the same severity by default; explain the split.
