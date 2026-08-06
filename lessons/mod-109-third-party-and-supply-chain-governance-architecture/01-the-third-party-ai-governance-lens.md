# The third-party AI governance lens — why vendor governance for AI is a distinct architectural concern

## Why this chapter exists

Ask an enterprise's third-party risk management (TPRM) function what it does about the frontier-model provider the customer-facing product now calls twenty million times a month, and one of two answers arrives.

The first answer: "That vendor went through our standard onboarding — a SIG-Lite questionnaire, a SOC 2 Type II review, an information-security architecture review, a data-processing addendum, a standard master services agreement. The vendor is on our approved-vendor list. There is a renewal review scheduled for Q3." The programme is doing what it does for every SaaS vendor. It is not doing anything about the fact that the vendor's outputs are the *decisions the enterprise is making in front of a regulator*, that the model behind the API has been silently updated three times since onboarding, that the training data the vendor used is not disclosed, that the evaluation set the vendor benchmarks against does not overlap with the enterprise's evaluation set, and that the contract's data-use clause is silent about whether the vendor's provider (a hyperscaler) sees enterprise data during inference.

The second answer: "We have an AI vendor questionnaire. The head of AI signs off on new AI vendors. Every vendor gets it." That is closer, but the questionnaire is the artefact of an ad-hoc committee response to an incident three quarters ago, no one has re-issued it, and the sign-off is a rubber stamp because the head of AI has no substrate to interrogate the answers against.

Neither answer is what the level-50 architect ships. The architect owns the *programme* — the tiering that decides which vendors get which depth of scrutiny, the due-diligence questionnaire attached to each tier, the contract-template controls that make evidence access enforceable, the ongoing-monitoring schedule that catches vendor-side drift, the supply-chain evidence contract that gates every third-party model artefact, and the coordination interfaces to procurement, legal, information security, sourcing, and the enterprise TPRM function. The programme is what turns "we have an AI vendor questionnaire" into "the enterprise can defend, at speed and with evidence, its exposure to any AI vendor in the population, at any time."

This chapter names the failure the programme prevents, defines the vendor typology the architect designs against, positions the Federal Reserve's SR 23-4 (Interagency Guidance on Third-Party Relationships: Risk Management) as the process-shape reference the AI-specific programme extends, and pins the invariants and failure modes that shape the module's remaining chapters. Chapters 02–07 design the substrate: tiering (chapter 02), the DDQ (chapter 03), contract controls (chapter 04), ongoing monitoring (chapter 05), the federal acquisition shape adapted for enterprise (chapter 06), and the supply-chain evidence slice for third-party artefacts (chapter 07). Every subsequent chapter is an instance of the programme this chapter fixes.

## What "third-party AI governance" is, structurally

The third-party AI governance programme is the enterprise's *authored design* of how it accepts, monitors, and retires dependencies on external AI vendors and AI-artefact suppliers. It consists of:

- **A vendor typology** — a controlled taxonomy of the classes of AI-relevant vendor the enterprise relies on, each class with a distinct risk shape and evidence expectation.
- **A tiering scheme** — the criteria that place each vendor in a risk tier (chapter 02).
- **A due-diligence questionnaire per tier** — the pre-contract diligence artefact whose questions match the tier's risk (chapter 03).
- **A contract-template control set** — the standard-form clauses procurement and legal use to convert the DDQ's expectations into enforceable obligations (chapter 04).
- **An ongoing-monitoring schedule** — periodic re-attestation, drift-triggered re-assessment, vendor-incident routing, renewal review (chapter 05).
- **A supply-chain evidence contract** — CycloneDX ML-BOM, SPDX 3.0 AI profile, SLSA provenance, Sigstore signatures — that gates ingestion of third-party model / dataset artefacts (chapter 07).
- **A coordination model** — declared handoffs with procurement, legal, information security, enterprise TPRM, the AI-accountable executive, and the level-35 AI infrastructure security peer.
- **A machine-readable vendor register** — the population of vendors, their tiers, their evidence freshness, their monitoring cadence, and their in-flight exceptions, queryable by the operating rhythm.

## What it is not

- **The enterprise TPRM function.** TPRM is the enterprise's cross-cutting third-party function; it typically owns the SIG questionnaire, the standard vendor onboarding workflow, and the vendor-risk register. The AI programme *composes* with TPRM — the AI-specific tier extends the TPRM tier, the AI-specific DDQ appends to the TPRM DDQ, the AI-specific contract clauses are addenda to the master agreement. Standing up a parallel AI-only TPRM function is invariant-1 failure below.
- **Legal contract drafting.** Chapter 04 designs the control *shape* the contract templates carry. Legal drafts the enforceable clauses. The architect's deliverable is the control library the legal drafter references; the legal drafter's deliverable is the enforceable text.
- **Procurement's approved-vendor list.** Procurement operates the list; the programme sets the criteria for being on it (tier plus DDQ discharge plus contract clauses plus monitoring cadence).
- **A one-time onboarding gate.** A vendor accepted at T=0 with tier-3 discipline is not a tier-3 vendor forever; the vendor's model updates, service updates, subprocessor list, jurisdiction, and incident history all evolve. Chapter 05's ongoing-monitoring schedule is what keeps the classification honest.
- **Only about frontier-model providers.** The programme covers every vendor whose failure the enterprise's AI systems inherit: the frontier-model provider, the guardrail vendor, the observability platform, the GRC-for-AI tool, the dataset supplier, the evaluation-set vendor, the fine-tuning platform, the vector-store SaaS. Each has a distinct risk shape; each fits the tiering scheme.
- **Sufficient by itself.** The programme is one architectural layer alongside the control library (mod-102), policy hierarchy (mod-103), AIMS (mod-105), risk taxonomy (mod-106), assurance architecture (mod-107), evidence architecture (mod-108), and post-market surveillance (mod-110). Compose or fail.

## The vendor typology the programme designs against

Five classes cover most of the AI-relevant vendor population. Each class has a characteristic risk shape and a characteristic evidence expectation. Chapters 02 and 03 attach tiering and DDQ specifics to each.

### Class 1 — foundation-model providers

Vendors that provide access to a general-purpose model via an API or a hosted deployment, or that publish open-weight checkpoints under a licence: Anthropic, OpenAI, Google, Meta, xAI, Mistral, and open-weights-model publishers (Hugging Face hub distributors of Llama, Mistral, Falcon, Qwen, DeepSeek, and successor families).

Characteristic risk shape:

- **Decision materiality is often high.** The vendor's outputs are the enterprise's decisions in front of the customer, the regulator, and the courts.
- **Data-use surface is subtle.** Whether prompts and completions are used for training, evaluation, abuse monitoring, retention, or research is contract-defined and can change with vendor policy updates.
- **Model version drift is intrinsic.** The model behind the same API endpoint or same version name has typically been silently updated over a horizon shorter than the enterprise's contract cycle. Some vendors offer pinned versions or dated-snapshot endpoints; some do not.
- **Replaceability is asymmetric.** Two providers' models are not drop-in substitutes even when both are frontier-grade — prompt patterns transfer imperfectly, evaluation results differ, latency and cost differ, safety-tuning differs.
- **Jurisdictional exposure follows the vendor's hosting geography and the enterprise's data-flow architecture.** EU–US data-transfer regimes, the vendor's US-federal-cloud posture, and the vendor's home jurisdiction all matter.

Characteristic evidence expectation:

- Vendor-published model card / system card / usage guide / responsible-use guide.
- Vendor's SOC 2, ISO/IEC 27001, and (where available) ISO/IEC 42001 attestations.
- Vendor's data-processing addendum and the specific data-use terms.
- Vendor's evaluation-set disclosure and safety-evaluation summary (where published).
- Vendor's incident-notification commitment and past-incident history.
- Where the vendor offers audit rights or SOC 2 sub-service treatment, the applicable letter.
- For open-weight distributions: the checkpoint hash, licence, and any provenance attestation the distributor publishes.

### Class 2 — guardrail / safety vendors

Vendors that provide runtime safety filtering, jailbreak detection, prompt-injection defence, or content-classification: examples include Lakera, Protect AI (Recon / Guardian), NeMo Guardrails, Llama Guard as a hosted service, or a specialised sector-specific safety vendor.

Characteristic risk shape:

- **The guardrail is a *decision-inflight* component.** A failure of the guardrail is a failure of the enterprise's safety envelope. If the guardrail lets an injection through or drops a safe request, the enterprise inherits the outcome.
- **Data-flow architecture places the guardrail on the hot path.** Latency, availability, and failure-mode behaviour are safety-critical.
- **The guardrail's own classification models drift.** The vendor's classifiers may be updated with new taxonomies, new thresholds, or new blocked-topic lists without notice, changing false-positive / false-negative behaviour.

Characteristic evidence expectation:

- Vendor's classifier evaluation methodology and disclosed metrics on the classes the enterprise relies on.
- Vendor's data-handling posture (do requests / responses / classifier decisions get retained; if so, for what).
- Vendor's SLA on availability and latency; failure-mode behaviour (fail-open, fail-closed, degrade).
- Vendor's incident-notification commitment on classifier updates.

### Class 3 — AI-observability / evaluation vendors

Vendors that provide LLM-observability, evaluation-orchestration, red-team-as-a-service, or model-monitoring: examples include LangSmith, Arize, Fiddler, WhyLabs, Weights & Biases, Braintrust, HumanLoop, or sector-specific evaluation providers.

Characteristic risk shape:

- **The vendor ingests the enterprise's prompts and completions.** The observability data is the trace of the enterprise's user interactions; the vendor's data-handling posture is a privacy and confidentiality question.
- **The vendor's dashboards influence enterprise decisions.** A miscalibrated hallucination-rate metric feeds directly into the enterprise's operating decisions and the second-line's assurance judgments.
- **Availability of the observability platform affects the enterprise's operational visibility.** An outage of the observability vendor is a blind spot for the enterprise, not a failure of the enterprise's own AI system.

Characteristic evidence expectation:

- Vendor's data-retention posture on prompts, completions, and derived telemetry.
- Vendor's SOC 2 and, where applicable, HIPAA / PCI-DSS attestations for the classes of data the enterprise sends.
- Vendor's methodology for any evaluation metric the enterprise depends on for gate decisions.

### Class 4 — GRC-for-AI platform vendors

Vendors that provide an AI governance / GRC platform: examples include Credo AI, Holistic AI, Fairly AI, Trustible, Modulos, Monitaur, and adjacent sector-specific platforms. Some enterprise GRC vendors (Archer, ServiceNow GRC, LogicGate, MetricStream, OneTrust) also ship AI-governance modules.

Characteristic risk shape:

- **The platform holds the enterprise's authoritative AI governance evidence.** Vendor lock-in matters — a switch is not free.
- **The platform's schema drives what evidence gets captured.** If the platform's shape does not match the enterprise's evidence architecture (mod-108), the substrate silently deforms to fit the platform.
- **The vendor's roadmap drives the enterprise's implementable coverage of new regulations.**

Characteristic evidence expectation:

- Vendor's data-export capability — the enterprise's governance data has to remain the enterprise's on exit.
- Vendor's schema alignment with the enterprise's evidence architecture (OSCAL support, ISO/IEC 42001 Annex A structure, EU AI Act Article 11 template support).
- Vendor's SOC 2 and, for regulated sectors, sector-specific attestations.
- Vendor's roadmap and past-cadence of regulatory-update delivery.

Chapter is picked up in mod-111 (GRC-for-AI platform and toolchain architecture) with the platform-selection depth; this chapter and this module cover the *vendor-governance* slice.

### Class 5 — dataset providers

Vendors that supply training or evaluation data — labelled datasets, benchmark evaluation sets, human-preference datasets, red-team prompt corpora, licensed-content datasets (news, books, code repositories, images). Examples include Scale AI, Surge AI, Toloka, Appen, licensed-content aggregators (Getty for images, Reuters for news, GitHub-licensed code), academic dataset distributors, and specialised sector datasets (health-record corpora under DUA, financial-tick datasets).

Characteristic risk shape:

- **Licence and provenance risk.** Whether the vendor had the right to license the data to the enterprise, and whether the licence permits the enterprise's downstream use (commercial training, redistribution of derived models, publication of evaluation results).
- **PII / sensitive-attribute risk.** Whether the dataset carries personal data or sensitive attributes; whether the vendor's disclosed provenance is what the DPA would require.
- **Contamination risk.** Whether the evaluation set the vendor sells overlaps with training data — a contaminated evaluation set produces optimistic metrics the enterprise then relies on.
- **Labour-practices risk.** Whether the vendor's labelling workforce is treated in ways consistent with the enterprise's supplier-code-of-conduct.

Characteristic evidence expectation:

- Dataset provenance record (SPDX 3.0 with the AI / dataset profile) or equivalent structured metadata.
- Licence terms, including permitted downstream uses and any redistribution constraints.
- PII / sensitive-attribute assessment.
- For evaluation sets: the vendor's contamination-check methodology.
- For labelled datasets: the vendor's labour-practices attestation and any relevant industry certifications.

## SR 23-4 as the process-shape reference

The Federal Reserve, OCC, and FDIC published the *Interagency Guidance on Third-Party Relationships: Risk Management* in June 2023 (commonly cited as SR 23-4 in Fed banking supervisory correspondence; the OCC and FDIC issued the same guidance under their own numbering). The guidance replaced the earlier agency-specific guidance (Fed SR 13-19, OCC Bulletin 2013-29, FDIC FIL 44-2008) with a single interagency shape.

SR 23-4's process shape has five phases:

- **Planning.** Before engaging a third party, plan against the business need, the risks, the tiering, and the required due diligence.
- **Due diligence and third-party selection.** Assess the third party's ability to perform the activity; scale the diligence to the risk. The guidance lists content areas (strategies and goals, legal and regulatory compliance, financial condition, business experience, qualifications, information security, operational resilience, and others).
- **Contract negotiation.** Reflect the risk in contract terms — SLAs, subcontracting controls, right-to-audit, incident-notification, information-security, data-ownership, exit / termination.
- **Ongoing monitoring.** Monitor the third party's performance and risk against the terms and expectations, on a cadence proportional to risk.
- **Termination.** Manage the exit — transition of data, records, and residual obligations; retention as required.

Two design decisions in SR 23-4 the AI programme inherits:

- **Risk-proportional diligence.** The depth of due diligence and the frequency of ongoing monitoring scale with the risk of the third-party activity, not the vendor's size or spend. This is what tiering (chapter 02) encodes.
- **Board-and-senior-management accountability.** The board and senior management remain accountable for the risks of third-party activities. The AI-accountable executive at the enterprise is the analogue for AI-specific vendor risk.

Two AI-specific extensions the SR 23-4 shape does not natively carry, which the module makes explicit:

- **Model-version drift as a first-class monitoring signal.** Classical third-party risk monitors the vendor's control environment. AI adds monitoring of the *artefact* — the model version, the guardrail classifier version, the dataset licence terms.
- **Supply-chain evidence contract.** SR 23-4 mentions software supply chain; AI needs the ML-specific evidence layer (chapter 07), which classical SR 23-4 practice does not currently reach.

Enterprises outside US federal banking use SR 23-4 as a *process-shape reference* — the phases, the risk-proportionality principle, and the accountability model compose with the enterprise's own TPRM programme regardless of sector. ISO/IEC 42001:2023 Clause 8 and Annex A (specifically the controls around third-party-provided AI systems and data) are the ISO analogue; NIST AI 100-1 (the AI RMF) MANAGE function includes third-party risk-management activities; the EU AI Act imposes provider / deployer obligations that bind on any vendor placing a general-purpose AI model on the EU market and cascade into the deployer's vendor management. The programme is designed to compose with all of them.

## The five architectural stances

The programme is designed against five stances. Each is an architectural commitment the architect makes and defends.

1. **Vendor risk is proportional to activity, not to spend.** A million-dollar SaaS contract for a low-materiality workflow tool is a lower-risk vendor than a hundred-thousand-dollar API subscription for a frontier-model provider whose outputs are consequential decisions. Tiering encodes materiality, not spend.
2. **The vendor's evidence is the enterprise's evidence, or the enterprise does not have evidence.** Every material claim the vendor makes about safety, security, evaluation, incident-handling, or data-use is captured in the enterprise's evidence substrate at contract time and re-captured on a cadence. Reliance on the vendor's live dashboards or unversioned assertions is not evidence.
3. **Model / classifier / dataset versions are first-class monitored artefacts.** Vendor-side updates are the drift signal that has no analogue in classical TPRM. The programme instruments the ingestion of vendor-version changes into the substrate and routes material changes into re-assessment (chapter 05).
4. **Contract terms are the enforcement point.** Governance intentions that are not encoded in contract terms are aspirational. Chapter 04 designs the control template; legal drafts the enforceable text.
5. **The programme has an exit posture.** Every material vendor engagement carries an exit / portability posture at contract time. Vendor lock-in without a defined exit is a residual-risk carry the risk register (mod-106) must reflect explicitly.

## A schematic of the programme

The programme as an artefact the architect authors and defends against the AI-accountable executive:

```yaml
third_party_ai_governance_programme:
  version: 1.0.0
  owner: senior-ai-governance-architect (level 50)
  ratifies:
    - head-of-ai-governance (level 60) — operational
    - ai-accountable-executive — annually
    - enterprise-tprm-head — for integration with enterprise TPRM
    - chief-procurement-officer — for procurement operating discipline
    - general-counsel — for contract-template control set

  vendor_typology:  # this chapter
    classes:
      - foundation-model-provider
      - guardrail-vendor
      - ai-observability-vendor
      - grc-for-ai-platform-vendor
      - dataset-provider
    each_class_declares:
      - characteristic_risk_shape
      - characteristic_evidence_expectation
      - reference_examples

  tiering:  # chapter 02
    dimensions:
      - data_sensitivity
      - decision_materiality
      - replaceability
      - jurisdictional_exposure
    tiers: [tier-1, tier-2, tier-3, tier-4]
    tier_definition_ref: chapter-02

  diligence:  # chapter 03
    ddq_per_tier: [tier-1.ddq, tier-2.ddq, tier-3.ddq, tier-4.ddq]
    reviewer_model: second-line + subject-matter-expert per section
    evidence_bundling: substrate ingest on close; freshness tracked

  contract_control_set:  # chapter 04
    templates:
      - data-use-limits
      - evaluation-access-rights
      - incident-notification-obligations
      - evidence-access-rights
      - exit-and-portability
      - subprocessor-controls
      - version-change-notification
    handoff:
      procurement: template selection + issuance
      legal: enforceable drafting + negotiation
      architect: control-shape ratification

  ongoing_monitoring:  # chapter 05
    periodic_reattestation: cadence per tier
    drift_triggered_reassessment: events per class (model-version, licence, incident, subprocessor, jurisdiction)
    incident_routing: vendor incident → enterprise risk register + AIMS non-conformity if applicable
    renewal_review: pre-renewal gate

  supply_chain_evidence_contract:  # chapter 07
    third_party_artefact_ingest:
      - foundation-model checkpoint (open-weight)
      - vendor-supplied fine-tune
      - vendor-supplied evaluation set
      - vendor-supplied dataset
    evidence_formats:
      - cyclonedx_ml_bom
      - spdx_3_ai_profile
      - slsa_attestation
      - sigstore_signature
    registry_gate: model-registry / dataset-registry ingestion policy (see mod-108 ch04)

  federal_acquisition_composition:  # chapter 06
    reference_shapes:
      - omb_m_24_18_2024
      - omb_m_25_22_2025
    enterprise_adaptation: rights-impacting / safety-impacting distinctions inherited; minimum practices adapted

  coordination:
    enterprise_tprm: append AI-specific tier + DDQ + contract addenda to enterprise process
    procurement: template issuance discipline; approved-vendor-list criteria
    legal: contract-template control set drafting; DPA + master agreement alignment
    information_security: SOC 2, penetration test, security-questionnaire evidence sharing
    ai_infra_security (level 35): supply-chain evidence gating; runtime security
    accountable_executive: escalation for exception approval

  register:  # chapter 05
    format: machine-readable
    fields:
      - vendor_id
      - class
      - tier
      - ddq_discharge_state
      - contract_template_version
      - contract_key_dates (start, notice-period-window, renewal, exit)
      - monitoring_cadence
      - open_exceptions
      - last_material_incident
      - last_material_version_change
      - substrate_evidence_refs

  invariants:  # see below
    - id: I1
      description: TPRM integration, not TPRM duplication
    - id: I2
      description: risk-proportional depth
    - id: I3
      description: version drift is monitored
    - id: I4
      description: contract encodes the control
    - id: I5
      description: every material vendor has an exit posture
    - id: I6
      description: supply-chain evidence gates ingestion
    - id: I7
      description: register is queryable and used
```

## The seven invariants the programme holds

**Invariant 1 — TPRM integration, not TPRM duplication.** The AI-specific programme is an *extension* of the enterprise's TPRM function, not a parallel programme. AI-specific tier, DDQ, and contract clauses are composed with the enterprise's standard shape; the vendor lands on one approved-vendor list, not two. Failure mode: procurement, security, and AI governance each ask the vendor a different questionnaire; the vendor answers three inconsistent stories; the enterprise cannot reconcile at renewal; the vendor complains and the head of AI is asked to stand down.

**Invariant 2 — risk-proportional depth.** Diligence effort, contract-clause coverage, and monitoring cadence scale with tier, not with vendor spend or vendor politeness. Failure mode: the highest-touch vendor is the SaaS-tool vendor whose sales team is the loudest, while the frontier-model provider gets a light-touch review because "everyone uses them."

**Invariant 3 — version drift is monitored.** The enterprise instruments the ingestion of vendor-side version changes — model, classifier, dataset, subprocessor list, jurisdiction — into the substrate, and routes material changes into re-assessment. Failure mode: the vendor silently rolls a new model behind the same API name; the enterprise's evaluation metrics shift; the enterprise attributes the shift to prompt drift and spends six weeks debugging a prompt problem that was actually a vendor problem.

**Invariant 4 — contract encodes the control.** Every governance intention material to the enterprise's exposure is encoded in a contract clause the enterprise can enforce. Aspirational commitments that live in Slack, in vendor marketing pages, or in unsigned annexes do not count. Failure mode: the vendor's roadmap page promised evaluation-access; when the enterprise asks for the evaluation the vendor's account manager says it isn't in the contract and the vendor's legal team declines.

**Invariant 5 — every material vendor has an exit posture.** For every tier-3 and tier-4 vendor, the enterprise has a defined exit posture — data portability, model / prompt / evaluation transferability, transition window, replacement candidate. Failure mode: the vendor's terms change materially; the enterprise's exit posture is theoretical; migration takes eighteen months during which the vendor's terms bind the enterprise.

**Invariant 6 — supply-chain evidence gates ingestion.** No third-party model artefact, dataset artefact, or vendor-supplied fine-tune enters the enterprise substrate without evidence discharge under the supply-chain evidence contract (chapter 07). Failure mode: an open-weight checkpoint is pulled into production because a data scientist wanted to prototype; the checkpoint has no provenance attestation, the licence terms were not reviewed, and it ends up serving production traffic for two quarters before anyone notices.

**Invariant 7 — the register is queryable and used.** The vendor register is machine-readable and the operating rhythm actually queries it — for evidence-freshness gaps, upcoming renewal windows, tier-4 vendors with open exceptions, incidents in the last 90 days. Failure mode: the register is a spreadsheet updated by one analyst; queries against it are hand-run at audit time; the head of AI governance cannot answer "what is our exposure today" without a two-day fire drill.

## Two failure modes to design against

**Failure mode 1 — the two-tier vendor list.** The enterprise maintains an "approved vendors" list and an "AI vendors" list. The AI list is where the AI governance function keeps its questionnaires; the approved list is what procurement operates against. A vendor accepted on the AI list has not always been onboarded through TPRM. A vendor onboarded through TPRM but flagged mid-cycle as "AI-relevant" is added to the AI list without being re-tiered under the AI programme. Both lists drift out of sync; the reconciliation happens at renewal; the reconciliation is manual and lossy. The fix is architectural: there is one approved-vendor list, with an AI-classification field per vendor, and the AI-specific tier + DDQ + contract addenda are appended to the enterprise's TPRM record rather than sitting in a parallel store. Chapter 02's tiering scheme is designed to compose with the enterprise's TPRM tiering.

**Failure mode 2 — the frontier-model provider treated as a SaaS vendor.** The enterprise onboards the frontier-model provider through its standard SaaS onboarding: SIG-Lite, SOC 2 review, standard DPA, standard MSA. The MSA does not carry the AI-specific clauses (data-use limits on prompts / completions / fine-tuning corpora; evaluation-access rights; model-version notification; safety-evaluation disclosure; exit portability for prompts and fine-tunes). Six quarters in, the enterprise is deeply dependent on the vendor's model behaviour and has none of the contract levers the AI programme was supposed to have built. The fix is architectural: chapter 02 tiers foundation-model providers as tier-3 or tier-4 by default (both because decision materiality is high and because replaceability is asymmetric); chapter 04 makes the AI-specific clauses standard-form so they land at onboarding rather than as a retrofit. Retrofits at renewal are difficult; the vendor has no incentive to accept clauses that were not there at signing.

## The composition with the rest of the architect's canvas

The third-party programme is not a self-contained artefact. It composes explicitly with:

- **mod-102 (control library architecture).** Third-party governance controls (SR 23-4 shape, ISO/IEC 42001 Annex A third-party controls) live in the enterprise control library; the SoA declares which apply per system; the third-party programme is the implementation shape for those controls.
- **mod-103 (policy taxonomy and policy-as-code).** The third-party AI acceptance policy is a first-line-facing standard the policy hierarchy carries; the ingestion-gate policies at the model registry (chapter 07) are policy-as-code artefacts.
- **mod-105 (AIMS).** ISO/IEC 42001 Clause 8 and the Annex A controls around third-party-provided AI systems and data bind the AIMS to the third-party programme; the programme's outputs (DDQ discharge, monitoring records, incident routing) are AIMS documented information.
- **mod-106 (risk taxonomy and appetite).** Vendor-risk categories are a first-class node in the risk taxonomy; the programme's monitoring cadence and exception-approval thresholds are calibrated against the appetite statement.
- **mod-107 (assurance architecture).** The pre-deployment gate (mod-107 ch02) checks that every third-party dependency the system inherits is tier-appropriately governed; the third-line audit (mod-107 ch04) samples the programme's operation.
- **mod-108 (evidence architecture).** Vendor-emitted evidence (SOC 2 letters, model cards, ML-BOMs, SLSA attestations, incident notifications) lands in the substrate under the schema catalog; chapter 07 leans directly on mod-108 ch04's supply-chain evidence slice.
- **mod-110 (post-market surveillance).** Vendor incidents and vendor-version drift are post-market signals routed into the risk register through the shared shape mod-110 designs.
- **mod-111 (GRC-for-AI platform).** GRC-for-AI vendors are themselves a class-4 vendor under this module's typology; mod-111 walks the platform-selection depth.
- **`ai-infra-security` (level 35).** The runtime supply-chain, signing platform, key management, and registry-admission-controller depth is owned by the level-35 peer. Chapter 07 pins the coordination interface — the architect owns the *evidence contract* on ingest, the level-35 peer owns the *runtime enforcement platform* underneath.
- **procurement, legal, information security, enterprise TPRM.** The programme is coordinated through declared handoffs; chapters 03, 04, and 05 pin the specifics.

## Summary

Third-party AI governance is a distinct architectural concern the level-50 architect owns. Its vendor typology names five classes (foundation-model providers, guardrail vendors, AI-observability vendors, GRC-for-AI platforms, dataset providers); its tiering (chapter 02) is risk-proportional; its DDQ (chapter 03) is tier-attached; its contract-template control set (chapter 04) makes intentions enforceable; its ongoing-monitoring schedule (chapter 05) catches vendor-side drift; its federal-acquisition composition (chapter 06) leans on OMB M-24-18 and M-25-22 as shape references; its supply-chain evidence contract (chapter 07) gates the ingestion of every third-party model artefact. SR 23-4 is the process-shape reference the AI-specific programme extends; ISO/IEC 42001 Clause 8 and Annex A, the NIST AI RMF, and the EU AI Act provider / deployer obligations compose. Five stances (proportional, evidence-backed, version-monitored, contract-enforced, exit-postured) shape the design. Seven invariants (TPRM integration, proportional depth, drift monitored, contract encoded, exit posture, supply-chain gated, register queryable) are testable. Two failure modes (two-tier list, frontier-model-as-SaaS) are common. The next chapter designs the tiering scheme that anchors the programme.
