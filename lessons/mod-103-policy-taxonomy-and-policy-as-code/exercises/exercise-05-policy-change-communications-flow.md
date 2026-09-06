# exercise-05: Policy-Change Communications Flow

**Estimated effort:** 3 hours

## Objective

Walk **one** real policy change through the change-communications flow from chapter [`05-policy-change-communications-and-deprecation-windows.md`](../05-policy-change-communications-and-deprecation-windows.md) end-to-end — from class assignment through the seven-part notice packet through the release notice through the deprecation-window plan — so the discipline is felt rather than described. The exercise is the fence against silent Friday publication and against collapsing the publication / effective / sunset dates into one; both failure modes are the primary source of surprise audit findings in AI-governance programmes.

## Prerequisites

- Chapter [`05-policy-change-communications-and-deprecation-windows.md`](../05-policy-change-communications-and-deprecation-windows.md) read once, with the five-class table, the routing list, and the seven-part notice-packet shape absorbed.
- Skim of the mod-102 control-authoring lifecycle in [`../../mod-102-ai-control-library-architecture/06-control-authoring-lifecycle.md`](../../mod-102-ai-control-library-architecture/06-control-authoring-lifecycle.md) — the coordinated cadence the policy release must bridge.
- Forward reference to chapter [`04-exception-and-waiver-workflow.md`](../04-exception-and-waiver-workflow.md) for how the waiver register consumes the effective-date event.
- The Northbrook Financial scenario carried through the track.

## Scenario

Pick **one** of the two change scenarios and run it end-to-end. Do not attempt both — the depth of the packet matters more than the breadth of the drill.

- **Scenario A — planned expansion.** Northbrook's AI Human Oversight Standard currently applies to tier-1 decision systems only. The internal audit function has recommended broadening applicability to tier-2 *agent* systems (agentic orchestrators that chain model calls) that were previously scoped out. The head-of-AI-governance has queued the change for the next release; you must run the flow.
- **Scenario B — regulator-driven tightening.** A jurisdiction where Northbrook operates has published a rule with a short compliance window that materially tightens the GenAI Acceptable Use Policy's requirements for customer-facing generative outputs — human-review requirement, mandatory logging, and a disclosure obligation. You must run the flow under emergency-class time pressure without discarding the notice-packet shape.

The two scenarios exercise different points on the class spectrum (A is major; B is likely emergency) — pick to stretch yourself, not to minimise work.

## Deliverables

Author four artefacts (plus a small directory tree) in a working directory of your choice:

1. **`change-classification.md`** — the one-page classification decision.
2. **`notice-packet/`** — a directory containing all seven required sections, each in its own file.
3. **`release-notice.md`** — the versioned release-notice artefact that will sit at the head of the policy repo.
4. **`window-plan.md`** — the deprecation-window plan.

## Requirements

### `change-classification.md`

Half a page. States:

- The class assigned (editorial / minor / major / breaking / emergency), the walk that produced the class (which of the class-defining criteria in chapter 05 apply), and the class's default notice window.
- The two most consequential *downstream artefact classes* affected — usually one control-library entry (`AIC-*`), one PaC bundle, or one evidence-collection cadence.
- The trigger (framework revision, regulator action, incident retrospective, internal audit finding). For scenario B, name the regulator and cite the specific rule (marking with `<!-- needs-research: ... -->` if you do not want to invent the citation).

### `notice-packet/`

Exactly seven files, matching the seven required sections from chapter 05:

- **`01-diff.md`** — the literal old-text / new-text diff. Side-by-side or unified-diff format. Summarising instead of showing the diff is not permitted — reviewers must see the exact wording.
- **`02-class-and-rationale.md`** — the class from `change-classification.md` (transcribed, not linked), the rationale in two-to-three paragraphs, and the alternatives considered with the one-line reason each was rejected.
- **`03-impacted-controls.md`** — the list of `AIC-*` control IDs whose statement, applicability, evidence contract, or crosswalk will need updating. State that this list is maintained by the ai-governance-analyst who walked the mapping matrix from chapter [`02-principle-to-policy-to-standard-to-control-traceability.md`](../02-principle-to-policy-to-standard-to-control-traceability.md) against the delta; if empty, say so and explain why.
- **`04-impacted-pac-bundles.md`** — the list of PaC bundle IDs (from chapter [`03-policy-as-code-enforcement-tiers.md`](../03-policy-as-code-enforcement-tiers.md)) whose rules will need updating, with a first-cut on which are simple substitutions vs. which require rule rewrites. State that this list is confirmed by the ai-risk-engineer; if empty, say so.
- **`05-dates-and-milestones.md`** — three dates named DISTINCTLY: **publication date** (when the new text appears in the repo), **effective date** (when the new text becomes authoritative), and **sunset date** (when the old text is no longer consumable). Plus the no-go-live-past-this-date milestones inside the window (e.g., "no new SoA may pin to the old text after week N").
- **`06-migration-guidance.md`** — a checklist of the migration steps for downstream roles (matrix rows to update, evidence to re-collect under the new cadence, PaC bundles to re-run under the new rule, model-card sections to refresh). Not a procedure — pointers.
- **`07-rollback-plan.md`** — who has authority to revert, to what state (usually the old text remains authoritative during the window so rollback is available), on what trigger, and what the rollback communication looks like. For scenario B (emergency), rollback may not be a full revert — name the *compensating stance* instead.

None of the seven may be skipped. If a section is thin (e.g., no PaC bundle affected), the file explicitly says so — the file is not deleted.

### `release-notice.md`

The versioned release-notice artefact ready to sit at the head of the policy repo. Follow the shape of chapter 05's worked release notice header: class, effective date, deprecation-window end, summary, changed clauses, impacted downstream artefacts, routing acknowledgements (with the deterministic roles marked for the class per chapter 05's routing-list table — no role skipped for convenience), rollback pointer, migration-guidance pointer. Include ISO dates and placeholder acknowledgement records with realistic acknowledgement dates.

The routing SLAs (5-day acknowledge / 15-day plan / 30-day complete or effective-date-whichever-later) are named explicitly in the notice.

### `window-plan.md`

The deprecation-window plan for the chosen change:

- The window length, justified against the class-minimum from chapter 05 (2 weeks minor / 4 weeks major / 8-12 weeks breaking; compressed but not skipped for emergency).
- The dual-check plan for any PaC bundle affected — how the CI pipeline evaluates the request against the old rule and the new rule in parallel during the window, with the old result authoritative and the new result logged for validation. If no bundle is affected, state that and explain why.
- The evidence-cadence handling if the new policy tightens cadence — old cadence continues to the effective date; new cadence begins at effective date; retention rules for the old-cadence artefacts.
- The sunset event description — the sunset-notice that ships (not a silent end), the marking of the old text as non-authoritative but retained in repo history, and the flag routed to any SoA still pinned to the old text.
- Coordination with the mod-102 control-library release cadence — which library release will land the downstream `AIC-*` changes, and how the bridging period (policy effective, library not yet updated) is tracked in the ai-governance-analyst's traceability index.
- Explicit callouts of the two chapter-05 shape mistakes (silent Friday publication; publication-date-treated-as-effective-date) with a one-line statement of how the plan avoids each.

## Starter guidance

- Draft the diff before you draft the rationale. If you cannot show the text delta, you have not made a policy decision — you have written a memo.
- The three dates in section 05 of the packet are non-negotiable. Even for editorial changes, name them (publication = effective = sunset = same day; make it explicit rather than implicit).
- For scenario B, the emergency class *compresses* the routing SLAs; it does not eliminate them. Acknowledge within 1 business day, plan within 3, complete on a schedule set by the regulator's own compliance clock. Document the compressed timeline in the emergency-change record.
- The temptation with the impacted-controls list is to punt to "the analyst will figure it out". Don't — the packet is not ready until the analyst has returned the list. State that as a hard gate at the top of `03-impacted-controls.md`.

## Acceptance criteria

- [ ] `change-classification.md` names the class and walks the class-defining criteria; the class matches the change described.
- [ ] The notice packet has all seven files. None is skipped; thin sections are populated with an explicit "no impact" note rather than deleted.
- [ ] The three dates (publication / effective / sunset) are named DISTINCTLY in `05-dates-and-milestones.md` and never collapsed elsewhere.
- [ ] The routing list in `release-notice.md` includes every role marked for the change class in chapter 05's routing-list table.
- [ ] The routing SLAs (5 / 15 / 30 business days for standard cadence; the compressed variants for emergency) are named in the release notice.
- [ ] `07-rollback-plan.md` names a concrete reverting authority and a concrete return-state; for emergency classes, the compensating stance is named where a full revert is not possible.
- [ ] `window-plan.md` addresses coordination with the mod-102 control-library cadence — either the same-release bundling or the bridging period with tracking.
- [ ] `window-plan.md` explicitly names both chapter-05 shape mistakes and how the plan avoided each.
- [ ] The `01-diff.md` file shows a literal text delta, not a summary.
- [ ] Emergency-class packet (if scenario B) documents the compressed SLA timeline and includes an emergency-change record fragment rather than skipping the packet.

## Stretch goals

- Draft the pre-merge repository hook (as a `.github/workflows/*.yaml` stub or an equivalent pipeline definition) that enforces notice-packet completeness as a gate — the merge fails if any of the seven files is missing or if the release-notice routing acknowledgements are absent.
- Sketch the dual-check log for one PaC bundle affected by the change. Include a mock log entry showing what the analyst would see if the new rule turned out unexpectedly stricter (e.g., blocked 30% of the previous week's deploys had the new rule been authoritative).
- Design the sunset-notice ship shape — short-form email plus repo artefact — so the sunset event is governed rather than silent. Name the recipients, the trigger, and the retention rule for the old text.
- Author the *policy-review sweep* query the ai-governance-analyst runs each quarter — a query over the waiver register from exercise-04 that lists policy clauses accumulating repeated waivers, since a clause with three waivers in a year is a policy-amendment signal that should feed back into this change flow. Half a page.
