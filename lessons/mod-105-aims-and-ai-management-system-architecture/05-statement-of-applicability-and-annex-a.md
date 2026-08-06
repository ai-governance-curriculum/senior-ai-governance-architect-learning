# The Statement of Applicability and the Annex A walk — Clause 6.1.3.d

## Why this chapter exists

The Statement of Applicability (SoA) is *the* most-sampled artefact in a stage-2 AIMS audit. The certification body will open the SoA within the first hour of on-site work and use it to structure the rest of the audit — for each Annex A control the SoA marks as *included*, the auditor will look for evidence the enterprise actually implements it; for each Annex A control the SoA marks as *excluded*, the auditor will read the justification and either accept it, challenge it, or raise a nonconformity. There is no other single artefact whose quality so directly determines the audit outcome.

Under 42001, the SoA is required by Clause 6.1.3.d — the risk-treatment sub-clause that requires the organisation to produce a Statement of Applicability containing the necessary controls (from Annex A and any additional controls determined by the risk-treatment process), a justification for their inclusion, whether they are implemented or not, and a justification for excluding any Annex A control. The SoA is where the risk process (Clause 6.1.2) meets the control library (mod-102) meets the operational reality of the enterprise.

This chapter walks the SoA discipline, the Annex A walk, the inclusion / exclusion / justification pattern, and the binding from the SoA row to the underlying enterprise AI control library.

## What Annex A of ISO/IEC 42001:2023 is

Annex A of ISO/IEC 42001:2023 is a *normative* annex — meaning it is part of the standard's requirements, not just informative background — that enumerates a reference set of controls the AIMS considers. The controls are organised into a set of *control objectives* covering areas such as:

- Policies related to AI.
- Internal organisation.
- Resources for AI systems.
- Impact assessment of AI systems.
- AI system life cycle.
- Data for AI systems.
- Information for interested parties of AI systems.
- Use of AI systems.
- Third-party and customer relationships.

Each control objective is realised by a set of specific controls that the organisation must *consider* for inclusion in its AIMS. (The 42001 standard is paywalled; the authoritative Annex A text is in the published standard. The categorisation above is based on the public-facing description of the annex; the exact clause numbering and control identifiers should be verified against the published standard when authoring the SoA.) <!-- needs-research: confirm the exact list of Annex A control objectives and controls against the published ISO/IEC 42001:2023 text. -->

Annex B of the standard supplies *implementation guidance* for Annex A — non-normative but heavily used. The auditor will not directly test conformance to Annex B, but the enterprise's implementation of Annex A controls will typically follow Annex B guidance as the default interpretation. Where the enterprise implements a control in a way that materially departs from Annex B guidance, the SoA justification should note the departure and the rationale.

Annex C supplies AI-specific risk sources, useful as an input to Clause 6.1.2 risk identification. Annex D touches on integration with other management systems, useful as an input to chapter `10-integrating-the-aims-with-the-iso-27001-isms.md`. <!-- needs-research: confirm the current annexes of ISO/IEC 42001:2023 and their titles. -->

The SoA walks *Annex A*. It does not walk Annexes B, C, or D as such — though it references them where relevant in justifications.

## The five columns of the SoA

A defensible SoA is a table (usually rendered as a spreadsheet or YAML register, backed by a machine-readable representation in the GRC-for-AI platform per mod-111) with five columns per Annex A control:

1. **Annex A control identifier and short title.** The identifier as it appears in the published standard. Do not renumber; the auditor uses the standard's numbering.
2. **Included / excluded.** A single flag. `included` means the control applies to the AIMS and the enterprise implements it. `excluded` means the control does not apply.
3. **Justification.** A paragraph explaining *why* the control is included (what risk it addresses, which interested-party requirement it satisfies) or *why* it is excluded (what makes it inapplicable to the enterprise's scope).
4. **Implementation reference.** For included controls: the identifier of the enterprise AI control library entry (from mod-102 — e.g. `AIC-GOV-014`) that implements the Annex A control. For excluded controls: `n/a`. The SoA is the *bridge* from the standard's Annex A vocabulary to the enterprise's own control catalog.
5. **Implementation status.** For included controls: whether the implementation is *in place*, *partially in place*, *planned*, or *deferred*. If partially in place or planned, the SoA should reference the risk-treatment plan entry (RTP-YYYY-Qn-nnn) that owns closing the gap. `in place` is the target state at certification; `planned` and `partially in place` are acceptable at certification only if the risk-treatment plan owns the gap with a credible timeline and top management has accepted the residual risk.

Some SoAs add a sixth column for *operational owner role* (typically pointing to the AIMS role register from Clause 5.3). This is not required by 42001 but is a useful cross-reference — it lets the auditor jump from the SoA row directly to the role holder who owns evidence for that control.

**The row shape in YAML**, for the machine-readable representation:

```yaml
soa_id: NORTHBROOK-AIMS-SOA-2026-06
version: 2.4
date: 2026-06-30
approved_by: head-of-ai-governance
approval_reference: MGMT-REVIEW-2026-Q2

controls:
  - annex_a_ref: A.6.2.4                      # placeholder — verify against standard
    short_title: AI system verification and validation
    decision: included
    justification: >
      The enterprise ships production AI systems that make consequential
      decisions in employment (candidate screening) and financial services
      (fraud detection). Systematic V&V of these systems is required to
      meet the enterprise Responsible AI principle of "demonstrable
      fitness for purpose" and to discharge EU AI Act Article 15
      (accuracy, robustness, cybersecurity) obligations for the two
      high-risk systems the enterprise provides in the EU. Risk register
      link: AI-RISK-2026-089, AI-RISK-2026-101.
    implementation_ref: AIC-ROB-041           # from enterprise AI control library
    implementation_status: in-place

  - annex_a_ref: A.6.2.6                      # placeholder — verify against standard
    short_title: AI system operation and monitoring
    decision: included
    justification: >
      All in-scope production systems require operational monitoring
      per mod-110. Model drift, data drift, and safety-signal monitoring
      are systemically part of the enterprise AIMS operating model.
    implementation_ref: AIC-MON-055
    implementation_status: partially-in-place
    rtp_link: RTP-2026-Q2-021                  # closes the gap on RAG-corpus drift monitoring
    residual_risk_accepted_by: head-of-ai-governance
    residual_risk_acceptance_date: 2026-05-15
    residual_risk_review_date: 2026-11-15

  - annex_a_ref: A.7.4                        # placeholder — verify against standard
    short_title: AI system life cycle management for GPAI
    decision: excluded
    justification: >
      The enterprise is not a GPAI provider. It fine-tunes and deploys
      third-party foundation models but does not train or release
      general-purpose AI systems under its own name. The control
      applies to GPAI providers only; the enterprise's role for its
      GenAI products is deployer (occasionally provider of narrow
      fine-tuned systems, for which the deployer/fine-tuning-provider
      controls in A.6.* apply). Legal opinion memo LEG-AIA-2025-041
      confirms the role classification. Trigger for re-inclusion:
      the enterprise takes on training or open-release of a
      general-purpose AI system.
    implementation_ref: n/a
    implementation_status: n/a
```

The row for `A.6.2.6` shows how a partially-in-place control is handled — the SoA links to the RTP entry that owns closing the gap and records the residual-risk acceptance decision with the accepting authority and the date. Auditors specifically look for this — a `partially-in-place` row *without* an RTP link and *without* a residual-risk acceptance is a nonconformity.

The row for `A.7.4` shows an exclusion done well: the justification names the enterprise's role (not a GPAI provider), cites the legal memo, and names the trigger for re-inclusion. This is what an auditor accepts.

## Inclusion justifications — what makes them defensible

Every included control needs a justification. The auditor is looking for two things:

1. **The justification connects the control to identified risks or interested-party requirements.** A justification of "we include this because Annex A says so" is not a justification. A defensible justification links to the risk register (a specific risk-id or set of risk-ids the control addresses) or to the interested-parties register (a specific requirement the control satisfies) or to a regulatory obligation (a specific article, memorandum, or clause the control discharges).

2. **The justification is *proportionate* to the enterprise's context.** For a systemically important use case the justification should convey why the control is *material*; for an included-but-low-risk use case the justification can be brief. The auditor is calibrating whether the enterprise understands *why* it does what it does, not testing verbosity.

## Exclusion justifications — where auditors dig

Exclusions are where auditors dig hardest, because a badly-justified exclusion is often where a real gap is being hidden. The three exclusion patterns that survive audit:

**Pattern 1 — Role-based exclusion.** The control applies to a role the enterprise does not occupy. If the enterprise is not a GPAI provider, controls scoped to GPAI providers may be excluded with a clear statement of the role decision. If the enterprise is not a deployer of a specific class of system, controls scoped to that class may be excluded. The justification names the role decision, cites the source of the decision (legal opinion, risk-based determination by the risk-and-impact committee, top-management scoping decision), and names the trigger under which the role decision would change and the control would re-enter scope.

**Pattern 2 — Scope-based exclusion.** The control addresses activities that fall outside the AIMS scope (Clause 4.3). If the AIMS scope excludes R&D prototypes, controls specific to research operations may be excluded on scope grounds. The justification names the scope decision, cites the scope statement, and names the trigger.

**Pattern 3 — Risk-based exclusion.** The control addresses a risk the risk-and-impact process has determined does not materialise in the enterprise's context. This is the hardest exclusion to defend and the one auditors scrutinise most carefully. The justification must name the specific risk-assessment analysis that reached the conclusion, the assumptions the analysis rests on, and the trigger under which the risk would be re-evaluated. Auditors will often accept a risk-based exclusion only when the analysis is documented and repeatable; a bare "we decided it does not apply" fails.

**Exclusion antipatterns to avoid.**

- *Cost-based exclusion.* "We decided the control was too expensive to implement." 42001 does not permit exclusion on cost grounds. The enterprise must either include the control (and accept the cost) or make a risk-based decision to reduce scope (and change Clause 4.3). Cost-driven omissions dressed as risk-based exclusions are a common finding at first-time certification.
- *Compliance-window exclusion.* "This control's underlying regulation does not take effect until 2027, so we exclude it now." Annex A controls do not have regulatory phase-in schedules; if the risk exists, the control is applicable. The enterprise may plan implementation over time (`implementation_status: planned` with a credible RTP link) but should not exclude.
- *Vague appeal to Annex B.* "Annex B guidance does not fit our context, so we exclude the control." Annex A is normative; Annex B is guidance. Departing from Annex B guidance is permitted (with justification); excluding the Annex A control on that basis is not.

## The SoA-to-control-library binding

The SoA's *implementation reference* column is the bridge from the Annex A vocabulary to the enterprise's own AI control library (mod-102). This is the mechanism by which the SoA row becomes *testable*. Without the binding, the SoA says "we do the thing" without naming what "the thing" is; with the binding, the auditor can trace SoA row → enterprise control library entry → evidence contract → evidence produced against a specific system → validation by internal audit.

Two rules make the binding survive:

**Rule 1 — Every included Annex A control maps to at least one enterprise control library entry.** Multi-mapping is allowed and common — a broad Annex A control ("AI system verification and validation") may map to several enterprise controls (adversarial-robustness testing, accuracy testing on the acceptance dataset, benchmark evaluation, pre-deployment red-team, deployment shadow-testing). List them all. Missing mappings are the most common SoA finding at first-time audit.

**Rule 2 — Every enterprise control library entry that the SoA references must exist in the library at the version the SoA cites.** No dangling references. If the SoA cites `AIC-ROB-041 v2.1`, the library must actually carry `AIC-ROB-041 v2.1`. Mod-102 chapter 09 details the control-library lifecycle and versioning; the AIMS's SoA obeys it.

## Approval, versioning, and refresh cadence

The SoA is *approved* documented information. The AIMS's role register (Clause 5.3) names the authority that approves the SoA — typically the head of AI governance at level 60, sometimes the AI-accountable executive for the initial approval and any material subsequent change. The approval reference (e.g. `MGMT-REVIEW-2026-Q2`) points at the meeting or the decision record where approval was recorded.

The SoA is *versioned*. Every change to an inclusion decision, an exclusion decision, a justification, or an implementation reference produces a new version with a change log. The auditor will look at version history to understand how the SoA has evolved between audit cycles.

The SoA is *refreshed on a defined cadence*. The default cadence is:

- **Quarterly** — desk review by the head of AI governance to check whether new risks, new systems, new regulatory changes, or new incidents warrant a change to any row. Most quarterly reviews produce zero changes; the point is that the review happened.
- **Annually** — full walk of every row, coinciding with the management review, ensuring implementation-status columns are current and no justifications have decayed.
- **Event-driven** — new AI system in scope, material change to an existing system, incident, regulatory change, standard revision. Event-driven changes are logged with the trigger.

## Two failure modes in SoA authoring

**Failure mode 1 — the SoA that copies Annex A verbatim.** The enterprise takes Annex A, marks every control `included`, and writes "the enterprise includes this control" as the justification. The SoA is 40 pages long, contains no exclusions, and reads like a compliance recital. The auditor asks "why is A.7.4 included when you are not a GPAI provider?" and the enterprise has no answer. Every audit produces the same finding: the SoA is not the enterprise's, it is a photocopy. The fix is doing the *work* — reading each Annex A control against the enterprise's actual scope, risk, and role, and writing a real justification.

**Failure mode 2 — the SoA that hides gaps behind `included`.** Every control is marked `included` regardless of whether it is actually implemented. Auditors sampling `included` controls find no evidence and raise nonconformities across the board. The AIMS fails stage-2 certification. The fix is honesty in the *implementation status* column: `partially-in-place` and `planned` are acceptable at certification if the RTP owns closing the gap and top management has accepted the residual risk. `in-place` marked against an unimplemented control is a fatal finding.

## The relationship to the risk-treatment plan

The SoA and the risk-treatment plan are *joined artefacts*. Their relationship:

- The risk process (Clause 6.1.2) identifies risks. The risk-treatment plan (Clause 6.1.3, chapter `06-risk-treatment-plan-and-operational-clauses.md`) selects treatments per risk. Some treatments *are* the operation of an included Annex A control; some are the strengthening of a partially-in-place control; some are the design and roll-out of a control that does not exist yet.
- The SoA records the *end-state* of the control set — every Annex A control's decision. The RTP records the *treatment actions* — the specific interventions needed to reach the SoA's end-state.
- A control that the SoA marks `in-place` may still have an RTP entry (e.g. a treatment to strengthen its evidence collection). A control that the SoA marks `partially-in-place` *must* have an RTP entry that owns closing the gap. A control that the SoA marks `planned` *must* have an RTP entry that owns the initial implementation.
- The RTP entries reference SoA rows (`soa_row_ref: A.6.2.4`) and the SoA rows reference RTP entries (`rtp_link: RTP-2026-Q2-021`). The bi-directional link is what makes the two artefacts joint.

## Summary

The SoA is the AIMS's most-sampled certifiable artefact — a table that walks each Annex A control, records whether it is included or excluded, records why, points at the enterprise AI control library entry that implements it, and records implementation status. Every inclusion needs a real justification tied to a risk or a requirement; every exclusion needs a defensible pattern (role-based, scope-based, or risk-based) with a stated trigger for re-inclusion. Exclusions on cost or compliance-window grounds are antipatterns. The SoA-to-control-library binding — from Annex A vocabulary to enterprise control identifier — is what makes the SoA testable. The SoA is approved by named authority, versioned, and refreshed on a defined cadence. Its relationship to the risk-treatment plan is joint: the SoA is the end-state; the RTP is the actions to reach it. The next chapter walks the RTP shape and the operational-planning clause it composes with.
