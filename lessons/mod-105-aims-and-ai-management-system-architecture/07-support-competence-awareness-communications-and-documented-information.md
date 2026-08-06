# Support — competence, awareness, communications, and documented information (Clause 7)

## Why this chapter exists

Clause 7 of ISO/IEC 42001 is the *support* clause — the clause that requires the AIMS to have resources (7.1), competent people (7.2), aware people (7.3), managed communications (7.4), and controlled documented information (7.5). It is short in the standard's text and long in the audit-day interviews. Auditors will interview people at random across the enterprise and ask *do you know what the AIMS is; do you know your role in it; do you know who to escalate to; do you know what the AI policy commits to*. If the answers are "I've never heard of it", Clause 7.3 (awareness) has failed and Clause 5 (leadership) usually shares the blame.

The architect designs the *plans* that Clause 7 requires — competence plan, awareness programme, communications plan, documented-information register — and hands them to the head of AI governance to operate. This is not a "write once and forget" exercise. Competence obsolesces as the AI stack evolves; awareness decays; communications channels grow stale; documented information drifts out of version control. The architect designs the plans to survive that decay, and specifies the refresh cadence explicitly.

## Clause 7.1 — resources

Clause 7.1 requires the organisation to determine and provide the resources needed for the establishment, implementation, maintenance, and continual improvement of the AIMS. This is where top management's Clause 5 commitment shows up as line items in the budget — headcount for the head of AI governance's team, contract capacity for external assessors when internal audit does not have AI-specific competence, tooling budget for the GRC-for-AI platform (mod-111), training budget for competence development.

The architect designs the *resource inventory* the head of AI governance maintains — a rolling register of the resources the AIMS commits to, by category, with a review cadence tied to the management-review calendar (Clause 9.3).

```yaml
resource_inventory_id: AIMS-RESOURCES-2026-H2
version: 1.0
approved_at_management_review: 2026-Q2

people:
  - role: head-of-ai-governance
    headcount: 1
    status: filled
  - role: ai-governance-analyst
    headcount: 2
    status: 1 filled, 1 open (Q3 hire).
  - role: ai-risk-engineer
    headcount: 4
    status: filled
  - role: ai-evaluation-engineer
    headcount: 3
    status: filled
  - role: internal auditor (AI competence)
    headcount: 1
    status: shared with information-security audit team.

tooling:
  - name: enterprise GRC-for-AI platform
    licence_status: active through 2027-Q3.
  - name: AI evaluation harness (internal build)
    ownership: ai-evaluation team; SLA maintained.

external_capacity:
  - assessor: <named third-party assessor>
    engagement_cadence: annual pre-audit readiness review.

known_gaps:
  - Sector-specific auditor competence for the medical-device
    products (currently satisfied by external consultant; RTP
    entry RTP-2026-Q2-034 tracks in-sourcing decision).
```

The resource inventory is one of the exhibits the management review inspects. If the resource inventory has known gaps that the RTP does not own, the review has a decision to make: fund the gap, accept the gap with explicit residual-risk acceptance, or reduce scope.

## Clause 7.2 — competence

Clause 7.2 requires the organisation to determine the necessary competence for persons doing work under its control that affects AI performance, to ensure those persons are competent on the basis of appropriate education, training, or experience, to take actions to acquire the necessary competence where gaps exist, and to retain documented information as evidence of competence.

**The competence matrix.** The architect designs a *competence matrix* — a table that names, per AIMS role (from the Clause 5.3 role register), the competences required for that role, the assessment method, and the evidence retained. The matrix is not a training catalogue; it is the *statement of what "competent" means* for each role in the AIMS.

```yaml
role: ai-risk-engineer (level 25)
competences_required:
  - id: COMP-AIR-01
    domain: AI risk methodology
    detail: >
      Able to facilitate ISO/IEC 23894-guided risk identification
      for an AI system, produce a risk register entry per the
      enterprise schema, and support Clause 6.1.2 assessment work.
    assessment_method: >
      Written case exercise on a synthetic AI system; oral defence
      to the head of AI governance.
    evidence_retained: >
      Assessment record signed by assessor and assessee, filed
      in the HR competence record.
    reassessment_trigger:
      - Every 24 months.
      - On material change to the enterprise risk methodology.
      - On assignment to a system class the engineer has not
        previously covered.

  - id: COMP-AIR-02
    domain: AI system evaluation
    detail: >
      Able to specify pre-deployment evaluation for a defined
      system tier per the enterprise evaluation standard;
      able to read and interpret an evaluation report produced
      by the level-35 evaluation engineer and identify residual
      risk.
    assessment_method: >
      Peer review of a written evaluation-specification for a
      candidate system; sign-off by ai-evaluation-engineer.
    evidence_retained: >
      Peer-review record, evaluation specification, sign-off note.
    reassessment_trigger:
      - Every 24 months.
      - On material change to the evaluation standard.

  - id: COMP-AIR-03
    domain: relevant regulatory frame
    detail: >
      Able to identify which enterprise AI systems attract
      obligations under the primary regulatory regimes in scope
      (EU AI Act, US federal frame, applicable state statutes,
      sector regulators) and translate obligations into
      control-library requirements.
    assessment_method: >
      Written exercise on the current portfolio; supported by
      internal legal.
    evidence_retained: >
      Exercise record; annual training-completion certificate.
    reassessment_trigger:
      - Every 12 months.
      - On material regulatory change (new statute in scope,
        material amendment, new implementing act).
```

The competence matrix is retained as documented information; individual competence records are retained per person per role assignment. Auditors will sample: pick a role holder, ask for their competence record, verify the assessments cited actually occurred, verify the reassessment cadence has been honoured.

**The delegation to HR.** In most enterprises HR and Learning-and-Development own the *operation* of the training programme, competence-record maintenance, and record retention. The AIMS *specifies* what competences the AIMS requires; HR *delivers*. The architect designs the interface — the competence matrix the AIMS publishes, the record schema HR maintains, the sign-off flow between them.

## Clause 7.3 — awareness

Clause 7.3 requires that persons doing work under the organisation's control are aware of the AI policy, their contribution to the effectiveness of the AIMS, the benefits of improved AI performance, and the implications of not conforming with AIMS requirements.

Awareness is not the same as competence. Competence is what the AI-risk engineer needs to do their work; awareness is what *every* person in the enterprise touching AI needs to know so they can play their part or escalate.

**The awareness programme.** The architect designs a *tiered awareness programme*:

- **Tier 1 — everyone.** Every employee: what the AI policy is, at a high level; who to escalate to if they see an AI-related concern (an AI system behaving unexpectedly, a customer complaint about AI, an ethical concern about a proposed use case, a suspected incident). Delivered through enterprise onboarding, annual refreshers, all-hands segments.
- **Tier 2 — anyone touching AI products.** Employees whose work involves designing, developing, deploying, operating, or interpreting outputs of AI systems: the AI system lifecycle overview, the enterprise AI standards they operate under, the incident-reporting process, the change-management process. Delivered through role-specific training modules and periodic refreshers.
- **Tier 3 — AI-specific roles.** Roles in the AIMS role register: full AIMS orientation, deep familiarity with the specific clauses their role satisfies, active participation in the AIMS operations calendar (chapter `08-performance-evaluation-internal-audit-and-management-review.md`).

Awareness completion rates are a Clause 7.3 audit exhibit. The awareness programme should report — into the AIMS operational cadence and up to the management review — completion rates by tier and gaps. Auditors have a habit of walking a random floor, tapping a random employee, and asking Tier 1 questions. When the answer is "I don't know", the awareness programme has failed and Clause 7.3 goes into the findings register.

## Clause 7.4 — communication

Clause 7.4 requires the organisation to determine the internal and external communications relevant to the AIMS, including on what to communicate, when, with whom, how, and by whom. The output is the *communications plan* — a documented plan that names each communication (audience, content, cadence, channel, owner) and is refreshed annually.

**Internal communications** typically include:

- Monthly AIMS operational update to the AI-accountable executive and the head of AI governance's leadership team.
- Quarterly AIMS operational review to the extended governance stakeholder group.
- Annual management review (Clause 9.3) to top management (chapter 08).
- Ad-hoc incident communications per mod-110.
- Ad-hoc regulatory-change communications when a new obligation or a material change lands.
- Annual all-hands segment on the state of AI governance.

**External communications** typically include:

- Regulatory filings and notifications (per regime): registrations, incident reports, post-market monitoring reports.
- Customer-facing transparency documents (per product): model cards, service disclosures, AI-use notices.
- Investor and rating-agency communications.
- Participation in industry consortia and standards bodies.
- Response to media, civil-society, or public enquiries.

The communications plan is not a marketing plan and not an internal-comms plan; it is the AIMS's own communication programme. Where the AIMS's outputs overlap with the enterprise's broader communications (e.g. the enterprise's ESG report includes AI governance content), the plan names the joint touchpoints and the split of ownership.

## Clause 7.5 — documented information

Clause 7.5 requires the AIMS to include the documented information required by the standard and the documented information the organisation determines is necessary. Documented information must be identified, formatted, and reviewed; created and updated; controlled (access, distribution, retrieval, retention, disposition).

The AIMS's documented-information register is a catalogue of every artefact the AIMS carries — from the AI policy (Clause 5.2) through the SoA (Clause 6.1.3) through the RTP (Clause 6.1.3) through the internal audit records (Clause 9.2) through the CAPA records (Clause 10). Each entry names the artefact, its owner, its location, its access controls, its retention policy, its disposition.

```yaml
documented_information_register:
  - artefact_id: DI-AIMS-001
    name: AI Policy
    clause: 5.2
    owner_role: head-of-ai-governance (draft); AI-accountable executive (approval).
    format: markdown source of truth; PDF published to intranet.
    location: enterprise document management system, path AIMS/policy/current.
    access_control: read — all employees; edit — head of AI governance and delegates.
    retention: current + previous three versions retained indefinitely.
    review_cadence: annual, at management review.

  - artefact_id: DI-AIMS-002
    name: AIMS scope statement
    clause: 4.3
    owner_role: head-of-ai-governance.
    format: markdown; part of the AIMS handbook.
    location: enterprise document management system, path AIMS/scope/current.
    access_control: read — all employees; edit — head of AI governance.
    retention: current + all previous versions.
    review_cadence: annual, at management review; event-driven on scope changes.

  - artefact_id: DI-AIMS-004
    name: Statement of Applicability
    clause: 6.1.3.d
    owner_role: head-of-ai-governance.
    format: YAML in GRC-for-AI platform; rendered to PDF for external sharing.
    location: GRC-for-AI platform, AIMS/soa/current; PDF export in AIMS/soa/exports.
    access_control: read — AIMS role register + internal audit + external assessors on request; edit — head of AI governance and delegates.
    retention: current + all previous approved versions.
    review_cadence: quarterly desk-review, annual full walk, event-driven on scope or Annex A changes.

  - artefact_id: DI-AIMS-006
    name: Risk-treatment plan
    clause: 6.1.3
    owner_role: head-of-ai-governance.
    format: YAML in GRC-for-AI platform.
    location: GRC-for-AI platform, AIMS/rtp/current.
    access_control: read — AIMS role register + top management; edit — head of AI governance and named delegates.
    retention: current + all previous quarterly snapshots.
    review_cadence: monthly operational review, quarterly formal review, annual archive snapshot.

  # ... continues through every AIMS artefact
```

The auditor will ask for the documented-information register on day one and use it as their exhibit map. Every artefact they subsequently sample they will trace back to this register.

**Access and retention policies must satisfy applicable law.** Where the AIMS holds documented information that is regulated by data-protection law (e.g. AIA records that include personal data of assessed populations), retention and access must satisfy the applicable data-protection regime. Where the AIMS holds information that regulators can compel disclosure of (e.g. EU AI Act Article 21 cooperation duties), retention must meet the disclosure horizon. The architect designs the register to expose these constraints; legal and data-protection review confirm them.

## The support layer as an operational load

Clause 7 is not glamorous — no dramatic risk decisions, no headline artefacts — but it is *heavy* to run. Competence records for a dozen roles across dozens of role holders; awareness completion for the whole workforce; a documented-information register with dozens of entries reviewed at defined cadences; a communications plan with an annual and per-event execution log. The head of AI governance's team needs the resources (Clause 7.1) to actually run this. If the resources are underpowered, Clause 7 is the first place the AIMS breaks.

The architect designs the *automation* opportunities: the GRC-for-AI platform maintains the documented-information register; the HR system tracks competence records against the AIMS competence matrix; the learning-management system tracks awareness completion; the intranet publication pipeline handles communications distribution. Chapter `10-integrating-the-aims-with-the-iso-27001-isms.md` will note that the ISMS's own Clause 7 apparatus is often reusable — one competence matrix covers ISMS and AIMS roles for shared staff, one awareness programme covers both, one documented-information register covers both.

## Two failure modes

**Failure mode 1 — the awareness that never was.** The awareness programme exists on paper. Training modules are published. Completion rates are not tracked. Auditors interview five employees at random on day one. None of them know what the AIMS is or what the AI policy commits to. The finding is severe: Clause 7.3 has failed and the AIMS's effectiveness is in doubt. The fix is *completion tracking, refresh cadence, and interviewability*. If a Tier-1 employee cannot in one sentence say what to do on encountering an AI concern, the programme is not landing.

**Failure mode 2 — documented information without version control.** The AI policy exists on the intranet at a URL. Someone updates it in place; the URL now shows a different version; there is no record of the change, no approval trail, no way to reconstruct what the policy said at the time of a specific audit or incident. This fails Clause 7.5 explicitly. The fix is *versioned storage* with change history, approvals, and the ability to render any point-in-time version on demand.

## Summary

Clause 7 makes the AIMS *supportable*. Resources (7.1) are the budget line items the head of AI governance's team draws on. Competence (7.2) is a matrix that defines what "competent" means per AIMS role, with assessment methods and reassessment cadence. Awareness (7.3) is a tiered programme covering everyone-through-role-specific, with completion tracking and interview-defensibility. Communications (7.4) is a documented plan of internal and external communications with audience, content, cadence, channel, owner. Documented information (7.5) is a controlled register of every AIMS artefact with owner, format, location, access, retention, disposition. The architect designs the shapes; the head of AI governance operates them; auditors sample them liberally because Clause 7 is where AIMS reality either survives or fails contact with the workforce. The next chapter walks Clause 9 — performance evaluation, internal audit, and management review — and the operations calendar that the support layer feeds.
