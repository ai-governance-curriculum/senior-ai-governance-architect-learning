# exercise-05: Federal Acquisition Shape Adaptation Drill

**Estimated effort:** 3 hours

## Objective

Derive, from the OMB AI acquisition memoranda (M-24-18 and its M-25-22 successor, with the M-24-10 governance ancestor) and the currently-effective successor at reading time, the **enterprise-adaptation mapping** that extends exercises 01–04's artefacts with the federal-shape overlay. The deliverable is the mapping artefact itself, plus the four augmentation records that touch the chapter 02 tiering scheme (rights-impacting / safety-impacting designation fields), the chapter 03 DDQ (vendor-representation front-matter), the chapter 04 contract catalog (the FED-01 through FED-05 family), and the chapter 05 monitoring schedule (federal-shape operating-rhythm queries). For enterprises that resell to US federal customers, add the **federal-customer schedule** as a separated composition layer.

The adaptation is where the previous four exercises' enterprise-general shape meets the specific US-federal procurement discipline that any enterprise selling to the federal government inherits by operation of contract, and that every well-designed enterprise AI procurement programme inherits in *shape* whether or not it sells to the federal government. If the mapping is a copy of the memoranda into a Word document, chapter 06 failure mode 1 is in play; if the mapping is dated against a superseded memorandum, chapter 06 failure mode 2 is in play. The drill rehearses the derivation discipline that avoids both.

## Prerequisites

- Chapter [`06-federal-acquisition-shape-adaptation.md`](../06-federal-acquisition-shape-adaptation.md) read once, with the six mapping domains (designation → tiering augmentation; vendor-representation → DDQ front-matter; mandatory-practices → enterprise bundle; contract-clause set → chapter 04 FED-01 through FED-05; post-award monitoring → chapter 05 augmentations; transparency composition), the federal-customer composition shape, the watchdog on federal-side change, the six invariants, and the two failure modes marked.
- Chapters [`02`](../02-vendor-tiering-criteria-and-tier-definitions.md), [`03`](../03-due-diligence-questionnaire-architecture.md), [`04`](../04-contract-template-controls.md), and [`05`](../05-ongoing-vendor-monitoring-schedule.md) re-read in the specific sections the adaptation touches: tiering worksheet schema, DDQ front-matter, contract catalog families, monitoring operating-rhythm queries.
- Exercises 01 (register), 02 (DDQ instance), 03 (contract-template controls), and 04 (monitoring schedule) as inputs — the vendor and scenario carry through, and the adaptation extends the same artefacts.
- The mod-104 chapter on multi-jurisdictional reconciliation — the federal-shape mapping is one jurisdictional overlay among several that the enterprise's regulatory-tracking discipline composes.
- The mod-108 chapter on the substrate's audit-log and the catalog-version discipline — the mapping is a versioned artefact; vendor engagements are signed against a specific mapping version.
- Access to the primary references — OMB M-24-10 (March 2024) <!-- needs-research: verify currently-effective governance memorandum applicable to federal customers at reading time; successor memoranda in a new administration may supersede M-24-10. -->; OMB M-24-18 (October 2024); OMB M-25-22 (April 2025) <!-- needs-research: verify currently-effective acquisition memorandum at reading time; the memorandum you cite is the one applicable to federal customer contracts formed under its effective window. -->; the AI in Government Act of 2020; the Advancing American AI Act (FY23 NDAA Sec. 7224); the FAR Council's current AI-specific case docket <!-- needs-research: verify current proposed rules and interim rules at reading time -->; NIST AI 100-1 and AI 600-1; FedRAMP, StateRAMP, and DoD SRG impact-level definitions where the enterprise has federal-cloud or DoD scope; the EU AI Act Article 6 / Annex III high-risk shape (for composition in domain 1). See [`../resources.md`](../resources.md).

## Scenario

Continue the enterprise scenario (A / B / C) and the vendor from exercises 01–04. In addition, the drill forks the enterprise's federal-customer posture — pick one that applies to your enterprise and state it at the top of the deliverable:

- **(F-NONE) Enterprise does not sell to US federal customers.** Scenario (C) B2B SaaS HR-tech is the natural fit — the enterprise's buyers are commercial; federal sales are not in the roadmap. The adaptation extends chapters 02–05 but omits the federal-customer schedule layer (domain described below in requirements) and the FED-04 flowdown clause.
- **(F-COMMERCIAL-WITH-FED-PILOT) Enterprise is commercial-first but is piloting federal-customer offerings.** Scenario (A) US regional bank is a stretch for this fork; scenario (B) healthcare payer / provider is natural (CMS, VA, or DHA engagements). The adaptation includes the federal-customer schedule for the pilot engagements only; the enterprise-general shape remains clean.
- **(F-DUAL)** The enterprise materially sells to both commercial and federal customers. The adaptation is at full depth including the federal-customer schedule, agency-specific supplements where applicable (DFARS, VAAR, HHSAR), and the FedRAMP / StateRAMP / IL-level authorisation posture composition.

The adaptation discipline is the same across all three forks; the federal-customer schedule section of the deliverable scales to the fork.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`federal-shape-adaptation.yaml`** — the authored mapping across the six domains per chapter 06's shape. One artefact, versioned, with the memoranda-referenced block, the designation scheme, the vendor-representation regime pointer, the mandatory-practices bundle, the contract-catalog additions, the monitoring augmentations, the transparency composition, the federal-customer composition (sized to your fork), the watchdog pointer, and the invariants.
2. **`tiering-worksheet-augmentations.yaml`** — the specific edits to exercise-01's `vendor-tiering-worksheets.yaml` and `vendor-register-population.yaml` that add the `rights_impacting_designation` and `safety_impacting_designation` fields with scoped definitions, and that re-score the auto-tier triggers against the designation-driven lifts.
3. **`ddq-front-matter-fed-rep.yaml`** — the FED-REP (federal-shape representation) front-matter section to prepend to exercise-02's tier-3 DDQ instance. Includes every representation statement the vendor must sign, the binding-as-contract-schedule note, and the composition with the existing DDQ categories.
4. **`contract-catalog-fed-family.md`** — the FED-01 through FED-05 contract-template control additions in the same shape as exercise-03's catalog entries (ID, name, scope, mandatory per tier, mandatory per designation, specification, fallback, measurement). If your fork is F-COMMERCIAL-WITH-FED-PILOT or F-DUAL, also author the **federal-customer schedule** as a separated schedule document that composes with the enterprise-general addendum per chapter 06.
5. **`monitoring-schedule-fed-augmentations.yaml`** — the specific additions to exercise-04's monitoring schedule and `operating-rhythm-queries.yaml`: periodic-reporting cadence for engagements inside the federal-shape scope (quarterly for rights-impacting / safety-impacting; annual otherwise), waiver-expiry sweep queries, performance-shortfall remedy-path linkage to contract clauses, and the mapping-version check on every onboarding.

## Requirements

### `federal-shape-adaptation.yaml`

Author per chapter 06's schematic. Decide and justify each of:

- **`memoranda_referenced`.** Enumerate the currently-effective OMB memoranda you are mapping against; include M-24-10 governance, M-24-18 acquisition, M-25-22 acquisition-successor, and any successor memorandum effective at reading time. For each, state the effective window the enterprise maps against (either "currently effective, no supersession noted" or "effective window X–Y, superseded by Z"). Mark `<!-- needs-research: ... -->` wherever a specific memorandum number, date, or section citation cannot be verified at authoring time.
- **`designation_scheme`.** Pin the rights-impacting and safety-impacting definitions as derived from M-24-10. State the composition rule: additive to the chapter 02 tier, union with EU AI Act Annex III high-risk designations where applicable, automatic tier lift (any rights-impacting or safety-impacting use is at least tier-3). State the designation-authority seat (candidate: head-of-AI-governance opens; AI-accountable-executive ratifies for tier-4 designations).
- **`vendor_representation_regime`.** Pin the FED-REP DDQ front-matter section as the vehicle; pin the binding-as-contract-schedule discipline via the FED-01 clause. Note the tier-neutral application (every AI vendor, every tier).
- **`mandatory_practices_bundle`.** Adopt chapter 06's MP-01 through MP-05 (all engagements), MP-R-01 through MP-R-06 (rights-impacting additions), MP-S-01 through MP-S-05 (safety-impacting additions). Add at least one scenario-specific mandatory practice. Candidates:
  - Scenario (A) bank: MP-A-01 — vendor commits to supporting SR 11-7 model-validation sampling of the vendor's model on request.
  - Scenario (B) healthcare: MP-H-01 — vendor commits to supporting HIPAA business-associate audit sampling and OCR-engagement cooperation.
  - Scenario (C) B2B SaaS: MP-C-01 — vendor commits to supporting customer-regulator inquiry on the vendor's AI-system behaviour when the enterprise's customer is itself regulated.
- **`waiver_policy`.** Adopt chapter 06's shape (named authoriser; 12-month expiry; substrate record on vendor register plus risk register). Add the specific authoriser seat for your enterprise per designation.
- **`contract_catalog_additions`.** Pin FED-01 through FED-05 as added to exercise-03's catalog. The specific entries are in `contract-catalog-fed-family.md`; this field references the IDs.
- **`monitoring_augmentations`.** Pin the three additions: federally-mandated periodic reporting cadence; performance-shortfall remedy-path linkage; waiver-expiry sweep in operating-rhythm queries. The specific additions are in `monitoring-schedule-fed-augmentations.yaml`.
- **`transparency_composition`.** State the enterprise's own transparency programme touchpoints — AI use-case inventory, published model / system cards, plain-language affected-individual notices where applicable — and the vendor-contribution shape (DDQ field + EA-04 evidence-access commitment).
- **`federal_customer_composition`.** Size to your fork. F-NONE: field is `not_applicable` with rationale. F-COMMERCIAL-WITH-FED-PILOT: schedule layer defined with pilot-engagement scope. F-DUAL: full composition with FedRAMP / StateRAMP / IL authorisation posture, agency-specific supplements, FAR-clause flowdown, and signing-authority routing.
- **`watchdog`.** Reference the mod-104 tracking artefact; enumerate the specific sources tracked (OMB memoranda, FAR Council docket, NIST guidance, agency supplements, standards references). State the re-derivation trigger (any material memorandum revision; any FAR interim rule publication; any agency supplement issuance touching enterprise-mapped practices).
- **`invariants`.** Adopt chapter 06's I1 through I6; add at least one enterprise-specific invariant motivated by your fork.
- **`version` and `effective_date`.** Version 1.0.0; effective date is your authoring date.

### `tiering-worksheet-augmentations.yaml`

Edit, do not re-author. Patch exercise-01's artefacts:

- **`vendor-tiering-worksheets.yaml` edits.** For each of the ten vendors, add the two boolean fields with a one-sentence rationale per true field:
  - `rights_impacting_designation`: true | false
  - `safety_impacting_designation`: true | false
- **Rationale discipline.** Use chapter 06's scoped definitions (rights-impacting = outputs meaningfully influence decisions about individuals' rights, benefits, access to services, or freedoms; safety-impacting = outputs meaningfully influence physical safety or critical-infrastructure reliability). Avoid conflating the two — a credit decision is rights-impacting but typically not safety-impacting; a clinical-decision-support recommendation can be both.
- **Automatic-tier-lift check.** For every vendor whose designation is true but whose exercise-01 tier is below 3, patch the tier upward and record the lift in a new section at the bottom of the worksheet: `designation_driven_tier_lift`, with the vendor ID, the designation that fired, the exercise-01 derived tier, the new tier, and the rationale.
- **`vendor-register-population.yaml` edits.** Patch the register's summary block to add two new counts: `rights_impacting_count` and `safety_impacting_count`. Patch each vendor's register entry to carry the two fields (already previewed as placeholders in exercise-01; now populated against the chapter 06 definition).
- **Composition with the scenario's own designations.** Where your scenario (A, B, or C) carries its own regulatory designations (Scenario A: SR 11-7 tier-1 / tier-2 model classification; Scenario B: FDA 21 CFR Part 11 / software-as-medical-device class; Scenario C: NYC LL144 AEDT designation, EU AI Act Annex III employment-high-risk), explicitly compose the federal-shape designation with the scenario's own designation. Where they align, state the alignment; where they diverge, state the resolution rule (typically the union of all applicable designations).

### `ddq-front-matter-fed-rep.yaml`

Author the FED-REP section to prepend to exercise-02's DDQ instance:

- **Representation statements.** For each of the following, the vendor's answer is both a diligence input and a binding representation under the FED-01 contract clause. Author the question text, the required answer type (boolean, enum, free-text), the evidence the vendor must attach, and the review-seat per answer:
  - FED-REP-01 — the offering incorporates AI (boolean) with model / component identity and version list if yes.
  - FED-REP-02 — the offering uses generative AI (boolean) with the specific generative component list if yes.
  - FED-REP-03 — training-data category disclosure (enum: public web, licensed, enterprise-contributed, synthetic, mixed) with sources list to the extent disclosable.
  - FED-REP-04 — supply-chain-evidence commitment (enum: SBOM / ML-BOM / SLSA-level / signatures delivered on request / cadence) with the specific commitment per artefact class the vendor delivers.
  - FED-REP-05 — safety-evaluation commitment (free-text) with pointers to the vendor's own safety-evaluation programme and the specific commitments the vendor makes to supporting enterprise evaluations.
  - FED-REP-06 — incident-notification commitment (free-text) referencing the specific IN-family clauses from exercise-03's catalog.
  - FED-REP-07 — data-use commitment on enterprise inputs (free-text) referencing the specific DU-family clauses from exercise-03's catalog.
  - FED-REP-08 — material-change notification commitment (free-text) referencing the specific OP-01 clause from exercise-03's catalog.
  - FED-REP-09 — subprocessor list and change-notification commitment (free-text) referencing the specific OP-03 clause.
  - FED-REP-10 — any additional representation the scenario motivates (candidate for Scenario B: HIPAA business-associate representation).
- **Binding-as-contract-schedule note.** Explicit statement at the top of the FED-REP section: "The vendor's answers in this section are a binding representation under the FED-01 contract clause; misrepresentation triggers the FED-01 remedy path including, where applicable, termination for cause under EX-01."
- **Composition with existing DDQ categories.** State how FED-REP relates to the existing six DDQ categories from chapter 03 / exercise-02:
  - FED-REP-01 and FED-REP-02 are front-matter prerequisites for the category-3 (safety, evaluation, quality) questions to apply.
  - FED-REP-03 composes with category-3 training-data-disclosure questions.
  - FED-REP-04 composes with category-5 supply-chain-evidence questions.
  - FED-REP-05 composes with category-3 safety-evaluation questions.
  - FED-REP-06 composes with category-4 incident-notification questions.
  - FED-REP-07 composes with category-2 data-use questions.
  - FED-REP-08 and FED-REP-09 compose with category-4 operational questions.
- **Tier-neutral application.** State explicitly: every AI vendor at every tier answers the FED-REP section; the depth and specificity of the required attachments scales with tier per chapter 03's tier-per-category coverage matrix.

### `contract-catalog-fed-family.md`

Author FED-01 through FED-05 in the same shape as exercise-03's catalog entries. Per entry:

- **FED-01 — Representation-and-warranty schedule.**
  - Scope: the vendor's FED-REP DDQ answers are incorporated into the executed contract as a schedule; misrepresentation triggers remedy.
  - Mandatory per tier: all tiers for AI vendors.
  - Mandatory per designation: unchanged by designation (universal).
  - Specification: the schedule is executed as the "FED-REP Representation Schedule, dated [date]"; a material misrepresentation discovered after contract formation triggers (i) notice-and-cure where cure is feasible, (ii) termination for cause under EX-01 where cure is not feasible, (iii) a potential indemnity claim under OP-05 where the misrepresentation caused enterprise regulatory exposure.
  - Fallback: no fallback; the schedule is a precondition to execution.
  - Measurement: the schedule is version-controlled on the vendor register; material changes to the vendor's facts trigger a schedule re-execution (not just an informational update).
- **FED-02 — Rights-impacting minimum-practices commitment.**
  - Scope: vendor commits to supporting each MP-R-01 through MP-R-06 practice from the enterprise's mandatory-practice bundle for engagements the enterprise has designated rights-impacting.
  - Mandatory per tier: tier-3 and tier-4.
  - Mandatory per designation: fires when `rights_impacting_designation = true`.
  - Specification: enumerate each MP-R and the specific vendor-side commitment it maps to; failure to support a committed practice triggers IN-family notification and OP-01 material-change discipline.
  - Fallback: waiver per the waiver policy with 12-month expiry.
  - Measurement: quarterly re-attestation per chapter 05.
- **FED-03 — Safety-impacting minimum-practices commitment.**
  - Scope: same shape as FED-02 but for MP-S-01 through MP-S-05 and the safety-impacting designation.
  - Mandatory per tier: tier-3 and tier-4.
  - Mandatory per designation: fires when `safety_impacting_designation = true`.
  - Specification: enumerate each MP-S; tighten the incident-notification timeline from exercise-03's IN-01 default to hour-scale for safety events; compose with EU AI Act Article 73 serious-incident reporting <!-- needs-research: verify Article 73 timeline against final text --> where applicable.
  - Fallback: waiver per the waiver policy; waivers on safety-impacting practices require joint head-of-AI-governance and accountable-executive sign-off per chapter 06.
  - Measurement: monthly signal check per chapter 05 tier-4 cadence.
- **FED-04 — Federal-customer flowdown.**
  - Scope: for enterprises that resell to federal customers, the vendor accepts flowdown of specific FAR clauses the enterprise's federal customer imposes.
  - Mandatory per tier: tier-3 and tier-4 of vendors whose output reaches a federal-customer offering.
  - Mandatory per designation: compose with the federal customer's own designation of the use case.
  - Specification: the flowdown is attached as a federal-customer schedule (see below); the schedule enumerates the specific FAR clauses, agency supplements, FedRAMP / StateRAMP / IL requirements, and periodic-reporting obligations.
  - Fallback: a vendor that cannot accept flowdown is not eligible to be used in the federal-customer-reaching system; the engagement is scoped to non-federal use or the vendor is replaced.
  - Measurement: per the federal customer's own monitoring regime; the enterprise's compliance surface composes.
  - If your fork is F-NONE: author the clause as `not_applicable_in_this_enterprise` with rationale rather than skip; the catalog is complete so that an acquisition that *does* bring federal-customer scope has the clause available.
- **FED-05 — Waiver documentation and expiry.**
  - Scope: every accepted waiver of a mandatory practice or a federal-derived clause carries a substrate record with named authoriser, rationale, expiry date (12 months maximum), and re-sweep in the operating rhythm.
  - Mandatory per tier: all tiers where any waiver is granted.
  - Mandatory per designation: enforcement is tighter for rights-impacting and safety-impacting designations (shorter expiry, joint sign-off).
  - Specification: waiver record format (vendor_id, clause_or_practice_waived, authoriser_seat, rationale, grant_date, expiry_date, conditions_for_renewal, substrate_ref); the operating-rhythm sweep queries for expiring waivers and either re-opens the waived practice or re-authorises the waiver with fresh rationale.
  - Fallback: no fallback; a waiver without a substrate record is a waiver that does not exist.
  - Measurement: operating-rhythm sweep weekly for expiry-within-90-days.

#### Federal-customer schedule (for F-COMMERCIAL-WITH-FED-PILOT or F-DUAL)

If your fork includes federal customers, author the schedule per chapter 06's composition shape:

- **Layer position.** The schedule composes on top of the enterprise-general MSA, DPA, and AI governance addendum; it does not replace them.
- **Content.** Enumerate (as applicable to your enterprise's federal customer base):
  - FAR AI-specific clauses that the federal customer's contract flows down <!-- needs-research: verify current list of FAR AI clauses at reading time; the FAR Council docket evolves. -->
  - Federal-agency-specific supplements (DFARS for DoD, VAAR for VA, HHSAR for HHS, etc.) where applicable
  - FedRAMP / StateRAMP / IL2-6 authorisation posture requirements
  - Specific rights-impacting / safety-impacting designations the federal customer has applied
  - Specific minimum-practices bundle the federal customer requires
  - Specific supply-chain-evidence obligations (SBOMs, ML-BOMs, SLSA levels)
  - Periodic-reporting cadence and content
- **Precedence.** The federal-customer schedule overrides the enterprise-general addendum on the specific federal engagement; enterprise-general terms apply where the schedule is silent.
- **Signing authority.** Federal-contracts function executes; AI governance architect ratifies the AI-specific overrides.
- **Substrate.** The federal-customer contracts form a distinct partition of the vendor register with additional query surfaces (contract-award / option-year windows, agency-specific reporting cadences, FAR-clause version tracking).

### `monitoring-schedule-fed-augmentations.yaml`

Augment exercise-04's monitoring schedule and operating-rhythm queries. Author the additions, not a re-authored schedule:

- **Federally-mandated periodic reporting.** For engagements inside the federal-shape scope (rights-impacting, safety-impacting, or federal-customer-reaching), the vendor delivers periodic reporting at a tier-and-designation-appropriate cadence. Suggested defaults:
  - Rights-impacting or safety-impacting: quarterly report covering continued conformance to the mandatory-practices bundle, evaluation results against the enterprise's defined metrics, any material changes to the offering, any incidents in the quarter.
  - All other federal-shape engagements: annual report of the same content.
  - Report ingests to the mod-108 substrate as a specific document class.
- **Performance-shortfall remedy-path linkage.** A performance shortfall detected under exercise-04's monitoring (mod-107 ongoing-assurance signals surfaced as E3 triggers) opens, in addition to the enterprise risk-register entry, a contract-remedy path. Author the linkage: which exercise-03 OP-family clauses fire, which FED-02 / FED-03 commitments bind, which escalation seats engage.
- **Waiver-expiry sweep.** Add to `operating-rhythm-queries.yaml`:
  - **Weekly:** `any waiver with expiry within 90 days?` — surfaces for pre-expiry re-authorisation or re-opening of the waived practice.
  - **Weekly:** `any expired waiver?` — surfaces immediate escalation; an expired waiver is treated as a non-conformity against the FED-05 clause.
- **Mapping-version check on onboarding.** Every new vendor engagement onboarded under the federal-shape mapping is tagged with the specific mapping version (1.0.0 at authoring; subsequent versions tracked). Add a query: `any vendor onboarded under mapping version N-2 or older?` — surfaces engagements for re-derivation review on the next renewal or sooner if a memorandum revision requires it.
- **Federal-customer-specific additions (if F-COMMERCIAL-WITH-FED-PILOT or F-DUAL).**
  - **Monthly:** federal-customer-reaching vendors with FedRAMP / StateRAMP authorisation lapsing within 180 days.
  - **Quarterly:** agency-supplement-flowdown vendors whose supplement version has been superseded.
  - **On every FAR Council case closure or OMB memorandum issuance:** mapping-re-derivation ticket opened for the architect; mapping version increments on acceptance of the re-derivation.
- **Scenario-specific additions.** At least one additional query motivated by your scenario and fork. Candidates:
  - Scenario (A) bank F-NONE: `any vendor whose offering touches a credit-decision use case but lacks the rights-impacting designation?` — catches under-designated vendors.
  - Scenario (B) healthcare F-COMMERCIAL-WITH-FED-PILOT: `any CMS / VA / DHA engagement vendor whose FED-REP representation is more than 12 months stale?`
  - Scenario (C) B2B SaaS F-NONE: `any customer-regulator-sensitive vendor whose watchdog has detected a peer-SaaS enforcement action in the last 90 days?`

## Starter guidance

- **Derive, do not copy.** The failure mode chapter 06 names first is copying memorandum language into a Word document. The adaptation is an engineering activity: each obligation maps to an existing element (patch exercise-01–04 artefacts), a new element (FED-REP front-matter, FED-01 through FED-05 clauses), or an explicit non-adoption with rationale. If your adaptation does not touch the chapter 02 worksheet, the chapter 03 DDQ, the chapter 04 catalog, and the chapter 05 schedule concretely, something has been copied rather than derived.
- **Mark memoranda citations carefully.** OMB memoranda evolve on administration cycles; M-24-18 was issued October 2024 and M-25-22 was issued April 2025, but subsequent revisions and successor memoranda are plausible by the time this exercise is attempted. For every specific section citation, enumeration of minimum practices, or specific attachment reference you cannot verify from the primary source at authoring time, use `<!-- needs-research: ... -->` rather than paraphrasing. The adaptation discipline the drill rehearses depends on *the mapping being derived against the right text*, not against a remembered or hallucinated version.
- **Rights-impacting vs safety-impacting is a scoped designation, not a vibe.** Chapter 06 pins the definitions. A refund decision is rights-impacting because it affects access to a service (and the chat feature behind V01 in exercise-01 fields refund enquiries); a credit-risk decision is rights-impacting; a clinical-decision-support recommendation is often both rights-impacting and safety-impacting; a developer-productivity summarisation tool with no customer-reach is neither. Resist designation drift: labelling everything safety-impacting devalues the designation.
- **Composition with the EU AI Act Annex III designation matters.** Where a vendor supports an EU-market deployment and the use is Annex III high-risk, the rights-impacting designation is almost certainly also present. The enterprise carries the union of the two designations; mandatory practices from both regimes apply.
- **Waivers are the quietest failure mode.** Chapter 06 invariant 4 is specifically about waivers not accumulating silently. In the FED-05 specification, the substrate record and the operating-rhythm sweep are what make the invariant real; without the sweep, the record is a filing cabinet.
- **The federal-customer schedule is deliberately separated.** The non-federal customer that pushes back at renewal on a clause that reads as FAR-specific is the signal that the enterprise's general addendum has accreted federal-specific content. The schedule-as-layer pattern (chapter 06 invariant 5) is the discipline that keeps the enterprise-general shape clean.
- **Mapping version is a first-class field.** Vendor engagements onboarded under mapping v1.0.0 inherit that mapping; a memorandum revision triggers a re-derivation (mapping v1.1.0 or v2.0.0 depending on the material of the change); the operating rhythm surfaces engagements on stale mappings for migration planning at renewal. Without the versioning, the enterprise's addendum silently ages against evolving federal expectations.
- **Do not score the whole programme on the federal shape alone.** The federal shape is one jurisdictional overlay among several (EU AI Act, sector overlays like HIPAA / NAIC / Colorado SB 24-205, enterprise-specific risk-appetite overlays). The mod-104 reconciliation discipline is what composes multiple overlays; this exercise rehearses one of them.

## Acceptance criteria

- [ ] Scenario (A / B / C) and federal-customer fork (F-NONE / F-COMMERCIAL-WITH-FED-PILOT / F-DUAL) are stated at the top of the deliverable.
- [ ] All five artefacts (`federal-shape-adaptation.yaml`, `tiering-worksheet-augmentations.yaml`, `ddq-front-matter-fed-rep.yaml`, `contract-catalog-fed-family.md`, `monitoring-schedule-fed-augmentations.yaml`) are present.
- [ ] `federal-shape-adaptation.yaml` enumerates the memoranda referenced with effective-window notes, the designation scheme with composition rules, the vendor-representation regime, the mandatory-practices bundle (with at least one scenario-specific addition), the waiver policy with named authoriser, the contract-catalog additions, the monitoring augmentations, the transparency composition, the federal-customer composition (sized to fork), the watchdog pointer, and the invariants (chapter 06 I1–I6 plus at least one enterprise-specific).
- [ ] Tiering-worksheet augmentations add `rights_impacting_designation` and `safety_impacting_designation` to each of the ten vendors with one-sentence rationale per true field; every designation-driven tier lift is recorded in a dedicated section; register summary carries both counts.
- [ ] DDQ FED-REP front-matter authors all ten representation statements with answer type, required evidence, and review-seat; the binding-as-contract-schedule note is explicit; the composition with the six chapter 03 DDQ categories is mapped per statement.
- [ ] Contract catalog FED-01 through FED-05 authored in the exercise-03 shape (ID, name, scope, mandatory per tier, mandatory per designation, specification, fallback, measurement); if fork is F-COMMERCIAL-WITH-FED-PILOT or F-DUAL, the federal-customer schedule is authored as a separated layer; if F-NONE, FED-04 is marked `not_applicable_in_this_enterprise` with rationale rather than omitted.
- [ ] Monitoring augmentations author the periodic-reporting cadence, performance-shortfall remedy-path linkage, waiver-expiry sweep queries, mapping-version check, and at least one scenario-specific query.
- [ ] Every unverified citation — OMB memorandum section / attachment / enumeration, FAR case number, EU AI Act Article 73 timeline, FedRAMP / StateRAMP / IL authorisation specifics, agency-supplement text — is marked `<!-- needs-research: ... -->`. No invented memorandum content, no invented enumeration, no invented timelines.
- [ ] The adaptation is a derivation, not a copy. Each of the five artefacts touches the exercise-01–04 outputs concretely (patches tiering worksheets, prepends DDQ sections, adds contract clauses, augments monitoring schedule).

## Stretch goals

- **Author the mapping-version migration plan.** Assume a hypothetical successor memorandum (M-26-NN) issues six months after your authoring date and makes two material changes: tightens the safety-impacting incident-notification timeline from 24 hours to 4 hours, and adds a new mandatory practice MP-07 covering supply-chain evidence delivery on request. Walk the mapping v1.0.0 → v2.0.0 re-derivation: which artefacts are re-authored, which vendor engagements are re-opened, which renewals carry the migration first, which waivers must be re-authorised against the new baseline.
- **Compose the federal shape with the EU AI Act Chapter V GPAI shape.** For a vendor that is both a federal-customer-reaching GPAI provider (likely V01 FrontierModelCo in exercise-01's population) and whose model falls under EU AI Act Article 55 systemic-risk GPAI obligations, sketch the specific composition: which federal-shape FED-REP representations overlap with which Chapter V Article 53 disclosures; where the two regimes diverge; which clauses the enterprise's contract addendum must carry to compose both regimes without double-counting or gap.
- **Author the pre-deployment-gate integration for the federal-shape designations.** Sketch the mod-107 chapter 02 pre-deployment gate check that reads the vendor register for every system's vendor dependencies, verifies rights-impacting and safety-impacting designations against the system's own designation, and raises a gate failure when the vendor's designation posture does not cover the system's designated use. Include the specific failure types (vendor missing designation; vendor has designation but mandatory practices not committed; vendor waiver expiring within gate window).
- **Draft the agency-engagement rehearsal.** For F-COMMERCIAL-WITH-FED-PILOT or F-DUAL enterprises, draft a one-page rehearsal of the agency's post-award review of a specific vendor engagement: what the agency reviewer asks for (the FED-REP schedule, the mandatory-practices evidence, the periodic reports, the waiver log), what the enterprise's substrate must surface in under 24 hours, what a reviewer finding looks like and how it routes.
- **Author the third-line audit engagement sampling the federal-shape mapping.** In one page, describe the third-line audit engagement that samples the enterprise's adaptation discipline: sampling frame (every vendor with rights-impacting or safety-impacting designation; every FED-02 / FED-03 commitment; every waiver record; every mapping version with at least one vendor engaged against it), what the auditor checks per sample (designation rationale quality, commitment-evidence currency, waiver-authoriser-seat correctness, mapping-version currency), what a finding versus a systemic finding is.
