# exercise-06: Assurance Owner Contracts With Evaluation Engineer (and Analyst)

**Estimated effort:** 2 hours

## Objective

Author the **two coordination contracts** the assurance architecture depends on for execution — the peer-to-peer coordination contract with the level-35 `ai-evaluation-engineer` who owns release-assurance methodology, and the role-scope contract for the level-15 `ai-governance-analyst` who does the day-to-day evidence collection, gate preparation, and re-assessment preparation that make the whole system operate.

The exercise composes with all five preceding exercises. The evaluation-engineer contract determines whether the pre-deployment gate (exercise-02), the ongoing programme's drift triggers (exercise-03), and the third-line audit's evaluation-workpaper samples (exercise-04) actually execute at the depth the architecture requires. The analyst role-scope contract determines whether the evidence contract (mod-108-shaped) is actually discharged for every gate meeting, whether re-assessment packages actually assemble, and whether external-audit package construction (exercise-05) is a rehearsed process or a scramble. Failure to author either contract cleanly is the "second line without methodology depth" or "second line without capacity" pathology chapter 06 names.

## Prerequisites

- Chapter [`06-coordination-contracts-with-evaluation-and-analyst.md`](../06-coordination-contracts-with-evaluation-and-analyst.md) read once, with the three-round procedure, the analyst's primary outputs, the interface between the two roles, and the two failure modes marked.
- Exercises 01, 02, 03, 04, 05 in this module complete (or the artefacts from those exercises available in a form you can reference). This exercise instantiates the coordination interfaces those earlier artefacts assume.
- The role scope brief for `ai-evaluation-engineer` in the peer track (level 35) and for `ai-governance-analyst` in this track (level 15). If unavailable, use the responsibilities enumerated in chapter 06 as the working reference.
- Access to primary references — IIA Three Lines Model (2020) for the peer coordination shape (the second line is not a hierarchy; peer coordination is contract-shaped); SR 11-7 for the validation-vs-development independence discipline the evaluation engineer operates under; ISO/IEC 42001 Clause 7 (Support: resources, competence, awareness, communication, documented information) for the analyst's competence-and-documented-information anchor. See [`../resources.md`](../resources.md).

## Scenario

Use the same scenario you chose in exercise-01 (US regional bank, global healthcare payer/provider, or B2B SaaS HR-tech vendor). Restate the scenario at the top of the deliverable. The scenario determines the *scale* of both contracts:

- The bank scenario typically has an established second-line function with dedicated evaluation-engineer and analyst capacity; the coordination is between well-resourced peers.
- The healthcare scenario typically has thinner second-line evaluation capacity and a large clinical-safety-oversight-committee overlap; the coordination has to specify how the evaluation engineer interfaces with the committee.
- The B2B SaaS scenario typically has a very small governance function; the analyst may cover functions a larger enterprise would split across two seats, and the evaluation engineer may be shared across product lines.

State the starting-position resourcing explicitly. The contracts must be executable at the resourcing level you state, not at a wished-for level.

## Deliverables

Author three artefacts in a working directory of your choice.

1. **`evaluation-engineer-coordination-contract.md`** — the peer-to-peer coordination contract with the level-35 evaluation engineer, including the three-round procedure specification, the shared-ownership scope, the reciprocal boundaries, and the standing-forum cadence.
2. **`analyst-role-scope-contract.yaml`** — the role-scope contract for the level-15 analyst, in the schematic shape chapter 06 fixed (primary outputs with input / output / quality bar / cadence / escalation per output; competences required; competences the analyst does *not* hold; supervision and growth path).
3. **`coordination-worked-scenarios.md`** — three worked scenarios that stress-test the two contracts against realistic conflict cases: (a) a proposal-response cycle between architect and evaluation engineer where the evaluation engineer disputes an architecture-shaped assumption in the proposal; (b) an analyst escalation where an evidence artefact required for a gate meeting cannot be obtained from the first-line owner in time; (c) an interface case where the analyst discovers an evaluation-related gap the evaluation engineer has not been reachable on.

## Requirements

### `evaluation-engineer-coordination-contract.md`

Decide and justify **each** of the following:

- **Parties and reporting relationship.** Named seats on both sides. Reporting-relationship discipline (both are second-line; the architect is level 50 and the evaluation engineer is level 35, but the reporting relationship is peer-to-peer through the second-line function, not manager-to-report). Confirm the architect does *not* task the evaluation engineer directly.
- **Scope of shared ownership.** The specific architectural artefacts both roles co-own: the gate charter's evaluation-related sections (evidence-contract tier-3 and tier-4 overlays, sampling quotas); the ongoing-assurance drift-trigger schema and specific-trigger registration; the red-team engagement contract shape; the reproducibility discipline for evaluation artefacts; the third-line audit-artefact contract sections that sample evaluation work; the calibration methodology outputs feeding ongoing-assurance re-assessment triggers.
- **The three-round procedure.** Round 1 (architect drafts architecture proposal with methodology-shaped questions deferred), Round 2 (evaluation engineer marks the proposal with methodology answers and architectural-assumption disputes), Round 3 (reconciliation; shared final; both sign). Specify concrete timing (typical round durations, expected round-1-to-final elapsed for routine proposals, escalation window if a round stalls). Specify the escalation destination when reconciliation cannot be reached (typically the head of AI governance for resource allocation or architecture adjustment).
- **The architect's boundaries.** The architect does *not*: choose the specific evaluations to run per system; design the eval-set curation and versioning; select the calibration statistic; conduct evaluation interviews or negotiate the red-team engagement's scope; integrate external evaluation methodology. These are evaluation-engineer decisions; the architect designs the *contract* they land in.
- **The evaluation engineer's boundaries.** The evaluation engineer does *not*: design the gate's shape or the gate charter's non-evaluation sections; author the assurance architecture's versioning and change-control; engage directly with external audit providers (that is the audit-liaison seat; the evaluation engineer supports); author the risk taxonomy or the appetite architecture.
- **Standing forums.** The monthly architect-evaluation-engineer sync (methodology roadmap; upcoming gate complexities; drift-trigger tuning; audit-workpaper preparation). The quarterly joint session with the head of AI governance and the risk engineer (portfolio review; taxonomy amendments; calibration outputs). Specify who chairs, what artefacts are produced, and what the escalation pathway from each forum looks like.
- **The signing and versioning discipline.** The contract is versioned (semver-shaped or comparable); both roles sign; the head of AI governance ratifies major-version changes. Contract updates flow into the assurance architecture change log per chapter 01 invariant 6.
- **Composition with the MRM function (bank scenario only).** For the bank scenario, the evaluation engineer's role overlaps with the SR 11-7 MRM validator function. Specify the interface — a single evaluation function under a shared reporting line; a distinct MRM function with a coordination contract to the evaluation engineer; or a hybrid with named overlap. Chapter 01 exercise-01 argued this in the three-lines architecture; instantiate the interface here.
- **Composition with the clinical-safety oversight committee (healthcare scenario only).** For the healthcare scenario, the clinical-safety oversight committee's post-launch review is not the ongoing-assurance function itself, but it produces safety findings the evaluation engineer must integrate into the release-assurance methodology. Specify the interface.
- **Composition with the product-team-embedded evaluation seat (B2B SaaS scenario only).** For the B2B SaaS scenario, product-teams often embed their own evaluation capacity (a first-line "evaluation lead" inside product). Specify how the peer-level evaluation engineer (second-line) interfaces with the product-embedded seats (first-line) without collapsing the two lines.

### `analyst-role-scope-contract.yaml`

Instantiate chapter 06's role-scope contract schematic for your scenario. The contract has the following top-level fields; every field must be populated.

```yaml
role_scope_contract_analyst:
  version: <e.g. 1.0.0>
  role: ai-governance-analyst (level 15)
  reports_to: <head-of-ai-governance seat>
  authored_by: senior-ai-governance-architect (level 50)
  ratified_by: <head-of-ai-governance>
  primary_outputs:
    - evidence_collection_and_filing:
        input: <e.g. mod-108 evidence contract per applicable SoA row>
        output: <e.g. evidence artefacts filed in AIMS documented-information register>
        quality_bar: <freshness / traceability / reproducibility per contract>
        cadence: <continuous>
        escalation: <where the escalation goes when evidence is unavailable, missing,
                     or stale beyond contract>
    - aims_documented_information: {...}
    - gate_preparation: {...}
    - re_assessment_preparation: {...}
    - external_audit_package_support: {...}
    - capa_administrative_tracking: {...}
  competences_required: [<enumerated>]
  competences_the_analyst_does_not_hold: [<enumerated, with clear delegation>]
  supervision_and_growth:
    technical_supervision: <seat>
    architectural_direction: <architect through contracts and checklists>
    growth_path: <analyst → senior analyst → risk-engineer or evaluation-engineer>
  escalation_discipline:
    - trigger: <e.g. evidence gap analyst cannot close by prompting first-line owner>
      destination: <e.g. second-line chair for gate escalation route>
      time_bound: <e.g. before gate convene decision>
```

Each of the six primary outputs must be populated at full depth against the shape chapter 06 fixed. The competences-not-held list must include at least: deep evaluation methodology (delegated to the evaluation engineer), risk-scoring judgement (delegated to the risk engineer), authorship of the assurance architecture (delegated to the architect), regulatory-posture judgement calls (delegated to legal + head of AI governance).

Add scenario-specific outputs where warranted — for the healthcare scenario, an output for clinical-safety-oversight-committee coordination (packaging the committee's findings into the AIMS documented information); for the B2B SaaS scenario, an output for customer-facing assurance-report support (packaging the enterprise's AI-scope evidence into the SOC 2 auditor's engagement package).

### `coordination-worked-scenarios.md`

Three scenarios that stress-test the contracts. Each scenario is a short case (about half a page) with a set-up, a walk-through, and a resolution.

- **Scenario 1 — Architect proposes an unrealistic sampling quota.** The architect drafts an ongoing-assurance drift-trigger schema section that specifies "the evaluation engineer independently reproduces 100% of first-line evaluation runs at every gate meeting." The evaluation engineer marks the proposal disputing the sampling-quota assumption; team capacity supports at most 15% reproduction at the current gate throughput. Walk the three-round procedure to reconciliation: either the sampling quota is reduced to 15% with a compensating discipline elsewhere, or the head of AI governance escalates to the AI-accountable executive for additional evaluation-engineer capacity. Show the shared-final artefact both parties sign.
- **Scenario 2 — Analyst cannot close an evidence gap before a gate meeting.** A tier-3 system is scheduled for gate meeting on Thursday. On Monday's completeness check, the analyst finds the model card is at version 0.9, not 1.0, and the data-provenance record is stale after last month's data-pipeline change. The analyst prompts the first-line owner Monday; response by Wednesday is that the update requires a data-engineering sprint and cannot land by Thursday. Walk the escalation through the role-scope contract: to the second-line chair for gate-convene decision; the chair either defers the gate meeting or opens gate-escalation route "evidence-contract failure" from exercise-02 chapter 02. Show the artefact the analyst produces (a completeness-check-outcome record naming the specific gaps and the artefacts affected).
- **Scenario 3 — Analyst discovers an evaluation-related gap the evaluation engineer has not been reachable on.** During gate preparation, the analyst files the eval set version and the seed as required, but discovers the eval set is not in the shared eval-set registry; the analyst cannot verify the version. The evaluation engineer is out of office on a red-team engagement and unreachable within the gate window. Walk the interface discipline from chapter 06 — the escalation crosses the boundary via the second-line chair, not directly from analyst to evaluation engineer. Show the second-line chair's action (peer-to-peer request to the evaluation engineer's designate, or gate-defer with reason). Confirm the analyst has not attempted evaluation-methodology-depth verification (that would be the analyst-as-fallback failure mode).

Each scenario must end with a short paragraph naming the *contract clause the scenario tests* and confirming that the clause survives the stress or requires the amendment.

## Starter guidance

- Draft the evaluation-engineer coordination contract *before* the analyst role-scope contract. The evaluation engineer's shared-ownership scope shapes what the analyst's outputs actually contain (what gets filed, what reproducibility fields the analyst carries).
- The three-round procedure is what makes the peer coordination survive resource pressure. Without the procedure, methodology decisions drift toward whichever side pushed harder at the moment; with the procedure, methodology decisions are traceable to a proposal-response-reconciliation record both roles signed. Do not shorten it below three rounds for material sections.
- The reciprocal boundaries clause is where under-specification bites. If the architect drifts into methodology decisions the evaluation engineer's authorial signature is subordinated; if the evaluation engineer drifts into architecture decisions the assurance system loses its authorship coherence. Name the boundaries explicitly and enforce them at contract review.
- The analyst role-scope contract's *competences the analyst does not hold* list is where the analyst's authority is bounded. Analyst-tier verification is filing and traceability; methodology-depth verification is evaluation-engineer-tier. The analyst-as-fallback failure mode (chapter 06) starts when the "not held" list is soft; make it firm.
- The escalation discipline is what makes the analyst effective under time pressure. Every escalation destination must have a named seat and a time bound; open-ended escalations back up in the analyst's queue and eventually degrade to omission.
- For the bank scenario, the composition with the MRM function is the most consequential design choice; the two typical patterns (single evaluation function absorbing AI MRM; distinct MRM function with a peer-to-peer contract to the evaluation function) each carry different signature and reporting implications; commit and defend.
- For the healthcare scenario, the clinical-safety oversight committee interface will be the site of the most friction; the committee's authority under the enterprise's clinical-governance discipline is separate from the AI-assurance discipline, but the committee's findings must feed the AI-assurance architecture. Specify the interface as a filing pathway.
- For the B2B SaaS scenario, the product-embedded evaluation seat interface is where the first-line-vs-second-line boundary is most porous; the peer evaluation engineer must resist the pull to become embedded in each product team's release process (which would collapse to first-line). The contract must reserve the peer evaluation engineer's second-line-authoritative role.

## Acceptance criteria

- [ ] Scenario is stated at the top with starting-position resourcing; both contracts are executable at the stated resourcing.
- [ ] `evaluation-engineer-coordination-contract.md` decides all nine requirements bullets (eight generic plus one scenario-specific composition), each with stated rationale.
- [ ] Parties and reporting relationship are correctly specified as peer-to-peer through the second-line function.
- [ ] Scope of shared ownership enumerates all the architectural artefacts chapter 06 pins.
- [ ] The three-round procedure specifies timing, artefact form per round, escalation window, and escalation destination.
- [ ] Reciprocal boundaries are explicit on both sides (architect boundaries and evaluation-engineer boundaries).
- [ ] Standing-forum cadence covers monthly and quarterly forums with chairs, artefacts, and escalation pathways.
- [ ] Signing and versioning discipline is stated; changes flow into the assurance architecture change log.
- [ ] Composition with MRM (bank), clinical-safety oversight (healthcare), or product-embedded evaluation seat (B2B SaaS) is specified.
- [ ] `analyst-role-scope-contract.yaml` populates all six primary outputs at full depth (input, output, quality bar, cadence, escalation each).
- [ ] Competences-not-held list explicitly delegates evaluation methodology, risk-scoring judgement, architecture authorship, and regulatory-posture judgement to their respective seats.
- [ ] Escalation discipline names a seat and a time bound for every escalation destination.
- [ ] Scenario-specific outputs (clinical-safety coordination for healthcare; customer-facing assurance-report support for B2B SaaS) are included where warranted.
- [ ] All three worked scenarios walk the three-round procedure or the analyst-escalation discipline concretely; each ends with the contract clause tested and the outcome (survives / requires amendment).
- [ ] Every unverified citation to IIA guidance, SR 11-7 provisions, ISO/IEC 42001 clauses, or scenario-specific regulations is marked `<!-- needs-research: ... -->` — no invented section numbers.

## Stretch goals

- Add a *contract-review cadence* — the annual joint review the architect, evaluation engineer, head of AI governance, and analyst run on the two contracts together, checking whether the boundaries and outputs still match the enterprise's operating reality; what triggers a major-version amendment.
- Sketch the *career-continuity move* — the analyst's growth path from analyst to risk-engineer or evaluation-engineer over 2 to 4 years, with the milestone certifications, rotations, and mentorship pattern. Preview of the mod-112 operating-model chapter's role-ladder discipline.
- Extend the coordination-worked-scenarios document with a *fourth scenario* — a scenario where the architect and the evaluation engineer reach the head of AI governance for escalation and the head cannot reconcile; the AI-accountable executive is engaged. Walk the interface up to the executive.
- Add a *cross-role RACI* — a role-vs-artefact matrix showing R/A/C/I across the architect, evaluation engineer, analyst, risk engineer, head of AI governance, and audit-liaison seat for at least ten artefacts from this module (gate charter, drift trigger, incident-driven re-assessment record, third-line engagement plan, external-audit package, remediation plan, ...). The matrix reveals whether the contracts cohere or overlap.
- Compose the evaluation-engineer coordination contract with the *NIST SP 800-37 Assess step's SP 800-53A analog* framing from chapter 07 — the evaluation engineer's methodology is the AI-specific analog to the assessment procedures in SP 800-53A; where the enterprise's assurance system claims RMF-composition to a federal-facing customer, the evaluation engineer's methodology is where the composition materialises.
