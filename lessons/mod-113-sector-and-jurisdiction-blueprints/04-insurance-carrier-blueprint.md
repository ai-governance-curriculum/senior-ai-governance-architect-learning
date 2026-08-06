# The insurance carrier blueprint — the NAIC model bulletin as a per-state activation

## Why this chapter exists

A US insurance carrier writing across multiple states operates under a supervisory frame that no horizontal AI statute captures. The National Association of Insurance Commissioners (NAIC) is not a regulator — it is a standard-setting body whose model bulletins acquire legal force only when an individual state department of insurance (DOI) *adopts* the model, verbatim or with amendments. The NAIC AI Model Bulletin on the Use of Artificial Intelligence Systems by Insurers (adopted December 2023) is the current shape. Colorado has gone further with a state-specific rulemaking (Insurance Regulation 10-1-1) that operationalises a Colorado statute (SB 21-169) and imposes quantitative testing methodology requirements no other state has yet matched. Sitting alongside these is ongoing state-DOI market-conduct examination activity, independent of any specific bulletin. For carriers with EU operations, the EU AI Act's Annex III classification of AI systems used in life-and-health-insurance underwriting and pricing as *high-risk* applies.

The level-50 architect's problem is that the reconciliation architecture (mod-104) has to carry a *per-state activation* dimension the horizontal frames do not need. A US G-SIB bank's SR 11-7 obligation is nationwide because the Federal Reserve is a federal supervisor; a carrier's NAIC-bulletin obligation is not, because the NAIC has no supervisory authority — every applicability filter runs through the adopting-state list. Within Colorado, the filter is not "Colorado" but "life insurers using External Consumer Data and Information Sources (ECDIS), algorithms, or predictive models in specified operational contexts", because Regulation 10-1-1's statutory basis is Colorado-specific and narrower than a naive "insurance-plus-AI" filter.

This chapter designs the reference AIMS-plus-control-library instantiation for a multi-state carrier, composes mod-102 through mod-112 for that shape, and writes the sector-adaptation record filling in the schematic defined in `01-the-sector-adaptation-methodology.md`.

## The NAIC model bulletin — programme structure

The NAIC AI Model Bulletin on the Use of Artificial Intelligence Systems by Insurers was adopted by the NAIC in December 2023. It does not create new statutory obligations; it interprets *existing* insurance-code obligations (unfair trade practices, market conduct, rate and form filing, unfair discrimination) as they apply to AI system use. Its programme structure has four load-bearing sections that map cleanly onto mod-102 control families:

- **Governance and risk management framework.** The carrier is expected to have a written AI programme with board or senior-management accountability, documented AI system inventory, risk-tiering by decision impact and consumer harm potential, defined roles and responsibilities, training, and a lifecycle framework covering development, validation, deployment, and retirement. This composes with mod-105 (AIMS) and mod-108 (the evidence architecture supporting the inventory).
- **Testing and validation.** The carrier is expected to test AI systems for performance and for unfair discrimination *before deployment* and *on an ongoing basis after deployment*. This composes with mod-107 (assurance architecture; the pre-deployment gate carries the pre-deployment test) and mod-110 (post-market surveillance; the monitoring architecture carries the ongoing test).
- **Third-party AI systems and data.** The carrier is expected to conduct due diligence on third-party AI system providers and third-party data sources, obtain sufficient contractual rights to evaluate performance and discrimination, and maintain oversight commensurate with the risk. This composes with mod-109 (third-party governance) with a sector-specific overlay walked in a later section.
- **Documentation and record-keeping.** The carrier is expected to maintain documentation sufficient for the DOI to conduct examinations — including the AI programme itself, decisions taken under it, testing methodology and results, and third-party arrangements. This composes with mod-108 (evidence architecture) with a record-retention overlay tuned to state examination cycles.

The mod-102 control families the bulletin activates are the shape-A extensions of the enterprise reference library — no shape-B controls are required *by the bulletin itself* — with the following anchoring:

```yaml
naic_bulletin_to_control_family_map:
  section_1_governance_and_risk_management:
    control_families: [ AIC-GOV-*, AIC-INV-*, AIC-ROLE-*, AIC-LC-* ]
    aims_anchor: ISO/IEC 42001 Clauses 4-7
  section_2_testing_and_validation:
    control_families: [ AIC-VAL-*, AIC-BIAS-*, AIC-MON-* ]
    aims_anchor: ISO/IEC 42001 Clauses 8-9
  section_3_third_party:
    control_families: [ AIC-3P-* ]
    aims_anchor: ISO/IEC 42001 Clause 8.1 + Annex A third-party controls
  section_4_documentation_and_records:
    control_families: [ AIC-DOC-*, AIC-RET-* ]
    aims_anchor: ISO/IEC 42001 Clause 7.5
```

The applicability filter on every control activated by the bulletin carries an `addressee_scope` field listing the *adopting states* — see the per-state activation section. Non-adopting-state operation does not activate the bulletin-derived obligations, though it may activate the state's underlying unfair-trade-practices statute directly.

<!-- needs-research: the number of states that have adopted the NAIC AI Model Bulletin (verbatim, with amendments, or via a bulletin of their own tracking the model closely) as of the current date; NAIC maintains an adoption tracker but the count evolves quarterly and should be verified against the tracker. -->

## Unfair-discrimination testing as a required control family

The bulletin's governance and testing sections point at unfair-discrimination testing without specifying methodology; state insurance codes' prohibitions on unfair discrimination in rating and underwriting are the substantive obligations. The bulletin's contribution is to make the testing a *programme requirement* — the carrier must have a testing methodology, must apply it, and must retain the results — as a matter of the AI programme itself, not only of rate-filing defence.

**Colorado Insurance Regulation 10-1-1** operationalises Colorado SB 21-169 (the "Insurers' Use of External Consumer Data and Information Sources, Algorithms, and Predictive Models" statute enacted in 2021). It applies to Colorado-licensed life insurers using ECDIS, algorithms, or predictive models in life-insurance underwriting; the DOI has signalled intent to extend the framework to other lines through subsequent rulemakings.

<!-- needs-research: verify the current scope of Colorado Regulation 10-1-1 (life-only or extended) and the status of related rulemakings for auto, homeowners, and health; the Colorado DOI's rulemaking calendar is authoritative. -->

Regulation 10-1-1 requires subject insurers to file *Governance and Risk Management Framework* reports and, distinctively, *Testing Methodology* reports with the Colorado DOI. The Testing Methodology filing is the quantitative substance: the insurer describes the methodology it uses to test ECDIS-and-algorithm use for unfair discrimination against protected classes, and the DOI retains authority to examine, comment on, and demand revision. The regulation names disparate-impact analysis, drivers analysis, and remediation approaches, and expects the insurer to specify the statistical methods, thresholds, and reference populations it uses.

The architectural composition for a carrier operating in Colorado runs through mod-102 with the following shape:

- **`AIC-BIAS-CO-TEST-METHODOLOGY-01`** — applicability scoped to Colorado life-insurance operations under Regulation 10-1-1. Its evidence contract includes the filed Testing Methodology document, the DOI's comment history on prior versions, the internal validation record against holdout data, and the actuarial-model-governance lead's sign-off (see roles section).
- **`AIC-BIAS-ENTERPRISE-TEST-METHODOLOGY-01`** — the enterprise's AI-programme testing regime across all lines and jurisdictions, with a *composes-with* edge to the Colorado control. The enterprise methodology is a superset; the Colorado-scoped control is the *filing shape* of the enterprise methodology, restricted to the Colorado-permissible protected-class taxonomy and the DOI-specified reference populations.

The composition matters because the Colorado protected-class taxonomy (race, colour, national origin, ancestry) is a subset of what the enterprise might separately test under EU AI Act Article 10 or sector adjacencies (Section 1557 in a health-insurance context; ECOA-adjacent frames on credit-scoring inputs). The enterprise runs one testing pipeline that produces filing-shaped outputs for Colorado, EU-shaped outputs for Annex III applicability, and enterprise-shaped outputs for internal risk management. If the pipelines are separate the carrier drifts between them and a DOI examination surfaces the inconsistency. The architectural defence is that the Colorado control's evidence contract *specifies* that the filed methodology is the enterprise methodology rendered through the Colorado filter, with mod-108 holding the rendering rules as first-class artefacts.

## The third-party AI vendor overlay from the NAIC bulletin

The NAIC bulletin's third-party section is prescriptive in a way mod-109 anticipates but a general third-party programme does not always reach. Two elements are load-bearing:

- **Contractual rights to evaluate.** The carrier must secure contractual rights to evaluate third-party AI system performance and to test for unfair discrimination. The mod-109 vendor contract template carries these as non-negotiable clauses for any vendor whose AI system informs an underwriting, rating, claims-handling, marketing, or fraud-detection decision.
- **Oversight commensurate with risk.** The carrier remains accountable even where the vendor operates the model. Vendor-provided testing does not discharge the obligation; the carrier must independently verify or, where infeasible, rely on vendor testing under a documented framework the DOI can examine.

The composition with mod-109 runs through a sector-specific vendor tier:

```yaml
sector_specific_vendor_tier_naic:
  tier_id: INSURANCE-DECISION-VENDOR
  applicability: vendors whose AI system informs underwriting, rating, claims-handling, marketing, or fraud-detection in an adopting-state jurisdiction
  overlays_on: mod-109 tier-2-or-above baseline
  additional_requirements:
    - contractual: rights to evaluate performance and test for unfair discrimination
    - contractual: vendor disclosure of material changes to model, training data, or evaluation methodology
    - contractual: cooperation with DOI examinations flowing through the carrier
    - operational: annual independent testing where feasible; documented reliance framework where not
    - operational: vendor testing results in the mod-110 monitoring pack
    - operational: escalation to the AI governance council on material vendor posture change
  evidence_contract:
    - contract clauses executed; due-diligence packet at onboarding
    - annual re-attestation record
    - independent-testing results or reliance-framework rationale
```

Failure mode: the mod-109 programme treats the AI-underwriting vendor as a standard technology supplier. The DOI examines a claims-denial pattern, requests documentation of the vendor's testing regime, and the carrier's response is that the vendor holds the artefacts. The DOI's response is that the carrier is accountable; the response is inadequate. The architectural defence is the sector-tier overlay that surfaces the AI-decision-vendor as its own category with its own evidence contract.

## EU AI Act Annex III applicability for life and health insurance

For the carrier with EU operations — an EU subsidiary, a reinsurance branch, or cross-border exposure meeting the Act's territorial scope — the EU AI Act's Annex III classification of AI systems used in *life and health insurance* risk assessment and pricing as *high-risk* activates a distinct obligation set. Annex III's insurance paragraph is narrower than a naive reading suggests: it covers *life and health* insurance in the *risk assessment and pricing of natural persons*, and does not sweep in property-and-casualty rating or non-natural-person cover.

<!-- needs-research: verify the specific paragraph number in Annex III on life and health insurance in the enacted consolidated text and any subsequent delegated-act adjustments; the scope language is enacted but the paragraph number requires verification. -->

The applicability transforms the enterprise obligation set for the covered systems:

- The carrier is a *provider* (in-house model) or *deployer* (third-party model) under EU AI Act definitions, with the reconciled obligation set in mod-104.
- Data-governance, technical-documentation, record-keeping, transparency, human-oversight, and accuracy-robustness-cybersecurity obligations (Articles 10-15) all activate as shape-A extensions of existing controls.
- Post-market monitoring obligations under Article 72 (composed with mod-110 chapter 01) activate, with insurance-specific incidents (adverse pricing outcomes systematically affecting protected categories under EU non-discrimination law) inside the monitoring scope.
- Fundamental-rights impact assessment under Article 27 may activate depending on the deployer's public-service nature; a typical private-carrier operation is not caught, but the applicability filter must resolve the question.

Both the NAIC bulletin and Annex III can activate on the *same underlying AI system* when it is used in adopting-state US operations and in EU operations. The reconciliation architecture (mod-104) produces a *single* target control set that composes both.

## The reserved-matters overlay for the AI governance council

The AI governance council charter (mod-112 chapter 01) names a positive reserved-matters list. The carrier's charter overlays sector-specific reserved matters that route to the council rather than to line-of-business forums. Three are load-bearing:

- **State DOI market-conduct findings.** A DOI market-conduct examination that produces findings on AI system use — examination report, corrective-action request, market-conduct annual statement (MCAS) inconsistency, data-call response — is a council reserved matter. The state-DOI liaison prepares the escalation packet; the head of AI governance triages; the council decides the enterprise response including remediation commitments, voluntary programme changes, and defence resourcing.
- **Model-form and rating-plan filing changes with AI implications.** Where an AI system informs the rating plan or is disclosed in the policy form, the filing change is a council reserved matter — the filing commits the enterprise to a representation binding subsequent disclosures and market-conduct posture. The actuarial-model-governance lead prepares the packet; the council ratifies before submission.
- **Colorado Testing Methodology filing revisions.** A change to the filed methodology under Regulation 10-1-1 is a council reserved matter distinct from ordinary programme operation, because the DOI has ongoing authority to examine and comment, and a change is a commitment on which subsequent DOI interactions will turn.

These overlays extend the mod-112 chapter 01 list without displacing any of it. The charter amendment naming them is itself a reserved matter per the standard list.

## The two roles introduced

The blueprint introduces two sector-specific roles:

- **Actuarial-model-governance lead.** Typically an FSA (Fellow of the Society of Actuaries) or FCAS (Fellow of the Casualty Actuarial Society) credentialled actuary sitting inside the second-line risk-governance function or a validation-independent actuarial function. The lead owns the composition between the AI programme's testing regime and the actuarial-model-governance frame (Actuarial Standards of Practice, including ASOP 56 on modelling). The lead signs off on the Colorado Testing Methodology filing, on the mod-107 pre-deployment gate for underwriting and rating models, and on the mod-110 monitoring pack's actuarial substance. The role sits alongside the level-25 ai-risk-engineer in mod-106; disagreement escalates to the head of AI governance.
- **State-DOI liaison.** Inside legal, compliance, or regulatory-affairs, whose remit is the ongoing relationship with the DOIs the carrier operates under. The liaison owns the DOI-communication log, market-conduct-examination management, data-call coordination, and the escalation packet for any DOI-originated council item. The role composes with the GC seat on the council; the liaison is a *standing non-voting attendee* at council meetings where insurance items are on the agenda.

Convention: both roles sit second-line (independent of model development and of business-unit sales pressure) so they can challenge without a reporting-line conflict.

## The per-state activation framing

The NAIC AI Model Bulletin has legal force only in adopting states. A carrier that reads the bulletin as a single uniform obligation misunderstands the mechanism; a carrier that reads it as fifty-plus independent obligations incurs unnecessary complexity. The correct reading is that it is a *single reference obligation* with a *per-state activation vector* — the applicability filter names the adopting states, and the deprecation-path field carries the adopting-state timeline (a state that adopts in 2025 activates the obligation from its effective date; a state that later withdraws deactivates it from its withdrawal date).

Mod-104 supports this through the standard obligation-record shape with sector-specific extensions:

```yaml
obligation_record_naic_bulletin:
  id: OBL-INS-NAIC-BULLETIN-2023-12
  source: NAIC AI Model Bulletin on the Use of AI Systems by Insurers
  source_type: model_bulletin
  source_authority: NAIC (standard-setting body; no direct supervisory authority)
  legal_effect: activated per state on state adoption
  applicability_filter:
    addressee_scope:
      # populated from NAIC adoption tracker
      adopting_states:
        - { state: <state>, instrument: <state bulletin id>, adopted_effective: <date>,
            deviations_from_model: <summary>, current_status: active|withdrawn|superseded }
    lines_of_business: [ life, health, personal-lines, commercial-lines ]  # per state
    ai_use_contexts: [ underwriting, rating, claims-handling, marketing, fraud-detection ]
  deprecation_path:
    per_state_timeline:
      - { state: <state>, adopted_effective: <date>, superseded_by: <instrument>,
          withdrawn_effective: <date> }
    enterprise_response_window: <days between DOI adoption and enterprise activation>
  composes_with:
    - OBL-INS-CO-REG-10-1-1        # Colorado life-insurance-specific
    - OBL-EU-AI-ACT-ANNEX-III      # EU-operations subset
    - OBL-STATE-UNFAIR-TRADE-*     # underlying statutes (per state)
```

The `enterprise_response_window` lets the carrier author a single AI programme and roll it forward across states as they adopt. A state DOI that adopts with a 90-day effective date gives a defined window; enterprise activation for that state's operations happens within it; mod-108 holds the state-by-state activation timeline as an examinable artefact.

## The sector-adaptation record — insurance carrier

Filling in the schematic defined in `01-the-sector-adaptation-methodology.md`:

```yaml
sector_adaptation_record:
  id: SAR-INSURANCE-CARRIER-v1.0
  sector: insurance-carrier (life, health, P&C; multi-state US; EU exposure)
  enterprise_shape_assumed:
    - multi-state US carrier writing at least one Annex-III-relevant line
    - EU subsidiary or reinsurance branch with life/health cover
    - mixed in-house and third-party AI system estate
  anchor_obligations:
    - { id: OBL-INS-NAIC-BULLETIN-2023-12, activation: per-state,
        composes_with: [ OBL-INS-CO-REG-10-1-1, OBL-EU-AI-ACT-ANNEX-III ] }
    - { id: OBL-INS-CO-REG-10-1-1, activation: Colorado life-insurance ECDIS/algorithms,
        distinctive: filed Testing Methodology and GRMF reports }
    - { id: OBL-EU-AI-ACT-ANNEX-III, activation: EU-territorial-scope life/health }
    - { id: OBL-STATE-DOI-MARKET-CONDUCT, activation: continuous per state,
        composes_with: mod-107 assurance architecture }
  control_family_extensions_shape_A:
    - AIC-BIAS-CO-TEST-METHODOLOGY-01
    - AIC-BIAS-ENTERPRISE-TEST-METHODOLOGY-01
    - AIC-3P-INSURANCE-DECISION-VENDOR-01
    - AIC-MON-INSURANCE-ONGOING-BIAS-01
    - AIC-DOC-STATE-EXAM-READINESS-01
    - AIC-INV-INSURANCE-DECISION-SYSTEM-01
  control_family_extensions_shape_B: [ ]  # none required by the bulletin itself
  reserved_matters_overlay_council:
    - state-DOI market-conduct findings
    - model-form and rating-plan filing changes with AI implications
    - Colorado Testing Methodology filing revisions
    - EU AI Act Annex III applicability determinations
  role_additions:
    - { role: actuarial-model-governance-lead, credential: FSA or FCAS, line: second }
    - { role: state-doi-liaison, function: legal/compliance/regulatory-affairs,
        council_status: standing non-voting attendee }
  evidence_contract_overlays:
    - state-by-state activation timeline (mod-108 examinable)
    - filed Testing Methodology and DOI comment history (Colorado)
    - filed Governance-and-Risk-Management Framework report (Colorado)
    - vendor contract clauses for insurance-decision vendors (mod-109 sector tier)
    - market-conduct-examination correspondence log
  deprecation_path_notes:
    - NAIC-bulletin per-state adoption timeline (quarterly refresh)
    - Colorado 10-1-1 rulemaking extensions to non-life lines
    - EU AI Act delegated acts affecting Annex III insurance paragraph
```

## Failure modes

**Failure mode 1 — treating the NAIC bulletin as a single obligation and missing per-state variance.** The reconciliation architecture treats "NAIC AI Model Bulletin" as one row activated for all states of operation. A non-adopting DOI issues a data call framed against its underlying unfair-trade-practices statute; the carrier's response cites bulletin compliance, which is non-responsive; the DOI escalates. Or an adopting DOI with material amendments finds the carrier's evidence responds to the model text rather than the adopted text. The defence is the per-state activation vector on the obligation record, the adopting-state list on every downstream control's applicability filter, and a mod-108 evidence-rendering rule that produces state-specific packets rather than a single generic packet.

**Failure mode 2 — applying Colorado 10-1-1 nationally when its statutory basis is Colorado-specific.** The second-line governance function, reading Regulation 10-1-1 as a directional signal, applies its Testing Methodology and Governance-and-Risk-Management Framework filing shapes to all states — filing analogous documents with DOIs that have not enacted analogous rulemakings, or applying the Colorado protected-class taxonomy in jurisdictions with different definitions. The consequence is not a violation but a *commitment*: the non-Colorado DOI now holds an enterprise attestation binding subsequent examinations, and the enterprise cannot walk it back without an inconsistency finding. The defence is that the Colorado-specific control is scoped to Colorado on its applicability filter, and the shared bias-testing platform renders Colorado-shaped outputs *only* for Colorado. Voluntary industry-leadership statements are separate from filed regulatory attestations and are minuted at the council as such.

## Summary

The insurance carrier blueprint composes mod-102 through mod-112 for a multi-state US carrier with EU exposure. The NAIC AI Model Bulletin (December 2023) is the anchor obligation but has legal force only in adopting states; the reconciliation architecture carries the adopting-state list as the applicability filter's addressee scope and the state-by-state timeline as the deprecation path. Colorado Insurance Regulation 10-1-1 operationalises Colorado SB 21-169 with quantitative Testing Methodology and Governance-and-Risk-Management Framework filing requirements; the enterprise bias-testing regime is unified, with Colorado-scoped controls rendering filing-shape outputs from the shared platform. The bulletin's third-party section composes with mod-109 through a sector-specific vendor tier (INSURANCE-DECISION-VENDOR) whose contractual and operational overlays are non-negotiable for underwriting, rating, claims-handling, marketing, and fraud-detection vendors in adopting-state jurisdictions. EU AI Act Annex III's high-risk classification of life and health insurance risk-assessment and pricing systems activates for the EU-scope subset; mod-104 composes both frames on the same underlying AI systems. The council's reserved-matters overlay adds state DOI market-conduct findings, model-form and rating-plan filing changes, and Colorado Testing Methodology revisions. Two sector roles are introduced — the FSA/FCAS-credentialled actuarial-model-governance lead and the state-DOI liaison — both second-line by convention. The sector-adaptation record fills in the chapter-01 schematic. Get the per-state activation right and the enterprise carries a single AI programme that resolves correctly across fifty-plus DOI addressees; get it wrong and the carrier discovers on examination that its programme was written to a fiction that no state actually enacted.
