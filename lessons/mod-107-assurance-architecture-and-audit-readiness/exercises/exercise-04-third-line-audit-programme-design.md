# exercise-04: Third-Line Audit Programme Design

**Estimated effort:** 3 hours

## Objective

Author the **third-line internal audit programme's foundational artefacts** for a specified enterprise scenario — the multi-year audit programme, one year's audit plan, one engagement-scope-of-work, the audit-artefact contract, and the audit-committee reporting cadence. The architect does not run internal audit (the chief audit executive does, reporting to the audit committee) but *designs the shape the programme takes* so the audit function can operate against a defensible bar. This deliverable is the shape.

The exercise is the acid test of chapters 01, 02, 03, and 04 composed. If the multi-year programme does not sample every material area of your assurance architecture across the cycle, the three-lines architecture from exercise-01 is not being audited. If the engagement scope reads "audit the AIMS clauses" without naming outcomes, you are reproducing chapter 04's failure mode 1. If the artefact contract permits the second line to negotiate findings, you are reproducing failure mode 2.

## Prerequisites

- Chapter [`04-third-line-independent-audit-programme.md`](../04-third-line-independent-audit-programme.md) read once, with the three competence patterns, the six-phase engagement procedure, the audit-artefact contract, and the two failure modes marked.
- Exercises 01, 02, and 03 in this module complete (or your scenario architecture, gate charter, and ongoing programme available in a form you can reference). Every audit engagement in your plan samples something the earlier deliverables produced — the multi-year programme must be defensible against the exercise-01 architecture's material areas.
- Mod-105 chapter 08 (AIMS internal audit — Clause 9.2) walked once, so your programme composes with the second-line-adjacent internal-audit-of-the-AIMS you have already committed to; the third-line programme extends that audit into first-line evidence and second-line assurance-record sampling.
- Chapter [`05-external-audit-and-certification-interface.md`](../05-external-audit-and-certification-interface.md) skimmed for the composition point — the third-line programme's outputs *feed* the external audit and certification interface, and where the internal audit function's competence bar does not match the certification body's, the mismatch shows up in stage-2 findings.
- Access to primary references — IIA International Professional Practices Framework (IPPF) 2024 update, IIA Three Lines Model (2020), ISO/IEC 42006:2025 (title-level enough if paywalled), ForHumanity IAAIS (as versioned), BABL AI Algorithmic Bias Auditor curriculum, SR 11-7 on internal audit of MRM. See [`../resources.md`](../resources.md).

## Scenario

Use the same scenario you chose in exercise-01 (US regional bank, global healthcare payer/provider, or B2B SaaS HR-tech vendor). Restate the scenario at the top of the deliverable so the exercise is self-contained. The third line's shape depends on the scenario's internal audit function — the bank scenario has an established internal audit function with an existing SR 11-7-shaped MRM audit programme; the healthcare scenario has an internal audit function typically thinner on model-specific competence; the B2B SaaS scenario has a small internal audit function likely co-sourced with a Big Four provider. State the *starting position* explicitly.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`multi-year-audit-programme.md`** — the three-year audit programme, with the areas-to-sample and the cadence-per-area.
2. **`annual-audit-plan.yaml`** — the coming year's audit plan, with the engagements enumerated and the audit-committee-ratification metadata.
3. **`engagement-scope-of-work.md`** — the engagement-scope-of-work document for one specific engagement from the plan, taken at full depth.
4. **`audit-artefact-contract.yaml`** — the audit-artefact contract that specifies what artefacts every engagement produces, at what retention, with what immutability.
5. **`audit-committee-reporting-shape.md`** — the audit-committee reporting cadence, the deck shapes per cadence, and the escalation discipline for event-driven reports.

## Requirements

### `multi-year-audit-programme.md`

Decide and justify **each** of the following:

- **The material areas the programme samples across the three-year cycle.** Enumerate at minimum: the AIMS clauses (ISO/IEC 42001), the control library and evidence contract (mod-102/108), the risk taxonomy and appetite (mod-106), the pre-deployment assurance gate (this module chapter 02), the ongoing assurance programme (this module chapter 03), the incident management flow (mod-110), the third-party governance flow (mod-109), the AIA process (mod-105 chapter 04). Add scenario-specific areas as required — the SR 11-7-shaped MRM audit for the bank, the FDA-adjacent SaMD-lifecycle audit for the healthcare scenario, the customer-facing SOC-2 leverage audit for B2B SaaS.
- **The cadence per area.** Chapter 04 argues higher-risk areas sample annually, lower-risk areas across the cycle. Specify the cadence for each area you enumerated, with a defence for anything sampled less than annually.
- **The competence pattern per engagement.** Pattern A (development), B (co-sourcing), C (external contract) from chapter 04. State the mix and defend it against your scenario's starting internal audit function. Anything sampling deep ML methodology requires either A with a developed capability or B with a specialist co-source partner.
- **The three-year sequencing.** Which engagements land in year 1, year 2, year 3. Sequence so that the year-3 audits align with the ISO 42001 certification-body's three-year recertification cycle (the third-line audit outputs feed the external re-cert package — chapter 05).
- **The rotation discipline.** Auditor rotation: no auditor leads consecutive engagements on the same second-line function's work; no engagement is led by an auditor who was on the second-line function during the engagement's scope window. State the rotation rules and how they are enforced.
- **The non-scope areas and the rationale.** At least three areas the programme deliberately does *not* sample in the three-year cycle, and why. Candidates: the internal audit function's own execution (out-of-scope by IPPF discipline; audited by external quality assessment); the frontier-model provider's model internals (out-of-scope; the third-party governance chapter 04 samples the provider's controls but not the provider's model); the AI-accountable executive's decisions (out-of-scope; those are the board's oversight).

### `annual-audit-plan.yaml`

The coming year's plan. For each engagement in the year, produce a full entry against chapter 04's schematic:

```yaml
engagement:
  id: ENG-2027-04
  title: <descriptive title>
  scope: <full paragraph>
  applicable_standards: [<ISO 42001 clauses, SR 11-7 sections, IPPF sections, ...>]
  competence_required: [<specific competence areas>]
  team:
    internal_audit_lead: <seat>
    internal_audit_associates: [<seat>, <seat>]
    co_source_partner: <partner or n/a>
  scope_boundaries:
    excludes: [<what is explicitly out-of-scope for this engagement>]
    independence_carve_outs: [<any auditor rotation constraints>]
  sampling_methodology:
    sample_frame: <what population is sampled>
    sample_size_and_justification: <how many, why>
    sampling_transparency_commitment: <what will and will not be sampled will be
                                       named in the report>
  planned_dates:
    fieldwork: <YYYY-MM-DD through YYYY-MM-DD>
    report_due: <YYYY-MM-DD>
    audit_committee_report: <YYYY-QN meeting date>
  cross_reference_to_programme_year: <Y1 / Y2 / Y3 area>
```

At least eight engagements across the year (adjust to your scenario — a small B2B SaaS internal audit function may have fewer; a large bank internal audit function typically has more). At least two engagements must sample the second line's own work (a pre-deployment gate audit; an ongoing assurance programme audit). At least one engagement must sample first-line evidence directly (a specific tier-3 or tier-4 system's evidence contract discharge). At least one engagement must be a *follow-up* on prior-year CAPA effectiveness.

### `engagement-scope-of-work.md`

Take one engagement from the plan — pick the pre-deployment gate audit or the ongoing assurance programme audit, whichever is more consequential for your scenario. Walk the six-phase engagement procedure from chapter 04 at full depth:

- **Phase 1 — Planning.** Scope confirmation with the audit committee sponsor; access map (which systems, which second-line seats, which documents, which meetings); competence-coverage assessment (which team members carry which competences; which gaps are closed by co-source); sampling methodology declaration.
- **Phase 2 — Preliminary evaluation.** The specific second-line records read (which decision-records, which risk register entries, which AIA references, which SoA rows). The preliminary observations register shape.
- **Phase 3 — Fieldwork trace testing.** The trace-test method for the sampled decisions or re-assessments. What "trace test" means concretely — the auditor pulls a decision-record, samples the evidence artefacts it cites, spot-checks the reproduction of an evaluation run, spot-checks the second-line reviewer's actual challenge language, spot-checks the escalation trail if the decision was conditional. Specify the workpaper shape.
- **Phase 4 — Fieldwork interviews.** The interview list (second-line reviewers; first-line model owners; evaluation engineer; risk engineer; head of AI governance; analysts). The interview-guide themes per interviewee category. The interview-memo shape and retention.
- **Phase 5 — Analysis and findings drafting.** The finding-severity taxonomy (nonconformity major, nonconformity minor, observation, opportunity for improvement — align to your scenario's audit function convention). The draft-response cycle discipline: response to findings during the engagement is factual clarification only, not severity negotiation; severity is the auditor's.
- **Phase 6 — Reporting.** The report shape; distribution list; the audit-committee deck shape; how findings feed the CAPA process (mod-105 chapter 09). The event-driven pathway for material findings that cannot wait for the next scheduled committee meeting.

For each phase, name the specific challenge questions the audit team asks. Under Phase 3 for a pre-deployment gate audit, a challenge question is not "was the decision-record filed?" — it is "walk me through the residual reading against the tolerance table at gate meeting M; if the eval report cited was updated after the meeting, what changed between the version the gate saw and the version now on file?"

### `audit-artefact-contract.yaml`

Instantiate chapter 04's audit-artefact contract for your scenario. Every per-engagement artefact type (engagement planning memo, workpapers, engagement report, audit-committee deck) plus every cross-engagement artefact type (annual audit-committee report, CAPA effectiveness review) must be specified with:

- **Content** — the fields the artefact carries.
- **Author** — the seat that produces it.
- **Retention** — how long, and under what records-retention discipline.
- **Immutability** — how the artefact is protected from post-hoc modification; how corrections are handled (chapter 04's additive-note-to-file discipline).
- **Distribution** — who receives, on what cadence, at what level (informational / formal / ratifying).

The contract must include the *sampling-transparency principle* from chapter 04 as a formal contract clause on the audit-committee deck: every deck names what was sampled and what was not, so the committee sees the coverage boundary as a first-class item.

### `audit-committee-reporting-shape.md`

Decide and justify:

- **Quarterly summary shape.** The rolling summary of the quarter's engagements, findings by severity, CAPA status, and the next quarter's upcoming engagements. Include the presenting seat (chief audit executive) and the meeting slot the report takes.
- **Semi-annual portfolio-view shape.** The cumulative-findings view, systemic-pattern analysis, trend against prior periods, and competence-programme status. Include what a *systemic pattern* is in this context (e.g., findings across three or more engagements pointing at the same root cause across systems or second-line functions).
- **Annual audit-plan ratification and annual audit-committee report shape.** The forward-looking plan ratification and the backward-looking annual report — the two are a paired agenda item at the annual audit committee meeting.
- **Event-driven emergency reporting.** The trigger criteria (a discovered material weakness in the assurance architecture; a discovered systemic pattern that changes the enterprise's risk posture; a discovered finding that requires the audit committee's ratification before management remediation can proceed); the pathway (chief audit executive to audit committee chair, notification to full committee, emergency meeting if warranted); the artefacts produced.
- **The management-response discipline.** The second-line function's response to findings is included in the report as *management response*; the response accompanies the finding but does not modify it. The audit committee sees both. Specify the discipline that prevents the finding from being softened during draft-response cycling.

## Starter guidance

- Draft the multi-year programme *before* the annual plan. If you cannot decide which areas sample across the cycle, the annual plan will over-fit to the coming year and under-cover the cycle.
- Chapter 04's competence question is where under-specification bites hardest. If your engagement scope requires deep ML methodology and your team does not carry it, name the co-source partner and the deliverable-ownership rules — do not defer the decision to the engagement.
- The engagement-scope-of-work challenge questions are what distinguishes a real audit from a clause-conformance audit (chapter 04 failure mode 1). Write challenge questions that could actually catch a defect — "was the escalation route followed?" is a real challenge question; "does the decision-record exist?" is a paperwork check.
- The audit-artefact contract's immutability discipline is what protects against the second-line function post-hoc massaging findings. Workpapers do not get modified after engagement close; corrections are additive notes-to-file with reason. Enforce it in the contract; do not leave it to hope.
- The management-response discipline is where organisational pressure lands. Under-competent audit functions under organisational pressure soften findings during the draft-response cycle. The reporting-shape document must name the discipline explicitly: findings are the auditor's; responses accompany findings; severity is not negotiated during draft cycling.
- For the bank scenario, the SR 11-7 audit-of-MRM audit is the deepest engagement in your plan; it is where the SR 11-7-shaped internal audit programme composes with the enterprise's AI-scope MRM. For the healthcare scenario, an audit engagement on the clinical-safety oversight committee's role in the assurance architecture is a scenario-specific engagement to include. For the B2B SaaS scenario, an audit engagement on the customer-facing SOC 2 attestation's AI-scope coverage is a scenario-specific engagement — the SOC 2 report is a customer-facing artefact but its AI-scope claims are internal-audit-testable.
- Composition with ForHumanity IAAIS, BABL AI, and ISO/IEC 42006 is the architect's move (chapter 04 walks it). Do not adopt IAAIS wholesale; use its criteria as a domain-coverage checklist your scope-of-work stops short of if any domain is un-touched.

## Acceptance criteria

- [ ] Scenario and starting internal audit function are stated at the top; programme is coherent against them.
- [ ] `multi-year-audit-programme.md` covers the six required areas plus scenario-specific additions; cadence per area is defended.
- [ ] The competence-pattern mix (A / B / C) is defended against the scenario's starting position; scenarios that assert pattern A alone must defend the development timeline.
- [ ] The three-year sequencing composes with the ISO 42001 certification-body re-cert cycle.
- [ ] Rotation discipline is stated and enforceable.
- [ ] Non-scope section names at least three areas deliberately excluded from the cycle.
- [ ] `annual-audit-plan.yaml` enumerates at least eight engagements (adjust for scenario); at least two sample second-line work; at least one samples first-line evidence directly; at least one is a CAPA follow-up.
- [ ] Every engagement in the plan carries scope, applicable standards, competence required, team composition, sampling methodology (including transparency commitment), planned dates, and cross-reference to the programme year.
- [ ] `engagement-scope-of-work.md` walks all six phases at full depth for one engagement; each phase names the specific challenge questions the audit team asks.
- [ ] `audit-artefact-contract.yaml` covers all per-engagement and cross-engagement artefacts; each specifies content, author, retention, immutability, distribution; sampling-transparency clause is present.
- [ ] `audit-committee-reporting-shape.md` decides all five requirements items; management-response discipline is named as a formal discipline.
- [ ] Every unverified citation to IPPF sections, IIA guidance, ISO/IEC 42006 requirements, IAAIS criteria, BABL AI curriculum, SR 11-7 provisions, or sector regulations is marked `<!-- needs-research: ... -->` — no invented section numbers or requirement labels.

## Stretch goals

- Add a *competence-development plan* alongside the multi-year programme — a chart showing how internal audit's own team develops the competences pattern A requires over the three-year cycle; the co-source engagements pattern B covers in the interim; the certifications and rotations that support both.
- Sketch an *audit-of-the-audit-programme* — the external quality assessment (EQA) IPPF requires at least every five years — as a preview of the composition with chapter 05's external-provider interface. Who performs the EQA; what scope; what the EQA's findings feed into.
- Extend the audit-committee reporting shape with a *finding-cost-of-late-discovery* item — for one finding discovered in the year, estimate the cost of the finding having been discovered by the certification body's stage-2 audit instead of by internal audit. Preview of the reporting-value argument the chief audit executive makes to the audit committee for continued investment.
- Add a *composition diagram* showing how the third-line programme feeds the external-audit interface (chapter 05) — which internal audit artefacts land in which external-provider package, which findings feed which remediation plan, which competence gaps in internal audit motivate which external-provider engagement.
- Compose one engagement scope with the *NIST SP 800-37 Assess step* framing from chapter 07 — where the engagement's fieldwork produces an artefact analogous to the RMF's Security Assessment Report, and where the enterprise's internal audit is functioning as the "control assessor" role the RMF assigns.
