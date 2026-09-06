# The US federal agency contractor blueprint — OMB M-25-21 / M-25-22, FedRAMP, and the Chief AI Officer designation as the composition anchor

## Why this chapter exists

The level-50 architect at a US federal agency contractor inherits a governance frame that does not sit on the same axis as ISO/IEC 42001. The contractor does not choose whether its AI ships; the customer agency does. The determination is made by the agency's Chief AI Officer (CAIO) under the OMB memoranda that govern federal AI use, adjudicated through the agency's CAIO Council process, and predicated on the FedRAMP authorisation status of the platform the AI runs on. The contractor's programme is therefore *composed downstream* of an authorisation the contractor itself does not issue — a shape none of the sector chapters preceding this one has had to accommodate at this depth.

The chapter's proposition is that the **composition anchor for a US federal agency contractor is the agency-side CAIO designation plus the FedRAMP authorisation of the delivery boundary**, not the contractor's own AIMS. The AIMS the contractor already operates (mod-105) is the framework the federal-programme extends *into*, and the reference architecture instantiations covered in mod-101 through mod-112 remain valid — but the sector-specific reality is that a mod-107 pre-deployment gate result of *approved* means nothing if the agency CAIO has not accepted the minimum-practices attestation, and no minimum-practices attestation matters if the underlying platform lacks an Authority to Operate (ATO). The failure mode this chapter designs against is the one where the contractor treats OMB M-25-21 and M-25-22 as *just another compliance overlay* — a checklist to append to the enterprise AIMS. That framing misses that the OMB memoranda flow through the customer agency to the contractor via contract clauses, that the flow-down obligations are enforceable through the Contracting Officer (CO) / Contracting Officer's Representative (COR) chain rather than through the contractor's own governance, and that the contractor's product ships or does not ship on the agency's determination — not the contractor's.

This chapter fills in the chapter-01 sector-adaptation record (see `01-the-sector-adaptation-methodology.md`) for the US-federal-agency-contractor sector. It reads the anchor obligations (OMB M-25-21, OMB M-25-22, FedRAMP, NIST SP 800-53 Rev. 5, NIST AI RMF, NIST AI 600-1, US AISI methodology, Section 508, FISMA, the E-Government Act privacy-impact-assessment regime), maps them onto the mod-101 through mod-112 architecture, and names the reserved-matters overlay the mod-112 council carries.

## OMB M-25-21 — the federal-use governance frame

**OMB Memorandum M-25-21 — "Accelerating Federal Use of AI through Innovation, Governance, and Public Trust"** was issued on 3 April 2025 <!-- needs-research: verify exact issuance date, full title, and superseding relationship with M-24-10 --> as the current OMB governance memorandum on federal agency AI use. It supersedes the earlier M-24-10, which was issued under a different administration and carried a different framing of the same underlying discipline. <!-- needs-research: confirm that M-25-21 supersedes M-24-10 in the operative sense; some provisions may be reissued unchanged. -->

The memorandum operates on four architectural moves the contractor's programme must compose against.

**The CAIO designation.** M-25-21 requires each covered agency to designate a Chief AI Officer with agency-wide authority over AI governance. The CAIO is the accountable executive for the agency's AI risk posture and is the seat that determines whether an AI use case may proceed. The contractor does not have a direct governance interface with the agency CAIO — the CO / COR is the contractual interface, and the CAIO's determinations reach the contractor through that channel. The architectural implication is that the contractor's own AI-accountable executive (per mod-105 Clause 5.3) operates *one seat removed* from the decision that governs deployment; the contractor's role is to produce evidence and attestations that let the agency CAIO make the determination, not to make the determination itself.

**The CAIO Council mechanism.** M-25-21 establishes a federal-government-wide CAIO Council that convenes agency CAIOs and issues cross-agency guidance, shared practices, and coordination on high-impact use cases that cross agency boundaries. The contractor whose product is deployed across multiple agencies faces the possibility that its use case is reviewed at the Council — an escalation the contractor does not initiate and typically does not attend. The programme therefore must be able to produce a *Council-readable pack* on demand: the minimum-practices attestation, the impact assessment, the evaluation record, the ATO status, and the incident history for the deployment across the affected agencies.

**Minimum risk-management practices for high-impact AI and rights-impacting AI.** M-25-21 defines categories of use cases — high-impact AI (systems whose output has significant consequences for the agency's mission or the public) and rights-impacting AI (systems whose output has legal or similarly significant effect on individuals' rights) — and imposes minimum practices on each. The practices span pre-deployment testing, ongoing monitoring, human oversight, and (for rights-impacting AI) additional consent, notice, appeal, and opt-out obligations. <!-- needs-research: verify the exact categorical language and the specific minimum practices enumerated in the current M-25-21 text; the categories carry forward from M-24-10 with revisions. -->

**AI Use Case Inventory obligation.** M-25-21 continues the AI Use Case Inventory obligation the earlier memoranda established — each covered agency publishes an inventory of its AI use cases annually, with defined attributes. The contractor whose product powers a use case appears in the inventory (typically at the level of the use case rather than the underlying product); the contractor's obligation is to provide the agency with the metadata the inventory requires and to notify the agency of material changes that affect the inventory entry.

**Flow-down expectations to the contractor.** The memorandum's obligations attach to the agency, not directly to the contractor. They flow to the contractor through the contract — through FAR clauses, agency supplement clauses, and any AI-specific clauses M-25-22 requires. The architect's task is to make the contractor's programme *readable* against the flow-down: every M-25-21-derived obligation the contract carries has a corresponding evidence-contract line item in mod-108 keyed by the obligation identifier, so the contract-compliance question the CO asks has an artefact answer the contractor can produce without a bespoke retrieval.

## OMB M-25-22 — the acquisition frame

**OMB Memorandum M-25-22 — "Driving Efficient Acquisition of Artificial Intelligence in Government"** was issued alongside M-25-21 on 3 April 2025 <!-- needs-research: verify exact issuance date and full title --> and supersedes M-24-18. Where M-25-21 governs the agency's *use* of AI, M-25-22 governs the agency's *acquisition* of AI — the procurement discipline, contract-clause expectations, and vendor-management shape that determine what the contractor sees in the contract.

M-25-22 operates on four architectural moves the contractor's programme composes against.

**Performance-based acquisition.** M-25-22 encourages performance-based contracting for AI — the agency specifies the outcome (accuracy, latency, availability, fairness thresholds where applicable) and the contractor is measured against the outcome rather than a prescribed implementation. The contractor's programme therefore must produce *outcome-evidence* at the cadence the contract specifies: the mod-108 evidence architecture serialises the performance record, and the mod-110 post-market surveillance function feeds the ongoing measurements the outcome-based clause requires.

**AI-specific contract clauses.** M-25-22 authorises (and in some cases requires) contract clauses covering AI-specific matters — evaluation-result disclosure, training-data provenance, model-behaviour-change notification, prohibitions on certain data uses, and reporting on serious incidents. The contractor's mod-109 third-party programme reads these clauses as the *upstream* mirror of the clauses the contractor itself imposes on its providers — the contractor is the third party from the agency's perspective, and the clauses M-25-22 authorises are the ones the contractor's provider clauses should be structured to satisfy transitively.

**Vendor-lock-in mitigation.** M-25-22 requires acquisition planning to consider vendor-lock-in — data portability, model portability, exit provisions, and continuity planning. The contractor whose product is architecturally locked to a specific foundation-model provider carries a vendor-lock-in risk the agency must consider at award; the contractor's programme should be able to produce a *portability posture* document on demand, cross-referenced to the mod-109 exit-planning artefacts.

**Flow-down obligations.** M-25-22 authorises the agency to flow down obligations to the contractor's subcontractors — evaluation-disclosure obligations, incident-notification obligations, and material-change notification obligations. The contractor's mod-109 provider register must therefore track *flow-down applicability* per provider: which contractor-agency obligations flow through to which provider, at what threshold, with what contract mechanism. A provider onboarded without the M-25-22-derived flow-down clauses is a gap the contractor's programme carries to the next agency-side supplier-risk review.

## FedRAMP — the platform authorisation frame

**FedRAMP (Federal Risk and Authorization Management Program)** is the government-wide programme that standardises security assessment, authorisation, and continuous monitoring for cloud products and services used by federal agencies. A contractor whose AI runs on a cloud service — which is nearly every contractor — encounters FedRAMP as a gate on *the platform the AI runs on*, and the platform's authorisation status determines whether the AI can be deployed for federal customers at all.

FedRAMP operates on three baselines (Low, Moderate, and High), keyed to the FIPS 199 impact-level categorisation of the information the system processes. A contractor's product typically inherits its baseline from the customer agency's determination of the sensitivity of the data the product will handle: a public-facing citizen-services chatbot may sit at Low; an internal case-management AI processing personally identifiable information sits at Moderate; a national-security-adjacent application sits at High. The baseline determines the FedRAMP control set applicable and, consequently, the depth of the assessment.

**Two authorisation paths** exist. **JAB authorisation** (Joint Authorization Board) is granted by a board comprising DoD, DHS, and GSA representatives and produces an authorisation valid across the federal government subject to individual agency acceptance. <!-- needs-research: verify current JAB composition and whether the JAB path remains active under FedRAMP 20x programme changes. --> **Agency authorisation** is granted by a single sponsoring agency and produces an authorisation the sponsoring agency accepts and other agencies may reuse. Both paths produce the same core artefacts.

**The core FedRAMP artefacts** are the ones the contractor's evidence architecture must be able to produce and maintain.

- **System Security Plan (SSP)** — the comprehensive documentation of the system, its boundary, its architecture, and the implementation of every applicable NIST SP 800-53 control. The SSP is the single largest artefact the contractor produces; the mod-108 evidence architecture treats it as a first-class artefact with its own schema, versioning, and reissuance cadence.
- **Security Assessment Report (SAR)** — the independent assessment of the SSP performed by an accredited Third-Party Assessment Organisation (3PAO). The SAR is the assessor's evidence-of-implementation and identification of findings; the contractor does not author the SAR (the 3PAO does) but stores it as evidence in the mod-108 architecture with a provenance edge to the assessing 3PAO.
- **Plan of Action and Milestones (POA&M)** — the tracked record of findings from the SAR and from continuous monitoring, with owner, target remediation date, and status. The POA&M is a live artefact — updated monthly at minimum, with material findings triggering agency notification.

**Continuous monitoring** is FedRAMP's ongoing discipline. The authorised system submits monthly vulnerability scan results, monthly POA&M updates, annual reassessment, and event-triggered reporting on significant changes. The contractor's mod-110 post-market surveillance function composes with the FedRAMP continuous monitoring stream — one PMS pipeline, extended attributes to satisfy FedRAMP CONMON, rather than two parallel surveillance regimes.

**FedRAMP 20x** is the current modernisation programme intended to accelerate authorisations and reduce the administrative burden. <!-- needs-research: verify the current shape and scope of FedRAMP 20x — the programme has evolved substantially and any specific claim about its provisions should be checked against current GSA / FedRAMP PMO publications. --> The architect's near-term posture is to author the evidence architecture to the current FedRAMP baseline and instrument it so that the 20x programme's shifts — greater automation, machine-readable SSPs, continuous authorisation — can be adopted without a re-architecture.

## NIST SP 800-53 Rev. 5 — the underlying control catalog

The FedRAMP baselines are subsets of **NIST SP 800-53 Rev. 5 — "Security and Privacy Controls for Information Systems and Organizations"**, the catalog of security and privacy controls the federal government treats as the reference security-controls library. The mod-102 catalog (the enterprise's single AI-nexus control library) does not replace SP 800-53; it *composes with* it, and the composition is one of the more architecturally delicate moves the contractor's programme executes.

The composition shape is that the mod-102 catalog entries which touch a FedRAMP-in-scope system carry a **crosswalk edge** to the SP 800-53 control families the entry inherits from or extends. The families that recur most often for AI-nexus controls are AC (Access Control), AU (Audit and Accountability), IA (Identification and Authentication), RA (Risk Assessment), SA (System and Services Acquisition), SI (System and Information Integrity), and SR (Supply Chain Risk Management). An AI-nexus control on model-behaviour monitoring inherits from SI (integrity) and RA (risk assessment); an AI-nexus control on training-data lineage inherits from SA (services acquisition) and SR (supply chain); an AI-nexus control on evaluation-record retention inherits from AU (audit).

The architectural principle is that the mod-102 entry is the *AI-nexus enrichment* on top of the SP 800-53 control — the AI-specific attributes, evidence-contract line items, and applicability filters — and the SP 800-53 control is the *security-and-privacy substrate* the enrichment presumes. The FedRAMP SSP renders the SP 800-53 implementation; the mod-108 evidence for the mod-102 entry renders the AI-specific extension. Both point to the same underlying implementation from opposite ends of the catalog.

**NIST AI RMF and NIST AI 600-1 as the AI-specific overlay.** SP 800-53 does not cover the AI-specific risks that the mod-106 taxonomy names — evaluation, safety, fairness, and third-party foundation-model behaviour — with the fidelity a level-50 architect needs. **NIST AI Risk Management Framework (AI RMF 1.0)** and **NIST AI 600-1 — "AI RMF Generative AI Profile"** are the AI-specific overlay the contractor's programme composes onto the SP 800-53 substrate. <!-- needs-research: verify the current publication status of AI 600-1 and any successor generative-AI profile; NIST's generative-AI publications have evolved. --> The RMF's GOVERN / MAP / MEASURE / MANAGE functions map onto the mod-105 AIMS Clause structure and the mod-107 assurance architecture directly; the 600-1 profile's generative-AI-specific practices map onto the mod-102 catalog entries covering foundation-model use, prompt-injection defence, and content-provenance handling.

The architect authors the mod-102 catalog so that a FedRAMP-in-scope AI system has one implementation per control that satisfies SP 800-53 (rendered in the SSP), NIST AI RMF (rendered in the AI RMF profile the programme maintains), and the enterprise's mod-102 catalog (rendered in the SoA). Three renderings of one implementation is the correct posture; three implementations of one control is the two-programme failure mode.

## US AI Safety Institute methodology

The **US AI Safety Institute (US AISI)**, housed within NIST, publishes evaluation methodology and (via voluntary agreements) collaborates with frontier-AI-model developers on pre-deployment evaluation. <!-- needs-research: verify the current organisational status of US AISI and the current state of the voluntary agreements framework; the institute's mandate and specific publications have evolved. --> A contractor whose product embeds a frontier model — a large language model, image-generation model, or multimodal foundation model — indirectly inherits evaluation-disclosure obligations that the voluntary agreements the frontier lab has entered impose. The evaluation results the lab produces under an AISI voluntary agreement are typically provider-facing artefacts; the contractor's mod-109 provider onboarding pack should require attestation that the provider participates in the applicable evaluation regime and that the provider will disclose material findings that affect the contractor's deployment.

The architectural move is that the US AISI methodology composes into the mod-107 pre-deployment gate as a *provider-side evaluation input*, not a contractor-side evaluation the contractor itself performs. The contractor's own evaluation is contractor-scope (behaviour of the composed system in the contractor's deployment); the AISI-adjacent evaluation is provider-scope (behaviour of the underlying model as the provider evaluates it). The mod-108 evidence architecture records both with distinct provenance edges so the contractor's programme can answer, at any point, *what does the underlying model's provider-side evaluation say about this risk?* and *what does our contractor-side evaluation say about the composed system's exposure to this risk?* Two questions, two artefacts, one integrated evidence record.

## The Chief AI Officer designation — agency-side and contractor-side

The agency CAIO designation M-25-21 mandates is a governance seat inside the customer agency. The contractor's programme composes against it — the CAIO is the ultimate decision-maker on whether the contractor's product proceeds to deployment for that agency — but the contractor's programme does not have a direct governance line to the CAIO. The interface is contractual, and the CO / COR is the intermediary.

The design question the level-50 architect answers is *whether the contractor designates its own federal-programme CAIO or ATO focal*. Three postures are defensible.

**Posture A — no dedicated seat.** The contractor's existing AI-accountable executive (per mod-105 Clause 5.3) carries the federal-programme responsibility as an extension of their enterprise mandate. This is the correct posture for a contractor whose federal business is small relative to enterprise scale and whose federal deployments are within the enterprise AIMS scope without material extension. The mod-105 role packet is unchanged; the mod-112 council adds a federal-programme standing item to its agenda.

**Posture B — a federal-programme CAIO or head-of-federal-AI-programme role.** The contractor designates a dedicated seat whose mandate is the federal programme end-to-end — contract-clause compliance, agency-CAIO interface preparation, ATO maintenance, POA&M ownership, and Use Case Inventory data-provider role for each customer agency. This is the correct posture when federal business is material and the coordination burden across agencies warrants a dedicated seat. The mod-105 role packet acquires a new role identifier; the mod-112 council seats the federal-programme CAIO as a voting member or a permanent-observer depending on materiality.

**Posture C — an ATO focal per platform.** For contractors whose federal business runs on a small number of platforms with ATO status, a *platform-scoped ATO focal* is designated per platform. Each focal is accountable for the ATO of their platform — the SSP, the POA&M, the continuous-monitoring stream, and the reassessment cycle. The role composes with (rather than replaces) an enterprise-scope federal-programme lead where one exists.

The **federal CAIO Council interface** — the cross-agency forum M-25-21 establishes — is not an interface the contractor participates in directly. Where a Council conversation about the contractor's product occurs, the affected agency CAIO carries the contractor's position via the CO / COR channel; the contractor's role is to prepare the material the agency CAIO uses. The architectural implication is that the contractor's federal-programme CAIO (or the enterprise AI-accountable executive playing that role) must be able to produce, at short notice, an integrated pack covering every deployment across every affected agency — a coordination artefact the mod-108 evidence architecture must be authored to render.

## The sector adaptation record — US federal agency contractor

Filling in the schematic chapter 01 of this module defines, the US-federal-agency-contractor sector adaptation record reads:

```yaml
sector_adaptation_record:
  id: SAR-US-FED-CONTRACTOR-v1.0
  ratified_by: ai-governance-council (mod-112 ch 01) on <ratification-date>
  reference_architecture_version:
    control_catalog: mod-102 catalog v<X.Y>
    reference_profile: enterprise-reference-profile v<X.Y>
    aims_scope_statement: mod-105 ch 04 statement v<X.Y>
    risk_taxonomy: mod-106 taxonomy v<X.Y>
    obligation_register: mod-104 register v<X.Y>

  sector:
    identifier: us-federal-agency-contractor
    scope_narrative: |
      the enterprise delivers AI-enabled products and/or services to US
      federal customer agencies, whether as a prime or subcontractor, and/or
      operates as a technology supplier to federal agencies whose deployment
      of the enterprise's platform is FedRAMP-in-scope. The scope includes
      civilian-agency, defence, and intelligence-community customers where
      the enterprise is engaged; classification-scoped systems are called
      out separately in the AIMS scope addendum.

  composition_anchor: |
    agency-side CAIO designation (M-25-21) + FedRAMP authorisation of
    the delivery platform. The enterprise AIMS extends into the federal-
    programme space; the federal-programme space does not extend into the
    AIMS.

  anchor_regulations:
    horizontal:
      - nist-ai-rmf-1.0 (Govern / Map / Measure / Manage functions)
      - nist-ai-600-1 (generative-AI profile — verify current publication)
    sector_specific:
      - id: omb-m-25-21
        title: Accelerating Federal Use of AI through Innovation, Governance, and Public Trust
        jurisdiction: US-federal
        supervisor: OMB (Office of Management and Budget)
        instrument_type: OMB-memorandum
        obligation_register_ids: [ OBL-USFED-M-25-21-... ]
        # supersedes: omb-m-24-10 <!-- needs-research: verify -->
      - id: omb-m-25-22
        title: Driving Efficient Acquisition of Artificial Intelligence in Government
        jurisdiction: US-federal
        supervisor: OMB
        instrument_type: OMB-memorandum
        obligation_register_ids: [ OBL-USFED-M-25-22-... ]
        # supersedes: omb-m-24-18 <!-- needs-research: verify -->
      - id: fedramp
        title: Federal Risk and Authorization Management Program
        jurisdiction: US-federal
        supervisor: GSA (FedRAMP PMO) + sponsoring agency or JAB
        instrument_type: authorisation-programme
        obligation_register_ids: [ OBL-USFED-FEDRAMP-... ]
      - id: nist-sp-800-53-r5
        title: Security and Privacy Controls for Information Systems and Organizations
        jurisdiction: US-federal
        supervisor: NIST (catalog) / FedRAMP (baseline selection)
        instrument_type: control-catalog
        obligation_register_ids: [ OBL-USFED-800-53-... ]
      - id: fisma
        title: Federal Information Security Modernization Act
        jurisdiction: US-federal
        supervisor: OMB / DHS-CISA
        instrument_type: statute
        obligation_register_ids: [ OBL-USFED-FISMA-... ]
      - id: section-508
        title: Section 508 of the Rehabilitation Act (accessibility)
        jurisdiction: US-federal
        supervisor: GSA / US-Access-Board
        instrument_type: statute
        obligation_register_ids: [ OBL-USFED-508-... ]
      - id: e-gov-pia
        title: E-Government Act Section 208 Privacy Impact Assessment obligation
        jurisdiction: US-federal
        supervisor: OMB (governance) / agency privacy officer (issuance)
        instrument_type: statute
        obligation_register_ids: [ OBL-USFED-PIA-... ]
      - id: us-aisi-methodology
        title: US AI Safety Institute evaluation methodology (voluntary-agreement-mediated)
        jurisdiction: US-federal
        supervisor: NIST / US AISI
        instrument_type: methodology + voluntary-agreements
        obligation_register_ids: [ OBL-USFED-AISI-... ]
        # <!-- needs-research: verify current AISI publications and voluntary-agreement shape. -->

  profile:
    id: PROFILE-US-FED-CONTRACTOR-v1.0
    derives_from: enterprise-reference-profile v<X.Y>
    delta_summary: |
      selects the reference-catalog controls that touch federal-scope
      systems as mandatory (rather than applicability-filter conditional);
      adds a small number of sector-specific catalog entries covering
      SSP-authoring, POA&M-maintenance, ATO-material-change notification,
      and AI-Use-Case-Inventory contribution as first-class artefact-
      producing controls. Parameter tuning centres on retention (federal-
      records retention overrides shorter enterprise defaults) and
      evidence-cadence (FedRAMP continuous monitoring imposes monthly
      minima on several controls).
    applicability_filter_overrides:
      - dimension: fedramp-baseline
        added_values: [ low, moderate, high ]
        rationale: |
          the FedRAMP baseline of the platform hosting the AI system
          governs the applicable SP 800-53 control set and the CONMON
          cadence; the applicability filter must express the baseline
          per system so the correct control set is composed.
      - dimension: fips-199-impact-level
        added_values: [ low, moderate, high ]
        rationale: |
          the impact-level categorisation of the information the system
          processes drives the FedRAMP baseline selection and independently
          governs several NIST SP 800-53 control tailoring decisions.
      - dimension: m-25-21-category
        added_values: [ high-impact-ai, rights-impacting-ai, neither ]
        rationale: |
          M-25-21 imposes different minimum practices on high-impact AI
          and rights-impacting AI (with some systems in both categories);
          the applicability filter must express the categorisation so the
          minimum-practices attestation contract selects the correct
          artefact set.
      - dimension: security-classification-scope
        added_values: [ unclassified, cui, secret, top-secret ]
        rationale: |
          the classification level of the environment the AI system
          operates in governs a substantial set of security controls and
          entirely bounds the assurance path (a Top-Secret deployment is
          not governed by FedRAMP but by the corresponding IC / DoD
          authorisation regime); the filter must carry the classification
          so the correct authorisation path is engaged.
        # <!-- needs-research: verify the IC / DoD authorisation regime nomenclature
        # and how it interacts with FedRAMP High for national-security systems. -->

  evidence_contract_extensions:
    - control_id: AIC-<security-controls-implementation>
      obligation_id: OBL-USFED-FEDRAMP-SSP
      added_line_items:
        - artefact: system-security-plan
          schema: fedramp-ssp-schema (mod-108 registry)
          cadence: per-major-release + annual-reassessment
          owner: ato-focal-per-platform (see additional_roles)
          retention: per-federal-records-schedule
    - control_id: AIC-<independent-assessment>
      obligation_id: OBL-USFED-FEDRAMP-SAR
      added_line_items:
        - artefact: security-assessment-report
          schema: fedramp-sar-schema (mod-108 registry)
          cadence: pre-authorisation + annual-reassessment
          owner: 3pao (provenance edge — assessor)
          retention: per-federal-records-schedule
    - control_id: AIC-<finding-management>
      obligation_id: OBL-USFED-FEDRAMP-POAM
      added_line_items:
        - artefact: plan-of-action-and-milestones
          schema: fedramp-poam-schema (mod-108 registry)
          cadence: monthly-update + on-material-finding
          owner: ato-focal-per-platform
          retention: per-federal-records-schedule
    - control_id: AIC-<use-case-registration>
      obligation_id: OBL-USFED-M-25-21-USE-CASE-INVENTORY
      added_line_items:
        - artefact: agency-use-case-inventory-contribution
          schema: agency-use-case-inventory-schema
          cadence: annual + on-material-change
          owner: federal-programme-caio (see additional_roles)
          retention: per-federal-records-schedule
    - control_id: AIC-<minimum-practices-attestation>
      obligation_id: OBL-USFED-M-25-21-MINIMUM-PRACTICES
      added_line_items:
        - artefact: minimum-practices-attestation
          schema: m-25-21-minimum-practices-schema
          cadence: per-deployment + annual + on-material-change
          owner: federal-programme-caio
          retention: per-federal-records-schedule
    - control_id: AIC-<privacy-impact-assessment>
      obligation_id: OBL-USFED-PIA
      added_line_items:
        - artefact: privacy-impact-assessment
          schema: e-gov-pia-schema (mod-108 registry)
          cadence: pre-deployment + on-material-change
          owner: privacy-officer-federal-programme
          retention: per-federal-records-schedule
    - control_id: AIC-<accessibility>
      obligation_id: OBL-USFED-508
      added_line_items:
        - artefact: section-508-conformance-report (VPAT / ACR)
          schema: acr-schema (mod-108 registry)
          cadence: per-release
          owner: accessibility-lead
          retention: contract-duration + records-schedule
    - control_id: AIC-<continuous-monitoring>
      obligation_id: OBL-USFED-FEDRAMP-CONMON
      added_line_items:
        - artefact: monthly-conmon-submission
          schema: fedramp-conmon-schema (mod-108 registry)
          cadence: monthly
          owner: ato-focal-per-platform
          retention: per-federal-records-schedule

  policy_as_code_guards:
    - guard_id: PAC-USFED-DATA-RESIDENCY
      template: mod-103-residency-boundary-template
      parameterisation:
        addressee_scope: us-federal-workload
        egress_boundary: fedramp-authorised-region-set
        # boundary set is the FedRAMP-authorised region set for the applicable
        # baseline; egress outside the boundary is blocked at the policy
        # enforcement point (mod-103 ch 05).
      enforcement_point: mod-103-egress-pep
    - guard_id: PAC-USFED-FEDRAMP-BOUNDARY-EGRESS
      template: mod-103-egress-boundary-template
      parameterisation:
        addressee_scope: fedramp-in-scope-workload
        allowed_upstream_services: fedramp-authorised-service-catalogue
        # the workload must only call upstream services that are themselves
        # FedRAMP-authorised at or above the workload's baseline; calls to
        # non-authorised services are blocked and generate an incident record.
      enforcement_point: mod-103-service-mesh-pep
    - guard_id: PAC-USFED-ATO-STATUS-REQUIRED
      template: mod-103-deployment-gate-template
      parameterisation:
        addressee_scope: federal-customer-deployment
        required_precondition: platform-ato-status == active
        # deployment to a federal customer is blocked at the CI/CD gate
        # unless the delivery platform's ATO status is active for the
        # applicable baseline; a lapsed ATO blocks deployment even for
        # unchanged code.
      enforcement_point: mod-103-cicd-pep
    - guard_id: PAC-USFED-RIGHTS-IMPACTING-HUMAN-OVERSIGHT
      template: mod-103-human-oversight-template
      parameterisation:
        addressee_scope: rights-impacting-ai-workload
        required_control: named-human-in-the-loop-with-override-authority
        # rights-impacting AI as classified per M-25-21 requires a named
        # human oversight seat with override authority per the minimum
        # practices; the guard verifies the configuration is in place
        # before the workload accepts traffic.
      enforcement_point: mod-103-runtime-pep

  aims_scope_addendum:
    inclusions: |
      the AIMS scope explicitly includes the enterprise's federal-programme
      activities — the delivery of AI-enabled products and services to US
      federal customer agencies and the operation of platforms hosting
      federal customer workloads. Federal-programme AI systems appear in
      the mod-108 inventory with the federal-programme tag and the
      applicable fedramp-baseline / fips-199-impact-level / m-25-21-category
      / security-classification-scope values populated.
    exclusions: |
      classified-scope deployments above the Secret level are excluded from
      the ISO/IEC 42001 AIMS certification scope and governed under the
      corresponding IC / DoD framework; the exclusion is documented with
      the rationale that ISO/IEC 42001 certification bodies do not have
      the classification authority to audit those systems.
      <!-- needs-research: verify current guidance on ISO/IEC 42001 auditor
      classification handling; some certification bodies operate cleared
      auditors under specific arrangements. -->
    interfaces:
      to_enterprise_scope: |
        the federal-programme addendum is an additive inclusion — federal
        systems are additional to the enterprise systems already in scope,
        not a partitioning of the enterprise scope. The Clause 9.3
        management review considers federal-programme risks as one input
        alongside enterprise risks; the Clause 5.3 role list is extended
        (not replaced) with the federal-programme roles this record names.

  risk_taxonomy_augmentation:
    added_categories:
      - id: RISK-USFED-MISSION-IMPACT
        parent_reference_category: operational
        definition: |
          risk arising from AI-system behaviour that materially impairs
          the customer agency's ability to execute its statutory or
          mission-defined function. A specialisation of the reference
          operational category, differentiated by the agency-mission
          consequence rather than the enterprise-operational consequence.
        severity_rubric: aligned to enterprise severity bands with mission-critical anchor
        owner: federal-programme-caio
      - id: RISK-USFED-NATIONAL-SECURITY-ADJACENT
        parent_reference_category: security
        definition: |
          risk arising from AI-system behaviour that has national-security
          implications through the sensitivity of the data processed, the
          integration with mission-critical systems, or the potential
          adversarial exploitation of the deployment. A specialisation of
          the reference security category, differentiated by the national-
          security consequence class.
        severity_rubric: aligned to enterprise severity bands with national-security anchor
        owner: cso-or-designee + federal-programme-caio (joint)
      - id: RISK-USFED-PUBLIC-FACING-SERVICE
        parent_reference_category: reputational
        definition: |
          risk arising from AI-system behaviour in a public-facing
          agency service (citizen-services chatbot, public-information
          portal, benefits-adjudication interface) that materially damages
          public trust in the customer agency. A specialisation of the
          reference reputational category, differentiated by the affected
          party being the public interacting with the agency rather than
          the enterprise's own reputation.
        severity_rubric: aligned to enterprise severity bands with public-trust anchor
        owner: federal-programme-caio
    added_appetite_triggers:
      - trigger_id: APP-USFED-ATO-LAPSE
        threshold: any active federal deployment with lapsed or suspended platform ATO
        declaring_seat: federal-programme-caio + ai-accountable-executive (joint)
      - trigger_id: APP-USFED-POAM-MATERIAL-OPEN
        threshold: material POA&M finding open beyond agreed remediation window
        declaring_seat: ato-focal-per-platform escalating to federal-programme-caio
      - trigger_id: APP-USFED-M-25-21-ATTESTATION-WITHDRAWN
        threshold: agency-side withdrawal of acceptance of minimum-practices attestation
        declaring_seat: federal-programme-caio

  additional_roles:
    - role_identifier: federal-programme-caio (or head-of-federal-ai-programme)
      level: 50 (or 60 where materiality warrants senior seating)
      owner_packet_route: |
        additive to mod-112 chapter 03 role list; where the enterprise
        adopts Posture A (see chapter text) the role is discharged by the
        existing ai-accountable-executive with a named federal-programme
        deputy; Posture B stands the role up as its own seat.
      responsibilities_delta: |
        interfaces with agency CO / COR (and, indirectly, with the
        agency CAIO); owns the minimum-practices attestation per M-25-21;
        owns the AI Use Case Inventory contribution per agency; convenes
        the internal federal-programme forum; represents the federal
        programme at the mod-112 ai-governance-council.
    - role_identifier: ato-focal-per-platform
      level: 40 (typically) with reporting line to federal-programme-caio
      owner_packet_route: |
        additive; per-platform seat responsible for the SSP, POA&M,
        continuous-monitoring stream, and reassessment cycle for the
        platform's ATO. Coordinates with the 3PAO for assessment cycles.
    - role_identifier: system-security-officer-for-ai
      level: 40
      owner_packet_route: |
        additive; the SSO-AI role composes the traditional System Security
        Officer role with AI-specific responsibilities — AI-supply-chain
        provenance verification, foundation-model-provider evaluation
        review, and AI-specific incident triage. Reports to the ATO focal
        for platforms hosting AI workloads.

  reserved_matters_additions:
    - matter: m-25-21-minimum-practices-attestation-to-agency
      trigger: pre-deployment for high-impact or rights-impacting AI; annual reissuance; material change
      pack_owner: federal-programme-caio
    - matter: poa&m-material-finding-with-agency-notification
      trigger: SAR or CONMON produces a material finding requiring agency notification
      pack_owner: ato-focal-per-platform (drafts); federal-programme-caio (owns council disposition)
    - matter: ato-material-change-notification
      trigger: significant change to a FedRAMP-authorised system triggering re-authorisation review
      pack_owner: ato-focal-per-platform
    - matter: agency-use-case-inventory-submission-and-annotation
      trigger: annual cycle + material change per agency
      pack_owner: federal-programme-caio
    - matter: rights-impacting-ai-designation-adjudication
      trigger: internal or agency-initiated dispute over whether a system is rights-impacting under M-25-21
      pack_owner: federal-programme-caio + head-of-ai-governance (jointly)
    - matter: aisi-provider-side-evaluation-material-finding
      trigger: US AISI or voluntary-agreement-mediated provider evaluation produces a finding that affects a contractor deployment
      pack_owner: mod-109-third-party-lead + federal-programme-caio

  deprecation_path_notes:
    - regulation_id: omb-m-24-10
      superseded_by: omb-m-25-21
      migration_deadline: <!-- needs-research: verify -->
      evidence_valid_until: |
        evidence produced under M-24-10 remains valid through the transition
        window OMB defines; the mod-104 register carries the transition
        edge with the specific windows once verified.
      handoff_notes: |
        the mod-104 reconciliation architecture handles the M-24-10 to
        M-25-21 transition; the sector adaptation record references
        those obligation-register edges rather than restating them.
    - regulation_id: omb-m-24-18
      superseded_by: omb-m-25-22
      migration_deadline: <!-- needs-research: verify -->
      evidence_valid_until: as above per mod-104 transition edges
      handoff_notes: as above
```

## The reserved-matters overlay at the mod-112 council

The mod-112 chapter 01 reserved-matters list acquires a federal-contractor overlay. Every item below is minuted at the ai-governance-council because the enterprise cannot delegate the disposition lower and no other enterprise forum owns it.

- **M-25-21 minimum-practices attestation issuance.** Each high-impact or rights-impacting AI deployment to a federal customer produces an attestation the federal-programme CAIO signs; the council minutes issuance, revocation, and material amendment. An attestation that is subsequently withdrawn by the agency or the enterprise triggers an APP-USFED-M-25-21-ATTESTATION-WITHDRAWN appetite breach and is re-minuted.
- **POA&M material findings.** A FedRAMP SAR or CONMON finding classified as material carries agency-notification obligations and often carries a remediation window the enterprise must commit to; the council minutes the finding, the disposition, and the remediation commitment.
- **ATO material changes.** A significant change to a FedRAMP-authorised system triggering re-authorisation review is a reserved matter — the enterprise cannot deploy the change without agency concurrence, and the council minutes the change proposal, the disposition, and the re-authorisation posture.
- **AI Use Case Inventory submission per agency.** The annual cycle and material-change reissuance for each agency's inventory contribution is minuted; a discrepancy between what the enterprise submits and what the agency publishes triggers a review.
- **Rights-impacting-AI designation adjudication.** Where the enterprise's classification of a system disagrees with the agency's classification, the council minutes the dispute and the resolution posture.
- **US AISI provider-side material findings.** A provider whose product embeds a frontier model may transmit AISI or voluntary-agreement-mediated evaluation findings that affect the contractor's deployment; the council minutes the finding, the mod-109 third-party disposition, and the deployment-side response.

The council charter is amended to name the overlay explicitly, and the federal-programme CAIO (or the ai-accountable executive playing that role under Posture A) sits as a voting member or a permanent-observer per the enterprise's federal-programme materiality.

## Failure modes

**Failure mode 1 — treating M-25-21 as agency-only and missing flow-down through contract clauses.** A common pattern is that the contractor's programme team reads M-25-21, notes that its addressees are federal agencies rather than contractors, and concludes that the memorandum does not directly obligate the enterprise. The conclusion is technically correct — M-25-21 obligates the agency — and operationally disastrous. The memorandum's obligations reach the contractor through the contract: FAR clauses, agency supplement clauses, and (where M-25-22 authorises them) AI-specific clauses the contract carries. The contractor whose programme is not authored to satisfy the flow-down finds itself, at the first CO-initiated contract-compliance review, unable to produce the artefacts the clause requires. The architectural defence is to author the mod-104 obligation register so that every M-25-21 and M-25-22 provision the contract flows down appears as an OBL-USFED-... entry with a resolved evidence-contract edge to the mod-108 architecture. The contractor does not read M-25-21 for its direct obligations; the contractor reads its contracts for the flow-downs and dereferences each into the register.

**Failure mode 2 — running the ISO/IEC 42001 AIMS and the FedRAMP SSP as two evidence trails.** The AIMS produces documented information (Clause 7.5) satisfying the management-system requirement; the FedRAMP SSP produces the security-controls documentation satisfying the FedRAMP authorisation requirement. If the two are authored independently, the enterprise ends up with two artefact families for the same underlying implementation — the AIMS document set describing the AI system's controls in ISO/IEC 42001 vocabulary, the SSP describing the same controls in NIST SP 800-53 vocabulary, with drift between them and reconciliation debt that examiners and auditors find at each cycle. The architectural defence is the composition principle stated in the NIST SP 800-53 section: one implementation per control, three renderings (AIMS documented information, SSP, mod-102 SoA). The mod-108 evidence architecture carries the implementation once with the renderings generated (as far as possible) as views over the same source, so that a change to the implementation propagates to all three renderings in the same cycle.

**Failure mode 3 — designating a federal-programme CAIO without giving the seat authority over deployment.** Some contractors stand up a federal-programme CAIO role as a compliance seat — the seat is accountable for the artefacts but not authorised to block deployment when the artefacts are not defensible. The consequence is that the deployment proceeds under commercial pressure, the artefacts are backfilled or waived, and the seat becomes a legibility function rather than a governance function. The defence is that the role packet for the federal-programme CAIO (or the enterprise AI-accountable executive discharging the role) carries explicit deployment-blocking authority for federal-customer deployments, enforced at the PAC-USFED-ATO-STATUS-REQUIRED and PAC-USFED-RIGHTS-IMPACTING-HUMAN-OVERSIGHT policy-enforcement points. Authority without artefact production is legibility; artefact production without authority is compliance theatre; the defence requires both.

## Summary

For a US federal agency contractor, the composition anchor for the AI governance programme is the agency-side CAIO designation the OMB memoranda mandate plus the FedRAMP authorisation status of the platform the AI runs on. The enterprise AIMS the contractor already operates extends into the federal-programme space; the federal-programme space does not fold into the AIMS. OMB M-25-21 governs the agency's use of AI (CAIO designation, CAIO Council, minimum practices for high-impact and rights-impacting AI, AI Use Case Inventory) and reaches the contractor through contract flow-down. OMB M-25-22 governs the agency's acquisition of AI (performance-based acquisition, AI-specific clauses, vendor-lock-in mitigation, subcontractor flow-down) and provides the contractual mechanism the M-25-21 obligations travel on. FedRAMP is the platform-authorisation gate — SSP, SAR, POA&M, continuous monitoring — with baselines keyed to FIPS 199 impact levels and authorisation paths through the JAB or a sponsoring agency, and with FedRAMP 20x as the modernisation programme the evidence architecture must be authored to accept. NIST SP 800-53 Rev. 5 is the underlying control catalog; the mod-102 catalog composes with SP 800-53 through crosswalk edges from AI-nexus entries into the AC / AU / IA / RA / SA / SI / SR families, and NIST AI RMF plus NIST AI 600-1 are the AI-specific overlay SP 800-53 does not carry. The US AISI methodology reaches the contractor as a provider-side evaluation input flowing through mod-109 third-party governance. The contractor's own CAIO designation is a choice among three postures — no dedicated seat, a federal-programme CAIO, or platform-scoped ATO focals — with the correct posture determined by federal-programme materiality; the interface to the agency CAIO is through the CO / COR, never direct. The mod-112 council reserved-matters list acquires an overlay for M-25-21 attestations, material POA&M findings, ATO material changes, Use Case Inventory submission, rights-impacting-AI designation adjudication, and AISI provider-side material findings. Anchor to the agency-side CAIO and the ATO of the delivery platform, extend the enterprise programme into the federal-programme space through contract flow-downs, render one implementation as three artefact families (AIMS documented information, SSP, mod-102 SoA), and the federal-contractor blueprint composes.
