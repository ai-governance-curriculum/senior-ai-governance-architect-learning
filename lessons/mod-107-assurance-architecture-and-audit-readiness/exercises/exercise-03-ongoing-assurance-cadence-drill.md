# exercise-03: Ongoing Assurance Cadence Drill

**Estimated effort:** 3 hours

## Objective

Author the **ongoing assurance programme charter and the trigger registry** for a specified enterprise scenario. The pre-deployment gate (exercise-02) makes an authoritative launch decision at a point in time; the ongoing assurance programme is the second-line-owned machinery that keeps that decision fresh — periodic re-assessment on a tiered cadence, drift-driven re-assessment on quantitative thresholds, incident-driven re-assessment on a taxonomy of classifications, and regulatory-change re-assessment on horizon-scan outputs.

The deliverable is the programme charter, a per-system trigger registry with quantitative thresholds pre-registered, one worked re-assessment record for each of the four trigger types, and an operations-calendar diagram anchored to the AIMS operations calendar. Downstream exercises (exercise-04 third-line audit programme, exercise-05 external interface, exercise-06 coordination contracts) all sample or depend on the artefacts you author here — an internal audit engagement of the ongoing programme (exercise-04) samples the trigger registry directly.

## Prerequisites

- Chapter [`03-ongoing-assurance-cadence-and-triggers.md`](../03-ongoing-assurance-cadence-and-triggers.md) read once, with the four trigger types and the two failure modes marked.
- Exercise-01 (three-lines architecture) and exercise-02 (pre-deployment gate charter) in this module complete, or your scenario architecture and gate charter available in a form you can reference. The trigger registry cross-references the gate charter's tier scheme, the pre-deployment decision-record ids, and the risk register entries pinned in exercise-01.
- Chapter [`06-coordination-contracts-with-evaluation-and-analyst.md`](../06-coordination-contracts-with-evaluation-and-analyst.md) skimmed for the analyst's re-assessment-preparation checklist boundary and the evaluation engineer's role in registering specific drift triggers per system.
- The mod-106 chapter on the risk register write-back discipline and the tolerance table, so the "write-back into the risk register" section of your charter composes with the risk-register schema you have already committed to.
- The mod-105 chapter on the AIMS operations calendar (chapter 08) so your operations-calendar diagram lands inside the existing cadence rather than parallel to it.
- The mod-110 chapter on post-market monitoring (skim to boundary depth) so your charter's "consumes monitoring outputs" section is honest about what mod-110 produces vs. what ongoing assurance derives.
- Access to primary references — Regulation (EU) 2024/1689 Article 72 (post-market monitoring) and Article 73 (serious-incident reporting), NIST AI RMF Generative AI Profile (NIST AI 600-1) MANAGE function, SR 11-7 on ongoing model validation, FDA guidance on the predetermined change-control plan for AI/ML-enabled devices where applicable to your scenario, Colorado AI Act (SB 24-205) risk-management-programme obligations where applicable. See [`../resources.md`](../resources.md).

## Scenario

Use the same scenario you chose in exercise-01 (US regional bank, global healthcare payer/provider, or B2B SaaS HR-tech vendor). Restate the scenario at the top of the deliverable so the exercise is self-contained. Where exercise-01 produced a per-system schematic for at least eight systems, the trigger registry in this exercise instantiates triggers for those same systems.

## Deliverables

Author four artefacts in a working directory of your choice.

1. **`ongoing-assurance-charter.md`** — the programme charter.
2. **`trigger-registry.yaml`** — the machine-readable per-system trigger registry with quantitative thresholds pre-registered.
3. **`worked-reassessment-records/`** — a directory with one YAML file per trigger type: `periodic.yaml`, `drift.yaml`, `incident.yaml`, `regulatory-change.yaml`. Each is a coherent instance of the re-assessment record for a specific system in your scenario.
4. **`operations-calendar.md`** — the operations-calendar diagram (Mermaid or ASCII) showing monthly / quarterly / semi-annual / annual cadences and the read/write points against the AIMS operations calendar, the pre-deployment gate, the risk register, and the audit committee reporting cycle.

## Requirements

### `ongoing-assurance-charter.md`

Decide and justify **each** of the following:

- **Programme ownership and interfaces.** The seat that operates the programme (typically the head of AI governance or a designated seat inside the governance office); the interfaces up (to the AI-accountable executive, to the audit committee) and across (to the evaluation engineer for drift-trigger registration, to the risk engineer for residual re-scoring, to the incident-management function per mod-110 for incident-trigger filing, to the legal-and-regulatory function for regulatory-change filing).
- **Periodic cadence per tier.** The four-tier cadence from chapter 03 defaults to annual / annual-with-mid-cycle / semi-annual / quarterly for tiers 1 through 4; adapt to your scenario if you defended a different tier scheme in exercise-02. State the *floor* (minimum cadence) and the conditions under which more frequent re-assessment is required (a system with a near-tolerance-boundary residual, a system in the first year post-launch, a system under active regulatory scrutiny).
- **Re-affirmation-record shape.** The lighter artefact a periodic re-assessment produces. Enumerate its fields against chapter 03's re-affirmation shape and against your gate charter's decision-record — the re-affirmation reads the launch decision-record and either re-affirms unchanged, re-affirms with conditions, or triggers a withdrawal.
- **Drift trigger design.** For at least three drift dimensions (model-output drift, data-input drift, performance drift on labelled ground truth, fairness drift on protected characteristics, adversarial-robustness drift, calibration drift), specify the metric family, the operational-alarm threshold (first-line responds), the assurance-trigger threshold (second-line responds), the convene-by window from trigger receipt, and the minimum re-assessment scope. Defend how the operational-vs-assurance separation is tuned to prevent both trigger-flood and periodic-only pathologies (chapter 03's two failure modes).
- **Incident classification-to-scope map.** The mapping from incident classification (minor / material / serious per chapter 03, adapted for your enterprise's incident taxonomy from mod-106 and mod-110) to the re-assessment scope. Include portfolio-consideration rules for material and serious incidents (which sibling systems in the portfolio get re-assessment consideration because they share controls, data provenance, or the same frontier-model base). Include the notification obligations that fire — AI Act Article 73 notification to the market surveillance authority for serious incidents where applicable; sector-specific notification per your scenario's regulatory posture.
- **Regulatory-change response.** The intake pathway from the horizon-scanning function (legal-and-regulatory) into the ongoing-assurance function; the change-type-to-scope map (interpretive / additive / substantive per chapter 03); the phased-response discipline (posture-alignment → progress-verification → readiness) and the migration-window commitments per change type. Include at least one worked change scenario the charter is designed to survive — a new state Act analogous to Colorado SB 24-205 taking effect in a state where you operate, a new FDA guidance revision on AI/ML SaMD where applicable, an EU AI Act implementing act clarifying an existing Article, or a sector regulator publishing new AI-specific supervisory guidance.
- **Write-back to the risk register and portfolio view.** For each of the four trigger types, specify what the re-assessment writes back into the risk register (mod-106) — residual score updates, category coverage adjustments, control-defeated scenario updates, stale-residual escalations. Specify how the portfolio view (mod-106 chapter 04) is kept current from the register.
- **Operations calendar.** The monthly / quarterly / semi-annual / annual cadences the programme runs, aligned to the AIMS operations calendar (mod-105 chapter 08). Specify the read/write points (which meeting produces which artefact for which consumer).
- **Composition with adjacent programmes.** Where the ongoing programme boundaries with post-market monitoring (mod-110), with the MRM function under SR 11-7 for banks, with the clinical-safety oversight committee for healthcare, with the enterprise-customer assurance-report cycle for B2B SaaS. Name the interfaces and specify what flows across each in each direction.
- **Non-scope.** At least three things you *chose not to* include in the charter and why. Candidates: a duplicated monitoring stack the second line runs alongside mod-110 (chapter 03 argues ongoing assurance consumes mod-110, does not replicate it); a "quarterly deep re-assessment for every system" cadence that would collapse under volume; a first-line-managed re-assessment for material incidents (chapter 01 invariant 1 failure).

### `trigger-registry.yaml`

Enumerate at least six systems from your scenario (subset of the exercise-01 registry). For each system, register:

```yaml
system:
  id: SYS-2027-0042
  name: <system name>
  tier: <tier from your enterprise scheme>
  launch_decision_ref: PDG-2027-04-0087
  periodic:
    cadence: <e.g. quarterly>
    next_scheduled: <YYYY-MM-DD>
    owner: <analyst seat that convenes the package>
    reviewer: <second-line reviewer seat>
  drift_triggers:
    - id: DT-2027-<system>-<dim>
      dimension: <e.g. disparate_impact_ratio_by_protected_class>
      metric_definition: <what is measured, how, from what data>
      operational_alarm:
        threshold: <quantitative>
        sustained_over: <window>
        responder: <first-line seat>
      assurance_trigger:
        threshold: <quantitative>
        sustained_over: <window>
        responder: <second-line seat>
        convene_by: <T+n business days>
        minimum_scope: <which risk-register entries re-scored; which evals re-run>
      registration_evidence:
        pre_deployment_gate_registration_date: <YYYY-MM-DD>
        registered_by: <evaluation-engineer seat>
        tuned_against_baseline: <baseline reference>
  incident_triggers:
    minor_classification_scope: <one-line summary of scope>
    material_classification_scope: <scope + portfolio-consideration seed>
    serious_classification_scope: <scope + external notification obligations>
  regulatory_change_watch:
    - regime: <e.g. Colorado AI Act SB 24-205>
      status: <e.g. rulemaking pending>
      change_type_anticipated: <interpretive / additive / substantive>
      response_lead_time_required: <weeks/months>
```

At least one system must have a drift-trigger where the operational-alarm and assurance-trigger thresholds are separated by a rationale you defend explicitly in a `rationale:` field (why this two-level separation, not a single threshold). At least one system must have a drift-trigger flagged as `tuning_review_pending` — a trigger you know is not yet well-tuned; the charter's operations calendar must include a tuning-review checkpoint that catches it.

### `worked-reassessment-records/`

One instance per trigger type. Each file is coherent — every field internally consistent, all cross-references pointing to sensible-shape identifiers.

- **`periodic.yaml`** — a re-affirmation record for a scheduled periodic re-assessment. Include the evidence-contract discharge state re-verified, residual scores re-read, control state re-checked, oversight and rollback re-checked, regulator-packaging currency re-checked. Include one *stale artefact* the re-affirmation catches (an evaluation report past its currency window, a data-provenance record not refreshed since a data-pipeline change) and specify the conditional-re-affirmation the record produces (fix condition, deadline, owner).
- **`drift.yaml`** — a drift-driven re-assessment record where the assurance-trigger fired. Include the trigger id, the measured-vs-threshold detail, the eval re-run outputs, the residual re-scoring against tolerance, and the decision (holds / conditions / withdrawal). If the decision is "withdrawal," walk the rollback contract and the notification obligations that fire.
- **`incident.yaml`** — an incident-driven re-assessment record for a material-classification incident. Include the incident-management cross-reference (mod-110 incident id), the affected-categories re-assessment, the portfolio-consideration sweep (which sibling systems were considered, which were re-assessed, which were cleared), the CAPA entries opened (root cause, corrective action, preventive action, effectiveness-review schedule per mod-105 chapter 09).
- **`regulatory-change.yaml`** — a regulatory-change re-assessment record for a substantive change type. Pick a change plausible in your scenario — a new Colorado AG rule under SB 24-205, an EU AI Act Article 6 implementing act, a new sector regulator supervisory letter — and walk the phased response through posture-alignment, progress-verification, and readiness for the effective date. Include at least one system whose launch decision has to be re-taken because the change is substantive enough to break the original evidence contract.

### `operations-calendar.md`

A Mermaid or ASCII diagram, plus a short prose description, showing:

- The monthly ongoing-assurance operational review (second-line internal); who convenes, who attends, what is decided.
- The quarterly readout at the AIMS operational review (mod-105 chapter 08).
- The semi-annual portfolio-view refresh to the audit committee.
- The annual programme review (is the cadence right? are the drift thresholds still tuned? does the trigger-flooded/periodic-only pattern show up in the data?).
- The read/write points from and to the risk register, the SoA, the RTP, the CAPA register, the audit committee reporting cycle, and the external-audit interface (chapter 05) for regimes with post-market-monitoring evidence obligations.

## Starter guidance

- Draft the charter *before* the trigger registry. The charter forces the trade-offs (cadence per tier, drift-threshold separation, incident-scope map, regulatory-change response); the registry instantiates them per system.
- The two failure modes chapter 03 names (periodic-only, trigger-flooded) are the diagnostic lens for the operational-alarm-vs-assurance-trigger separation. If your assurance thresholds fire for every alarm, you are in trigger-flooded territory; if they never fire, you are in periodic-only territory. The separation is tuned at drift-monitor registration time, per system, and re-tuned at the annual programme review.
- Do not let the trigger registry fall behind the AI inventory. A registry that carries six systems while the inventory carries forty is a chapter-01 invariant-3 failure showing up in the ongoing programme.
- The re-affirmation record is *lighter* than the pre-deployment decision-record, not vestigial. The re-affirmation still requires the analyst-tier evidence discharge, the risk engineer's residual re-read, and the second-line reviewer's signature. Do not degrade the re-affirmation into an email confirmation.
- The regulatory-change response is where enterprises most often improvise. Design the intake pathway from horizon-scan output to the ongoing-assurance function as a *filing* rather than a *meeting* — the legal-and-regulatory function files a regulatory-change record with change-type and effective-date; the ongoing-assurance function opens a re-assessment on receipt.
- For the bank scenario, the SR 11-7 ongoing-validation cadence *is* the periodic re-assessment for MRM-in-scope models; do not run ongoing assurance separately from MRM for these models. For the healthcare scenario, the clinical-safety oversight committee's post-launch review cadence is a first-line-or-second-line committee, not the ongoing-assurance function itself; specify the interface. For the B2B SaaS scenario, the customer-facing SOC 2 continuous-monitoring cadence overlaps with the ongoing-assurance periodic cadence; specify how the enterprise avoids running two parallel programmes with different results.
- For the drift dimensions, tune the thresholds against a *baseline* recorded at pre-deployment. A threshold picked without a baseline is a placeholder; a threshold cited in a real deliverable should reference the baseline artefact from the launch evidence contract.

## Acceptance criteria

- [ ] Scenario is stated at the top; charter and registry are coherent against it.
- [ ] `ongoing-assurance-charter.md` decides all ten requirements bullets, each with stated rationale.
- [ ] Periodic cadence per tier is specified with the floor conditions and the more-frequent-re-assessment conditions.
- [ ] At least three drift dimensions are specified with operational-alarm threshold, assurance-trigger threshold, convene-by window, and minimum scope.
- [ ] Incident classification-to-scope map covers minor / material / serious with portfolio-consideration rules for material and serious, and includes the notification obligations that fire.
- [ ] Regulatory-change response includes the intake pathway, the change-type-to-scope map, the phased-response discipline, and at least one worked change scenario.
- [ ] Write-back to risk register and portfolio view is specified per trigger type.
- [ ] Operations-calendar section aligns with the AIMS operations calendar and the audit committee reporting cycle.
- [ ] Composition with mod-110 monitoring, MRM (bank), clinical-safety oversight (healthcare), or customer-assurance cycle (B2B SaaS) is pinned.
- [ ] `trigger-registry.yaml` covers at least six systems from the exercise-01 registry; at least one has a defended two-level threshold separation; at least one is flagged for tuning-review.
- [ ] All four worked re-assessment records are internally coherent, cross-reference sensible-shape identifiers, and each carries at least one non-trivial feature (stale artefact caught on re-affirmation; withdrawal decision on drift; portfolio-consideration sweep on incident; launch-decision re-take on substantive regulatory change).
- [ ] `operations-calendar.md` diagram plus prose is readable and honest about read/write points.
- [ ] Non-scope section names at least three things deliberately excluded and why.
- [ ] Every unverified citation to AI Act articles, SR 11-7 provisions, FDA guidance, Colorado SB 24-205 rulemaking, NIST AI 600-1 sections, or sector-specific regimes is marked `<!-- needs-research: ... -->` — no invented article numbers, clause labels, or effective dates.

## Stretch goals

- Add a *trigger-flood diagnostic* the annual programme review would run — an aggregation over trigger-firing frequency by system and dimension over the year, with a rule-of-thumb signal for when tuning is required (e.g., an assurance-trigger firing more than N times per quarter without a substantive material finding likely signals over-tuning).
- Sketch the *re-authorisation* pathway for a system whose ongoing-assurance re-assessment produces a withdrawal — how does the system re-enter the deployment population? A shortened return-to-service gate? A full re-run of the pre-deployment gate? Specify.
- Add a *cross-programme correlation view* — for a small subset of your scenario's systems, overlay the ongoing-assurance re-assessments (this exercise), the third-line audit findings (previews exercise-04), and the external-audit findings (previews exercise-05). Where the three views disagree on a system's assurance state, name the pattern and propose a reconciliation.
- Extend the regulatory-change section with a *cross-jurisdiction reconciliation move* — where the same underlying system faces re-assessment triggers from more than one regime (Colorado SB 24-205 and NYC LL144 for an HR-tech B2B SaaS deployment; SR 11-7 and EU AI Act for a global bank system; FDA and state medical-board expectations for a US healthcare system). Show how you sequence the re-assessments to close the highest-consequence obligation first without duplicating work.
- Compose one drift trigger with a *NIST SP 800-37 continuous-monitoring* framing (chapter 07). Show how the same trigger reads as an ATO-side re-authorisation event when the enterprise's assurance system is documented against the RMF process shape for a federal-facing customer.
