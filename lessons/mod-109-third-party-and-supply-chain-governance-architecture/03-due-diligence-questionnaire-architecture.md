# Due-diligence questionnaire architecture — tier-attached DDQ shape

## Why this chapter exists

A vendor questionnaire without an architecture is a folk-authored list of questions the enterprise wishes it had asked the last vendor that went wrong. The list grows with each incident. It repeats the enterprise information-security questionnaire's questions without adding to them. Its scoring is qualitative and inconsistent between analysts. Its answers land in a vendor's response as a PDF, get skimmed by the sponsoring team, land in a shared drive, and are never referenced again unless a regulator or a subpoena reaches for them. The enterprise thinks it has done diligence; the enterprise has a filing.

The architect's DDQ is not that. It is:

- **Tier-attached.** The DDQ the vendor completes is derived from the vendor's tier (chapter 02). A tier-1 vendor gets a short-form DDQ; a tier-4 vendor gets a comprehensive one. There is no single DDQ for everyone.
- **Structured against a schema.** Every question has a category, a target evidence type, a scoring convention, and a reviewer discipline. The schema is versioned. The vendor's answers are captured in the schema's shape, not as free-form prose.
- **Anchored in the enterprise's control library and evidence architecture.** Every question maps to a control (from mod-102) the enterprise implements against, and the vendor's answer becomes an evidence artefact under the mod-108 schema catalog.
- **Reviewed by named seats.** No question in the DDQ is un-owned; every category has a reviewer seat and a scoring convention that reviewer applies.
- **Reproducible and refreshable.** The vendor's answers are versioned; re-issuance on the monitoring cadence (chapter 05) is a diff exercise, not a fresh authoring cycle.

This chapter designs the DDQ architecture. The schema of a well-formed question; the six DDQ categories; the tier-per-category coverage matrix; the review-and-scoring model; how the DDQ composes with the enterprise's TPRM questionnaire (SIG, SIG-Lite, or CAIQ family); the failure modes to design against. Exercise-02 walks the drill.

## What the DDQ is, structurally

The DDQ is:

- **A per-tier bundle of question categories.** Six categories cover the AI-specific risk surface; each category's depth scales with tier.
- **A schema over each question.** Category, question text, evidence expected, scoring rubric, reviewer seat, weight, applicability filter (some questions apply only to certain vendor classes).
- **A machine-processable capture format.** Vendor answers land in a YAML / JSON structure the schema defines; PDFs and free-form attachments are permitted per-question but the primary answer is structured.
- **A versioned artefact.** DDQ template versions are ratified by second-line; every issued DDQ instance references the template version; re-issuance produces a diff against the previous answer instance.
- **An evidence pack on close.** The completed DDQ, its attached evidence, and the reviewer's scoring land as a single evidence bundle in the mod-108 substrate, referenced from the vendor register.

## What it is not

- **A SIG questionnaire.** The Shared Assessments SIG family (SIG, SIG-Lite, SIG-Core) and the Cloud Security Alliance CAIQ are excellent, widely-adopted vendor security questionnaires. They cover information security, privacy, business continuity, and general vendor-risk categories. They do *not* natively cover AI-specific risk shape — model version disclosure, evaluation methodology, training-data provenance, safety-evaluation posture, guardrail configurability, model-hallucination rate disclosure, third-party red-team engagement, etc. The AI DDQ is an *AI-specific extension* to the SIG/CAIQ, not a replacement. Chapter's coordination section walks the composition.
- **A single questionnaire for the whole population.** A single fits-none questionnaire for tier-1 through tier-4 vendors will over-ask small vendors and under-ask consequential ones. Tier-attached is the design.
- **A free-form questionnaire.** Free-form answers are un-scorable, un-comparable across vendors, and un-refreshable. The DDQ is schema-structured.
- **The whole diligence.** The DDQ is *one* input to the vendor-onboarding decision. Other inputs include the vendor's SOC 2 / ISO 27001 / ISO 42001 attestations, third-party safety-audit reports, published model / system cards, references, financial-condition review (for tier-4), and any regulator inquiry / incident history. The DDQ organises the vendor's structured self-disclosure; it does not substitute for the corroborating artefacts.

## The schema of a well-formed question

Every question in the DDQ is authored against a fixed schema. The schema is the discipline that makes the DDQ scorable, comparable, refreshable, and auditable.

```yaml
question_schema:
  id: <stable-identifier, e.g., DDQ-EVAL-014>
  category: <one of the six categories below>
  question_text: <one clear, specific question — no compound questions>
  intent: <what the question is trying to establish; a one-sentence why>
  applicability:
    tiers: [<tier(s) for which this question is required or optional>]
    vendor_classes: [<vendor class(es) to which this question applies>]
    conditional_on: <optional pre-condition — e.g., "vendor offers fine-tuning capability">
  answer_format:
    type: <one of: yes-no, multiple-choice, structured-object, free-text-with-required-fields, attachment-plus-summary>
    required_fields: [<if structured>]
  evidence_expected:
    - <attestation letter (SOC 2 / ISO 27001 / ISO 42001)>
    - <model card / system card excerpt>
    - <SLSA attestation>
    - <published incident post-mortem>
    - <policy / procedure excerpt>
    - <third-party test report>
    - <internal statement by named signatory>
  scoring:
    rubric: <values on the scale, definitions>
    scale: <e.g., 0-3, or accept / conditional / reject>
    weight: <in the category's aggregate>
  reviewer_seat: <seat that scores this question; see reviewer table below>
  gap_treatment:
    if_unanswered: <default disposition when the vendor does not answer>
    if_answered_below_threshold: <default disposition and remediation path>
  refresh_cadence:
    tier_1: <cadence>
    tier_2: <cadence>
    tier_3: <cadence>
    tier_4: <cadence>
  cross_references:
    controls: [<control library rows the question maps to>]
    regulations: [<regulator obligations the question supports>]
    substrate_binding: <how the answer lands in the evidence substrate>
```

Every question in every DDQ instantiates this schema. Chapter's exercise (exercise-02) walks the drill of authoring a set of tier-3 questions to this shape.

## The six DDQ categories

The DDQ's questions are organised into six categories. Each category has a scope, a characteristic evidence expectation, and a characteristic reviewer seat. The tier scales the depth within each category.

### Category 1 — Vendor company and governance

*Who is the vendor company, what is its financial and governance posture, and does it have its own AI governance and risk-management discipline?*

Scope: company legal identity, ownership, jurisdiction of incorporation and operation, financial condition (tier-4), sanctions / export-control posture, board oversight of AI risk (tier-4), the vendor's own AI-governance programme (tier-3+), the vendor's own third-party AI risk management (recursive; tier-4).

Reviewer seat: enterprise TPRM function for the classical questions; AI governance for the AI-governance-programme questions; treasury / risk for the financial questions (tier-4); legal for sanctions / export-control.

Characteristic evidence: incorporation documents, cap table (tier-4), most recent audited financials (tier-4), sanctions-screening evidence, the vendor's own published AI-governance materials (an AI-policy page, a responsible-use guide, a public model card, a system card, a published incident-response commitment).

### Category 2 — Information security, privacy, and operational resilience

*What is the vendor's security, privacy, and business-continuity posture?*

Scope: SOC 2 Type II / ISO 27001 posture, penetration-test cadence and remediation practice, data-processing shape (locations, storage, encryption, key management, retention, deletion), personal-data handling (GDPR / CCPA / HIPAA / sector-specific), business-continuity and disaster-recovery (RTO / RPO commitments), sub-service organisation controls and subprocessor list, incident-response programme.

Reviewer seat: information security for classical InfoSec questions; privacy office for personal-data questions; enterprise resilience for BC/DR.

Characteristic evidence: SOC 2 Type II report currency; ISO 27001 certificate currency; recent penetration test summary; DPA; subprocessor register; incident-response plan summary; BC/DR test summary.

The AI programme does not re-ask what the enterprise's InfoSec, privacy, and resilience functions already ask through SIG / CAIQ. Category 2 references the enterprise's existing questionnaire and adds only AI-specific extensions where they are needed (e.g., special handling of prompt / completion telemetry that would not appear in a classical SIG).

### Category 3 — AI-specific safety, evaluation, and quality

*What is the vendor's safety posture, its evaluation practice, and its quality discipline for its AI outputs?*

Scope: safety-evaluation methodology and disclosed results; red-team practice (internal or third-party engaged, cadence, scope, findings-disclosure posture); hallucination-rate disclosure where meaningful; jailbreak-resistance posture; guardrail configurability where applicable; content-policy shape; evaluation-benchmark disclosures; the vendor's methodology for evaluating updates before release.

Reviewer seat: AI evaluation engineer (level 35) — this is the peer role's substantive contribution to the DDQ.

Characteristic evidence: published model card and system card with evaluation results; third-party safety-audit report (Apollo Research, METR, or similar for frontier models — tier-3+); red-team report summary; evaluation-benchmark results; safety-tuning approach summary; policy commitments (a vendor "responsible use" policy, a published child-safety commitment, a published elections-integrity commitment, etc.); commitments made under a public voluntary regime (US voluntary commitments 2023, EU AI Pact, UK Frontier AI Safety Institute testing agreements).

### Category 4 — AI-specific data-use, provenance, and IP

*How does the vendor handle enterprise data, what is the provenance of the vendor's training data, and what IP posture attaches to inputs and outputs?*

Scope: data-use commitments on prompts, completions, fine-tuning corpora, telemetry (retention, use for vendor training, use for abuse monitoring, human review); training-data provenance disclosure (to the extent disclosed — most frontier vendors disclose category-level, not specific-source-level); copyright / IP commitments on outputs (indemnity where offered); dataset provenance and licence terms (for dataset providers); PII detection and redaction posture; children's data posture; opt-out mechanisms and their effective scope.

Reviewer seat: legal (for IP / licence questions); privacy office (for data-use and personal-data questions); AI governance (for the AI-specific commitments and their auditability).

Characteristic evidence: vendor's published data-use policy or DPA schedule; vendor's published training-data disclosure; vendor's published IP-indemnity commitment; vendor's opt-out UI or contractual mechanism; for dataset providers, the SPDX 3.0 record with the dataset profile (see chapter 07).

### Category 5 — Model / component version, change, and update posture

*How does the vendor version its model / classifier / dataset, what notice does the vendor commit to on changes, and what is the vendor's change-management discipline?*

Scope: model / classifier / dataset version naming and pinning; commitment to notify on material changes (definition, timeline, channel); rollback / prior-version-access support; deprecation cadence; ability to hold a version (opt-out of automatic updates); the vendor's own regression-testing / evaluation-before-release discipline; the vendor's post-market monitoring on its own outputs.

Reviewer seat: AI evaluation engineer for the version discipline; AI infrastructure security peer (level 35) for the change-management platform aspects.

Characteristic evidence: vendor's version-pinning capability (documented endpoint or SDK support); vendor's material-change-notification commitment (contract or published policy); vendor's release notes cadence and shape; vendor's evaluation-before-release attestation; vendor's post-market monitoring commitments; documented deprecation history for prior versions.

### Category 6 — Incident, exit, and contract-adjacent

*What is the vendor's incident-notification posture, and what is the enterprise's exit posture from the vendor?*

Scope: vendor's incident-severity taxonomy; notification timelines (hour scale for safety and security incidents; day scale for operational); post-mortem publication commitment; regulator-cooperation commitment; exit / portability shape (data export, prompt export, fine-tune export, evaluation export, transition-services agreement); records retention post-exit; termination-for-cause triggers (safety incident above threshold, licence breach, transfer-regime failure); audit-rights posture.

Reviewer seat: AI governance for the incident and exit shape; legal for the contract-adjacent commitments; enterprise resilience for the exit shape and TSA.

Characteristic evidence: vendor's incident-notification commitment (contract clause or DPA schedule); vendor's public incident history and post-mortem shape (where published); vendor's data-portability documentation; vendor's TSA template (tier-4); vendor's audit-rights posture (SOC 2 sub-service, on-site, or none).

## The tier-per-category coverage matrix

Each tier binds a coverage depth per category. The matrix is not the final list of questions — it is the *category-level directive* to the DDQ author.

```yaml
tier_category_matrix:
  version: 1.0.0

  tier-1:
    company_and_governance:      { depth: baseline,   question_count_target: 5-8 }
    infosec_privacy_resilience:  { depth: baseline,   question_count_target: 10-15  # largely SIG-Lite passthrough }
    ai_safety_evaluation_quality:{ depth: baseline,   question_count_target: 3-5 }
    data_use_provenance_ip:      { depth: baseline,   question_count_target: 3-5 }
    version_change_update:       { depth: baseline,   question_count_target: 2-3 }
    incident_exit_contract:      { depth: baseline,   question_count_target: 5-7 }
    total_target: ~30-45 questions

  tier-2:
    company_and_governance:      { depth: standard,   question_count_target: 8-12 }
    infosec_privacy_resilience:  { depth: standard,   question_count_target: 15-25 }
    ai_safety_evaluation_quality:{ depth: standard,   question_count_target: 8-12 }
    data_use_provenance_ip:      { depth: standard,   question_count_target: 8-12 }
    version_change_update:       { depth: standard,   question_count_target: 5-8 }
    incident_exit_contract:      { depth: standard,   question_count_target: 8-12 }
    total_target: ~55-80 questions

  tier-3:
    company_and_governance:      { depth: heightened, question_count_target: 12-18 }
    infosec_privacy_resilience:  { depth: heightened, question_count_target: 25-40 }
    ai_safety_evaluation_quality:{ depth: heightened, question_count_target: 18-25 }
    data_use_provenance_ip:      { depth: heightened, question_count_target: 15-20 }
    version_change_update:       { depth: heightened, question_count_target: 10-15 }
    incident_exit_contract:      { depth: heightened, question_count_target: 15-20 }
    total_target: ~100-140 questions

  tier-4:
    company_and_governance:      { depth: comprehensive, question_count_target: 20-30 }
    infosec_privacy_resilience:  { depth: comprehensive, question_count_target: 40-60 }
    ai_safety_evaluation_quality:{ depth: comprehensive, question_count_target: 25-40 }
    data_use_provenance_ip:      { depth: comprehensive, question_count_target: 20-30 }
    version_change_update:       { depth: comprehensive, question_count_target: 15-25 }
    incident_exit_contract:      { depth: comprehensive, question_count_target: 20-30 }
    total_target: ~150-215 questions
```

The counts are *targets*, not prescriptions. A well-designed tier-3 DDQ with 90 sharply-scoped questions beats a poorly designed one with 140 duplicative ones. The counts exist to prevent tier-4 DDQs from silently deflating to 30-question quickies.

## Illustrative question set — tier-3, category 3 (AI safety, evaluation, quality)

Not the full tier-3 DDQ; a representative slice to show the schema at work. Each question is instantiated to the schema above.

```yaml
- id: DDQ-EVAL-014
  category: ai-safety-evaluation-quality
  question_text: >
    For each model version made available to the enterprise under this
    engagement, does the vendor publish or disclose the safety-evaluation
    results the vendor ran prior to release, including the evaluation
    categories, the methodology, and the disclosed metric values?
  intent: >
    Establish whether the enterprise has evidence to satisfy its own
    pre-deployment gate (mod-107 chapter 02) that the vendor's model has
    been evaluated against the enterprise's declared risk categories.
  applicability:
    tiers: [tier-3, tier-4]
    vendor_classes: [foundation-model-provider]
  answer_format:
    type: structured-object
    required_fields:
      - published_uri
      - evaluation_categories_covered
      - methodology_summary
      - metric_values_disclosed (yes/no)
      - independent_third_party (yes/no)
      - report_currency (date of most recent)
  evidence_expected:
    - vendor-published model card / system card excerpt
    - third-party safety-audit report (where independent third party engaged)
  scoring:
    rubric:
      3: >
        Comprehensive published evaluation across the enterprise's declared
        risk categories, with methodology and metric values disclosed;
        third-party independent evaluation performed and disclosed.
      2: >
        Published evaluation covering most of the enterprise's declared
        risk categories, with methodology; some metric values disclosed;
        no third-party independent evaluation.
      1: >
        Vendor asserts evaluation is performed but does not disclose
        methodology or results; or evaluation covers a narrow subset of
        the enterprise's declared risk categories.
      0: >
        No published evaluation; or vendor declines to disclose whether
        evaluation is performed.
    scale: 0-3
    weight: 3
  reviewer_seat: ai-evaluation-engineer (level 35 peer)
  gap_treatment:
    if_unanswered: default to score 0; escalate to reviewer
    if_answered_below_threshold: >
      score ≤ 1 requires either compensating evidence (the enterprise runs
      the enterprise's own evaluation suite against the vendor's model at
      each version — a chapter 04 contract clause) or an executive-sponsor
      exception with a documented residual-risk carry to the risk register
      (mod-106).
  refresh_cadence:
    tier_1: n/a
    tier_2: n/a
    tier_3: quarterly (auto-recheck of published-URI currency); annual full re-answer
    tier_4: monthly (auto-recheck); quarterly full re-answer
  cross_references:
    controls:
      - CTRL-VENDOR-EVAL-01 (evaluation-evidence-currency-per-tier)
    regulations:
      - EU AI Act Article 9 (risk-management-system) — vendor evidence supports enterprise's discharge
      - ISO/IEC 42001 Annex A control on third-party AI systems evaluation
    substrate_binding: vendor-register/{vendor_id}/ddq/{template_version}/answers/DDQ-EVAL-014

- id: DDQ-EVAL-015
  category: ai-safety-evaluation-quality
  question_text: >
    Does the vendor engage independent third-party red teams against the
    model version(s) offered to the enterprise, and does the vendor make
    the resulting reports (or a redacted equivalent) available to the
    enterprise on request?
  applicability:
    tiers: [tier-3, tier-4]
    vendor_classes: [foundation-model-provider]
  answer_format:
    type: structured-object
    required_fields:
      - third_party_engaged (yes/no)
      - engagement_scope_summary
      - report_availability (public / on-request / no)
      - most_recent_report_date
      - remediation_practice_summary
  evidence_expected:
    - vendor's published red-team report or letter
    - independent third-party statement of work (redacted)
  scoring:
    rubric:
      3: >
        Independent third-party red-team engaged on a periodic cadence; reports
        available on request under NDA; remediation practice documented.
      2: >
        Independent third-party engaged; report availability limited but the
        vendor is willing to walk through findings in a discussion under NDA.
      1: >
        Internal red-team only, but disciplined and documented.
      0: >
        No red-team programme, or vendor declines to answer.
    scale: 0-3
    weight: 2
  reviewer_seat: ai-evaluation-engineer (level 35 peer)
  gap_treatment:
    if_unanswered: default to score 0
    if_answered_below_threshold: >
      score ≤ 1 for tier-4 requires an executive-sponsor exception and a
      compensating enterprise-run adversarial evaluation programme.
  refresh_cadence:
    tier_3: annual
    tier_4: semi-annual
  cross_references:
    controls:
      - CTRL-VENDOR-EVAL-02 (adversarial-evaluation-evidence-per-tier)
    regulations:
      - NIST AI 600-1 (Generative AI Profile) — MEASURE and MANAGE red-team practices

- id: DDQ-EVAL-016
  category: ai-safety-evaluation-quality
  question_text: >
    Where the vendor publishes or asserts a hallucination / factuality /
    grounding metric for the model version(s) offered, disclose the metric's
    definition, the evaluation dataset(s) used, the reported value(s), the
    methodology's known limitations, and the metric's stability across
    version updates.
  applicability:
    tiers: [tier-3, tier-4]
    vendor_classes: [foundation-model-provider]
    conditional_on: vendor claims a hallucination / factuality metric
  answer_format:
    type: structured-object
    required_fields:
      - metric_definition
      - evaluation_datasets
      - reported_values
      - methodology_limitations
      - inter_version_stability_notes
  scoring:
    rubric:
      3: >
        Metric defined, dataset disclosed, methodology transparent,
        limitations acknowledged, inter-version stability tracked.
      2: >
        Metric defined and reported but limitations under-disclosed.
      1: >
        Metric asserted without methodology.
      0: >
        No answer or the vendor declines to disclose.
    scale: 0-3
    weight: 1
  reviewer_seat: ai-evaluation-engineer (level 35 peer)
  gap_treatment:
    if_answered_below_threshold: >
      The enterprise runs its own factuality evaluation as compensating
      control; the metric is not used as a load-bearing input to the
      enterprise's own reporting or gate decisions.
  cross_references:
    controls:
      - CTRL-VENDOR-EVAL-03 (metric-methodology-disclosure)
```

Three questions is not a complete tier-3 category-3 DDQ; it is enough to show the schema working at the density the tier requires. Exercise-02 asks the learner to produce the whole category, and to sketch categories 4–6 for the same tier.

## The reviewer model

Every question in the DDQ has a *reviewer seat*. The reviewer scores the question against the rubric, requests remediation evidence when the vendor's answer is below threshold, and records the disposition (accept, accept-with-conditions, remediate, escalate). The reviewer is *not* the sponsoring team from the first line — reviewers are seated in second line or in peer specialist roles.

```yaml
reviewer_seat_assignment:
  by_category:
    company_and_governance:
      classical_questions: enterprise-tprm-analyst
      ai_governance_programme_questions: ai-governance-analyst (level 15) with head-of-ai-governance sign-off
      financial_questions (tier-4): treasury / risk analyst
      sanctions_export_control: legal-compliance-analyst
    infosec_privacy_resilience:
      classical_infosec_and_bcdr: information-security-analyst
      privacy_and_dpa: privacy-office-analyst
      ai_specific_extensions: ai-governance-analyst + information-security-analyst joint
    ai_safety_evaluation_quality:
      all_questions: ai-evaluation-engineer (level 35 peer role)
      with_second-line_review_by: ai-governance-analyst
    data_use_provenance_ip:
      ip_and_licence_questions: legal-analyst
      privacy_and_personal_data: privacy-office-analyst
      ai_governance_disclosures: ai-governance-analyst
    version_change_update:
      version_platform_and_infrastructure: ai-infrastructure-security (level 35 peer)
      evaluation_before_release: ai-evaluation-engineer (level 35 peer)
    incident_exit_contract:
      incident_shape: ai-governance-analyst + information-security-analyst joint
      exit_and_tsa: enterprise-resilience-analyst
      contract_shape: legal-contract-analyst

  aggregate_scoring:
    per_category_score: weighted-average-of-question-scores-per-category
    ddq_aggregate: category-scores-with-category-weights (weights set per tier)
    decision_thresholds:
      tier_1: aggregate >= 2.0 (of 3) required; no category < 1.0
      tier_2: aggregate >= 2.2 required; no category < 1.5
      tier_3: aggregate >= 2.5 required; no category < 2.0
      tier_4: aggregate >= 2.7 required; no category < 2.3; specific hard-fail questions

  hard-fail questions:
    - DDQ-INFOSEC-001: vendor lacks SOC 2 Type II or equivalent  → hard-fail tier-2+
    - DDQ-DATA-005: vendor uses enterprise prompts / completions for training without explicit opt-in → hard-fail tier-3+ absent contract override
    - DDQ-EXIT-002: vendor cannot export enterprise fine-tune weights on exit → hard-fail tier-4 without executive-sponsor exception
```

The aggregate score is *not* the whole decision. It is one input; the reviewer's narrative disposition, the compensating-control considerations, and the residual-risk carry to the risk register (mod-106) round out the decision. The architect designs the aggregation and the thresholds; the head of AI governance operates them.

## Composition with SIG / CAIQ

The enterprise's existing security questionnaire (SIG, SIG-Lite, SIG-Core; CAIQ from CSA; a bespoke enterprise variant) covers the classical InfoSec, privacy, and BC/DR space. The AI DDQ does not re-ask those questions; it *references* the SIG / CAIQ answers and adds the AI-specific extensions:

```yaml
sig_ai_composition:
  version: 1.0.0
  base_questionnaire: SIG 2027   # or CAIQ v4.x, or enterprise variant
  ai_extensions:
    - questions in DDQ categories 1 (AI-governance-programme slice), 3, 4, 5, and part of 6
  composition_rule:
    - SIG covers infosec / privacy / BC/DR: DDQ category 2 references SIG answers as evidence, adds only questions the SIG doesn't cover (e.g., prompt / completion telemetry retention specifics)
    - AI-specific categories (3, 4, 5) run in full
    - AI-adjacent categories (1, 6) reference the SIG's overlap sections and add the AI-specific extensions
  reviewer coordination:
    - SIG reviewer's scoring feeds category-2 aggregate
    - AI-DDQ reviewer's scoring feeds AI-specific categories
    - joint disposition at end of DDQ close
```

This composition is what invariant 1 from chapter 01 (TPRM integration, not duplication) looks like at the DDQ layer.

## The refresh cadence — how the DDQ stays current

The DDQ is not a one-time artefact. Chapter 05 walks the full monitoring schedule; the DDQ-specific refresh cadence attaches per-question. Two shapes:

- **Auto-recheck on published fields.** Questions whose evidence is a published URI (a vendor's public model card, a vendor's SOC 2 letter posted on their trust page) are auto-rechecked on a defined cadence — the pipeline pulls the URI, hashes the content, records the freshness, and flags material differences for reviewer attention. Auto-recheck happens quarterly for tier-3 and monthly for tier-4 by default; the question schema per-question overrides.
- **Full re-answer.** Questions whose answer depends on the vendor's private posture (subprocessor list, fine-tuning disclosure, incident history) require the vendor to re-answer. Full re-answer is annual for tier-2, semi-annual for tier-3, and quarterly for tier-4 by default; per-question overrides apply.

The refresh discipline is what makes the DDQ a *living evidence artefact* rather than a one-time filing.

## The five invariants the DDQ architecture holds

**Invariant 1 — every question has a reviewer seat.** No question is un-owned. Failure mode: a DDQ with 120 questions and 40 of them shepherded by "the AI governance team" as a collective — the answers get skimmed and scored in bulk; the questions that needed specialist attention (the evaluation-methodology question that only the evaluation engineer can score, the IP indemnity question that only legal can score) get generic scores.

**Invariant 2 — every answer lands in the schema.** Vendors sometimes want to respond with a marketing PDF and a link to their trust page. The DDQ requires the schema-shaped answer plus (optionally) the attachments. Failure mode: the vendor's response is 40 attachments and a cover letter; the reviewer cannot compare across vendors; scoring is impressionistic.

**Invariant 3 — every completed DDQ is versioned and refreshable.** The DDQ instance is a versioned artefact; refresh is a diff, not a fresh authoring cycle. Failure mode: the vendor's original response lives in a shared drive; a year later the DDQ is "re-run" by asking the vendor for a fresh questionnaire; the reviewer cannot see what changed.

**Invariant 4 — the reviewer's disposition is a substrate artefact, not a private note.** Accept / accept-with-conditions / remediate / escalate lands as a structured disposition record in the substrate; the reviewer's narrative and the compensating-control rationale (where applicable) are attached. Failure mode: the reviewer says "looks fine" in a Slack thread; the disposition is not queryable; the risk register cannot cite the diligence.

**Invariant 5 — the DDQ composes with the enterprise's SIG / CAIQ, not duplicates it.** Failure mode: the vendor answers 400 questions across two questionnaires that overlap 60%; the vendor gives inconsistent answers to overlap questions; the enterprise cannot reconcile.

## Two failure modes to design against

**Failure mode 1 — the check-the-box DDQ.** The DDQ is issued, the vendor's account manager farms the questions to a sales-engineering pool that has answered thousands of similar questionnaires, the responses are optimistic and lightly-corroborated. The reviewer scores generously because pushing back is friction and the sponsoring team wants to close the vendor. The aggregate score is above threshold; the DDQ closes; the vendor onboards; six quarters in the actual posture is materially worse than the responses claimed and the enterprise's evidence is a set of vendor-supplied assertions. The fix is architectural: (i) evidence-expected per question is *required*, not optional — a bare-answer without the attachment scores below threshold; (ii) hard-fail questions exist and are unambiguous; (iii) the reviewer's narrative disposition is a substrate artefact, not a Slack line; (iv) second-line sampling covers 100% of tier-4 close decisions and a defined percentage of tier-3; (v) the third-line audit (mod-107 ch04) samples DDQ closures periodically to check for reviewer inflation.

**Failure mode 2 — the DDQ that has never been refreshed.** The vendor's DDQ was completed at onboarding two years ago. The vendor has since released three major model versions, added five subprocessors, changed the DPA schedule, moved its EU inference infrastructure to a different hosting region, and experienced two publicly-disclosed incidents. The DDQ answers on file still describe the T=0 posture. The enterprise's risk register cites a stale DDQ. The fix is architectural: (i) per-question refresh cadence is enforced; (ii) auto-recheck on published URIs runs on schedule and flags material differences; (iii) chapter 05's monitoring signals wire vendor-side change events into targeted DDQ refresh triggers; (iv) the register carries a "DDQ freshness" field that the operating rhythm queries; (v) stale DDQs above tier-2 auto-open a re-issuance workflow.

## Summary

The DDQ is a tier-attached, schema-structured, category-organised, reviewer-owned, versioned artefact. Six categories (company / governance, infosec-privacy-resilience, AI safety-evaluation-quality, data-use-provenance-IP, version-change-update, incident-exit-contract) cover the risk surface; each category's depth scales with tier per a coverage matrix; each question instantiates a shared schema (id, category, applicability, answer format, evidence expected, scoring rubric, reviewer seat, gap treatment, refresh cadence, cross-references). Reviewer seats are named per category. Aggregate scoring feeds a tier-appropriate decision threshold with hard-fail questions. Composition with SIG / CAIQ prevents duplication; refresh cadence keeps the DDQ current. Five invariants and two failure modes shape the design. Exercise-02 walks the drill of authoring a category slice against the schema. The next chapter designs the contract-template controls that convert DDQ commitments into enforceable obligations.
