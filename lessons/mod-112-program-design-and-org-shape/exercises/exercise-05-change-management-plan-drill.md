# exercise-05: Change-Management Plan Drill

**Estimated effort:** 2.5 hours

## Objective

Author the **change-management plan** for the enterprise chosen in exercise-02, and — as the applied test — walk a specific **major change** through the seven-step playbook end to end. Chapter 05 designs the plan; this exercise produces the plan and instruments a worked rollout so that the exception process, training composition, and adoption-monitoring metrics are exercised, not merely stated.

The deliverable set is the machine-readable change-management plan across the four change classes, the seven-step-playbook runbook, an exception-process template and register schema, a worked rollout timeline for the scenario's major change, and the post-rollout review template. The correctness spine is chapter 05's six invariants (every change has a named class; every non-emergency change traverses the seven-step playbook; every change with training implications updates mod-105; exceptions are tracked and closed; post-rollout reviews are conducted; exception-window defaults are calibrated against enterprise adoption data) and its three failure modes (publish-and-pray; training-update lag; silent-emergency backlog).

## Prerequisites

- Chapter [`05-change-management-plan-for-policy-and-standard-rollouts.md`](../05-change-management-plan-for-policy-and-standard-rollouts.md) read once, with the four change classes, the seven-step playbook, the exception process, the training-update composition, and the invariants marked.
- Chapter [`01-ai-governance-council-charter-and-decision-forum.md`](../01-ai-governance-council-charter-and-decision-forum.md) skimmed — the council ratifies major changes as reserved matters; emergency changes are retroactively ratified at the next standing meeting.
- Chapter [`04-cross-audience-communications-architecture.md`](../04-cross-audience-communications-architecture.md) skimmed — the rollout communications set draws on the audience map and derivatives shape.
- Chapter [`08-boundary-to-head-of-ai-governance-and-chief-ai-officer.md`](../08-boundary-to-head-of-ai-governance-and-chief-ai-officer.md) skimmed — the head's bounded emergency authority is the ratification path when the council cannot convene even ad-hoc.
- The mod-103 policy taxonomy, mod-105 chapter 06 competence programme, and mod-111 GRC-for-AI platform workflow-layer discussions — the plan composes with all three.

## Scenario

Carry the enterprise chosen in exercise-02 forward. The specific major change the worked rollout addresses:

- **Bank scenario.** The mod-104 jurisdiction-reconciled control set increments to v2.0 because a state-level AI law (California SB 942-adjacent or a state-attorney-general enforcement action, depending on the plausible current shape) has added a documentation-and-disclosure obligation to consumer-facing AI-enabled decision tools. `<!-- needs-research: verify current state-law landscape at authoring date; the specific triggering law may be different by the time the exercise is completed -->` The rollout affects the enterprise's model-owner cohort (documentation obligations), the customer-facing product organisation (disclosure obligations), and the second-line seats (updated attestation shape).
- **SaaS vendor scenario.** The CEN-CENELEC JTC 21 harmonised standard for EU AI Act Article 9 (risk management system) has published, and the enterprise's mod-104 jurisdiction-reconciled control set increments to v2.0 to compose with the harmonised standard. The rollout affects the enterprise's platform team (implementing the risk-management shape the harmonised standard requires), the second-line seats (attestation shape), and the enterprise's customer contracts (disclosure obligations to enterprise deployers). `<!-- needs-research: verify current publication status of the CEN-CENELEC JTC 21 harmonised standard set at authoring date -->`
- **Healthcare scenario.** The FDA publishes an updated guidance on PCCP applicability to a class of AI-enabled clinical-decision-support tools the enterprise ships, and the enterprise's mod-107 assurance architecture and mod-102 control library increment to compose. The rollout affects the enterprise's clinical-decision-support product team (PCCP composition), the clinical-safety committee (updated review shape), and the second-line seats. `<!-- needs-research: verify current FDA PCCP guidance status at authoring date -->`

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`change-management-plan-v1.0.yaml`** — the machine-readable plan across the four change classes.
2. **`seven-step-playbook-runbook.md`** — the operational runbook the plan's step-by-step execution follows, with owner, artefact, and gate criteria per step.
3. **`exception-process-template.md`** — the exception-request template, the exception-register schema, and the approval-authority table.
4. **`worked-rollout-timeline-<change-id>.md`** — a Gantt-style timeline of the scenario's major change through the seven steps with dated milestones, communications-set inventory, training-module inventory, and adoption metrics.
5. **`post-rollout-review-template.md`** — the template for the step-7 review the head-of-AI-governance authors and the council (or ARB, for standard changes) minutes.

## Requirements

### `change-management-plan-v1.0.yaml`

The machine-readable plan. Structure per chapter 05's schematic:

- **Metadata.** `id`, `ratified_by` (AI governance council), `composes_with` (mod-103, mod-105 chapter 06, mod-107, mod-112 chapter 04, mod-111 workflow layer).
- **Change classes.** One block per class (small / standard / major / emergency) with cadence, communications shape, training-update discipline, exception window (state the days), ratification body.
- **Seven-step playbook reference.** `playbook_ref` pointing at `seven-step-playbook-runbook.md`.
- **Exception process reference.** `exception_process_ref` pointing at `exception-process-template.md`.
- **Training update composition.** State the composition rules with the mod-105 chapter 06 competence programme: standard changes absorb into monthly refresh; major changes carry a dedicated pre-effective-date module with 90-day completion; emergency changes trigger just-in-time briefings and absorb at the next major cycle; third-line-facing training discipline (internal-audit lead consulted at step 1).
- **Exception-window calibration.** State the enterprise's exception-window defaults with the calibration expectation — that after four quarterly cycles the enterprise reviews the defaults against actual adoption data and re-ratifies (chapter 05's invariant 6).
- **Emergency authority.** State the council ad-hoc convening path and the head-of-AI-governance's bounded emergency authority (per chapter 08). Retroactive council ratification is mandatory at the next standing meeting; state that the retroactive item is a standing agenda item.
- **Invariants block.** All six chapter-05 invariants with a `test:` per invariant.

### `seven-step-playbook-runbook.md`

The operational runbook. One section per step:

1. **Proposal and technical draft.** Owner: originating seat (typically architect for major; may be a second-line specialist for scoped standard changes). Artefact: change proposal (class, artefacts affected, seats affected, rationale, recommended effective date). Gate: proposal complete and reviewed by head-of-AI-governance before step 2.
2. **Ratification.** Owner: council (major) or ARB (standard). Artefact: ratification minute with conditions attached (exception window, training completion date, monitoring window). Gate: minute signed per chapter 01 minute discipline.
3. **Communications drafting.** Owner: head-of-AI-governance (with communications function for major). Artefact: multi-audience communications set (programme newsletter segment; manager-cascade briefing; engineering all-hands segment; affected-seat personal notifications; customer-facing communication where relevant; regulator-facing communication where relevant). Chapter 04's audience map is the reference; every audience the change affects has an item in the set. Gate: all items authored and reviewed before step 5.
4. **Training update.** Owner: mod-105 chapter 06 competence-programme owner. Artefact: updated training module (dedicated for major; monthly-refresh absorption for standard; just-in-time briefing for emergency) with defined completion tracking. Gate: module authored before effective date for major changes.
5. **Rollout communication and effective date.** Owner: head-of-AI-governance. Artefact: rollout communication set issued through planned channels; GRC-for-AI platform updated (mod-111 workflow updates, version pins, evidence-artefact schema updates); exception window opens. Gate: rollout communication delivered on schedule.
6. **Adoption monitoring.** Owner: head-of-AI-governance + architect. Artefact: adoption dashboard (new work products using new shape; training completions; exception requests received; in-flight adaptations). Cadence: weekly for major changes over the exception window. Gate: adoption on trajectory or additional-engagement action triggered.
7. **Closure and post-rollout review.** Owner: head-of-AI-governance. Artefact: post-rollout review (see `post-rollout-review-template.md`). Gate: review authored and minuted at council (major) or ARB (standard).

### `exception-process-template.md`

Three sub-artefacts:

- **Exception-request template.** Fields: request id, requester (first-line seat), work-product id, change id, requested disposition (complete against new shape / extend exception window beyond default / other), reason, second-line contact, requested closure date.
- **Exception-register schema.** Fields per exception: exception id, change id, work-product id, disposition, approver seat, approval date, closure date, audit-flag (true where the exception is open past window without documented escalation, per chapter 05's invariant 4 test).
- **Approval-authority table.** For standard changes: exceptions approved by architect at ARB. For major changes: exceptions approved by head-of-AI-governance; where a major-change exception would delay a mod-102 control implementation beyond 180 days or would affect a jurisdiction-reconciled control-set obligation, the exception escalates to the council.

### `worked-rollout-timeline-<change-id>.md`

The Gantt-style timeline. Structure:

- **Header.** Change id, change class (major for the scenario), effective date, exception-window closure date (90 or 180 days per chapter 05 default).
- **Step-by-step timeline.** Each of the seven steps with a start date, end date, owner, and artefacts produced. State the calendar reference (e.g., "week of month 1" or specific date placeholders).
- **Communications-set inventory.** The specific items produced in step 3, one row per audience — the audience name, the item name, the item shape, the delivery date, the delivery channel.
- **Training-module inventory.** The specific training modules updated or created in step 4, with the seat cohort affected, the module version, the pre-effective-date authoring milestone, and the 90-day completion tracking milestone.
- **Adoption metrics.** The metrics tracked in step 6 — new work products using the new shape, training completions percent, exception requests count and disposition, in-flight adaptations count. State the reporting cadence to the head and the escalation trigger where adoption falls below trajectory.
- **Closure milestone.** The date the exception window closes, the seats that produce the closure minute, and the seat that authors the post-rollout review.

### `post-rollout-review-template.md`

The review template. Sections:

- **Change summary.** What changed, when it became effective, when the exception window closed.
- **Adoption outcome.** Metrics at closure — new-shape adoption rate, training completion rate, exception request count and disposition, any escalations.
- **What went well.** Elements of the rollout that operated as designed — the head-of-AI-governance and architect co-author.
- **What did not.** Elements that fell short — publish-and-pray-style shortcuts that were caught (or missed), training-lag issues, exception-process gaps.
- **Lessons for future rollouts.** Named improvements to the change-management plan or the playbook that the review recommends. Recommended changes to the plan become their own change proposals at step 1.
- **Council or ARB minute reference.** The forum where the review was minuted.

## Starter guidance

The four change classes are the plan's discipline; every change must be classifiable and no change is unclassified. A change that resists classification is a change the plan is not designed for, and the fix is to either (a) reclassify by breaking the change into smaller changes each fitting a class, or (b) explicitly amend the plan to add a class (which is itself a major change under chapter 05). Chapter 05's invariant 1 is uncompromising: every change has a named class.

The seven-step playbook is boring. That is by design. Chapter 05's failure-mode-1 defence (publish-and-pray) is that every non-emergency change traverses the same seven steps whether the enterprise is enthusiastic about the change or grudging about it. The runbook must specify the owner and artefact per step so that step-skipping is visible in the record; the head-of-AI-governance's role at each step is enforcement, not optional.

Training update is where most rollouts fail. Chapter 05's failure-mode-2 (training-update lag) is the pattern where the change is rolled out and the training module is scheduled for the next quarterly refresh; the affected seats operate for a quarter without updated training. The plan's discipline is that major changes carry a dedicated pre-effective-date module; the runbook's step 4 gate is that the module must be authored before the effective date, not after. If the training team cannot meet this cadence, the rollout is deferred until it can — not the other way around.

The exception process is the plan's escape valve. Without exceptions, in-flight work is forced to abandon and restart against the new shape, throughput collapses, and first-line adoption becomes a compliance chore. Chapter 05's default windows (30 / 60 / 90 / 180 days) are calibrated for a mid-enterprise programme; the exception-register's audit flag (exceptions open past window without documented escalation) is what catches the second failure mode of exceptions: the "temporary" exception that becomes permanent. Track and close.

Emergency changes are ratified retroactively at the next standing council meeting. Chapter 05's failure-mode-3 (silent-emergency backlog) is where the change was resolved and the retroactive ratification does not happen because it no longer feels urgent. The plan's discipline is that emergency changes are a mandatory standing agenda item; the head-of-AI-governance does not have discretion on whether they appear. State this explicitly and pin it to invariant 4 of chapter 01's council charter (every decision produces a minuted decision of record).

For the bank scenario, the state-law change composes with the existing MRM discipline. The rollout must not create a parallel training track that duplicates MRM-side updates; the training-module inventory names the composition explicitly (which cohort receives the AI-governance module, which receives the MRM update, which receives both).

For the SaaS vendor scenario, the customer-facing communication is a distinct artefact class. Chapter 04's customer audience shape applies; the rollout's communications set includes the customer-notification (both to enterprise-deployer customers and to any downstream data subjects where applicable) with the required lead time (typically 30 or 60 days per contract — cite with `<!-- needs-research -->`).

For the healthcare scenario, the FDA PCCP update affects both the shipped-product surface and the internal-use surface. The rollout's communications set includes the FDA-facing update (notification of the PCCP composition change if it materially alters the change-control shape), the clinical-safety-committee brief, and the internal clinician training. Chapter 05's third-line-facing training discipline applies — the internal audit lead is consulted at step 1.

## Acceptance criteria

- [ ] Chosen scenario is stated at the top of `change-management-plan-v1.0.yaml`; every artefact is coherent against it.
- [ ] `change-management-plan-v1.0.yaml` includes metadata, one block per class (small / standard / major / emergency) with cadence, communications shape, training-update discipline, exception window (in days), and ratification body; references playbook, exception-process, training composition, exception-window calibration expectation, emergency authority; invariants block with test fields for all six chapter-05 invariants.
- [ ] `seven-step-playbook-runbook.md` covers all seven steps with owner, artefact, and gate criteria per step; step 3 references chapter 04's audience map; step 4 references mod-105 chapter 06 competence programme.
- [ ] `exception-process-template.md` includes exception-request template, exception-register schema (with audit flag), and approval-authority table (architect for standard; head for major; council for material-extension major).
- [ ] `worked-rollout-timeline-<change-id>.md` includes header, seven-step timeline with dated milestones, communications-set inventory keyed to chapter 04's audience map, training-module inventory keyed to mod-105 chapter 06, adoption metrics with cadence and escalation triggers, and closure milestone.
- [ ] `post-rollout-review-template.md` includes change summary, adoption outcome, what-went-well and what-did-not sections, lessons for future rollouts, and council or ARB minute reference.
- [ ] Every design choice is pinnable to a chapter-05 invariant (I1–I6) as enforcer or a chapter-05 failure mode (publish-and-pray; training-lag; silent-emergency backlog) as defence; a `pinning:` block or footnote in the plan YAML makes this explicit.
- [ ] Every unverified specific — state-law citations, EU AI Act article numbers, harmonised-standard identifiers, FDA guidance titles, contract-notification lead times — carries `<!-- needs-research -->` rather than a guessed value.

## Stretch goals

- **Emergency-rollout walkthrough.** Take a plausible emergency scenario (a serious-incident-driven policy withdrawal, a regulator's cease-and-desist letter, a material third-party posture change) and walk it through the compressed emergency path — head-of-AI-governance's bounded emergency authority, just-in-time briefing, immediate-effective-date discipline, retroactive council ratification. State how the seven-step playbook composes in emergency mode.
- **Manager-cascade briefing generator.** Author a template the head-of-AI-governance uses to generate manager-cascade briefings for a major change — a one-page brief the enterprise's people-manager cohort delivers to their teams. The template composes with the CHRO's manager-briefing conventions.
- **Adoption-monitoring dashboard mock-up.** Design the dashboard the head-of-AI-governance consults for step 6. Metrics, thresholds, escalation triggers, drilldowns. The dashboard is a mod-111 GRC-for-AI-platform artefact; state the platform-side implementation the dashboard requires (query patterns, event sources, refresh cadence).
