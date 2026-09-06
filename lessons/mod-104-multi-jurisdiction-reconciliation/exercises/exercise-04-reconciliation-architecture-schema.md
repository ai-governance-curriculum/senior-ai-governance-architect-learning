# exercise-04: Reconciliation architecture schema

**Estimated effort:** 4 hours

## Objective

Author **the reconciliation-architecture schema** for the enterprise's obligation register — the concrete YAML shape for the obligation record, the applicability-filter extension, the evidence-contract per-obligation-rendering block, and the deprecation-path state machine — together with a *seed corpus* of ten to fifteen obligation records populated across three regimes so the schema is exercised end-to-end. Then run one *end-to-end evaluation trace* through the architecture: pick one AI system record, evaluate one control's applicability, produce one control's evidence-contract renderings, and walk one deprecation-path event through the state machine.

This exercise is the module's *load-bearing* deliverable — it is the artefact set the level-50 architect personally owns and hands to legal (for interpretation review), to the ai-risk-engineer (for the applicability-engine implementation in mod-111), to the ai-governance-analyst (for operational maintenance), and to internal audit (for testability review). Every downstream module in the track — mod-105 AIMS, mod-106 risk taxonomy, mod-107 assurance, mod-108 evidence architecture, mod-111 GRC toolchain — assumes this schema exists and is well-shaped.

## Prerequisites

- Chapter [`07-designing-the-reconciliation-architecture.md`](../07-designing-the-reconciliation-architecture.md) read and internalised — this exercise implements what the chapter designs.
- Chapters [`02-reading-the-eu-ai-act-as-architectural-input.md`](../02-reading-the-eu-ai-act-as-architectural-input.md) through [`06-the-international-patchwork.md`](../06-the-international-patchwork.md) read — you will populate obligation records against several of these.
- Exercises [`exercise-01-eu-ai-act-articles-to-controls-map.md`](exercise-01-eu-ai-act-articles-to-controls-map.md), [`exercise-02-us-federal-plus-state-crosswalk-drill.md`](exercise-02-us-federal-plus-state-crosswalk-drill.md), and [`exercise-03-international-patchwork-overlay-drill.md`](exercise-03-international-patchwork-overlay-drill.md) completed — the obligation decompositions you produced there feed this exercise.
- mod-102 chapter 01 (anatomy of a control-library entry) and chapter 04 (OSCAL projection) as reference — the schema must interoperate with the mod-102 control shape.
- YAML familiarity; a schema-validation approach (JSON Schema, Cue, or equivalent) is optional but useful for the stretch goals.

## Scenario

Continue with either the Northbrook Financial Services scenario from exercise-02 or the Meridian AI Assistants scenario from exercise-03 — pick the one where you produced the more substantial atomic-requirement decomposition. Note your choice at the top of the deliverable. The seed corpus draws from the obligations you already decomposed in the earlier exercises.

Assume the enterprise is running:

- The mod-102 control library (well-shaped, with the families named in the earlier exercises).
- A mod-105 AIMS with per-system records (you will invent one system record for the evaluation trace).
- A mod-111 GRC-for-AI toolchain with an applicability-engine that consumes this schema — you do not need to implement the engine, but the schema must be shaped so a reasonable engineer could.

## Deliverables

1. **`obligation-record-schema.yaml`** — the canonical schema for the obligation record.
2. **`applicability-filter-vocabulary.yaml`** — the controlled vocabulary the applicability filter uses (dimensions, allowed values, evaluation semantics).
3. **`evidence-contract-extension.yaml`** — the per-obligation-rendering extension to the mod-102 chapter 07 evidence-contract shape.
4. **`deprecation-path-state-machine.md`** — the state machine documentation (states, transitions, invariants, migration and superseded-evidence windows).
5. **`obligation-register-seed.yaml`** — ten to fifteen populated obligation records exercising the schema across at least three regimes and every enumerated field.
6. **`end-to-end-trace.md`** — a single worked trace through the architecture (system record → applicability evaluation → evidence-contract rendering → deprecation event).
7. **`schema-review-brief.md`** — a one-page brief.

## Requirements

### `obligation-record-schema.yaml`

Author the canonical YAML shape for one obligation record. Extend the chapter-07 sketch into a *complete* schema: every field named, every enumerated value listed, every optional-vs-required distinction stated, every foreign-key relationship named.

At minimum the schema must carry:

- `id` (format convention: `OBL-<regime-short>-<citation-short>-<slug>`)
- `regime` (nested: `identifier`, `version`, `publisher`, `citation_url`)
- `citation` (nested: `article` / `section` / `paragraph` / `subparagraph` as appropriate)
- `title` (short)
- `statement` (the atomic-requirement restatement — one to three sentences)
- `addressee.role` (enumerated: union across regimes — provider, deployer, developer, importer, distributor, authorised_representative, user, controller, processor, data_fiduciary, operator, employer, GPAI_provider, GPAI_provider_with_systemic_risk, ...) and `addressee.role_condition` (any additional qualifier)
- `trigger` (nested: `system_classification` list; `use_case` list; `data_class` list; `place_of_user` list; `scale_threshold` object with `metric` + `value`; `event` list — the six trigger dimensions from chapter 1 axis 2)
- `demand` (nested: `category` enumerated — design_state / process / document / test / disclosure / notification / filing; `artefact_shapes` list; `frequency` where applicable; `window` where applicable — e.g. Article 73 15-day serious-incident window)
- `consequence` (nested: `type` enumerated — administrative_penalty / private_right_of_action / supervisory_action / procurement_disqualification / criminal / reputational / contractual; `ceiling_reference` where a specific penalty band applies)
- `territorial_scope` (nested: the *evaluation function description* — what facts about the system or the enterprise trigger the obligation's territorial reach; where legal owns the interpretation)
- `filing_entity` (the legal entity type expected to file / attest, where different from the parent)
- `language_of_record` (list of languages the evidence artefact must be produced in for this obligation)
- `effective_date`
- `supersedes` (list of obligation ids)
- `superseded_by` (nested: `obligation_id`, `migration_deadline`, `superseded_evidence_valid_until`)
- `status` (enumerated: watched / proposed / active / superseded / withdrawn)
- `related_obligations` (list of obligation ids for cross-regime overlap)
- `authoritative_interpretations` (list of `source` + `identifier`)
- `watch_list_events` (list of `date` + `event_description` — populated for `watched` and `proposed` records tracking legislative progress)
- `owner` (the role responsible for maintaining this record — usually `level_15_ai_governance_analyst` or `legal`)

For each field, state whether it is *required* or *optional*, and — for enumerated fields — enumerate the current allowed values. Where the schema is extensible (regime identifiers, addressee roles), state the extension process (who adds a value; where the enumeration is stored; how existing records are migrated).

### `applicability-filter-vocabulary.yaml`

Author the controlled vocabulary the applicability filter uses across all controls. This is the boolean-expression language the chapter-07 applicability-engine evaluates. Include:

- **Dimensions** — the named attributes the filter can reference (system_kind, system_tier, use_case, data_class, jurisdiction, addressee_role, variant, filing_entity, effective_date_reached, contract_flow_down, evidence_mode from chapter 8, plus any dimensions your Meridian or Northbrook scenario added).
- **Allowed values per dimension** — enumerated where practical; free-form with format convention where not (`us_state.<statename>`; ISO 3166 codes; enterprise-specific system-tier scale).
- **Evaluation functions** — `effective_date_reached.for_obligation(<id>)`; `addressee_role.for_jurisdiction(<jurisdiction>)`; `variant() == <variant>`; the operator set (AND, OR, NOT, `in`, `==`); the closed-world convention (a dimension not named in an expression is not evaluated).
- **Reference resolution** — how an expression's obligation-id reference is resolved against the register (must be `status: active` at evaluation time; expressions referencing `status: proposed` obligations are evaluated but flagged; expressions referencing `status: superseded` obligations follow the deprecation-path rendering).
- **Compilation guarantees** — what the toolchain must validate before an expression is deployed: known-dimension check, known-value check, obligation-id resolution, unreachable-branch detection.

### `evidence-contract-extension.yaml`

Extend the mod-102 chapter 07 evidence-contract shape with the per-obligation-rendering block from chapter 07 of this module. Author a complete schema for:

- `base_artefacts` (list of artefact-class references; the mod-102 shape).
- `per_obligation_renderings` (map from obligation-id to a rendering object; each rendering carries `required_form`, `language`, `retention`, `recipient`, `format_specification`, `attestation_shape` where applicable).
- `sampling_rule` and `freshness_rule` (mod-102 shape; unchanged).
- `evidence_mode` (chapter 8 addition: `direct_evidence_route` / `standards_route`, per obligation, with a default and a per-rendering override).
- `provenance` (which artefact-producing pipeline in the mod-108 evidence architecture produces this class; where the produced artefacts are stored; how they are retrievable at audit time).

State the invariants: no rendering may reference an obligation not in the control's crosswalk; every attached obligation must have a rendering (a rendering explicitly `null` is allowed and means the obligation is satisfied by evidence produced against a related obligation).

### `deprecation-path-state-machine.md`

Document the state machine chapter 7 sketches, with:

- The state diagram (watched / proposed / active / superseded / withdrawn) with allowed transitions and the events that trigger each.
- The invariants — a record in `superseded` state must carry `superseded_by` with a non-null `obligation_id`; a record in `withdrawn` state must carry a stated reason; a record in `proposed` state must carry an `effective_date` estimate.
- The three change kinds — amendment in place, supersession, withdrawal — with a worked example of each. Use the OMB M-24-10 → M-25-21 supersession as the supersession example; use an EU AI Act implementing act as the amendment example; use the EO 14110 revocation as the withdrawal example.
- The *migration window* — the meaning of `migration_deadline` (when controls must have migrated their crosswalk to the new record) vs `superseded_evidence_valid_until` (when evidence produced against the old obligation ceases to be audit-defensible for the pre-transition period). Include a short worked timeline.
- The *watch-list-to-proposed* trigger — the specific event (bill signed; regulation published in OJEU; effective-date decree issued) that moves a record from `watched` to `proposed`. Reference the chapter-6 international watch-list mechanism and the chapter-5 US state watch-list mechanism.
- The *review cadence* — how often each state's records are reviewed (quarterly for `watched`, on effective-date reach for `proposed`, annually plus on regulatory-change signal for `active`, event-driven for supersessions and withdrawals).

### `obligation-register-seed.yaml`

Populate ten to fifteen obligation records against the schema. Draw from your earlier exercises. Cover, at minimum:

- **Three EU AI Act records** — one from Article 9-15 provider block, one from Article 27 (deployer FRIA), one from Article 73 (serious-incident reporting).
- **Two US-federal records** — one from OMB M-25-21 and one from OMB M-25-22, chosen from the Northbrook contract-flow-down exercise-02 or a Meridian federal-customer scenario.
- **Two US-state records** — one from Colorado SB24-205 and one from NYC LL 144 or California SB 942.
- **Three international records** — one from China Interim Measures, one from Korea AI Basic Act, one from UK ATRS (or the equivalent from the scenario you chose).
- **One superseded record** — OMB M-24-10 with the full `superseded_by` block pointing at the M-25-21 record, with realistic `migration_deadline` and `superseded_evidence_valid_until` values.
- **One watched record** — Brazil PL 2338 (or a Canadian AIDA reintroduction, or an Australian Mandatory Guardrails bill) with the watch-list-events history and the trigger-to-active checklist.

Each record must be *fully populated* — no `TBD`s, no bare fields. Where a fact cannot be verified against the primary source at authoring time, use `<!-- needs-research: ... -->` inside the record rather than inventing a paragraph number or effective date.

### `end-to-end-trace.md`

Produce a single worked trace demonstrating the architecture:

1. **Invent one AIMS system record** — pick one AI system from your scenario (Northbrook Career Match, or Meridian Advocate's UK-public-sector variant, or an equivalent). Populate `system_kind`, `system_tier`, `use_case`, `data_class`, `jurisdiction`s where deployed, `addressee_role` per jurisdiction, `variant`, `filing_entity`, `contract_flow_down` if applicable.
2. **Pick one control** from the enterprise library (e.g. the multi-jurisdictional human-oversight control from chapter 07 with its cross-regime applicability filter). Walk the applicability-filter evaluation against the system record: which branches evaluate true, which evaluate false, which obligations attach.
3. **Produce the evidence-contract rendering set** for the attached obligations. State which `base_artefacts` are produced, then walk the `per_obligation_renderings` for each attached obligation, showing the language / recipient / retention differences.
4. **Walk one deprecation event** — imagine the OMB M-25-21 memorandum is superseded next year by an M-26-XX. Show what changes in the obligation register (new record created, old record moved to `superseded`, `migration_deadline` and `superseded_evidence_valid_until` set), what changes in the applicability filter (references migrated by the deadline), what changes in the evidence contract (renderings updated to new memorandum reference), and what does *not* change (the underlying control statement; the base artefacts; the system record).

The trace should read as a single 500-1000 word narrative walking a reader through the architecture end to end. It is the artefact you would use in an internal-audit walk-through.

### `schema-review-brief.md`

One page. Must contain:

- **The three schema design decisions you are most confident in and why** — brief justifications so legal, engineering, and audit understand your rationale before they push back.
- **The three schema design decisions you are least confident in** — the open questions. Common candidates: exactly how much of the territorial-scope evaluation function belongs on the obligation record vs delegated to legal; whether variant is a first-class dimension or a system-record attribute; how the schema handles a *conditional* supersession (chapter 8 standards route).
- **The extensibility posture** — how the schema absorbs a new regime, a new addressee role, a new demand category without a breaking change. State whether the schema uses closed enums (with a formal extension process) or open enums (with a validation regime).
- **The interfaces to other modules** — the two-sentence handoff to mod-105 AIMS (system record shape), mod-108 evidence architecture (artefact-producing pipelines), mod-111 GRC toolchain (applicability engine, register operational tooling), and mod-107 assurance (testability of the schema).

## Starter guidance

- **Do not start from a blank YAML file.** Start from the chapter-07 sketches, then fill fields until every field has an enumerated value set or a format convention. Every `TBD` in your schema is a decision the reader will have to make later.
- **The addressee enumeration is a union, not an intersection.** The set of addressee roles must include the roles from every regime you cover, even if a single obligation uses only one — otherwise the schema fails on the next regime that adds a role (Korean AI Basic Act's domestic representative is a good stress test).
- **The territorial-scope field is not the same as `jurisdiction`.** `jurisdiction` on the applicability filter is *where the enterprise says the system is deployed / used*; `territorial_scope` on the obligation record is *what facts about the system trigger the obligation's reach*. GDPR reaches Meridian's US-hosted API when Meridian offers services to EU data subjects even without an EU deployment; encode that.
- **Deprecation is a first-class field, not a bolt-on.** Every seed record in your corpus must carry `supersedes` and `superseded_by` fields, even if both are `null`. If you find yourself adding these fields after the fact for one record, add them everywhere.
- **The end-to-end trace is the acceptance test.** If you cannot walk a system through the architecture in the trace, the schema is under-specified. Do the trace early — halfway through the schema, not after — and let the trace surface schema gaps.
- **Use `<!-- needs-research: ... -->` freely.** The seed corpus must be primary-source-verifiable, not memorised. Where you cannot verify a paragraph number, an effective date, or a memorandum title, mark it rather than guessing.

## Acceptance criteria

- [ ] The obligation-record schema names every required and optional field, enumerates every closed enum, states the format convention for every open field, and names the extension process for extensible enumerations.
- [ ] The applicability-filter vocabulary is *compilable* — a reasonable engineer can implement the evaluator without further specification.
- [ ] The evidence-contract extension composes cleanly with mod-102 chapter 07 (no field collisions, no ambiguity about which module owns which field).
- [ ] The deprecation-path state machine document names every state, every transition, every invariant, and includes a worked example of each change kind (amendment, supersession, withdrawal).
- [ ] The seed corpus carries at least ten records, covers at least three regimes, includes at least one superseded and one watched record, and populates *every* schema field on every record (no bare or `TBD` fields).
- [ ] The end-to-end trace walks system → applicability → evidence → deprecation in a single readable narrative, and surfaces at least one schema decision the reader would not have seen from the schema alone.
- [ ] The schema-review brief states three high-confidence and three low-confidence design decisions and names the interfaces to at least four downstream modules.
- [ ] Every regulatory fact in the seed corpus is either verifiable against the primary source cited in `../resources.md` or marked `<!-- needs-research: ... -->`.
- [ ] Every YAML file validates as YAML (no syntax errors); files are self-consistent (an id referenced in one file is defined in another).

## Stretch goals

- **Author a JSON Schema (or Cue schema) for the obligation record** — actual machine-validatable schema, not just documentation. Wire the seed corpus through the validator and report violations.
- **Sketch the applicability-engine algorithm** — pseudocode for how the mod-111 GRC toolchain evaluates a control's applicability filter against a system record, including the closed-world convention, the obligation-id resolution, and the effective-date evaluation. Half a page.
- **Add a *conflict-detection* pass to the schema** — the algorithm that would flag pairs of obligations attached to the same control whose renderings are incompatible (e.g. an obligation requiring public disclosure and another requiring confidentiality). One paragraph plus a worked example from your seed corpus.
- **Produce the *migration playbook* for the M-24-10 → M-25-21 supersession** as a step-by-step procedure the ai-governance-analyst would execute: which records to create, which to update, which controls to inspect for crosswalk migration, which evidence artefacts to re-render. One page.
- **Add an *audit-testability* section to the schema review brief** — how internal audit tests the register itself: sample records for currency, sample controls for correct obligation attachment, sample evidence for correct rendering per obligation. One paragraph.
