# exercise-05: Control-Authoring Lifecycle Flow

**Estimated effort:** 2 hours

## Objective

Walk one candidate control through **every** stage of the chapter 06 lifecycle — proposed → published → deprecated → sunset (and, in one variant, → withdrawn) — producing the artefacts each stage requires. The point is to exercise the transitions and their governance shape, not to author a novel control: you will use a single running example (a training-data provenance variant) and see it move through the entire lifecycle in one sitting.

The deliverable is an audit-narrative packet: any auditor, any regulator, any successor architect should be able to reconstruct *why the control changed the way it did, when, and who approved each transition* by reading the packet alone.

## Prerequisites

- Chapter [`06-control-authoring-lifecycle.md`](../06-control-authoring-lifecycle.md) read once.
- Chapters [`01-anatomy-of-a-control-library-entry.md`](../01-anatomy-of-a-control-library-entry.md) and [`07-evidence-contract-per-control.md`](../07-evidence-contract-per-control.md) — you will reference the seven fields and the evidence-contract shape.
- Exercise-02 completed (you have a `AIC-DAT-014`-equivalent training-data provenance entry to run through the flow).
- Skim of Semantic Versioning 2.0 (<https://semver.org/>) — chapter 06 uses a semver-ish scheme; the drill needs you to classify the changes correctly.

## Scenario

You are the Northbrook architect. The following sequence of events happens over eight quarters:

1. **2026-Q1 (proposal).** `ai-risk-engineer` (assume the seat has now been filled, per the exercise-01 gap you flagged) proposes a *new* control `AIC-DAT-014` for training-data provenance. The proposal is scoped tightly.
2. **2026-Q2 (adoption).** Approved; ships in `AIC 2026.1.0`.
3. **2026-Q3 (minor modification).** A `MITRE ATLAS` mitigation crosswalk edge is added; the guidance points to a new enterprise dataset-license registry.
4. **2027-Q1 (major modification).** The EU AI Act delegated act tightens Article 10's data-quality obligations. The applicability filter is broadened (new: `jurisdiction: [EU]` was already there, adding `system_kind: agent-with-tool-use`) and an evidence-contract change is required (schema URN bumps to `#2.0`, which is *not* backwards-compatible with `#1.4`).
5. **2027-Q2 (deprecation).** Northbrook consolidates provenance under a new control `AIC-DAT-030` that generalises over both training-data and RAG-corpus provenance. `AIC-DAT-014` is deprecated in favour of it.
6. **2028-Q2 (sunset).** End of the 12-month deprecation window; `AIC-DAT-014` is sunset.
7. **Alternate branch (exercise-only).** In a parallel-universe branch, suppose the 2027-Q1 major had *not* been well-scoped and had turned out to name an outcome the enterprise could never satisfy on any system. Show the *withdrawal* path instead of the deprecation path.

## Deliverables

Author a single **lifecycle packet** in a working directory of your choice, containing:

1. **`proposal-2026-Q1.md`** — the initial proposal.
2. **`review-2026-Q1.md`** — the architect's stage-2 review, with the four required questions answered.
3. **`release-notes-2026.1.0.md`** — the release-notes entry for adoption.
4. **`change-2026-Q3-minor.md`** — the minor modification proposal, its classification argument, and the release-notes fragment for `2026.2.0`.
5. **`change-2027-Q1-major.md`** — the major modification proposal, its classification argument, the release-notes fragment for `2027.1.0`, and the migration guidance for downstream SSPs.
6. **`deprecation-2027-Q2.md`** — the deprecation notice, the `superseded_by` pointer, the migration guide, and the deprecation-window justification.
7. **`sunset-2028-Q2.md`** — the sunset notice, the POA&M template for non-migrated SSPs, and a statement of why the sunset entry stays in the catalog forever.
8. **`change-log-AIC-DAT-014.md`** — the per-control append-only change log covering every version transition above.
9. **`withdrawal-alternate.md`** — the alternate-universe withdrawal artefact.

Every artefact must be dated, signed (by role, not person), and reference the specific chapter-06 stage it belongs to.

## Requirements

### `proposal-2026-Q1.md`

The five-field proposal shape from chapter 06:

- **Trigger** — the concrete incident, obligation, evaluation finding, vendor change, or jurisdictional update that motivates the proposal. Do not write "we should have this"; write the specific triggering event.
- **Draft entry** — the seven fields from chapter 01. Gaps allowed as `<!-- needs-research: … -->`.
- **Overlap check** — the specific search against existing controls (assume the catalog is small on 2026-Q1) and the argument that no existing control's outcome already covers this.
- **Affected profiles** — which of exercise-03's profiles (or their equivalents) would need to include this control.
- **Downstream impact** — which analyst / engineer / evaluation roles will produce or consume evidence for it, and what changes for them.

### `review-2026-Q1.md`

The stage-2 architect review, answering the four questions from chapter 06 explicitly and in order:

1. Is this a new outcome? (Not: is it a new *statement* — is the outcome new.)
2. Is the statement outcome-oriented, testable, and free of mechanism?
3. Is the applicability filter tight?
4. Are the evidence and testing fields sufficient for the level-15 and level-25 roles to consume without a follow-up conversation?

Explicit *reject / revise / approve* per question. If any question is answered "revise," the review artefact must state the specific revision requested, not "tighten the applicability filter."

### `release-notes-2026.1.0.md`

The chapter 06 release-notes format, with an `## Added` entry for the new control. The entry must include, per chapter 06, *what it commits the org to, why the org adopted it, which profiles include it, which roles produce evidence for it*. The *why* field is what downstream consumers use to decide whether to migrate proactively — do not omit it.

### `change-2026-Q3-minor.md`

- Proposal shape (light-weight relative to a new-control proposal).
- **Classification argument** — the change is *minor*. Defend it against the chapter 06 rules: additive crosswalk edge, guidance expansion, no statement change, no evidence-contract change. If your defence has any wobble, the change is not minor and you have misclassified.
- Release-notes fragment for the next scheduled minor (`2026.2.0`).

### `change-2027-Q1-major.md`

- Proposal shape.
- **Classification argument** — the change is *major*. Defend against the chapter 06 rules: statement is not changing here, but the *applicability filter is broadening* (a major move) and the *evidence-contract schema is a breaking bump* (a major move). Either alone is major; both together underline it.
- **Migration guidance for downstream SSPs** — a specific, per-SSP-audience paragraph explaining which SSPs must revisit, which evidence artefacts remain valid under the old schema, which need re-collection under the new schema, and the deadline.
- Release-notes fragment for the next scheduled major (`2027.1.0`).

### `deprecation-2027-Q2.md`

- The deprecation notice with the specific effective date and the sunset date (12-month window per chapter 06's data-family default).
- The `superseded_by: AIC-DAT-030` pointer.
- The migration guide (`MIG-DAT-014-to-030.md` may be a stub file with a proper skeleton).
- The deprecation-window justification — chapter 06 says the window is *family-appropriate*; justify 12 months for a data-family control against Northbrook's downstream SSP count and revalidation cost.

### `sunset-2028-Q2.md`

- The sunset notice.
- The POA&M template that the governance analyst will populate with the list of SSPs that failed to migrate before sunset (the analyst's tracker will flag them).
- One paragraph on why the sunset OSCAL entry stays in the catalog forever, referencing chapter 06's "audit-trail retention" rule.

### `change-log-AIC-DAT-014.md`

An append-only log with one entry per version transition — `1.0.0 → 1.1.0`, `1.1.0 → 2.0.0`, `2.0.0 (deprecated)`, `2.0.0 (sunset)`. Each entry: date, versions, diff summary, release, rationale (pointing back to the proposal artefact), approver role. Append-only means no edits, ever — a bad entry is corrected by a new entry.

### `withdrawal-alternate.md`

The parallel-universe withdrawal artefact: a one-page memo stating (a) the discovery that the 2027-Q1 major named an unattainable outcome, (b) the routing of the withdrawal decision through the architect + head-of-AI-governance escalation chapter 06 requires (a withdrawal is *not* an architect-alone decision), (c) the correction notice to downstream artefacts, and (d) the reflection: what stage-2 review discipline would have caught the flaw before publication.

## Starter guidance

- Do the work in the artefact order listed. Each stage depends on the prior artefact being complete; skipping ahead makes the classification arguments impossible to defend.
- On the 2027-Q1 major, resist the temptation to classify the *applicability broadening* as minor because it "just adds systems in scope." Chapter 06 lists applicability narrowing OR broadening as major, on purpose: newly-in-scope systems need to revisit.
- The evidence-contract schema bump from `#1.4` to `#2.0` is the field most likely to be mis-classified. If the old schema's artefacts cannot be re-validated under the new schema without reprocessing, that is a breaking change. Say so.
- Do not merge the change-log entries into paragraph form. Chapter 06's append-only immutability is a specific commitment; the change log is a *log*, not a narrative.
- On the withdrawal alternate, remember that withdrawal is *rare*. If your artefact starts to feel routine, you have written a deprecation. Withdrawal exists for the "we authored a bad control" case; treat it as the exceptional event it is.

## Acceptance criteria

- [ ] All nine artefacts produced, each dated and signed by role.
- [ ] Proposal contains all five fields from chapter 06.
- [ ] Stage-2 review answers all four questions in order with an explicit reject/revise/approve.
- [ ] Both modification proposals classify the change *and* defend the classification against chapter 06's specific rules.
- [ ] 2027-Q1 major-modification artefact includes an SSP migration paragraph, not just a release-notes bump.
- [ ] Deprecation window (12 months) is justified against Northbrook's downstream count and family-appropriate defaults.
- [ ] Sunset artefact explicitly states the sunset OSCAL entry remains in the catalog for the life of the catalog.
- [ ] Change log is append-only (no edits, only new entries) and covers every version transition.
- [ ] Withdrawal-alternate names the head-of-AI-governance in the routing per chapter 06.
- [ ] No artefact uses "TBD," "as needed," or "when possible" for a governed transition date.

## Stretch goals

- Add a *deprecation-window escalation* artefact for the case where the pricing team (from exercise-04) refuses to migrate off the deprecated control before the sunset date. What does the architect do? Chapter 06 says the POA&M is the answer; walk the specific routing, including the head-of-AI-governance escalation.
- Sketch the *release-cadence dashboard* the head-of-AI-governance sees at the end of 2027-Q4: number of proposals in queue, number of majors vs. minors this year, deprecation-window compliance rate, sunset backlog. One paragraph, not a chart.
- Extend the change log with a `patch` transition (`2.0.0 → 2.0.1`) covering an editorial-only fix (a typo in the guidance). Verify per chapter 06 that patches ride the *next* release rather than getting their own release.
