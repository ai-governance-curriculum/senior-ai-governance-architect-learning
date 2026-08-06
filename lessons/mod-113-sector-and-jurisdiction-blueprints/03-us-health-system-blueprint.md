# The US health-system blueprint — SaMD change-control as the composition anchor

## Why this chapter exists

A US health system that both builds and deploys clinical AI occupies two regulatory identities at once. When it develops a Software-as-a-Medical-Device (SaMD) product — an ambient-scribe model with clinical-decision-support features, an imaging-triage classifier, a sepsis-prediction score integrated into the EHR — it is a *medical-device manufacturer* under FDA CDRH authority. When it deploys the same product (or a purchased one) into patient care, it is a *covered entity* under HIPAA and a *health programme or activity receiving federal financial assistance* under Section 1557 of the ACA. The sector-adaptation methodology (`./01-the-sector-adaptation-methodology.md`) is agnostic about *which* regulatory identity anchors the blueprint; the health-system blueprint is not. The anchor is the SaMD manufacturer identity, because that is where the change-control obligation lives, and change-control is what forces the composition of every other module into a defensible whole.

The reason change-control is the anchor is architectural, not regulatory-priority. HIPAA and Section 1557 impose control families that layer additively on top of an existing AIMS (mod-105) and evidence architecture (mod-108). The SaMD identity imposes a *lifecycle discipline* — the FDA's Predetermined Change Control Plan (PCCP) — that reorganises how the enterprise's mod-102 profile update policy, mod-107 pre-deployment gate, mod-110 post-market surveillance, and mod-112 council interact. If the architect gets the PCCP shape right, HIPAA and Section 1557 slot in as additional control-family applicability values. If the architect gets the PCCP shape wrong, no amount of HIPAA or Section 1557 discipline will make the change-control record defensible when a CDRH inspector arrives.

This chapter reads the current-state anchors — FDA Good Machine Learning Practice (GMLP) guiding principles, the FDA SaMD Action Plan, the Predetermined Change Control Plans final guidance, HIPAA, Section 1557 — plus the state medical-AI overlay, and instantiates the sector-adaptation record against them. The chapter's scope is CDRH (medical devices). The pharma pathway (CDER for drugs, CBER for biologics) is chapter 05; the boundary is drawn explicitly below because integrated delivery networks with pharmacy programmes sit close to it.

## Anchor obligations

The health-system blueprint's anchor obligations, in the order the sector-adaptation record cites them:

- **FDA Good Machine Learning Practice guiding principles** — a joint publication of the US FDA, Health Canada, and the UK MHRA, October 2021. Ten principles for the development of medical-device AI/ML. Non-binding but treated as authoritative by CDRH reviewers.
- **FDA Software as a Medical Device Action Plan** — January 2021. Sets out FDA's five-part action plan for AI/ML-based SaMD, including the PCCP concept, real-world performance monitoring, and Good Machine Learning Practice development. Referenced by every subsequent FDA AI/ML guidance as the framing document.
- **FDA Marketing Submission Recommendations for a Predetermined Change Control Plan for AI-Enabled Device Software Functions** — final guidance issued December 2024. Superseded the April 2023 draft. Sets out the three components a PCCP must contain: the description of modifications, the modification protocol, and the impact assessment. <!-- needs-research: verify the exact issuance date and full title of the final PCCP guidance; the draft was April 2023 and the final was announced in late 2024, but the specific date should be confirmed against the FDA docket. -->
- **HIPAA Privacy Rule, Security Rule, and Breach Notification Rule** — 45 CFR Parts 160 and 164, administered by HHS Office for Civil Rights. Applies to covered entities (health-care providers, health plans, health-care clearinghouses) and, through business-associate agreements, to their business associates.
- **Section 1557 of the ACA — Nondiscrimination in Health Programs and Activities** — HHS OCR final rule, May 2024. Prohibits discrimination on the basis of race, colour, national origin, sex, age, or disability by health programmes or activities receiving federal financial assistance, and explicitly addresses discrimination arising through patient-care decision-support tools.
- **EU AI Act Annex III — medical devices as high-risk AI** — medical-device AI integrated with EU MDR (Regulation 2017/745) or IVDR (Regulation 2017/746) reaches high-risk classification via the AI Act's harmonised-legislation route. <!-- needs-research: confirm whether medical devices reach EU AI Act high-risk status via Annex III paragraph 3 (safety components of products already regulated under harmonised legislation) or exclusively via Article 6(1) plus Annex I MDR/IVDR listing; the two routes have different obligations and the sector-adaptation record must cite the correct one. -->
- **State medical-AI regulation.** California AB 3030 (2024) requires disclosure when generative AI is used in patient communications about clinical information; enforced by state licensing boards and the California Department of Public Health where applicable. Texas HB 149 (the Texas Responsible AI Governance Act — TRAIGA) and Illinois HB 3773 (amendments to the Illinois Human Rights Act) intersect on the employment side of the enterprise, where clinical staff are hired and clinical AI tools are used in adverse-employment decisions. <!-- needs-research: confirm current effective dates and enforcement postures for AB 3030, TRAIGA, and IL HB 3773; the state overlay landscape has been changing month-over-month and the record should carry the current-state citation. -->

## The SaMD lifecycle plus PCCP — where the composition anchors

The FDA's SaMD framework treats the software function as a medical device in its own right; the PCCP is the mechanism by which a manufacturer receives premarket authorisation not just for the *current* device but for a *pre-specified set of future modifications* that the manufacturer commits to implementing under a defined protocol without requiring a new premarket submission for each change. For an AI/ML device — where retraining on new data, threshold adjustments, and re-tuning are expected in the ordinary course — the PCCP is the difference between a submission every few months and a stable authorisation with an operating envelope.

The final December 2024 guidance requires three components in every PCCP:

1. **Description of modifications.** The set of pre-specified changes the manufacturer intends to make post-authorisation without a new submission. Each modification is described specifically enough that a CDRH reviewer can determine whether a proposed post-authorisation change falls inside or outside the pre-specified set. Categories typically include performance modifications (retraining on new data of the same modality; threshold recalibration), input modifications (new input data types or device configurations), and use-case modifications (extending to a new patient population or clinical workflow within the cleared indication).
2. **Modification protocol.** The methodology the manufacturer will follow when implementing each pre-specified modification: data-collection and management protocol, model-development and re-validation protocol, performance-evaluation protocol including bias assessment and human-factors re-evaluation where applicable, update-procedure protocol including staging and rollback, and communication protocol for patients and users. The protocol is what the manufacturer is *committing* to follow; deviation from the protocol is a deviation from the authorisation.
3. **Impact assessment.** The manufacturer's analysis of the risks that the pre-specified modifications introduce, the benefits they are expected to deliver, and the residual-risk analysis assuming the modification protocol is followed. The impact assessment is what convinces the reviewer that the modification protocol is adequate to bound the risk of the described modifications.

The composition into the mod-102 and mod-107 architecture is where the level-50 architect earns their pay.

**PCCP and mod-102 profile update policy.** The mod-102 profile update policy defines when a change to the enterprise's control profile requires ratification and what the ratification path is. For a SaMD system operating under a PCCP, the profile update policy acquires a *PCCP boundary check*: any proposed modification to the SaMD's control profile is first evaluated against the description of modifications. If the modification is inside the pre-specified set, the mod-102 update follows the SaMD change-control lead's ordinary post-authorisation update path (a section 806 correction-and-removal-adjacent workflow, plus the AIMS Clause 8.3 change-control record). If the modification is outside the pre-specified set, the mod-102 update triggers a *new premarket submission* — a 510(k), De Novo, or PMA supplement as applicable — and the modification cannot be deployed until the submission is cleared. The mod-102 update policy therefore names the PCCP boundary as a first-class gate.

**PCCP and mod-107 pre-deployment gate.** The mod-107 pre-deployment gate for a SaMD system carries additional required artefacts: the modification-protocol conformance record for the proposed change, the impact-assessment update if the change alters the pre-specified risk-benefit balance, and the CDRH-facing summary the SaMD change-control lead will file if the change is later inspected. The gate does not authorise deployment of a modification that violates the modification protocol; the SaMD change-control lead has a hard-stop authority in the gate, analogous to the CISO's veto in the mod-112 council. The gate's decision-of-record for a PCCP-covered change is the artefact the CDRH inspector requests first.

**PCCP and mod-110 post-market surveillance.** The modification protocol requires ongoing performance monitoring; mod-110's post-market surveillance shape (chapter 01, EU AI Act Article 72 as enterprise shape) is the mechanism by which the manufacturer discharges the monitoring commitment. The mod-110 PMS record for a SaMD system carries additional fields for the FDA MDR (Medical Device Reporting) obligation under 21 CFR Part 803 — reportable events must reach FDA within the statutory timeline, and the PMS shape's material-incident classification must be co-defined with the MDR reportable-event definition so that a single incident triage produces both the FDA MDR report and the mod-112 council material-incident minute.

**PCCP and mod-112 council.** The council's reserved-matters list acquires two new items: PCCP scope changes (the description of modifications is amended, or a new modification protocol is introduced) and PCCP-boundary-exception ratification (a proposed change is outside the PCCP scope but the enterprise seeks to file a supplement rather than defer the change indefinitely). Both are enterprise-level decisions that bind the SaMD change-control lead's future actions.

## The GMLP guiding principles — enumerated

The FDA / Health Canada / MHRA joint publication of October 2021 sets out ten guiding principles for Good Machine Learning Practice. The health-system blueprint carries them as the acceptable-methodology reference on the SaMD control family's evidence contract; a SaMD submission or PCCP that cannot demonstrate substantive engagement with each principle is at risk of a review-cycle deficiency letter. The ten principles, enumerated by name:

1. Multidisciplinary expertise is leveraged throughout the total product life cycle.
2. Good software engineering and security practices are implemented.
3. Clinical study participants and datasets are representative of the intended patient population.
4. Training datasets are independent of test datasets.
5. Selected reference datasets are based upon best available methods.
6. Model design is tailored to the available data and reflects the intended use of the device.
7. Focus is placed on the performance of the human-AI team.
8. Testing demonstrates device performance during clinically relevant conditions.
9. Users are provided clear, essential information.
10. Deployed models are monitored for performance and re-training risks are managed.

The naming is from the joint publication; the health-system blueprint treats the principles as an evidence-contract line item on the SaMD control family — for each principle, the enterprise names the artefact (or artefacts) that discharge it and the seat that owns the artefact. Principle 3 (representative datasets) and principle 8 (deployment-condition performance) also feed the Section 1557 discrimination-testing composition below.

## HIPAA overlay on training-data provenance and access control

HIPAA is not a change-control regime; it is a data-protection regime whose scope reaches training data, inference-time data, and monitoring data alike. The health-system blueprint composes HIPAA onto mod-108 (evidence architecture, particularly the data-provenance and access-control artefacts) rather than mod-107.

**Training-data provenance.** Where the enterprise trains a SaMD model on protected health information (PHI) drawn from its own operational systems, the training-data provenance artefact (mod-108) acquires HIPAA-specific fields: the legal basis for the use of PHI in training (typically the health care operations use permitted under 45 CFR 164.506, or a specific research authorisation under 45 CFR 164.508 where operations does not cover the use), the de-identification status of the training corpus under the Safe Harbor method (45 CFR 164.514(b)(2)) or the Expert Determination method (164.514(b)(1)), and the minimum-necessary analysis for the fields retained. Where the training corpus is not de-identified, the provenance record must justify the retention of identifiers against the minimum-necessary principle and name the business-associate agreements (BAAs) that cover any downstream processors.

**Access control.** The mod-108 access-control artefact carries the HIPAA Security Rule's administrative, physical, and technical safeguards (45 CFR 164.308, 164.310, 164.312) as required control families for any system that touches PHI. For a SaMD system with a monitoring pipeline that consumes inference-time PHI, the monitoring-pipeline access-control record is a first-class HIPAA-relevant artefact — the pipeline's access controls, audit logging, and encryption in transit and at rest are HIPAA controls, not incidental engineering choices.

**Business-associate agreements.** Where the SaMD manufacturer identity is a subsidiary or affiliate of the health-system provider identity, the intra-enterprise flow of PHI from the provider identity to the manufacturer identity typically requires a BAA, or the two identities must be organised into an OHCA (organised health care arrangement) or an affiliated covered entity structure that permits the flow without a BAA. The mod-108 third-party record for the manufacturer identity — even though it is *part* of the same enterprise — carries the BAA or the OHCA/ACE structure as its legal basis. This is the composition mod-109 (third-party governance) makes possible; the manufacturer identity is a *first-party but externally-regulated* processor from the provider identity's perspective.

**Breach notification.** The Breach Notification Rule (45 CFR 164.400-414) imposes obligations that compose with the mod-110 incident-response and the mod-112 council material-incident record. The 60-day notification clock for a breach of unsecured PHI is a hard timeline the mod-110 material-incident classification must accommodate; a monitoring-pipeline data spill involving PHI is both a mod-110 incident and a HIPAA breach, and the two workflows must be co-driven from a single triage rather than run independently.

## Section 1557 as a required control family — discrimination testing on decision-support tools

The HHS OCR final rule of May 2024 made explicit what had been contested under the earlier iterations of Section 1557: patient-care decision-support tools (the rule uses the term to encompass a broad range of clinical AI, from risk scores to imaging assistants to triage classifiers) are within scope, and covered entities are required to make reasonable efforts to identify and mitigate the risk of discrimination arising from their use. The composition into the enterprise architecture is a required control family, an evidence-contract line item, and a risk-taxonomy attribute.

**Required control family.** The health-system blueprint adds a `1557-nondiscrimination` control family whose applicability filter is *any decision-support tool used in patient-care decisions by the provider identity*. The family's controls include: (a) an intake control that identifies the tool as a decision-support tool subject to the rule and inventories its inputs, outputs, and decision points; (b) a discrimination-testing control that requires the enterprise to conduct testing for disparate performance across the protected classes named in the rule (race, colour, national origin, sex, age, disability); (c) a mitigation control that requires the enterprise to act on identified disparities, whether by retraining, threshold adjustment, restricted-use conditions, or withdrawal; (d) a documentation control that requires the mitigation record to be retained and made available to HHS OCR on request; and (e) a governance control that assigns the discharge of (b), (c), and (d) to a named seat inside the provider identity.

**Composition with mod-106 risk taxonomy and appetite.** The mod-106 risk taxonomy acquires a `discriminatory-clinical-outcome` risk category as a first-class node under harm-to-persons. The enterprise's risk-appetite statement for this category is, in practice, *zero-tolerance for unmitigated known disparate performance in high-severity clinical decisions* — the risk-appetite trigger names both the severity threshold and the mitigation-timeline SLA. A residual above appetite on this category is a mod-112 reserved matter and reaches the council with the mod-107 pre-deployment gate's recommended disposition (typically: do not deploy until mitigation is demonstrated). The Section 1557 control family's evidence contract is what feeds the mod-106 residual: the discrimination-testing artefact is the aggregation input for the risk register's `discriminatory-clinical-outcome` node.

**Where discrimination testing sits with respect to GMLP principle 3.** GMLP principle 3 (representative datasets) is a *development-time* obligation on the manufacturer identity; Section 1557 discrimination testing is a *deployment-time* obligation on the provider identity that consumes the manufacturer's device. The two are not substitutes. A device may be developed with a representative training dataset (satisfying GMLP 3) and still exhibit disparate performance in the specific provider's patient population (triggering Section 1557 mitigation). The enterprise architecture must carry both artefacts; conflating them is the second failure mode below.

## The sector-adaptation record — YAML example

The sector-adaptation record shape from chapter 01 (`./01-the-sector-adaptation-methodology.md`) instantiated for the US health-system blueprint. The example below shows the `applicability` extension whereby a subset of controls acquires the `clinical-decision-support` value on the sector-facing applicability dimension.

```yaml
sector_adaptation_record:
  id: SAR-US-HEALTH-SYSTEM-v1.0
  sector: us-health-system
  identity_scope:
    - manufacturer: samd-manufacturer (CDRH pathway)
    - provider: covered-entity + section-1557-recipient
  anchor_obligations:
    - fda-samd-action-plan-2021
    - fda-gmlp-2021 (joint FDA / Health Canada / MHRA)
    - fda-pccp-final-guidance-2024-12
    - hipaa-privacy-security-breach-notification (45 CFR 160 + 164)
    - aca-section-1557-final-rule-2024-05 (HHS OCR)
    - eu-ai-act-annex-iii (via MDR/IVDR conformity — see needs-research above)
    - state-overlay:
        - ca-ab-3030 (patient-communication disclosure)
        - tx-hb-149-traiga (employment-side interaction)
        - il-hb-3773 (employment-side interaction)

  control_library_extensions:
    applicability_dimension_added:
      name: clinical-context
      values:
        - clinical-decision-support
        - patient-communication-generative
        - imaging-triage
        - clinical-documentation-ambient
        - workflow-non-clinical
    families_added:
      - AIC-SAMD-CHANGE-CONTROL-*      # PCCP composition
      - AIC-GMLP-*                     # ten guiding principles
      - AIC-HIPAA-*                    # privacy, security, breach notification
      - AIC-1557-*                     # nondiscrimination
      - AIC-MDR-REPORTING-*            # 21 CFR Part 803

  example_controls_with_clinical_decision_support_applicability:
    - AIC-107-GATE-CLINICAL-DECISION-SUPPORT
    - AIC-1557-DISCRIMINATION-TESTING
    - AIC-1557-MITIGATION-RECORD
    - AIC-108-PROVENANCE-TRAINING-DATA-PHI
    - AIC-110-PMS-CLINICAL-PERFORMANCE-DRIFT
    - AIC-110-MDR-REPORTABLE-EVENT-TRIAGE
    - AIC-GMLP-3-REPRESENTATIVE-DATASETS
    - AIC-GMLP-7-HUMAN-AI-TEAM-PERFORMANCE
    - AIC-GMLP-8-DEPLOYMENT-CONDITION-PERFORMANCE
    - AIC-GMLP-10-DEPLOYED-MODEL-MONITORING
    - AIC-SAMD-CHANGE-CONTROL-PCCP-BOUNDARY-CHECK
    - AIC-SAMD-CHANGE-CONTROL-MODIFICATION-PROTOCOL-CONFORMANCE
    - AIC-CA-AB3030-PATIENT-COMMUNICATION-DISCLOSURE

  roles_introduced:
    - samd-change-control-lead:
        applies_to: samd-manufacturer identity (always, US-facing)
        accountability:
          - PCCP scope stewardship
          - modification-protocol conformance authority
          - CDRH-facing regulatory correspondence lead
          - hard-stop authority in mod-107 pre-deployment gate for PCCP-covered changes
        seats_at:
          - mod-107 pre-deployment gate (voting)
          - mod-112 council (rotating specialist advisor on SaMD items)
    - clinical-safety-officer-dcb-0129-0160:
        applies_to: uk-operations only (jurisdictional overlay)
        accountability:
          - NHS DCB 0129 (manufacturer safety case) authorship
          - NHS DCB 0160 (deploying-organisation safety case) authorship
          - clinical-risk-management-file (CRMF) ownership
        seats_at:
          - mod-107 pre-deployment gate (voting on UK-deployed systems)
        note: NOT applicable to purely US-facing operations; the record flags it as
              a jurisdictional-overlay role and does not require the seat when the
              UK dimension is out of scope for the adaptation instance.

  invariants:
    - id: I1
      description: any SaMD modification is boundary-checked against the PCCP scope
                   before it can enter the mod-102 profile update workflow
    - id: I2
      description: every patient-care decision-support tool has a live
                   1557-discrimination-testing artefact whose age is within the
                   review interval defined on AIC-1557-DISCRIMINATION-TESTING
    - id: I3
      description: any monitoring-pipeline PHI incident is co-triaged as a mod-110
                   incident AND a HIPAA breach; single-workflow triage is required
    - id: I4
      description: the manufacturer identity's intra-enterprise flow of PHI to the
                   provider identity (or vice versa) has a documented legal basis
                   (BAA / OHCA / ACE) on the mod-108 third-party record
```

## Two roles — the SaMD change-control lead and the clinical safety officer

The blueprint introduces two roles at the sector layer. Their applicability differs; the sector-adaptation record must reflect that.

**SaMD change-control lead.** US-facing, always applies to the SaMD-manufacturer part of the enterprise. Accountable for PCCP scope stewardship (the description of modifications is kept current with the modelling reality; when it drifts, the lead files an amendment or narrows the enterprise's ambition), modification-protocol conformance authority (a change proposed inside the PCCP scope is either conformant with the modification protocol or it is not; conformance is the lead's call), CDRH-facing regulatory correspondence lead (the lead is the enterprise's named point of contact for CDRH inquiries relating to the SaMD's post-authorisation life), and hard-stop authority in the mod-107 pre-deployment gate for PCCP-covered changes. The lead sits on the pre-deployment gate as a voting member for SaMD-relevant items and rotates through the mod-112 council as a specialist advisor when PCCP scope changes or PCCP-boundary-exception ratifications are on the agenda.

**Clinical safety officer (DCB 0129 / DCB 0160).** UK-jurisdiction overlay. Applies only where the enterprise operates in the UK (or deploys through the NHS). Accountable for authoring the DCB 0129 manufacturer-safety-case document (for the manufacturer identity's UK-market products) and the DCB 0160 deploying-organisation-safety-case document (for the provider identity's deployment of any clinical IT, including third-party SaMD, into NHS services). Owns the clinical-risk-management-file (CRMF) and sits on the mod-107 pre-deployment gate as a voting member for UK-deployed systems.

The two roles do not conflict; they occupy different jurisdictions. The sector-adaptation record's `roles_introduced` block flags the clinical safety officer's applicability with the jurisdictional-overlay note so that a US-only instance of the adaptation does not require the seat. The instantiation guidance is: read the enterprise's operating footprint before adding the DCB seat; do not add it because the reference blueprint mentions it.

## Boundary to the pharma blueprint (chapter 05)

The FDA is organised by product type; the sector-adaptation record must be too. **Drugs** — small-molecule pharmaceuticals — are regulated by the Center for Drug Evaluation and Research (CDER). **Biologics** — vaccines, blood products, gene therapies, cellular therapies — are regulated by the Center for Biologics Evaluation and Research (CBER). **Medical devices** — including software as a medical device — are regulated by the Center for Devices and Radiological Health (CDRH). This chapter (03) covers the CDRH pathway. Chapter 05 covers the CDER/CBER pathway; a drug-manufacturer using AI in target discovery, clinical-trial patient stratification, pharmacovigilance signal detection, and manufacturing process control operates under CDER/CBER guidance (e.g. the FDA's 2023 discussion paper on AI/ML in drug development; ICH E6(R3) for clinical-trial AI use where applicable) and not under the SaMD framework this chapter builds against.

An integrated delivery network with an in-house pharmacy programme and a research arm that supports drug-development partnerships must therefore compose the two blueprints. The composition is *not* automatic — the enterprise builds a joint sector-adaptation record that carries the CDRH pathway on the SaMD-manufacturer/provider dimensions and the CDER/CBER pathway on the drug-development-partnership dimension. The `applicability_dimension_added` block from the sector-adaptation record supports the composition: a `regulatory-pathway` dimension with values `cdrh-samd`, `cder-drug`, `cber-biologic`, and `none` disambiguates the controls that fire.

The line the architect must not cross: do not attempt to squeeze pharma AI into the SaMD framework. A model that supports pharmacovigilance signal detection at a drug manufacturer is not a SaMD device; it does not receive a PCCP; the CDRH controls do not apply to it. Chapter 05 will develop the pharma pathway on its own terms.

## Failure modes

**Failure mode 1 — treating the health system as a pure "deployer" and missing the SaMD manufacturer obligations when the system builds its own.** A large integrated delivery network with a data-science team that trains its own clinical models, integrates them into the EHR, and rolls them out across service lines *is* operating as a SaMD manufacturer for those models, whether or not the enterprise has named itself as one. The FDA's enforcement discretion for hospital-developed software has never been unlimited, and the boundary between "clinical decision support" that is excluded from device regulation under section 3060 of the 21st Century Cures Act (as amended by section 520(o) of the Federal Food, Drug, and Cosmetic Act) and clinical decision support that constitutes a device is *narrow* — display of the basis of the recommendation to the clinician, ability of the clinician to independently review the basis, and no time-critical decision-support intent are conditions that must all be met, and many enterprise-built models fail one or more. <!-- needs-research: cite the exact 21st Century Cures Act section 3060 language and the FDA's September 2022 guidance on clinical decision support software to verify the four-part exclusion criteria; the architect authoring the sector-adaptation record must know the current-state boundary. -->

The architectural defence is the mod-102 profile-update policy's SaMD boundary check. Every new enterprise-built model is classified through the SaMD triage at intake: is it a device or is it excluded? If it is a device, the SaMD control family applies from day one and the enterprise cannot back into it later without accumulating enforcement exposure. If it is excluded, the classification is documented with the reason, the reason references the specific exclusion criterion, and the classification is re-examined on any material change. The failure mode is defended against by making the classification a *reserved matter for a designated authority* (the SaMD change-control lead) rather than an implicit engineering assumption.

**Failure mode 2 — reading HIPAA only and missing Section 1557 disparate-impact requirements.** HIPAA is the health-data regulation the enterprise has been reading for two decades; Section 1557's May 2024 final rule is comparatively new and its explicit reach into patient-care decision-support tools is often absorbed as "we already do bias testing" rather than as a distinct required control family with its own evidence contract and enforcement risk. The enterprise's HIPAA compliance can be excellent and its Section 1557 exposure can still be material — the two regulations address different harms, are enforced by different HHS OCR workstreams, and produce different remedies.

The architectural defence is the sector-adaptation record's explicit `AIC-1557-*` control family and the mod-106 risk taxonomy's `discriminatory-clinical-outcome` node. The enterprise cannot deploy a patient-care decision-support tool without the Section 1557 discrimination-testing artefact being live and current; the mod-107 pre-deployment gate lists the artefact as a required input; the mod-110 post-market surveillance shape monitors performance drift by protected class as well as in aggregate. The failure mode is what the architecture prevents by making the control family first-class.

## Summary

The US health-system blueprint anchors the sector-adaptation record on the SaMD change-control obligation because the PCCP discipline is what forces the composition of every other module into a defensible whole. The FDA GMLP guiding principles (October 2021, joint FDA/Health Canada/MHRA) supply the acceptable-methodology reference; the FDA SaMD Action Plan (January 2021) supplies the framing; the December 2024 PCCP final guidance supplies the three-component change-control structure (description of modifications, modification protocol, impact assessment) that composes with mod-102 (profile update policy), mod-107 (pre-deployment gate), mod-110 (post-market surveillance and MDR reporting), and mod-112 (council reserved matters). HIPAA overlays on mod-108's data-provenance and access-control artefacts and drives the intra-enterprise BAA/OHCA/ACE structure between the manufacturer and provider identities. Section 1557's May 2024 final rule adds a required control family — discrimination testing on patient-care decision-support tools — with its own evidence contract, its own mod-106 risk-taxonomy node, and its own risk-appetite trigger. The EU AI Act reaches medical devices through the harmonised-legislation route, and the state overlay (CA AB 3030 on patient communication; TX TRAIGA and IL HB 3773 on the employment side) adds jurisdiction-specific applicability values that the sector-adaptation record carries. Two roles are introduced: the SaMD change-control lead (US-facing, always applies) and the clinical safety officer per NHS DCB 0129/0160 (UK-jurisdiction overlay only, not required for US-only operations). The boundary to chapter 05 is drawn at the FDA centre — CDRH for SaMD, CDER/CBER for drugs and biologics — and integrated enterprises compose the two blueprints through a `regulatory-pathway` applicability dimension. Two failure modes recur — the enterprise that trains its own models and never realises it is a SaMD manufacturer; the enterprise that reads HIPAA and never realises Section 1557 has its own control family — and both are defended against by making the boundary check and the control family first-class in the sector-adaptation record.
