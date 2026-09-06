# exercise-05: Critical Infrastructure Blueprint Drill

**Estimated effort:** 4 hours

## Objective

Author the **sector adaptation record** for a critical-infrastructure operator per chapter [`07-critical-infrastructure-operator-blueprint.md`](../07-critical-infrastructure-operator-blueprint.md) (in parallel authoring — reference the expected filename and its stated topics: CISA/NCSC *Guidelines for Secure AI System Development*, ENISA's multilayer framework for AI in critical infrastructure, NIST CSF 2.0, NIST SP 800-82 on OT security, and the sector-regulator overlay of NERC CIP, TSA Security Directives, or NIS2 competent-authority regime). The record instantiates the mod-101–mod-112 reference architecture for an operator whose AI systems touch — directly or by inference boundary — an operational-technology environment where a wrong actuation is a safety event, not a business error.

The correctness spine is chapter 01's six invariants (I1..I6) and its two failure modes (fork temptation; applicability-filter sprawl), extended by chapter 07's *AI-in-OT boundary discipline*: no-write-to-safety-instrumented-system, human-in-the-loop-for-actuation, and segmented-inference-boundary. The boundary invariants are the load-bearing discipline for this drill — they express as parameterised policy-as-code (per I4) on the mod-103 policy engine, not as a sector-forked control programme. Every design choice must be pinnable to a chapter-01 invariant (as enforcer) or to a chapter-07 failure mode (as defence).

## Prerequisites

- Chapter [`01-the-sector-adaptation-methodology.md`](../01-the-sector-adaptation-methodology.md) read once, with the six invariants, the record schematic, and the two failure modes marked.
- Chapter [`07-critical-infrastructure-operator-blueprint.md`](../07-critical-infrastructure-operator-blueprint.md) read once (or, where authored in parallel, the expected content topics reviewed: CISA/NCSC Secure AI life-cycle four-stage mapping; ENISA multilayer framework; NIST CSF 2.0 Govern/Identify/Protect/Detect/Respond/Recover; NIST SP 800-82 OT-security overlay; NERC CIP or TSA SD or NIS2 depending on jurisdiction; the AI-in-OT boundary discipline).
- Mod-102 control library — the enterprise catalog whose entries the record's profile selects, tunes, and parameterises (I1, I2).
- Mod-103 policy taxonomy and policy-as-code — **load-bearing** for the boundary invariants; the AI-in-OT guards are authored as parameterised policies (I4), not sector-forked programmes.
- Mod-105 AIMS — the scope statement whose sector addendum names the OT-adjacent inclusions and boundary exclusions (I5).
- Mod-107 three-lines and the pre-deployment gate — the assurance-and-gate discipline through which every OT-touching AI system must pass before promotion.
- Mod-108 evidence architecture — the schema registry the CISA/NCSC crosswalk artefacts and the inspection-readiness pack are written against.
- Mod-110 post-market surveillance and incident-response — the enterprise PMS the incident-notification playbook extends, obligation-keyed (I3), not sector-forked.
- Mod-112 chapter 01 reserved-matters register — where the sector-specific reserved matters this drill produces land at the ai-governance-council.

## Scenario

You are the level-50 architect at one of the following critical-infrastructure operators. Choose the one whose sector-regulator overlay you are **least familiar with**; that is where the exercise will teach you most. State your choice at the top of the sector adaptation record and carry it consistently across all five artefacts.

- **(A) A North American electric utility** operating a bulk-electric-system portfolio subject to NERC CIP. Two AI systems in scope: an AI-assisted outage-management system (advisory to the distribution control room) and a generative-AI operator-assistant piloted in the transmission control room. Anchors: NIST CSF 2.0 + NIST SP 800-82 + NERC CIP (and the CIP-008 incident-reporting family in particular).
- **(B) A US natural-gas pipeline operator** subject to TSA Security Directives (the Pipeline-2021-01 series and successors <!-- needs-research: exact successor-directive identifiers current at ratification date -->). Two AI systems in scope: an AI/ML leak-detection system fused with fibre-optic distributed acoustic sensing, and a generative-AI compliance-document assistant used by the regulatory-affairs team. Anchors: NIST CSF 2.0 + NIST SP 800-82 + TSA Security Directives.
- **(C) A European water utility** classified as an essential entity under NIS2. Two AI systems in scope: an AI-based demand-forecasting model feeding treatment-plant scheduling and an ML-based leak-detection deployment across the distribution network. Anchors: ENISA multilayer framework for AI in critical infrastructure <!-- needs-research: exact publication title current at ratification date --> + NIS2 (Directive (EU) 2022/2555) with the Article 23 24-hour early-warning and 72-hour incident-notification cadence <!-- needs-research: verify current article numbering and cadence text -->.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`sar-critical-infra-<scenario>-v1.0.yaml`** — the sector adaptation record per the chapter-01 schematic, filled in for the chosen critical-infrastructure operator.
2. **`ai-in-ot-boundary-policy-as-code.md`** — the parameterised policy-as-code guards enforcing the three AI-in-OT boundary invariants (no-write-to-SIS; human-in-the-loop-for-actuation; segmented-inference-boundary), authored in the mod-103 template shape.
3. **`incident-notification-playbook.md`** — the mapping from AI-related incident classifications (per mod-110) to the sector-regulator-required notification (NERC CIP-008 self-report / TSA SD reporting / NIS2 Article 23 early-warning-plus-incident-notification), authored as an obligation-keyed extension to the mod-110 playbook.
4. **`cisa-ncsc-lifecycle-crosswalk.md`** — a crosswalk mapping the enterprise SDLC (mod-107 gate stages, mod-108 evidence stages) to the CISA/NCSC *Guidelines for Secure AI System Development* four stages (secure design; secure development; secure deployment; secure operation and maintenance <!-- needs-research: verify stage names against current CISA/NCSC publication -->).
5. **`sector-regulator-inspection-preparedness.md`** — the enterprise's readiness pack for a sector-regulator inspection (NERC CIP audit / TSA inspection / NIS2 competent-authority supervisory action) touching the two AI systems named in the chosen scenario.

## Requirements

### `sar-critical-infra-<scenario>-v1.0.yaml`

- Conforms to the chapter-01 record schematic in full. Every top-level block populated (`sector`, `anchor_regulations`, `profile`, `evidence_contract_extensions`, `policy_as_code_guards`, `aims_scope_addendum`, `risk_taxonomy_augmentation`, `additional_roles`, `reserved_matters_additions`, `deprecation_path_notes`).
- `anchor_regulations.horizontal` names the horizontal frame the enterprise already reconciles (EU AI Act for scenario C; NIST AI RMF and any US federal horizontal frames for A and B). `anchor_regulations.sector_specific` names the sector overlay (NERC CIP / TSA SD / NIS2) with obligation-register identifiers cross-referencing mod-104.
- `profile.delta_summary` states which reference-catalog controls are selected in addition (e.g., OT-network-segmentation, safety-instrumented-system-integrity, emergency-manual-override), which reference parameters are tuned (e.g., inference-boundary latency ceiling for control-room-adjacent workloads), and which reference-optional controls become mandatory in this sector.
- `profile.applicability_filter_overrides` adds — and only adds — the sector dimensions that cannot be folded into existing dimensions. State the parsimony defence explicitly (chapter-01 failure mode 2). Likely additions: `ot-adjacency-class` (SIS-adjacent | control-room-adjacent | corporate-only), `regulator-scope` (NERC-CIP | TSA-SD | NIS2-competent-authority | none), `criticality-tier` mapped to the sector's classification (NERC BES-cyber-system impact rating <!-- needs-research: exact CIP-002 categorisation names --> / TSA pipeline criticality / NIS2 essential-vs-important entity classification).
- `evidence_contract_extensions` keys every extension by mod-104 obligation identifier (per I3). No sector-keyed extensions. Extensions likely include: incident self-report artefacts (CIP-008 / TSA / NIS2 Article 23 <!-- needs-research: exact article numbering -->), OT change-management logs, human-in-the-loop actuation records, red-team results scoped to AI-in-OT threat models, and CISA/NCSC-life-cycle stage attestations.
- `policy_as_code_guards` references — not restates — `ai-in-ot-boundary-policy-as-code.md`. Every guard entry names the mod-103 template it parameterises, the parameterisation values (which draw from the sector-profile applicability values above), and the mod-103 enforcement point.
- `aims_scope_addendum` states OT inclusions explicitly, states which OT-adjacent activities are excluded from the AIMS scope (e.g., safety-instrumented systems themselves may be governed by an IEC 61511 safety-management-system that the AIMS does not subsume — state the interface <!-- needs-research: verify IEC 61511 interface treatment against chapter 07 -->), and states the interface to the enterprise scope statement.
- `risk_taxonomy_augmentation.added_categories` adds at least one specialisation of the reference *safety* category (e.g., `ot-actuation-harm-risk`) and at least one specialisation of the reference *operational* category (e.g., `service-continuity-harm-risk` for essential-service outage). Each addition maps to a reference category and aligns to the enterprise severity bands (per I6).
- `additional_roles` names at minimum: an OT/AI-boundary architect (the seat that owns the boundary policy-as-code and its parameter tables), a sector-regulator liaison (the seat that carries the enterprise's posture to NERC / TSA / the NIS2 competent authority), and an OT-safety representative (the seat that carries chapter-07's boundary invariants into the mod-107 pre-deployment gate).
- `reserved_matters_additions` names at minimum: (a) any AI-in-OT boundary policy amendment (a change to the parameter tables the boundary guards read from); (b) any sector-regulator-facing incident notification issued or withheld above a stated threshold; (c) any new AI-in-OT deployment class not previously ratified.
- Every unverified specific carries `<!-- needs-research -->`.

### `ai-in-ot-boundary-policy-as-code.md`

- The three boundary invariants each expressed as a **parameterised policy-as-code template**, not as a sector-forked programme (satisfies I4 — this is the distinct pass/fail item).
- Template shape follows mod-103: `template_id`, `policy_intent` (one sentence), `parameters` (data tables the sector profile populates), `evaluation_logic` (pseudocode), `enforcement_point` (mod-103 chapter 05 identifier), `failure_action` (deny / require-human-approval / log-and-alert).
- **No-write-to-safety-instrumented-system.** Template denies any actuation write from an AI system whose applicability filter includes `ot-adjacency-class: SIS-adjacent` unless the target is on the SIS-safe-actuation allow-list (a parameter table) *and* the write path terminates in a documented emergency-manual-override boundary. The allow-list is populated per scenario; the template is not.
- **Human-in-the-loop-for-actuation.** Template requires a named human operator's acknowledgement, within a stated latency budget, for any actuation recommendation the AI system emits at `ot-adjacency-class: control-room-adjacent` or higher. The latency budget is a parameter tuned per scenario (advisory-only vs. time-bounded-actuation).
- **Segmented-inference-boundary.** Template requires that inference for any workload at `ot-adjacency-class: SIS-adjacent` or `control-room-adjacent` runs on a segmented inference boundary whose egress is constrained to the OT enclave; the segmentation constraint is a parameter row keyed off `regulator-scope` and `criticality-tier`.
- State the mod-107 gate step that verifies each template's parameterisation before promotion; state the mod-110 monitoring signal that catches parameter drift in production.
- State the anti-pattern explicitly: a sector-forked "critical-infrastructure policy programme" that owns these guards outside the mod-103 engine. Note why it fails (per chapter-07 failure mode <!-- needs-research: name the specific mode from chapter 07 --> and chapter-01 failure mode 1).

### `incident-notification-playbook.md`

- Authored as a **mod-110 extension keyed by obligation**, not a sector-forked incident-response programme (satisfies I3 — this is the distinct pass/fail item).
- For each of the sector-regulator notification obligations the chosen scenario is subject to, the playbook carries: the obligation id (mod-104), the trigger (the mod-110 incident classification that raises the obligation), the notification recipient (regulator or competent authority), the cadence (initial / update / final), the artefact schema (mod-108), the drafter, the reviewer, the signer, and the record-of-notification storage.
  - Scenario A: NERC CIP-008 reportable-cyber-security-incident self-report path <!-- needs-research: exact CIP-008 revision current at authoring date -->.
  - Scenario B: TSA Security Directive reporting timelines to CISA <!-- needs-research: verify current TSA-SD reporting cadence and recipient --> and any parallel PHMSA notifications where the incident intersects pipeline-safety obligations.
  - Scenario C: NIS2 Article 23 early-warning (24 hours), incident-notification (72 hours), and final report (one month) cadence <!-- needs-research: verify cadence and article numbering against Directive (EU) 2022/2555 -->; the competent-authority recipient varies per member state — the playbook names the enterprise's competent authority per operating jurisdiction.
- State the composition rule: when a single AI-related incident triggers more than one notification obligation (e.g., a NIS2 competent-authority notification and a sector-regulator notification and an internal-council escalation under mod-112), the playbook prescribes the concurrent-notification discipline and the seat that owns cross-notification coherence.
- State the withhold-decision path: withholding a notification above a stated materiality threshold is a chapter-01 reserved matter and traverses the ai-governance-council per mod-112.

### `cisa-ncsc-lifecycle-crosswalk.md`

- A table mapping the enterprise SDLC gates (as named in mod-107) to the CISA/NCSC four life-cycle stages: secure design; secure development; secure deployment; secure operation and maintenance <!-- needs-research: verify current stage names -->.
- For each stage, the table names: the existing enterprise gate that discharges the stage, the enterprise controls (from mod-102) that operationalise the stage, the evidence artefacts (from mod-108) that attest to the stage, and any augmentation the sector requires (typically OT-specific threat modelling in secure design, OT-network-segmentation validation in secure deployment, and OT-adjacent monitoring in secure operation).
- State the anti-pattern: authoring a parallel "CISA/NCSC-aligned SDLC" as if the enterprise did not already have one. The crosswalk *names* the existing SDLC gates and *augments* where the sector requires it; it does not fork the SDLC.
- Cross-reference the NIST CSF 2.0 *Govern* function as the connective tissue between the CISA/NCSC life cycle and the enterprise's AIMS governance (mod-105) — the Govern function is the layer that ensures the life cycle is not merely a technical checklist but an artefact of the enterprise's management system.

### `sector-regulator-inspection-preparedness.md`

- The readiness pack the enterprise renders **on inspection request**, not pre-authored per inspection. The pack composes from mod-108 evidence architecture as a query against the schema registry — the sector adaptation record's evidence-contract extensions are what allow the pack to be generated on demand.
- The pack includes: the sector adaptation record (this deliverable) at ratified version, the AIMS scope statement plus sector addendum, the profile derivation from the reference profile with delta explained, the applicability filter values for each AI system in scope, the CISA/NCSC crosswalk, the AI-in-OT boundary policy-as-code with the parameter tables populated for the AI systems in scope, the incident-notification playbook, the mod-107 pre-deployment gate records for each AI system in scope, and the mod-110 post-market surveillance record for each AI system in scope.
- State the rendering discipline: the pack is generated by a mod-111 GRC-for-AI-platform query, signed by the sector-regulator liaison, reviewed by the head of AI governance, and provided to the inspector under the enterprise's inspection-cooperation protocol.
- State the anti-pattern: authoring a bespoke inspection binder per inspection. That path fails at scale (three inspections per year across the operator's footprint quickly consumes the sector-liaison seat) and drifts (each binder contains a hand-curated snapshot the enterprise cannot re-derive later).

## Starter guidance

The AI-in-OT boundary invariants are the most consequential deliverable in this drill — get them wrong and the incident that will bring the sector regulator to the door is a matter of when, not if. Author `ai-in-ot-boundary-policy-as-code.md` first, ratify the templates at the mod-112 council as a reserved matter, and only then work outward to the SAR structure. Doing this in the reverse order tends to produce a SAR whose policy-as-code section is a set of English-language rules the mod-103 engine cannot enforce — which is not policy-as-code, it is policy-as-prose, and it fails I4.

The CISA/NCSC crosswalk is often skipped in favour of jumping straight to the sector-regulator overlay. Do not skip it. NIST CSF 2.0's *Govern* function is where the CISA/NCSC life-cycle and the enterprise AIMS meet; without the crosswalk the sector-regulator overlay hangs from a set of controls whose operational stages the enterprise has not verified. The crosswalk is the connective tissue that makes the rest of the record defensible under inspection.

The incident-notification playbook is where the fork temptation is strongest. A sector team faced with the specificity of NERC CIP-008 or TSA SD reporting or NIS2 Article 23 will find it locally reasonable to author a "critical-infrastructure incident-response programme" that owns these notifications end-to-end. Refuse. The mod-110 post-market surveillance is the enterprise incident-response programme; the sector-regulator notifications are obligation-keyed extensions to it. The playbook's job is to key each notification to the mod-104 obligation register and dereference each artefact to the mod-108 schema registry — nothing more.

Applicability-filter parsimony (chapter-01 failure mode 2) is easy to under-defend for critical infrastructure because the sector regulators speak in bespoke vocabularies (BES-cyber-system-impact-rating, pipeline-criticality-tier, essential-vs-important entity) that tempt the architect to add a dimension per vocabulary. Fold where you can — `criticality-tier` typically absorbs the sector's own tiering as a value set — and defend each new dimension with a parsimony note in the SAR.

The mod-112 reserved-matters additions should be scoped tightly. The council does not want to ratify every parameter change to the boundary allow-lists; it wants to ratify the discipline that governs those changes and to hear about breaches. State the reserved matters at the level of *policy amendment*, *notification-withheld above threshold*, and *new deployment class* — not at the level of *any parameter change*.

## Acceptance criteria

- [ ] Chosen scenario is stated at the top of the sector adaptation record and every artefact is coherent against it.
- [ ] The record populates every block of the chapter-01 schematic and dereferences all cross-references (mod-102 catalog identifiers; mod-103 policy templates; mod-104 obligation ids; mod-105 scope statement version; mod-106 taxonomy version).
- [ ] **Distinct pass/fail:** AI-in-OT boundary is expressed as parameterised policy-as-code in the mod-103 template shape (not as English-language rules and not as a sector-forked programme). Failing this fails the exercise regardless of the rest.
- [ ] **Distinct pass/fail:** incident-notification playbook is obligation-keyed against the mod-104 register and authored as a mod-110 extension (not as a sector-forked incident-response programme). Failing this fails the exercise regardless of the rest.
- [ ] CISA/NCSC crosswalk names the existing enterprise SDLC gates and augments where the sector requires it; it does not fork the SDLC.
- [ ] Inspection-readiness pack is a mod-111 query against the mod-108 schema registry, rendered on demand — not a pre-authored per-inspection binder.
- [ ] Every design choice is pinnable to a chapter-01 invariant (I1–I6) as enforcer or to a chapter-01 or chapter-07 failure mode as defence; a `pinning:` block or footnote in the SAR makes this explicit.
- [ ] Applicability-filter additions carry a parsimony defence in the SAR (chapter-01 failure mode 2); the level-50 architect's ratification note is authored inline.
- [ ] Reserved-matters additions in the SAR name at least the three items above (boundary policy amendment; notification-withheld above threshold; new deployment class) with pack-owner named per mod-112.
- [ ] Every unverified specific — CIP-008 revision numbers, TSA-SD identifiers, NIS2 article numbering, CISA/NCSC stage names, ENISA publication titles, IEC 61511 interface details — carries `<!-- needs-research -->` rather than a guessed value. No fabricated URLs, no invented obligation identifiers.

## Stretch goals

- **Worked incident scenario.** Draft one worked incident in which an ML system's misclassification (a leak-detection false negative, an outage-forecast miss, a demand-forecast excursion) fed downstream systems in a way that raised a sector-regulator notification obligation. Populate the relevant self-report or notification artefact (a CIP-008 report skeleton, a TSA-SD notification, or a NIS2 Article 23 early-warning) with realistic content, cross-reference the mod-110 incident record, and identify the mod-112 reserved matter (if any) the incident triggered.
- **Reserved-matters overlay.** Draft the mod-112 reserved-matters overlay entries specific to the chosen scenario in the register format from `mod-112` chapter 01 — id, title, cross-references, originating operational forum, escalation trigger, decision authority, frequency expectation, and scenario-tailoring — and cross-reference them from the SAR's `reserved_matters_additions` block.
- **Sector-regulator-liaison role packet extension.** Draft the packet extension for the sector-regulator-liaison seat: the responsibilities delta beyond the reference role, the notifications the seat owns, the standing meetings the seat attends (mod-112 council rotation, mod-110 PMS review), the inspection-cooperation protocol the seat executes, and the escalation path the seat uses when a notification decision exceeds seat authority.
