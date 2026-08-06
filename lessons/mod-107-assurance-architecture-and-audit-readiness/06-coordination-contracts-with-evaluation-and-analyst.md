# The coordination contracts — with the evaluation engineer (level 35) and the governance analyst (level 15)

## Why this chapter exists

The assurance architecture the previous chapters designed is executable only when two specific role-interfaces are pinned. The first is with the peer-level `ai-evaluation-engineer` at level 35 — the role that owns the *release-assurance methodology* running inside the pre-deployment gate (chapter 02), the drift-driven re-assessment methodology inside the ongoing assurance programme (chapter 03), and the evaluation-methodology depth the internal audit function is expected to sample against (chapter 04). The second is with the `ai-governance-analyst` at level 15 — the role that collects the *analyst-tier evidence* the audit consumes (mod-108's evidence contract instantiated at the analyst level), files the AIMS documented information (mod-105 chapter 07), keeps the SoA current (mod-105 chapter 05), and does much of the day-to-day work of the second line that makes the gate and the ongoing programme actually run.

These two roles are on different levels and carry different competences, but they share the property that the level-50 architect *depends on them* without *managing them*. The evaluation engineer is a peer — coordinated through a written contract, not a reporting line. The analyst reports upward through the head of AI governance, not through the architect — coordinated through a role-scope contract that the architect authors and the head of AI governance ratifies. Chapter 06 walks both contracts. Getting them right is what turns the architecture from paper to practice; getting them wrong produces the "second line without capacity" or "second line without methodology depth" failure modes that neither the internal audit function (chapter 04) nor the certification body (chapter 05) will miss.

## The contract with the level-35 evaluation engineer

### What the evaluation engineer owns

The `ai-evaluation-engineer` role at peer level 35 sits in the second line as the *methodology owner* for AI-system evaluation. The role's responsibilities that this module depends on:

- **Release-assurance methodology design and execution.** The specific set of evaluations, the sampling approaches, the eval-set versioning discipline, and the reproducibility standards that a specific system is measured against at pre-deployment gate. The architect designed the *gate's shape* (chapter 02); the evaluation engineer designs the *methodology that runs inside review 2 (evidence-contract discharge) and provides methodological input to review 4 (residual against appetite)*.
- **Red-team engagement design.** For tier-3 and tier-4 systems, the red-team engagement's scope, methodology, and reporting shape. The evaluation engineer decides whether an engagement is internal or external, what threat scenarios are covered, and what the finding-severity taxonomy is. The architect pins that a red-team engagement is required for these tiers; the evaluation engineer decides what a red-team engagement *is*.
- **Independent reproduction of first-line evidence.** During gate review 2 and during drift-driven re-assessments, the evaluation engineer independently reproduces sampled first-line evaluation runs — the same eval set, the same version of the model, the same seed — and confirms the reported metric matches. This is where the "sampling quota" the gate charter specifies (chapter 02) is discharged operationally.
- **Calibration methodology (with the risk engineer at level 25).** Chapter 06 of mod-106 walked the calibration methodology the evaluation engineer owns for the risk-scoring practice. The mod-107 interface is that the calibration methodology's outputs feed the ongoing assurance programme (drift-in-calibration is a re-assessment trigger of its own).
- **External evaluation-methodology integration.** US AISI methodology publications, UK AISI publications, NIST AI 100-3 and related evaluation methodology, MLCommons AILuminate, Stanford CRFM's HELM — the evaluation engineer integrates external methodology into the enterprise's practice. The architect informs the integration (does the enterprise's assurance system compose with the external methodology?) but does not choose it.

### What the architect owns at the interface

The architect owns the *contract shape* the evaluation engineer's methodology drops into. Specifically:

- **The pre-deployment gate charter's evaluation-related sections.** The evidence contract's tier-3 and tier-4 overlays are the architect's design; the specific evaluations they require are the evaluation engineer's design.
- **The sampling quotas the gate operates against.** The architect writes "at least *k* percent of first-line evaluation runs are independently reproduced per gate meeting" into the charter; the evaluation engineer decides which runs and executes.
- **The ongoing-assurance drift-trigger schema.** The architect writes the schema for a drift trigger (chapter 03); the evaluation engineer registers specific triggers per system with specific thresholds.
- **The audit-artefact contract's evaluation-related sections.** For the third-line audit (chapter 04) to sample the evaluation engineer's work, the audit-artefact contract must specify what the evaluation engineer's workpapers look like, at what retention, with what immutability. The architect authors that; the evaluation engineer discharges.

### The three-round coordination

Coordination between the architect and the evaluation engineer runs on a three-round pattern the architect owns:

**Round 1 — architecture proposal.** The architect drafts a section of the assurance architecture that depends on evaluation methodology (a gate section, a drift schema, an audit-artefact contract section). The proposal is written to be executable from the evaluation engineer's chair — not "the evaluation engineer will run appropriate evaluations" but "the evaluation contract specifies the following about what evaluation coverage is expected." The proposal lands with a defined set of methodology-shaped questions the architect explicitly does not answer.

**Round 2 — methodology response.** The evaluation engineer reads the proposal, answers the methodology questions, marks any architecture-shaped assumptions the proposal makes that the evaluation engineer disputes, and returns the proposal marked. Common disputes: the sampling quota is unrealistic given team capacity; the drift threshold is set at a point where signal is drowned in noise; the reproducibility discipline is under-specified for the evaluations named.

**Round 3 — reconciliation and ratification.** The architect and the evaluation engineer meet, reconcile the marked proposal, and produce a shared final. Where the reconciliation cannot be reached (typically because of resource constraints — the methodology requires more capacity than the evaluation function has), the architect escalates to the head of AI governance for either resource allocation or architecture adjustment. The shared final is versioned into the assurance architecture; both roles sign.

The three rounds prevent the two failure modes: the architect designing evaluation-dependent architecture that the evaluation engineer cannot execute (fails at gate meetings), and the evaluation engineer designing methodology that cannot land in the architect's contract (fails at audit).

### A schematic — the evaluation-engineer coordination contract

```yaml
coordination_contract_evaluation_engineer:
  version: 1.4.0
  parties:
    - senior-ai-governance-architect (level 50)
    - ai-evaluation-engineer (peer, level 35)
  scope_of_shared_ownership:
    - pre-deployment gate evidence-contract tier-3 / tier-4 overlays
    - pre-deployment gate sampling quotas (evaluation-engineer verification depth)
    - ongoing-assurance drift-trigger schema and specific-trigger registration
    - red-team engagement contract shape
    - reproducibility discipline for evaluation artefacts
    - third-line audit-artefact contract sections that sample evaluation work
    - calibration methodology outputs feeding ongoing-assurance re-assessment triggers
  three_round_procedure:
    round_1: architect drafts architecture proposal with methodology-shaped questions
             explicitly deferred
    round_2: evaluation-engineer marks the proposal with methodology answers and
             architectural-assumption disputes
    round_3: reconciliation; shared final; both sign
    escalation: irreconcilable → head-of-ai-governance for resource or architecture
                adjustment
  boundary_the_architect_does_not_cross:
    - choosing the specific evaluations to run per system
    - designing the eval-set curation and versioning
    - selecting the calibration statistic
    - conducting the interviews or negotiating the red-team engagement's scope
    - integrating external evaluation methodology
  boundary_the_evaluation_engineer_does_not_cross:
    - designing the gate's shape or the gate charter's non-evaluation sections
    - authoring the assurance architecture's versioning and change-control
    - engaging directly with external audit providers (the audit-liaison seat does
      this; the evaluation-engineer supports)
    - authoring the risk taxonomy or the appetite architecture
  standing_forums:
    - monthly architect-evaluation-engineer sync (methodology roadmap; upcoming gate
      complexities; drift-trigger tuning; audit-workpaper preparation)
    - quarterly joint session with head-of-ai-governance and risk-engineer (portfolio
      review; taxonomy amendments; calibration outputs)
```

## The contract with the level-15 governance analyst

### What the analyst owns

The `ai-governance-analyst` role at level 15 is the *doing* seat of the second line — the role whose day-to-day work is what makes the AIMS operate. Responsibilities that this module depends on:

- **Evidence collection.** For every SoA row applicable to a system in scope, the analyst collects the evidence artefacts the mod-108 evidence contract requires, files them per retention discipline, and tracks their currency. The evaluation engineer's reproduction work sits on top of the analyst's collection work; the analyst's collection is what makes reproduction possible at all.
- **AIMS documented information housekeeping.** The AIMS documented-information register (mod-105 chapter 07) is the analyst's daily instrument. Every artefact the architecture references — the AI policy, the SoA, the RTP, the AIA register, the risk register, the CAPA register, the internal audit reports, the management-review minutes — is filed under a scheme the analyst maintains.
- **Pre-deployment gate preparation.** For each pre-deployment gate meeting, the analyst assembles the evidence package the six reviews walk through, cross-references artefacts, confirms freshness, surfaces gaps to the second-line chair. Chapter 02's "pre-gate gate" (the completeness check) is largely the analyst's work.
- **Ongoing-assurance re-affirmation preparation.** For periodic re-assessments, the analyst assembles the update package the second-line function reviews. For drift-driven and incident-driven re-assessments, the analyst assembles the record the re-assessment convenes on.
- **External-audit-package assembly.** Under the audit-liaison seat's direction, the analyst does much of the artefact-gathering that produces external-audit packages (chapter 05).
- **CAPA administrative tracking.** Every CAPA in the register has status, owner, timing, and effectiveness-review scheduling that the analyst maintains.

### What the architect owns at the interface

The architect *does not manage the analyst* — the analyst reports through the head of AI governance. The architect owns the *role-scope contract* the analyst operates against:

- **The evidence-contract specification (with mod-108 as the primary source).** What artefacts exist, what fields they have, how they are filed. The analyst instantiates the contract; the architect designs the contract shape.
- **The gate-preparation checklist.** The step-by-step procedure the analyst runs before every gate meeting: pull the applicable SoA rows; pull the evidence artefacts per row; check freshness; check traceability; check reproducibility; check independence-of-authorship; surface gaps to chair. The architect authors the checklist; the analyst runs it.
- **The re-assessment-preparation checklist.** Analogous to the gate checklist, tuned for periodic vs drift vs incident vs regulatory-change triggers.
- **The external-audit-package assembly guide.** The step-by-step procedure the analyst runs under the audit-liaison seat's direction. Cross-references the packaging discipline of chapter 05.
- **The documented-information register schema.** What is filed, in what shape, with what retention. The analyst maintains the register; the architect designs the schema.

### The role-scope contract

The role-scope contract for the analyst is *not* a job description in the HR sense — it is an assurance-architecture artefact that specifies what the analyst produces, at what quality, at what cadence, subject to what escalation. The head of AI governance ratifies (the analyst reports to the head); the architect authors.

```yaml
role_scope_contract_analyst:
  version: 2.1.0
  role: ai-governance-analyst (level 15)
  reports_to: head-of-ai-governance (level 60)
  authored_by: senior-ai-governance-architect (level 50)
  ratified_by: head-of-ai-governance
  primary_outputs:
    - evidence_collection_and_filing:
        input: mod-108 evidence contract per applicable SoA row
        output: evidence artefacts filed in AIMS documented-information register
        quality_bar: freshness per contract; traceability per contract; reproducibility
                     per contract (where applicable to analyst-tier)
        cadence: continuous
        escalation: if evidence unavailable, missing, or stale beyond contract → gate
                    completeness fail; ongoing-assurance stale-residual trigger
    - aims_documented_information:
        input: every artefact the AIMS references
        output: register current and correctly indexed
        quality_bar: no dangling references; version history complete; retention
                     discipline honoured
        cadence: continuous
        escalation: if register drifts → mod-105 chapter 07 nonconformity
    - gate_preparation:
        input: system under review; applicable SoA rows; evidence base
        output: gate evidence package; completeness-check outcome; gate calendar entry
        quality_bar: package complete on gate charter's evidence contract
        cadence: per-gate
        escalation: completeness fail → second-line chair does not convene gate;
                    escalation to head-of-ai-governance for resourcing to close gaps
    - re_assessment_preparation:
        input: trigger (periodic / drift / incident / regulatory-change)
        output: re-assessment record package; convene calendar entry
        quality_bar: package appropriate to trigger type per chapter 03
        cadence: per-trigger
        escalation: trigger not filed by ongoing-assurance function within window →
                    programme monthly-review finding
    - external_audit_package_support:
        input: engagement-scope-of-work from audit-liaison seat
        output: engagement package sections the analyst is scoped for
        quality_bar: chapter 05 packaging discipline
        cadence: per-engagement
        escalation: package fail → audit-liaison seat resolves before delivery
    - capa_administrative_tracking:
        input: CAPA register entries
        output: status current, owners current, timings current, effectiveness reviews
                scheduled
        quality_bar: no CAPA in the register without full metadata
        cadence: weekly review; monthly report to ongoing-assurance operational review
        escalation: overdue CAPA count above threshold → head-of-ai-governance
  competences_required:
    - literacy on the enterprise's control library (mod-102) and evidence contract
      (mod-108)
    - literacy on ISO/IEC 42001 clauses to the level of understanding where each
      analyst-owned artefact fits
    - literacy on the enterprise's applicable regulatory obligations (mod-104) to the
      level needed to file the right artefacts for the right systems
    - use of the GRC-for-AI platform (mod-111) if adopted
  competences_the_analyst_does_not_hold:
    - deep evaluation methodology (that is the evaluation engineer)
    - risk-scoring judgement (that is the risk engineer)
    - authorship of the assurance architecture (that is the architect)
    - regulatory-posture judgement calls (that is legal + head-of-ai-governance)
  supervision_and_growth:
    - technical supervision: head-of-ai-governance (or a designated senior seat)
    - architectural direction: architect (through contracts and checklists, not through
      day-to-day tasking)
    - growth path: analyst → senior analyst → risk-engineer or evaluation-engineer
      after methodology depth is developed
```

### The escalation discipline

The analyst's authority is bounded. Escalations run cleanly through the head of AI governance, with named-and-timed decision destinations for common escalations:

- **Evidence gap the analyst cannot close by prompting the first-line owner.** Escalate to the second-line chair for gate escalation route (chapter 02) or to the head of AI governance for AIMS-operations escalation.
- **Register drift the analyst cannot resolve within the routine housekeeping window.** Escalate to the head of AI governance for a housekeeping sprint or a documentation review.
- **Trigger not filed by the ongoing-assurance function within the trigger's window.** Escalate to the ongoing-assurance function's monthly operational review; if unresolved, to the head of AI governance.
- **External-audit-package gap the audit-liaison seat has not resolved.** Escalate to the audit-liaison seat first; if unresolved to the head of AI governance.

The architect *does not* receive these escalations directly — they route through the head of AI governance, who involves the architect where the escalation points at an architectural issue (a gate charter that is under-specified, a checklist that produces false negatives, a schema that has gaps).

## The interface between the evaluation engineer and the analyst

The two roles the chapter designs contracts for are not independent. The evaluation engineer's reproduction work depends on the analyst's collection work; the analyst's gate-preparation depends on the evaluation engineer's methodology being executable. Interface disciplines the architect specifies:

- **Analyst provides raw material; evaluation engineer verifies.** The analyst does not verify evaluation-methodology outputs (that is beyond analyst competence). The evaluation engineer does not collect and file (that is beyond evaluation-engineer bandwidth). The gate meeting depends on both.
- **Reproducibility artefacts are analyst-collected under evaluation-engineer specification.** The analyst files eval-set version, seed, model version, artefact hash — but the *contents* of the eval-set and the *choice* of seed are the evaluation engineer's.
- **Escalations cross the boundary via the second-line chair.** Where the analyst discovers an evaluation-related gap (an eval-set version not in the register) and cannot reach the evaluation engineer within the window, the escalation goes through the second-line chair to the evaluation engineer as a peer-to-peer request — not from analyst directly.

## Two failure modes to design against

**Failure mode 1 — the analyst-as-fallback.** The evaluation engineer is under-resourced; the analyst is asked to "just verify these evaluation results" for the gate meeting. The analyst does not have the methodology depth to challenge the results; the check is nominal; the gate meeting nevertheless treats the check as substantive. The certification body's stage-2 audit catches the pattern — evaluation-methodology depth was never resident where it should have been. The fix is *strict competence boundaries in the role-scope contract*: analyst-tier verification is filing and traceability; methodology-depth verification is evaluation-engineer-tier; if the evaluation engineer cannot execute at gate cadence, the fix is capacity, not delegation to the analyst.

**Failure mode 2 — the architect-as-manager.** The architect drifts into direct tasking of the analyst — "please pull these artefacts for me by Friday" — and into direct methodology negotiation with the evaluation engineer — "I need you to run this specific evaluation with these thresholds." Both drifts break the coordination shape. The head of AI governance loses visibility into the analyst's queue; the evaluation engineer's methodology decisions become architect-directed rather than methodology-owned. The fix is *architect-as-contract-author, not tasker*: the architect tasks through the checklist and the charter, not through direct assignment. The head of AI governance owns the analyst's day; the evaluation engineer owns the methodology.

## Summary

The assurance architecture depends on two coordination contracts the architect authors and maintains. With the peer-level `ai-evaluation-engineer` at level 35 the contract is peer-to-peer, run through a three-round proposal-response-reconciliation procedure that resolves at the head of AI governance where methodology and architecture cannot be reconciled; the boundaries are firm on both sides — the architect does not design methodology and the evaluation engineer does not design architecture. With the `ai-governance-analyst` at level 15 the contract is a role-scope artefact that specifies primary outputs, quality bars, cadences, escalations, and competences — authored by the architect, ratified by the head of AI governance, operated by the analyst through the head of AI governance's day-to-day supervision. The two roles interface at gate preparation, at reproducibility discipline, and at escalation routing; each's failure modes (analyst-as-fallback, architect-as-manager) are architectural and preventable. The next chapter walks the composition of NIST SP 800-37 RMF with the whole assurance system — the process-shape reference the enterprise's assurance architecture composes with.
