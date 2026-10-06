# exercise-02: Model and System and Dataset Card Schema Authoring

**Estimated effort:** 3 hours

## Objective

Author the **card-family schema** — the machine-readable JSON Schemas the level-15 analyst will fill against when they author a model, system, dataset, or risk card for a specific system. The deliverable is *not* an individual card for an individual system; it is the schema shape itself, plus one worked instance per card class demonstrating the schema in use. This is the architect's job: the analyst downstream authors cards *against* your schemas, and the OSCAL catalog (chapter 06) ingests the front-matter your schemas define.

Chapter 03 fixes the stance: a card is a controlled, versioned, signed disclosure, authored in Markdown for humans and emitted as JSON / YAML for machines, and traceable into the audit-log substrate (chapter 02) by ID and hash. Your schemas encode that stance. Get the schemas right and the analyst can produce cards at scale that pass the publication gate; get them wrong and every downstream card inherits the flaw — the analyst is forced to invent fields, the publication gate cannot mechanically check completeness, and the OSCAL catalog receives inconsistent front-matter across cards for the same system.

## Prerequisites

- Chapter [`03-the-card-family-model-system-dataset-and-risk-cards.md`](../03-the-card-family-model-system-dataset-and-risk-cards.md) read once, with the four card sections and the six invariants marked.
- Chapter [`02-audit-log-architecture-retention-and-immutability.md`](../02-audit-log-architecture-retention-and-immutability.md) — every card publication produces a family-3 governance-workflow log event, and every substrate binding in a card is an ID into families 1 (training / evaluation runs), 2 (datasets), or 3 (governance workflow).
- Chapter [`04-ai-supply-chain-evidence-cyclonedx-spdx-slsa-sigstore.md`](../04-ai-supply-chain-evidence-cyclonedx-spdx-slsa-sigstore.md) — the ML-BOM reference the model card emits binds here.
- Exercise-01 substrate design available (cards live inside the substrate design's event and content stores).
- The mod-106 risk register schema — the risk card cross-references risk-register entries by `RR-ID`; do not duplicate the register's fields into the risk card, cross-reference them.
- The mod-105 chapter 05 Clause 7.5 documented-information discipline — cards are Clause-7.5 documented information; their control-of-documented-information practice must be legible.
- Primary references you must cite by name in the schema comments (see [`../resources.md`](../resources.md)):
  - Mitchell et al., "Model Cards for Model Reporting," FAT* 2019.
  - Gebru et al., "Datasheets for Datasets," *Communications of the ACM*.
  - Hugging Face Model Cards guide (`huggingface.co/docs/hub/model-cards`).
  - Partnership on AI ABOUT ML framework.
  - UK Algorithmic Transparency Recording Standard (ATRS) — <!-- needs-research: verify current ATRS publication URL and current tier structure at gov.uk -->
  - OpenAI, Anthropic, and Google DeepMind system cards for major model releases (cite at title level only; **do not** fabricate specific model names, publication dates, or field-level claims).

## Scenario

You are the level-50 architect at one of the following enterprises. Pick the one whose card discipline you are least familiar with; state your choice at the top of the deliverable.

- **A US regional bank** (Northbrook Financial-style) with ~40 AI systems: fraud classifiers, document extraction, an internal RAG legal assistant, a customer-facing generative chat, third-party AI SaaS integrations. Colorado + NYC deployments; EU expansion planned. SR 11-7-aligned MRM programme and an ISO/IEC 27001 ISMS in place.
- **A global healthcare payer / provider** with clinical-decision-support pilots, patient-facing chat, coding automation, and utilisation-management AI. US federal HIPAA scope; EU AI Act relevance for European insurance subsidiaries; multi-state US deployments including Illinois and California.
- **A B2B SaaS platform vendor** shipping GenAI-augmented HR-tech capabilities into enterprise customers across the US, UK, EU, and Singapore. Customers include public-sector deployments (OMB M-25-21 shape) and financial-services enterprises (SR 11-7 vendor-review shape). Customer-facing DPA appendices are a first-order card audience.

## Deliverables

Author seven artefacts in a working directory of your choice.

1. **`card-family-schema.md`** — the decision document declaring the card-family schema shape, the tiering, the review workflow, the publication gate, the shared front-matter cross-cutting fields.
2. **`schemas/model-card.schema.json`** (or `.yaml`) — the JSON Schema for the model card.
3. **`schemas/system-card.schema.json`** — the JSON Schema for the system card.
4. **`schemas/dataset-card.schema.json`** — the JSON Schema for the dataset card.
5. **`schemas/risk-card.schema.json`** — the JSON Schema for the risk card.
6. **`worked-instances/`** — one filled-in card per class (model / system / dataset / risk) for one representative system in your scenario. Each instance validates against its schema.
7. **`card-lifecycle-and-publication-gate.md`** — the review workflow, the publication gate composition and checks, the signature discipline, the re-review triggers, the deprecation procedure.

## Requirements

### `card-family-schema.md`

Decide and justify each of the following:

- **Card classes and the scope boundary between them.** Four classes (model / system / dataset / risk) or a collapsed three-class shape. Defend the choice against the chapter 03 warning that collapsing model and system cards tends to produce cards that describe the model well and the surrounding system poorly.
- **Tiering.** Which tiers (from mod-105 chapter 02's AI-inventory tiering) require which cards, at which content depth. Chapter 03's tier-3 and tier-4 overlay pattern is the shape; you specify the tier-to-card matrix.
- **Shared front-matter schema.** The cross-cutting fields every card carries (`card_type`, `card_schema_version`, `card_version`, `system_id`, `tier`, `jurisdictions_in_scope`, `authored_by`, `reviewed_by`, `signed_by`, `substrate_bindings`, `published_at`, `publication_signature`, `publication_scope`, `next_review_at`, `re_review_triggers`). Types, enums, and required-vs-optional per tier.
- **Machine-readable representation stance.** JSON Schema draft version (e.g. draft `2020-12`); every field typed; enums where content is controlled (e.g. `card_type`, `publication_scope`, `jurisdictions_in_scope`); `$id` and `$ref` discipline across the four schemas so the shared front-matter is one referenced schema; `additionalProperties: false` posture and how you handle enterprise-specific extension fields.
- **Author / reviewer / signer role model.** The named seats per card class per section. Chapter 03 fixes the role-per-section discipline; you spell it out: model-owner drafts model card details; evaluation-engineer signs off the quantitative-analyses section for tier-3 and tier-4; risk-engineer drafts the risk card; governance-analyst assembles and is second-line reviewer across all four.
- **OSCAL back-matter binding.** Preview of chapter 06. Each card is an OSCAL `resource` referenced from the SSP `implemented-requirement`; state the `resource.uuid`, `resource.rlink.href`, and `resource.rlink.hash` shape and how the card front-matter emits into it.
- **Non-scope.** At least three things you deliberately do *not* put into the schema — candidates: the AI Impact Assessment (a separate decision artefact per mod-105 chapter 04, cross-referenced not embedded); user-manual content (the card is not an end-user document); an EU-AI-Act-Article-11-shaped technical file (the technical file is a *packaging* over the card content per chapter 05, not the card itself).

### `schemas/model-card.schema.json`

The model card's JSON Schema. Grounded in Mitchell et al. FAT* 2019, extended with the enterprise substrate-binding fields chapter 03 pins.

Mandatory (across all tiers) top-level sections that must be modelled as required properties:

- **Model details.** `model_name`, `model_version` (semver), `artefact_hash`, `training_run_pipeline_id` (family-1 substrate ID), `base_model` (nullable; for fine-tunes), `architecture`, `parameter_count`, `training_compute_order_of_magnitude`, `authoring_team`, `licence`, `contact`.
- **Intended use.** `primary_intended_uses` (array), `primary_intended_users` (array of population descriptors), `out_of_scope_uses` (array). This is the section the deployer and the regulator read first; the schema must forbid the analyst leaving it empty.
- **Factors.** `evaluated_factors` (array of factor objects; each factor names the group, environment, or deployment context and states whether evaluation covers it or explicitly does not).
- **Metrics.** `metrics` (array of metric objects with `name`, `selection_rationale`, `uncertainty_characterisation`).
- **Evaluation data.** `evaluation_dataset_refs` (array of `DS-ID` values cross-referenced to dataset cards); `evaluation_run_pipeline_ids` (family-1 substrate IDs); evaluation-set version hashes and seeds.
- **Training data.** `training_dataset_refs` (array of `DS-ID` values); the disclosure-restriction posture for proprietary corpora (a controlled enum for restriction reasons).
- **Quantitative analyses.** `disaggregated_results` (array of result objects with `factor_ref`, `metric_ref`, value, and uncertainty band); this field should reference an evaluation-run ID, not require the analyst to type numbers by hand (chapter 03 discipline; preview of chapter 07 coordination).
- **Ethical considerations.** `foreseeable_harms`, `mitigations`, `unresolved_concerns`; cross-references to the risk card and the AIA.
- **Caveats and recommendations.** `residual_limitations` (array).

Tier-3 and tier-4 overlay properties (required only at those tiers): `red_team_engagement_summary_ref` (ID of a red-team run in the substrate); `robustness_adversarial_ml_measurements`; `interpretability_reason_code_evidence_ref`; `independent_evaluation_attestation_ref` (signed by the level-35 evaluation engineer per chapter 07).

Also required:

- **JSON Schema draft version** declared at `$schema`.
- **Every enum externally referenced** where the enum is shared (jurisdictions, publication scopes, restriction reasons) — do not duplicate enums across the four schemas.
- **Author / reviewer / signer per section** — encoded either as schema-level annotations (`x-authoring-role`, `x-signing-role`) or documented in the accompanying `card-family-schema.md` with a table the schema references.
- **Version discipline.** `card_version` is a semver; any change to a content-material field bumps at least the minor. State which fields are "content-material" and which are "housekeeping."
- **Deprecation.** A `deprecated` flag with a required `deprecated_reason` and `deprecated_at` when set; the schema should forbid a `deprecated: true` card being published to `publication_scope` values that are external-facing.
- **Hugging Face compatibility projection.** A note in the schema (comment or `description`) identifying which fields project into HF Model Card metadata (`license`, `datasets`, `metrics`, `tags`, `pipeline_tag`, `model-index`) when the model is pushed to a registry. <!-- needs-research: verify current HF Model Card metadata field list at huggingface.co/docs/hub/model-cards -->

### `schemas/system-card.schema.json`

The integrated-system disclosure. Chapter 03 grounds the shape in the frontier-lab system-card practice — cite at title level (OpenAI system cards; Anthropic system cards; Google DeepMind system cards) but do not fabricate specific content from any of them. The enterprise's schema is a defensible derivative sized for the enterprise's own systems.

Mandatory top-level sections:

- **System identity.** `system_name`, `system_version`, `deployment_scope` (population, jurisdictions, product surface), `owner_team`.
- **System composition.** `model_refs` (array of model-card IDs; the system card cites the model card, it does not duplicate the model card); `retrieval_stores` (array of retrieval-store objects with dataset-card cross-references); `tools` (array of tool objects — function-calling interfaces, code-execution sandboxes, web-browsing capabilities, external APIs); `guardrails` (array of input filter, output classifier, refusal-pattern objects).
- **Prompt-engineering surface.** `system_prompt_ref` (an ID into the substrate; the schema should forbid the analyst pasting the full system prompt into a field where a substrate reference belongs); `accepted_user_input_shape`; `prompt_injection_posture`.
- **Human-oversight design.** `oversight_surface` cross-referenced to the mod-105 chapter 09 human-oversight architecture; `reviewer_roles`, `review_latency_sla`, `reviewer_authority`.
- **Guardrails and safety features.** `content_safety_classifiers`, `refusal_patterns`, `escalation_paths`, `kill_switch_design`.
- **Known behaviours.** `intended_behaviours`, `known_unintended_behaviours` (hallucination classes; systematic biases; adversarial failure modes).
- **Evaluation summary.** `system_level_evaluation_run_refs` (family-1 substrate IDs); `red_team_summary_ref`.
- **Change history.** `material_changes` (array of change objects with substrate event refs).

Tier-3 and tier-4 overlay properties: `post_market_monitoring_plan_ref` (mod-110 cross-reference); `serious_incident_response_plan_ref`; `rollback_kill_switch_demonstration_record_ref`.

Also required:

- Cross-reference discipline: the system card cites model cards, dataset cards (via the model card's `evaluation_dataset_refs` and `training_dataset_refs`, and via the retrieval-store objects), and risk cards. Every cross-reference is an ID with schema-enforced format.
- Deployment-environment constraints as an enumerable list: `deployment_environment_constraints` (e.g. locale requirements, latency SLOs, data-residency posture, accessibility posture) — the field is required and the enum is shared.

### `schemas/dataset-card.schema.json`

Grounded in Gebru et al. Datasheets for Datasets. Mandatory sections modelled as required properties:

- **Motivation.** `dataset_purpose`, `intended_tasks`, `intended_populations`, `funding_creation` — who funded and created.
- **Composition.** `instance_types`, `instance_count`, `sampling_process`, `recommended_splits`, `pii_posture` (controlled enum), `sensitivity_posture` (controlled enum).
- **Collection process.** `acquisition_method`, `sampling_method`, `annotation_method`, `time_frame`, `collector_identity` (individuals / automated processes), `consent_notice_arrangements` (required where `pii_posture` is anything other than `no-pii`).
- **Preprocessing / cleaning / labelling.** `transformations_applied`, `labelling_protocol_ref`, `annotator_population`, `inter_annotator_agreement` (nullable but required where labelling is human-mediated).
- **Uses.** `intended_uses`, `discouraged_uses`, `known_off_label_uses`, `downstream_considerations`; cross-references to `model_card_refs` and `system_card_refs` that use this dataset.
- **Distribution.** `distribution_mode` (controlled enum: internal-only, licensed, sold, restricted), `licence_terms`, `distribution_channel`.
- **Maintenance.** `maintainer`, `versioning_scheme`, `deprecation_schedule`, `retention_posture`, `dsar_modification_handling` (required where personal data is present).

Substrate linkage — mandatory:

- `dataset_registry_id` (the enterprise's dataset-registry ID; the analyst reads it from the mod-108 chapter 02 family-2 log).
- `dataset_manifest_hash` (the manifest hash the training run's substrate event cites).
- `spdx_3_0_record_ref` (chapter 04) — required at tier-3 and tier-4.

Tier-3 and tier-4 overlays: `representativeness_measurements_ref`; `harmfulness_offensive_content_analysis_ref`.

### `schemas/risk-card.schema.json`

Distinct from the risk register (mod-106). The risk register is the *record*; the risk card is the *disclosure*. The risk card cross-references risk-register entries by `RR-ID`, it does not duplicate them.

Mandatory sections:

- **System identity.** `system_id`, `system_version`, `model_card_refs`, `system_card_refs`.
- **Applicable taxonomy categories.** `applicable_categories` (array of mod-106 taxonomy category IDs); `applicability_rationale` (why the excluded categories are excluded).
- **Inherent-risk scoring.** Per applicable category: `inherent_risk_reading`, `scoring_methodology` (controlled enum: qualitative-tier, semi-quantitative-heat-map, quantitative-loss-exceedance), `confidence_band`.
- **Controls in place.** Per applicable category: `control_library_row_refs` (mod-102 control IDs), `implementation_state` (controlled enum: implemented / partial / documented-only / not-yet-implemented), `control_implementation_evidence_refs` (substrate IDs).
- **Residual-risk scoring.** Per applicable category: `residual_reading`, `appetite_tolerance_comparison` (from mod-106 chapter 03 appetite / tolerance table).
- **Control-defeated scoring.** Per applicable category: `control_defeated_reading` (the residual under the control-defeated hypothesis, mod-106 chapter 03 vocabulary; the mod-107 chapter 02 pre-deployment gate reads this field).
- **Monitoring plan.** Per applicable category: `metric`, `threshold`, `trigger_action`; cross-reference to `post_market_monitoring_plan_ref` (mod-110).
- **Accepted residuals.** Where the residual is above tolerance and has been accepted: `acceptance_signature_ref` (mod-105 chapter 03 accountable-executive sign-off record), `acceptance_authority`, `acceptance_expiry`.
- **Risk-register cross-reference.** `risk_register_refs` — array of `RR-ID` values. The schema requires at least one; a risk card that references no risk-register row is a floating disclosure.

Tier-3 and tier-4 overlays: `quantitative_scenario_reads` (a controlled shape for the frontier-lab-style capability-tier / catastrophic-scenario reading — Anthropic RSP, OpenAI Preparedness Framework, Google DeepMind Frontier Safety Framework as shape references cited at title level only); `scenario_narrative` (a plaintiff-facing account with the mitigating factors that make the enterprise's acceptance defensible).

### Cross-cutting requirements (across all four schemas)

- **Author / reviewer / signer per section per tier.** Named seats. Chapter 03's discipline: the risk-engineer signs the risk card; the evaluation-engineer signs the quantitative-analyses section of the model card for tier-3+; the governance-analyst is second-line reviewer across all four; the publication-gate chair signs the front-matter at publication; a legal seat signs where the publication scope includes external-facing audiences.
- **Audit-log substrate linkage.** Every card publication produces a **family-3 governance-workflow event** (chapter 02); the event class names to use — e.g. `card.draft.created`, `card.review.completed`, `card.signoff.completed`, `card.published`, `card.rereview.triggered`. Name the family-3 event classes in `card-family-schema.md`.
- **OSCAL back-matter link discipline.** Preview of chapter 06: each card is an OSCAL `resource` in the SSP back-matter; the SSP `implemented-requirement` `link` points to the `resource.uuid`; the `resource.rlink.href` points to the published card artefact; the `resource.rlink.hash` matches the artefact's signed hash. Spell out the front-matter fields that emit into the OSCAL `resource` shape.
- **Signature discipline.** Sigstore cosign or enterprise PKI. State the choice and the transparency-log posture (Rekor for cosign; enterprise-managed log for PKI). Every signed publication has a Rekor / log entry ID recorded in the front-matter's `publication_signature`.
- **Version discipline.** Semver on `card_version`; any content-material change bumps minor or major. Define "content-material" in `card-family-schema.md` — as a rule of thumb, any change that alters a claim the enterprise makes to an external audience is content-material.
- **Separation of shape from render.** The schemas define the machine-readable shape (JSON); the Markdown body is a rendered projection of the same content in human-readable form. Do not couple the render format to the schema; a card whose schema demands a specific Markdown heading structure is a schema that has confused shape with render (chapter 03 discipline).

### `card-lifecycle-and-publication-gate.md`

For each tier (tier-1 through tier-4 per the enterprise scheme):

- **Named signers.** Who signs which section; who chairs the publication gate.
- **Pre-publication review committee.** The forum, its members, its cadence, its quorum.
- **Publication-gate checks.** The six chapter-03 checks encoded operationally: minimum-fields, traceability, redaction, consistency, audience-appropriateness, signature-block completion. Name the automated checks (schema validation, hash re-verification) and the human checks (redaction, audience appropriateness).
- **Re-review triggers.** Chapter 03's list operationalised: material change (which substrate-binding fields count as material); drift (which monitoring thresholds count); incident (which incident classes trigger); scheduled cadence (tier-3 quarterly for evaluation section; tier-2 annual; tier-4 monthly for evaluation; state your enterprise's numbers); regulatory change (which regulator-scope changes trigger).
- **Deprecation procedure.** How a card is deprecated: `deprecated_at` set; `deprecated_reason` populated; downstream consumers notified (customer-facing DPA appendix consumers via a defined notice channel; internal registry consumers via a defined feed); the deprecated card retained (not deleted) in the substrate.
- **Failure-mode countermeasures.** The two chapter-03 failure modes — the card that lives in a wiki, the card that is never re-reviewed — with the workflow moves you make to prevent each.

### Worked instances (`worked-instances/`)

Pick **one representative system** from your scenario (e.g. for the bank scenario, the customer-facing generative chat; for the healthcare scenario, a clinical-decision-support pilot; for the B2B SaaS scenario, an HR-tech resume-screening capability shipped into an enterprise customer). Author all four cards for that one system:

- `worked-instances/model-card-<system>.md` — front-matter validating against `model-card.schema.json`, Markdown body per chapter 03's section list.
- `worked-instances/system-card-<system>.md`.
- `worked-instances/dataset-card-<one-of-the-datasets>.md`.
- `worked-instances/risk-card-<system>.md`.

Cross-referencing across the four is the point of the worked instance:

- The system card's `model_refs` cites the model card's `card_id`.
- The model card's `evaluation_dataset_refs` cites the dataset card's `dataset_registry_id`.
- The risk card's `system_id` and `model_card_refs` and `system_card_refs` cite the model and system cards.
- The risk card's `risk_register_refs` cites at least one mod-106 `RR-ID` (invent a plausible ID; state it is an example).

Each instance validates against its schema — the learner runs `ajv validate -s schemas/model-card.schema.json -d worked-instances/model-card-<system>.md` (extracting the front-matter first) or equivalent, and validation passes.

## Starter guidance

- **Do not conflate a model card with an AIA.** The AIA (mod-105 chapter 04) is regime-specific and decision-oriented — it recommends whether and how to deploy under a specific regulatory frame. The card is the enterprise's *authoritative disclosure* across regimes and is descriptive, not recommending. Cards and AIAs cross-reference; they do not embed each other.
- **Do not author schemas that require field content the analyst cannot know without the risk engineer or evaluation engineer.** That is a coordination failure, not a schema virtue. If the model card requires a `disaggregated_results` array and the analyst cannot produce disaggregated numbers without the evaluation engineer, the schema either references an evaluation-run ID (correct) or forces the analyst to duplicate the evaluation engineer's output (wrong). Preview of chapter 07 coordination.
- **Every tier's field-set inflation should be defensible.** For each field that becomes required at tier-3 or tier-4 but is optional at tier-2, state *why* — what claim does the higher tier make that the field substantiates? A tier overlay that inflates the field set without a stated rationale becomes documentation theatre.
- **The risk card is distinct from the risk register entry.** They cross-reference. The card is a disclosure; the register entry is a record. Do not duplicate risk-register fields into the card schema — you will drift.
- **Do not couple the Markdown render to the JSON schema.** Chapter 03's discipline: the front-matter is the machine-readable shape; the body is the human render. A schema that demands specific Markdown heading text is a schema that has conflated the two.
- **The primary literature is a shape reference, not a template.** Mitchell et al. fixes the shape of a model card; the enterprise's model card is a *disciplined derivative* of that shape sized for the enterprise's tier structure, substrate bindings, and jurisdictional-scope reality. Do not copy Mitchell et al.'s field list verbatim; extend it with substrate binding and role assignment.
- **Do not require the analyst to author natural-language fields the substrate could produce.** An evaluation summary field should reference an evaluation-run ID and the substrate's summary projection of it; requiring the analyst to type an evaluation summary by hand invites drift between the card and the run.
- **Do not fabricate frontier-lab system-card content.** Cite OpenAI, Anthropic, Google DeepMind system cards at title level — "system cards for major model releases" — do *not* invent specific model names, publication dates, or field-level claims from them. Mark uncertainty with `<!-- needs-research: ... -->`.

## Acceptance criteria

- [ ] Scenario stated at the top; one representative system named for the worked instances.
- [ ] All four card classes have JSON Schemas — `model-card.schema.json`, `system-card.schema.json`, `dataset-card.schema.json`, `risk-card.schema.json`.
- [ ] Each schema declares a JSON Schema draft version at `$schema` and passes lint (`ajv compile` or equivalent).
- [ ] Each worked instance validates against its schema (`ajv validate` or equivalent) — the learner has actually run the validator, not just eyeballed the file.
- [ ] Each schema traces its shape choices to primary sources by name in schema `description` or comments — Mitchell et al. for the model card, Gebru et al. for the dataset card, frontier-lab system cards (title level) for the system card, mod-106 for the risk card.
- [ ] Shared front-matter fields (`card_type`, `card_version`, `system_id`, `substrate_bindings`, `publication_scope`, `re_review_triggers`) are factored into a shared referenced schema, not duplicated across the four.
- [ ] Substrate linkage explicit: every card publication event is named against a chapter-02 family-3 event class (`card.draft.created`, `card.review.completed`, `card.signoff.completed`, `card.published`, `card.rereview.triggered`).
- [ ] Author / reviewer / signer roles are named per card class per section per tier.
- [ ] Publication-gate composition has named signers per tier and encodes the six chapter-03 checks operationally.
- [ ] Re-review triggers enumerated (material change, drift, incident, scheduled cadence, regulatory change) with the specific field / metric / class boundaries the enterprise uses.
- [ ] Deprecation procedure defined — `deprecated` flag, `deprecated_reason`, `deprecated_at`, consumer notification path, retention posture.
- [ ] Cross-referencing across the four worked-instance cards demonstrated — system card cites model card ID; model card cites dataset card ID; risk card cites system, model, and mod-106 RR-ID.
- [ ] OSCAL back-matter binding sketched — how the front-matter emits into an OSCAL `resource` shape (preview of chapter 06 and exercise-05).
- [ ] Every unverified citation (ATRS URL, HF Model Card metadata field list, frontier-lab system-card specifics) marked `<!-- needs-research: ... -->` — no invented dates, URLs, or field lists.

## Stretch goals

- **Publish schemas as an OSCAL component-definition profile** — preview of chapter 06 and exercise-05. Emit each card schema as an OSCAL component-definition `component` whose `implementation` block describes the schema's control-implementation posture; the SSP's `implemented-requirement` references the component-definition component.
- **Add jurisdiction-specific field overlays.** Extend the schemas with jurisdiction-scoped required-field overlays — ATRS-specific fields for UK public-sector cards; Colorado AI Act consumer-notice fields for Colorado-deployed cards; NYC LL144 bias-audit citation fields for AEDTs (automated employment decision tools) deployed in NYC. Model overlays as JSON Schema `if / then / else` conditional constraints keyed on `jurisdictions_in_scope`. <!-- needs-research: verify current Colorado AI Act consumer-notice requirement and NYC LL144 bias-audit citation requirement at rulemaking-current-version level -->
- **Demonstrate re-emission of a filed card at a past `snapshot_ts`.** Chapter 02's reproducibility discipline: given a `snapshot_ts` in the substrate, the card as-of-that-timestamp can be reconstructed from the substrate event history. Sketch the reconstruction query.
- **Author a machine-readable representation of the card-lifecycle state machine.** A Statechart, or a plain YAML state table naming the six lifecycle stages (draft / review / sign-off / gate / published / re-review), the transitions between them, the event class each transition emits, and the guards each transition requires (schema validation pass; substrate-binding hash re-verification; signature-block completeness). The state machine is the specification the workflow engine implements.
