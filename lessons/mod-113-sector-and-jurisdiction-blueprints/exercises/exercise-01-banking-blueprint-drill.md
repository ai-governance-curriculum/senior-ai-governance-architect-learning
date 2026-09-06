# exercise-01: Banking Blueprint Drill

**Estimated effort:** 3 hours

## Objective

Fill in chapter 01's sector adaptation record schematic for a US G-SIB with a Canadian FI subsidiary and an EMEA subsidiary, and author the four derived artefacts the record composes against — a unified model-inventory schema, a joint validation-report schema, an applicability-filter values register with a parsimony defence, and a reserved-matters overlay for the mod-112 council. The product is a defensible instantiation the level-50 architect can carry into a supervisory dialogue with a Federal Reserve or OCC examination team and, without editing, hand to the head of AI governance as the coaching artefact for the sector programme.

The correctness spine is chapter 01's six invariants (I1 single-source catalog, I2 sector applicability values on the profile, I3 evidence extends per obligation, I4 policy-as-code parameterisation, I5 single AIMS with scope addendum, I6 risk taxonomy augmented not replaced) and its two failure modes (the fork temptation; applicability-filter sprawl). Every design choice in every artefact must be pinnable to an invariant as enforcer or to a failure mode as defence. Do not reproduce chapter 02's worked record verbatim; the exercise is where the learner earns the record shape by writing it against the specified enterprise.

## Prerequisites

- Chapter [`01-the-sector-adaptation-methodology.md`](../01-the-sector-adaptation-methodology.md) read once with the six invariants and the two failure modes marked, and the sector-adaptation-record schematic annotated so the learner can populate it without re-reading.
- Chapter [`02-us-g-sib-banking-blueprint.md`](../02-us-g-sib-banking-blueprint.md) read once with the SR 11-7 composition-anchor argument marked, the SR 23-4 third-party overlay marked, and the OSFI E-23 / Colorado SB 24-205 / EU AI Act Annex III applicability discussions marked.
- Mod-102 chapter 04 (profile shape) and chapter 06 (catalog authoring lifecycle) skimmed — every catalog addition the record proposes traverses the chapter-06 lifecycle, not a sector-branded fork.
- Mod-105 Clause 4.3 scope and Clause 5.3 roles discussion skimmed — the AIMS scope addendum inherits the enterprise scope statement's shape, and the chief model risk officer's placement in the role list is a Clause 5.3 decision the record surfaces.
- Mod-107 chapter on three-lines-of-defence and the pre-deployment gate skimmed — the SR 11-7 pre-implementation validation gate composes with the mod-107 gate; the record names the composition rather than describing a parallel gate.
- Mod-108 chapters on evidence contracts and the evidence-schema registry skimmed — the joint validation-report schema and joint ongoing-monitoring schema are mod-108 registry entries, not bespoke sector artefacts.
- Mod-109 chapter on the third-party lifecycle skimmed — the SR 23-4 overlay attaches to the mod-109 provider-facing controls as evidence-contract line items, not as a parallel third-party programme.
- Mod-112 chapter 01 (AI governance council charter) skimmed — the reserved-matters overlay is authored as an *addition* to the mod-112 baseline register produced by that chapter's exercise-01, not as a replacement.

## Scenario

You are the level-50 governance architect at **Cascadia National Financial Group**, a hypothetical US G-SIB with the following structure and AI footprint. Treat the entities and books as real for the exercise; do not swap them for a different enterprise.

- **US national bank charter** (OCC-supervised) — Cascadia National Bank, N.A., the primary depository entity, running consumer credit, commercial credit, and treasury.
- **US Federal Reserve-supervised bank holding company** — Cascadia National Financial Group, Inc., the parent BHC in scope for consolidated supervision.
- **Canadian FI subsidiary** (OSFI-supervised) — Cascadia Canada Bank, a federally regulated deposit-taking institution operating retail and commercial banking in Ontario, Alberta, and British Columbia.
- **EMEA subsidiary** — Cascadia Europe Bank, S.A., an EU-established credit institution offering consumer credit to natural persons across France, Germany, and Ireland, and within EU AI Act territorial scope.
- **Colorado consumer-lending book** — Cascadia National Bank operates a Colorado-domiciled consumer-lending desk (personal loans and credit cards) whose adverse-decision volume for CO-domiciled natural persons is material.

The AI systems in scope (each system runs across a defined jurisdiction footprint stated in the model inventory):

- **Retail credit underwriting** — an ML classifier scoring personal-loan and credit-card applications. Runs in the US (CO and other states), in the EMEA subsidiary against EU natural-person applicants, and (as a separate deployment) in the Canadian subsidiary.
- **Fraud detection** — an ML anomaly detector on card-authorisation and account-takeover signals. Runs across all in-scope entities; carve-out from EU AI Act high-risk classification via the Annex III fraud exception is asserted and must be defended in evidence.
- **AML transaction monitoring** — a hybrid rules-plus-ML system feeding suspicious-activity investigators. Runs across all in-scope entities.
- **Generative-AI wealth-management advisor** — a foundation-model-based conversational agent used inside the US private-bank wealth-advisory desk to draft client communications; not a decisioning system for lending but subject to supervisory expectations on model use.
- **Foundation-model-based document triage** — a foundation-model-based classifier routing inbound customer correspondence into servicing queues; deployed across all in-scope entities.

Assume the mod-102 enterprise reference catalog, the mod-104 reconciled obligation register, the mod-105 AIMS scope statement, the mod-106 taxonomy, and the mod-112 council charter exist as v1.0 artefacts. The exercise produces the sector-specific deltas — nothing more, nothing less.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`sar-us-gsib-v1.0.yaml`** — the sector adaptation record populated to chapter 01's schematic, sector `financial-services / banking / US-G-SIB`, in-scope entities as scenario, all record blocks populated with cross-references to the four derived artefacts below.
2. **`unified-model-inventory-schema.md`** — the schema for the single enterprise model inventory that carries both SR 11-7 attributes and AI-specific extended attributes on one record per model, with a stated system-of-record and a stated authoritative-owner packet.
3. **`validation-report-schema.md`** — the joint schema that renders one artefact per validation event satisfying SR 11-7 §V-A validation-report expectations and EU AI Act Article 11 technical-file expectations simultaneously, so a validated credit-scoring model produces one report — not two.
4. **`applicability-filter-values.md`** — the applicability-filter dimension and value extensions the sector profile carries (jurisdiction values, line-of-business values, and any other dimension the sector requires), with a per-value rationale and a parsimony defence against chapter 01's failure mode 2.
5. **`reserved-matters-overlay.md`** — the mod-112 council reserved-matters register additions the G-SIB brings, expressed as a delta on the mod-112 chapter 01 baseline (do not restate the baseline), with a per-matter escalation trigger, originating operational forum, and Canadian-board / EU-supervisor reporting hook where relevant.

## Requirements

### `sar-us-gsib-v1.0.yaml`

- **Identifier and ratification block.** `id: SAR-US-GSIB-v1.0`, `ratified_by: ai-governance-council (mod-112)` with a `ratification_minute: <placeholder>` field. The `reference_architecture_version` block names the mod-102 catalog version, the enterprise reference profile version, the mod-105 scope statement version, the mod-106 taxonomy version, and the mod-104 register version the record composes against (versions may be `TBD` placeholders).
- **Sector and scope narrative.** `sector.identifier: financial-services / banking / US-G-SIB`; `scope_narrative` naming the five in-scope entities, the geographical footprint, and the five AI system families in the scenario.
- **Anchor regulations block.** `horizontal` naming EU AI Act (for the EMEA subsidiary), NIST AI RMF (as risk-management-framework acceptable posture for CO SB 24-205). `sector_specific` naming SR 11-7 / OCC 2011-12, SR 23-4 / OCC 2023-17 / FDIC FIL-29-2023, OSFI Guideline E-23, EU AI Act Annex III paragraph 5(b), and Colorado SB 24-205, each with a `jurisdiction`, `supervisor`, `instrument_type`, and `obligation_register_ids` list dereferencing into the mod-104 register (ids may be `OBL-<TBD>` placeholders).
- **Profile block.** `profile.id: PROFILE-US-GSIB-v1.0`, `derives_from: enterprise-reference-profile v<TBD>`, `delta_summary` narrating (in three or four sentences) which reference-catalog controls are additionally selected, which reference parameters are tuned, and which controls are made mandatory that were optional. Reference `applicability-filter-values.md` by version — do not restate the filter here.
- **Evidence contract extensions block.** At least one extension per anchor obligation (SR 11-7 model-development documentation on the mod-102 model-lifecycle-documentation control; SR 11-7 ongoing-monitoring on the model-performance-monitoring control; SR 23-4 third-party diligence on the mod-109 provider-diligence control; OSFI E-23 board-level oversight evidence on the AIMS-management-review control; EU AI Act Article 11 technical-file evidence on the model-technical-documentation control; CO SB 24-205 consumer-adverse-decision-disclosure on the ECOA / adverse-action control). Every extension keyed by `obligation_id` from the mod-104 register — never by sector name (I3 defence).
- **Policy-as-code guards block.** At least three parameterisations of existing mod-103 templates — a jurisdiction-egress guard for EU natural-person data crossing the boundary; a HIPAA-adjacent BAA guard is not required (this is banking) but a Reg-P consumer-data egress guard is; a provider-classification guard for foundation-model providers used inside the wealth-advisor system. Each guard names the mod-103 template referenced, the parameterisation values, and the mod-103 chapter-05 enforcement point.
- **AIMS scope addendum block.** `inclusions` naming the additional systems (the four Canadian-subsidiary deployments; the EMEA subsidiary deployments) that the enterprise scope did not already reach explicitly; `exclusions` naming, with defence, systems the sector excludes (e.g., rule-based systems below the AIMS scope threshold); `interfaces.to_enterprise_scope` stating that the addendum is additive to the enterprise scope statement — one AIMS, one certification (I5 defence).
- **Risk taxonomy augmentation block.** At least four added categories, each with a `parent_reference_category` mapping (specialisation, not peer): discriminatory-lending-outcomes (specialises `fairness`); CECL-model-error (specialises `operational` — financial-reporting integrity); stress-test-model-error (specialises `operational`); CO-AI-Act-consumer-notice-failure (specialises `compliance`). Two added appetite triggers with declaring seat (the CRO, per SR 11-7's programme-pillar accountability).
- **Additional roles block.** Model-validation-lead (banking) at level 45–50, owner-packet route into the mod-105 Clause 5.3 role register; Canadian-subsidiary-MRM-lead at level 40–45, owner-packet route into the CA subsidiary's local role register with an enterprise reporting hook.
- **Reserved matters additions block.** Reference `reserved-matters-overlay.md` by version — do not restate the register here.
- **Deprecation path notes block.** At minimum, the pre-2023 Fed / OCC / FDIC third-party guidance (Fed SR 13-19, OCC Bulletin 2013-29, FDIC FIL-44-2008) superseded by SR 23-4 / OCC 2023-17 / FDIC FIL-29-2023 (June 2023); state the evidence-transition posture. Do not fabricate migration deadlines — mark them `<!-- needs-research: ... -->` if uncertain.
- **Invariants pinning block.** Every one of the six chapter-01 invariants (I1–I6) named with a one-sentence `test:` clause stating how Cascadia verifies it — the tests must be executable on the enterprise's actual artefacts (a sample-and-check statement, not a design assertion).

### `unified-model-inventory-schema.md`

- **Statement of the composition move.** One paragraph stating that the SR 11-7 model inventory and the mod-108 AI system inventory are the same inventory (chapter 02's core architectural move) and naming the failure mode this defends against (the two-inventory drift that examiners find within one cycle).
- **System-of-record.** A single named enterprise system (the mod-111 GRC-for-AI platform, or an existing MRM inventory tool the enterprise elects to extend). State the authoritative-owner packet (level 60 head of AI governance, jointly with the chief model risk officer).
- **Attribute families.** Author the schema in four groups with every attribute typed and marked `MRM-source | AI-source | joint`:
  - **Identity and lineage** — model id, name, version, business owner, model developer, model validator, model use.
  - **SR 11-7 attributes** — materiality tier, validation status, last validation date, next revalidation date, findings status, user population, model-tier-driven validation-depth expectation.
  - **AI-specific extended attributes** — model class, training-data lineage reference, provider (for foundation-model-based systems), fine-tuning artefact reference, evaluation-protocol reference, mod-106 risk taxonomy classification, mod-107 assurance-tier, jurisdiction-applicability set (US-Fed-SR-11-7, US-OCC-2011-12, CA-OSFI-E-23, EU-AI-Act, US-CO-AI-Act as scenario-relevant values).
  - **Monitoring and outcomes** — ongoing-monitoring cadence, outcomes-analysis window, monitoring-dashboard reference, PMS reporting hook (mod-110 chapter 03) where the model is in EU AI Act scope.
- **Materiality-tier reconciliation.** State how the SR 11-7 materiality tier and the mod-107 assurance tier reconcile — either by defining the two as the same field with a lookup, or by defining a reconciliation table and stating who owns keeping it current. Do not leave the reconciliation implicit; the failure mode defended against is that the two tierings drift and produce contradictory validation-depth expectations.
- **Governance hooks.** Named field owners (developer for lineage, validator for validation attributes, mod-107 assurance-owner for assurance-tier, mod-110 PMS-owner for the reporting hook). One-sentence completeness-and-currency SLO with a sampling method internal audit can execute.
- **Invariant pinning.** Explicit footnote citing I1 (single library implies a single inventory keyed off it) and I3 (extensions keyed by obligation not sector), plus the chapter-02 failure-mode-1 defence (no parallel MRM / AI inventory).

### `validation-report-schema.md`

- **Statement of the composition move.** One paragraph stating that a validated model produces one report satisfying both SR 11-7 §V-A validation-report expectations and EU AI Act Article 11 technical-file expectations. Cite chapter 02's failure-mode-1 argument (parallel evidence trails is the two-programme failure mode multiplied per artefact family).
- **Required sections.** Author the report structure with every section typed by source frame:
  - **Model description** (SR 11-7 + Article 11 §1) — intended purpose, methodology, inputs, outputs, integration.
  - **Data governance description** (Article 10; SR 11-7 §V-A data-quality expectation) — data sources, quality, lineage, preprocessing, splits.
  - **Conceptual soundness evaluation** (SR 11-7 §V-A) — assumptions, methodology suitability, limitations.
  - **Technical documentation** (Article 11 §2–§9) — architecture, training methodology, evaluation results (metrics against declared performance claim), robustness / cybersecurity / accuracy assertions with evidence.
  - **Human oversight design** (Article 14) — where and how a human overrides or halts the system; oversight competencies expected.
  - **Ongoing monitoring design** (SR 11-7 §V-B; Article 72) — metrics, thresholds, cadence, revalidation triggers.
  - **Outcomes analysis design** (SR 11-7 §V-B) — window, comparison, patterns implying revalidation.
  - **Findings and restrictions on use** (SR 11-7 §V-A) — validator findings, materiality classification, restrictions on model use with expiry.
  - **Validator sign-off** — named individual, date, competency register cross-reference.
- **Crosswalk table.** A per-section table listing SR 11-7 clause and EU AI Act article satisfied. Where a section satisfies only one frame, state it explicitly — the reader must be able to see coverage at a glance.
- **Schema registry entry.** State the mod-108 schema registry id (e.g., `EVID-SCHEMA-VAL-REPORT-BANK-v1.0`), the schema owner (level 50 architect, jointly with head of validation), and the retention posture (SR 11-7's implied retention plus EU AI Act Article 18 retention where longer).
- **What this schema does not cover.** State explicitly that outcomes-analysis reports and ongoing-monitoring records are separate artefacts with their own joint schemas — the validation-report schema is one artefact family; the two-frames-into-one-artefact discipline is applied per family, not by concatenating everything into one document.
- **Invariant pinning.** Footnote citing I3 (evidence extends per obligation, not per sector — the report is not a sector-forked artefact but a mod-108 schema-registry entry with obligation-keyed content) and the chapter-02 failure-mode-1 defence.

### `applicability-filter-values.md`

- **Preamble.** State the reference-profile filter dimension budget (per mod-102 chapter 04) and the reference-profile per-dimension value budget. If the exact numbers are not known from the reference architecture, mark them `<!-- needs-research: mod-102 ch 04 reference-profile filter budgets -->` rather than guessing.
- **Added values per existing dimension.** For each existing reference dimension the sector extends, list the added values with a per-value rationale:
  - `applicability.jurisdictions` — values `US-Fed-SR-11-7`, `US-OCC-2011-12`, `CA-OSFI-E-23`, `EU-AI-Act` (subdivided by Annex III paragraph where relevant), `US-CO-AI-Act`. Rationale per value states which anchor regulation drives inclusion and which controls in the profile are gated by the value.
  - `applicability.line_of_business` — values `consumer-credit`, `commercial-credit`, `market-risk`, `operational-risk`, `AML-fraud`, `capital-and-regulatory-reporting`, `wealth-advisory`, `card-authorisation`. Rationale per value states the AI system family in the scenario that the value keys.
- **New dimensions (if any).** If a genuinely new dimension is required (e.g., `supervisory-authority-scope` if it cannot be expressed as a compound of jurisdiction and line-of-business), state the dimension, defend it against the chapter-01 failure-mode-2 test (can the value be expressed as a compound of existing dimensions?), and name the head-of-AI-governance approval required. If no new dimension is required, state so explicitly with the reasoning — parsimony is a positive design choice, not a default.
- **Parsimony review.** A short section (three or four paragraphs) authored *as if* addressing the head of AI governance at a filter-parsimony review. Show the total dimension count after the extension and the total value count per dimension. Argue why each added value cannot be expressed as a compound of existing dimensions. Name at least one candidate value the architect considered and *rejected* on parsimony grounds — the review is not credible if every proposed value is accepted.
- **Filter compilability check.** State the test: an analyst is given a novel workload (say, a fraud-detection model deployed to the Canadian subsidiary that also flags cross-border US-Fed activity) and must compile the applicable control set from the filter alone. State whether the current filter can be compiled by a level-15 analyst in under 15 minutes, or whether the filter has already crossed into the sprawl failure mode.
- **Invariant pinning.** Footnote citing I2 (sector applicability values live on the profile) and the chapter-01 failure-mode-2 defence explicitly.

### `reserved-matters-overlay.md`

- **Preamble.** State that this file is a *delta* on the mod-112 chapter 01 exercise-01 reserved-matters register. Do not restate the baseline register — cross-reference it by version and name only the additions.
- **Additions.** One row per matter (at minimum the six chapter-02 overlay items plus one or two additional scenario-specific matters):
  - **SR 11-7 material-findings acceptance** — trigger, originating operational forum (the model-validation function), pack owner, cross-reference to the mod-107 pre-deployment gate escalation path.
  - **OSFI supervisory-letter response** — trigger, originating forum (the Canadian subsidiary's compliance function), pack owner, reporting hook back to the Canadian-entity board.
  - **Board-level material third-party relationships (SR 23-4)** — trigger, originating forum (mod-109 third-party programme lead), pack owner, downstream reporting hook to the board's risk committee.
  - **EU AI Act Article 73 serious-incident disposition** — trigger, originating forum (mod-110 PMS function), pack owner, reporting-obligation window.
  - **Colorado AI Act AG-notification disposition** — trigger, originating forum (mod-110 PMS function jointly with GC), pack owner, notification-window and AG-notification content.
  - **MRM policy amendments (parent + CA subsidiary)** — trigger, originating forum (chief model risk officer for parent; CA subsidiary MRM lead for subsidiary), pack owner, ratification path with the Canadian-board reporting hook.
- **Scenario-specific additions.** At least one addition specific to the Cascadia scenario — e.g., a wealth-advisory-generative-AI incident-disposition matter, or a foundation-model-provider material-behaviour-change matter with the mod-109 third-party programme lead as originator.
- **Cross-reference discipline.** Every row references (a) the mod-104 obligation the matter escalates against, (b) the mod-102 control family whose failure or dispute triggers it, and (c) the mod-108 evidence family the pack composes from.
- **Invariant and failure-mode pinning.** Footnote stating that every added matter defends the mod-112 chapter-01 invariants (single decision forum for enterprise-material AI decisions) and defends the chapter-02 failure-mode-1 case (a supervisory dispute is not escalated into a sector-forked forum but into the single enterprise council).

## Starter guidance

Start with the reconciled anchor obligations in chapter 02. The SR 11-7 pillars, the SR 23-4 lifecycle stages, the OSFI E-23 board-oversight expectations, the EU AI Act Annex III + Chapter III article set, and the Colorado SB 24-205 duties give the exercise its concrete content; every derived artefact traces to one or more of these anchors. If the anchor is unclear, the derived artefact will be unclear and the failure-mode defence will collapse.

The most consequential derived artefact is `validation-report-schema.md`. Get it right and the enterprise runs one artefact family across SR 11-7 and Article 11; get it wrong and the two-evidence-trails failure mode ships on day one. Draft the required-sections list first, then the crosswalk table, and check that every SR 11-7 §V-A expectation and every Article 11 sub-clause is either present or explicitly deferred to a separate schema. A section satisfying only one frame is acceptable; a section duplicating content already in another section is not.

The second-most-consequential artefact is `applicability-filter-values.md`. The failure mode 2 defence — the filter-parsimony review — is where the architect earns the invariants. Draft the added values, then read the list adversarially: for each value, ask whether it could be expressed as a compound of existing dimensions. If the answer is yes, delete the value and rewrite the affected controls to key off the compound. If the answer is no, defend the value in prose the head of AI governance can read at the review. The rejected-value example is not decoration; without it the review reads as rubber-stamping.

The `sar-us-gsib-v1.0.yaml` record is the joining artefact — it references and is referenced by the other four. Draft the record's structural blocks (identifier, sector, anchor regulations, profile, invariants) first; draft the cross-referenced blocks (applicability filter, reserved matters) after the derived artefacts exist so that the cross-references resolve. Do not backfill the derived artefacts to whatever the record ends up saying — the derived artefacts are load-bearing and their content constrains the record, not the other way round.

The unified inventory schema and the reserved-matters overlay are the two artefacts the examiner-facing supervisory dialogue lands on. A Fed or OCC examiner reads the inventory and asks how it satisfies SR 11-7's completeness-and-currency expectation; a Canadian OSFI examiner reads the reserved-matters overlay and asks how the Canadian-board reporting hook composes. Author both as if a supervisor is reading them in draft — that is close to the actual audience.

Where a specific supervisory instrument, article number, effective date, or URL cannot be verified from the substrate chapters or established knowledge, write `<!-- needs-research: ... -->` inline. Chapter 02 already carries several `needs-research` markers (the OSFI E-23 revision date, the SR 23-4 exact SR number, the CO SB 24-205 amendment posture, the EU AI Act Annex III paragraph and fraud carve-out language); the exercise inherits those markers and adds any additional ones the derived artefacts surface. Guessing an effective date or an SR number is worse than marking it — a level-50 architect who writes an unverified date into a supervisory-facing artefact is not a level-50 architect.

## Acceptance criteria

- [ ] `sar-us-gsib-v1.0.yaml` populates every block of chapter 01's schematic (identifier, ratification, reference-architecture version, sector, anchor regulations, profile, evidence-contract extensions, policy-as-code guards, AIMS scope addendum, risk-taxonomy augmentation, additional roles, reserved-matters additions, deprecation-path notes, invariants), with cross-references to the four derived artefacts by version.
- [ ] Every evidence-contract extension in the record is keyed by an `obligation_id` from the mod-104 register (I3 defence) — no sector-keyed extensions.
- [ ] `unified-model-inventory-schema.md` names one system-of-record and one authoritative-owner packet, groups attributes into the four families with each attribute typed `MRM-source | AI-source | joint`, and states the materiality-tier reconciliation with mod-107 assurance-tier explicitly.
- [ ] `validation-report-schema.md` carries the required-sections list, the SR 11-7 / Article 11 crosswalk table with every section attributed to one or both frames, the mod-108 schema registry id and owner, and the explicit statement of what the schema does not cover (outcomes analysis, ongoing monitoring — separate joint schemas).
- [ ] `applicability-filter-values.md` lists every added value with per-value rationale, states the reference-profile filter budget (or marks it `needs-research`), names at least one rejected candidate value on parsimony grounds, and states the filter-compilability test explicitly.
- [ ] `reserved-matters-overlay.md` is authored as a delta on the mod-112 chapter 01 baseline (does not restate it), carries the six chapter-02 overlay matters plus at least one scenario-specific matter, and cross-references the mod-104 obligation, mod-102 control family, and mod-108 evidence family for every row.
- [ ] Every design choice in every artefact is pinnable to a chapter-01 invariant (I1–I6) as enforcer or to a chapter-01 / chapter-02 failure mode as defence; the pinning appears as an inline footnote or annotation the reader can verify.
- [ ] Applicability-filter values pass a parsimony review — the review is authored in the file, the rejected candidate is named, and the total dimension and value counts are stated.
- [ ] The validation-report schema does not fork evidence trails — the crosswalk table proves the one-artefact-two-frames property section by section.
- [ ] Every unverified specific (SR number, bulletin number, effective date, article paragraph, regulation URL) carries `<!-- needs-research: ... -->` rather than a guessed value. Chapter 02's inherited `needs-research` markers are carried forward unchanged unless the learner has verified them.

## Stretch goals

- **Worked SR 11-7 material-findings escalation packet.** Take a plausible finding on the retail-credit-underwriting model (e.g., ongoing-monitoring evidence indicates outcome drift for a protected-class segment beyond the tolerance) and populate the mod-112 chapter-01 escalation-packet template with realistic content. Then draft the minute the council would produce on ratifying (or refusing) the finding's remediation posture. The worked example demonstrates the reserved-matters overlay's SR-11-7-material-findings row end-to-end.
- **Effective-challenge evidence mapping table.** Author a table mapping each SR 11-7 effective-challenge expectation (validator independence, competence, incentive, stature, influence, findings-acted-upon) to a specific field or set of fields in the unified model inventory and the joint validation-report schema. The table is the artefact the level-50 architect hands to internal audit as the audit-test-plan input for the effective-challenge review.
- **Canadian FI subsidiary reporting-hook design.** Draft the reporting-hook shape that the Canadian subsidiary's OSFI E-23-scoped MRM lead uses to send subsidiary-level MRM programme evidence back into the parent AIMS Clause 9.3 management review. Name the artefact set that flows up, the cadence, the seat that owns the hook on each side, and the failure mode the hook is designed against (subsidiary evidence not reaching the parent management review, or parent scope statements not being tested against subsidiary conditions).
