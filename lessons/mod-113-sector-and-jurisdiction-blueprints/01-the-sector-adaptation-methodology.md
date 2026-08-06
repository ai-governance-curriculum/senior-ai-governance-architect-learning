# The sector-adaptation methodology — instantiating the reference architecture without forking the library

## Why this chapter exists

The twelve modules that precede this one have handed the level-50 architect a complete enterprise reference architecture: a single control library (mod-102), a policy taxonomy (mod-103), a jurisdiction-reconciled obligation register (mod-104), an AIMS (mod-105), a risk taxonomy and appetite statement (mod-106), an assurance architecture (mod-107), an evidence architecture (mod-108), a third-party programme (mod-109), a post-market surveillance shape (mod-110), a GRC-for-AI reference architecture (mod-111), and an operating model with an AI governance council at its centre (mod-112). Each of those artefacts was authored to be *sector-agnostic* — the control library is not a banking library, the AIMS is not a healthcare AIMS, the appetite statement is not a defence appetite statement. Sector agnosticism is what lets the reference architecture compose; sectoral specificity is what makes it usable in production.

Mod-113 is where the reference architecture meets the sector. Every chapter that follows this one — financial services (chapter 02), healthcare and life sciences (chapter 03), public sector and defence (chapter 04), critical infrastructure and energy (chapter 05), employment and HR-tech (chapter 06), education (chapter 07), and consumer platforms (chapter 08) — takes the same reference architecture and instantiates it for its sector. The failure mode this chapter designs against is the one where each sector team, faced with the specificity of its own regulatory frame, decides that its instantiation is a *new* control library, a *new* AIMS, and a *new* risk taxonomy. That decision looks locally reasonable — the banking supervisor's expectations *are* different from the healthcare regulator's — but its second-order consequence is catastrophic. Within a year the enterprise has four control libraries whose entries have drifted, three AIMS with overlapping but incompatible scope statements, three risk taxonomies whose severity bands do not compare, and a third-party programme that cannot answer whether the same frontier-model provider has been approved for the banking business, denied for the healthcare business, and pending in the defence business.

The architectural answer is that **every sector below is a profile plus applicability-filter tuning plus sector-specific evidence-contract line items plus sector-specific policy-as-code guards** — not a new control library, not a new AIMS, not a new risk taxonomy. The reference architecture does not fork; it instantiates. This chapter states the invariants that make the instantiation defensible, defines the *sector adaptation record* that each of chapters 02-08 fills in for its sector, and names the two failure modes the methodology is designed against.

## The failure mode this chapter designs against — the fork temptation

The most common way sector programmes go wrong is not that the sector team ignores the reference architecture. It is that the sector team *reads* the reference architecture, concludes that its sector's specificity cannot be expressed within the reference shape, and forks. The fork is rarely announced as such. It appears as a "banking-specific control library" authored to speak SR 11-7 vocabulary natively, or a "healthcare AIMS" authored to speak Section 1557 and HIPAA natively, or a "defence risk taxonomy" authored to speak DoDI 5000.87 and the Responsible AI Toolkit natively. Each looks locally competent. Each is, at the enterprise level, a disaster.

Four second-order failures follow from the fork.

- **Drift between sector libraries.** The banking library patches its adverse-action-notice control on a Tuesday; the healthcare library patches its equivalent-in-spirit clinical-decision-support disclosure control on a Thursday. Six months later the two controls' evidence contracts have diverged in ways nobody catalogued. The enterprise now has two answers to *what does a defensible disclosure artefact look like?* — and no shared authority to reconcile them.
- **Evidence contracts diverge.** The mod-108 evidence architecture depends on a single evidence-schema registry keyed off a single control catalog. Two catalogs produce two schema registries; two schema registries produce two artefact renderings; two artefact renderings produce audit findings when the third-line samples across sectors and cannot compare like-for-like.
- **The third-party programme fragments.** The mod-109 third-party programme depends on a single provider register and a single set of provider-facing controls. When each sector maintains its own third-party programme (because each sector maintains its own control library and its own AIMS), a single frontier-model provider is onboarded three times, evaluated three times against three different rubrics, and carries three different residual-risk positions in three different registers. The provider's account team eventually notices; the enterprise's negotiating position collapses.
- **AIMS scope explodes into multiple management systems.** ISO/IEC 42001 permits an enterprise to operate multiple AIMS in principle (Clause 4.3 allows scope-tuned management systems), but the certification burden multiplies and the internal-audit programme (mod-107) must be sized for each. An enterprise that operates one AIMS across all sectors passes one Clause 9.3 management review per year; an enterprise that operates four operates four, with four scope statements, four Statements of Applicability, and four surveillance-audit cycles.

Each of the four failures is expensive to reverse. Reconvergence of drifted libraries takes twelve to eighteen months of controlled deprecation and re-adoption. Reconciling divergent evidence contracts requires a full re-issuance cycle of the artefacts under the reconciled contract. Consolidating the third-party programme requires re-negotiating provider contracts. Consolidating multiple AIMS into one requires a certification-cycle transition with the certification body. The methodology in this chapter is what prevents the fork before it happens.

## The six invariants of sector adaptation

The methodology rests on six invariants. Each is a stated architectural constraint that a sector adaptation record must satisfy; each references the reference-architecture module that owns the underlying design.

### I1 — the control library stays single-source-of-truth

The mod-102 control catalog is the enterprise's single control library. There is no *banking control library*, no *healthcare control library*, no *defence control library*. Sector-specific obligations that the sector team believes require new controls are handled in one of two ways: either the obligation is expressible as an evidence-contract extension on an existing control (the common case; see I3), or, where it is genuinely novel, the control is added *to the enterprise catalog* as a new entry with the applicability filter constraining it to the relevant sector (the rare case). In either case the addition traverses the mod-102 chapter 06 authoring lifecycle — proposal, architect review, adoption, versioning — and appears in the next catalog release. It does not appear in a sector-branded catalog nobody else can see.

The test for I1: sample any sector's control set six months after adaptation; every control it names dereferences into the enterprise catalog by identifier and version. There is no sector-catalog control that fails to dereference.

### I2 — the profile carries sector-specific applicability values

Mod-102 chapter 04 introduces the OSCAL *profile* as the mechanism that selects, tunes, and parameterises the catalog for a specific implementation target. The enterprise reference profile selects the enterprise-default control set with enterprise-default parameters; a sector profile *derives from* the reference profile and expresses only the *delta* — the additional controls it selects, the parameters it tunes, the applicability values it constrains. A sector profile is small (a few hundred lines of YAML/XML) because it inherits everything the reference profile already carries.

The applicability filter is where most sector specificity lives. The mod-102 filter dimensions — system tier, use-case classification, jurisdiction, risk-appetite band, addressee scope — are augmented in the sector profile with *sector-specific* filter values. Financial services adds *supervisory-authority scope* (Federal Reserve, OCC, FDIC, CFPB, PRA, FCA, MAS, HKMA, ...) as a filter dimension whose values the reference profile does not name. Healthcare adds *covered-entity classification* (HIPAA covered entity, HIPAA business associate, non-HIPAA-covered clinical decision support, FDA-regulated software-as-medical-device tier) as a filter dimension. Defence adds *classification level* and *ITAR/EAR export-control scope*. Each addition is a profile-level augmentation, not a catalog-level change.

The test for I2: sample any control's applicability filter in the sector profile; every filter value dereferences either into the reference profile's enumeration (unchanged) or into the sector profile's declared extension (documented in the profile). There are no undocumented values.

### I3 — the evidence contract extends per obligation, not per sector

Mod-108 hardens the evidence-contract-per-control field on every catalog entry. The contract states what evidence must exist, what schema it must conform to, what cadence it must be produced at, and what retention it must survive. Sector-specific obligations rarely require *new controls*; they overwhelmingly require *additional evidence-contract line items* on existing controls.

The mechanism is a per-obligation extension. When a banking-supervisor obligation (SR 11-7's model-development documentation expectations, say) requires additional documentation on the model-lifecycle-documentation control, the mod-108 evidence contract on that control acquires a new line item keyed by the obligation identifier — not by the sector name. The line item states: *when the obligation applies (applicability filter matches), the following additional artefact is produced under this additional schema at this cadence*. When the same obligation applies in another sector (SR 11-7 is a banking obligation, but its shape informs regulatory expectations elsewhere), the same line item applies through the applicability filter — the enterprise does not duplicate it.

The test for I3: sample any sector's evidence-contract extensions; every extension is keyed by an obligation identifier that dereferences into the mod-104 obligation register. There are no sector-keyed extensions that lack an obligation-register anchor.

### I4 — policy-as-code carries per-sector guards as parameterised policies

Mod-103 designs the policy taxonomy and the policy-as-code layer that enforces it at runtime. Sector-specific policy-as-code guards — the rule that a US federal customer's inference request never touches a non-FedRAMP-authorised inference boundary, the rule that a HIPAA-covered-entity workload never emits training data in cleartext outside a BAA-covered boundary, the rule that a defence-classified workload never traverses a commercial-cloud region — are policies *parameterised by the applicability filter's sector-specific values*. They are not sector-specific policy programmes.

The parameterisation shape is straightforward. A single policy template — *the workload's egress boundary must satisfy the addressee-scope constraint attached to the workload's classification* — is authored once. The addressee-scope constraint is a data table that the sector profile populates; when the workload is classified as a HIPAA-covered-entity workload, the constraint dereferences the HIPAA row; when classified as US-federal, it dereferences the FedRAMP row. The policy engine (mod-103 chapter 05) evaluates the same template; the sector-specific behaviour comes from the parameters, not from a sector-forked policy programme.

The test for I4: sample any sector's policy-as-code guards; every guard is either a parameterisation of a reference-architecture policy template or a documented extension registered in the mod-103 policy registry. There are no sector-specific policy programmes running outside the reference engine.

### I5 — the AIMS scope statement is sector-tunable without producing a second AIMS

ISO/IEC 42001 Clause 4.3 requires the enterprise to determine the AIMS's boundaries and applicability. The Clause allows scope decisions to reflect the enterprise's structure — including operating in multiple sectors — but it does not require a separate AIMS per sector. The mod-105 chapter 04 scope statement carries the sectoral shape as an *addendum* to the enterprise scope — a per-sector paragraph that names the sector-specific inclusions, exclusions, and boundary conditions and cross-references the sector profile (I2) that operationalises them.

The scope addendum is what makes the AIMS certifiable across sectors under a single certification. The certification body reads the enterprise scope statement plus the sector addenda together; the internal-audit programme (mod-107) samples across sectors on a risk-weighted plan; the Clause 9.3 management review considers the enterprise's AI risks across the full sectoral footprint. A second AIMS is not required and, per the fork-temptation failure mode, should be actively resisted.

The test for I5: sample the AIMS scope statement; each sector operating in the enterprise appears either in the enterprise scope (implicit inclusion) or as an explicit addendum (bounded inclusion). No sector operates outside the AIMS.

### I6 — the risk taxonomy is sector-augmented, not replaced

Mod-106 designs the enterprise risk taxonomy — the categories of AI-nexus risk (evaluation, safety, fairness, privacy, security, third-party, operational, reputational) that the risk register partitions across and the appetite statement bands. A sector's specificity typically adds *categories* the reference taxonomy does not name — clinical-harm risk for healthcare, financial-consumer-harm risk for financial services, national-security risk for defence — and *appetite triggers* that specific supervisors or missions demand. The augmentation is *additive*: it extends the taxonomy without replacing categories the reference taxonomy already carries.

The mechanism is a per-sector extension to the taxonomy registry. Each additional category is registered with an owner, a definition, a severity-scoring rubric that aligns to the enterprise's severity bands (so aggregation composes; mod-106 chapter 06), and a mapping to the reference categories where the sector-specific category is a specialisation (clinical-harm is a specialisation of the reference *safety* category, not a peer of it). Each additional appetite trigger is registered against the enterprise appetite statement with the sector-specific threshold and the seat authorised to declare a breach.

The test for I6: sample the sector's risk register entries; each entry's category dereferences either into the reference taxonomy or into a documented sector extension. Each sector-extension category maps to a reference category as either a specialisation or a documented peer. No sector operates a parallel taxonomy the enterprise cannot roll up.

## The sector adaptation record — the artefact this chapter produces

The methodology's deliverable is the *sector adaptation record*: a YAML schematic that each of chapters 02-08 fills in for its sector. The record is what makes the instantiation testable — a sector adaptation whose record does not compile against the reference architecture is not adopted. The schematic below is the shape every sector chapter follows.

```yaml
sector_adaptation_record:
  id: SAR-<sector-slug>-v<major.minor>
  ratified_by: ai-governance-council (mod-112 ch 01) on <ratification-date>
  reference_architecture_version:
    control_catalog: mod-102 catalog v<X.Y>
    reference_profile: enterprise-reference-profile v<X.Y>
    aims_scope_statement: mod-105 ch 04 statement v<X.Y>
    risk_taxonomy: mod-106 taxonomy v<X.Y>
    obligation_register: mod-104 register v<X.Y>

  sector:
    identifier: <e.g. financial-services / healthcare / public-sector-defence / ... >
    scope_narrative: |
      short paragraph naming the enterprise's activities in the sector,
      the operating entities affected, the geographical footprint, and
      the material AI systems whose classification falls into this sector.

  anchor_regulations:
    horizontal:
      - <e.g. EU AI Act — Article X applicability for this sector>
      - <e.g. NIST AI RMF — sector-profile publications where applicable>
    sector_specific:
      - id: <regulation identifier>
        title: <regulation full title>
        jurisdiction: <e.g. US-federal / EU / UK / SG / ...>
        supervisor: <e.g. Federal Reserve / OCC / EBA / MAS / ...>
        instrument_type: <statute | regulation | supervisory-letter | guidance>
        obligation_register_ids: [ OBL-... , OBL-... ]

  profile:
    id: PROFILE-<sector-slug>-v<major.minor>
    derives_from: enterprise-reference-profile v<X.Y>
    delta_summary: |
      short paragraph naming which reference-catalog controls are
      selected in addition, which are made mandatory that were optional,
      and which reference parameters are tuned.
    applicability_filter_overrides:
      - dimension: <e.g. supervisory-authority-scope>
        added_values: [ ... ]
        rationale: <why this dimension is required for this sector>
      - dimension: <e.g. covered-entity-classification>
        added_values: [ ... ]
        rationale: <...>

  evidence_contract_extensions:
    - control_id: <AIC-... reference-catalog control>
      obligation_id: <OBL-... from mod-104 register>
      added_line_items:
        - artefact: <name>
          schema: <evidence-schema registry id (mod-102 ch 07 / mod-108)>
          cadence: <one-off | per-release | quarterly | continuous | on-incident>
          owner: <role packet at level ... >
          retention: <years, or expression against obligation duration>

  policy_as_code_guards:
    - guard_id: <PAC-<sector-slug>-...>
      template: <mod-103 policy template referenced>
      parameterisation:
        addressee_scope: <sector-specific enumeration value>
        egress_boundary: <sector-specific enumeration value>
        <further parameters>
      enforcement_point: <mod-103 chapter 05 enforcement point identifier>

  aims_scope_addendum:
    inclusions: |
      sector-specific inclusions to the mod-105 ch 04 scope statement.
    exclusions: |
      sector-specific exclusions (with rationale — an exclusion the
      certification body would question requires a defence here).
    interfaces:
      to_enterprise_scope: <how the sector inclusions compose with the
        enterprise scope; typically an intersection or additive statement>

  risk_taxonomy_augmentation:
    added_categories:
      - id: RISK-<sector-slug>-...
        parent_reference_category: <safety | fairness | privacy | ... >
        definition: <one-paragraph definition>
        severity_rubric: <alignment to enterprise severity bands mod-106 ch 04>
        owner: <role packet>
    added_appetite_triggers:
      - trigger_id: APP-<sector-slug>-...
        threshold: <threshold expression>
        declaring_seat: <role packet with authority to declare breach>

  additional_roles:
    - role_identifier: <e.g. model-validation-lead — banking>
      level: <curriculum level>
      owner_packet_route: <path to the level-N owner packet or "N/A — role
        instantiated per business unit without curriculum-level packet">
      responsibilities_delta: |
        what this role does that no existing reference role covers.

  reserved_matters_additions:
    - matter: <sector-specific reserved matter at the ai-governance-council
        (mod-112 ch 01)>
      trigger: <what causes the matter to reach the council>
      pack_owner: <role packet that authors the pack for the council>

  deprecation_path_notes:
    - regulation_id: <regulation being deprecated or transitioned>
      superseded_by: <successor regulation>
      migration_deadline: <date>
      evidence_valid_until: <date under superseded regulation>
      handoff_notes: |
        the reconciliation-architecture (mod-104) handling of the transition
        for the sector; typically references the obligation-register entries
        that carry the transition edges.
```

The record is versioned; each chapter 02-08 produces its own v1.0 record; changes to the record follow the mod-102 chapter 06 lifecycle rules the catalog changes follow (because the record's controls, profile, and evidence contracts are catalog artefacts). The record is a *reserved matter* at the ai-governance-council (mod-112 chapter 01) at each version increment — the council ratifies the sector adaptation record for each sector the enterprise operates in.

## The two failure modes to design against

**Failure mode 1 — the fork temptation.** A sector team, faced with the density of its regulatory frame, concludes that the reference architecture cannot express its sector's specificity and drafts a sector-branded control library. The library is technically excellent; it maps SR 11-7 or Section 1557 or DoDI 5000.87 to a catalog whose entries speak the supervisor's vocabulary natively. It arrives at the ai-governance-council for adoption, and if adopted it produces the four second-order failures the opening of this chapter named.

The architectural defence is *the six-invariant test plus the sector-adaptation-record discipline*. Any sector proposal that fails I1 (single-source catalog), I3 (evidence extends per obligation), I5 (single AIMS), or I6 (taxonomy augmented not replaced) is refused at the council. The proposal is not sent away; it is returned with a route through the reference architecture — the sector-specific vocabulary is expressible as evidence-contract line items keyed by obligation, as applicability-filter values in a sector profile, as policy-as-code parameterisations, and (rarely) as new catalog entries added via the mod-102 chapter 06 authoring lifecycle. The council chair (typically the AI-accountable executive per mod-105 chapter 03) enforces the discipline; the head of AI governance (level 60) is expected to coach sector teams into the record before the proposal reaches the council.

**Failure mode 2 — applicability sprawl.** The sector team accepts the six invariants and expresses its sector-specific requirements as applicability-filter values (per I2) on a sector profile. The profile is small; the record compiles; the council ratifies. Six months later a second sector team does the same. A year later a fourth. Each sector's profile added five to eight applicability-filter values. The reference-profile applicability filter now carries seventy values across five sector-added dimensions. The filter is nominally the source of truth for *which controls apply to which workloads*; in practice it is unusable — the analyst who tries to determine the applicable control set for a novel workload cannot compile the filter in their head, and the automated compilation surfaces conflicting matches the sector profiles never reconciled with each other.

The architectural defence is *applicability-filter dimension budgeting plus a filter-parsimony review at each sector profile ratification*. The reference profile fixes a maximum number of applicability-filter dimensions the enterprise operates (typically five to seven — mod-102 chapter 04 fixes the exact number for the reference profile) and a maximum number of values per dimension per sector (typically no more than three or four new values). A sector proposal that exceeds the budget is required to fold its excess values into existing dimensions (a supervisor-scope value that is a specialisation of an addressee-scope value should be expressed as a compound of two existing dimensions, not as a new dimension) or to justify a permanent budget increase at the council. The head of AI governance owns the filter-parsimony review; the level-50 architect (this role) authors it for each sector-profile ratification and defends it at the council.

Neither failure is prevented by invariants alone. The fork is prevented by the coaching-and-refusal loop the head and the architect run before the council sees a proposal. The sprawl is prevented by the parsimony review the architect authors at ratification. Both defences are a discipline the reference architecture depends on the level-50 architect to carry.

## How chapters 02-08 use the methodology

Each of the seven sector chapters that follows this one uses the same structure:

1. Reads chapter 01 (this one) for the invariants and the record schematic.
2. Names the sector's anchor regulations — the horizontal frame (EU AI Act, US-federal per mod-104 chapter 04, UK per mod-104 chapter 06, and so on) *plus* the sector-specific frame (SR 11-7, Section 1557, DoDI 5000.87, and so on).
3. Fills in the sector adaptation record schematic — profile delta, applicability-filter overrides, evidence-contract extensions, policy-as-code guards, AIMS scope addendum, risk-taxonomy augmentation, additional roles, reserved-matters additions, deprecation-path notes.
4. Defends each choice against the six invariants — for every added catalog entry (rare), the chapter shows why the reference catalog could not carry the obligation as an evidence-contract extension; for every added applicability-filter dimension, the chapter shows why an existing dimension could not carry the value; for every added risk category, the chapter shows the specialisation-mapping to the reference taxonomy.
5. Names the two or three sector-specific failure modes the sector adaptation must design against — modes that are additional to the enterprise-level failure modes the reference architecture already handles.

The chapters do not repeat this chapter's methodology; they instantiate it. When a chapter's record shows a large delta to the reference profile, the level-50 architect reading the chapter asks *why is the delta this large?* and, if the answer is not defensible against the six invariants, escalates the record back through the head and the council. When a chapter's record shows a small delta, the architect asks *is this sector genuinely close to the reference or are we missing obligations?* and cross-checks against the sector's anchor regulations. Neither question is answered by the record alone; both are answered by the record combined with the mod-104 obligation register (which the record dereferences into) and the mod-102 catalog (which the record dereferences into).

## Coordination — the artefacts the record connects

The sector adaptation record is a joining artefact: it references and is referenced by several other reference-architecture artefacts. The interfaces are worth stating explicitly so that the sector chapters know what they compose against.

- **Mod-102 control catalog and profile registry.** The record's `profile` block *is* an OSCAL profile (mod-102 chapter 04) once serialised; the record is the human-readable authoring form. Additions to the catalog itself (the rare case) traverse the mod-102 chapter 06 authoring lifecycle.
- **Mod-103 policy taxonomy and policy-as-code registry.** The record's `policy_as_code_guards` block registers new parameterisations of existing templates in the mod-103 policy registry; new templates (rarer still) traverse the mod-103 chapter 06 authoring lifecycle.
- **Mod-104 obligation register.** The record's `anchor_regulations.sector_specific.obligation_register_ids` and `evidence_contract_extensions.obligation_id` fields dereference into the mod-104 register; every sector-specific obligation the record names *must* exist in the register with a resolved reconciliation edge.
- **Mod-105 AIMS scope statement.** The record's `aims_scope_addendum` block is authored as a scope-statement patch against the mod-105 chapter 04 statement; the ISMS-adjacent Clause 4.3 discipline the AIMS chapter fixes applies unchanged.
- **Mod-106 risk taxonomy and appetite statement.** The record's `risk_taxonomy_augmentation` block is registered against the mod-106 taxonomy registry; added appetite triggers are registered against the appetite statement with council ratification (mod-112 chapter 01).
- **Mod-107 assurance architecture.** The record does not modify the assurance architecture directly; the sector's controls, once in the catalog and profile, are sampled by the internal-audit programme under the enterprise assurance plan. The sector-specific sampling weight is a mod-107 chapter 03 concern, not a record field.
- **Mod-108 evidence architecture.** The record's `evidence_contract_extensions` block adds line items to the mod-108 evidence-schema registry; the schema registry validation the mod-108 chapter 04 fixes applies unchanged.
- **Mod-109 third-party programme.** Where a sector's provider expectations extend the mod-109 provider-facing controls, the extension appears as an evidence-contract line item on the relevant provider-facing control, not as a new provider register. The single provider register discipline is what the fork temptation would break; the record's I1 invariant preserves it.
- **Mod-110 post-market surveillance.** Where a sector's supervisor demands additional PMS reporting (a banking supervisor's model-performance report; a healthcare regulator's adverse-event report), the additional reporting is an evidence-contract line item on the PMS control family with the applicability filter constraining it to the sector.
- **Mod-111 GRC-for-AI platform.** The record is written to the GRC platform as a first-class artefact (per the mod-112 chapter 01 minute-keeping shape) and dereferenced from the AIMS documented information (mod-105 chapter 05). The platform surfaces the record's cross-references so that a query for *what applies to a HIPAA-covered-entity workload in the US-federal jurisdiction* returns the composed control set from the reference profile plus the healthcare sector profile plus the US-federal jurisdiction obligations without the analyst navigating three separate systems.
- **Mod-112 operating model.** The record is ratified at the ai-governance-council (mod-112 chapter 01) as a reserved matter at each version increment; the escalation ladder for a sector team's proposal traverses the head of AI governance's triage step (mod-112 chapter 01) before reaching the council pre-read.

The joining nature of the record is what makes the methodology work at the enterprise level. A sector adaptation authored in isolation from these interfaces will produce the fork-temptation failure regardless of how carefully it is written; a sector adaptation authored against these interfaces produces an instantiation the reference architecture composes.

## Summary

The sector-adaptation methodology instantiates the mod-101-through-mod-112 reference architecture for a specific sector without forking any reference-architecture artefact. Every sector below (chapters 02-08) is a *profile* (mod-102 chapter 04) plus *applicability-filter tuning* plus *sector-specific evidence-contract line items* (mod-108) plus *sector-specific policy-as-code guards* (mod-103) — not a new control library, not a new AIMS, not a new risk taxonomy. Six invariants discipline the instantiation: I1 single-source catalog, I2 sector applicability values on the profile, I3 evidence extends per obligation, I4 policy-as-code parameterisation, I5 single AIMS with scope addendum, I6 risk taxonomy augmented not replaced. The artefact each sector chapter produces is the *sector adaptation record* — a YAML schematic naming anchor regulations, profile delta, applicability-filter overrides, evidence-contract extensions, policy-as-code guards, AIMS scope addendum, risk-taxonomy augmentation, additional roles, reserved-matters additions at the ai-governance-council, and deprecation-path notes for regulation transitions. Two failure modes recur — the fork temptation (a sector team drafting its own control library) and applicability sprawl (the filter proliferating past usability) — and the defences are the six-invariant test enforced at the council plus the filter-parsimony review the level-50 architect authors at each sector-profile ratification. Chapters 02-08 read this chapter, fill in the record for their sector, defend each choice against the six invariants, and name the sector-specific failure modes their adaptation must handle in addition to the enterprise-level modes. Get the methodology right and the enterprise operates one control library, one AIMS, one risk taxonomy, one third-party programme, and one AI governance council across every sector it serves — sector-specific in behaviour, single-instance in architecture.
