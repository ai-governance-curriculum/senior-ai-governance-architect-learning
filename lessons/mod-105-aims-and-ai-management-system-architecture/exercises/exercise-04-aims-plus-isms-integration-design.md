# exercise-04: AIMS + ISMS integration design

**Estimated effort:** 3 hours

## Objective

Design the **integrated management-system (IMS) shape** that runs the ISO/IEC 42001 AIMS and the enterprise's existing ISO/IEC 27001 ISMS as facets of a single integrated system — one documented-information register, one internal audit programme, one management review, one competence-and-awareness programme, one incident-management process, one nonconformity-and-CAPA process — while preserving the AI-specific requirements the AIMS carries and the information-security-specific requirements the ISMS carries. Produce the IMS charter, the facet map, the shared-apparatus specifications, and the operating-model diagram showing where the CISO and the head of AI governance meet.

The trap this drill teaches you to avoid is the *two-parallel-systems* trap on one side (duplicated artefacts, competing audit programmes, contradictory decisions on shared controls) and the *AIMS-absorbed-into-ISMS* trap on the other (AI-specific requirements collapse into generic security control language, the certification body cannot find the 42001-specific evidence, the AIMS certification fails). The integration is a middle way: shared *shape*, distinct *content* at the facet level.

This is the artefact the level-50 architect designs with the CISO's architectural counterpart. The head of AI governance and the CISO operate the two facets inside the integrated system. Top management commits to the integrated system as a whole under Clause 5 of both standards simultaneously.

## Prerequisites

- Chapter [`10-integrating-the-aims-with-the-iso-27001-isms.md`](../10-integrating-the-aims-with-the-iso-27001-isms.md) read and internalised — this exercise implements what that chapter designs.
- Chapter [`07-support-competence-awareness-communications-and-documented-information.md`](../07-support-competence-awareness-communications-and-documented-information.md) read — the shared support-clause apparatus lands there.
- Chapter [`08-performance-evaluation-internal-audit-and-management-review.md`](../08-performance-evaluation-internal-audit-and-management-review.md) read — the integrated audit programme and management review land there.
- Chapter [`09-non-conformity-corrective-action-and-continual-improvement.md`](../09-non-conformity-corrective-action-and-continual-improvement.md) read — the integrated CAPA process is designed there.
- Exercises 01, 02, and 03 completed — the scope, SoA, and RTP the integration composes with sit there.
- Working familiarity with ISO/IEC 27001:2022 clause structure and Annex A control set. The 27001 SoA shape and Annex A are assumed; you do not re-author them.
- Primary references: ISO/IEC 42001:2023 Annex D (integration with other management systems, if published in the standard); ISO/IEC 27001:2022; Annex SL harmonised structure. Links in [`../resources.md`](../resources.md).

## Scenario

Continue with **Halden Insurance Group** from exercises 01-03. Assume:

- Halden's certified ISO/IEC 27001 ISMS is in its third surveillance cycle. It covers the parent and the UK, German, and US entities. The CISO (Elisabeth Storm) is the ISMS operating owner. The ISMS SoA is stable; the ISMS internal audit programme runs on a two-year cycle; management review is annual with a semi-annual interim review.
- Halden's AIMS is being established (per exercises 01-03) with the head of AI governance (position filled: J. Amiri) as the operating owner and top-management commitment from the Chief Risk and Compliance Officer (CRCO).
- Halden also operates a nascent business-continuity capability (not yet certified against ISO 22301) and a privacy management capability under GDPR that is not organised as a formal ISO/IEC 27701 privacy management system today.
- Halden's ERM function is chaired by the CRCO. The board's Risk and Compliance Committee oversees ERM, ISMS, and (as of this year) AIMS.
- The Halden GRC platform (mod-111 shape) supports multiple management-system facets; the migration to a shared documented-information register can be planned as part of this exercise.

Assume the AIMS is *being brought up alongside* the ISMS — the integration design is what allows both to be certified at the AIMS's initial stage-2 audit without duplicating apparatus.

## Deliverables

1. **`ims-charter.md`** — the two-to-three-page charter of the Halden Integrated Management System.
2. **`facet-map.yaml`** — the facet configuration (facet ids, standards, operating owners, per-facet scope and SoA references, shared apparatus references).
3. **`shared-apparatus-specifications.md`** — the specifications for the six shared-apparatus components (documented-information register; competence-and-awareness programme; internal audit programme; management review; incident management; CAPA and continual improvement).
4. **`operating-model-diagram-and-narrative.md`** — the roles-and-forums diagram plus a one-page narrative walking a reader through how a joint decision gets made.
5. **`integration-audit-defence-brief.md`** — a two-page brief pre-answering the two certification bodies' questions about the integration.

## Requirements

### `ims-charter.md`

Two-to-three pages. Must name:

- **The IMS's name, purpose, and scope.** Halden Integrated Management System, purpose statement, and the scope the IMS itself carries (which is the union of the facet scopes — do not re-scope the ISMS or the AIMS).
- **The facets in the IMS at charter time.** ISMS facet (existing, ISO/IEC 27001:2022), AIMS facet (new, ISO/IEC 42001:2023), and a stated posture on future facets (business continuity against ISO 22301 planned; privacy against ISO/IEC 27701 to be decided). Each facet named with its operating owner, its top-management-committing executive, and its own scope statement reference.
- **The integration principles.** The principles the design commits to: shared *shape*, distinct *content*; no duplicated artefacts; no facet-specific content collapsed into another facet's language; every facet's Annex A controls remain traceable in that facet's SoA; joint decisions made at joint forums with named participation from all affected facets.
- **The governance shape.** The board committee that oversees the IMS (Risk and Compliance Committee at Halden); the executive committing to the IMS (the CRCO as the delegated executive across the ISMS and the AIMS; other facets attach as they arrive); the joint management-review forum.
- **The interfaces to non-facet apparatus.** ERM (parent risk-management framework the facets compose with); legal and compliance (regulatory-obligation interpretation); the enterprise-wide GRC platform.
- **The commitments the CEO / CRCO make** — an explicit sentence-per-facet commitment that the enterprise will maintain the facet's certification, resource the facet operationally, and treat findings from either certification body as findings against the IMS.

### `facet-map.yaml`

Populate the facet map in the shape from chapter 10:

```yaml
integrated_management_system:
  name: Halden Integrated Management System
  charter_ref: DI-IMS-CHARTER-2026
  facets:
    - facet_id: FACET-ISMS
      standard: ISO/IEC 27001:2022
      scope_statement_ref: <doc-id>
      soa_ref: <doc-id>
      risk_treatment_plan_ref: <doc-id>
      operating_owner: chief-information-security-officer
      operating_owner_person: Elisabeth Storm
      certification_state: certified (surveillance cycle 3)
      certification_body: <named or n/a>
    - facet_id: FACET-AIMS
      standard: ISO/IEC 42001:2023
      scope_statement_ref: <doc-id>              # from exercise-01
      soa_ref: <doc-id>                          # from exercise-02
      risk_treatment_plan_ref: <doc-id>          # from exercise-03
      operating_owner: head-of-ai-governance
      operating_owner_person: J. Amiri
      top_management_committer: Chief-Risk-and-Compliance-Officer
      certification_state: pre-certification (stage-1 target Q3 next year)
      certification_body: <named or n/a>
    - facet_id: FACET-BCMS
      standard: ISO 22301:2019
      scope_statement_ref: <planned>
      operating_owner: <chief-operating-officer or delegate>
      certification_state: not-yet-certified
      posture: planned FY2028

shared_apparatus:
  documented_information_register:
    location: Halden GRC platform (integrated)
    owner: chief-compliance-officer
    per_facet_sections:
      - facet_id: FACET-ISMS
        section: /ims/isms/*
      - facet_id: FACET-AIMS
        section: /ims/aims/*
      - facet_id: FACET-BCMS
        section: /ims/bcms/*
  competence_matrix:
    per_role_facet_coverage: yes
    operator: HR business partner + facet operating owners
  awareness_programme:
    tier1_module: joint (annual)
    tier2_modules: facet-specific
    tier3_modules: facet-specific plus role composites
  internal_audit_programme:
    plan_shape: single multi-year programme covering all facets
    lead: internal audit function (third line)
    facet_specialists:
      - facet_id: FACET-ISMS
        auditor_competence: ISMS lead auditor + information-security specialist
      - facet_id: FACET-AIMS
        auditor_competence: AIMS lead auditor + AI-competence specialist (per ISO/IEC 42006)
      - facet_id: FACET-BCMS
        auditor_competence: business-continuity auditor
  management_review:
    forum: IMS Management Review
    cadence: annual + semi-annual interim
    chair: <named>
    per_facet_input_owner: facet operating owner
    per_facet_agenda_time: allocated
    joint_agenda_time: allocated
  incident_management:
    single_process: yes
    classification_carries_facet_flags: yes
    per_facet_reporting_overlay:
      - facet_id: FACET-ISMS
        overlay: information-security notification (per regulator and contractual)
      - facet_id: FACET-AIMS
        overlay: AI-incident notification (per mod-110 shape; EU AI Act Article 73 for high-risk systems)
  capa_process:
    single_process: yes
    per_facet_findings_carry_facet_tag: yes
    escalation_path: through IMS Management Review
```

### `shared-apparatus-specifications.md`

Author the specifications for the six shared-apparatus components. Each specification includes what is shared, what is facet-specific, and how facet-specificity is preserved inside the shared apparatus:

1. **The documented-information register.** Structure of the register (per-facet sections, cross-facet index, shared master document types). How a document that serves both facets (an integrated policy; a shared vendor register) is tagged. Versioning discipline. Retention rules per facet's regulatory context.
2. **The competence-and-awareness programme.** The competence matrix (per-role, per-facet coverage). The awareness tiers (tier-1 all-hands, tier-2 role-based, tier-3 facet-specialist). The programme owner (HR business partner or equivalent) and the facet-specific content owners. Completion tracking and reporting.
3. **The internal audit programme.** A single multi-year plan covering both facets. Auditor-competence expectations per facet (AI competence per ISO/IEC 42006 for the AIMS facet; information-security competence for the ISMS facet; multi-competence for auditors testing shared apparatus). Sampling strategy (per-facet Annex A coverage; shared-apparatus coverage). Reporting and escalation. Interface to the certification bodies (both may attend and observe internal audits under some agreements).
4. **The management review.** The IMS Management Review forum: cadence, chair, standing agenda structure (joint sections + per-facet sections), inputs required (per-facet KPIs, incident summaries, nonconformity summaries, RTP summaries, external-issue-register refresh, interested-parties-register refresh, audit results), outputs (decisions, resource commitments, escalations). The rule the chapter names — each facet's mandatory inputs and outputs stay identifiable even when the review is joint.
5. **The incident management process.** A single incident-management process with facet classification flags. How an incident is triaged (is it an information-security incident? an AI incident? both?), how it is worked, how facet-specific reporting overlays are triggered (Article 73 15-day serious-incident window for AI-Act-high-risk AIMS-scope systems; NIS2 or GDPR reporting for information-security incidents), and how the shared post-incident review feeds both facets' continual-improvement processes.
6. **The CAPA process.** A single nonconformity-and-corrective-action process with per-facet tags. How a finding from an AIMS internal audit produces a CAPA record that references the AIMS SoA row and the AIMS RTP; how a shared-apparatus finding (e.g. a documented-information-register gap) produces a CAPA that touches all affected facets. How CAPA closure evidence is verified independently.

For each specification, name at least one *anti-pattern* the design guards against (e.g. for the awareness programme: the anti-pattern is a joint tier-1 module that never mentions AI-specific content, so the AIMS auditor cannot find AI awareness evidence).

### `operating-model-diagram-and-narrative.md`

A one-page ASCII / mermaid / simple-figure diagram showing:

- The board committee (Risk and Compliance Committee) at the top.
- The CRCO as the top-management-committing executive.
- The CISO (ISMS facet) and the head of AI governance (AIMS facet) as facet operating owners.
- The IMS Management Review forum as a joint forum with both facet owners plus the CRCO plus other named participants (CIO, general counsel, chief compliance officer, internal audit as observer).
- The GRC platform as the shared apparatus.
- The internal audit function as the third-line function serving both facets.
- The line to top management for escalations and commitments.

Below the diagram, a one-page narrative walking a *joint decision* through the operating model. Pick a scenario that touches both facets — for example: a serious hallucination incident on the claims-triage assistant that also involves an information-security control failure (an over-broad access role on the RAG corpus). Walk who first identifies the incident, how it is triaged into both facets, who owns the CAPA, how the fix is implemented in both facet-specific and shared controls, and how the incident feeds the joint management review.

### `integration-audit-defence-brief.md`

Two pages. Pre-answer the questions both certification bodies will ask about the integration:

- **From the AIMS certification body's perspective.** "Where is your Clause 4 documentation for the AIMS? Where is your SoA against ISO/IEC 42001 Annex A? Where is your risk-treatment plan? Show me the AI-specific evidence in your internal audit programme and management review that says the AIMS is being operated, not collapsed into ISMS." Pre-write the answers with pointers to the deliverables from exercises 01-03 and the specifications from this exercise.
- **From the ISMS certification body's perspective.** "How has your ISMS been affected by adding the AIMS facet? Does the ISMS still cover what it did? Are there new information-security risks introduced by AI systems that are now covered by the ISMS SoA and RTP? Are the internal audit programme and management review still testing the ISMS with the required focus?" Pre-write the answers, including any 27001 SoA changes triggered by the AIMS addition (typical: extensions to supplier-relationship, secure-development, monitoring, and incident-management controls to cover AI-specific concerns).
- **The joint questions.** "How do the two certification bodies coordinate? What happens when one body raises a finding that touches both facets? Which body issues the certificate for shared apparatus?" State the enterprise's posture (typically: both bodies certify their own facet against their own standard; shared apparatus is treated as a common resource each body samples in its own way; findings are shared under a joint-audit MOU where the enterprise arranges one).
- **The three integration risks and their mitigations.** Common candidates: (a) the AIMS-absorbed-into-ISMS drift risk (mitigation: named facet inputs and outputs at the management review); (b) the shared-apparatus-gap risk where a fix helps one facet and inadvertently regresses another (mitigation: the CAPA process's cross-facet impact check); (c) the ownership-ambiguity risk on shared vendors and shared incidents (mitigation: the operating-model diagram's clear joint-decision path).

## Starter guidance

- **The integration is *shape*, not *content*.** The chapter-10 pattern preserves the two SoAs against their respective annexes and preserves the two facets' distinct risks and controls. Do not "merge" the SoAs into a joint SoA — the certification bodies audit against their own standards.
- **Do not re-author the ISMS.** The exercise's job is to design the integration; the ISMS scope, SoA, and RTP exist and are stable. Reference them, do not re-author them.
- **Every shared-apparatus component must be *facet-traceable*.** A joint awareness module must still deliver AI-specific content that an AIMS auditor can find. A joint audit programme must still sample AIMS-specific Annex A controls with an AI-competent auditor. Traceability is what preserves each facet's audit-defensibility.
- **Anti-patterns are as important as the design.** For each shared-apparatus component, name at least one drift-into-nothing anti-pattern the design guards against. The auditor tests for the anti-patterns, not just for the presence of the apparatus.
- **The IMS Management Review has mandatory-per-facet content.** The chapter-10 rule is that the joint review is *not permitted* to drop the AIMS-mandatory management-review inputs or outputs. Design the standing agenda so those inputs and outputs are structurally present.
- **The CISO and the head of AI governance are peers, not one-under-the-other.** In many enterprises the CISO's authority predates the AIMS; the temptation is to place the head of AI governance under the CISO. This collapses the AIMS into an ISMS extension and fails the certification. Both facets carry Clause 5 top-management commitment through the same executive; the operating owners are peers.
- **Use `<!-- needs-research: ... -->` for standards-specific claims you cannot verify.** ISO/IEC 42001 Annex D's exact content, ISO/IEC 27001:2022 Annex A specific control identifiers, ISO 22301 clause references — verify or mark.

## Acceptance criteria

- [ ] The IMS charter names both facets with operating owners and committing executives, states the integration principles, names the governance shape, and includes explicit CEO / CRCO commitments per facet.
- [ ] The facet map is complete for the two current facets and states the posture on the two planned / candidate facets. Every facet's scope, SoA, and RTP references exist (or are marked planned with dates).
- [ ] All six shared-apparatus components are specified — what is shared, what is facet-specific, and how facet-specificity is preserved. Each specification names at least one drift-into-nothing anti-pattern the design guards against.
- [ ] The internal audit programme specification names AI-competence expectations per ISO/IEC 42006 for the AIMS facet.
- [ ] The management review specification carries the mandatory-per-facet-input rule and a standing agenda that structurally preserves both facets' Clause 9 inputs and outputs.
- [ ] The incident-management specification names the AI-Act Article 73 15-day serious-incident overlay for the AIMS facet.
- [ ] The operating-model diagram places the CISO and the head of AI governance as peers and names the joint decision forum. The narrative walks a plausible joint incident end-to-end.
- [ ] The audit defence brief pre-answers questions from both certification bodies' perspectives, addresses the two-body coordination question, and names three integration risks with mitigations that map to specific shared-apparatus specifications.
- [ ] The integration composes with exercises 01-03 — the facet map's `scope_statement_ref`, `soa_ref`, and `risk_treatment_plan_ref` fields point to the artefacts you authored there.
- [ ] Every claim about ISO/IEC 42001 Annex D, ISO/IEC 27001 Annex A, or ISO 22301 clause structure is either verifiable or marked `<!-- needs-research: ... -->`.

## Stretch goals

- **Add the *privacy-management-system decision*** — a one-page brief on whether Halden should stand up an ISO/IEC 27701 privacy facet or continue running privacy under the ISMS's data-protection controls. State the trade-offs, the operating-model implications, and your recommendation with rationale.
- **Author the *joint-vendor-assessment procedure*** — the specific procedure for assessing a vendor that supplies both AI capability and information-processing capability (typical: the foundation-model provider that also processes personal data). One page.
- **Sketch the *integrated-CAPA-record shape*** — the YAML shape for a CAPA record that carries facet tags, supports cross-facet impact evaluation, and joins to both facets' SoAs and RTPs. Fifteen-to-twenty lines of YAML.
- **Design a *cross-facet metric*** — one composite KPI that would matter to the IMS Management Review because it touches both facets (candidate: mean-time-to-close on cross-facet incidents; joint-vendor risk-tier distribution). Half a page.
- **Handle the *facet-add procedure*** — the procedure the IMS uses when a new facet is added (BCMS in FY2028). Name the pre-conditions, the charter amendment, the shared-apparatus onboarding, and the audit-programme extension. Half a page.
