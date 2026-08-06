# The pharmaceutical blueprint — GxP validation as the composition anchor

## Why this chapter exists

The pharmaceutical enterprise is the sector whose regulatory frame most resists direct application of the horizontal AI stack. A bank has SR 11-7 and a modern AI system reads as a model whose risk-management shape is broadly familiar. A hospital deploying software-as-a-medical-device (SaMD) has FDA CDRH clearance pathways whose predicate-device logic composes with mod-107's pre-deployment gate directly (chapter `03-us-health-system-blueprint.md` walks that composition). A pharmaceutical enterprise has neither. It has GxP — a family of quality regulations (GLP, GCP, GMP, GDP, GVP) whose shape predates the AI question by four decades and whose vocabulary (validation, qualification, predicate rule, CSV, CSA) does not map cleanly onto the mod-101 control library, the mod-105 AIMS, or the mod-107 assurance architecture without deliberate compositional work.

The failure mode this chapter designs against is the one where the architect walks in carrying the enterprise-generic AIMS and control library and attempts to layer them onto GxP. The result is a shadow programme — the AI governance council makes decisions the quality unit does not honour; the pre-deployment gate approves systems the GxP validation lead has not signed off; mod-108 emits artefacts that will not survive a Part 11 audit. The regulatory-submission workflow — the point at which the enterprise's AI use meets an FDA reviewer — exposes the incoherence.

The architectural answer is to treat *GxP validation as the composition anchor*. The AIMS composes onto the enterprise's pharmaceutical quality management system (PQS); the pre-deployment gate composes onto CSV/CSA; the evidence architecture composes onto the Part 11 audit-trail and the regulatory-submission dossier shape. Where the mod-107 gate would be the terminal assurance step in a generic enterprise, in a pharmaceutical enterprise the CSV/CSA validation exit is the terminal step, and the mod-107 gate is a *co-composed* step whose evidence flows into the validation dossier. This chapter walks that composition and then instantiates the sector-adaptation record schematic chapter `01-the-sector-adaptation-methodology.md` fixed.

## The obligation anchor set

The pharmaceutical enterprise's obligation register carries anchors at three layers — the GxP predicate rules, the electronic-records rule that sits on top of them, and the AI-specific guidances that are only now emerging. The register carries each with its scope and its predicate-rule perspective explicit.

### The GxP predicate rules

- **Good Laboratory Practice (GLP)** — 21 CFR Part 58 governs non-clinical laboratory studies that support applications for research or marketing permits for products regulated by the FDA. Scope is preclinical safety (toxicology, pharmacology, ADME) rather than efficacy. Where AI is used in a GLP study — a computational toxicology model, a machine-learning-driven image analysis of histopathology slides — the study director and the quality assurance unit remain accountable for the study integrity, and the AI system falls inside the study protocol and inside the QA audit.
- **Good Clinical Practice (GCP)** — 21 CFR Parts 50 (informed consent), 54 (financial disclosure), 56 (IRBs), 312 (IND), 812 (IDE), plus **ICH E6(R3)** as the international consolidated standard. <!-- needs-research: verify ICH E6(R3) adoption date and current transposition status in FDA guidance; the objective states R3 was adopted by the ICH Assembly in January 2025 and R2 remains in force as the reference version transitionally --> Scope is the design, conduct, monitoring, recording, analysis, and reporting of clinical trials. Where AI is used in a GCP-scope activity — a risk-based monitoring model, an eligibility-screening decision-support tool, a digital-endpoint measurement algorithm, a synthetic-control-arm generative model — the sponsor and the investigator remain accountable and the AI system is a *computerised system used in a clinical trial* under E6(R3) Section 4.
- **Good Manufacturing Practice (GMP)** — 21 CFR Parts 210 and 211 for finished pharmaceuticals; 21 CFR Part 600 (with 606, 610, 630, 640, 660, 680) for biological products; EU Annex 11 for computerised systems in the EU GMP inspection frame. <!-- needs-research: verify current EU GMP Annex 11 revision status; consultation on a revised Annex 11 was in progress in 2024–2025 --> Scope is drug substance and drug product manufacturing, packaging, labelling, and testing. Where AI is used in a GMP-scope activity — a predictive maintenance model on bioreactor equipment, a computer-vision inspection of finished-product visual defects, an ML-driven process-analytical-technology (PAT) model, a batch-release recommendation model — the manufacturing quality unit signs the batch, and the AI system falls inside the validated state.
- **Good Distribution Practice (GDP)** — governs the supply-chain integrity of finished product from release through delivery. Where AI is used in cold-chain monitoring, demand forecasting that materially shapes distribution, or serialisation-integrity analytics, GDP scope attaches.
- **Good Pharmacovigilance Practice (GVP)** — EU EMA GVP Modules I–XVI plus 21 CFR Part 314.80 (postmarketing adverse-event reporting for drugs) and 21 CFR Part 600.80 (biologics). Where AI is used in case-processing (auto-triage of adverse-event reports from spontaneous sources, signal detection across the safety database, literature screening), GVP scope attaches; the qualified person for pharmacovigilance (QPPV) in the EU and the safety officer in the US frame remain accountable.

The predicate-rule perspective the register carries as a first-class field is: *which underlying GxP rule governs the activity the AI system participates in?* An AI system supporting a GLP study inherits GLP; an AI system supporting a GCP trial inherits GCP; and so on. The AI system does not carry an independent obligation set — it inherits the predicate rule's obligations by virtue of its participation in the regulated activity.

### FDA 21 CFR Part 11 — the electronic-records-and-signatures rule

Part 11 governs electronic records and electronic signatures where the underlying predicate rule (GLP / GCP / GMP / GVP) requires records or signatures. Part 11 is *subordinate*: it does not impose a record-keeping requirement of its own; it specifies the *shape* an electronic record must take when a predicate rule requires that record. The Part 11 requirements the enterprise's evidence architecture is composed against are:

- **Audit trails** — secure, computer-generated, time-stamped audit trails to independently record the date and time of operator entries and actions that create, modify, or delete electronic records. §11.10(e).
- **Record integrity** — the ability to generate accurate and complete copies of records in both human-readable and electronic form suitable for inspection, review, and copying by the agency. §11.10(b).
- **Access controls** — limiting system access to authorised individuals; use of authority checks for system operations. §11.10(d) and (g).
- **Electronic signatures** — where signatures are executed electronically, they must be linked to their records, the signer identified, and the meaning of the signature (approval, review, responsibility) recorded. §11.50 and §11.70.
- **Validation** — validation of systems to ensure accuracy, reliability, consistent intended performance, and the ability to discern invalid or altered records. §11.10(a).

The last of those — validation under §11.10(a) — is the join point where Part 11 composes with CSV/CSA. The audit-trail requirements compose with mod-108 evidence retention as the sub-section below walks.

### FDA AI/ML guidance in the drug and biological product setting

- **CDER discussion paper — "Using Artificial Intelligence and Machine Learning in the Development of Drug and Biological Products"** — published by CDER in May 2023 as a discussion paper (not a guidance), soliciting feedback on the use of AI/ML across the drug development lifecycle (discovery, non-clinical, clinical, manufacturing, post-market). The paper is *reference*, not *regulation*, but it frames the vocabulary FDA reviewers use.
- **FDA draft guidance — "Considerations for the Use of Artificial Intelligence to Support Regulatory Decision-Making for Drug and Biological Products"** — issued as draft in January 2025 by CDER/CBER/OCP. Introduces a *risk-based credibility assessment framework* for AI models used to produce information that supports regulatory decisions. <!-- needs-research: verify whether this draft has moved to final and confirm exact issuance date and title; the objective indicates the draft was issued in early 2025 and final status was pending --> The framework is: (1) define the question of interest; (2) define the context of use; (3) assess the model risk from (a) model influence and (b) decision consequence; (4) plan credibility activities proportionate to risk; (5) execute and document.
- **FDA guidance on good machine learning practice (GMLP)** — the joint FDA/Health Canada/MHRA Good Machine Learning Practice for Medical Device Development: Guiding Principles (October 2021) applies primarily to CDRH SaMD territory but is referenced by CDER/CBER staff for AI in drug development that shares methodological ground.

### EMA and ICH anchors

- **EMA Reflection paper on the use of AI in the medicinal-product lifecycle** — final version issued by EMA in September 2024. <!-- needs-research: verify final publication date; the objective states September 2024 final following draft in mid-2023 --> The paper introduces a *risk-based categorisation* of AI use across the medicinal-product lifecycle — drug discovery, non-clinical development, clinical development, manufacturing, post-authorisation — with expectations proportionate to the influence the AI has on regulatory decisions and on patient outcomes. High-influence uses in clinical and manufacturing are expected to carry the full weight of GCP/GMP validation and, where the model directly informs the benefit-risk assessment, the reflection paper flags additional expectations on transparency and interpretability.
- **ICH E6(R3) — Good Clinical Practice** — adopted by the ICH Assembly in January 2025 and being transposed into regional regulation. <!-- needs-research: confirm ICH Assembly adoption date and current FDA/EMA transposition status --> E6(R3) is more principles-based than E6(R2), addresses computerised systems (Section 4) with a risk-based validation shape, and explicitly contemplates decentralised and AI-supported trial operations.
- **EU AI Act — applicability to pharmaceutical AI**. The Act's Annex III does *not* categorically capture pharmaceutical R&D. AI systems used in *internal research pipelines* (drug discovery, target validation, in-silico toxicology) generally fall outside Annex III's list of high-risk uses. AI systems that are *medical devices or components of medical devices* under Regulation (EU) 2017/745 (MDR) or Regulation (EU) 2017/746 (IVDR) fall inside Annex III point 5 and are high-risk regardless of Annex III's other categories. AI systems that are *commercial customer-facing generative AI* — a patient-facing symptom checker, a healthcare-professional-facing prescribing decision support — are not exempt from the Act; they attach through Annex III (medical device path) or through the GPAI rules (chapter `02-eu-ai-act-as-horizontal-frame.md` in mod-104). The obligation register carries an EU-AI-Act applicability field on every AI system record — *R&D-internal (out of scope)*, *medical-device path*, *GPAI-provider*, *commercial customer-facing (specify)* — so the composition is explicit.

## Composition point (a) — CSV/CSA and the mod-107 pre-deployment gate

The mod-107 pre-deployment gate grants launch authority for AI systems that clear the risk-tiered assurance path. In a pharmaceutical enterprise that authority is *not sufficient* on its own for a GxP-scoped system — the GxP validation exit is required in addition. The architectural question is how the two compose.

**Computer System Validation (CSV)** is the traditional GxP-industry practice for establishing documented evidence that a computerised system consistently produces a result meeting predetermined specifications. The reference methodology is GAMP 5 (ISPE Good Automated Manufacturing Practice, Second Edition, 2022), aligned with ICH Q9(R1) (Quality Risk Management, 2023). <!-- needs-research: confirm ICH Q9(R1) revision date --> GAMP 5 categorises software (Categories 1–5) and matches validation effort to category and risk. CSV documentation typically includes URS, FS, DS, IQ, OQ, PQ, and a validation summary report.

**Computer Software Assurance (CSA)** is the FDA's more recent framing, introduced in draft guidance titled *"Computer Software Assurance for Production and Quality System Software"* (FDA CDRH, September 2022 draft). <!-- needs-research: verify whether the CSA guidance has been finalised --> CSA reframes validation as a *risk-based assurance* activity where the effort is proportional to the risk the software presents to product quality or patient safety, and where unscripted testing, ad-hoc testing, and vendor-supplied evidence are acceptable methods for lower-risk software. CSA is not a replacement for CSV — it is a reframing of how CSV effort is allocated. The FDA CDRH draft applies to production and quality-system software; CDER/CBER staff have adopted CSA-thinking for drug-development software by analogy, though the formal guidance is CDRH-anchored.

### The composition — mod-107 gate co-composed with CSV/CSA — two patterns work, a third does not.

**Pattern 1 — CSV/CSA as evidence input to the gate.** The AI system's CSV/CSA package is one of the artefacts the mod-107 gate inspects. The gate rules on launch authority; the CSV/CSA rules on validated state. Both are required for a GxP-scoped system to enter production use. The gate's evidence contract is extended to require the CSV/CSA package for GxP-scoped systems, and the gate's decision does not substitute for validation exit.

**Pattern 2 — the gate as a co-composed forum inside the CSV/CSA lifecycle.** The GxP validation lead chairs a validation review that is *co-attended* by the mod-107 gate. The validation review takes CSV/CSA-shaped decisions; the mod-107 gate takes AI-specific decisions (evaluation coverage, generative-AI safety, foundation-model provenance). Both sign the same combined decision-of-record. This pattern works when the PQS carries a modernised validation-review shape and composes with the gate's evidence contract without duplication.

**Pattern 3 (anti-pattern) — the gate as the sole assurance forum.** The gate rules on launch authority and treats CSV/CSA as an internal detail. This does not compose. The quality unit does not honour the gate's decision; the batch record does not carry it as validation evidence; the FDA inspector does not accept the gate's minute as evidence of §11.10(a) validation. The enterprise ends up performing CSV/CSA anyway.

The choice between Pattern 1 and Pattern 2 is a function of PQS maturity. A large enterprise with a modernised, digitised PQS usually chooses Pattern 2; a smaller or newer enterprise usually chooses Pattern 1. The sector-adaptation record must state the choice.

### GAMP 5 category mapping onto the mod-101 control library

The mod-101 control library's risk-tiering shape (chapter `01-what-the-library-is-and-is-not.md` in mod-101) composes with GAMP 5's software categories as follows:

- **GAMP Category 3 (non-configured commercial off-the-shelf)** — vendor-supplied AI tools used as-is (e.g., an off-the-shelf electronic lab notebook with embedded AI features). Assurance leans CSA-light; the control library's foundational tier applies.
- **GAMP Category 4 (configured products)** — configured commercial platforms (e.g., a Veeva or Argus configuration that materially shapes the workflow). CSA-medium; the control library's mid-tier applies with extended configuration-management evidence.
- **GAMP Category 5 (bespoke / custom software)** — custom AI models built by the enterprise or a custom-development vendor. CSV-heavy; the control library's high-tier applies with the AI-specific extensions (evaluation, monitoring, provenance) layered on top of the GAMP 5 lifecycle.

The mapping the sector-adaptation record carries is not GAMP-category-to-control-tier one-to-one — it is *GAMP category and predicate-rule inherited risk together determine assurance depth*. A GAMP Category 3 tool used inside a GLP toxicology study still inherits GLP; the assurance depth is CSA-light on the tool but the study integrity depends on the tool being fit-for-purpose, and that determination is made by the study director inside the QA audit.

## Composition point (b) — regulatory-submission workflow

The pharmaceutical enterprise's AI use meets FDA (or EMA) reviewers at the *submission*. An IND (Investigational New Drug), NDA (New Drug Application), BLA (Biologics License Application), or MAA (Marketing Authorisation Application) is the artefact bundle the enterprise ships to the regulator. Where AI has been used in the work that produced the submission — a computational toxicology model in the non-clinical package, a machine-learning-derived clinical endpoint in the Phase 3 package, a PAT model in the CMC (chemistry, manufacturing, and controls) package — the submission carries the AI system's documentation as part of the submission.

The architectural answer is that the AI system's *submission-ready dossier* is a first-class mod-108 evidence artefact. The dossier composes:

- **The FDA credibility-assessment record** — the risk-based credibility assessment framework from the January 2025 draft guidance, with the question of interest, the context of use, the model influence and decision consequence assessment, the credibility activities plan, and the credibility-activities execution record. The dossier includes the credibility-assessment record as a self-contained sub-module.
- **The model documentation** — the model card, the training data provenance, the evaluation methodology and results, the intended-use statement, the out-of-scope-uses statement, the limitations statement. Mod-108's model documentation shape (chapter `03-model-documentation-and-technical-file.md` in mod-108) provides the underlying artefact; the pharmaceutical-submission shape adds the credibility-assessment framing.
- **The CSV/CSA record** — the validation evidence for the system that produced the submission-relevant output.
- **The Part 11 audit trail** — the record of who authored, reviewed, approved, and (where applicable) signed the outputs the submission incorporates.
- **The regulatory-submission format wrapper** — the eCTD (electronic Common Technical Document) module the submission-ready dossier is packaged into. The wrapper is *submission-format-specific*; the underlying artefacts are the same across FDA / EMA / PMDA / Health Canada, but the module numbers, the file naming, and the cross-reference conventions differ.

The evidence architecture composes the underlying artefacts once and re-renders them into the submission-format wrapper at submission time. The enterprise does not maintain N copies of the model documentation for N regulators; it maintains one authoritative model documentation artefact and a rendering layer that produces the eCTD module for FDA, the EMA-format module for EMA, and so on. The evidence-contract line item on the underlying model documentation control carries a *submission-format renderer* attribute.

## Composition point (c) — EMA reflection paper risk categorisation into mod-106

The EMA reflection paper's risk-based categorisation composes onto the mod-106 risk taxonomy as an *AI-influence-on-medicinal-product-decisions* dimension, retained alongside the taxonomy's existing dimensions (impact-on-people, impact-on-organisation, likelihood, control-effectiveness). The risk-engineer function (mod-106) evaluates the tier for every GxP-scoped AI system:

- **Tier P1 — indirect influence.** AI supports the work that produces regulatory-relevant outputs but is not the submission's basis (e.g., a literature-screening tool feeding a systematic review). Assurance is CSA-shape; documentation is intended-use-and-limitations plus vendor evidence where the AI is Category 3.
- **Tier P2 — supporting influence.** AI output contributes to the submission's analysis but is not the sole basis (e.g., a Bayesian meta-analysis with AI-informed priors; a QC image-analysis model as one of multiple release criteria). Assurance is CSV/CSA-mixed; documentation is model documentation plus credibility-assessment record.
- **Tier P3 — primary influence.** AI output is the direct basis for the regulatory-relevant conclusion (e.g., an AI-derived digital endpoint; a computer-vision batch-release model). Assurance is CSV-heavy with the full credibility-assessment framework, and the reflection paper's transparency and interpretability expectations apply.
- **Tier P4 — patient-facing decision.** AI output is the basis for a decision at point of patient care (e.g., an AI-informed dose recommendation through a labelled digital therapeutic). The system is in medical-device territory (Regulation (EU) 2017/745, FDA CDRH SaMD) and the composition hands over to chapter `03-us-health-system-blueprint.md`; the pharmaceutical enterprise remains the sponsor.

The tier is a first-class field on the mod-106 risk-register entry. A Tier P2 system that migrates into primary-endpoint use becomes Tier P3 and re-enters the CSV-heavy assurance path.

## Composition point (d) — Part 11 audit-trail composition with mod-108 evidence retention

Mod-108's evidence architecture is designed around durable, tamper-evident retention of the artefacts the gate ruled on. Part 11 §11.10(e) requires computer-generated, time-stamped audit trails on operator actions on electronic records that predicate rules require. The composition principles the sector-adaptation record fixes:

- **The predicate rule governs.** Part 11 does not create the record-keeping requirement; the predicate rule (GLP §58, GCP §312 / E6(R3), GMP §211, GVP) does. The audit trail attaches to the predicate-rule-required record. The mod-108 evidence-contract entry for a control producing a predicate-rule-required record carries a Part 11 audit-trail attribute; a control whose output is *not* predicate-rule-required does not require Part 11 treatment (though the enterprise may apply it anyway for internal traceability).
- **Audit-trail integrity is a storage-layer design attribute.** The mod-108 evidence store (chapter `05-evidence-storage-and-retention.md` in mod-108) is configured for Part 11-compliant audit trails on predicate-rule-captured record classes — WORM or equivalent immutability; time-stamped, secure, computer-generated entries; independent recording of create/modify/delete actions; access-control-logged review. AI-specific artefacts (model card, evaluation report, monitoring snapshots) are stored under the same regime when predicate-rule-relevant.
- **Retention periods are set by the predicate rule, not by Part 11.** GLP records under §58.195, GCP records under §312.62, GMP records under §211.180, and GVP records under EMA GVP Module I / §314.80 each carry their own retention-period rules. <!-- needs-research: reconcile precise retention periods against current regulatory text; the objective directional statement is correct but exact wording varies by record class --> The mod-108 retention policy encodes the predicate-rule period as the governing attribute; AI-artefact retention aligns to the predicate rule the artefact supports.
- **Copies must be generatable in human-readable form.** §11.10(b) requires the enterprise can produce copies on regulator request. Mod-108's export renderers must produce inspection-ready copies of AI artefacts — model card as PDF with signed provenance, evaluation report as PDF, monitoring dashboard as point-in-time snapshot — with the audit trail attached.

## Composition point (e) — sector-adaptation record example (YAML)

The sector-adaptation record schematic is fixed in chapter `01-the-sector-adaptation-methodology.md`. The pharmaceutical instantiation carries the fields the schematic specifies:

```yaml
sector_adaptation_record:
  id: SAR-PHARMA-v1.0
  sector: pharmaceutical
  scope:
    functions_in_scope:
      - research_and_early_development (drug discovery, target validation, in-silico toxicology)
      - non_clinical_development (GLP toxicology, ADME)
      - clinical_development (GCP Phase I–IV; ICH E6(R3))
      - chemistry_manufacturing_and_controls (CMC; GMP)
      - commercial_manufacturing (drug substance, drug product; biologics under 21 CFR 600)
      - distribution (GDP)
      - pharmacovigilance (GVP; §314.80; §600.80)
      - medical_affairs_and_commercial (customer-facing content, HCP interactions)
    functions_out_of_scope:
      - medical_device_products (handed over to SAR-HEALTH; ch 03 blueprint)

  predicate_rule_map:
    GLP: 21 CFR Part 58
    GCP:
      us: 21 CFR Parts 50, 54, 56, 312, 812
      international: ICH E6(R3) [adoption Jan 2025; transposition ongoing]
    GMP:
      drugs: 21 CFR Parts 210, 211
      biologics: 21 CFR Part 600 (with 606, 610, 630, 640, 660, 680)
      eu: EU GMP + Annex 11 for computerised systems
    GDP: EMA GDP guidelines 2013/C 343/01 [and successors]
    GVP:
      eu: EMA GVP Modules I–XVI
      us_drugs: 21 CFR 314.80
      us_biologics: 21 CFR 600.80

  electronic_records_rule:
    us: 21 CFR Part 11 (subordinate to predicate rules)
    eu: Annex 11 (EU GMP)
    perspective: predicate-rule-governs; Part 11 shapes the record

  ai_specific_guidance:
    us:
      - FDA_CDER_discussion_paper_may_2023 (reference-not-regulation)
      - FDA_credibility_assessment_draft_guidance_jan_2025 [needs-research: final status]
      - FDA_GMLP_guiding_principles_oct_2021 (CDRH-anchored; CDER by-analogy)
    eu:
      - EMA_reflection_paper_ai_in_medicinal_product_lifecycle_sep_2024 [needs-research: final date]
    international:
      - ICH_E6(R3) (adopted by ICH Assembly Jan 2025) [needs-research: date confirmation]

  eu_ai_act_applicability:
    rd_internal_pipelines: out-of-scope (Annex III does not capture)
    medical_device_components: Annex III point 5 (high-risk); hand off to device path
    commercial_customer_facing_gen_ai: attaches; assess Annex III + GPAI rules
    gpai_provider_role: assess if enterprise trains or fine-tunes foundation models above thresholds

  aims_composition:
    baseline: mod-105 AIMS ISO/IEC 42001-aligned
    composed_onto: enterprise pharmaceutical quality management system (PQS) per ICH Q10
    integration_point: AIMS documented information (Clause 7.5) references PQS documented information; no duplication
    management_review: Clause 9.3 output ratified at AI governance council; PQS management review is a separate cadence
    quality_unit_authority: retained; not superseded by AI governance council

  assurance_composition:
    baseline: mod-107 pre-deployment gate
    csv_csa_pattern: Pattern-2 (co-composed validation review) for GAMP Cat 5 GxP-scoped systems
    csv_csa_pattern_fallback: Pattern-1 (CSV/CSA as gate evidence input) for GAMP Cat 3/4 systems
    validation_reference: GAMP 5 (2022) + ICH Q9(R1) + FDA CSA draft (Sep 2022) [needs-research]
    submission_readiness: dossier is first-class mod-108 evidence artefact; eCTD renderer specified

  risk_taxonomy_extension:
    mod-106 baseline dimensions: retained
    pharmaceutical extension:
      dimension: ai_influence_on_medicinal_product_decision
      tiers:
        - P1_indirect_influence
        - P2_supporting_influence
        - P3_primary_influence
        - P4_patient_facing_decision (hand off to device path)

  evidence_architecture_extension:
    baseline: mod-108
    part_11_audit_trail:
      applies_to: controls producing predicate-rule-required electronic records
      integrity: WORM or equivalent; time-stamped; computer-generated
      access_control_logged: true
    retention_period_source: predicate rule (GLP/GCP/GMP/GVP); Part 11 does not set retention
    human_readable_export: mandatory for all predicate-rule-relevant AI artefacts

  role_additions:
    gxp_validation_lead:
      accountability: CSV/CSA composition for GxP-scoped AI systems
      reports_to: head of quality (PQS accountable executive)
      interfaces: mod-107 gate; mod-108 evidence architecture; mod-101 control library
    qualified_person_equivalent:
      eu: Qualified Person (QP) under Directive 2001/83/EC Article 48 [batch release]
      us_analogue: quality unit signatory under 21 CFR 211.22
      pharmacovigilance: QPPV per EMA GVP Module I
      ai_specific_extension: signs releases where AI materially informed the release decision

  combination_product_boundary:
    definition: 21 CFR Part 3.2(e) — products combining drug/biologic and device components
    lead_centre_determination: primary mode of action; hand-off to SAR-HEALTH once determined
    joint_governance: sar-pharma and sar-health co-govern until lead-centre determination lands

  council_interfaces:
    ai_governance_council (mod-112): retained
    quality_council (PQS): retained; parallel forum with its own authority
    interface_forum: joint review for GxP-scoped AI reserved matters; charter names both forums

  invariants:
    - id: PHARMA-I1
      description: no GxP-scoped AI system enters production without CSV/CSA validation exit
      test: sample any production-live GxP-scoped AI system; find CSV/CSA validation summary report
    - id: PHARMA-I2
      description: predicate-rule perspective is explicit on every AI system record
      test: every AI-system record carries a predicate_rule field
    - id: PHARMA-I3
      description: submission-relevant AI artefacts are eCTD-renderable
      test: sample any AI system referenced in an active IND/NDA/BLA/MAA; produce the eCTD module on demand
    - id: PHARMA-I4
      description: Part 11 audit trail attaches to predicate-rule-required records only
      test: no Part 11 audit-trail overhead on records not required by any predicate rule
    - id: PHARMA-I5
      description: combination-product line item resolves to lead-centre determination
      test: any combination-product AI system has a documented lead-centre determination
```

## Composition point (f) — the CDRH / CDER-CBER boundary and combination products

The boundary between AI-in-medical-devices (FDA CDRH SaMD) and AI-in-drug-development (FDA CDER for drugs; CBER for biologics) hands off to chapter `03-us-health-system-blueprint.md`. Determination is made by *primary mode of action* and by regulatory-centre assignment:

- **AI as a component of a medical device** — hand off to CDRH SaMD path (chapter 03).
- **AI used in drug or biological product development, manufacturing, or post-market** — this chapter applies (CDER for drugs; CBER for biologics).
- **AI in combination products** — 21 CFR Part 3.2(e) defines a combination product as one combining drug/biologic and device components. Under 21 CFR Parts 3 and 4 the FDA determines the lead centre by primary mode of action; the non-lead centre is consulted. The AI system's regulatory frame follows the lead-centre determination. Until determination lands (typically at pre-IND / pre-submission meeting), SAR-PHARMA and SAR-HEALTH co-govern; the sector-adaptation record carries a *combination-product interim disposition* attribute.

Example: a drug-eluting stent with an AI-driven dosing controller. Primary mode of action is drug elution; CDER may be lead centre with CDRH consulted. The AI dosing controller's regulatory frame is CDRH-influenced but its parent product's lead centre is CDER. The evidence architecture produces artefacts satisfying both centres; the combination-product line item resolves ownership.

## Composition point (g) — the two role additions

The pharmaceutical enterprise's role packet — additional to mod-112's enterprise-generic roles — includes two roles the sector-adaptation record names.

### The GxP validation lead

Accountability: owns the CSV/CSA composition for GxP-scoped AI systems. Where the enterprise chose Pattern 2 (co-composed validation review) as the composition pattern, the GxP validation lead chairs the joint review. Where the enterprise chose Pattern 1 (CSV/CSA as evidence input to the mod-107 gate), the GxP validation lead is a standing attendee at the gate for GxP-scoped systems and produces the CSV/CSA package the gate inspects.

The lead reports to the head of quality — the PQS-accountable executive — rather than to the head of AI governance. The reporting line is deliberate: CSV/CSA validation is a PQS function, and the lead's authority derives from the PQS. The lead's *interface* to AI governance is through the mod-107 gate and through the AI governance council's quality-adjacent reserved matters. Evidence-contract line items include the CSV/CSA validation plan, the validation summary report as the terminal artefact, the periodic revalidation record, and the deviation and change-control records §11.10(k) implies. The lead's role packet names the interfaces to mod-107, mod-108, the mod-101 extensions, and the mod-110 monitoring outputs that feed revalidation triggers.

### The Qualified Person (QP) equivalent for EU releases

Under Directive 2001/83/EC Article 48 and the EU GMP framework, the Qualified Person (QP) signs the batch release for medicinal products manufactured or imported into the EU. In the US, the analogous authority is the quality unit signatory under 21 CFR 211.22 whose signature releases the batch. For pharmacovigilance, the EU Qualified Person for Pharmacovigilance (QPPV) per EMA GVP Module I signs the safety-system output.

Where AI materially informs a release decision — a computer-vision inspection whose output is one of the release criteria; a PAT model whose output substitutes for a released-specification test; a case-triage AI whose output shapes the ICSR submitted to authorities — the QP (or US signatory, or QPPV) is signing on the AI system's output. The QP is trained on the AI system's intended use and limitations; is authorised through the QP's competent-authority framework to sign on AI-informed decisions; the QP's electronic signature under §11.50 is linked to the AI system's version and the specific inference signed on.

The QP is not a *voting member* of the AI governance council — the QP's authority is granted by competent authority, not by the enterprise. The QP is an *interface role* whose signature is on the batch record downstream of the AI system's operation, and whose training depends on the AI-system documentation being complete and current. The sector-adaptation record names the QP-interface as first-class and the QP's documentation dependencies as first-class evidence-contract line items.

## Failure modes

**Failure mode 1 — importing SaMD-shaped controls onto drug-development AI and failing on regulatory submission format.** The enterprise adopts the mod-101 control library extensions the SaMD blueprint (chapter 03) fixed and applies them to AI systems used in drug-development work. The control names and evidence contracts are SaMD-shaped — predicate-device analysis, 510(k) equivalence, MDR clinical evaluation. The submission is an NDA whose eCTD Module 3 (Quality) and Module 4 (Non-clinical) do not accept SaMD-shaped artefacts. The CDER reviewer requests the CSV/CSA package the enterprise did not produce, the credibility-assessment framework the enterprise did not apply, and the model documentation shape CDER expects. Information-request cycles add months, and the enterprise retrofits CSV/CSA against a system validated to a device shape that does not compose. The defence is the sector-adaptation record's *predicate rule perspective* on every AI-system record — an AI system whose predicate rule is GLP/GCP/GMP/GVP inherits the CDER/CBER submission format from the outset, and mod-108 emits the eCTD module rendering rather than a device-file rendering. The CDRH-SaMD path is used for AI systems that are themselves medical devices — nothing else.

**Failure mode 2 — applying CSV effort-heavy across the entire estate when CSA-shape assurance is appropriate for lower-risk categories.** The enterprise applies GAMP 5 Category 5 CSV lifecycle to every AI system regardless of risk tier — including the AI-assisted literature screening tool and the off-the-shelf electronic lab notebook whose vendor-supplied evidence should suffice. Thousands of pages of URS/FS/DS/IQ/OQ/PQ documentation are generated for systems whose risk to product quality is negligible. Validation stretches to twelve months for systems that should ship in weeks; adoption velocity collapses; the governance programme becomes indistinguishable from a validation programme; the business unit routes AI experimentation outside the governance perimeter. The defence is the *risk-tiered assurance path* the sector-adaptation record fixes — the FDA CSA draft's whole purpose is to reframe validation as a risk-based activity. The GxP validation lead differentiates CSA-light, CSA-medium, and CSV-heavy paths; the gate's evidence contract for GxP-scoped systems specifies depth by GAMP category + risk tier + predicate-rule inheritance jointly. Tier P1 ships CSA-light; Tier P3 receives CSV-heavy treatment; adoption velocity is preserved and validation effort tracks patient risk.

## Summary

The pharmaceutical enterprise's regulatory frame — GxP predicate rules (GLP, GCP, GMP, GDP, GVP), Part 11 as the subordinate electronic-records shape, the FDA CDER discussion paper and the January 2025 credibility-assessment draft guidance, the EMA September 2024 reflection paper, ICH E6(R3) — resists direct application of the mod-101 through mod-112 stack. GxP validation (CSV per GAMP 5 / ICH Q9; CSA per the FDA September 2022 draft) becomes the composition anchor: the mod-107 pre-deployment gate co-composes with CSV/CSA rather than replacing it, and mod-108 produces submission-ready dossiers whose eCTD renderings are first-class artefacts. The EMA reflection paper's risk-based categorisation extends the mod-106 risk taxonomy with an AI-influence dimension whose four tiers (P1 indirect, P2 supporting, P3 primary, P4 patient-facing) determine assurance depth. Part 11's audit-trail requirement composes with mod-108 retention through predicate-rule perspective — the predicate rule governs; Part 11 shapes the record; retention periods derive from the predicate rule. The sector-adaptation record instantiates the chapter `01-the-sector-adaptation-methodology.md` schematic with pharmaceutical fields — scope, predicate-rule map, AI-guidance anchors, EU-AI-Act applicability, AIMS and assurance composition, risk-taxonomy extension, evidence-architecture extension, role additions, combination-product boundary, and council interfaces. Two roles enter the packet: the GxP validation lead (owning the CSV/CSA composition, reporting to the head of quality) and the Qualified Person equivalent (signing batch release, ICSR, or other predicate-rule-authoritative outputs where AI materially informed the decision). The boundary with the SaMD blueprint (chapter 03) is walked through the combination-product line item with 21 CFR Part 3 lead-centre determination governing hand-off. Two failure modes recur — importing SaMD-shaped controls onto drug-development AI, and applying CSV-heavy effort across the entire estate — and the defences are the predicate-rule perspective on every record and the risk-tiered assurance path. Get the composition anchor right and the enterprise's AI governance survives the submission review, the GMP inspection, and the pharmacovigilance audit without a shadow programme.
