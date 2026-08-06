# The third-line independent audit programme — designing the internal audit function's AI scope

## Why this chapter exists

The third line of defence for AI is *internal audit* — the function reporting to the board's audit committee that provides independent assurance the first and second lines are actually working. Under the IIA 2020 Three Lines Model this function's independence is what allows the board to certify the enterprise's governance in front of shareholders, regulators, and the market. Under SR 11-7 the internal audit of the MRM programme is what the federal banking supervisors expect to sample. Under ISO/IEC 42006 the certification body will look at whether the internal audit function is competent to have found what the certification body itself is finding — if not, the internal audit programme itself is a nonconformity. Under ForHumanity's Independent Audit of AI Systems (IAAIS) framework the third-line-analogue independent-auditor role provides an external check with a comparable rigour bar. Under BABL AI's Algorithmic Bias Auditor and similar independent-auditor certifications (New York City Local Law 144 bias-audit ecosystem) an external analogue to the third line performs an audit the enterprise's own third line should have been able to conduct.

The level-50 architect *does not run* the internal audit function — internal audit reports to the audit committee, not to the governance office. But the architect *designs the audit programme's shape* — the annual audit plan template, the engagement scope-of-work template, the audit-artefact contract, and the audit-committee reporting cadence — as an architectural asset the internal audit function operates against. Doing so gives the internal audit function a defensible bar to work against; it gives the audit committee a legible reporting deck; and it gives the certification body's external auditor a reference point when the certification body asks "how does your internal audit programme sample your AIMS."

This chapter walks the design.

## What the third line's scope covers

The third line's AI-scope covers the whole assurance architecture the previous chapters have built. It samples first-line evidence, second-line assurance records, and — critically — the operations of the second-line function itself. Concretely, the third-line programme's scope includes:

- **The AIMS clauses and the SoA.** Does the AIMS conform to ISO/IEC 42001 as the enterprise has committed? Is the SoA current, coherent, and accurate against the systems in scope? Mod-105 chapter 08 walks the AIMS internal audit as the second-line-adjacent function; the third-line audit *also* samples this ground, from an independent view of the audit committee.
- **The control library and the evidence contract.** Does the enterprise control library (mod-102) actually get implemented on the systems it applies to? Do the controls produce the evidence the evidence contract (mod-108) requires? Does the evidence flow through the pre-deployment gate to a defensible decision-record?
- **The risk taxonomy, the appetite architecture, and the risk register.** Is the taxonomy being applied correctly? Is the appetite being respected? Is the register current? Does the portfolio view reflect what is in the register?
- **The pre-deployment assurance gate.** Does the gate actually challenge first-line evidence, or does it ratify? Are the escalation paths used when they should be? Are the decision-records complete and coherent?
- **The ongoing assurance programme.** Are the triggers firing? Are the re-assessments producing findings that route into the risk register? Is the stale-residual escalation actually happening?
- **The AIA process.** Is the AIA firing when it should? Are AIAs feeding the risk register and the RTP?
- **The incident management flow (per mod-110).** Are incidents being triaged, classified, contained, root-caused, and closed? Are the corrective actions preventing recurrence?
- **The third-party governance flow (per mod-109).** Are third-party AI providers assessed and monitored per the enterprise's programme?

The third line does not audit itself. Where the internal audit function's own execution is in scope — for a certification body audit, for an external quality review — that is the certification body's or the external quality reviewer's role, not the internal audit function's.

## The competence question — and why it is architectural

Under IIA 2020 and SR 11-7, internal audit's independence is what makes it third-line. Under ISO/IEC 42006, an internal audit function that samples an AIMS must be *competent* to sample it. Under IAAIS and BABL AI, the independent-auditor competence bar is explicit — an AI auditor is expected to hold specific credentials and to have specific practical competencies. The architect's problem: the enterprise's classical internal audit function typically was not staffed against these competencies. The architecture must specify how the competence gap is closed.

**Three legitimate patterns.**

- **Pattern A — competence development.** The internal audit function invests in AI-audit competence for its own people: certifications (IAAIS, BABL AI Algorithmic Bias Auditor, ISACA's Certified in the Governance of AI / AI Assurance credentials as they mature, sector-specific credentials such as Certified Model Risk Auditor in banking), practical rotations through the AI evaluation function (with a structured return-to-audit path), and ongoing continuing education. Costs are internal and controlled; the timeline to competence is typically 12 to 24 months.
- **Pattern B — co-sourcing.** The internal audit function retains its independence but engages a specialist firm (Big Four with an AI-audit practice; ForHumanity-listed auditors; specialised AI-audit consultancies) as a co-source partner. The co-source partner works under the internal audit function's engagement scope; the co-source deliverables are internal audit's deliverables. Costs scale with engagement; the timeline to standing up the arrangement is short (typically weeks to months). The architect specifies the co-source partner's role, the independence discipline (the co-source partner cannot advise the second line on remediation for anything it audits), and the deliverable-ownership rules.
- **Pattern C — external contract.** The internal audit function commissions an external firm to conduct a full audit on their behalf and to a standard the internal audit function ratifies. Common when the internal audit function is small and cannot support co-source. The external firm's work is delivered to the internal audit function; the internal audit function reviews and files with the audit committee. The independence is preserved through the contract's terms.

The three patterns are not mutually exclusive; a mature enterprise typically uses A for foundational audit competence and B for specialist gaps (e.g., a specific ML security audit, a specific fairness audit for a specific system class). The architect designs the mix, subject to the audit committee's ratification.

## The audit-plan authoring shape

The internal audit function publishes an *annual audit plan* — the document that names, for the coming year, which audit engagements will be conducted, at what scope, on what timing. The audit committee ratifies the plan. The architect designs the shape the plan takes so it is legible to the committee and defensible under external audit sampling.

**The multi-year audit programme.** A one-year plan is insufficient. The audit function publishes a *multi-year programme* — typically three years — that shows how every material area of the assurance architecture is sampled at least once across the cycle. Areas that are lower risk are sampled less frequently; higher-risk areas are sampled annually. The three-year programme protects against the "high-risk areas sampled annually forever, low-risk areas never sampled" pathology.

**Per-year plan shape.** For each engagement in the coming year:

```yaml
engagement:
  id: ENG-2027-04
  title: End-to-end audit of the pre-deployment assurance gate on tier-3 systems
  scope: >
    All tier-3 pre-deployment decisions taken 2026-Q3 through 2027-Q1
    (~28 decisions expected); representative sample of at least 6 decisions
    with full trace test (evidence contract discharge, six-review outcomes,
    escalation record, decision-record filing).
  applicable_standards:
    - ISO/IEC 42001 clauses 8, 9.2 (AIMS operational planning; internal audit)
    - SR 11-7 (validation independence; where applicable)
    - IAAIS (independence and integrity criteria; where the audit function chooses to
      align its findings language)
    - enterprise policy AI-POL-001 (AI policy under mod-103)
  competence_required:
    - AI evaluation methodology depth (level equivalent to IAAIS or BABL AI)
    - ISO 42001 audit competence (level equivalent to ISO 42006's requirements for
      certification-body auditors)
    - Sector-specific competence where the sample includes sector-regulated systems
  team:
    - internal_audit_lead: audit-lead-2
    - internal_audit_associate: audit-assoc-4
    - co_source_partner: FirmX-AI-audit-practice (for the ML methodology depth)
  scope_boundaries:
    - excludes: audit of the internal audit function's own execution (out-of-scope,
      audited by external quality reviewer per IIA IPPF)
    - excludes: audit of any decision-record where lead auditor was seat-in-second-line
      during the decision window
  planned_dates:
    - fieldwork: 2027-04-01 through 2027-05-15
    - report_due: 2027-06-15
    - audit_committee_report: 2027-Q3 meeting (2027-08-14)
  cross_reference_to_programme_year:
    - Y1 (2027): pre-deployment gate; ongoing assurance; incident management; third-party
    - Y2 (2028): control library; SoA; risk taxonomy; risk register; evidence architecture
    - Y3 (2029): AIMS clauses end-to-end (dovetails with certification body re-cert audit)
```

The audit committee reads the multi-year programme and the year's plan, and ratifies both. The plan is a public artefact within the enterprise (subject to the audit function's confidentiality discipline); the second-line function reads it and prepares.

## The engagement — how a single audit runs

Within a scoped engagement, the audit team walks a defined procedure. The architect authors the *engagement-scope-of-work template* — the document that structures every engagement so audit-committee reports are comparable across engagements and the certification body reviewing internal audit findings can sample against a consistent shape.

**Engagement procedure — six phases.**

1. **Planning.** Audit team confirms scope with the audit committee's designated sponsor; obtains access to the sampled records; confirms the sampling methodology; confirms competence coverage. Deliverable: *engagement planning memo*.
2. **Preliminary evaluation.** Read the second-line records for the scope. For the pre-deployment gate audit above, this is reading the sampled decision-records, the risk register entries cross-referenced, the AIA references, the SoA state at each decision date. Deliverable: *preliminary observations register*.
3. **Fieldwork — trace testing.** Walk the sampled decisions end-to-end, from first-line evidence through second-line review to decision-record. Sample first-line evidence artefacts (spot-check the model card, spot-check the eval-set reproduction, spot-check the training-data provenance). Sample second-line review artefacts (spot-check the reviewer's actual challenges to first-line claims — where the second-line reviewer signed off without a defended challenge, the audit records it). Deliverable: *trace-test workpapers*.
4. **Fieldwork — interviews.** Interview the second-line reviewers who signed the sampled decisions. Interview the first-line model owners whose systems were reviewed. Interview at random across the assurance ecosystem (analysts, evaluation engineer, risk engineer). Deliverable: *interview memos*.
5. **Analysis and findings drafting.** Compose observations into findings; classify (nonconformity major, nonconformity minor, observation, opportunity for improvement); draft recommendations. Circulate draft to the second-line function for factual response only (not for negotiation of finding severity). Deliverable: *draft findings register*.
6. **Reporting.** Produce the engagement report. Distribute to the head of AI governance (informational), the audit committee sponsor (formal), and the audit committee (deck at the next scheduled meeting). Findings feed the CAPA process. Deliverable: *engagement report and audit-committee deck*.

The six-phase procedure is not novel; it is the classical IIA IPPF-shaped procedure specialised for AI. What is AI-specific is the depth of trace testing (a decision-record trace-test that stops at "the record is complete" is superficial; the audit is expected to sample first-line evidence and second-line review substance) and the competence depth on interviews (an interviewer who cannot follow the model owner's technical explanation cannot audit the model owner's decisions).

## The audit-artefact contract

The architect authors an *audit-artefact contract* — the specification of what artefacts the audit function commits to producing per engagement, so the audit committee, the certification body, and the enterprise's own quality-assurance function can rely on their shape.

**The audit-artefact contract.**

```yaml
audit_artefact_contract:
  version: 1.1.0
  per_engagement:
    - engagement_planning_memo:
        content: scope confirmation; sampling methodology; competence coverage; access map
        author: engagement lead
        retention: 7 years (or matching the enterprise's records-retention policy for internal audit workpapers)
    - workpapers:
        content: trace-test evidence, interview memos, observations, sampling records
        author: engagement team
        retention: 7 years
        immutability: workpapers are not modified after engagement close; corrections
                      produce a new note-to-file with reason
    - engagement_report:
        content:
          - scope audited
          - sampling methodology
          - findings (each with description, evidence citation, severity classification,
            recommendation, required response date, CAPA route)
          - management response (second-line's response to findings, filed alongside)
          - opinion (where the engagement scope warrants an opinion)
        author: engagement lead
        retention: indefinite (institutional record)
    - audit_committee_deck:
        content:
          - engagement summary
          - findings by severity
          - trend against previous engagements in the same area
          - open findings from previous engagements
          - what was sampled and what was not sampled (the sampling-transparency principle)
        author: chief audit executive or designate
        retention: indefinite
  cross_engagement:
    - annual_audit_committee_report:
        content: portfolio view of AI-scope audits over the year; findings-trend
                 analysis; competence-programme status; multi-year programme progress
        author: chief audit executive
        published: annually, at the audit committee meeting following the fiscal year close
        retention: indefinite
    - CAPA_effectiveness_review:
        content: for each CAPA closed in the year, an effectiveness assessment
                 (did the corrective action actually prevent recurrence?)
        author: engagement lead of the audit that surfaced the finding, or a
                separately scoped follow-up
        cadence: annually per CAPA cohort
```

The contract is *architected* — the architect designs it and the audit function operates against it. Changes to the contract require the audit committee's ratification. The certification body reads the contract when sampling internal audit's discipline.

## The audit-committee reporting cadence

The architect specifies the cadence at which the audit function reports to the audit committee. The design pressure is: enough reporting that the committee's assurance role is discharged; not so much that the committee is buried in operational detail. The typical shape:

- **Quarterly.** A rolling summary of engagements completed in the quarter, findings by severity, CAPA status, upcoming engagements in the next quarter. Presented by the chief audit executive.
- **Semi-annually.** A portfolio-view readout — cumulative findings, systemic patterns, trend analysis, competence-programme status.
- **Annually.** The annual audit-plan ratification (looking forward) plus the annual audit-committee report (looking back).
- **Event-driven.** For serious findings — a discovered material weakness in the assurance architecture, a discovered systemic pattern that changes the enterprise's risk posture — an emergency or ad-hoc reporting cadence with the audit committee's chair.

The reporting language is the internal audit function's, not the second-line function's. Findings that the second-line function contests are still reported; the second-line function's contest is included as management response. The audit committee sees both.

## Composition with ForHumanity IAAIS, BABL AI, and ISO/IEC 42006

The three references named in the module's learning objectives provide *shape templates* the architect adapts. Each carries a distinct emphasis; composed, they give the third-line programme a defensible bar.

**ISO/IEC 42006 — the audit-body competence shape.** 42006 (mod-105 chapter 11 walked its use for the certification body) specifies auditor competence in AI-specific domains. The internal audit function's competence programme should match or exceed the 42006 bar so that internal audit findings are competent to have caught what the certification body catches. Where the enterprise's internal audit function is not resourced to match 42006 alone, co-sourcing (pattern B above) with a partner that does closes the gap.

**ForHumanity IAAIS — the independent-auditor discipline shape.** ForHumanity's Independent Audit of AI Systems (IAAIS) framework specifies a rigorous independence discipline and an audit-criteria structure that the ForHumanity independent auditor operates against. The internal audit function need not adopt IAAIS wholesale, but the IAAIS *criteria* (across ethics, bias, cybersecurity, privacy, trust, and other named domains) are a useful checklist against which to scope audit engagements — an engagement that covers only ISO 42001 clauses without touching the IAAIS domains may leave systemic gaps. <!-- needs-research: verify the current IAAIS criteria structure and named domains against the ForHumanity publication, which has evolved and is versioned. -->

**BABL AI Algorithmic Bias Auditor and NYC LL144 ecosystem.** BABL AI's Algorithmic Bias Auditor programme and the New York City Department of Consumer and Worker Protection's bias-audit ecosystem under Local Law 144 (bias-audit requirement for automated employment decision tools) provide the *bias-audit specialism* the third-line programme composes into its engagements. Where the enterprise deploys AI in employment or hiring contexts under NYC LL144's scope, the third-line audit engagement for those systems typically composes a BABL-AI-shaped bias audit as a specialist sub-engagement. <!-- needs-research: verify the current NYC LL144 bias-audit criteria and the BABL AI Algorithmic Bias Auditor curriculum against the DCWP guidance and BABL AI publications. -->

The composition is the architect's move: 42006 for the audit-competence baseline; IAAIS for the independence discipline and the domain-criteria checklist; BABL AI (and comparable specialisms) for the technical-depth sub-audits within engagements. The result is a third-line audit programme that has a defensible bar against every major external reference the enterprise's stakeholders might name.

## Two failure modes to design against

**Failure mode 1 — the AIMS-clauses-only audit.** The internal audit function conducts audits scoped tightly to the ISO/IEC 42001 clause structure and reports "no nonconformity" year after year. The clauses are being followed on paper. The certification body's stage-2 audit — with the sampling depth from mod-105 chapter 11 — finds material issues on the same systems the internal audit passed. The audit committee is embarrassed and the certification body raises a Clause 9.2 nonconformity against the internal audit programme itself. The fix is *scope beyond the clauses* — audit engagements are scoped by *outcome* (does the pre-deployment gate actually challenge?, does ongoing assurance actually re-assess?, do incidents actually produce systemic corrective actions?) and only secondarily by the clause structure. The clause conformance falls out of the outcome audit; the outcome audit does not fall out of clause conformance.

**Failure mode 2 — the friendly audit.** The internal audit function is under-resourced, under-competent, and unwilling to challenge the second-line function that has more organisational capital than the audit function does. Findings are downgraded during the draft-response cycle; opinions are softened; recommendations are made non-binding. The audit committee, unfamiliar with AI, does not spot the softening. The fix is a set of architectural moves: the chief audit executive reports functionally to the audit committee (not administratively to the CEO in a way that mutes them); the audit committee's ratification of the multi-year programme and the audit-artefact contract is the discipline that makes softening visible; the composition with 42006, IAAIS, and BABL AI gives the audit function an external bar to point to when internal pressure is applied.

## Summary

The third-line internal audit function samples the whole assurance architecture the previous chapters designed, reporting to the audit committee. The architect designs the annual audit plan template, the multi-year audit programme, the engagement-scope-of-work template, the audit-artefact contract, and the audit-committee reporting cadence. Competence is closed through a mix of development (pattern A), co-sourcing (pattern B), and external contract (pattern C). Engagements walk a six-phase procedure with substantive trace testing and interview depth; artefacts are defined in a contract the audit committee ratifies. Reporting cadence spans quarterly operational summary, semi-annual portfolio view, annual audit-plan ratification and annual report, and event-driven emergency reporting. ISO/IEC 42006, ForHumanity IAAIS, and BABL AI (with the NYC LL144 ecosystem) compose as shape references — 42006 for audit-competence baseline, IAAIS for independence discipline and domain criteria, BABL AI for technical bias-audit specialism. Two failure modes — clauses-only audit, friendly audit — are architectural, not operational, and the design must specifically protect against them. The next chapter walks the external-audit interface where the third line's programme composes with the certification body, the independent auditor, and the sector regulator's examiner.
