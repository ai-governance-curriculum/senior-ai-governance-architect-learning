# Vendor tiering — the four dimensions and the tier definitions

## Why this chapter exists

Every subsequent artefact in the programme keys off a tier. The due-diligence questionnaire (chapter 03) has a tier-specific length and depth. The contract-template control set (chapter 04) has tier-mandatory versus tier-optional clauses. The ongoing-monitoring schedule (chapter 05) has a tier-scaled cadence. The supply-chain evidence contract (chapter 07) has a tier-scaled minimum SLSA level and evidence bundle. If the tier is wrong, everything downstream is wrong at the same time — the questionnaire is over-long for a low-risk vendor and under-long for a high-risk one, the contract carries clauses the enterprise did not need or lacks clauses it did, the monitoring cadence is either wasteful or blind.

Tiering also sits at a political seam. Procurement wants a vendor onboarded quickly. The sponsoring product team wants the vendor in production before the quarterly launch. The vendor's account manager wants a faster path than tier-4 delivery. The head of AI wants a tiering discipline that survives contact with all three. The architect's design is what makes the discipline survive: an explicit dimension-by-dimension score, a machine-scored heuristic that is defensible and reproducible, a small named exceptions surface with an approving executive, and a re-tiering cadence that catches vendors whose risk has drifted after onboarding.

This chapter designs the tiering scheme. Four dimensions (data sensitivity, decision materiality, replaceability, jurisdictional exposure) with a defined scoring rubric per dimension; four tiers with definitions written to be applied by a non-architect; a small set of *auto-tiering rules* (a vendor with property X is automatically at least tier-N); a tier-appropriate DDQ / contract / monitoring bundle stub that chapters 03–05 fully populate; and an explicit re-tiering trigger set. Exercise-01 walks the drill against a real vendor list.

## What the tier is, structurally

A tier is *the enterprise's canonical statement of the depth of governance discipline the vendor receives*. It is:

- **A single ordinal.** Tier-1, tier-2, tier-3, tier-4 (the four-tier shape used consistently across this track; the enterprise can rename).
- **A function of four dimensions.** Data sensitivity, decision materiality, replaceability, jurisdictional exposure. Each dimension has a defined scale; the tier is derived from a combination-rule the architect designs.
- **A binding contract with the vendor and with the enterprise.** The tier drives which DDQ the vendor completes, which contract clauses land, which monitoring cadence attaches, and which incident-notification and evidence-access rights the vendor concedes.
- **A machine-readable field on the vendor register.** The tier is a first-class field, queryable by the operating rhythm.
- **A re-evaluable state.** The tier is not fixed at onboarding; drift, incidents, scope changes, and the periodic cadence re-open it.

## What it is not

- **A vendor-quality judgement.** The tier does not say the vendor is good or bad; it says the enterprise's exposure to the vendor is high or low. A tier-4 vendor may be an excellent partner — the tier reflects what happens *if the vendor fails*, not whether the vendor is trustworthy.
- **A cost-of-onboarding proxy.** Tier drives depth of diligence but the depth is proportional to risk, not to procurement effort. Under-tiering to save onboarding time is the failure this chapter's discipline prevents.
- **The enterprise's TPRM tier verbatim.** The AI tier composes with the enterprise's TPRM tier; the two are not always the same. A vendor may be TPRM-tier-3 (from information-security-and-privacy exposure) but AI-tier-4 (from decision materiality). The AI tier is a *specialisation* of TPRM, not a replacement.
- **A fixed input to procurement.** The tier is an input to procurement's onboarding workflow, not a decision procurement gets to override.

## The four dimensions

The tiering scheme uses four dimensions. The dimensions are chosen because they cover the risk shape the AI programme is designed to manage: what happens if the vendor mishandles data, what happens if the vendor's outputs are wrong, what happens if the enterprise needs to leave, and what happens if the vendor's or enterprise's jurisdiction changes.

### Dimension 1 — Data sensitivity

*What class of enterprise data flows to the vendor, and what is the enterprise's exposure if the vendor mishandles it?*

Scoring rubric (0–3):

- **0 — Public / non-personal enterprise data only.** Public marketing corpora, published enterprise documentation, benchmark data with no PII, synthesised or fully de-identified corpora certified to the enterprise's de-identification standard.
- **1 — Enterprise-confidential non-personal data.** Internal documentation, technical designs, non-sensitive telemetry, aggregated statistics.
- **2 — Personal data (regular categories) or regulated non-personal data.** Customer names, contact info, transaction metadata, non-special-category personal data under GDPR / CCPA, non-regulated PHI, financial-account metadata below the sector's sensitivity threshold.
- **3 — Special-category personal data or highly regulated data.** Health data under HIPAA, financial-account credentials, sensitive personal data under GDPR Article 9 (health, race, biometrics, sexual orientation, political / religious / trade-union data), children's data under COPPA / GDPR Article 8, sector-sensitive data (attorney-client, priest-penitent, security-classified, export-controlled).

Notes:

- The score reflects the *classes of data the vendor will process*, not the data's aggregate volume. Ten records of Article-9 data is tier-3 material; a million records of already-public data is not.
- The score reflects what actually flows to the vendor in the intended use, not the theoretical possible flow. If the vendor's endpoint is contractually forbidden from receiving customer data (an internal-only fine-tuning vendor operating on synthetic corpora), the score is 0 even though the vendor is technically an API.
- For inference-time flow to a foundation-model provider, the score reflects the classes of data in the enterprise's typical prompt content, including retrieved context (RAG passages). Prompt content is often under-scored at onboarding because the sponsoring team's initial demo used only sample data.

### Dimension 2 — Decision materiality

*What is the enterprise's exposure if the vendor's outputs are wrong, biased, or unavailable?*

Scoring rubric (0–3):

- **0 — Advisory, non-user-facing.** The vendor's outputs inform an internal-only workflow; a wrong output is caught before it leaves the enterprise; no customer is affected.
- **1 — User-facing but reversible; low individual harm.** The vendor's outputs are visible to end users but not consequential — a code-suggestion tool the developer accepts or rejects; a marketing-copy generator with human sign-off before publication; a summarisation tool where the summary is reviewed.
- **2 — Consequential to individual customers or subjects.** The vendor's outputs materially affect a customer's experience, a decision made about a customer, or a business outcome — a customer-service chatbot fielding refund requests; a lead-scoring tool influencing sales prioritisation; a triage tool routing tickets to a human agent.
- **3 — Consequential decisions with legal, financial, safety, or fundamental-rights impact.** The vendor's outputs are (or directly drive) decisions on credit, employment, insurance, healthcare, benefits eligibility, education, law enforcement, immigration, or safety-critical operations. The class of decision an EU AI Act "high-risk" designation attaches to; the class of decision a US state AI Act (Colorado SB24-205 and successors) attaches consumer-protection obligations to; the class of decision Article 22 GDPR restrictions attach to.

Notes:

- Where the vendor is a component (a guardrail, an observability platform), the materiality is the *materiality of the decision the enterprise makes with the vendor's component in the loop* — not the vendor's component in isolation. A guardrail vendor on a tier-3 decision path is a tier-3 material component.
- Advisory outputs that are *labelled advisory but routinely deferred to* score higher than their label suggests. A "human-in-the-loop-decision" workflow where the human accepts the AI's recommendation 98% of the time is materially a tier-3 decision. The enterprise's actual practice, not the workflow's label, is what scores.

### Dimension 3 — Replaceability

*How difficult would it be to migrate off the vendor, and how long would migration take?*

Scoring rubric (0–3):

- **0 — Commodity substitutable.** Two or more equivalent vendors on the enterprise's approved list; the vendor's API is a de facto standard; migration is measured in days.
- **1 — Substitutable with effort.** Alternative vendors exist; migration involves data transformation, prompt / integration rewrites, or evaluation-set re-baselining, but no capability the enterprise depends on is lost. Migration is measured in weeks.
- **2 — Substitutable with material rework.** Alternative vendors exist but the vendor's specific capabilities, model behaviour, tuning, or integration are load-bearing; migration involves capability-parity work, re-tuning, or partial re-architecture. Migration is measured in months.
- **3 — Effectively irreplaceable within a defensible horizon.** No alternative vendor provides the required capability; the vendor's model has been fine-tuned or specialised in a way the enterprise cannot readily reproduce; the vendor's dataset is unique; the vendor's regulatory posture (a specific certification, a specific jurisdiction) is not matched. Migration is measured in years or is materially blocked.

Notes:

- Replaceability is a *point-in-time* score. A vendor that is score-3 today may become score-1 in eighteen months as the market matures; a vendor that is score-1 today may become score-3 if the enterprise's usage pattern deepens (fine-tuning on the vendor's platform; heavy prompt-engineering to the vendor's specific model quirks; integration into critical-path workflows).
- Enterprises consistently under-score replaceability for foundation-model providers because "there are several frontier providers." At tier-4 scrutiny, replaceability is not just "is there another frontier vendor" — it is "can the enterprise migrate prompts, few-shot patterns, safety configurations, evaluation baselines, fine-tunes, and customer-facing behaviour to another frontier vendor in a defensible period." That answer is often at least score-2, not score-0.

### Dimension 4 — Jurisdictional exposure

*Where does the vendor operate, where does the enterprise's data flow, and what regulatory regimes attach?*

Scoring rubric (0–3):

- **0 — Single-jurisdiction, matched.** Vendor operates in the same jurisdiction as the enterprise's operations that consume the vendor; data does not cross a border; no external transfer regime applies.
- **1 — Multi-jurisdiction within a single regulatory bloc.** Vendor operates in the EU / EEA when the enterprise's data subjects are EU / EEA; vendor operates across US regions when data is US-domestic; no cross-bloc transfer.
- **2 — Cross-bloc transfer with an established transfer mechanism.** EU–US data flows under the EU–US Data Privacy Framework and Standard Contractual Clauses; UK international data transfer with the UK IDTA; the transfer is happening and the enterprise's DPA reflects the mechanism.
- **3 — Cross-bloc transfer with contested or novel regime, or a sanctions / export-control exposure, or a jurisdiction with material regulator ambiguity.** Data flow to a jurisdiction that has an active adequacy question; a jurisdiction subject to sanctions or export control constraints on model access; a jurisdiction whose AI-specific regulations differ materially from the enterprise's home regime (China's Generative AI Measures, EU AI Act general-purpose model obligations, Korean AI Framework Act); a jurisdiction where the vendor's compliance posture is being contested by a regulator.

Notes:

- Foundation-model providers frequently score at least 2 on this dimension because inference traffic reaches a US-hosted service from an EU-hosted enterprise, or vice versa. The programme's DPA and TIA (transfer impact assessment) discipline attaches at score 2 or higher.
- The jurisdictional score is re-opened by any material change in the transfer regime (Schrems III equivalent; a sanctions listing; a new AI-specific law in scope). Chapter 05's drift-triggered re-assessment enumerates.

## The tier-derivation rule

The four dimensions each yield a score of 0–3. The tier is derived from the combined score by the following rule, which the architect authors and pins:

```yaml
tier_derivation:
  version: 1.0.0
  inputs: [data_sensitivity, decision_materiality, replaceability, jurisdictional_exposure]

  # The default rule is the max-plus-floor rule:
  #   tier = max(dimension_scores) + floor(count(scores >= 2) / 2)
  # then clamped to [tier-1, tier-4] with an inverted numbering (higher = more risk).

  default_rule:
    id: max-plus-floor
    formula: |
      base   = max(scores)                     # base tier from the worst dimension
      lift   = 1 if count(s >= 2 for s in scores) >= 2 else 0
      tier_n = base + lift
      tier   = clamp(tier_n, 1, 4)             # tier-1..tier-4

  # Auto-tiering rules: a vendor with property X is automatically at least tier-N.
  auto_tiering:
    - id: A1
      description: >
        Any foundation-model provider (Anthropic, OpenAI, Google, Meta, xAI,
        Mistral, DeepSeek, or open-weight-hub distribution the enterprise
        relies on as its base-model source) whose outputs enter a
        customer-facing or regulated-decision path is at least tier-3.
    - id: A2
      description: >
        Any vendor processing special-category personal data (GDPR Article 9),
        PHI under HIPAA, or safety-critical operational data is at least tier-3.
    - id: A3
      description: >
        Any vendor whose outputs directly drive an EU AI Act Article 6 /
        Annex III high-risk decision, or an equivalent US state or sector
        "consequential decision" designation, is at least tier-4.
    - id: A4
      description: >
        Any vendor whose exit would require more than six calendar months
        to complete under the enterprise's realistic operating conditions
        (replaceability score 3) is at least tier-3.
    - id: A5
      description: >
        Any guardrail vendor on the hot path of a tier-3 or tier-4 system
        inherits the system's tier, capped at the tier-3 minimum.
    - id: A6
      description: >
        Any GRC-for-AI platform vendor holding the enterprise's authoritative
        AI-governance evidence is at least tier-3.

  # Manual override permitted only by written exception:
  override_policy:
    permitted_direction: up_only
    down_override: requires ai-accountable-executive sign-off; substrate log entry
    up_override: routine; documented in vendor register

  # Recompute cadence:
  recompute_triggers:
    - periodic: annual (all vendors); semi-annual (tier-3+); quarterly (tier-4)
    - event: vendor incident of severity >= 2
    - event: material vendor model / classifier / dataset version change
    - event: material change in vendor subprocessor list
    - event: jurisdictional-regime change (adequacy, sanctions, new AI-specific law in scope)
    - event: material change in enterprise's use pattern (new use case; higher-materiality decision path)
    - event: renewal minus 6 months
```

The default rule is one architectural choice; the enterprise may pin a different one. What matters is that the rule is *explicit*, *reproducible*, and *auditable* — not that any particular formula is right. Two enterprises with the same vendor set may pin different rules and both defend them.

Auto-tiering rules are the discipline that prevents obvious tier-4 vendors from being onboarded as tier-2. They should be tight, defensible, and evolvable. Chapter 05 walks the recompute triggers.

## The four tier definitions

Once the tier is derived, it binds to a bundle: a DDQ length, a contract-clause set, a monitoring cadence, an evidence expectation. Chapters 03–05 fully populate; the following is the scaffold each subsequent chapter fills.

### Tier-1 — routine

*A low-exposure AI vendor whose failure is contained within an internal workflow with no customer, regulated, or safety impact.*

Representative examples: an internal-only developer-productivity tool with no customer data; an internal knowledge-management search tool over public documentation; an AI-adjacent SaaS tool the enterprise consumes at low volume for internal experimentation.

Bundle:

- **DDQ.** Tier-1 short-form (baseline security, data-use, incident-notification). Reviewed by the enterprise TPRM function; AI governance signs off with a lightweight review.
- **Contract.** Standard master services agreement + standard DPA + AI-governance addendum with baseline clauses (data-use limits, incident-notification, exit-portability minimum).
- **Monitoring.** Annual re-attestation; incident-driven review; renewal review.
- **Evidence expectation.** SOC 2 (or equivalent) attestation currency; vendor model-card / usage-guide if applicable.

### Tier-2 — standard

*A vendor whose outputs affect enterprise operations but not consequential decisions; or whose data flows include personal data but not special-category data.*

Representative examples: an AI-assisted marketing-copy generator with human sign-off before publication; a summarisation tool over internal meeting recordings; a low-materiality AI-observability platform used for prototype telemetry.

Bundle:

- **DDQ.** Tier-2 standard-form (baseline plus data-handling depth, model-version disclosure, subprocessor list, evaluation methodology summary).
- **Contract.** MSA + DPA + AI-governance addendum with data-use limits explicit, model-version-notification clause, incident-notification obligation with defined timeline, evidence-access rights for SOC 2 letters and vendor model card.
- **Monitoring.** Semi-annual re-attestation; incident-driven review; drift-triggered review on material vendor change; renewal review.
- **Evidence expectation.** SOC 2 currency; vendor model-card and any published evaluation summary; DPA schedule of subprocessors current.

### Tier-3 — heightened

*A vendor whose outputs are material to customer-facing operations or regulated decisions, or whose data flows include personal or regulated data, or whose replaceability is material.*

Representative examples: a foundation-model provider serving customer-facing production traffic; a guardrail vendor on the hot path of tier-3 systems; a customer-service chatbot vendor fielding refund and billing decisions; an AI-observability platform holding the enterprise's authoritative telemetry for regulator-facing systems; a GRC-for-AI platform vendor.

Bundle:

- **DDQ.** Tier-3 long-form (baseline plus tier-2 plus: safety-evaluation methodology and results, red-team practice, model-training-data disclosure to the extent disclosed, model-update cadence and notification, incident history, sub-service organisation controls, licence terms on outputs, data-use around fine-tuning corpora, human-oversight support, guardrail configurability where relevant).
- **Contract.** MSA + DPA + AI-governance addendum plus: model-version-pinning where offered, evaluation-access rights (right to run the enterprise's evaluation suite against the vendor's model on the enterprise's cadence), incident-notification with defined severity taxonomy and hour-scale timeline, evidence-access rights (SOC 2 audit letter, ISO 42001 audit letter where available, third-party safety-audit letters where available), data-use exclusion from training and evaluation without explicit opt-in, exit / portability clause with defined transition assistance and retention posture, subprocessor consent-and-notification, sanctions-and-export-control cooperation.
- **Monitoring.** Quarterly re-attestation on specified fields; incident-driven review with defined SLAs; drift-triggered review; renewal review with minimum 6-month pre-window; supply-chain evidence re-verification on artefact updates (chapter 07).
- **Evidence expectation.** All tier-2 plus: safety-evaluation summaries current; model-version and system-card currency; supply-chain evidence bundle for any artefact ingested (chapter 07); vendor incident history and post-mortem summaries where published; annual attestation to contract-defined controls.

### Tier-4 — critical

*A vendor whose failure is a material enterprise risk. Consequential decisions with legal, safety, or fundamental-rights impact; irreplaceable within a defensible horizon; or a vendor whose failure would materially impair the enterprise's regulator obligations.*

Representative examples: a foundation-model provider whose model drives Article-6 / Annex-III EU AI Act high-risk decisions or equivalent US state / sector "consequential decisions" (credit, employment, insurance, healthcare, education, benefits, law enforcement); a dataset provider supplying the corpus for a regulator-facing model where dataset withdrawal would strand the enterprise; a GRC-for-AI platform vendor whose failure would blind the enterprise's second-line function for the duration of a certification cycle; a sector-specialised model vendor with no substitute.

Bundle:

- **DDQ.** Tier-4 comprehensive (all tier-3 plus: fine-grained safety and evaluation disclosures, third-party safety-audit report currency, red-team report currency, sanctions-and-export-control posture, board-and-senior-management oversight arrangement at the vendor, financial-condition and going-concern basis, business-continuity and disaster-recovery specifics, insurance coverage on AI-specific exposures, vendor's own third-party AI risk programme).
- **Contract.** All tier-3 plus: independent-audit rights (on-site or remote assessor engagement) or SOC-2 sub-service-organisation right, regulator-cooperation obligation (vendor cooperates when enterprise's regulator requests documentation the vendor holds), minimum-notice for material change (model, subprocessor, ownership, key-personnel), termination-for-cause explicitly enumerated (safety incident above threshold, licence breach, transfer-regime failure, unauthorized model change on pinned version), transition-services agreement (TSA) shape pre-agreed for exit, financial protection appropriate to the exposure (indemnity, insurance, liability cap consistent with the exposure, guarantee where the vendor entity's balance sheet does not support), source-code / model-weights escrow where feasible and legally supportable, extended evidence-retention.
- **Monitoring.** Monthly monitoring on defined signals; quarterly deep-dive re-attestation; incident-driven review with hour-scale SLA; drift-triggered review with same-day mobilisation; renewal review with minimum 9-month pre-window and defined executive engagement; annual on-site (or remote-formal-equivalent) review; supply-chain evidence re-verification on every artefact update with same-day gating decision.
- **Evidence expectation.** All tier-3 plus: independent-audit reports; sub-service organisation controls SOC 2 or equivalent; vendor's own AI governance programme evidence; supply-chain evidence at SLSA L3 minimum for any artefact ingested; formal statement of continued regulator-compliance posture per applicable regime.

## The tiering worksheet — a schematic

The tiering exercise for a specific vendor lands as an artefact. The scoring per dimension, the auto-tiering rule triggers, the derived tier, and the sign-off are captured on a worksheet:

```yaml
vendor_tiering_worksheet:
  vendor_id: V-2027-0142
  vendor_display_name: "ExampleCo Fine-Tuning Platform"
  vendor_class: foundation-model-provider   # or guardrail, ai-observability, grc-for-ai, dataset
  scoring_at: 2027-03-01
  scored_by: analyst-A (level 15)
  reviewed_by: architect-B (level 50) — or head-of-ai-governance (level 60)

  intended_use:
    description: >
      Fine-tune vendor's 70B open-weight base model on our internal contact-centre
      transcripts. Serve fine-tuned model as customer-facing chat backend for the
      billing-support workflow.
    data_classes_flowing_to_vendor:
      - customer contact information
      - transaction / billing history
      - contact-centre transcripts (may contain incidental Article-9 disclosures)
    decision_context: >
      Chatbot fields refund and billing-adjustment requests up to $500 without
      human review; escalates above that threshold.
    replaceability_context: >
      Two alternative fine-tuning platforms in market. Migration would require
      re-tuning and re-evaluation; estimated 3-4 months of effort.
    jurisdictional_context: >
      Vendor hosted in US. Enterprise operates in US, UK, EU. UK / EU data flows
      to US via EU-US Data Privacy Framework + UK IDTA.

  dimension_scores:
    data_sensitivity:
      score: 3
      rationale: >
        Contact-centre transcripts may include Article-9 disclosures (health,
        family situation). Cannot reliably de-identify at scale in production
        flow.
    decision_materiality:
      score: 3
      rationale: >
        Automated financial decisions up to $500 with no human review meet
        the "consequential decision" designation under multiple US state AI
        acts and would be scrutinised as an Article-22 GDPR automated
        decision-making case in EU.
    replaceability:
      score: 2
      rationale: >
        Alternative platforms exist; the migration cost is material but
        bounded to a defensible horizon.
    jurisdictional_exposure:
      score: 2
      rationale: >
        Cross-bloc transfer with an established mechanism; enterprise DPA
        reflects it. No sanctions or export-control exposure at scoring time.

  derived_tier:
    formula: max-plus-floor
    base: 3
    lift: 1                     # count(scores >= 2) = 4, floor(4/2) = 2, capped at +1 for the lift
    tier_n: 4
    clamped_tier: tier-4        # A3 auto-tier: consequential-decision path

  auto_tier_triggers:
    - A1: applies (foundation-model provider on customer-facing path)
    - A2: applies (Article-9 data may flow)
    - A3: applies (consequential decision path)

  final_tier: tier-4

  bundle_pointer:
    ddq: templates/ddq-tier-4.yaml         # chapter 03
    contract_addendum: templates/contract-addendum-tier-4.md   # chapter 04
    monitoring_schedule: schedules/tier-4.yaml   # chapter 05
    supply_chain_evidence_profile: profiles/tier-4-artefact-ingest.yaml   # chapter 07

  signoff:
    tprm_seat: {name: SEAT-A, signed_at: 2027-03-03}
    ai_governance_seat: {name: SEAT-B, signed_at: 2027-03-04}
    ai_accountable_executive: {name: SEAT-C, signed_at: 2027-03-08}

  substrate_refs:
    tiering_worksheet_uri: substrate://vendor-register/V-2027-0142/tiering/2027-03-01
    signature_uri: substrate://signatures/vendor-register/V-2027-0142/tiering/2027-03-01
```

The worksheet is a lightweight artefact. It exists so the tiering decision is *reproducible* and *auditable* — the reviewer can trace the tier back to the dimension scores; the risk register can cite the worksheet; the third-line audit can sample the worksheets and check the derivation.

## Where tiering interacts with the enterprise TPRM function

The AI tiering is composed with, not parallel to, the enterprise's TPRM tier. Two possible arrangements the architect defends:

- **Parallel-fields arrangement.** The enterprise vendor record carries both a TPRM tier (populated by the enterprise TPRM function per its criteria) and an AI tier (populated by this programme per this chapter). Onboarding and monitoring operate against the *higher* of the two, per artefact type — e.g., the DDQ is the union of TPRM's questionnaire and the AI DDQ at the higher tier; the contract clauses are the union.
- **Max-tier arrangement.** The enterprise vendor record carries a single tier that is the max of TPRM tier and AI tier; the operating discipline (DDQ length, contract clauses, monitoring cadence) is the AI-composed one at that max tier.

Enterprises that already have a mature TPRM function tend to prefer the parallel-fields arrangement because it keeps ownership boundaries clear. Enterprises building both together tend to prefer the max-tier arrangement because it collapses the operating discipline into one workflow. Neither is architecturally right or wrong; the architect picks and documents.

## The re-tiering discipline

Tier drift is real. A vendor at tier-2 at onboarding routinely climbs to tier-3 or tier-4 within the first year of the engagement because the sponsoring team deepens the use case, moves the vendor onto a hotter decision path, or expands the data classes flowing to the vendor. A vendor at tier-3 sometimes descends to tier-2 (the enterprise builds an internal capability that substitutes for the vendor's specialisation and the migration completes). Chapter 05 walks the recompute cadence and triggers; the tiering scheme is designed to be re-executed cheaply.

Two re-tiering practices the discipline enforces:

- **Use-pattern change triggers re-tier.** Any change in the vendor's use pattern (new use case; new data class; new geography; new decision-materiality) triggers a re-scoring of the four dimensions. The sponsoring team is required to disclose. The AI governance function has a periodic sweep as a backstop.
- **Vendor-side change triggers re-tier.** Vendor-side changes captured by chapter 05's monitoring signals (material model version change, subprocessor list change, jurisdictional change, ownership change, incident of severity >= 2) trigger re-scoring on the affected dimensions.

## The six invariants the tiering scheme holds

**Invariant 1 — the tier is derivable from documented dimension scores.** No vendor is at a tier by executive fiat without a scoring rationale on the worksheet. Failure mode: a vendor sits at tier-2 because the sponsoring executive said so; the tier survives a rotation of executives; nobody knows why the vendor is tier-2; the risk register shows tier-2 residual but the actual exposure is tier-4.

**Invariant 2 — auto-tiering rules are non-overridable downward without executive sign-off.** A vendor auto-tiered to at least tier-3 by A1–A6 cannot be dropped below the floor by a routine reviewer. Failure mode: the sponsoring team pressures the analyst to "just check the box lower to get through onboarding faster"; the analyst does; the vendor onboards at tier-2 with tier-4 exposure.

**Invariant 3 — the tier is a first-class field on the vendor register and queryable.** Queries like "how many tier-4 vendors are we exposed to today" and "which tier-3 vendors have DDQ freshness > 12 months" run without a fire drill. Failure mode: the tier is in a Word file per vendor; the register is a spreadsheet a compliance analyst maintains; nobody queries the population.

**Invariant 4 — the tier binds the bundle.** DDQ length, contract clauses, monitoring cadence, and evidence expectations are keyed off the tier automatically; there is no way to onboard a tier-4 vendor with a tier-2 DDQ. Failure mode: the tier is set correctly but the operating workflow does not enforce the bundle; the vendor onboards with an inconsistent set of artefacts.

**Invariant 5 — the tier is re-openable on defined triggers.** Chapter 05's triggers are wired to actually re-open worksheets. Failure mode: the triggers exist in policy but the workflow never fires; the tier at onboarding is the tier forever.

**Invariant 6 — the tiering scheme is a versioned artefact.** The dimension rubrics, the derivation rule, and the auto-tiering rules are versioned; every worksheet references the version used; scheme updates go through second-line review and produce a governance-workflow log event. Failure mode: the analyst pool uses two informally different rubrics; scores are inconsistent; the same vendor lands differently depending on who scored.

## Two failure modes to design against

**Failure mode 1 — the empathetic under-scoring loop.** The analyst scoring a new vendor is under pressure from the sponsoring team, from procurement's cycle-time target, and from the vendor's account manager. Every dimension has a defensible reading that is one score lower than the honest reading. Data sensitivity: "the transcripts are usually not Article-9; score-2 not score-3." Decision materiality: "the chatbot escalates above $500; score-2 not score-3." Replaceability: "there are three alternative vendors on the market; score-1 not score-2." Jurisdictional: "the DPF is established; score-1 not score-2." The vendor lands at tier-2 with a bundle that never asks the vendor the hard questions. The fix is architectural: the rubric text is written to make the honest reading the easy reading; the auto-tiering rules catch obvious tier-3+ cases regardless of dimension scoring; second-line review samples 100% of tier-3-or-above initial scoring and a defined percentage of tier-2 initial scoring for calibration.

**Failure mode 2 — the frozen tier.** The vendor onboards at tier-2, the tier survives without re-evaluation, the sponsoring team quietly deepens the use case, the vendor's outputs slide onto a customer-facing consequential path, no worksheet is re-run, the DDQ is stale, the contract lacks tier-3 clauses, the monitoring is annual. The failure surfaces at incident, audit, or renewal. The fix is architectural: the re-tiering triggers in chapter 05 are wired to fire; the operating rhythm queries the register for vendors whose use pattern has changed since scoring; the annual sweep re-runs all worksheets at the minimum; every material change in the sponsoring team's usage produces a re-tier prompt in the workflow.

## Summary

The tiering scheme is what makes everything downstream in the programme risk-proportional. Four dimensions (data sensitivity, decision materiality, replaceability, jurisdictional exposure) each score 0–3; a documented derivation rule (max-plus-floor as a default) converts scores to a tier-1..tier-4 ordinal; a small set of auto-tiering rules catches obviously high-risk vendors regardless of scoring; each tier binds a bundle of DDQ, contract clauses, monitoring cadence, and evidence expectations that chapters 03–07 fully populate. The scheme composes with the enterprise's TPRM tiering (parallel-fields or max-tier); it is versioned; it is re-executed on defined triggers. Six invariants (derivable, auto-tier respected, queryable, bundle-binding, re-openable, versioned) and two failure modes (empathetic under-scoring, frozen tier) shape the design. Exercise-01 walks the drill. The next chapter designs the tier-attached due-diligence questionnaires.
