# The US G-SIB banking blueprint — model risk management as the composition anchor

## Why this chapter exists

For a US global systemically important bank (G-SIB), AI governance is not a greenfield programme. The bank has run a model risk management (MRM) programme against Federal Reserve SR 11-7 and OCC Bulletin 2011-12 for over a decade. It has a chief model risk officer, an independent model validation function, a model inventory, a validation-report evidence trail, and a supervisory-examination relationship with its primary federal regulator that reads the MRM programme end-to-end on a rhythm. That programme already governs credit scoring, market risk, capital, anti-money-laundering, and — increasingly and unevenly — machine-learning and generative-AI systems.

The level-50 architect at a G-SIB inherits this. The chapter's proposition is that MRM is not one of several inputs to the AI governance programme; MRM is the **composition anchor** — the pre-existing framework the AI programme extends and reconciles into, rather than the framework the AI programme competes with. Every element the preceding twelve modules built — the AIMS (mod-105), the control library (mod-102), the risk taxonomy (mod-106), the assurance architecture (mod-107), the evidence architecture (mod-108), the third-party programme (mod-109), the post-market surveillance function (mod-110), the GRC-for-AI reference architecture (mod-111), and the AI governance council (mod-112) — attaches to the bank's MRM shape, not the other way round. The architect who reverses this — who builds a parallel AI governance programme alongside MRM — creates the *two-programme* failure mode this chapter's second failure section names, and every examiner conversation for the next five years will re-litigate it.

This chapter fills in the chapter-01 sector-adaptation record (see `01-the-sector-adaptation-methodology.md`) for the US G-SIB sector. It reads the anchor obligations (SR 11-7, OCC Bulletin 2011-12, SR 23-4 as it flows into third-party governance, EU AI Act for EMEA operations, Colorado AI Act SB 24-205 for CO-domiciled activity, OSFI Guideline E-23 for Canadian FI subsidiaries), maps them onto the mod-101 through mod-112 architecture, and names the reserved-matters overlay at the mod-112 council.

## SR 11-7 as the composition anchor

**Federal Reserve Supervisory Letter SR 11-7 — "Guidance on Model Risk Management"** was issued on 4 April 2011 jointly with **OCC Bulletin 2011-12** carrying the same title and substantive text. A G-SIB with a national bank charter falls under both; the joint text means one MRM framework satisfies both supervisors. <!-- needs-research: verify FDIC-issued MRM guidance for insured state non-member banks; some G-SIB entity structures include FDIC-supervised subsidiaries. -->

The SR 11-7 framework names three elements. Every AI governance decision the architect makes at a G-SIB should be checkable against these three:

1. **Robust model development, implementation, and use.** First-line delivery — design, coding, integration, use — including data quality, methodology selection, implementation testing, user documentation, and change control.
2. **Sound model validation.** Second-line challenge — independent review of conceptual soundness, ongoing monitoring, and outcomes analysis. This is where SR 11-7's *effective challenge* requirement lives.
3. **Governance, policies, and controls.** The programme pillar — board and senior-management oversight, policies and procedures, roles and responsibilities, internal audit, and documentation.

SR 11-7 also names two operational disciplines that read as first-class controls on any AI system in scope:

- **Ongoing monitoring** — the validation function tracks model performance on live data, with defined tolerances and revalidation triggers. This is the same activity mod-110 chapter 02 named post-market surveillance for the EU AI Act frame; SR 11-7 ongoing-monitoring evidence *is* PMS evidence, and the mod-108 evidence architecture serialises them into one artefact family.
- **Outcomes analysis** — periodic review of decisions against realised outcomes. For a credit model the window is quarters; for a fraud model, weeks; for a generative-AI system without a clean outcome variable, the analysis translates into a downstream-effect study whose shape the model owner and the validation function negotiate through the mod-108 evidence contract.

**Mapping onto the AIMS.** The bank's ISO/IEC 42001 AIMS (mod-105) treats SR 11-7 as *organisational context* (Clause 4.1) and *interested-party requirements* (Clause 4.2) shaping the Clause 4.3 scope. The Clause 5.3 role list nominates the chief model risk officer as an AI-accountable executive alongside (or in place of) the chief AI officer. The Annex A control the AIMS instantiates for AI system validation *is* the SR 11-7 validation function — same function, extended attributes for AI-specific evaluation the mod-102 catalog entry names.

## Effective challenge and the second-line challenger

SR 11-7's most-cited phrase is **effective challenge** — the requirement that model validation be performed by parties who are independent of model development and use, who possess the incentive, competence, and influence to challenge the model, and whose findings are acted upon. The phrase is the interpretive key to how the MRM framework composes with the mod-107 three-lines architecture.

The **challenger function** is:

- **Not the developer.** SR 11-7 requires organisational separation. The quantitative modeller who builds the credit-risk model does not validate it; the ML engineer who trained the classifier does not validate it; the fine-tuning team that produced a customised generative-AI model does not validate it. Where scale prevents complete separation, compensating controls must document the residual dependency.
- **Not internal audit.** Validation is second-line; audit is third-line. The mod-107 chapter 01 invariant that third-line does not pre-negotiate findings applies directly: internal audit reviews *the MRM programme* including the validation function's performance, but does not itself perform validation. A hybrid where audit performs validation-like activities reads to examiners as a validation-independence deficiency.
- **The second-line model validation function.** Typically reporting to the chief risk officer or chief model risk officer, with a mandate distinct from both first-line risk-taking units and third-line audit.

The **challenger's mandate** covers three activities on every in-scope model: evaluation of conceptual soundness (approach, theory, assumptions), ongoing monitoring design (metrics, thresholds, triggers), and outcomes-analysis design (window, comparison, patterns implying revalidation).

**Staffing.** SR 11-7 requires resource, competence, and stature to perform effective challenge. For AI systems the competence bar rises: the validator must be able to evaluate the ML methodology, training data lineage, evaluation protocol, and — for foundation-model-based systems — the composition of provider-supplied behaviour with enterprise fine-tuning and prompting. The architect's contribution is a **validator competency register** listing per-AI-model-family the competencies the validator must hold or engage. External specialist review is a legitimate compensating control where competence gaps exist — documented, minuted, subject to mod-109 third-party governance.

**Evidence trail.** Every validation activity produces a validation report — model description, conceptual-soundness evaluation, ongoing-monitoring design, outcomes-analysis design, findings, limitations, restrictions on use, and validator sign-off with named individual and date. The architect maps this onto the mod-108 evidence architecture as a first-class artefact type linked to the AI system record in the inventory, the model in the mod-102 SoA, and the pre-deployment gate decision (mod-107 chapter 02).

## Model inventory as SR 11-7 requirement

SR 11-7 requires a firm-wide model inventory covering every model in use, under development, or recently retired. The inventory is a supervisory-examination artefact — an examiner asks for the inventory as part of the MRM programme review, and the inventory is expected to be current, complete, and reconciled to the general ledger where models drive financial reporting.

**The architectural move is that the SR 11-7 model inventory and the mod-108 AI system inventory are the same inventory** — one authoritative record, one system-of-record, with extended attributes covering both frames. The alternative — two inventories, one for MRM and one for AI governance — produces reconciliation debt that examiners find within one cycle and that internal audit finds every cycle.

The unified inventory carries, at minimum, the following attribute families:

- **Identity and lineage** — model id, name, version, owner (first-line business), model developer, model validator (assigned), model use.
- **SR 11-7 attributes** — materiality tier (per internal MRM tiering), validation status (validated / conditionally validated / not validated / restricted use), last validation date, next revalidation date, findings status (open / closed / accepted-with-restriction), user population.
- **AI-specific extended attributes** — model class (traditional statistical / classical ML / deep learning / foundation-model-based / agentic-composition), training-data lineage reference, provider (if third-party foundation model), fine-tuning artefact reference, evaluation-protocol reference, mod-106 risk taxonomy classification, mod-107 assurance-tier, jurisdiction-applicability set (EU AI Act, Colorado AI Act, OSFI E-23 as applicable — see below).
- **Monitoring and outcomes** — ongoing-monitoring cadence, outcomes-analysis window, monitoring-dashboard reference, PMS reporting hook (mod-110 chapter 03) where applicable.

**Materiality tiering.** SR 11-7 expects a consistent tiering scheme with clear implications for validation depth and frequency. Most G-SIBs run three or four tiers driving validation frequency (annual for material, biennial or triennial for lower tiers), depth (independent replication vs desk review), and revalidation triggers. For AI systems the scheme reads across mod-107's assurance-tier design; the architect reconciles the two so a system's SR 11-7 tier and its mod-107 assurance tier are consistent, or the difference is documented.

## SR 23-4 and third-party model risk

**SR 23-4 / OCC Bulletin 2023-17 / FDIC FIL-29-2023 — "Interagency Guidance on Third-Party Relationships: Risk Management,"** issued 6 June 2023 by the Federal Reserve, OCC, and FDIC jointly. Superseded the individual agencies' prior third-party guidance (Fed's SR 13-19, OCC's Bulletin 2013-29 for large banks, FDIC's FIL-44-2008). <!-- needs-research: verify the exact SR and bulletin numbers and issuance date; the June 2023 interagency guidance is definitively the current instrument but the SR number should be pinned. -->

The guidance covers the full third-party lifecycle — planning, due diligence and selection, contract negotiation, ongoing monitoring, and termination — with board-level oversight where materiality warrants.

**Flowing into mod-109 as a G-SIB overlay.** The bank's mod-109 third-party programme reads SR 23-4 as its supervisory anchor and layers AI-specific extensions on top:

- **Inventory** — every third party carries an SR 23-4 materiality tier alongside the mod-109 AI-exposure classification. Reconciliation between the two tierings is a governance-council reserved matter (see below).
- **Pre-contract due diligence** — the mod-109 diligence checklist for AI providers (evaluation-protocol disclosure, model-behaviour disclosure, subprocessor list, incident-notification commitments) attaches to the SR 23-4 diligence process as one integrated process.
- **Contract** — SR 23-4 clauses (audit rights, data ownership, termination, subcontracting) and mod-109 AI-specific clauses (evaluation-disclosure obligations, provider-side incident notification, model-behaviour-change notification, provider Responsible Scaling Policy adherence) are co-drafted by legal, sourcing, and the head of AI governance.
- **Ongoing monitoring** — the pack includes the provider's periodic evaluation disclosures, incident notifications received, behaviour-change notifications, and independent evidence of contract compliance.
- **Termination** — SR 23-4's exit planning with AI-specific extensions on data return (training- and user-data retention posture) and model-behaviour continuity (successor replication vs different capability).

Material third-party relationships in a G-SIB are typically minuted at the board's risk committee as well as the mod-112 council; the sector-adaptation record names this as a delta from the generic mod-112 shape.

## OSFI Guideline E-23 for Canadian FI subsidiaries

G-SIBs typically operate Canadian FI subsidiaries (federally regulated by OSFI). **OSFI Guideline E-23 — "Enterprise-Wide Model Risk Management for Deposit-Taking Institutions"** was originally issued in 2017 and revised in 2024 to broaden the scope and update the framework. <!-- needs-research: verify the current version and effective date of OSFI E-23; the 2024 revision expanded scope beyond DTIs to insurers and to AI/ML systems explicitly. Confirm the exact title and revision date. -->

E-23 differs from SR 11-7 in four architecturally material ways:

- **Enterprise-wide framing.** E-23 requires MRM at the enterprise level with board-level accountability. For a US G-SIB's Canadian subsidiary the framework is inherited from the parent but the *board of the Canadian entity* carries the accountability; the mod-112 council delegates the Canadian view to the subsidiary board with a reporting hook back.
- **In-scope models enumeration.** E-23 defines "model" broadly — AI/ML, quantitative methods, and (in the 2024 revision) generative-AI applications — expecting explicit scope inclusions and exclusions with rationale. The architect publishes an enterprise-wide scope with the Canadian-inclusion delta annotated; models the parent scope excludes but E-23 includes carry E-23 evidence in mod-108.
- **Board-level oversight expectations.** Specific board responsibilities (MRM policy approval, programme-effectiveness review, material-findings review). The mod-112 reserved-matters list acquires an E-23 overlay routing through the Canadian board.
- **Risk-based tiering.** Defined tier criteria, validation depth by tier, monitoring cadence by tier — architecturally similar to SR 11-7 but with potentially different tier definitions; the reconciliation record names the mapping.

**Composition move.** The parent MRM framework is the base; E-23 is a jurisdiction-applicable overlay on the applicability filter of relevant controls. Mod-102 catalog entries carry an `applicability.jurisdictions` set naming `US-Fed-SR-11-7`, `US-OCC-2011-12`, `CA-OSFI-E-23` explicitly. The Canadian-entity SoA is a derivation of the parent SoA with E-23-triggered controls added and US-only controls filtered out where not required.

## Colorado AI Act SB 24-205 — consumer-lending overlay

**Colorado Consumer Protections for Artificial Intelligence Act (SB 24-205)**, signed 17 May 2024, effective 1 February 2026. Regulates deployers and developers of *high-risk AI systems* that make or are a substantial factor in making *consequential decisions* affecting Colorado consumers. Consequential-decision domains explicitly include financial or lending services. <!-- needs-research: verify the effective date has not been amended by subsequent Colorado legislative session; the CO legislature has considered amendments to SB 24-205. -->

**Applicability to a G-SIB.** The act attaches when the bank is a deployer of a high-risk AI system that is a substantial factor in a consequential decision affecting a Colorado consumer. Consumer-lending decisions — credit-card underwriting, personal-loan underwriting, mortgage underwriting, credit-line adjustments — are within scope where AI is materially involved in the decision. Business-lending decisions with individual guarantors may also be in scope depending on how the consumer definition applies.

**Obligation shape.** SB 24-205 imposes duties on deployers including (as materially relevant to a G-SIB): a risk-management programme aligned to a recognised framework (NIST AI RMF and ISO/IEC 42001 are named as acceptable); an impact assessment updated annually and on material modification; consumer disclosure of AI use plus, on adverse decision, an explanation and correction opportunity; and attorney-general notification of discovered algorithmic discrimination within an obligation window.

**Architectural move.** The mod-104 reconciled control set already carries the Colorado obligations. The G-SIB-specific addition is the *consumer-lending-applicability filter*: the existing ECOA / adverse-action-notice control family acquires a CO-consumer-jurisdiction overlay mandating the SB 24-205 disclosure and correction-opportunity artefacts alongside the federal ECOA artefact. The impact-assessment obligation composes with the mod-105 AIMS impact-assessment shape and the mod-108 DPIA/AIA-adjacent artefact family; one impact assessment per CO-in-scope AI system carries the SB 24-205 required content.

## EU AI Act Annex III paragraph 5(b) — creditworthiness

For G-SIB EU operations (EMEA subsidiaries; EU customers of US-domiciled products in ways that put the bank within EU AI Act territorial scope), **EU AI Act Annex III paragraph 5(b)** classifies AI systems intended to be used for evaluating the creditworthiness of natural persons or for establishing their credit score as *high-risk*, with the exception (as of the final adopted text) of AI systems used for the purpose of detecting financial fraud. <!-- needs-research: verify the exact Annex III paragraph number and the fraud-detection carve-out language against the final adopted text of Regulation (EU) 2024/1689. -->

**Applicability.** Consumer-credit scoring, personal-loan and mortgage underwriting, and credit-limit-adjustment AI where the customer is a natural person and the bank is within EU AI Act scope trigger Annex III. Chapter III Section 2 obligations then attach: risk-management system (Article 9), data governance (Article 10), technical documentation (Article 11), record-keeping / logging (Article 12), transparency to deployers (Article 13), human oversight (Article 14), accuracy / robustness / cybersecurity (Article 15), plus post-market monitoring (Article 72), incident reporting (Article 73), and conformity assessment.

**Architectural move.** These obligations *compose* with SR 11-7 rather than duplicating it: Article 9 is the SR 11-7 MRM framework with EU-specific deliverables added; Article 10 is the SR 11-7 data-quality expectation with EU-specific documentation; Article 12 is the SR 11-7 model-documentation and validation-report chain with EU logging added; Article 72 is the SR 11-7 ongoing-monitoring discipline serialised into the mod-110 PMS shape. The architect authors the library so that a credit-scoring model has one validation-report artefact, one ongoing-monitoring artefact, one outcomes-analysis artefact, and those satisfy SR 11-7, EU AI Act Annex III + Chapter III, and (for CO-consumer-jurisdiction hits) Colorado AI Act. Three artefact families for three regulators is the two-programme failure mode multiplied.

## The sector-adaptation record — US G-SIB

Filling in the schematic chapter 01 of this module defines (see `01-the-sector-adaptation-methodology.md`), the US G-SIB sector-adaptation record reads:

```yaml
sector_adaptation_record:
  id: SAR-US-GSIB-v1.0
  sector: financial-services / banking / US-G-SIB
  status: ratified
  ratified_by: ai-governance-council (mod-112)
  ratification_minute: AIGC-<id>-<date>

  in_scope_entities:
    - US-national-bank (OCC-supervised)
    - US-Fed-supervised-bank-holding-company
    - CA-FI-subsidiary (OSFI-supervised)
    - EU-EMEA-subsidiary (EU-AI-Act-in-scope)
    - CO-operating-consumer-lending-book (CO-AI-Act-in-scope)

  anchor_obligations:
    primary:
      - fed-sr-11-7 (Guidance on Model Risk Management, 4 April 2011)
      - occ-bulletin-2011-12 (same text, joint issuance)
    third_party:
      - fed-sr-23-4 / occ-bulletin-2023-17 / fdic-fil-29-2023 (Interagency Third-Party Guidance, 6 June 2023)
    ca_subsidiary:
      - osfi-guideline-e-23 (Enterprise-Wide Model Risk Management, 2024 revision)
    eu_scope:
      - eu-ai-act-annex-iii-para-5b (creditworthiness high-risk classification)
      - eu-ai-act-articles-9-through-15 (high-risk obligations)
      - eu-ai-act-article-72 (post-market monitoring)
      - eu-ai-act-article-73 (serious-incident reporting)
    co_scope:
      - co-sb-24-205 (Colorado AI Act, effective 1 February 2026)
    horizontal_us:
      - ecoa-adverse-action-notice-regime
      - fair-lending-regime (Regulation B, HMDA reporting)
      - ftc-act-section-5 (unfair or deceptive acts)

  composition_anchor: sr-11-7-mrm-framework
  # AI programme extends MRM; does not run parallel to MRM.

  aims_delta:
    scope_statement:
      - includes: all in-scope AI systems as defined per mod-108 inventory
      - excludes: rule-based systems below the mod-105 AIMS scope threshold
      - explicit_exclusion_rationale: documented per system class
    clause_5_3_roles:
      ai_accountable_executive: shared between chief-ai-officer (where role exists) and chief-model-risk-officer
      second_line_validation_owner: chief-model-risk-officer
      first_line_delivery_owner: head-of-model-development / relevant business unit head
    clause_9_3_management_review:
      integrated_with: annual-MRM-programme-review-to-the-board
      forum: ai-governance-council quarterly + board risk committee annual

  control_library_delta:
    inheritance_from: enterprise-mod-102-catalog
    additions:
      - AIC-BANK-VAL-01: independent-validation-of-AI-model (SR 11-7 §V-A)
      - AIC-BANK-CHAL-01: effective-challenge-record (SR 11-7 §III)
      - AIC-BANK-OM-01: ongoing-monitoring-of-model-performance (SR 11-7 §V-B)
      - AIC-BANK-OA-01: outcomes-analysis-window (SR 11-7 §V-B)
      - AIC-BANK-INV-01: enterprise-model-inventory-attributes-SR-11-7 (SR 11-7 §VII)
      - AIC-BANK-3P-01: third-party-model-diligence-SR-23-4
      - AIC-BANK-3P-02: third-party-ongoing-monitoring-SR-23-4
      - AIC-BANK-EU-01: creditworthiness-annex-iii-conformity (EU AI Act)
      - AIC-BANK-CO-01: consumer-adverse-decision-disclosure-and-correction (CO SB 24-205)
      - AIC-BANK-CA-01: canadian-fi-subsidiary-e-23-overlay
    filter_extensions:
      applicability.jurisdictions: [ US-Fed-SR-11-7, US-OCC-2011-12, CA-OSFI-E-23, EU-AI-Act, US-CO-AI-Act ]
      applicability.line_of_business: [ consumer-credit, commercial-credit, market-risk, operational-risk, AML-fraud, capital-and-regulatory-reporting ]

  risk_taxonomy_delta:  # mod-106
    additions:
      - discriminatory-lending-outcomes (Regulation B + fair-lending)
      - CECL-model-error (credit-loss-estimation risk on financial-reporting models)
      - stress-test-model-error (CCAR / DFAST scope)
      - CO-AI-Act-consumer-notice-failure
      - EU-AI-Act-Article-72-PMS-reporting-lapse

  assurance_architecture_delta:  # mod-107
    three_lines_naming:
      first_line: model-development-and-business-use
      second_line: model-validation-function (SR 11-7 effective challenge)
      third_line: internal-audit (SR 11-7 §VI)
    pre_deployment_gate:
      composed_with: MRM-pre-implementation-validation-gate
      # One gate; validation and AI-assurance criteria are co-adjudicated.

  evidence_architecture_delta:  # mod-108
    unified_inventory: yes  # SR 11-7 model inventory == AI system inventory
    extended_attributes: as listed in "Model inventory" section above
    artefact_families_unified:
      - validation-report (SR 11-7) == conformity-technical-file evidence (EU AI Act Art 11) with joint schema
      - ongoing-monitoring-record (SR 11-7) == PMS-record (EU AI Act Art 72) with joint schema
      - outcomes-analysis-report (SR 11-7) — G-SIB-native; feeds mod-110 PMS

  third_party_delta:  # mod-109
    overlay: SR-23-4-lifecycle
    board_level_material_relationships: yes  # certain relationships minuted at board risk committee

  post_market_surveillance_delta:  # mod-110
    integration: SR-11-7-ongoing-monitoring is the primary PMS mechanism
    incident_reporting_pipeline:
      - internal-material-incident (mod-110 severity schema)
      - EU-AI-Act-Article-73-serious-incident (EU-scope)
      - Colorado-AG-notification (CO-scope discriminatory outcomes)
      - Fed / OCC supervisory notification (per bank's Reg-defined obligations)

  governance_council_reserved_matters_overlay:  # mod-112
    additions:
      - SR-11-7-material-findings-acceptance
      - OSFI-supervisory-letter-response (E-23 or otherwise)
      - board-level-material-third-party-relationships (SR 23-4)
      - EU-AI-Act-Article-73-serious-incident-disposition
      - CO-AI-Act-AG-notification-disposition
      - MRM-policy-amendments (parent + CA subsidiary)

  invariants:
    - { id: I1, description: one MRM framework extended for AI; no parallel programme, test: enterprise programme inventory shows one MRM programme covering AI }
    - { id: I2, description: validator is not developer, not internal audit, test: any in-scope AI model's validator seat is second-line }
    - { id: I3, description: one inventory, joint attributes, test: sample any in-scope model; both attribute families populated in the single inventory }
    - { id: I4, description: examiner-facing artefacts satisfy multiple frames, test: sample validation report renders as SR 11-7 and as EU AI Act Article 11 evidence }
```

## The reserved-matters overlay at the AI governance council

Mod-112 chapter 01 defined the AI governance council's reserved-matters list as a positive enumeration. For a G-SIB the list acquires a sector overlay — items the council minutes because they belong to no other enterprise forum and the enterprise cannot delegate them lower:

- **SR 11-7 material findings acceptance.** The validation function issues findings against models it validates; findings above a materiality threshold that the model owner disputes, or that require a policy-level exception, land at the council for adjudication.
- **OSFI supervisory letter response.** For the Canadian subsidiary, an OSFI supervisory letter arising from an E-23-scoped examination is minuted at the council with the response posture and the executional accountability for remediation.
- **Board-level material third-party relationships (SR 23-4).** Onboarding, material change, or exit for third-party relationships whose materiality invokes board-level oversight is minuted at the council with a downstream reporting hook to the board's risk committee.
- **EU AI Act Article 73 serious-incident disposition.** A serious incident subject to Article 73 reporting is minuted at the council with the reporting posture and the disposition.
- **Colorado AI Act AG notification disposition.** A discovery of algorithmic discrimination triggering CO AG notification is minuted at the council with the notification posture and disposition.
- **MRM policy amendments.** Parent MRM policy amendments and Canadian-subsidiary MRM policy amendments (where they diverge) are ratified by the council with the CA-board reporting hook active.

These extend rather than replace the mod-112 baseline. The council charter is amended to name the overlay explicitly, and a business-unit executive representative (consumer-lending or commercial-banking) is brought into voting membership if enterprise scale warrants.

## Failure modes

**Failure mode 1 — treating AI as separate from MRM and building a parallel programme.** A common pattern in G-SIBs is that the AI programme starts in a chief-data-office or chief-technology-office reporting line, disconnected from the chief model risk officer. The AI programme builds its own inventory, its own validation-like review function, its own governance forum, and its own evidence trail. The MRM programme continues in parallel with its own inventory, its own validation function, its own governance forum, and its own evidence trail. Within one supervisory cycle the examiner discovers that models are in the AI inventory but not the MRM inventory (or vice versa), that AI-programme reviews are not SR 11-7-compliant validation, and that the two evidence trails do not reconcile. The bank is written up for MRM programme deficiencies; the AI programme is subsumed into MRM by supervisory pressure; the two years of programme-build effort delivers a smaller result than the merger-from-day-one would have. The architectural defence is the composition-anchor decision this chapter argues: MRM is the anchor, the AI programme extends it, and the two evidence trails are one from the start.

**Failure mode 2 — reading only the horizontal frame and missing OCC / Fed examiner expectations.** An architect steeped in mod-101 through mod-112 who reads EU AI Act, NIST AI RMF, and ISO/IEC 42001 as the primary references and does not anchor to SR 11-7 will produce a control library that is technically credible against the horizontal frame but reads to a Fed or OCC examiner as *missing the point*. The examiner is not looking for AI RMF sub-category coverage; the examiner is looking for effective-challenge evidence, validation-report discipline, ongoing-monitoring cadence, and outcomes-analysis rigour on every in-scope AI system. The architect who cannot answer *how does this satisfy SR 11-7?* on a per-control basis is not defensible in the supervisory dialogue. The defence is the composition move: every control that touches an in-scope model carries an SR 11-7 crosswalk edge in its mod-102 catalog entry, and the evidence contract renders artefacts that read as SR 11-7 validation evidence first and as horizontal-frame evidence in the same document.

## Summary

For a US G-SIB, SR 11-7 is the composition anchor for the entire AI governance programme. The bank's decade-plus MRM programme is not one input among many; it is the framework the AI programme *extends*, and every mod-101 through mod-112 artefact attaches to the MRM shape. The three SR 11-7 pillars (robust development / implementation / use; sound validation; governance / policies / controls) map onto the mod-107 three-lines and the mod-105 AIMS Clause 5.3 role design directly. Effective challenge is discharged by the second-line validation function — not the developer, not internal audit — with a validation-report evidence trail the mod-108 architecture serialises. The model inventory is one inventory with SR 11-7 attributes and AI-specific extended attributes co-populated. SR 23-4 flows into mod-109 as the third-party-lifecycle anchor. OSFI E-23 governs the Canadian FI subsidiary with an enterprise-wide framing, in-scope enumeration, and board-level oversight delta accommodated through applicability-filter jurisdictions. Colorado AI Act SB 24-205 attaches to consumer-lending consequential decisions with disclosure, correction-opportunity, and AG-notification obligations. EU AI Act Annex III paragraph 5(b) triggers Chapter III high-risk obligations on credit-scoring AI for EMEA operations, composing with SR 11-7 rather than duplicating it. The mod-112 reserved-matters list acquires an overlay for SR 11-7 material findings, OSFI supervisory letters, board-level material third-party relationships, EU Article 73 dispositions, CO AG notifications, and MRM policy amendments. Anchor to MRM, extend outward to the horizontal and jurisdiction-specific frames, run one inventory and one evidence trail, and the G-SIB blueprint composes.
