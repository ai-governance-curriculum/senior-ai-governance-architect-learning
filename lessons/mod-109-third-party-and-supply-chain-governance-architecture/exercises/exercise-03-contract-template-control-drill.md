# exercise-03: Contract Template Control Drill

**Estimated effort:** 3 hours

## Objective

Convert the exercise-02 DDQ commitments into a **tier-scaled contract-template control specification** that legal can draft against. The deliverable is a per-control specification set (control shape, intent, vendor commitment, evidence reference, enforcement lever, tier applicability), a **tier-mandatory matrix** for your enterprise, a **DDQ-to-contract closure record** that shows the below-threshold answers turning into contract remediations, a **residual-risk carry** for any vendor-proposed carveouts your scenario admits, and a **contract-shape review** for the V10 LegacyChatbotVendor retrofit from exercise-01.

The drill's discipline is *specification*, not *drafting*. The architect ships the control shape; legal drafts the enforceable clause. If the specification is under-scoped, legal's draft is under-scoped; if the specification is over-scoped into clause language, the drafter's craft is displaced and the enforceable text loses jurisdictional judgement.

## Prerequisites

- Chapter [`04-contract-template-controls.md`](../04-contract-template-controls.md) read once, with the six control families (DU data-use, EA evaluation-access, IN incident-notification, EV evidence-access, EX exit-and-portability, OP operational cross-cutting), the tier-mandatory matrix scaffold, the composition with MSA / DPA / order form / sector schedules, the drafting-handoff contract with procurement and legal, and the six invariants marked.
- Chapter [`06-federal-acquisition-shape-adaptation.md`](../06-federal-acquisition-shape-adaptation.md) read once for the FED-01 through FED-05 additions that compose here.
- Exercises 01 (`vendor-register-population.yaml`, `vendor-tiering-worksheets.yaml`) and 02 (`ddq-instance-<vendor-id>.yaml`) as inputs — the vendor at hand is the same across the three exercises.
- The mod-106 chapter on risk-register carries — the residual-risk artefact terminates at register entries here.
- The mod-108 chapter 07 coordination-contracts walk-through — this exercise pins the architect / procurement / legal / accountable-executive coordination shape from chapter 04.
- Access to the primary references — SR 23-4 (contract-negotiation phase); ISO/IEC 42001 Clause 8 and Annex A third-party controls; ISO/IEC 27036-2 supplier-relationships requirements; EU AI Act Article 25 (importers) and Article 27 (deployers) and Chapter V (GPAI providers); the sector overlays for your scenario (HIPAA business-associate discipline; PCI DSS 12.8; NAIC Model Bulletin; NYC LL144; MAS FEAT); the foundation-model vendor public commitment materials referenced in exercise-02. See [`../resources.md`](../resources.md).

## Scenario

Continue the enterprise scenario (A / B / C) and the specific tier-3-or-tier-4 vendor from exercises 01–02. In addition, the drill picks up **V10 LegacyChatbotVendor** (from exercise-01) for the contract-retrofit slice — the exercise's second scenario. State both vendors at the top.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`contract-control-catalog-v1.yaml`** — the enterprise-authored control catalog per chapter 04's shape: for each of the six families (DU, EA, IN, EV, EX, OP), the control shapes your enterprise adopts, each with intent, vendor commitment, evidence reference, enforcement lever, tier-mandatory disposition, and any conditional-on-vendor-class filter. Include the federal-adaptation family (FED-01 through FED-05) as a seventh family per chapter 06 (preview).
2. **`tier-mandatory-matrix.yaml`** — per chapter 04's shape, the tier-mandatory matrix showing which controls are mandatory at tier-1 through tier-4. Include the exception policy (permitted carveouts, approval authority per tier, register entry requirement, expiry).
3. **`ddq-to-contract-closure-<vendor-id>.md`** — the closure record for the exercise-02 vendor: for every DDQ answer that scored below threshold, either the specific contract clause remediation (with control-shape citation), the compensating enterprise-run control (with substrate binding), or the substrate-recorded residual-risk carry (with register-entry ID). At least three carve-outs are documented with residual-risk carries.
4. **`legacy-vendor-retrofit-<v10-id>.md`** — the contract-retrofit scenario for V10 LegacyChatbotVendor. The vendor added GenAI to its stack without notice; the enterprise's contract does not reflect. Author: (i) the gap analysis (which control shapes are missing from the executed contract compared to the tier V10 now sits at per exercise-01's re-tiering), (ii) the retrofit playbook (which controls the enterprise pursues at renewal versus which at an addendum negotiation now), (iii) the vendor's likely pushback and the enterprise's negotiation posture, (iv) the residual carry if the retrofit fails partially, and (v) the exit posture if the retrofit fails entirely.
5. **`architect-legal-procurement-handoff.md`** — the coordination-contract artefact per chapter 04. Names the architect's slice, legal's slice, procurement's slice, and the AI-accountable-executive's slice for exception approval. Includes a specific illustrative handoff — you take one control shape (recommend IN-01 severity-notification or EA-01 evaluation-access) and walk what the architect delivers to legal, what legal drafts, what procurement issues to the vendor, what the vendor's negotiation move typically is, what the escalation path looks like, and where the executed contract's schedule lands.

## Requirements

### `contract-control-catalog-v1.yaml`

Author the full six control families per chapter 04 with your enterprise's specific choices:

- **Family DU (data-use).** Adopt DU-01 through DU-05 as chapter 04 authors them; add at least one scenario-specific control. Candidate additions:
  - Scenario (A) bank: `DU-06-financial-record-limitation` — vendor may not use financial-transaction data or credit-decision inputs for any purpose beyond the specific engagement, even in anonymised or aggregate form; extends DU-01 to bind on secondary-use forms banks are particularly exposed to.
  - Scenario (B) healthcare: `DU-06-phi-flow-limitation` — vendor's use of PHI is bounded to the specific business-associate agreement's permitted uses; extends DU-01 to bind on de-identified derivations.
  - Scenario (C) B2B SaaS: `DU-06-customer-content-limitation` — vendor may not use enterprise customers' end-user content (which the enterprise's SaaS surfaces to the vendor) beyond the specific engagement; extends DU-01 to bind on the enterprise's customers' interests.
- **Family EA (evaluation-access).** Adopt EA-01 through EA-05; author the intent, evidence reference, and enforcement lever per chapter 04. For EA-01 specifically, include the specific commitment that the vendor's rate-limits and abuse-monitoring do not block enterprise disclosed evaluation runs (composes with EA-02); author the disclosed-evaluation whitelist mechanism as a concrete shape.
- **Family IN (incident-notification).** Adopt IN-01 through IN-05; add scenario-specific severity taxonomies. For scenario (A) bank, align the severity-notification hours with the sector's incident-reporting expectations (SEC 8-K, OCC / Fed supervisory reporting, state-level financial-services reporting) <!-- needs-research: verify current SEC 8-K cybersecurity incident-reporting timelines and any sector-specific vendor-incident supervisory expectations at authoring time -->. For (B) healthcare, align with HIPAA breach-notification timelines <!-- needs-research: verify HIPAA breach-notification timelines and threshold for reportable breaches at authoring time -->. For (C), align with GDPR Article 33 72-hour and UK ICO analogue.
- **Family EV (evidence-access).** Adopt EV-01 through EV-05; author with attention to the specific certification set your vendor class typically holds (SOC 2 Type II is common; ISO 42001 is emerging; sector-specific attestations vary). For EV-05 supply-chain evidence, cite the class × tier minimum bundle from chapter 07 preview.
- **Family EX (exit-and-portability).** Adopt EX-01 through EX-06; align EX-05 extended-retention with your scenario's applicable regime (EU AI Act typical horizon; SR 11-7 documentation retention for bank scenario; HIPAA record retention for healthcare scenario).
- **Family OP (cross-cutting operational).** Adopt OP-01 through OP-07; author OP-05 indemnity and OP-06 insurance with attention to the AI-specific exposures your scenario carries (IP-indemnity on outputs; data-breach indemnity; safety-incident indemnity; discrimination-claim indemnity for AEDT scenarios).
- **Family FED (federal-adaptation preview).** Adopt FED-01 through FED-05 per chapter 06. FED-01 binds the DDQ representations as contract representations; FED-02 and FED-03 bind the rights-impacting and safety-impacting minimum practices; FED-04 handles the federal-customer flowdown (may not apply to your scenario); FED-05 documents waivers.

Each control shape in the catalog carries:

- `id`, `family`, `intent` (one paragraph), `vendor_commitment` (a specific declarative sentence), `evidence_reference` (which DDQ question(s) support), `enforcement_lever` (material-breach cure, termination-for-cause, audit-right, injunctive-relief threshold, price protection), `tier_mandatory` (list of tiers), `conditional_on` (vendor-class filter or capability filter), `substrate_binding` (where the vendor's specific commitment lands in the executed contract's schedule), `drafting_notes_for_legal` (a few sentences of specification detail the drafter needs — jurisdiction sensitivity flags, cross-references to MSA / DPA sections, negotiation-fallback shape).

### `tier-mandatory-matrix.yaml`

Per chapter 04's shape:

- For each of the four tiers, enumerate the mandatory control IDs. Reference the chapter 04 defaults (tier-1 ~6 light; tier-2 ~12; tier-3 ~28; tier-4 ~32+).
- Add your enterprise-specific controls to the mandatory list where you authored them (the scenario-specific DU-06 addition; any additional IN or EV controls; FED family controls).
- Enumerate the *conditional-mandatory* controls (mandatory only where the vendor offers a specific capability — DU-03 fine-tuning-corpora limitations, EX-02 fine-tune weight portability, OP-07 escrow, EA-05 sub-service organisation SOC-2).
- Enumerate the *hard-fail* controls (chapter 04 doesn't strictly separate hard-fail from mandatory, but the exercise asks for it) — the controls whose absence at execution blocks the vendor's onboarding absent an executive-sponsor exception.
- Exception policy: named authoriser per tier (chapter 04's shape: head of AI governance for tier-3 carveouts; AI-accountable-executive for tier-4 carveouts); expiry (typical 12 months); register entry requirement; substrate log entry shape.
- Version 1.0.0; effective date; author.

### `ddq-to-contract-closure-<vendor-id>.md`

For each below-threshold DDQ answer from exercise-02's instance file:

- The question ID, the score, the applicable control shape (from your catalog).
- The chosen remediation path — one of:
  1. **Contract remediation** — the vendor commits to remediating within a defined window; the contract's schedule carries the specific commitment; the enterprise's evidence expectation shifts to the remediated posture; the risk-register entry is opened with a defined closure trigger (remediation confirmed by the vendor at date X; evidence verified by seat Y).
  2. **Compensating enterprise-run control** — the enterprise runs a control that substitutes for the vendor's absence of the commitment (candidate: enterprise-run evaluation suite substituting for a missing vendor safety-evaluation disclosure; enterprise's own guardrail substituting for a missing vendor guardrail commitment); the substrate carries the compensating-control evidence artefact; the risk-register entry is opened with an ongoing-monitoring reference.
  3. **Substrate-recorded residual-risk carry** — the enterprise accepts the residual risk with an executive-sponsor exception, a defined expiry, and a substrate log entry. This is the option of last resort.

For each residual-risk carry:

- The specific vendor commitment absence.
- The exposure the enterprise inherits.
- The compensating enterprise controls if any partial ones exist.
- The residual dollar (or equivalent) exposure estimated to the risk register's method (mod-106).
- The exception's authorising executive (head of AI governance for tier-3; AI-accountable-executive for tier-4).
- The expiry date (12 months typical for a first-instance exception; 6 months for renewals).
- The register entry ID that the risk-register maintains and the monitoring rhythm queries against.

At least three residual carries must be present. If the exercise-02 DDQ instance scored all questions above threshold, invent at least three below-threshold scenarios grounded in typical foundation-model vendor commitments (candidate: vendor does not offer contractual model-version-pinning for the enterprise's endpoint; vendor's incident-notification commitment for cross-customer safety incidents is inconsistent with IN-05 expectation; vendor's IP-indemnity on outputs excludes training-data copyright claims).

### `legacy-vendor-retrofit-<v10-id>.md`

- **Gap analysis.** V10's tier at exercise-01 (probably tier-2 or tier-3 after re-tiering; scenario-dependent). The executed contract's shape (standard MSA + standard DPA + no AI-specific addendum; no representation of generative-AI use). Enumerate which control shapes at V10's current tier are missing from the executed contract. This is a substantive list — for tier-3, expect at least 15 missing controls across families.
- **Retrofit playbook.** Which controls the enterprise pursues at renewal (the majority for typical vendor relationships without material immediate risk); which at an immediate addendum negotiation (the safety-and-security-critical ones, typically IN-01 severity-notification, DU-01 no-training-on-enterprise-data, and a subset of the EV family for evidence access). State the sequencing and the sponsoring team's involvement.
- **Vendor negotiation posture.** What the vendor's likely response is (a chatbot vendor pivoting to GenAI is often at a smaller scale than a frontier vendor and may push back on evaluation-access rights, on subprocessor-consent rights, and on extended-retention obligations). Your architectural response to each pushback: what you concede, what you hold, what you require executive-sponsor sign-off to concede.
- **Residual carry if retrofit is partial.** If the vendor accepts, say, half of the pursued controls, what the residual is, whose sign-off it takes, and what the register entry's shape is.
- **Exit posture.** If the vendor refuses material controls, the exit path — how the enterprise migrates the internal HR-self-service workflow to an alternative (in-house-hosted equivalent using an open-weight base model plus enterprise-run guardrails is a plausible internal alternative; another chatbot vendor with better AI-governance discipline is another). Estimate the migration horizon and the interim risk carry.

### `architect-legal-procurement-handoff.md`

Per chapter 04's shape:

- **Architect (level 50) slice.** Owns the control shape, the intent, the vendor commitment declarative sentence, the evidence reference, the enforcement lever, the tier-mandatory disposition. Ratifies the catalog. Reviews vendor-proposed carveouts.
- **Legal slice.** Converts the control shape to clause text. Carries jurisdictional judgement, common-law posture, negotiation drafting, fallback language. Owns the MSA and DPA the addendum composes onto.
- **Procurement slice.** Selects the tier-appropriate bundle for a specific vendor; issues to the vendor's negotiation counterpart; runs the negotiation; escalates carveouts.
- **AI-accountable executive slice.** Approves exceptions for tier-4 mandatory-control carveouts; head of AI governance approves for tier-3.
- **Illustrative walk-through.** Pick one control (IN-01 severity-notification is a good candidate; EA-01 evaluation-access is another). Walk:
  1. What the architect delivers to legal (the control specification per your catalog).
  2. What legal drafts in enforceable-clause form (the specification's translation into a specific jurisdiction's contract text; you don't have to draft the text, but describe the drafting move — the definitions, the timing clauses, the notification-channel clauses, the cure-period clauses, the termination-for-cause link).
  3. What procurement issues to the vendor as part of the redlined addendum.
  4. What the vendor's negotiation move typically is (candidate: vendor pushes back on hour-scale notification citing internal escalation process; vendor pushes back on evaluation-access citing rate-limit abuse-monitoring).
  5. The escalation path when negotiation reaches an impasse (procurement escalates to architect for shape review; architect escalates to head of AI governance; head of AI governance escalates to AI-accountable-executive for exception approval or continue-negotiating direction).
  6. Where the executed contract's schedule lands (the specific severity taxonomy, notification timelines, notification channel, cure period) — the substrate-referenced schedule that binds the vendor at execution.

## Starter guidance

- The catalog is *your enterprise's* instantiation of the chapter 04 shape. Do not paraphrase chapter 04 verbatim; make specific choices — which controls at which tiers, which enforcement levers, which scenario-specific additions. The specificity is what the drill teaches.
- Do not draft clause text. If you find yourself writing "the Vendor shall...", you have crossed into legal's craft; step back and describe the control shape and vendor commitment instead. The `drafting_notes_for_legal` field is where drafting-specific detail lands.
- The DDQ-to-contract closure record is where the exercise-02 discipline pays off. If the exercise-02 instance scored every question above threshold, the closure record is thin and the drill's residual-risk-carry discipline goes untested; invent below-threshold scenarios per the guidance.
- Residual-risk carries are the least-preferred option, not the default. Every carry represents an accepted exposure the operating rhythm must monitor. If your closure record has more than half of the below-threshold answers going to residual carry (rather than contract remediation or compensating control), the discipline is inflating the residual and the auditor will surface the pattern.
- V10 LegacyChatbotVendor is where the exercise really tests contract-retrofit discipline. The vendor added GenAI without notice. The enterprise's contract does not reflect. Real enterprises face this exact scenario repeatedly. The retrofit playbook is where "we should have had these clauses" becomes "here is how we get them without a full renegotiation."
- The vendor negotiation posture in the legacy retrofit is not adversarial from the vendor; it is *misaligned incentive*. The vendor's account manager is not incentivised to renegotiate an existing contract; the vendor's legal team is not incentivised to accept new obligations mid-term. The architect's move is to make the retrofit either (i) contingent on a mutually-desired outcome (renewal on better terms; new engagement expansion) or (ii) unavoidable given the enterprise's tier-appropriate discipline (which is what forces the exit posture as a defensible alternative).
- The architect-legal-procurement handoff artefact is a *governance artefact*, not a process document. It exists because when the drill breaks down (a control is silently dropped in negotiation, an exception is granted without register entry, a jurisdiction-specific clause is drafted without architect review), the artefact is what the third-line audit reads to identify where the failure occurred and who is accountable.

## Acceptance criteria

- [ ] Scenario (A / B / C), exercise-02 vendor, and V10 LegacyChatbotVendor are stated at the top of the deliverable.
- [ ] All five artefacts (`contract-control-catalog-v1.yaml`, `tier-mandatory-matrix.yaml`, `ddq-to-contract-closure-<vendor-id>.md`, `legacy-vendor-retrofit-<v10-id>.md`, `architect-legal-procurement-handoff.md`) are present.
- [ ] The catalog carries all six chapter-04 families plus the FED family; each control shape has id, family, intent, vendor_commitment, evidence_reference, enforcement_lever, tier_mandatory, conditional_on, substrate_binding, drafting_notes_for_legal.
- [ ] At least one scenario-specific control addition per family for the families where the scenario has specific exposures (DU family gets DU-06 addition; IN family gets scenario-specific severity taxonomy; EV family gets scenario-specific attestation set).
- [ ] Tier-mandatory matrix enumerates each tier's mandatory list, conditional-mandatory list, hard-fail list, and exception policy with named authoriser per tier.
- [ ] DDQ-to-contract closure record covers every below-threshold DDQ answer from exercise-02 with a specified remediation path; at least three residual-risk carries are documented with authoriser, expiry, register ID, and estimated exposure.
- [ ] Legacy-vendor retrofit artefact contains gap analysis (at least 15 missing controls for a tier-3 legacy vendor), retrofit playbook, vendor negotiation posture, partial-retrofit residual carry, and exit posture.
- [ ] Architect-legal-procurement handoff artefact enumerates each slice's ownership and walks at least one specific illustrative control through the full handoff (architect → legal → procurement → vendor → escalation → executed schedule).
- [ ] Every unverified citation — SR 23-4 phase names, EU AI Act article number, sector-rule identifier, incident-reporting timeline — is marked `<!-- needs-research: ... -->`. No invented article numbers, timelines, or sector-rule specifics.

## Stretch goals

- **Draft the catalog-version-migration playbook.** Assume the catalog is at v1.0.0 at authoring time; two versions later (v1.2.0 with a materially strengthened IP-indemnity control and a new supply-chain evidence tier for tier-3), how does the enterprise migrate existing executed contracts? Walk: renewal-window migration (chapter 05 renewal review), mid-cycle amendment where risk warrants, deferred migration with rationale, the substrate binding on catalog version per executed contract.
- **Compose with the enterprise's cyber-insurance and D&O-insurance posture.** Sketch how the OP-06 insurance control shape composes with the enterprise's cyber-insurance and directors-and-officers-insurance posture — where the vendor's insurance sits, where the enterprise's carrier sits, where the seams are (subrogation, notice, first-payer / secondary-payer). This is where enterprise legal and risk-management joins the drill.
- **Draft the sector-schedule composition for one specific sector.** Pick HIPAA business-associate agreement (BAA) for scenario (B), or the FedRAMP schedule for a federal-customer overlay in any scenario. Sketch how the AI-governance addendum composes with the sector schedule — where the schedule overrides, where the addendum overrides, where the composition is ambiguous and needs precedence resolution.
- **Sketch the substrate representation of contract state.** Author the vendor-register field shape (chapter 05) that carries the executed contract's control-by-control state — which controls landed, which had carveouts, which have residual carries, which are up for renewal amendment. This is the machine-readable input to the operating rhythm's queries.
- **Draft the vendor-facing summary of AI-specific commitments.** In one page, author a summary the vendor's account team can share internally to align the vendor's operations to the enterprise's AI-specific expectations — the summary is not the contract; it is the operating summary that helps the vendor's product, engineering, and safety teams understand what the enterprise's contract requires so they can build against it. This is the "why we need the vendor's cooperation, not just their signature" artefact.
