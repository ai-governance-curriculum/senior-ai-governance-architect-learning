# The coordination contracts — with the analyst (level 15), the risk engineer (level 25), and the evaluation engineer (level 35)

## Why this chapter exists

The evidence architecture the previous six chapters designed does not populate itself. The substrate (chapter 02) accumulates events only because first-line and second-line seats emit them under contracts that name the events. The card family (chapter 03) exists only because named authors write drafts against named schemas and drive them to publication. The AI supply-chain slice (chapter 04) is enforceable only because the model-registry ingestion policy the architect wrote is executed against attestations first-line seats produce. The regulator-facing packagings (chapter 05) render correctly only because the substrate underneath them is complete. The OSCAL catalog (chapter 06) is queryable only because the assessment-results and system-security-plan-equivalent content is dropped into it as the operating rhythm turns over. Every one of those preconditions is a coordination contract the level-50 architect writes and maintains against three specific roles: the level-15 `ai-governance-analyst`, the level-25 `ai-risk-engineer`, and the peer level-35 `ai-evaluation-engineer`.

The architect authors the shape; those three roles populate it. Without coordination contracts pinned, the boundary between "architect authors the schema" and "role populates the schema" degrades in two directions. In one direction the analyst (or the risk engineer, or the evaluation engineer) starts filling schema-shaped gaps in-line — the substrate ends up carrying content the schema does not describe, and the architecture silently forks. In the other direction the architect drifts into direct tasking — chasing an analyst for a specific card, negotiating an evaluation methodology, hand-writing a risk-scoring rationale on a system-under-gate — and becomes the coordinator-of-last-resort the architecture was designed to obviate. Chapter 07 pins the contracts that prevent both drifts.

The chapter is deliberately parallel in shape to mod-107 chapter 06, which pinned the coordination contracts for the *assurance* side (pre-deployment gate, ongoing assurance, internal audit). This chapter pins the coordination contracts for the *evidence* side (substrate, cards, supply-chain evidence, regulator packagings, OSCAL catalog). The two composed pin the peer / role landscape the architect operates against across both mod-107 and mod-108. Composition is walked in the "Composition with mod-107 assurance architecture" section below.

## The three peer / downstream roles and what each contributes

The three roles carry different competences, sit at different levels, and produce different slices of the evidence architecture. What follows names each role's primary outputs against the substrate and the not-in-scope boundaries the architect defends.

### `ai-governance-analyst` (level 15) — evidence records author

The analyst is the *doing* seat inside the evidence architecture — the role whose day-to-day is authoring, filing, indexing, and updating evidence records against the schemas the architect authored. The role reports upward through the head of AI governance, not through the architect; the architect writes the role-scope contract the head of AI governance ratifies. <!-- needs-research: confirm AICG role-scope document for `ai-governance-analyst` at level 15 covers the outputs enumerated below -->

Primary outputs the evidence architecture depends on:

- **Card drafts driven to publication.** The analyst is the author of record for model cards, system cards, and dataset cards. First-line model owners contribute source material (training compute, artefact hashes, intended-use language); the analyst is the seat that assembles the draft against the chapter-03 schema, drives it through second-line review, presents it to the publication gate, and files the signed version to the card registry. Cards without a named analyst-author are cards the architecture does not carry.
- **AI Impact Assessment records.** The analyst authors AIA records against the mod-105 chapter 04 shape and files them to the AIMS documented-information register (mod-105 chapter 07). The AIA is a decision artefact (mod-105) but its record is an evidence artefact (mod-108); the analyst maintains the record instantiation, freshness, and cross-reference to the card family.
- **Substrate audit-log ingestion sanity checks.** The analyst does not author the audit-log schema (that is the architect, chapter 02) and does not emit events (that is first-line MLE / MLOps / data engineering). The analyst does run the sanity queries the architect specifies against the substrate — event-count-per-family per day, monotonic hash-chain, retention-policy adherence, dangling-reference detection — and files the ingestion sanity report to the governance-workflow log. Ingestion sanity is analyst-tier; ingestion methodology is architect-tier.
- **Regulator-packet-content assembly.** The analyst runs the assembly pipeline chapter 05 designs — a scoped invocation against the substrate at a specific point in time — and files the assembled packet for signer review. The analyst does *not* author the section content the substrate provides; the assembly pipeline projects substrate content into template sections. The analyst does surface template-content-versus-substrate-content divergence when the pipeline flags it.
- **OSCAL system-governance-plan authoring per system.** The analyst authors the per-system OSCAL system-governance-plan-equivalent (chapter 06) — the system's applicable-controls declaration, its risk-tier classification, its evidence-artefact cross-references — and files the plan to the OSCAL catalog. The architect authors the OSCAL profile; the analyst authors the per-system plan instance against the profile.
- **Evidence gap register maintenance.** The analyst maintains a running register of substrate gaps — schema fields the analyst attempted to populate for a specific system where no substrate content exists to populate them. The register is the architect's early-warning surface: gaps recurring across systems indicate a schema-versus-reality mismatch the architect must reconcile.

Not-in-scope for the analyst:

- **Authoring the schemas themselves.** Model-card schema, dataset-card schema, risk-card schema, audit-log event schema, packet template shape, OSCAL profile — architect only. Analyst authoring schema is out-of-competence and produces silent forks.
- **Authoring risk-card content.** Risk-card content (risks named, scoring rationale, treatment plan, residual-versus-appetite reconciliation) is the risk engineer's, below. The analyst may file the risk card the risk engineer authored, but does not author the substance.
- **Authoring evaluation-run content.** Evaluation-run records, evaluation reports, red-team engagement records are the evaluation engineer's, below. The analyst may file the record the evaluation engineer authored, but does not author the substance.
- **Signing regulator packets.** Packet signatures come from the seats each template names (head of AI governance, general counsel, AI-accountable executive, MRM head, regulatory affairs head; see chapter 05). The analyst assembles and files; the analyst does not sign.

### `ai-risk-engineer` (level 25) — risk-engineering evidence slice producer

The risk engineer is the *methodology owner* for the risk-engineering evidence slice — the seat that produces the risk-side content the substrate carries and that the card family, the regulator packagings, and the OSCAL catalog each project. The role reports (as with the analyst) through the head of AI governance; the architect writes the role-scope contract the head of AI governance ratifies. <!-- needs-research: confirm AICG role-scope document for `ai-risk-engineer` at level 25 covers the outputs enumerated below -->

Primary outputs the evidence architecture depends on:

- **Risk-card content per system.** The risk engineer authors the substance of every risk card the enterprise publishes — the risks named against the mod-106 taxonomy, the scoring rationale, the treatment plan, the residual reconciliation against appetite, the drift-trigger declarations tied to the specific system. The analyst may drive the resulting card through the publication gate and file it; the substance is the risk engineer's.
- **Risk-register entries against the mod-106 taxonomy.** The risk register that mod-106 chapter 07 designs is populated by the risk engineer. Entries land under the taxonomy the architect ratified, with scoring produced through the methodology the risk engineer owns, with treatment-plan cross-reference to the RTP artefact.
- **Risk-treatment-plan artefacts for the substrate.** The RTP artefact class chapter 05 references (an Annex IV input, an SR 11-7 input, an ISO/IEC 42001 documented-information register entry under mod-105 chapter 07) is authored by the risk engineer. The architect authored the artefact-class schema; the risk engineer authors the per-system instance.
- **Quantitative risk scoring output that feeds cards and regulator packets.** Where the enterprise's risk-scoring practice produces quantitative outputs (loss distributions, calibrated probability estimates, expected-loss aggregations per mod-106), those outputs are the risk engineer's. The card's "quantitative risk analysis" section and the packet's residual-risk narrative project the risk engineer's outputs.
- **Drift-driven re-assessment records that populate ongoing-assurance evidence.** When a drift trigger fires (mod-107 chapter 03), the risk engineer produces the re-assessment record — the analysis of what changed, what the revised scoring is, what the revised treatment is. The record populates the ongoing-assurance evidence slice the substrate holds.

Not-in-scope for the risk engineer:

- **Authoring card templates.** Model-card schema, system-card schema, dataset-card schema, risk-card schema — architect only. The risk engineer writes risk-card content against the risk-card schema the architect authored.
- **Authoring regulator packet templates.** Article 11 template, Article 72 template, Article 73 template, SR 11-7 template, FDA PCCP template — architect only. The risk engineer writes the substrate content the packet templates project.
- **Signing substrate immutability posture.** WORM enforcement, hash-chain replay attestation, break-glass audit-trail sign-off (chapter 02) — architect and the immutability sub-plan owner, not the risk engineer.
- **Owning the catalog.** The OSCAL catalog (chapter 06) is the architect's. The risk engineer's outputs land in it (via the analyst-authored per-system plans and via the risk-register cross-references); the catalog's shape and integrity are the architect's.

### `ai-evaluation-engineer` (level 35) — assurance evidence slice packager, peer

The evaluation engineer is the *peer* seat in this coordination — one level below the architect, coordinated horizontally rather than vertically, running its own methodology domain against a shared boundary. The architect and the evaluation engineer are joint stewards of a specific set of boundary artefacts named below. Contracts across peer boundaries are structurally distinct from role-scope contracts with downstream roles; the "peer-to-peer coordination contract" section below designs it. <!-- needs-research: confirm AICG role-scope document for `ai-evaluation-engineer` at level 35 covers the outputs enumerated below -->

Primary outputs the evidence architecture depends on:

- **Evaluation-run records.** Every evaluation the evaluation engineer or a first-line MLE runs against a system produces a family-1 audit-log event (chapter 02) with a `pipeline_run_id` that is referenceable from downstream artefacts — the card's "evaluation" section, the packet's evaluation-summary section, the OSCAL assessment-results content. The record shape is architect-authored (event schema in family 1); the record content is the evaluation engineer's.
- **Evaluation reports that populate "evaluations" section of every card and packet.** The evaluation engineer authors the substantive evaluation report — metrics chosen, methodology used, coverage across factors, uncertainty representation, deviations from prior evaluation — that the card family's "evaluation" section projects and the packet templates' evaluation-summary sections project. The analyst may transclude; the evaluation engineer authors.
- **Red-team / adversarial-eval records.** Red-team engagement records, adversarial-evaluation records, and their finding-severity taxonomy are the evaluation engineer's. Where tier-3 and tier-4 systems require red-team evidence (chapter 03), the evaluation engineer produces it.
- **Pre-deployment gate assurance evidence.** The specific evaluation runs, reproduction records, calibration outputs, and methodology attestations the mod-107 chapter 02 pre-deployment gate consumes are the evaluation engineer's. These records land in the substrate for the assurance side to consume and in the evidence side's catalog for downstream regulator-packet assembly.
- **Reproducibility packages the substrate stores.** For every material evaluation, the reproducibility package (eval-set version, seed, model version, code version, environment manifest, artefact hash) lands in the substrate. The evaluation engineer produces the package; the architect authored the substrate slot it lands in.

Not-in-scope for the evaluation engineer:

- **Authoring schemas.** Model-card schema, evaluation-run event schema, packet templates, OSCAL profile — architect. The evaluation engineer marks the schemas as executable-from-the-evaluation-chair or disputes them where they are not; the architect authors and ratifies.
- **Authoring regulator packet templates.** Same as above — architect only.
- **Owning the substrate.** The audit-log substrate, the artefact registries, the immutability posture, the retention policy — architect and the immutability sub-plan owner.

## The peer-to-peer coordination contract with the evaluation engineer

The evaluation engineer at level 35 is not downstream of the architect. The coordination is peer-to-peer, expressed in a shared contract both roles sign, with reconciliation escalation to the head of AI governance where the two cannot align. The contract borrows the three-round pattern the assurance architecture uses (mod-107 chapter 06) and tunes it for the evidence architecture's boundary artefacts.

Reciprocal boundaries the contract states:

- **The architect authors the *shape* that receives the evaluation engineer's evidence.** Evaluation-run event schema in family 1 of the audit-log substrate (chapter 02); the "evaluations" card section schema (chapter 03); the assessment-results shape in the OSCAL catalog (chapter 06); the evaluation-summary section shape in every packet template (chapter 05). These shapes are the architect's and the architect defends them against ad-hoc mutation.
- **The evaluation engineer authors the *methodology* that produces evidence to that shape.** The specific evaluations run per system, the sampling design, the eval-set curation and versioning, the seed policy, the metric choice, the calibration methodology, the red-team engagement methodology. These decisions are the evaluation engineer's and the architect does not overwrite them.

The three-round procedure the contract runs (each round is time-boxed and versioned):

**Round 1 — architecture proposal from the architect.** The architect drafts a section of the evidence architecture that depends on evaluation methodology (a family-1 event-schema change, a card-section schema change, an OSCAL assessment-results extension, a packet-template evaluation-section change). The proposal is written to be executable from the evaluation engineer's chair — not "the evaluation engineer will populate the evaluation section appropriately" but "the evaluation section carries the following fields with the following semantics; the evaluation engineer's methodology output must project into these fields." The proposal explicitly defers a defined set of methodology-shaped questions that the architect does not answer.

**Round 2 — methodology response from the evaluation engineer.** The evaluation engineer reads the proposal, answers the methodology questions, marks any architecture-shaped assumptions the proposal makes that the evaluation engineer disputes, and returns the proposal marked. Common disputes: the schema forces a metric shape the methodology does not naturally produce; the schema over-flattens uncertainty representation; the schema is silent about a category of evidence (drift-in-calibration, methodology change over the eval set) that the methodology actually emits.

**Round 3 — reconciliation and ratification.** Architect and evaluation engineer meet, reconcile the marked proposal, and produce a shared final. Where the reconciliation cannot be reached, the architect escalates to the head of AI governance for either resource allocation or architecture adjustment. The shared final is versioned into the evidence architecture; both roles sign.

Standing forums the contract requires:

- **Monthly architect-and-evaluation-engineer sync.** Methodology roadmap; upcoming card-schema evolution; drift-trigger tuning in the substrate; audit-artefact preparation for the internal-audit population that samples evaluation work (mod-107 chapter 04). One hour, agenda-driven, minutes filed to the governance-workflow log.
- **Quarterly joint architecture-and-evaluation council.** Head of AI governance chairs; architect, evaluation engineer, risk engineer, analyst attend; portfolio-wide taxonomy amendments and schema-evolution items are ratified. Minutes filed to the governance-workflow log with retention aligned to the AIMS documented-information register.

Decision rights the contract pins:

- **Schema changes.** Architect proposes; evaluation engineer responds; joint ratification at monthly sync or council. Change lands in the OSCAL catalog with semver bump and change record filed to governance-workflow log.
- **Evaluation-methodology changes.** Evaluation engineer proposes; architect responds on evidence-shape implications (does the change fit the current schema? does it require a schema evolution?); joint ratification at monthly sync. Methodology change lands in the evaluation engineer's methodology register with cross-reference to any schema evolution triggered.
- **Joint boundary artefacts.** Evaluation-run event schema in family 1; "evaluations" card-section schema; assessment-plan and assessment-results in the OSCAL catalog; evaluation-summary section in each packet template. Both roles sign every version.

A schematic of the peer contract:

```yaml
coordination_contract_evaluation_engineer:
  version: 1.0.0
  parties:
    - senior-ai-governance-architect (level 50)
    - ai-evaluation-engineer (peer, level 35)
  scope_of_shared_ownership:
    - family-1 evaluation-run event schema (chapter 02)
    - "evaluations" card-section schema across model / system / dataset cards (chapter 03)
    - "evaluation-summary" section shape in every packet template (chapter 05)
    - OSCAL assessment-plan and assessment-results content shape (chapter 06)
    - reproducibility-package schema stored in the substrate
    - red-team / adversarial-evaluation record schema
  three_round_procedure:
    round_1: architect drafts architecture proposal with methodology-shaped questions
             explicitly deferred
    round_2: evaluation-engineer marks the proposal with methodology answers and
             architectural-assumption disputes
    round_3: reconciliation; shared final; both sign
    escalation: irreconcilable -> head-of-ai-governance for resource or architecture
                adjustment
  boundary_the_architect_does_not_cross:
    - choosing the specific evaluations to run per system
    - designing eval-set curation, versioning, seed policy
    - selecting metrics or calibration statistics
    - negotiating red-team engagement scope
    - integrating external evaluation methodology (AISI publications; NIST evaluation
      methodology; MLCommons; HELM)
  boundary_the_evaluation_engineer_does_not_cross:
    - authoring the schemas that receive evaluation evidence
    - authoring regulator packet templates
    - authoring the audit-log substrate or its immutability posture
    - authoring the OSCAL profile (as opposed to producing assessment-results content
      against it)
  standing_forums:
    - monthly architect-evaluation-engineer sync
    - quarterly joint architecture-and-evaluation council with head-of-ai-governance,
      risk-engineer, analyst attending
  filing_discipline:
    - every reconciled change filed to governance-workflow log with semver bump
    - every version of every joint artefact signed by both parties
```

## The role-scope contract with the risk engineer

The risk engineer at level 25 is downstream of the architect (reports through the head of AI governance); the coordination is a role-scope contract the architect authors and the head of AI governance ratifies. The contract is *not* a job description in the HR sense — it is an evidence-architecture artefact that specifies what the risk engineer produces, at what quality, at what cadence, subject to what escalation.

Primary outputs, competences held vs. not held, escalation discipline, and boundaries:

```yaml
role_scope_contract_risk_engineer:
  version: 1.0.0
  role: ai-risk-engineer (level 25)
  reports_to: head-of-ai-governance (level 60)
  authored_by: senior-ai-governance-architect (level 50)
  ratified_by: head-of-ai-governance
  primary_outputs:
    - risk_card_content_per_system:
        input: system under governance; mod-106 taxonomy; system's operating context
        output: risk-card content the analyst files against the chapter-03 risk-card schema
        quality_bar: every risk named against taxonomy; scoring rationale documented;
                     treatment plan cross-referenced; residual-vs-appetite reconciled
        cadence: per-system at first publication; on drift; on incident; periodic per
                 mod-107 chapter 03 re-affirmation
        escalation: risk not describable under current taxonomy -> architect for
                    taxonomy amendment (mod-106); scoring disagreement with model
                    owner -> head-of-ai-governance
    - risk_register_entries:
        input: system risk-card outputs; incident records; drift-trigger firings
        output: risk-register entries against mod-106 chapter 07 schema
        quality_bar: register current within window; cross-references complete;
                     appetite-reconciliation surfaced monthly
        cadence: continuous
        escalation: register drift -> head-of-ai-governance
    - risk_treatment_plan_artefacts:
        input: risk-card content + system's residual-vs-appetite state
        output: RTP artefact filed to AIMS documented-information register (mod-105
                chapter 07) and referenced from Article 11 packet, SR 11-7 packet, and
                OSCAL system plan
        quality_bar: named controls per treatment; owners named; timings named;
                     effectiveness-review schedule pinned
        cadence: per-system at publication; on material change
        escalation: treatment underspecified -> second-line chair at pre-deployment
                    gate for gate escalation route
    - quantitative_risk_scoring_output:
        input: system evidence; mod-106 quantitative methodology; evaluation-engineer
               calibration outputs where applicable
        output: quantitative scoring output populating card and packet residual
                narrative
        quality_bar: methodology cited; inputs cited; uncertainty represented;
                     calibration status disclosed
        cadence: at publication; on material change; on calibration drift
        escalation: scoring methodology disagreement with evaluation engineer ->
                    peer-to-peer reconciliation; if unresolved -> architect for
                    boundary ratification
    - drift_driven_re_assessment_records:
        input: drift-trigger firing (mod-107 chapter 03)
        output: re-assessment record filed to ongoing-assurance evidence slice
        quality_bar: change analysed; revised scoring produced; revised treatment
                     produced; downstream card and packet updated within window
        cadence: per-firing
        escalation: re-assessment not produced within window -> programme monthly
                    review finding
    - risk_taxonomy_maintenance_input:
        input: recurring risks not describable under current mod-106 taxonomy
        output: taxonomy-amendment proposal filed to architect
        quality_bar: proposal named; example systems cited; proposed placement
                     articulated
        cadence: on recurrence
        escalation: taxonomy amendment refused -> head-of-ai-governance
  competences_required:
    - deep literacy on mod-106 risk taxonomy and its quantitative methodology
    - literacy on the enterprise's control library (mod-102) sufficient to write
      treatment plans that reference real controls
    - literacy on ISO/IEC 42001 and the enterprise's applicable regulatory obligations
      (mod-104) sufficient to reconcile appetite against obligation
    - working knowledge of evaluation-engineer output shape sufficient to consume
      calibration outputs and reproducibility packages
  competences_the_risk_engineer_does_not_hold:
    - authoring the card schemas or the packet templates (that is the architect)
    - authoring evaluation methodology (that is the evaluation engineer)
    - authoring the OSCAL profile or the substrate's immutability posture (architect)
    - authoring the assurance architecture itself (architect; mod-107)
  boundary_with_evaluation_engineer:
    - calibration outputs flow evaluation-engineer -> risk-engineer; risk-scoring
      does not overwrite calibration methodology
    - disagreement resolves peer-to-peer with escalation to architect on boundary
      questions and to head-of-ai-governance on resource questions
  boundary_with_analyst:
    - risk engineer authors risk-card content; analyst files the resulting card
      through the publication gate; analyst does not author risk content
    - risk-register entries authored by risk engineer; register housekeeping done by
      analyst
  supervision_and_growth:
    - technical supervision: head-of-ai-governance
    - architectural direction: architect (through role-scope contract, schema, and
      the mod-106 taxonomy; not through day-to-day tasking)
    - growth path: risk engineer -> senior risk engineer -> architect track after
      architecture depth developed
```

## The role-scope contract with the analyst

The analyst at level 15 is the level-15 seat with the shallowest competence surface and the widest daily contact with the substrate. The role-scope contract for the analyst is the most consequential of the three because analyst-tier work is where most volume lives and where boundary drift is most likely.

```yaml
role_scope_contract_analyst:
  version: 1.0.0
  role: ai-governance-analyst (level 15)
  reports_to: head-of-ai-governance (level 60)
  authored_by: senior-ai-governance-architect (level 50)
  ratified_by: head-of-ai-governance
  primary_outputs:
    - card_draft_authoring_and_drive_to_publication:
        input: first-line model-owner source material; chapter-03 card schema; risk-card
               content from risk engineer; evaluation content from evaluation engineer
        output: signed and filed card in the card registry
        quality_bar: card complete against tier-applicable schema; publication gate
                     minutes filed; signature verifiable
        cadence: per-artefact at publication; on re-review trigger per chapter 03
        escalation: source material insufficient to populate mandatory fields ->
                    first-line model owner; if unresolved -> head-of-ai-governance;
                    schema field with no substrate content across systems ->
                    architect (evidence gap register)
    - aia_records:
        input: mod-105 chapter 04 AIA methodology and shape
        output: AIA record filed to AIMS documented-information register
        quality_bar: mod-105 chapter 04 quality bar; cross-referenced to card family
        cadence: per-system at initial assessment; on material change
        escalation: AIA judgement calls beyond analyst competence -> head-of-ai-
                    governance or risk engineer per mod-105 chapter 04
    - substrate_ingestion_sanity_checks:
        input: architect-specified sanity queries against family-1, family-2, family-3
               logs (chapter 02)
        output: ingestion sanity report filed to governance-workflow log
        quality_bar: queries run per cadence; anomalies surfaced within window
        cadence: daily automation; weekly analyst review
        escalation: substrate anomaly detected -> architect and the immutability
                    sub-plan owner; if the anomaly is a control-implementation gap
                    -> first-line owner per second-line chair route
    - regulator_packet_assembly_pipeline_execution:
        input: regulator ask; template version; substrate snapshot
        output: assembled packet in packet registry; signer-review calendar entry
        quality_bar: chapter-05 packaging discipline; template and snapshot versions
                     recorded on the filed packet
        cadence: per-ask; per-scheduled-cadence (Article 72 post-market cadence, for
                 example)
        escalation: assembly pipeline flags substrate-versus-template divergence ->
                    architect; missing signature -> signer's seat; missing signature
                    beyond window -> head-of-ai-governance
    - oscal_system_governance_plan_authoring:
        input: architect-authored OSCAL profile (chapter 06); system's applicable
               controls declaration; system's tier and risk-card content
        output: per-system OSCAL system plan filed to catalog
        quality_bar: plan complete against profile; evidence artefacts cross-
                     referenced with resolvable IDs
        cadence: per-system at first publication; on material change; on control-
                 library evolution
        escalation: profile does not describe a real applicability -> architect
                    (evidence gap register entry)
    - evidence_gap_register_maintenance:
        input: gaps surfaced during card authoring, packet assembly, or OSCAL plan
               authoring
        output: gap register current
        quality_bar: gaps named; systems cited; frequency tracked; architect notified
                     at recurrence threshold
        cadence: continuous with monthly summary
        escalation: recurring gap unresolved by architect -> head-of-ai-governance
  competences_required:
    - literacy on the enterprise's control library (mod-102) and evidence contract
      (this module)
    - literacy on ISO/IEC 42001 clauses (mod-105) sufficient to file correctly
    - literacy on the enterprise's applicable regulatory obligations (mod-104)
      sufficient to route artefacts by regime
    - use of the GRC-for-AI platform (mod-111) if adopted
    - facility with the assembly pipeline and the OSCAL tooling
  competences_the_analyst_does_not_hold:
    - authorship of the card schemas or the packet templates (architect)
    - risk-scoring judgement or risk-card content authorship (risk engineer)
    - evaluation-methodology judgement or evaluation-report authorship (evaluation
      engineer)
    - authorship of the OSCAL profile or the substrate's immutability posture
      (architect)
    - regulatory-posture judgement calls (legal + head-of-ai-governance)
  authoring_cadence:
    - card cadence: at publication; on re-review trigger per chapter 03
    - AIA cadence: per-system at initial assessment; on material change
    - packet-assembly cadence: per-regulator-ask; per-scheduled-cadence
    - OSCAL plan cadence: per-system at publication; on material change
    - gap register cadence: continuous with monthly summary
  boundary_with_risk_engineer:
    - risk-card content authored by risk engineer; analyst files the resulting card
      through publication gate
    - risk-register entries authored by risk engineer; analyst does housekeeping
    - RTP artefact authored by risk engineer; analyst may cross-reference from card
      or packet
  supervision_and_growth:
    - technical supervision: head-of-ai-governance (or a designated senior seat)
    - architectural direction: architect (through role-scope contract, schema, and
      assembly pipeline; not through day-to-day tasking)
    - growth path: analyst -> senior analyst -> risk-engineer or evaluation-engineer
      after methodology depth is developed
```

Escalation discipline for the analyst is the most consequential part of the contract. Two escalations are worth naming explicitly:

- **Analyst finds a substrate gap the schema does not describe.** The correct escalation is *to the architect*, not to the auditor, not to the head of AI governance in the first instance. The gap indicates a schema evolution the architect must consider. Escalating to the auditor produces a finding; escalating to the head-of-ai-governance produces a resource conversation. Neither closes the gap architecturally. The evidence gap register is the standing mechanism.
- **Analyst finds an evaluation-methodology or risk-scoring question in the source material during card assembly.** The correct escalation is *to the peer role* (evaluation engineer or risk engineer) through the second-line chair, not to the architect and not to the auditor. The analyst does not adjudicate; the analyst routes.

## Interface between the three roles

The three roles interact with each other under the architect's schema authority. The interface can be represented as a who-authors / who-reviews / who-signs / who-ratifies schematic:

| Artefact class | Author | Reviewer | Signer | Ratifies |
|---|---|---|---|---|
| Model-card schema | architect | evaluation engineer (peer) | architect | head-of-ai-governance |
| System-card schema | architect | evaluation engineer (peer) | architect | head-of-ai-governance |
| Dataset-card schema | architect | evaluation engineer (peer) | architect | head-of-ai-governance |
| Risk-card schema | architect | risk engineer | architect | head-of-ai-governance |
| Audit-log event schemas | architect | first-line lead + evaluation engineer for family 1 | architect | head-of-ai-governance |
| ML-BOM and SLSA attestation schemas | architect (via chapter 04 discipline) | first-line lead | architect | head-of-ai-governance |
| Regulator packet templates | architect | legal + head-of-ai-governance | architect | AI-accountable executive (annually) |
| OSCAL profile | architect | analyst (executability review) | architect | head-of-ai-governance |
| Model-card instance (per model) | analyst (draft) | second-line reviewer | seats per chapter 03 | publication gate |
| System-card instance (per system) | analyst (draft) | second-line reviewer | seats per chapter 03 | publication gate |
| Dataset-card instance (per dataset) | analyst (draft) | second-line reviewer | seats per chapter 03 | publication gate |
| Risk-card instance content | risk engineer | analyst (executability against schema) | risk engineer + head-of-ai-governance | publication gate |
| Evaluation-run record content | evaluation engineer or first-line MLE per methodology | evaluation engineer | evaluation engineer | (substrate ingestion is automated) |
| Evaluation report | evaluation engineer | second-line reviewer | evaluation engineer | (feeds cards and packets) |
| Red-team engagement record | evaluation engineer | head-of-ai-governance | evaluation engineer + external engagement lead | head-of-ai-governance |
| AIA record | analyst | risk engineer | analyst + head-of-ai-governance | (mod-105 chapter 04) |
| RTP artefact | risk engineer | second-line reviewer | risk engineer + head-of-ai-governance | (feeds packets) |
| Regulator packet instance | analyst (assembly) | signers per template | seats per template | filed to packet registry |
| Per-system OSCAL plan | analyst | architect (schema conformance) | analyst + head-of-ai-governance | catalog ingestion |
| Ingestion sanity report | analyst | architect | analyst | governance-workflow log |
| Evidence gap register | analyst | architect | analyst | monthly review with architect |

Two invariants the schematic makes visible:

- **No artefact class has an unassigned reviewer or signer.** The gap in any cell is a finding waiting to land. The architect defends the completeness of the schematic quarterly.
- **The architect signs schemas, not instances.** Every instance-level signature belongs to the role whose competence adjudicates the substance. This is the boundary that keeps the architect from becoming coordinator-of-last-resort.

## Boundary discipline — architect does not, will not

An enumerable list of what the architect *does not* do inside the evidence architecture, each pointing to the role that owns it:

- **Author individual model / system / dataset cards.** Analyst authors the draft; risk engineer supplies risk-card content; evaluation engineer supplies evaluation content. Architect ratified the schema; the artefact is not the architect's.
- **Sign individual regulator packets.** Signers per template are the head of AI governance, general counsel, AI-accountable executive, MRM head, regulatory affairs head (chapter 05). Architect authored the template; the packet is not the architect's to sign.
- **Own individual system-governance-plans in the OSCAL catalog.** Analyst authors the per-system plan against the profile the architect authored. Architect ratifies the profile; the per-system plan is the analyst's.
- **Run the analyst's daily workflow.** Card draft queues, packet assembly queues, OSCAL plan authoring queues, ingestion sanity queues — these are the head-of-ai-governance's supervision surface. Architect does not chase, task, or prioritise analyst work directly.
- **Execute evaluation runs.** Evaluation runs are the evaluation engineer's methodology domain (and first-line MLE's execution surface where the methodology delegates). Architect does not author a specific evaluation or select a specific eval set.
- **Author risk-scoring for individual systems.** Risk-card content, quantitative scoring, treatment plan authoring — the risk engineer's. Architect ratifies the taxonomy and the schema; the scoring is not the architect's.
- **Adjudicate methodology disputes between the evaluation engineer and the risk engineer.** Boundary reconciliation between peer methodology domains resolves peer-to-peer first, then escalates to the head of AI governance for resource questions and to the architect only where the dispute is about the *shape* the substrate holds their outputs in.
- **Serve as the operational escalation destination for analyst work.** Analyst escalations route through the head of AI governance (day-to-day) or through the second-line chair (gate-adjacent) or through the peer role (methodology-adjacent). The architect receives escalation only on schema-evolution questions and evidence gap register recurrence.

Each item is a *does not* the architect defends against drift. The drifts that erode this list are the two failure modes named below.

## Composition with mod-107 assurance architecture

mod-107 chapter 06 authored the coordination contracts for the *assurance* side of the architect's mandate — the pre-deployment gate, the ongoing assurance programme, and the internal-audit-facing surface. This chapter authors the coordination contracts for the *evidence* side. The two composed pin the peer / role landscape the architect operates against across both modules.

The composition is not additive; it is *the same three roles on both sides of the boundary*. The analyst who assembles the pre-deployment gate evidence package (mod-107 chapter 02) is the same analyst who authors the card draft (this chapter). The risk engineer who authors the risk-card content is the same risk engineer whose calibration outputs feed the ongoing-assurance drift triggers. The evaluation engineer who runs the release-assurance methodology inside the pre-deployment gate is the same evaluation engineer whose evaluation reports populate the "evaluations" section of every card and packet. Coordination has to compose or the roles fracture.

Three specific composition items the architect maintains:

- **A single role-scope contract per role, covering both mod-107 and mod-108 outputs.** The analyst has one role-scope contract; the risk engineer has one; the evaluation engineer has one peer contract. The primary-outputs section of each is the union of the mod-107 and mod-108 outputs. Two contracts per role produces two escalation routes and two competence assessments; one contract holds.
- **A shared standing forum cadence.** The monthly architect-and-evaluation-engineer sync (this chapter) is the same monthly sync mod-107 chapter 06 named. The quarterly joint council is the same quarterly council. Doubling the forums fractures attention.
- **A shared change-control discipline.** Every schema change, every methodology change, every role-scope-contract amendment goes through the same versioning discipline and is filed to the governance-workflow log with the same retention. The evidence side and the assurance side share a change-log.

Where mod-107 chapter 06 named an artefact (audit-artefact contract, gate-preparation checklist, re-assessment-preparation checklist), this chapter treats those artefacts as substrate inputs to the evidence architecture — the audit-artefact contract's evidence expectations are honoured by the substrate; the gate-preparation checklist is executed against the substrate the analyst maintains; the re-assessment record is a substrate write. The two modules author against each other; neither is complete alone.

## Six invariants the coordination holds

**Invariant 1 — every artefact class has a named author, reviewer, signer, and consumer.** The chapter-01 stance 5 that named roles per artefact type is executable only because the artefact-class schematic above enumerates each. Vacant cells in the schematic are findings-in-waiting. Test: quarterly review of the schematic; every artefact class that appears in the substrate but not in the schematic is either added or removed; every cell has a named seat.

**Invariant 2 — boundary disputes ratify at the architect for shape and at the head of AI governance for resource.** Two escalation destinations, chosen by dispute type. Disputes about the *shape* the substrate holds an output in ratify at the architect (schema question). Disputes about the *capacity* to produce the output ratify at the head of AI governance (resource question). Test: sample a quarter's boundary-dispute log; every dispute has a named destination and a resolution timestamp.

**Invariant 3 — peer coordination with the evaluation engineer is standing rather than ad-hoc.** The monthly sync and the quarterly council are on the calendar, not summoned when a crisis lands. Test: minutes filed for every monthly sync and every quarterly council over the last four quarters; zero cancelled without rescheduling.

**Invariant 4 — escalation discipline is documented in the role-scope contract, not learned in-flight.** Every role-scope contract enumerates the escalations by input type with named destinations. Test: for each role, walk the primary-outputs section; every named quality-bar failure has a named escalation.

**Invariant 5 — the architect does not become coordinator-of-last-resort.** The boundary-discipline enumeration in the section above holds. Test: sample a quarter's architect calendar and inbox; measure fraction of time on schema evolution / role-scope contract maintenance / joint council / boundary reconciliation vs. fraction on individual card / packet / plan chasing. The former dominates; the latter shrinks.

**Invariant 6 — role-scope contracts are versioned artefacts.** Each contract has a semver, a change log, a ratification history. Amendments go through the same discipline as schema changes. Test: retrieve the role-scope contract for any role at any prior version; every prior version resolvable; the change log connects them.

## Two failure modes to design against

**Failure mode 1 — the analyst-as-fallback.** The evaluation engineer is under-resourced; the risk engineer is on parental leave; a card publication gate is landing this week. The analyst is asked to "just fill in the evaluation section" or "just draft the risk-card content and we'll review after publication." The analyst does not have the methodology depth to author either; the substance is nominal; the publication nevertheless treats it as authored. A subsequent internal audit finds an evaluation section citing metrics the evaluation engineer never selected, or a risk card scoring against a methodology the risk engineer does not recognise. The fix is *strict competence boundaries in the role-scope contract*: analyst-tier authorship is card draft assembly, AIA records, packet assembly, OSCAL plan, gap register, ingestion sanity. Methodology-depth authorship is the peer role. When the peer role cannot execute at gate cadence, the fix is capacity, not delegation to the analyst; the publication slips or the head of AI governance re-allocates capacity. The card is not published against a nominal section.

**Failure mode 2 — the architect-as-manager.** The architect drifts into direct tasking. "Please assemble the Article 11 packet for system X by Thursday." "I need you to run this specific evaluation with these thresholds for the gate." "Draft the risk card content for the new deployment and send me the first cut." Each drift is understandable — the deadline is real, the resource shortfall is real, the alternative feels worse in the moment — and each breaks the coordination shape. The head of AI governance loses visibility into the analyst's queue; the evaluation engineer's methodology decisions become architect-directed rather than methodology-owned; the risk engineer's authorship of risk content becomes advisory. Once the pattern establishes, undoing it is expensive. The fix is *architect-as-schema-author, not tasker*: the architect responds to a capacity shortfall by proposing a schema simplification, a template version bump, or a resource escalation to the head of AI governance — not by taking the work directly. Direct tasking is legible in the architect's calendar; the fix begins with re-planning the week rather than accepting one more emergency assist.

Both failure modes have the same tell: the substrate accumulates content whose author is not who the schematic in the "Interface between the three roles" section names. Quarterly audit of the schematic against the substrate surfaces both drifts before they become the operating norm.

## Summary

The evidence architecture the previous six chapters designed is populated correctly only because three coordination contracts hold. The `ai-governance-analyst` at level 15 authors card drafts, AIA records, ingestion sanity checks, packet assembly, per-system OSCAL plans, and the evidence gap register — against schemas the architect authored, under the head of AI governance's day-to-day supervision, escalating gaps to the architect and methodology questions to the peer roles. The `ai-risk-engineer` at level 25 authors risk-card content, risk-register entries, RTP artefacts, quantitative risk scoring output, and drift-driven re-assessment records — against the mod-106 taxonomy and the chapter-03 risk-card schema. The `ai-evaluation-engineer` at level 35 is a peer, coordinated through a three-round proposal / response / reconciliation contract, authoring evaluation-run records, evaluation reports, red-team records, gate-consumable assurance evidence, and reproducibility packages — against shape schemas the architect and evaluation engineer jointly steward. The architect does not author individual instances, does not sign individual packets, does not run analyst workflow, does not execute evaluations, does not score individual systems, does not adjudicate peer methodology disputes. The two failure modes (analyst-as-fallback, architect-as-manager) are both preventable by holding the role-scope contracts and the boundary-discipline enumeration current. The composition with mod-107 chapter 06 fixes the shared role landscape across the assurance and evidence sides.

Chapter 01's stance 5 named "roles are assigned per artefact-type" as an architectural commitment; this chapter is that commitment executed. Chapter 01's invariant 6 tested for "roles are assigned per artefact type"; the schematic in this chapter is the standing test surface. Every subsequent module the architect composes with — mod-109 (third-party and supply-chain governance), mod-110 (post-market monitoring architecture), mod-111 (GRC-for-AI platform), mod-112 (executive-facing communication and board packet architecture) — inherits the same three-role landscape and the same architect-as-schema-author discipline. mod-109 in particular composes tightly: the third-party evidence slice the enterprise consumes from vendors (SBOM-M, third-party model cards, upstream evaluation reports) drops into the substrate this module designed, under the same schemas, ingested by the same analyst, reviewed by the same risk engineer, verified by the same evaluation engineer for methodology plausibility, ratified by the architect only on schema evolution. Coordination contracts across the whole architecture surface compose or fracture; this chapter is the local pinning of the three-role coordination that all subsequent modules assume.
