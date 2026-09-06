# exercise-01: Third-Party AI Vendor Tiering Drill

**Estimated effort:** 3 hours

## Objective

Score and tier a **ten-vendor synthetic population** against the chapter 02 rubric, and produce the three artefacts every subsequent exercise in this module depends on: a per-vendor **tiering worksheet**, an **auto-tiering carve-out log** capturing where the derived tier was lifted by an auto-tier rule, and the **vendor register population artefact** the operating rhythm queries against.

The population you tier here is the anchor for the rest of the module. Exercise-02's DDQ authoring picks one of these vendors and instantiates the tier-3 DDQ against the worksheet you wrote. Exercise-03's contract-template control drill converts the DDQ commitments into a contract-shape specification for the same vendor. Exercise-04's monitoring schedule attaches to the same vendor at the tier this exercise assigns. Exercise-05's federal-adaptation extends the register with the rights-impacting and safety-impacting designations you author here. If you under-score a dimension in this exercise, every downstream exercise inherits the misalignment; if you skip the auto-tiering log, the downstream exercises will not have the escalation record to compose against.

## Prerequisites

- Chapter [`01-the-third-party-ai-governance-lens.md`](../01-the-third-party-ai-governance-lens.md) read once, with the vendor typology (five classes), the SR 23-4 five-phase reference, and the seven invariants marked.
- Chapter [`02-vendor-tiering-criteria-and-tier-definitions.md`](../02-vendor-tiering-criteria-and-tier-definitions.md) read once, with the four dimensions, their scoring rubrics, the tier-derivation rule (`max-plus-floor`), the six auto-tiering rules (A1 through A6), the four tier definitions, and the tiering worksheet template marked.
- The mod-106 chapter on the risk taxonomy and appetite statement (the residual-risk carries the worksheet references terminate at register entries here).
- The mod-108 chapter 04 walk-through of the supply-chain evidence slice (the register carries evidence-freshness references you will populate here).
- Access to the primary references — Federal Reserve SR 23-4 (2023), OCC Bulletin 2023-17, FDIC FIL-29-2023; ISO/IEC 42001:2023 Clause 8 and Annex A third-party controls; NIST AI 100-1 MAP function (context of use) and MANAGE function (third-party risk); NIST AI 600-1 GenAI Profile; EU AI Act Regulation (EU) 2024/1689 Article 6 and Annex III (high-risk classification), Chapter V (general-purpose AI providers), Article 25 (importers), Article 27 (deployers); the sector overlays applicable to your scenario. See [`../resources.md`](../resources.md).

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose tiering shape you are least familiar with; that is where the drill will teach you most. State your choice at the top of the deliverable.

- **(A) A US regional bank** with an SR 11-7-aligned MRM programme, an ISO/IEC 27001 ISMS in place, and an ISO/IEC 42001 AIMS in build. Colorado + NYC deployments today; EU expansion planned within the next fiscal year. Ten AI vendors below.
- **(B) A global healthcare payer / provider** with HIPAA scope in the US, an EU insurance subsidiary that brings EU AI Act relevance, FDA 21 CFR Part 11 scope for regulated device software components, and multi-state deployments across California, Illinois, and New York. Ten AI vendors below.
- **(C) A B2B SaaS HR-tech vendor** shipping GenAI-augmented HR-tech across the US, UK, EU, and Singapore. Customer overlays include NYC LL144 for AEDT deployments, EU AI Act high-risk employment classification, UK Data Protection Act discipline, and MAS FEAT-style expectations for Singapore financial-services customers. Ten AI vendors below.

### The ten-vendor synthetic population

The population is deliberately mixed across the five vendor classes and across risk shapes; a well-authored tiering will produce a distribution of tier-1, tier-2, tier-3, and tier-4 vendors, not a monoculture. Do not assume every vendor is tier-4; do not assume every routine SaaS is tier-1. Score against the rubric.

- **V01 — FrontierModelCo.** Foundation-model provider (class 1). Enterprise consumes via API for a customer-facing chat feature that fields refund and billing enquiries. Prompt content includes customer names, account numbers, and free-text customer narratives that may include incidental sensitive-attribute disclosures. Vendor is US-headquartered with EU inference regions available. Vendor's standard DPA supports EU–US DPF. Vendor publishes model cards and system cards. Enterprise's usage is $2M / year and growing.
- **V02 — SmallLLMHost.** Foundation-model provider (class 1) hosting an open-weight base model as an API. Enterprise consumes for internal developer-productivity summarisation of ticketing data. No customer data flows. Vendor is US-only, small company with SOC 2 Type II but no ISO 27001 or ISO 42001. Usage is $30K / year.
- **V03 — RuntimeSafetyCo.** Guardrail vendor (class 2). Enterprise deploys the vendor's classifier on the hot path of the customer-facing chat feature that V01 backs. Failure of the classifier is failure of the enterprise's safety envelope. Vendor is EU-headquartered with US-hosted classifiers for US-region enterprise traffic. Vendor publishes classifier evaluation methodology; SOC 2 Type II; ISO 27001.
- **V04 — MarketingContentGen.** Foundation-model-backed content-generation SaaS. Enterprise uses it for marketing-copy generation with human sign-off before publication. No customer data flows; only internal marketing briefs. Vendor is US-based. Usage $80K / year.
- **V05 — DatasetLabelCo.** Dataset provider (class 5). Enterprise contracts the vendor to label 500K contact-centre transcripts for fine-tuning a sector-specialised classifier. Transcripts contain PII and incidental sensitive-attribute disclosures. Vendor's labelling workforce is distributed across Kenya, the Philippines, and Poland. Vendor is US-headquartered. Standard MSA + DPA in place.
- **V06 — LLMObserveIO.** AI-observability vendor (class 3). Enterprise sends prompts, completions, and telemetry from all production AI systems for LLM-observability, evaluation orchestration, and dashboards. Vendor's dashboards feed into second-line assurance judgements. Vendor is a US SaaS. SOC 2 Type II.
- **V07 — GRCPlatform.** GRC-for-AI platform vendor (class 4). Enterprise onboards the vendor to hold the authoritative AI governance evidence — DDQ answers, model cards, risk-register entries, evidence-of-compliance packets. Vendor is a UK-headquartered SaaS with US-region hosting. SOC 2 Type II. Enterprise's evidence architecture (mod-108) would live on the platform.
- **V08 — OpenWeightsHub.** Open-weight model distribution hub (class 1 via distribution). Enterprise's ML platform team pulls base-model checkpoints from the hub for internal experimentation and, in some cases, fine-tunes for production use. Hub carries a mix of licences (Apache-2.0, Llama Community License, MIT, various OpenRAIL variants). Hub is a US operator.
- **V09 — SectorSpecialistCo.** Sector-specialised model vendor (class 1). Enterprise consumes a highly-tuned domain-specific model whose outputs directly drive a consequential decision — for scenario (A) a credit-risk model, for scenario (B) a clinical-decision-support recommendation, for scenario (C) a candidate-shortlisting decision. No alternative vendor provides the required specialisation at present. Vendor is a mid-sized US or EU vendor per scenario. Usage $500K / year.
- **V10 — LegacyChatbotVendor.** Rules-and-ML chatbot vendor (class 3-ish / hybrid). Enterprise has used the vendor for six years for internal HR self-service chat (benefits questions, PTO balance, minor policy questions). Vendor added a GenAI backend in the last twelve months without formal notice, moving from a rules-plus-classification stack to a hybrid stack that generates responses from an LLM behind the vendor's product surface. Enterprise's contract was signed pre-GenAI. No new DPA; no representation about generative-AI use.

## Deliverables

Author four artefacts in a working directory of your choice. Each stands alone; together they compose the tiering artefact set the module's subsequent exercises inherit.

1. **`tiering-scheme.yaml`** — the enterprise-authored tiering scheme instance: derivation rule (default `max-plus-floor` or a documented alternative with rationale), auto-tiering rules (with any enterprise-specific additions), override policy, recompute triggers. This is the shape from chapter 02 pinned as an artefact of *your enterprise*.
2. **`vendor-tiering-worksheets.yaml`** — one worksheet per vendor (10 total), each instantiating the chapter 02 worksheet schema: intended use, dimension scores with rationale per dimension, auto-tier triggers that fire, derived tier, final tier, bundle pointers, sign-off seats, substrate refs.
3. **`auto-tier-carveout-log.md`** — a narrative log of every vendor whose derived tier was lifted by an auto-tier rule (A1 through A6), and every vendor whose sponsoring team pressured for a lower tier that you upheld the auto-tier against. Include the rationale a subsequent reviewer would need to reconstruct your reasoning.
4. **`vendor-register-population.yaml`** — the register-population artefact the operating rhythm queries against, per the chapter 01 register shape (vendor_id, class, tier, ddq_discharge_state, contract_template_version, contract_key_dates, monitoring_cadence, open_exceptions, last_material_incident, last_material_version_change, substrate_evidence_refs). Populate with defensible placeholder values for fields the later exercises will fill (e.g. `ddq_discharge_state: not_yet_started` for onboarding vendors).

## Requirements

### `tiering-scheme.yaml`

Decide and justify **each** of the following:

- **Derivation rule.** Pin the `max-plus-floor` rule as default, or a documented alternative (e.g. weighted-sum, decision-tree). If you pin an alternative, justify against the two enterprises whose derivation you are not scoring (would this rule tier the same vendors materially differently in scenarios (A) and (B) than in your scenario? if yes, is that a feature or a defect?).
- **Auto-tiering rules.** Adopt A1 through A6 as authored; add at least one enterprise-specific auto-tier rule motivated by your scenario (candidate for scenario A: any credit-decision model at least tier-4; candidate for scenario B: any PHI-flowing model with hallucination risk on clinical content at least tier-4; candidate for scenario C: any AEDT falling under NYC LL144 at least tier-3). Include the rule's ID, description, and rationale.
- **Override policy.** Adopt the chapter 02 shape (upward routine; downward requires AI-accountable-executive sign-off with substrate log entry). Add the specific downgrade approval seat for your enterprise.
- **Recompute cadence and triggers.** Adopt the chapter 02 shape and add any sector-specific triggers (candidate for A: any change to Colorado SB 24-205 rulemaking that reaches the vendor's use case; candidate for B: any FDA guidance update touching a device-classified vendor; candidate for C: any UK ICO or EU AI Office enforcement action touching an equivalent vendor).
- **Version and effective date.** Version 1.0.0; effective date is your authoring date.
- **Composition with enterprise TPRM.** Pick either the *parallel-fields arrangement* or the *max-tier arrangement* from chapter 02; justify.

### `vendor-tiering-worksheets.yaml`

Ten worksheets, one per vendor. For each worksheet:

- **Vendor identification.** vendor_id (assign VENDOR-2027-NNNN), vendor_display_name, vendor_class (from the chapter 01 typology).
- **Scoring date** and **scored_by** (a level-15 analyst seat), **reviewed_by** (a level-50 architect seat or level-60 head-of-governance seat depending on tier).
- **Intended-use statement.** A concrete description of what the vendor's outputs will be used for, what data flows to the vendor, and what decision materiality attaches. Do not paraphrase the scenario; author a specific use.
- **Dimension scores.** Data sensitivity (0–3), decision materiality (0–3), replaceability (0–3), jurisdictional exposure (0–3). Each score has a rationale of 1–3 sentences tied to the rubric wording from chapter 02. Do not give a score without a rationale.
- **Auto-tier triggers.** Enumerate which of A1 through A6 (and your added rule) apply and why.
- **Derived tier via the formula.** Show the base (max of scores), the lift (per the `max-plus-floor` rule), the clamped tier.
- **Final tier.** After auto-tier lifts and any override, the final tier the vendor sits at.
- **Bundle pointers.** DDQ template (candidates: templates/ddq-tier-N.yaml), contract addendum (templates/contract-addendum-tier-N.md), monitoring schedule (schedules/tier-N.yaml), supply-chain evidence profile if applicable (profiles/tier-N-artefact-ingest.yaml). These will be authored in exercises 02–07; here the pointers establish the binding.
- **Sign-off seats.** For tier-3 add head-of-ai-governance; for tier-4 add ai-accountable-executive. TPRM seat and AI governance seat required for all.
- **Substrate refs.** Placeholder URIs of the form `substrate://vendor-register/{vendor_id}/tiering/{scoring_date}`.

Special attention to vendors the drill is designed to test on:

- **V01 FrontierModelCo** should trigger A1 (foundation-model provider on customer-facing path), likely A2 (Article-9 data may flow in incidental customer narratives), likely A3 (consequential-decision path via refund / billing under multiple US state AI acts and Article 22 GDPR framing). Expect tier-4.
- **V02 SmallLLMHost** should not trigger A1 despite being a class-1 vendor because the use is internal-only with no customer data; expect tier-1 or tier-2 depending on your scoring.
- **V03 RuntimeSafetyCo** triggers A5 (guardrail on hot path of tier-4 system inherits tier, capped at tier-3 minimum). Expect tier-3 as the floor, tier-4 if the enterprise treats guardrail failure as tier-4 exposure.
- **V05 DatasetLabelCo** should trigger data-sensitivity 3 (transcripts include PII + incidental sensitive-attribute disclosures), replaceability probably 2, jurisdictional 2 (labour distributed across three jurisdictions), decision materiality depending on downstream model use. Author the rationale carefully; the exercise reads as tier-3 for most reasonable scorings.
- **V07 GRCPlatform** triggers A6 (GRC-for-AI platform holding authoritative evidence). Expect tier-3 minimum, tier-4 if evidence-loss would blind the enterprise's second-line for the duration of a certification cycle.
- **V09 SectorSpecialistCo** triggers A3 (consequential decision) and probably A4 (irreplaceable within defensible horizon). Expect tier-4.
- **V10 LegacyChatbotVendor** is the discipline test — the vendor added GenAI without formal notice; the enterprise's contract does not reflect. Score against the *current* posture, not the historical posture. Expect the tier to rise from wherever it was originally.

### `auto-tier-carveout-log.md`

For every vendor whose derived tier was lifted by an auto-tier rule, log:

- The vendor ID and the auto-tier rule that fired.
- The dimension scores at the moment the rule fired.
- The tier the derivation formula would have produced without the rule.
- The final tier after the rule.
- The rationale a subsequent reviewer needs to reconstruct: why this rule fires here, what the rule is protecting against, whether the enterprise sees any case where the rule might be exception-approved downward (typically the answer is no, and the log should say so).

For each vendor whose sponsoring team would plausibly push for a lower tier, add:

- The pushback argument in one sentence (e.g. "sponsoring team argues V05 DatasetLabelCo should be tier-2 because the labelling is bounded to a single project and does not touch production inference").
- Your architectural response and the specific rubric or auto-tier rule you cite.
- The substrate log entry the sponsoring team's request would produce if it were escalated for override.

Include a short closing narrative (2–3 paragraphs) reflecting on which vendors were surprising to score at the tier they landed at, and what the drill teaches about the auto-tiering discipline.

### `vendor-register-population.yaml`

Populate the register per the chapter 01 shape for all ten vendors. Fields:

- `vendor_id`, `vendor_display_name`, `vendor_class`, `tier`, `rights_impacting_designation` (from your read of the intended-use — this preview of chapter 06 is intentional; author true / false with a one-sentence rationale for tier-3+ vendors), `safety_impacting_designation` (same shape).
- `ddq_discharge_state`: use `not_yet_started` for vendors not being onboarded now; use `in_flight` for the vendor exercise 02 will pick up.
- `contract_template_version`: null or `pending`.
- `contract_key_dates`: use synthetic dates (start today, initial term two years, renewal at term-end).
- `monitoring_cadence`: derive from tier per chapter 05's defaults.
- `open_exceptions`: empty list for now; V10 LegacyChatbotVendor should carry an open exception (`historical-contract-lacks-genai-representation`) that exercise 03 will address.
- `last_material_incident`: null for now.
- `last_material_version_change`: `unknown` for vendors whose vendor-side change monitoring is not yet wired; a defined value for vendors you have discipline against.
- `substrate_evidence_refs`: URIs pointing to the worksheet locations.

Include at the top of the register a machine-processable summary block: total count by tier, count of rights-impacting, count of safety-impacting, count with open exceptions.

## Starter guidance

- Score the vendor before considering the auto-tier rule. The rule is a floor, not a shortcut; if the honest dimension scoring already produces the tier the auto-tier rule would produce, you have less work to defend. The auto-tier rule is what catches the *under-scored* vendor, not what the honest scoring already reaches.
- The rubric wording in chapter 02 is written to make the honest score the easy score. If you find yourself picking a score one down from the rubric text, ask why — the sponsoring team's pressure, the vendor's account manager's charm, and the procurement cycle's timeline are all present in the drill even though the drill is synthetic.
- Data sensitivity is scored against *what actually flows*, not against the theoretical worst case. V01's prompt content includes customer names and account numbers — that is 2. The incidental sensitive-attribute disclosures raise it to 3 because the enterprise cannot reliably filter them at scale. Do not score 3 for "the vendor is technically an API and someone might put in something."
- Decision materiality is scored against *the enterprise's actual practice*, not the workflow's label. If the workflow says "human review" and the human accepts the AI's recommendation 98% of the time, that is score 3, not score 2.
- Replaceability is a point-in-time score, but the honest score reflects the *realistic* migration horizon for your enterprise, not the theoretical existence of alternative vendors. Two frontier vendors in market do not make foundation-model provider replaceability score 0; the specific fine-tune, the prompt engineering, the safety configuration, the evaluation baselines, and the customer-facing behaviour all bind.
- Jurisdictional exposure is where enterprises consistently under-score. If the vendor's inference reaches a US-hosted service and your data subjects are in the EU, you are at least at 2 regardless of what the vendor's marketing pages say.
- V10 LegacyChatbotVendor is the test the drill really wants to catch. The vendor added GenAI without formal notice. Score the *current* posture, not the T=0 posture. The lift in tier this produces is what forces the exercise-03 contract retrofit and the exercise-04 re-tiering discipline.
- The auto-tier carve-out log is the artefact the third-line audit samples for evidence that the enterprise's tiering discipline is not just the formula but also the *explicit judgement about vendors the formula was designed to catch*. If your log is thin, your discipline is thin.
- Do not populate the register with fields you have not yet designed against. `ddq_discharge_state: not_yet_started` is honest; `ddq_discharge_state: complete` for a vendor whose DDQ has not been authored is a lie the operating rhythm will surface at query time.
- Preview chapter 06's rights-impacting / safety-impacting fields with care. Rights-impacting is broad (affects individuals' rights, benefits, access to essential services); safety-impacting is narrower (affects physical safety or critical-infrastructure reliability). V01's customer-facing chat is arguably rights-impacting (refund decisions affect access to a service); V09 scenario A's credit-risk model is squarely rights-impacting; V09 scenario B's clinical-decision-support may be both rights-impacting and safety-impacting. Rationale in one sentence per field.

## Acceptance criteria

- [ ] Scenario (A / B / C) is stated at the top of the deliverable; all ten worksheets are coherent against the scenario's sector overlay.
- [ ] All four artefacts (`tiering-scheme.yaml`, `vendor-tiering-worksheets.yaml`, `auto-tier-carveout-log.md`, `vendor-register-population.yaml`) are present.
- [ ] `tiering-scheme.yaml` pins the derivation rule, adopts A1–A6 with at least one scenario-specific addition, defines the override policy, and enumerates the recompute triggers.
- [ ] All ten worksheets carry per-dimension rationale (not just numeric scores).
- [ ] Every vendor's derived tier via the formula is shown; every auto-tier lift is enumerated by rule ID.
- [ ] `auto-tier-carveout-log.md` covers every vendor whose tier was auto-lifted and includes at least three sponsoring-team-pushback narratives with your architectural response.
- [ ] Register populates the tier distribution (count by tier), the rights-impacting count, the safety-impacting count, and the open-exceptions count in the summary block.
- [ ] V10 LegacyChatbotVendor carries the `historical-contract-lacks-genai-representation` open exception the exercise 03 drill will address.
- [ ] No vendor is at tier by fiat — every worksheet's tier is defensible from its dimension scores plus any auto-tier lift.
- [ ] Every unverified citation — EU AI Act article number, ISO clause number, sector-rule identifier — is marked `<!-- needs-research: ... -->`. No invented article numbers, clause numbers, or duration figures.

## Stretch goals

- **Compose the enterprise TPRM tier with the AI tier explicitly.** For each vendor, sketch what the enterprise's TPRM function would have assigned as its tier (using SIG-Lite's typical inputs — data classes, financial materiality, integration depth) and show how the composition — either parallel-fields or max-tier — resolves the two. Flag any vendor where the TPRM tier and the AI tier diverge by more than one level; these are the vendors the operating rhythm will surface at renewal and the auditor will sample.
- **Sketch a re-tiering scenario for one vendor.** Pick V04 MarketingContentGen. Assume six months after onboarding, the sponsoring team adds a use case where the vendor's output is published customer-facing without human review — a marketing landing-page generator. Walk the re-tiering: which dimensions shift, which auto-tier rules newly apply, what the new tier is, what the operating impact is (DDQ re-issuance, contract addendum amendment, monitoring cadence shift, evidence bundle refresh).
- **Author a machine-processable version of the tiering scheme against JSON Schema.** Draft the JSON Schema for the tiering-scheme.yaml and validate your instance against it. This previews mod-108 chapter 06's OSCAL representation discipline and the mod-111 GRC platform's ingest shape.
- **Draft the pre-deployment gate integration point.** Sketch how the pre-deployment gate (mod-107 chapter 02) checks the vendor register at gate time — the query it runs, the data it reads, the failure it raises when a vendor dependency is at a tier the system's own risk tier cannot absorb. This is the mod-107 → mod-109 interface the third-line audit tests.
- **Author the third-line audit sampling programme for tiering.** In one page, describe the third-line audit engagement that samples the enterprise's tiering discipline: sampling frame (all tier-4 initial scorings; percentage of tier-3; percentage of tier-2 and tier-1), what the auditor checks per sample (rubric application, rationale quality, auto-tier rule application, override discipline, register consistency), what a finding looks like, what a systemic finding versus an isolated finding is.
