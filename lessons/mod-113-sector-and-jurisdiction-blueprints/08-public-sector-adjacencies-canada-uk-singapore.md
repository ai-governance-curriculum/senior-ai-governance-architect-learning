# Public-sector adjacencies — Canada TBS Directive on ADM, UK ATRS, and Singapore AI Verify as extensions to the private-sector reference architecture

## Why this chapter exists

Chapter 06 of this module authored a full US-federal-contractor blueprint (OMB M-25-21, M-25-22, FedRAMP, NIST SP 800-53, NIST AI RMF, US AISI methodology). That chapter reshapes the reference architecture because a federal-contractor posture materially changes the AIMS scope, the third-party programme, the assurance architecture, and the residency/boundary policy set — full-blueprint work, not overlay work. The private-sector enterprise that also sells into non-US public-sector customers — a Canadian federal department, a UK central-government department, a Singapore statutory board — is often surprised to discover that those customers carry *their own* public-sector obligations that flow down into the vendor's delivery process. Those obligations are not full blueprints. They are lightweight, high-frequency, transparency-and-assessment-heavy adjacencies that attach *when a procurement is in flight or an in-scope decision is live* and detach when the customer relationship changes shape.

This chapter positions three such adjacencies — Canada's Treasury Board Directive on Automated Decision-Making, the UK's Algorithmic Transparency Recording Standard, and Singapore's AI Verify testing framework — as *lightweight extensions to the reference architecture*, not as sector blueprints in their own right. The architectural claim is that all three compose as evidence-contract extensions (I3) and applicability-filter values (I2) on the existing enterprise catalog, with a small AIMS scope-addendum edit (I5) and a modest risk-taxonomy augmentation (I6). No forked control library (I1), no jurisdiction-specific policy-as-code programme (I4 is discharged by parameterisation). Failure to recognise the adjacency shape produces two symmetrical mistakes: the enterprise either over-invests (treats a lightweight adjacency as a full sector and forks) or under-invests (fails to produce the required artefact at a delivery gate and loses the contract). Both are avoidable, and both are avoided by treating the three adjacencies as artefact-rendering problems rather than programme-redesign problems.

## Canada — Treasury Board Directive on Automated Decision-Making

The **Treasury Board of Canada Secretariat Directive on Automated Decision-Making** entered into force on 1 April 2019 and has been revised periodically since. <!-- needs-research: verify the most recent revision date and version of the TBS Directive on ADM; the Directive has been revised more than once since 2019 to broaden scope and update Algorithmic Impact Assessment requirements. --> The Directive binds federal departments and agencies to which it applies; a private-sector vendor delivering an automated decision system (ADS) to an in-scope department inherits Directive obligations through the procurement contract's flow-down clauses. The vendor is not itself the regulated party in the primary sense — the department is — but the vendor cannot deliver a compliant system without instantiating the Directive's evidence expectations into its own build and evaluation pipeline.

The Directive's operational core is the **Algorithmic Impact Assessment (AIA) tool** — a structured questionnaire the department (typically with the vendor's technical input) completes before an ADS enters production. The AIA produces a numerical score that maps to an *impact level* on a four-level scale (levels I through IV, ascending), and the impact level drives the specific requirements the ADS must satisfy across categories including public notification, explanation of decisions, human intervention, quality assurance, peer review, contingency planning, and staff training. <!-- needs-research: verify the exact requirement categories and the escalation structure across impact levels I-IV against the current Directive appendix. --> Higher impact levels impose more stringent versions of each requirement — a level-I system may require a short notification and basic documentation, while a level-IV system requires published notification in plain language, formal explanation on demand, human-in-the-loop intervention capability, independent peer review by a qualified third party, and a contingency plan for system failure. The precise mapping is set out in the Directive's appendices and evolves with revisions; the level-50 architect does not hard-code the mapping into the vendor's evidence contract but dereferences the current Directive appendix at each engagement.

**Composition point.** The AIA is a first-class evidence artefact in the mod-108 registry. The architect adds an `AIA-canada-tbs` schema entry that captures the questionnaire responses, the computed impact level, the assessing authority (the department, with the vendor as technical contributor), the effective date, and the review-cadence trigger. The AIA output — the impact level itself — is then registered as a value in the mod-102 applicability filter under a `Canada-TBS-ADS-impact-level` dimension with enumeration `{I, II, III, IV, N/A}`. Existing catalog controls whose behaviour is impact-level-dependent (public-notification, explanation-on-demand, human-intervention, contingency) acquire evidence-contract line items keyed to the applicable impact level. No control is forked. The system delivered to the Canadian federal department composes the enterprise reference-profile control set plus the additional line items the applicable impact level selects.

**Mod-104 obligation-register anchor.** Every Directive requirement the AIA activates is registered in the mod-104 obligation register as an `OBL-CA-TBS-ADS-*` entry with the Directive section and the affected controls named. The evidence-contract extensions dereference the obligation-register identifier, per invariant I3.

## UK — Algorithmic Transparency Recording Standard (ATRS)

The **Algorithmic Transparency Recording Standard** was jointly developed by the UK Central Digital and Data Office (CDDO) and the Centre for Data Ethics and Innovation (CDEI). <!-- needs-research: verify current ATRS version, publishing body (post-CDDO/CDEI reorganisation), and current status as of the assessing date; ATRS moved from pilot to broader roll-out in the 2023-2024 period and the exact adoption-mandate posture across UK central government evolves. --> The Standard defines a public record that UK central-government departments and their arms-length public bodies publish for algorithmic tools they use in decision-making. A private-sector vendor delivering such a tool contributes to the record's authoring even though the department is the publisher of record; the vendor typically supplies the technical content that populates the more detailed portion of the record and reviews the customer-authored summary portion for factual accuracy.

The ATRS record is authored in two tiers:

- **Tier 1** — a short, plain-English summary describing what the tool does, who it is used by, and what problem it solves. The tier-1 record is written for a lay audience and is intentionally free of technical detail.
- **Tier 2** — the detailed record covering how the tool works (data sources, model type and training approach at appropriate abstraction, decision process), oversight arrangements (human review, sign-off, appeal), technical specification (limitations, performance considerations), and a contact point plus further-information references. Tier 2 is where the technical substance lives and where the vendor's contribution concentrates.

The tier-1/tier-2 split is not a legal distinction; it is an authoring pattern that lets the public read a short description without wading through architectural detail and lets an interested reader (a researcher, a journalist, an affected individual, an ombuds) reach the detail without a formal request.

**Composition point.** The ATRS record is not a new document the vendor produces from scratch. It is a *view* rendered from artefacts the mod-108 evidence architecture already carries — the model-documentation artefact, the AIMS Statement of Applicability content that names oversight arrangements, the mod-107 assurance-gate outputs, the mod-110 post-market surveillance summary. The architect authors an `ATRS-record-uk` schema entry in the evidence-schema registry that describes the required fields and their source artefacts (which mod-108 artefact populates each field), and a rendering step that produces the record from those artefacts on request. The vendor's day-to-day production process does not change; the ATRS record is a rendering discipline layered onto the existing evidence, not a parallel authoring stream. A `UK-ATRS-tier` filter value is added to the mod-102 applicability filter with enumeration `{tier-1-only, tier-1-plus-tier-2, out-of-scope}` — most in-scope engagements will select `tier-1-plus-tier-2`, but the value is preserved for the rare case where only the summary is required.

**Interaction with the broader UK AI-assurance ecosystem.** ATRS is a transparency mechanism, not a risk-management framework. The UK's risk-management expectations are carried by the relevant sector regulator (Financial Conduct Authority, Medicines and Healthcare products Regulatory Agency, Ofcom, and so on) and, where personal data is involved, by the Information Commissioner's Office under UK GDPR and the Data Protection Act. The vendor's mod-102 catalog already carries the ICO-anchored controls; ATRS attaches on top as a disclosure obligation without displacing the risk-management posture. The AI Standards Hub and the AI Safety Institute sit in the same ecosystem as sources of guidance and evaluation methodology respectively; the architect tracks their outputs through the mod-104 horizon-scanning cadence but does not treat either as a source of directly binding obligations for the ATRS overlay.

## Singapore — AI Verify Foundation and AI Verify testing framework

The **AI Verify Foundation** was established in 2023 as a not-for-profit body hosted under the Infocomm Media Development Authority (IMDA), with the mission of developing and stewarding the **AI Verify testing framework and toolkit**. <!-- needs-research: verify AI Verify's current toolkit version, the current scope of technical tests and process checks it covers, and the specific alignment claims against the Singapore Model AI Governance Framework and the Model AI Governance Framework for Generative AI. --> AI Verify is voluntary self-assessment aligned to the **Singapore Model AI Governance Framework** published by the IMDA and the Personal Data Protection Commission (PDPC). The Model Framework organises around a set of guiding principles covering fairness, transparency, explainability, safety, accountability, human agency and oversight, reproducibility, data governance, robustness, security, and inclusive growth (with the precise principle count and framing evolving across Model Framework versions and the newer Generative AI addition). <!-- needs-research: confirm the exact principle enumeration and count for the current Model Framework version and for the Generative AI Model Framework — the principle set has shifted across editions and the Generative AI Framework introduces framing not present in the classical Model Framework. -->

AI Verify runs two kinds of checks: **technical tests** (executable evaluations run against the model — fairness metrics on defined subgroups, robustness perturbation tests, explainability method outputs where the model architecture supports them) and **process checks** (structured attestations against the Model Framework principles that reference documentation the enterprise already maintains). The output is a report the enterprise can share with customers, regulators, or the public.

**Composition point.** The AI Verify toolkit's outputs are captured as evidence artefacts in the mod-108 registry under an `AI-Verify-report-sg` schema entry. Technical-test outputs feed the mod-107 pre-deployment assurance-gate evidence bundle for Singapore-scope systems — the same gate the enterprise runs for every deployment, extended with an additional evidence line item that says *for Singapore-scope systems, the AI Verify technical-test bundle at a specified toolkit version is required as part of the gate pack*. Process-check outputs are rendered from the AIMS documented-information set (mod-105) and the mod-102 SoA content without additional authoring. A `SG-AIVerify-scope` filter value is added with enumeration `{in-scope, out-of-scope}` and the assurance-gate control's evidence contract adds the Singapore line item behind that filter.

**Interaction with the IMDA/PDPC posture and the ASEAN Guide.** AI Verify is voluntary in the primary sense but is increasingly cited by Singapore customers and by IMDA/PDPC guidance as a preferred demonstration mechanism. The **ASEAN Guide on AI Governance and Ethics** (published 2024) frames Singapore's approach in a regional context; a vendor operating across ASEAN member states benefits from treating the Singapore-anchored AI Verify posture as the regional demonstration baseline and layering member-state-specific overlays only where a specific member state has published mandatory requirements. <!-- needs-research: verify the ASEAN Guide's publication status, its non-binding character, and the specific member-state overlays that have appeared since its publication. -->

## The unifying architectural move

The three adjacencies look superficially different — a Canadian pre-production impact assessment, a UK public transparency record, a Singapore voluntary test-and-attest package — but they compose into the reference architecture through the same mechanism. Each is an *artefact rendering* that dereferences existing evidence and adds a small number of adjacency-specific line items; none is a *programme redesign* that requires a new AIMS, a new control library, or a new risk taxonomy. The mapping onto the six invariants of chapter 01 is explicit:

- **I1 — no forked control library.** All three adjacencies attach as evidence-contract line items on existing mod-102 catalog entries and, at most, add one or two small catalog entries (an `AIA-participation` control, an `ATRS-record-contribution` control, an `AI-Verify-participation` control) authored through the standard mod-102 chapter 06 lifecycle. There is no *Canada catalog*, no *UK catalog*, no *Singapore catalog*.
- **I2 — added filter values, not new dimensions.** Three new applicability-filter values are added under a `public-sector-adjacent-scope` dimension (or under the existing jurisdiction dimension, depending on the enterprise's filter design): `Canada-TBS-ADS-impact-level` with a level enumeration, `UK-ATRS-tier` with a tier enumeration, `SG-AIVerify-scope` with an in-scope/out-of-scope enumeration. The filter-parsimony discipline of chapter 01 is honoured — no new dimension proliferation, three values on the appropriate existing dimension.
- **I3 — evidence contract extends per obligation.** The AIA schema, the ATRS-record schema, and the AI Verify report schema are registered in the mod-108 evidence-schema registry and named on the affected catalog controls as evidence-contract extensions keyed to obligation identifiers in the mod-104 register.
- **I4 — parameterised policy-as-code.** Any policy-as-code guards the three adjacencies require (jurisdictional data-residency for Canadian-federal or Singapore-hosted engagements, public-facing disclosure guards for ATRS-tier-1 releases) are parameterisations of existing mod-103 policy templates, not new policy programmes.
- **I5 — AIMS scope addendum.** The mod-105 chapter 04 scope statement acquires a short public-sector-adjacent addendum naming the departments, agencies, and public bodies within the enterprise's customer footprint that fall under the three adjacencies, with an intersection statement showing that the addendum extends the enterprise scope rather than fragmenting it.
- **I6 — risk taxonomy augmented.** A `public-trust` risk category is added as a specialisation of the reference *reputational* category, and a `public-facing-service` category is added as a specialisation of the reference *operational* category, both with severity rubrics aligned to the enterprise bands per mod-106 chapter 04.

The move is small on purpose. A level-50 architect who authors a large delta against the reference for any of these three adjacencies is authoring the wrong artefact.

## The sector adaptation record — public-sector-adjacent scope (partial)

Because the delta is small, the sector adaptation record for the public-sector-adjacent scope is a partial record — a subset of the schematic chapter 01 defines, populated only where the adjacencies materially extend the reference. Empty sections (assurance-architecture delta, third-party-programme delta, post-market surveillance delta beyond the small transparency hook) are deliberately absent because the adjacencies do not touch them.

```yaml
sector_adaptation_record:
  id: SAR-PUBLIC-SECTOR-ADJACENT-v1.0
  ratified_by: ai-governance-council (mod-112 ch 01) on <ratification-date>
  reference_architecture_version:
    control_catalog: mod-102 catalog v<X.Y>
    reference_profile: enterprise-reference-profile v<X.Y>
    obligation_register: mod-104 register v<X.Y>

  sector:
    identifier: public-sector-adjacent
    scope_narrative: |
      Enterprise activities where an AI product is procured by, deployed for,
      or must interoperate with a public-sector customer in Canada (federal
      departments and agencies), the UK (central-government departments and
      arms-length public bodies), or Singapore (statutory boards and other
      public entities), and where the customer's public-sector obligations
      flow down to the vendor through the procurement contract.

  anchor_regulations:
    sector_specific:
      - id: OBL-CA-TBS-ADS
        title: Treasury Board Directive on Automated Decision-Making
        jurisdiction: CA-federal
        supervisor: Treasury Board of Canada Secretariat
        instrument_type: directive
      - id: OBL-UK-ATRS
        title: Algorithmic Transparency Recording Standard
        jurisdiction: UK-central-government
        supervisor: CDDO / CDEI (current stewardship per needs-research above)
        instrument_type: standard
      - id: OBL-SG-AIVERIFY
        title: AI Verify testing framework (voluntary; Model AI Governance Framework-aligned)
        jurisdiction: SG
        supervisor: AI Verify Foundation / IMDA / PDPC
        instrument_type: voluntary-framework

  profile:
    id: PROFILE-PUBLIC-SECTOR-ADJACENT-v1.0
    derives_from: enterprise-reference-profile v<X.Y>
    delta_summary: |
      Filter-values-only delta. No new catalog controls in the primary path;
      one or two small adjacency-participation controls added through the
      standard mod-102 chapter 06 lifecycle. All obligation-specific behaviour
      is expressed through evidence-contract extensions and filter-driven
      applicability.
    applicability_filter_overrides:
      - dimension: public-sector-adjacent-scope
        added_values:
          - Canada-TBS-ADS-impact-level: [ I, II, III, IV, N/A ]
          - UK-ATRS-tier: [ tier-1-only, tier-1-plus-tier-2, out-of-scope ]
          - SG-AIVerify-scope: [ in-scope, out-of-scope ]
        rationale: |
          Adjacency-specific values gate the evidence-contract extensions
          that produce the AIA, the ATRS record, and the AI Verify report.

  evidence_contract_extensions:
    - control_id: AIC-<reference-catalog-control-for-pre-production-assessment>
      obligation_id: OBL-CA-TBS-ADS
      added_line_items:
        - artefact: algorithmic-impact-assessment
          schema: AIA-canada-tbs (mod-108 schema registry)
          cadence: pre-production; re-assessed on material modification
          owner: customer-department (primary); vendor-technical-contributor
          retention: per Directive requirement (dereferenced at engagement)
    - control_id: AIC-<reference-catalog-control-for-external-transparency>
      obligation_id: OBL-UK-ATRS
      added_line_items:
        - artefact: atrs-record
          schema: ATRS-record-uk (mod-108 schema registry)
          cadence: on deployment; on material modification
          owner: customer-department (publisher); vendor (tier-2 contributor)
          retention: while system is in operational use plus retention tail
    - control_id: AIC-<reference-catalog-control-for-pre-deployment-assurance>
      obligation_id: OBL-SG-AIVERIFY
      added_line_items:
        - artefact: ai-verify-report
          schema: AI-Verify-report-sg (mod-108 schema registry)
          cadence: pre-deployment for SG-in-scope systems; refreshed on
            material modification or on toolkit-version change
          owner: vendor (primary); customer (reviewer)
          retention: aligned to mod-108 assurance-gate retention

  policy_as_code_guards:
    - guard_id: PAC-CA-TBS-RESIDENCY-01
      template: mod-103 jurisdictional-data-residency template
      parameterisation:
        addressee_scope: Canada-federal-department
        egress_boundary: <Canada-permitted-boundary per engagement>
      enforcement_point: mod-103 chapter 05 egress-enforcement point
    - guard_id: PAC-UK-ATRS-DISCLOSURE-01
      template: mod-103 external-disclosure-approval template
      parameterisation:
        addressee_scope: UK-central-government-public-record
        disclosure_type: atrs-record
      enforcement_point: mod-103 chapter 05 disclosure-approval point
    - guard_id: PAC-SG-AIVERIFY-EVIDENCE-01
      template: mod-103 assurance-gate-evidence-completeness template
      parameterisation:
        addressee_scope: SG-in-scope
        required_artefacts: [ ai-verify-report ]
      enforcement_point: mod-103 chapter 05 pre-deployment-gate point

  aims_scope_addendum:
    inclusions: |
      Public-sector-adjacent engagements as defined in scope_narrative above,
      inclusive of the AI systems whose customer footprint triggers one or
      more of Canada-TBS-ADS, UK-ATRS, or SG-AIVerify obligations.
    exclusions: |
      Enterprise engagements with no public-sector customer footprint in
      Canada, the UK, or Singapore. US-federal-contractor scope is handled
      by the chapter 06 full blueprint and is out of scope for this record.
    interfaces:
      to_enterprise_scope: |
        Additive. The addendum extends the mod-105 chapter 04 scope with a
        named set of engagements; no exclusion of enterprise-scope systems.

  risk_taxonomy_augmentation:
    added_categories:
      - id: RISK-PUBLIC-TRUST
        parent_reference_category: reputational
        definition: |
          Erosion of public trust in the enterprise or its public-sector
          customer arising from the deployment or perceived misuse of an
          AI system in a public-facing decision or service.
        severity_rubric: aligned to enterprise reputational-severity bands
      - id: RISK-PUBLIC-FACING-SERVICE
        parent_reference_category: operational
        definition: |
          Availability, accuracy, or accessibility failure of a public-facing
          AI-enabled service that public-sector users or beneficiaries depend on.
        severity_rubric: aligned to enterprise operational-severity bands

  additional_roles:
    - role_identifier: public-sector-engagement-liaison
      level: often part of an existing engagement-management or account seat
      responsibilities_delta: |
        Track flow-down of public-sector obligations through the procurement
        contract; author the vendor-side contribution to AIA, ATRS, and
        AI Verify artefacts; hold the timeline for the delivery-gate
        production of each artefact; convene the customer relationship for
        obligation-refresh cadences.
```

The record is small on purpose. A record with more content is either mis-scoped (the enterprise is doing US-federal-contractor work under this record, which belongs in chapter 06) or has drifted into fork-temptation territory (a Canada-specific control library is being authored under the pretence of an evidence-contract extension).

## Interaction with chapter 06 — the US-federal-contractor full blueprint

Chapter 06 is a *full sector blueprint*. It reshapes the AIMS scope (Chief AI Officer designation, agency-facing obligations that flow into second-line and third-line responsibilities), extends the assurance architecture (FedRAMP boundary evidence composed with the mod-107 pre-deployment gate), and adds obligation-heavy layers around M-25-21 and M-25-22 flow-down. The three adjacencies in this chapter do not do that. They add transparency and assessment artefacts to existing controls without touching the AIMS scope structurally, without extending the assurance architecture, and without reshaping the third-party programme. The distinction is deliberate. A private-sector enterprise that discovers it needs *both* the chapter 06 full blueprint (because it is a US-federal contractor) *and* one or more of the adjacencies here (because it also sells into Canadian, UK, or Singapore public-sector customers) instantiates both — chapter 06 as a full blueprint and this chapter's record as a partial-record adjacency layer on top. The two do not conflict. The failure mode is treating the adjacencies as if they carried chapter 06's weight, which would fragment the AIMS scope and multiply the third-party programme's overhead without commensurate benefit.

## Failure modes

**Failure mode 1 — treating one adjacency as a full sector and forking.** The most damaging failure is the sector team that reads the TBS Directive appendices in detail, or the ATRS record schema and CDDO/CDEI guidance, or the AI Verify toolkit documentation, and concludes that the material warrants its own control library, its own AIMS scope, or its own risk taxonomy. The material is genuinely detailed — the Directive appendices are long, the ATRS field set is precise, the AI Verify testing methodology is technical — but detail is not the same as sectoral distinctness. All three adjacencies are expressible as evidence-contract line items on existing controls and as filter values on the existing profile. The invariant I1 test at the ai-governance-council is the defence: any adjacency proposal that requires a new catalog family is refused and returned with an evidence-contract-extension route.

**Failure mode 2 — failing to produce the adjacency artefact at the delivery gate.** The mirror-image failure is under-recognition. The vendor's engagement team wins the procurement, delivers the system to the department or statutory board, and only at the delivery-acceptance gate discovers that the customer required an AIA before production, an ATRS record on deployment, or an AI Verify report as part of the acceptance evidence. The artefacts are not producible retrospectively without months of rework — the AIA in particular is a pre-production assessment whose retrospective authoring reads to a Canadian federal reviewer as procedural non-compliance regardless of the system's actual behaviour. The architectural defence is the mod-102 applicability filter's `public-sector-adjacent-scope` dimension: any workload whose engagement metadata carries a value on that dimension triggers the mod-103 policy-as-code gate that refuses production release without the corresponding artefact registered against the workload's evidence bundle. The public-sector-engagement liaison role owns the front-line detection at contract signature; the gate is the backstop when the liaison misses it.

## Summary

Canada's TBS Directive on Automated Decision-Making, the UK's Algorithmic Transparency Recording Standard, and Singapore's AI Verify testing framework are three high-frequency public-sector adjacencies that a private-sector enterprise selling into Canadian, UK, or Singapore public-sector customers must instantiate — but they are *not* full sector blueprints. All three compose into the enterprise reference architecture as evidence-contract extensions (I3) and applicability-filter values (I2) on the existing mod-102 catalog, with a modest AIMS scope-addendum edit (I5) and a two-category risk-taxonomy augmentation (I6); the policy-as-code layer (I4) is discharged by parameterisation of existing mod-103 templates; the control library is not forked (I1). The partial sector adaptation record for the public-sector-adjacent scope names one filter dimension carrying three filter values, three evidence-schema extensions (AIA schema, ATRS record schema, AI Verify report schema), three policy-as-code parameterisations (residency, disclosure, gate-evidence-completeness), a short AIMS scope addendum, two augmented risk categories (public-trust and public-facing-service), and a public-sector-engagement liaison role usually instantiated within an existing account or engagement seat. Chapter 06 remains the anchor for full US-federal-contractor work; the three adjacencies here layer on top of chapter 06 where an enterprise carries both postures. The two failure modes to design against are the fork temptation (treating an adjacency as a full sector — refused at the ai-governance-council by the invariant tests) and the delivery-gate miss (failing to produce the AIA, ATRS record, or AI Verify report on time — caught by the applicability-filter-triggered policy-as-code gate and, front-line, by the engagement liaison). Get the adjacency shape right and each of the three obligations composes as a rendering discipline over evidence the enterprise already produces, not as a programme the enterprise runs in parallel.
