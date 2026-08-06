# Integrating the AIMS with the ISO/IEC 27001 ISMS

## Why this chapter exists

Practically every enterprise mature enough to be reading this module already operates an ISO/IEC 27001 Information Security Management System. The ISMS has a scope statement, an SoA (against ISO/IEC 27001 Annex A), a risk-treatment plan, an internal audit programme, a management review, a competence and awareness programme, and a nonconformity and corrective-action process. So does the AIMS. Both were designed to the same Annex SL harmonised structure. If the enterprise stands them up as two independent management systems, the enterprise duplicates every artefact, runs two competing audit programmes, holds two disconnected management reviews, maintains two versions of the same awareness content, and eventually watches the two systems make contradictory decisions on shared controls (encryption, access management, incident response, third-party risk).

The alternative — one integrated management system with an information-security facet, an AI facet, and (usually) other facets over time — is what mature ISO-conformant enterprises actually run. The AI-specific and information-security-specific requirements remain distinct at the *content* level; the *shape* (documentation, audit programme, management review) is shared. Auditors from either regime read the same interlock and sample the same shared artefacts.

This chapter walks how to design the integration. The architect owns the *integration design*; the head of AI governance operates the AIMS facet inside the integrated system; the CISO operates the ISMS facet; a jointly-chaired integrated management-review forum ratifies decisions that affect both.

## The overlap — where the two systems necessarily meet

The AIMS and the ISMS overlap in specific places. Some overlaps are obvious; some are subtle. The architect enumerates them explicitly so the integration design has a clear surface to work on.

**Overlap 1 — information security of AI systems.** AI systems process information. Every AI system in the AIMS scope is also (or should be) an information system in the ISMS scope. ISO/IEC 27001 Annex A controls on access control, cryptography, secure development, operations security, communications security, supplier relationships, and incident management apply to AI systems. Enforcing them twice (once through the ISMS control set and once through an AI-specific rewrite) produces duplication with drift. The integration treats AI systems as ISMS-scope information systems *and* as AIMS-scope AI systems, with a joint SoA row indicating both regimes' inclusion.

**Overlap 2 — data protection intersecting both systems.** Data used to train, evaluate, and operate AI systems is often personal data. It is subject to the enterprise's data-protection controls (typically operated under a Privacy Management System or as part of the ISMS's data-protection controls, depending on how the enterprise is organised) *and* to AI-specific data-governance controls under 42001. Both systems need to know about the data; the enterprise designs one joint data-governance layer that both systems reference.

**Overlap 3 — third-party risk.** AI providers (foundation-model vendors, AI SaaS providers) are third parties. The ISMS carries a third-party-risk programme under Annex A supplier controls; the AIMS carries an AI-specific third-party governance programme (mod-109). The two must not duplicate the vendor register, the risk assessment, or the ongoing monitoring; they compose so that AI providers are assessed once under an integrated third-party programme that carries both information-security and AI-specific dimensions.

**Overlap 4 — incident management.** The ISMS has an incident-management process (Annex A). The AIMS references its own incident process for AI-specific incidents (mod-110). AI incidents often *are* information-security incidents (a training-data leak; a prompt-injection attack; an adversarial-example attack) and vice versa. One incident-management process serves both, with the AI-specific classification and reporting overlaid.

**Overlap 5 — governance context.** The board committee that oversees information security is often the same committee that oversees AI (or a jointly-attended pair). The CISO and the head of AI governance report into overlapping structures. The Clause 5 leadership commitment for both systems typically comes from the same executive body.

**Overlap 6 — competence and awareness.** ISMS awareness modules and AIMS awareness modules can share delivery infrastructure, tracking, and completion reporting. Competence definitions overlap for people (security engineers who also have AI competence; auditors with both ISMS and AIMS competence).

## The integrated management-system design pattern

The design pattern the architect works to has one integrated management system with multiple *facets* — an ISMS facet against ISO/IEC 27001, an AIMS facet against ISO/IEC 42001, and often others (business-continuity facet against ISO 22301, privacy facet against ISO/IEC 27701 where the enterprise adopts it). Each facet carries its own scope statement, its own SoA (against its own annex), and its own facet-specific controls. Shared apparatus — documented information register, competence matrix, awareness programme, communications plan, internal audit programme, management review, nonconformity and CAPA process — is operated once.

**A worked instance — Northbrook Medical Group.**

```yaml
integrated_management_system:
  name: Northbrook Integrated Management System
  facets:
    - facet_id: FACET-ISMS
      standard: ISO/IEC 27001:2022
      scope_statement: DI-IMS-SCOPE-ISMS
      soa: DI-IMS-SOA-ISMS
      operating_owner: chief-information-security-officer
    - facet_id: FACET-AIMS
      standard: ISO/IEC 42001:2023
      scope_statement: DI-IMS-SCOPE-AIMS
      soa: DI-IMS-SOA-AIMS
      operating_owner: head-of-ai-governance
    - facet_id: FACET-BCMS
      standard: ISO 22301:2019
      scope_statement: DI-IMS-SCOPE-BCMS
      soa: n/a (22301 shape)
      operating_owner: chief-operating-officer (delegated to bcm-lead).

shared_apparatus:
  documented_information_register:
    location: enterprise GRC platform (integrated).
    owner: chief-compliance-officer (integrated owner).
    per_facet_sections:
      - facet_id: FACET-ISMS
        section: /ims/isms/*
      - facet_id: FACET-AIMS
        section: /ims/aims/*
      - facet_id: FACET-BCMS
        section: /ims/bcms/*
  competence_matrix:
    per_role_facet_coverage: yes
    operator: enterprise HR + facet operating owners.
  awareness_programme:
    tier1_module: joint (covers all facets at high level).
    tier2_modules: facet-specific plus role-specific composites.
    tier3_modules: facet-specific.
  internal_audit_programme:
    plan_shape: single multi-year programme covering all facets.
    lead: internal audit function (third line).
    facet_specialists: named per facet (AI-competence auditor for
      AIMS; security-competence auditor for ISMS; bc-competence
      auditor for BCMS).
  management_review:
    forum: integrated management-system management review.
    cadence: annually, with semi-annual interim reviews.
    chair: designated by CEO; typically CFO or Chief Risk
      Officer for enterprise-level view.
    inputs_per_facet: each facet provides its Clause 9.3 package.
    outputs: decisions may affect one facet, several, or the
      integrated shape.
  nonconformity_and_capa:
    register: single integrated register.
    classification_extension: CAPA record has a facet tag so
      per-facet trend reporting is possible.
```

The integration is *composable* — new facets can be added over time without re-architecting the management system. This is how enterprises that reach ISO/IEC 42001 certification after already holding ISO/IEC 27001 certification most efficiently extend their programme.

## Shared documentation — how to do it without collapsing distinctions

Shared documentation is where the integration produces the most efficiency and the most risk. Efficiency: one competence matrix instead of two. Risk: collapsing genuinely distinct AIMS content into ISMS templates that erase the AI specifics.

**Rules the architect applies.**

- *One documented-information register, per-facet sections.* Every artefact is filed once, in its facet's section, with cross-references where relevant. The register is one; the artefacts are per facet.
- *Shared apparatus artefacts are jointly owned.* The competence matrix, the awareness programme, the audit programme, the CAPA register — these have integrated owners who serve both facets and are answerable to both operating owners.
- *Facet-specific artefacts remain per facet.* The AIMS scope statement is not the ISMS scope statement, even if they cover overlapping systems. The AIMS SoA is not the ISMS SoA; they walk different annexes. The AI policy is not the information-security policy.
- *Overlap artefacts get a specific integration shape.* Where an artefact addresses both facets (e.g. an integrated incident-management procedure), the artefact carries clauses tagged to their originating facet so auditors from either regime can walk it and find the parts that satisfy their standard.
- *Version and change control operate at the artefact level*, not at the facet level. When an integrated procedure changes, the change is recorded once; per-facet impact is annotated in the change record.

## Shared audit programme

The internal audit programme is one of the biggest integration wins.

**One audit universe.** The audit function maintains a single multi-year audit universe covering all facets. Facet-specific coverage requirements are constraints (e.g. every AIMS Annex A control audited at least once per three-year cycle) but the plan optimises across facets. An audit round that samples a system's Clause 8 operational controls under AIMS can simultaneously sample the same system's ISMS control set — one visit, two audit outputs.

**Shared audit standards.** The audit function uses a single audit-standard set (independence, sampling, reporting, follow-up) with facet-specific competence overlays. Findings are classified with a facet tag so per-facet reporting to the operating owners and to the management review remains distinct.

**Integrated findings register.** Findings feed the integrated CAPA register with facet tags. Where a finding straddles facets — for example, a shared incident-management gap — the finding is dual-tagged and the corrective action addresses both.

## Shared management review

The integrated management review is the forum where top management sees the whole management system at once.

**Cadence.** Typically annually with a semi-annual interim review. Some enterprises run quarterly integrated reviews for very active programmes; the cadence should be consistent with each facet's individual Clause 9.3 expectation.

**Chair.** Designated by the CEO. Often the CFO, the chief risk officer, or the chief compliance officer — someone whose portfolio spans the facets. Where the enterprise has a chief AI officer at level 70, that officer participates as an AIMS-facet stakeholder rather than as chair (otherwise the review becomes AI-centred and information-security and business-continuity are second-class).

**Inputs.** Each facet's operating owner presents their facet's Clause 9.3 package. Shared apparatus (documented information, competence, awareness, audit, CAPA) has an integrated section. The integrated view surfaces cross-facet themes.

**Outputs.** Decisions may affect one facet, several, or the integrated shape. Every decision has a facet tag; actions land against facet-specific or integrated ownership.

**Minutes.** Filed once in the integrated documented-information register; each facet has visibility of decisions affecting it.

## Cross-facet decisions — where to be careful

Some decisions genuinely need to be made at the integrated level, and getting them wrong is where integration goes off the rails.

**Decision 1 — the AI system that changes ISMS classification.** Deploying a GenAI product changes the enterprise's information-security posture — new attack surface, new data flows, new third-party dependencies. The ISMS Clause 6 planning needs to see the change; the AIMS is where the change originates. The integration design defines a specific interface — new-AI-system additions to the AIMS scope trigger an ISMS-scope-review event.

**Decision 2 — the ISMS control decision that constrains AI.** The ISMS decides to require data classification labels on all data at rest. The AIMS's data-governance controls need to inherit the classification into training-data provenance records. The integration design defines the interface — ISMS-scope Annex A control changes trigger an AIMS-side impact review.

**Decision 3 — the incident that spans both.** A prompt-injection attack that exfiltrates training data through model outputs is *both* an information-security incident and an AI incident. The integrated incident-management process handles it once, classifies it under both facets, reports to both facets' regulatory pipelines, and produces one CAPA that satisfies both facets' corrective-action requirements.

**Decision 4 — the third-party relationship that touches both.** A foundation-model provider is both an information-security supplier and an AI-provider. The integrated third-party programme assesses them once, with facet-specific control coverage overlaid, and the ongoing monitoring feeds both facets. Mod-109 walks the specifics.

## Where the integration must *not* collapse the AIMS

Integration efficiency has a limit. The following AIMS-specific properties cannot be collapsed into ISMS shape without breaking 42001 conformance:

- **The AI policy is not the information-security policy.** The AI policy carries AI-specific commitments (protection of individuals and groups affected by AI, responsible-AI principles, AI-lifecycle commitments) that the information-security policy does not. Merging them produces an information-security policy with an AI paragraph — insufficient for 42001 Clause 5.2.
- **The AIMS SoA is not the ISMS SoA.** They walk different annexes. Merged SoAs fail conformance in both directions.
- **The AI risk process is not the ISMS risk process.** The AIMS risk process composes 31000 with 23894 with 42005 (chapter 04). The ISMS risk process composes 31000 with 27005 with the enterprise's information-security risk methodology. Shared *framework* (31000 at the parent layer) is correct; shared *specialised guidance* is wrong.
- **The AI impact assessment is not an information-security risk assessment.** 42005 AIA addresses foreseeable impacts on individuals, groups, and society. ISMS risk assessments address information-security risk. They serve different purposes; collapsing them loses the AI-impact focus.
- **The AI-specific Annex A controls have no ISMS analogue.** Controls on AI transparency, on AIA process, on AI system verification and validation, on AI-specific incident classification exist in 42001 Annex A and not in 27001 Annex A. The integrated management system carries them under the AIMS facet.

The architect's discipline is knowing where integration produces efficiency and where it produces error. Documented information register, competence, awareness, audit programme, management review, CAPA — integration wins. Policy, scope, SoA, risk process, AIA, AI-specific controls — facet remains.

## Two failure modes

**Failure mode 1 — the AIMS as ISMS annex.** The enterprise "integrates" by adding an AI paragraph to the information-security policy, a couple of AI-specific rows to the ISMS SoA, and calling the AIMS "the AI part of the ISMS". The result satisfies neither 42001 (the AI-specific clauses are unaddressed at the shape level) nor the enterprise's own AI-governance ambition (the AI-specific work is subordinated to information-security priorities). The 42001 certification-body auditor will find the AIMS clauses missing within an hour. The fix is respecting the facet distinction — the AIMS gets its own scope, SoA, policy, risk process, and AIA; the shared apparatus is what integrates.

**Failure mode 2 — the parallel AIMS and ISMS with no integration.** The enterprise stands up a completely independent AIMS with duplicated everything. Six months later the two systems make contradictory decisions on shared controls (the AIMS mandates one encryption approach; the ISMS mandates another; teams asked to do both cannot). Audits take twice as long; management reviews contradict each other; the enterprise's ability to speak with one voice on integrated topics (third-party governance, incident response) is broken. The fix is design-time integration — do not stand up an independent AIMS; stand up an AIMS facet inside an integrated management system.

## Summary

Enterprises with an existing ISO/IEC 27001 ISMS should stand up their ISO/IEC 42001 AIMS as an *integrated facet*, not as a parallel management system. The integration shares the documented-information register, the competence matrix, the awareness programme, the communications plan, the internal audit programme, the management-review forum, and the nonconformity-and-CAPA process; it keeps distinct the AI policy, the AIMS scope statement, the AIMS SoA, the AI risk process, the AIA process, and the AI-specific Annex A controls. Cross-facet decisions have defined interfaces so an AIMS scope change triggers an ISMS review, an ISMS control change triggers an AIMS impact review, and cross-facet incidents flow through one handling process. The two failure modes to avoid — collapsing the AIMS into an ISMS annex, or running two disconnected systems — are prevented by the architect's discipline at design time. The next and final chapter walks ISO/IEC 42006 in reverse — reading the auditor's rulebook to design an AIMS that a third-party certification body can audit.
