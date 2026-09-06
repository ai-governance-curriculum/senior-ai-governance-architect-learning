# exercise-02: Health Blueprint Drill

**Estimated effort:** 3 hours

## Objective

Instantiate chapter 01's *sector adaptation record* schematic for a US integrated health system that carries both a HIPAA-covered-entity provider identity and a wholly-owned SaMD-manufacturer subsidiary shipping an AI/ML-enabled radiology triage device under an FDA CDRH 510(k) with a Predetermined Change Control Plan. The record must anchor on the SaMD change-control obligation (chapter 03's composition anchor), and every downstream design choice — profile delta, applicability-filter overrides, evidence-contract extensions, policy-as-code guards, AIMS scope addendum, risk-taxonomy augmentation, additional roles, reserved-matters additions — must be defensible against chapter 01's six invariants (I1..I6) and the two failure modes (fork temptation; applicability-filter sprawl).

The correctness spine is the double-identity boundary. Enterprises collapse the SaMD-manufacturer identity and the HIPAA-covered-entity identity into a single evidence surface, and then produce artefacts one regulator does not want while missing artefacts the other requires. The drill forces you to work the boundary before the record — the SaMD/HIPAA boundary map is deliverable (3), not an appendix.

## Prerequisites

- Chapter [`01-the-sector-adaptation-methodology.md`](../01-the-sector-adaptation-methodology.md) read once, with the six invariants and the sector-adaptation-record YAML schematic marked.
- Chapter [`03-us-health-system-blueprint.md`](../03-us-health-system-blueprint.md) read closely — the SaMD/PCCP composition anchor, the GMLP ten principles, the HIPAA overlay on mod-108, the Section 1557 required control family, and the two failure modes.
- Mod-102 chapter 04 (OSCAL profile mechanics and applicability filter) and chapter 06 (control authoring lifecycle).
- Mod-105 chapter 04 (AIMS scope statement) and chapter 07 (Clause 9.3 management review).
- Mod-107 pre-deployment gate design; mod-108 evidence architecture and schema registry; mod-110 post-market surveillance and material-incident classification; mod-112 chapter 01 council charter and reserved-matters register.
- FDA finalised PCCP guidance (December 2024) skimmed for the three required components; HIPAA Privacy, Security, and Breach Notification Rules at part-level; Section 1557 May 2024 final rule at the algorithm-decision-support paragraph.

## Scenario

You are the level-50 architect at a hypothetical US integrated health system, *Meridian Health*, with the following footprint.

- **Provider identity.** HIPAA-covered-entity clinical operations across twelve hospitals and roughly four hundred ambulatory sites in five US states. Section 1557 applies (federal financial assistance received).
- **SaMD-manufacturer identity.** Wholly-owned subsidiary *Meridian Imaging AI* ships an AI/ML-enabled radiology triage device (Class II, cleared via 510(k) with a Predetermined Change Control Plan) to hospital customers including Meridian's own facilities and third-party health systems.
- **Research arm.** Academic-affiliated research programmes under IRB oversight; some studies use PHI drawn from the provider identity's operational systems.
- **Payer arm.** In-network health-plan subsidiary regulated as a health plan under HIPAA and, for its Medicare Advantage line, under CMS utilisation-management rules.
- **EMEA activity.** One clinical-decision-support product deployed into EU health systems; the product falls under EU AI Act Annex III via the harmonised-legislation route with EU MDR/IVDR.
- **Colorado deployment.** Provider identity operates one hospital and eighteen clinics in Colorado; the state's medical-AI regulation applies.

AI use cases in scope for the record: (a) the SaMD radiology triage device; (b) point-of-care clinical decision support (sepsis prediction, deterioration index) built in-house by the provider identity; (c) a clinician-facing generative-AI note assistant embedded in the EHR; (d) a utilisation-management adverse-determination assist used by the payer arm.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`sar-us-health-v1.0.yaml`** — the sector adaptation record instantiated to chapter 01's YAML schematic for Meridian Health, covering both identities and all four AI use cases.
2. **`pccp-authoring-guide.md`** — the Meridian Imaging AI authoring guide for a Predetermined Change Control Plan, structured against the FDA finalised PCCP guidance's three required components.
3. **`hipaa-covered-entity-vs-samd-boundary-map.md`** — the SaMD-manufacturer vs HIPAA-covered-entity evidence-surface boundary map, showing which artefacts sit on which side, which cross the boundary under a documented legal basis, and how evidence is walled off without being duplicated.
4. **`clinical-safety-committee-interface.md`** — the interface specification between the enterprise clinical safety committee (chaired under the CMO) and the mod-112 AI governance council.
5. **`state-medical-ai-regulation-scan.md`** — a scan-and-reconciliation plan for at least three US states with active medical-AI regulation touching Meridian's footprint, with each state's obligation family mapped to record fields.

## Requirements

### `sar-us-health-v1.0.yaml`

- Conforms exactly to the chapter 01 sector-adaptation-record schematic; every top-level key from the schematic is present.
- `identity_scope:` names both the SaMD-manufacturer and HIPAA-covered-entity identities and states their legal-basis interface (BAA / OHCA / ACE) per chapter 03's HIPAA-overlay guidance.
- `anchor_obligations:` cites FDA SaMD Action Plan, FDA GMLP guiding principles, FDA finalised PCCP guidance, HIPAA (parts 160 and 164), Section 1557 final rule, EU AI Act Annex III via harmonised-legislation route, and the state overlay. Unverified specifics carry `<!-- needs-research -->`.
- `profile:` derives from the enterprise reference profile and expresses only the delta; the `applicability_filter_overrides` block adds the sector's dimensions (chapter 03 names `clinical-context` and `regulatory-pathway`) with the values Meridian's use cases require. Filter-parsimony budget from chapter 01 is respected — a justification appears where a new dimension is added.
- `evidence_contract_extensions:` list line items keyed by obligation identifier (I3), not by sector; every line item's obligation identifier dereferences into a mod-104 obligation register entry (state the id even if placeholder).
- `policy_as_code_guards:` parameterise existing mod-103 templates (I4); a PHI-egress guard, a PCCP-scope-boundary guard, and a Section-1557 discrimination-testing-freshness guard are all present.
- `aims_scope_addendum:` includes both identities as an addendum to the enterprise scope, not as a second AIMS (I5).
- `risk_taxonomy_augmentation:` adds `discriminatory-clinical-outcome` and `clinical-harm` categories as specialisations of reference categories (I6), with severity rubrics aligned to enterprise bands.
- `additional_roles:` names the SaMD change-control lead (always applies) and the clinical safety officer (UK overlay — flag as not-applicable to Meridian's current footprint but present in the record for completeness).
- `reserved_matters_additions:` at least two matters for the mod-112 council — PCCP scope amendment and PCCP-boundary-exception ratification — with pack-owner and trigger.
- `deprecation_path_notes:` handle at least one plausible transition (e.g., a state regulation superseded by a successor).

### `pccp-authoring-guide.md`

- Structured against the three required components of the FDA finalised PCCP guidance: description of modifications, modification protocol, impact assessment.
- Names, for each component, the artefact Meridian Imaging AI authors, the seat that owns it, the review interval, and the evidence-schema id that binds it to mod-108.
- States the PCCP boundary check as the mod-102 profile-update-policy gate for the SaMD device: how a proposed modification is triaged as inside-PCCP-scope (ordinary post-authorisation update) or outside-PCCP-scope (new 510(k) submission required).
- Names the SaMD change-control lead's hard-stop authority at the mod-107 pre-deployment gate and the interface into mod-110 PMS and 21 CFR Part 803 MDR reporting.
- Any specific date, docket number, or clause language that cannot be verified carries `<!-- needs-research -->`.

### `hipaa-covered-entity-vs-samd-boundary-map.md`

- A table (or annotated diagram) whose rows are evidence artefacts and whose columns are: SaMD-manufacturer scope, HIPAA-covered-entity scope, both (with the legal basis that permits the cross-boundary flow), neither.
- Covers, at minimum: training-data provenance record; access-control record on the monitoring pipeline; discrimination-testing record; PCCP modification-protocol conformance record; MDR reportable-event triage record; HIPAA breach-notification triage record; GMLP-principle discharge artefacts; Section 1557 mitigation record; utilisation-management adverse-determination record (payer arm).
- Shows how the mod-108 evidence architecture holds each artefact once, indexed twice (or scoped once with an applicability-filter value), rather than duplicating (I3).
- Names the intra-enterprise BAA / OHCA / affiliated-covered-entity structure that makes the SaMD subsidiary's use of provider-identity PHI lawful, and cross-references chapter 03's HIPAA-overlay guidance.
- States one worked example of an artefact enterprises commonly wrongly duplicate and shows how the record avoids the duplication.

### `clinical-safety-committee-interface.md`

- Names the clinical safety committee's chair, membership, cadence, and current reserved-matters scope (assume a plausible pre-existing shape under the CMO).
- States the interface into the mod-112 AI governance council per chapter 03's mod-112 composition — which clinical-safety findings terminate at the council; which stay at the committee; which are co-owned.
- Defines the escalation-packet shape for a clinical-safety finding rising to the council (borrowing the mod-112 exercise-01 escalation-packet template shape).
- Names the joint standing item at the council's quarterly meeting where the clinical safety committee chair presents; states the pack owner and the pre-read interval.
- Names the failure mode this interface guards against — the collapse of clinical safety into AI governance (which strips the committee of its clinician authority) and the parallel-programme failure (which produces two answers to the same question).

### `state-medical-ai-regulation-scan.md`

- Covers at least three US states with active medical-AI regulation touching Meridian's footprint (Colorado is required by scenario; two others chosen by the learner). Learners verify current effective dates and enforcement postures against primary sources; do not embed guessed regulation content — every state citation carries `<!-- needs-research -->` where the specific provision cannot be confirmed.
- For each state: the obligation family, the enforcement authority, the sector-adaptation-record field it lands in (typically `anchor_obligations.state-overlay`, an applicability-filter value on `jurisdiction`, and one or more `evidence_contract_extensions` line items), and the reconciliation plan against the federal frame (mod-104 chapter 04 reconciliation-architecture applies).
- States the deprecation-path handling if a state regulation is likely to change within the record's version cycle.

## Starter guidance

Work the SaMD/HIPAA boundary map (deliverable 3) *before* the record. The boundary is where enterprises trip: an artefact drafted to satisfy the HIPAA Security Rule's audit-log requirements may look identical to an artefact drafted to satisfy the FDA's post-authorisation performance-monitoring commitment, and the enterprise convinces itself they are the same artefact. They are not — different retention, different disclosure surface, different oversight seat, different failure mode on tampering. Draft the boundary map first; the record's evidence-contract extensions then fall out of it.

The PCCP shape drives change-control velocity for the SaMD subsidiary. A PCCP whose description of modifications is too narrow requires a new 510(k) for every meaningful retraining cycle; a PCCP whose description is too broad invites a CDRH deficiency letter. The modification protocol is what convinces the reviewer that the pre-specified modifications are safely bounded — treat protocol-authoring as the load-bearing task, not the description of modifications. The impact assessment is where the enterprise's benefit-risk framing lives; keep it consistent with the mod-106 risk-appetite statement.

Section 1557 is not HIPAA. GMLP principle 3 (representative datasets) is not Section 1557 discrimination testing. These are different obligations against different populations at different lifecycle stages. Chapter 03's failure mode 2 is exactly the collapse of the three into a single "we test for bias" artefact — resist the collapse in the record; author three distinct evidence-contract line items.

The clinical safety committee is older than the AI governance council in most health systems and outranks it inside the CMO's remit. The interface has to respect that; the mod-112 council does not take over clinical safety review of AI-enabled clinical decision support. What the council does is terminate escalations that exceed the CMO's authority scope — enterprise-level residual acceptances, PCCP scope changes that alter the risk-benefit balance, incident findings that touch the AIMS scope. Author the interface as a peer-forum relationship, not a hierarchy.

## Acceptance criteria

- [ ] `sar-us-health-v1.0.yaml` conforms exactly to the chapter 01 sector-adaptation-record schematic; every top-level key is present and every value is either concrete or marked `<!-- needs-research -->`.
- [ ] Both the SaMD-manufacturer and HIPAA-covered-entity identities are named in `identity_scope:` with the intra-enterprise legal-basis interface stated.
- [ ] The `pccp-authoring-guide.md` covers all three FDA-required PCCP components (description of modifications, modification protocol, impact assessment) with per-component artefact, owner seat, review interval, and mod-108 schema id.
- [ ] The `hipaa-covered-entity-vs-samd-boundary-map.md` covers at least the artefact list named in the requirements section and shows one worked non-duplication example (I3 discipline honoured).
- [ ] Section 1557 appears as a first-class control family in the record, distinct from GMLP principle 3 and from HIPAA-derived artefacts; the `discriminatory-clinical-outcome` risk category is present in `risk_taxonomy_augmentation`.
- [ ] The clinical-safety-committee interface names at least one standing joint agenda item and states which finding classes terminate at the council vs stay at the committee.
- [ ] The state-medical-AI-regulation scan covers at least three states (Colorado plus two chosen), each mapped to record fields, with a reconciliation plan against the federal frame.
- [ ] Every design choice pinnable to a chapter-01 invariant or chapter-03 failure mode; a `pinning:` block or footnote in the record makes this explicit.
- [ ] Unverified specifics carry `<!-- needs-research -->`; no invented URLs, docket numbers, effective dates, clause wording, or state-statute text.

## Stretch goals

- **Worked PCCP amendment scenario.** Draft a plausible amendment to the description-of-modifications block (e.g., extending the pre-specified set to cover a new input modality on the triage device), author the escalation packet the SaMD change-control lead files with the mod-112 council for the reserved matter, and draft the council minute that ratifies the amendment.
- **Joint clinical-safety and AI-governance meeting pack.** Author the pack for a single joint meeting: agenda, per-item pre-read structure, standing minute template, and the write-to-GRC-for-AI hook. Show how the joint meeting's minutes land in both the clinical-safety-committee record system and the mod-111 GRC-for-AI platform without diverging.
