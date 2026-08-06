# Change-management — rolling policy and standard changes across the organisation

## Why this chapter exists

The mod-103 policy taxonomy names the enterprise's Responsible-AI principles, binding policies, standards, procedures, and work instructions. Every one of them changes. The EU AI Act publishes a delegated act (`<!-- needs-research: identify current delegated / implementing acts published under Regulation (EU) 2024/1689 relevant to the enterprise's control library at authoring date -->`) and the mod-104 jurisdiction-reconciled control set increments; the mod-102 control library adds a new family for a threat class the enterprise had not previously covered; a mod-107 assurance-architecture invariant is reinterpreted after a certification body finding; the mod-105 AIMS scope narrows or broadens after an acquisition. Each of these is an enterprise change with downstream effects on daily work.

The failure mode this chapter designs against is the one where the architect writes the new policy, the head-of-AI-governance ratifies it at the AI governance council, the policy is published to the intranet, and then within a fortnight the second-line finds that the first-line teams are still operating under the old policy. Nobody is defying the enterprise; the change did not reach them in a form they could act on. Six months later the certification body samples for the new policy's operation and finds a gap between the ratification date and the actual first-line adoption; the audit finding is systemic.

The architectural correction is a **change-management plan for policy and standard rollouts** — a first-class artefact that composes with the mod-103 policy taxonomy, the mod-105 competence programme, the mod-107 assurance architecture's ongoing cadence, and the chapter-04 communications architecture. This chapter designs the plan.

The plan applies to policy changes, standard changes, procedure changes, mod-102 control-library increments, mod-104 jurisdiction-reconciled control-set increments, mod-105 AIMS scope changes, mod-107 assurance-architecture changes, mod-109 third-party-programme changes, and mod-111 reference-architecture changes. It does not apply to individual gate decisions, incident-response actions, or ARB technical-decision minutes — those are operational artefacts with their own routing.

## The change classes — small, standard, major, emergency

The plan opens by classifying the change. Four classes; each has a different cadence, communications shape, training-update discipline, and exception window.

### Small change

**Definition.** A change to a work instruction, a template, a checklist item, or a mod-108 evidence-artefact schema field that does not modify the enterprise's substantive obligation. Example: adding a field to the model-card schema, updating a checklist item to reflect a tool-name change, tightening a formatting rule.

**Cadence.** Continuous. The change is made when needed and takes effect immediately on publication.

**Communications shape.** Change note on the artefact itself (change-log entry), summary line in the weekly programme newsletter, GRC-for-AI platform notification to affected seats.

**Training update.** None required. The updated artefact carries the new shape and self-teaches through use.

**Exception window.** 30 days: existing in-flight work products may complete against the prior version; new work products use the new version from publication.

### Standard change

**Definition.** A change to a procedure, a mod-102 control's implementation guidance, a mod-108 evidence-artefact schema (structural), or a mod-107 pre-deployment-gate charter element that modifies how work is done but not what obligation is being discharged. Example: adding a residual-close-to-appetite challenge question to the gate charter, revising the mod-108 fairness-evidence lineage requirement, updating the AIA composition template.

**Cadence.** Monthly. Standard changes accumulate into a monthly rollout cycle authored by the head-of-AI-governance and the architect, ratified at the ARB (chapter 04), and communicated on a fixed calendar.

**Communications shape.** Monthly programme newsletter dedicated segment; targeted communication to affected first-line seats; GRC-for-AI platform notification; direct engagement with the ai-evaluation-engineer and ai-risk-engineer seats whose day-to-day work changes.

**Training update.** The mod-105 competence programme's monthly refresh cycle absorbs the change; the affected training modules are updated within the same cycle. Analysts and engineers whose seats are affected complete the updated module within 30 days.

**Exception window.** 60 days: existing in-flight gate decisions and AIA reviews may complete against the prior shape; new work products use the new shape from the rollout date.

### Major change

**Definition.** A change to a policy in the mod-103 policy taxonomy, a mod-102 control family (addition, retirement, or material scope change), a mod-104 jurisdiction-reconciled control-set version increment, a mod-105 AIMS scope statement, a mod-107 assurance-architecture element, a mod-109 third-party-programme threshold, a mod-111 reference-architecture invariant, or an operating-model seat inventory (chapter 02) change.

**Cadence.** Quarterly by default; more often only when the underlying obligation cannot wait for the quarterly cycle. Major changes are ratified by the AI governance council under the reserved-matters process (chapter 01).

**Communications shape.** Quarterly major-change bulletin authored by the head-of-AI-governance, reviewed by the architect for technical accuracy, distributed to the enterprise via multiple channels — the intranet, the manager cascade, the engineering all-hands, the second-line functional meetings, the third-line internal-audit annual-plan intake. Each affected seat receives a personal notification with the specific work products the change affects.

**Training update.** The mod-105 competence programme's quarterly major-refresh cycle absorbs the change. Affected seats complete the updated training within 90 days; the training completion becomes a mod-102 compliance-with-training attestation the second-line samples.

**Exception window.** 90 days by default; extended to 180 days for changes affecting mod-102 control implementation where the control's implementation-lead-time is documented in the mod-102 evidence contract (mod-108) as exceeding 90 days. Existing in-flight programme-level artefacts (AIMS scope statement, jurisdiction-reconciled control set, assurance architecture) may complete their current cycle against the prior shape; the new shape becomes effective at the cycle boundary.

### Emergency change

**Definition.** A change required by an incident (mod-110), a regulatory action (a competent authority letter, a sector regulator's cease-and-desist, an enforcement action), or a material third-party posture change (mod-109) that cannot wait for the standard or major cycle. Example: withdrawing a policy after a serious-incident-driven root-cause-analysis; tightening a control after a certification-body finding; suspending a third-party provider after a material posture change.

**Cadence.** As required. Emergency changes are ratified by the AI governance council under its ad-hoc-meeting provision (chapter 01) or, where the change cannot wait even for an ad-hoc meeting, by the head-of-AI-governance under a defined emergency-authority the charter reserves (chapter 08). Emergency changes ratified under head authority are ratified retroactively by the council at the next standing meeting.

**Communications shape.** Immediate broadcast — the head-of-AI-governance's all-hands communication, direct notification to affected seats, GRC-for-AI platform alert, and (where the change affects customer-facing behaviour) customer notification per the customer-facing communications shape (chapter 04).

**Training update.** Just-in-time — the affected seats receive a briefing at the point of the change; the mod-105 competence programme absorbs the change into the next scheduled cycle.

**Exception window.** None; the change is effective immediately. In-flight work products continue under close supervision and may be required to pause pending the change's absorption.

## The rollout playbook — the seven steps

Every non-emergency change traverses the same seven-step rollout playbook. The steps are the same for standard, major, and (with compression) emergency changes; the depth of each step varies by class.

**Step 1 — proposal and technical draft.** The originating seat (typically the architect, sometimes the head-of-AI-governance, sometimes a second-line specialist) drafts the change proposal. The proposal names the change class, the artefacts it affects, the seats it affects, the rationale, and the recommended effective date.

**Step 2 — ARB or council ratification.** Standard changes ratified at the ARB; major changes ratified at the council. The ratification minute references the proposal version and records any conditions attached to the change (exception windows, training completion by named date, monitoring the change for defined intervals).

**Step 3 — communications drafting.** The head-of-AI-governance (with the communications function for major changes) drafts the multi-audience communications set: the programme newsletter segment, the manager-cascade briefing, the engineering all-hands segment, the affected-seat personal notifications, the customer-facing communication where relevant, and the regulator-facing communication where relevant. Chapter 04's communications architecture applies at every step.

**Step 4 — training update.** The mod-105 competence programme's owner (typically the head-of-AI-governance or a delegated seat) updates the affected training modules and schedules the completion cadence. For major changes, the training update is a distinct artefact with its own version and its own completion tracking.

**Step 5 — rollout communication and effective date.** On the agreed rollout date, the communications set is issued through the planned channels. The change becomes effective; the exception window begins. The GRC-for-AI platform (mod-111) is updated to reflect the new shape (version pins, workflow updates, evidence-artefact schema updates).

**Step 6 — adoption monitoring.** The head-of-AI-governance and the architect monitor adoption over the exception window. Metrics — new work products using the new shape, training completions, exception requests received, in-flight adaptations. Where adoption is not on trajectory, the head triggers additional engagement (targeted communication to slow-adopting teams, additional training sessions, direct outreach to first-line management).

**Step 7 — closure and post-rollout review.** At the end of the exception window, the head-of-AI-governance closes the change formally: the change is fully effective, in-flight-work-product completions under the prior shape are counted, exception requests are resolved. A post-rollout review is authored — what went well, what did not, what the plan should absorb for future rollouts. The review is minuted at the council (or at the ARB for standard changes) and referenced from the change record.

## The exception process — how existing work handles the transition

A change to a policy, standard, or control family means that work already in flight must decide whether to complete against the prior shape or the new shape. The exception process is what makes that decision predictable.

**Default rule.** Work in flight at the effective date completes against the prior shape until the exception window closes; new work started after the effective date uses the new shape.

**Exception request.** Where a first-line team wants to complete in-flight work against the *new* shape (typically because the change strengthens the enterprise's posture and the team wants to be first in), or where a first-line team wants to extend the exception window beyond the default for material reasons, an exception request is filed. The request states the work product, the desired disposition, the reason, and the second-line contact.

**Exception approval.** For standard changes, exceptions are approved by the architect at the ARB. For major changes, exceptions are approved by the head-of-AI-governance; where a major-change exception would delay a mod-102 control implementation beyond 180 days or would affect a jurisdiction-reconciled control-set obligation, the exception escalates to the council.

**Exception tracking.** All exceptions are logged in the GRC-for-AI platform (mod-111) as first-class artefacts with the change identifier, the affected work product, the exception disposition, the approver, and the closure date. Exceptions that remain open past the exception window without a documented escalation are red-flagged for the internal audit function.

## The training-update discipline

The mod-105 chapter 06 competence programme is the enterprise's authoritative training track for AI-governance roles. The change-management plan composes with the competence programme so that policy and standard changes are absorbed into training without breaking the programme's own cadence.

**Standard changes** absorb into the monthly training-content refresh. The refresh's authoring process — architect drafts, head reviews, competence-programme owner publishes — is the same as for the change itself, and the refresh cycle is deliberately paced to allow inline absorption.

**Major changes** carry their own dedicated training module that is authored *before* the effective date. The module is published on the rollout date; affected seats complete it within 90 days; the completion is tracked in the mod-102 compliance-with-training attestation the second-line samples during the exception window.

**Emergency changes** trigger a just-in-time briefing on the day of the change and a formal training module absorbed at the next scheduled major cycle.

**Third-line-facing training.** The internal audit function's competence must keep pace with the change. Where a major change affects the assurance architecture, the internal audit lead is consulted during the change proposal (step 1), attends the ratification, receives an audit-scope briefing on the change, and updates the annual audit plan's sampling shape.

## The exception-window calibration — the trade-off the plan makes visible

The exception window is a trade-off between adoption speed and disruption tolerance. Too short: in-flight work must abandon its current shape and restart against the new one, throughput collapses, first-line adoption of the change becomes a compliance chore. Too long: the enterprise operates under two shapes for a prolonged period, second-line seats have to run reviews against both shapes, the certification body finds the divergence at sampling.

The default windows (30 / 60 / 90 / 180 days per class and material-implementation-lead-time exception) are calibrated for a mid-enterprise programme with quarterly-cadence major-change accumulation. Enterprises should recalibrate against their own historical adoption data. Where the enterprise finds that major-change adoption reliably completes within 45 days, the window can shorten; where implementations reliably take 120 days, the window should lengthen. The calibration is council-ratified (as a chapter-01 reserved-matters charter amendment); the default windows are the starting point, not the answer.

## The change-management-plan schematic

```yaml
change_management_plan:
  id: CHANGE-MGMT-PLAN-v1.0
  ratified_by: ai-governance-council
  composes_with:
    - mod-103 policy taxonomy
    - mod-105 chapter 06 competence programme
    - mod-107 assurance architecture ongoing cadence
    - mod-112 ch 04 communications architecture
    - mod-111 grc-for-ai platform workflow layer

  change_classes:
    small:
      cadence: continuous
      communications: change-log + newsletter line + platform notification
      training_update: none
      exception_window: 30 days
      ratification: originating seat + peer review
    standard:
      cadence: monthly
      communications: newsletter dedicated segment + targeted communication
      training_update: monthly competence-refresh cycle
      exception_window: 60 days
      ratification: arb
    major:
      cadence: quarterly (default; more often only when obligation cannot wait)
      communications: quarterly major-change bulletin + multi-channel cascade + affected-seat personal notification
      training_update: dedicated module; 90-day completion
      exception_window: 90 days (default); 180 days where mod-108 implementation-lead-time documents it
      ratification: ai-governance-council (reserved matter)
    emergency:
      cadence: as required
      communications: immediate all-hands + platform alert + customer / regulator where relevant
      training_update: just-in-time briefing + absorption at next major cycle
      exception_window: none (immediate)
      ratification: council ad-hoc or head-of-ai-governance emergency authority (retroactive council ratification at next standing meeting)

  seven_step_playbook:
    1_proposal: originating seat drafts proposal (class, artefacts, seats, rationale, effective date)
    2_ratification: arb (standard) or council (major)
    3_communications_drafting: multi-audience set per mod-112 ch 04
    4_training_update: mod-105 ch 06 competence programme absorbs
    5_rollout: communications issued; change effective; exception window opens; grc-for-ai platform updated
    6_adoption_monitoring: head-of-ai-governance + architect monitor over the exception window
    7_closure: post-rollout review; council or arb minute

  exception_process:
    filed_via: grc-for-ai platform request form
    approver: architect (arb; standard); head-of-ai-governance (major); council (major with 180d+ material extension)
    tracking: platform artefact with change-id, work-product, disposition, approver, closure date
    audit_flag: exceptions open past window without documented escalation

  training_update_composition:
    standard: monthly refresh cycle
    major: dedicated module; pre-effective-date authoring; 90-day completion
    emergency: just-in-time briefing + absorption at next major
    third_line_facing: internal-audit lead consulted at step 1; audit plan updated

  invariants:
    - id: I1
      description: every change has a named class
      test: sample changes; find the class recorded on each
    - id: I2
      description: every non-emergency change traverses the seven-step playbook
      test: sample changes; find each step's artefact
    - id: I3
      description: every change with training implications updates mod-105 competence programme
      test: sample major changes; find the associated training module
    - id: I4
      description: exceptions are tracked and closed
      test: sample exceptions; find closure or escalation record
    - id: I5
      description: post-rollout reviews are conducted
      test: sample rollouts; find the review minute
    - id: I6
      description: exception-window defaults are calibrated against enterprise adoption data
      test: check for periodic council-ratified recalibration
```

## Three failure modes to design against

**Failure mode 1 — the "publish and pray" rollout.** The change is ratified, the intranet page is updated, and the enterprise is expected to comply from the effective date. No newsletter segment, no training update, no adoption monitoring, no exception process. Adoption is variable; the certification body finds the variance at sampling. The defence is the seven-step playbook — every non-emergency change traverses the same steps; publish-and-pray is not one of them.

**Failure mode 2 — the "training update lags by a quarter" adoption gap.** The change is rolled out; the training is scheduled for the next quarterly refresh; the affected seats operate for a quarter without the updated training. In-flight work products drift from the change's intent. The defence is invariant 3 — major changes carry a dedicated training module that is authored *before* the effective date and completed by affected seats within the exception window.

**Failure mode 3 — the "silent emergency" backlog.** Emergency changes are made under head authority (chapter 08), but the council retroactive ratification does not happen at the next standing meeting because the change was resolved and no longer urgent. Emergency changes accumulate outside the council's minuted record. The defence is that emergency changes have a mandatory retroactive-ratification agenda item at the next council meeting; the head-of-AI-governance does not have discretion on this.

## Coordination — the roles the plan interfaces with

- **AI governance council (chapter 01)** — ratifies major changes as reserved matters and retroactively ratifies emergency changes.
- **Architecture-review-board (chapter 04)** — ratifies standard changes and small-change technical questions.
- **Head-of-AI-governance (level 60)** — owns the rollout communications, the training-update composition, the adoption monitoring, and the closure minute.
- **Head of communications** — carries the enterprise-wide communication for major changes.
- **CHRO / people function** — owns the manager cascade and the change-management support for affected seats.
- **Mod-105 competence programme owner** — carries the training-update composition.
- **Mod-111 GRC-for-AI platform owner** — updates the platform's workflow and evidence-artefact schemas for each change.
- **Internal audit lead** — consulted on major changes affecting the assurance architecture; updates the audit plan.

## Summary

The change-management plan composes the mod-103 policy taxonomy, mod-105 competence programme, mod-107 assurance architecture cadence, mod-111 workflow layer, and the mod-112 chapter 04 communications architecture into a first-class artefact that governs how policy and standard changes reach the enterprise. Four change classes — small, standard, major, emergency — carry different cadences (continuous / monthly / quarterly / as-required), communications shapes (change-log line / newsletter segment / multi-channel cascade / all-hands broadcast), training-update disciplines (none / refresh cycle / dedicated module / just-in-time), and exception windows (30 / 60 / 90 / 180 default with immediate for emergency). Every non-emergency change traverses the seven-step playbook — proposal, ratification, communications drafting, training update, rollout, adoption monitoring, closure — and the exception process tracks in-flight work through the exception window in the GRC-for-AI platform. Six invariants hold (named class, seven-step traversal, training absorption, exception tracking, post-rollout review, exception-window calibration) and three failure modes recur (publish-and-pray, training-update lag, silent-emergency backlog). Chapter 04's communications architecture supplies the audience-shaping the rollout draws on; chapter 01's council charter supplies the reserved-matter path major changes are ratified through; chapter 08's boundary discussion supplies the head-of-AI-governance's emergency authority. The plan is the artefact exercise-05 authors and defends.
