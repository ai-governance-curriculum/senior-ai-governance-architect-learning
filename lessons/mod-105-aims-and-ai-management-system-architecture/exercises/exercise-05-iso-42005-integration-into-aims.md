# exercise-05: ISO/IEC 42005 integration into the AIMS

**Estimated effort:** 2 hours

## Objective

Wire **ISO/IEC 42005 (AI system impact assessment)** into the AIMS as a *first-class input* to the risk register and the risk-treatment plan — not as a parallel process that produces filed-and-forgotten AIAs. Deliver the AIA process specification (trigger, authoring, review, output shape, wire-up), the AIA record schema, an end-to-end worked AIA for one Halden system, and the compositional trace showing the AIA's outputs landing in the risk register and the RTP as concrete linked entries.

The failure this drill teaches you to prevent is chapter 04 failure mode 1 — the AIA that lives beside the risk register instead of feeding it. Enterprises stand up an AIA process because 42001 Clause 6.1.4 requires one; AIAs get authored, reviewed, filed; and the risk register, populated from a separate risk workshop, never reads them. The two artefacts drift and a stage-2 auditor finds the seam in an hour. The wire-up in this exercise is what prevents that.

This is the artefact the level-50 architect designs (the process; the schemas; the wire-up). The AI-risk engineer (level 25) at each system authors AIAs. The risk-and-impact review committee (chaired by the head of AI governance) reviews and ratifies them. The wire-up runs through the mod-111 GRC-for-AI platform's data model.

## Prerequisites

- Chapter [`04-risk-and-impact-assessment-composition.md`](../04-risk-and-impact-assessment-composition.md) read and internalised — this exercise implements what that chapter designs.
- Chapter [`06-risk-treatment-plan-and-operational-clauses.md`](../06-risk-treatment-plan-and-operational-clauses.md) skimmed — the RTP entries that AIAs surface land in the shape from chapter 06.
- Exercise [`exercise-03-risk-treatment-plan-shape-drill.md`](exercise-03-risk-treatment-plan-shape-drill.md) completed — some AIA-surfaced RTP entries in this exercise reference the RTP shape you authored there.
- mod-103 chapter 06 (IEEE 7000 as concept-of-operations methodology) skimmed — IEEE 7000's ethics-review output is the upstream input for the AIA's initial impact identification.
- Primary references: ISO/IEC 42005:2025 (AI system impact assessment); ISO/IEC 42001:2023 Clause 6.1.4; ISO/IEC 23894:2023 for the AI-risk taxonomy. Paywalled — verify structural specifics against the published standards.

## Scenario

Continue with **Halden Insurance Group** from exercises 01-04. Assume:

- The AIMS scope statement, SoA, RTP, and IMS integration design are approved (exercises 01-04).
- The risk-and-impact review committee is stood up, chaired by the head of AI governance (J. Amiri), with membership from legal, security, privacy, ethics (delegated to the CRCO's office in the absence of a dedicated ethics function), and the CROs of the P&C and life business units.
- Halden's incident-management process (single integrated per exercise-04) is stood up.
- Two AIAs have been authored under an interim process: AIA-2026-034 (claims-triage assistant) and AIA-2026-041 (broker-facing conversational assistant). This exercise brings the *AIA process itself* under formal specification and demonstrates the wire-up with one worked new AIA for a third system.
- The **worked AIA** for this exercise is for the **pricing and reserving model family** (specifically, the new life-book pricing model going into production next quarter). This system is high-risk under EU AI Act Annex III (insurance-life-and-health), Solvency-II-overlay, and materially affects individuals whose policies are priced by it.

## Deliverables

1. **`aia-process-specification.md`** — the two-page process specification (trigger, authoring, review, output shape, wire-up, cadence).
2. **`aia-record-schema.yaml`** — the canonical AIA record schema.
3. **`aia-2026-052-pricing-model.yaml`** — the fully populated AIA for the life-book pricing model.
4. **`wire-up-trace.md`** — the one-page trace showing AIA impacts → risk register entries → RTP entries → SoA row references, with the specific ids from the seed data you produce.
5. **`aia-failure-mode-defence-brief.md`** — a one-page brief pre-answering the two failure modes from chapter 04.

## Requirements

### `aia-process-specification.md`

Two pages. Must specify:

- **The three triggers** (per chapter 04): (a) new AI system in scope of the AIMS being considered for development or deployment; (b) existing in-scope AI system undergoes a material change; (c) an event occurs that changes the impact context (incident, regulatory change, discovered emergent behaviour, foundation-model change). For each trigger, name the specific event-classes at Halden (a life-book product launch; a foundation-model version upgrade the vendor announces; a Solvency II supervisory letter; an incident classified above threshold).
- **The material-change definition.** The definition Halden uses to decide when a material change re-fires the AIA. State the enumerated criteria (new use case; new user population or affected-party population; model architecture change beyond a threshold; training-data source change; new deployment jurisdiction; model performance regression beyond a threshold). The definition is a governance artefact — imprecise definitions produce inconsistent AIA firing.
- **The authoring assignment.** The AI-risk engineer (level 25) who owns the system authors the AIA. Contributors: product management (business context), legal (regulatory context), AI-evaluation engineer (level 35, performance and known limitations), data steward (data provenance and consent basis), affected-population representatives where reasonably obtainable. The head of AI governance is a *reviewer*, not the author — this separation-of-duties matters.
- **The review process.** The risk-and-impact review committee reviews the AIA. The three outcomes: accept; accept-with-conditions (specific conditions the entry names, tracked as RTP entries); revise-and-resubmit; escalate to the AI-accountable executive. Cadence: the committee meets at least monthly plus event-driven; AIAs enter the queue at the AI-risk engineer's submission.
- **The output shape.** The AIA record schema (deliverable 2 below).
- **The wire-up to the risk register.** Every material impact in the AIA produces at least one risk register entry with `source: aia` and `source_ref: <aia-id>`. Reciprocally, the risk register's schema includes an `aia_links` field populated at ingest.
- **The wire-up to the RTP.** Every proposed mitigation in the AIA is authored as an RTP entry (or attaches to an existing RTP entry) at the time of committee acceptance. RTP entries carry `aia_links` (per exercise-03 schema).
- **The wire-up to the SoA.** Where an AIA-surfaced risk requires a control the enterprise does not yet have, the SoA's implementation status for the relevant Annex A row changes from `in-place` to `partially-in-place` and the RTP entry that closes it is linked in the SoA row.
- **The refresh cadence.** Annual refresh minimum per system; event-driven refresh on material change; a portfolio-wide AIA review at the annual management review to detect systemic patterns.
- **The interface to the operational incident process.** Incidents above threshold trigger an AIA re-review; the re-review either updates or supersedes the existing AIA.
- **The documented-information retention.** AIAs are retained for the operational life of the system plus a defined post-decommissioning retention window (state the window; align with the ISMS documented-information retention rules).
- **The role of IEEE 7000 upstream.** Where the enterprise has adopted IEEE 7000 for concept-of-operations ethics review (mod-103 chapter 06), the 7000 output is a *first input* to the AIA at authoring. Do not duplicate; reference.

### `aia-record-schema.yaml`

Extend the chapter-04 sketch into a complete schema. At minimum:

- `aia_id` (format: `AIA-YYYY-nnn`).
- `system_id` (points at the AIMS system register — the system record shape from exercise-04 handling).
- `system_name`.
- `version_of_system_under_assessment` (a specific version the AIA is bound to).
- `assessment_type` (enumerated: `pre-deployment` | `material-change` | `event-triggered` | `scheduled-refresh`).
- `trigger_ref` (points at the event that fired the AIA — a change record; an incident id; a regulatory-change id; the launch decision id).
- `authored_by_role` (level-25 AI-risk engineer).
- `authored_by_person`.
- `contributors` (list of `role: person` pairs — legal, product, evaluation, data steward, affected-population representative).
- `authored_at`.
- `review_committee`.
- `reviewed_at`.
- `review_outcome` (enumerated: `accepted` | `accepted-with-conditions` | `revise-and-resubmit` | `escalated`).
- `conditions_of_acceptance` (list — required when outcome is accepted-with-conditions; each condition traceable to an RTP entry).
- `escalation_target` (role/person — required when escalated).
- `system_description` (structured: purpose, deployment context, autonomy level, human oversight shape, decision consequentiality).
- `affected_parties` (list — individuals, groups, non-user affected persons, environment, society).
- `foreseeable_impacts` (list — each with `id: IMP-<n>`, `category`, `directness`, `severity`, `likelihood`, `reversibility`, `scale`, `mitigations_proposed`, `residual_after_mitigation_estimated`).
- `positive_impacts` (list — the impact assessment considers positive as well as negative).
- `assumptions_and_uncertainty` (paragraph — where the assessment is confident, where it is uncertain, where the assessment defers to post-deployment measurement).
- `stakeholder_input_evidence` (paragraph — where affected-party representatives were consulted; where the enterprise did not consult and why).
- `risks_registered` (list of `AI-RISK-YYYY-nnn` ids the AIA produced or updated).
- `risk_treatment_plan_links` (list of `RTP-YYYY-Qn-nnn` ids).
- `soa_touchpoints` (list of Annex A control refs the AIA's mitigations affect).
- `regulatory_obligation_refs` (list — the specific regulatory obligations the AIA's assessment discharges, e.g. EU AI Act Article 27 FRIA for deployer-side high-risk systems, GDPR Article 35 DPIA where personal data is processed).
- `next_review_trigger` (list — the events that will re-fire the AIA; the scheduled refresh date).
- `retention_class` (per the documented-information retention rules).
- `change_log` (list — creation, review outcome, subsequent revisions).

For each field, state whether required or optional; for enumerated fields, enumerate the current allowed values.

### `aia-2026-052-pricing-model.yaml`

Produce a fully populated AIA for the life-book pricing model. Every schema field populated (no `TBD`s). Cover, at minimum:

- **System description** — purpose (life-book pricing for term-life and whole-life products for EEA policyholders); deployment context (Halden Deutschland GmbH and Halden ASA primary book; brokers submit applications through the standard channel); autonomy (assistive — the model produces a price recommendation and a rationale; the underwriter approves or overrides for above-threshold applications; below-threshold applications may auto-approve subject to policy limits); human oversight (per-application underwriter review above a defined premium threshold; sample-based review below); consequentiality (materially affects individuals' access to life insurance and its price).
- **Affected parties** — applicants (with attention to protected-characteristic subgroups where legally analysable), existing policyholders (indirect through cross-subsidy dynamics), beneficiaries, non-user affected persons (dependants whose life cover is priced), brokers as intermediaries, regulators (EIOPA supervisory-convergence expectations; national supervisor).
- **Foreseeable impacts** — at least five, drawn from the domain: (i) discriminatory outcomes on a legally analysable protected characteristic (e.g. by proxy); (ii) opacity of the price rationale to the applicant and the broker; (iii) systematic under-pricing or over-pricing due to model drift as morbidity assumptions age; (iv) exclusion of hard-to-model risk profiles; (v) over-reliance by underwriters on the model recommendation; (vi) potential for the model to encode historic underwriting patterns that were themselves subject to bias. Each impact carries directness, severity, likelihood, reversibility, scale, and mitigations proposed.
- **Positive impacts** — at least two — e.g. more consistent underwriting across brokers; earlier detection of anti-selection.
- **Assumptions and uncertainty** — where the assessment is confident (the deployment context and human-oversight shape); where uncertain (the actual bias profile of the deployed model on the deployed population; the interaction with Solvency II standard-formula capital calculations); where deferred to post-deployment measurement (the actual override rate and its distribution across underwriters).
- **Stakeholder input evidence** — what consultation was undertaken (typical for Halden: workers' council consultation on adjacent processes; broker forum feedback on transparency artefacts; consumer-representative body outreach if arranged); where consultation was not undertaken and why (typical: individual policyholders are consulted through the standard transparency notice, not per-AIA).
- **Risks registered** — at least three risk register entries created or updated, with realistic `AI-RISK-*` ids.
- **RTP links** — at least three RTP entries in the exercise-03 shape (state their ids).
- **SoA touchpoints** — at least three Annex A control refs (from exercise-02) the AIA's mitigations touch.
- **Regulatory obligation refs** — EU AI Act Article 9 (risk-management system for high-risk providers), Article 27 (FRIA for deployer where deployer role applies), Article 26 (deployer obligations more broadly), GDPR Article 35 (DPIA), Solvency II model-governance expectation (`<!-- needs-research: cite the specific Solvency II Delegated Regulation article for internal-model governance if internal model is used, or the standard-formula assumptions calculation for standard-formula use -->`).

### `wire-up-trace.md`

One page. Walk the wire-up in narrative form. Structure:

- Start with one foreseeable impact from the AIA (pick IMP-01 or another material one).
- Show the risk-register entry it produced or updated (`AI-RISK-YYYY-nnn`, with the risk statement and the fields the wire-up populates — `source: aia`, `source_ref: AIA-2026-052`).
- Show the RTP entry that addresses it (`RTP-YYYY-Qn-nnn`, with `aia_links: [AIA-2026-052]` and `risk_register_links: [AI-RISK-YYYY-nnn]`).
- Show the SoA row(s) the RTP entry closes (the Annex A ref, the pre-RTP status, the post-RTP status).
- Show the operational control library entry (`AIC-*`) the RTP implements.
- Show the evidence artefact (a monitoring report; a documented decision; an audit-trail record) that closes the RTP entry.

Repeat abbreviated for a second impact so the reader sees the pattern hold.

Then in one paragraph, state how the wire-up is *inspectable* — how the head of AI governance runs a query in the GRC platform (mod-111) to demonstrate to the auditor that every AIA-surfaced impact is linked to a risk-register entry, an RTP entry, and closure evidence. State what the query returns for a wire-up-complete AIA and what it returns for an AIA that has drifted.

### `aia-failure-mode-defence-brief.md`

One page. Pre-answer the two failure modes from chapter 04:

- **Failure mode 1 — the AIA that lives beside the risk register instead of feeding it.** State the mechanism that prevents it at Halden — the mandatory wire-up fields (`aia_links` on risk register and RTP; `risks_registered` and `risk_treatment_plan_links` on the AIA), the GRC-platform ingest-time validation, and the risk-and-impact review committee's acceptance gate (the committee cannot accept an AIA without the wire-up ids populated). Include the query the head of AI governance runs monthly to catch drift.
- **Failure mode 2 — collapsing 23894 into "we did a risk assessment".** State the mechanism that prevents it — the AIA schema's requirement that each `foreseeable_impacts` entry name the 23894 risk source(s) it aligns with (add this field to your schema if it is not already there under `treatment_23894_source_ref` or equivalent), and the review-committee checklist that verifies the 23894 taxonomy has actually been walked at authoring time.

Close with the two most likely auditor questions about the AIA process and the pre-written two-sentence answers to each.

## Starter guidance

- **The AIA is an *engineering artefact with legal and ethical content*, not a marketing document.** Precise about impacts, honest about uncertainty, explicit about mitigations, traceable to the risk register and RTP. Chapter-04 language.
- **Do not defer *all* impacts to "post-deployment measurement".** The AIA is done *before* deployment (for pre-deployment triggers) precisely to catch impacts that are still remediable at design time. State what you are deferring, what you are not, and why.
- **Positive impacts belong in the AIA.** 42005 asks for foreseeable impacts, positive as well as negative. If your AIA only enumerates harms, you have produced a risk assessment, not an impact assessment.
- **Stakeholder input is not "we sent an email".** State what consultation actually happened, what it produced, and — where you did not consult — the reasoned basis for the non-consultation. Auditors read this section.
- **The wire-up is where the composition becomes real.** The ids in `risks_registered`, `risk_treatment_plan_links`, and `soa_touchpoints` are the composition. Populate them, or the AIA is drifting from the AIMS.
- **The material-change definition is a *governance artefact*.** Imprecise definitions produce inconsistent AIA firing. Enumerate the criteria; be prepared to defend the thresholds.
- **Use `<!-- needs-research: ... -->` freely for regulatory specifics** — Solvency II article numbers, EU AI Act FRIA scope specifics, DPIA-vs-AIA delineation questions.

## Acceptance criteria

- [ ] The process specification names all three triggers, the material-change definition, the authoring assignment (with the reviewer-not-author separation), the review process (with the four outcomes), the wire-up to the risk register / RTP / SoA, the refresh cadence, the incident interface, the retention rule, and the IEEE 7000 upstream reference.
- [ ] The AIA record schema names every required and optional field, enumerates every closed enum, and includes the wire-up fields (`risks_registered`, `risk_treatment_plan_links`, `soa_touchpoints`, `regulatory_obligation_refs`).
- [ ] The pricing-model AIA is fully populated (no `TBD`s), includes at least five foreseeable-impact entries with reversibility / scale / mitigations, and includes at least two positive impacts.
- [ ] The pricing-model AIA populates the wire-up fields with realistic ids; the RTP entries referenced compose with the exercise-03 shape; the SoA rows referenced compose with the exercise-02 shape.
- [ ] The wire-up trace walks one impact end-to-end (AIA impact → risk register entry → RTP entry → SoA row → control library entry → evidence artefact) and abbreviates for a second impact so the pattern is visible.
- [ ] The wire-up trace names the GRC-platform query the head of AI governance runs to catch AIA drift.
- [ ] The defence brief pre-answers both failure modes with a specific enforcement mechanism for each, including the 23894 taxonomy walk enforcement.
- [ ] All regulatory citations are verifiable against `../resources.md` or marked `<!-- needs-research: ... -->`.

## Stretch goals

- **Author a *material-change decision tree*** — the one-page decision tree the AI-risk engineer walks when a change lands to decide whether an AIA re-fire is required. Include the three failure modes (over-fire noise, under-fire drift, and the manual-override path when the tree does not apply).
- **Sketch the *portfolio-wide AIA review*** — the annual review the head of AI governance conducts across all AIAs to detect systemic patterns (recurring impact categories; recurring mitigation gaps; recurring failure of a control-library entry to close AIA-surfaced risks). Half a page.
- **Design the *DPIA-and-AIA join*** — the one-page brief on how Halden avoids duplicating a GDPR Article 35 DPIA and an AIA for the same system. State what is shared, what is DPIA-specific, what is AIA-specific, and how the two records reference each other.
- **Handle the *foundation-model-version-change trigger*** — the specific procedure for the claims-triage assistant when the third-party foundation-model provider announces a version upgrade. Which AIA fires, on what timeline, with what committee attention. Half a page.
- **Add an *AIA-effectiveness measure*** — the field the schema does not yet carry that captures, at the next annual refresh, whether the AIA's foreseeable-impact predictions came true or not. State how the measure is populated and how negative results (impacts materialised that the AIA missed, or that the AIA over-weighted) feed the AIA process improvement.
