# exercise-02: Due Diligence Questionnaire Authoring

**Estimated effort:** 3 hours

## Objective

Author a **tier-3 due-diligence questionnaire instance** for a specific vendor from the exercise-01 population, plus the DDQ **template** the instance derives from. The deliverable is a machine-processable DDQ schema, a full question set for one category (chapter 03 category 3 — AI safety, evaluation, quality), and a category-scaffold sketch for the remaining five categories. The instance and the template are what the exercise-03 contract-template control drill picks up; if the DDQ shape is under-specified here, the contract shape downstream inherits the gap.

The category-3 slice is chosen because it is where the AI programme's discipline is most distinct from the enterprise's existing SIG / CAIQ posture, where the reviewer seat is the peer level-35 evaluation engineer, and where the failure-mode risk (a check-the-box DDQ against a foundation-model vendor) is highest.

## Prerequisites

- Chapter [`03-due-diligence-questionnaire-architecture.md`](../03-due-diligence-questionnaire-architecture.md) read once, with the question schema, the six DDQ categories, the tier-per-category coverage matrix, the reviewer model with aggregate scoring, the composition with SIG / CAIQ, and the five invariants marked.
- Chapter [`02-vendor-tiering-criteria-and-tier-definitions.md`](../02-vendor-tiering-criteria-and-tier-definitions.md) read once for the tier-3 bundle scaffold.
- The mod-108 chapter 03 walk-through of the card-family model / system / dataset / risk cards, which the DDQ references as vendor-provided evidence.
- The peer role scope for `ai-evaluation-engineer` (level 35) — the reviewer seat for category-3 questions.
- Access to the primary references — Shared Assessments SIG (current release), CSA CAIQ (current release), NIST AI 600-1 (GenAI Profile), the foundation-model provider public materials (Anthropic, OpenAI, Google, Meta, xAI, Mistral, Hugging Face trust materials), the ISO/IEC 42001 Clause 8 third-party controls, the EU AI Act Chapter V (general-purpose AI providers) and Article 27 (deployers), the sector overlays applicable to your scenario. See [`../resources.md`](../resources.md).

## Scenario

Continue the enterprise scenario (A / B / C) from exercise-01. From your exercise-01 register, pick the vendor you scored as tier-4 that is closest to a *frontier foundation-model provider on a customer-facing path* — for most scenarios this is V01 FrontierModelCo. (If your exercise-01 scoring landed V01 at tier-3, use it; the drill works at either tier-3 or tier-4 with the appropriate coverage-matrix shape.) State the vendor and the tier at the top of the deliverable.

The instance you author is *this vendor*'s DDQ for *this engagement*. The template you author is the *tier-3 shape* your enterprise would issue to any tier-3 vendor of this class. Both must be produced.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`ddq-template-tier-3.yaml`** — the tier-3 DDQ template your enterprise issues. Per the tier-per-category coverage matrix from chapter 03: category counts in the tier-3 target range, question schema applied to every question, reviewer seat assignments per category, applicability filters where required, refresh cadences per question or per category.
2. **`ddq-category-3-full.yaml`** — the *full* category 3 (AI safety, evaluation, quality) question set — at least 18 questions (the tier-3 target lower bound from chapter 03) instantiating the chapter 03 question schema. Every question fully populated: id, category, question_text, intent, applicability, answer_format, evidence_expected, scoring rubric with 0–3 scale, weight, reviewer_seat, gap_treatment, refresh_cadence per tier, cross_references (controls, regulations, substrate_binding).
3. **`ddq-categories-1-2-4-5-6-scaffold.yaml`** — a scaffold of the remaining five categories showing the category's scope, the reviewer seat map, the expected question count in the tier-3 range, and *at least three fully-authored questions per category* to establish the question density and the SIG / CAIQ composition boundary.
4. **`ddq-instance-<vendor-id>.yaml`** — the DDQ instance for the specific vendor from your scenario, with (i) the front-matter section that would carry the federal-adaptation representations (preview chapter 06's FED-01), (ii) illustrative *vendor answers* for a sampled subset of five category-3 questions (a mix of scores 3, 2, 1, and 0 to exercise the reviewer disposition path), (iii) the reviewer's disposition on each sampled answer with rationale, (iv) the aggregate scoring computation for category-3, (v) the overall gate disposition (accept / accept-with-conditions / remediate / escalate), and (vi) the substrate binding.
5. **`sig-composition-note.md`** — a short (1–2 page) note describing how your DDQ template composes with your enterprise's SIG (or CAIQ) posture: which SIG sections your DDQ references rather than duplicates, which AI-specific extensions category 2 adds beyond what SIG covers, how the joint scoring aggregates.

## Requirements

### `ddq-template-tier-3.yaml`

- **Preamble.** Version 1.0.0; effective date; author (level-50 architect seat); ratifier (level-60 head of AI governance); scope (this template is issued to every tier-3 vendor of vendor-class C1, C2, C3, C4, and C5 as designated in the vendor register).
- **Category counts against the coverage matrix.** For each of the six categories, the target question count in the tier-3 range (12–18 for category 1; 25–40 for category 2 net of SIG passthrough; 18–25 for category 3; 15–20 for category 4; 10–15 for category 5; 15–20 for category 6). State the count.
- **Applicability filters.** Where questions apply only to a subset of vendor classes (e.g. fine-tuning-related questions apply only where the vendor offers fine-tuning; guardrail-configurability questions apply only where the vendor is a class-2 guardrail vendor), pin the filter shape.
- **Reviewer seat map.** Per category, the reviewer seat that scores. Reference chapter 03's reviewer table.
- **Aggregate scoring.** Weights per category (author defensible weights — a common shape gives category 2 and 3 heavier weight for tier-3 foundation-model vendors), the tier-3 aggregate threshold (>= 2.5 of 3 by chapter 03's default), the per-category minimum threshold (>= 2.0), the hard-fail questions the template carries.
- **Refresh cadence per category** as a default; per-question overrides apply.
- **Substrate binding shape.** Where the completed DDQ instance lands (`vendor-register/{vendor_id}/ddq/{template_version}/{completion_date}`), how versioning works, how re-issuance produces a diff.

### `ddq-category-3-full.yaml`

Author at least 18 questions covering:

- Vendor's safety-evaluation methodology and disclosed results (2–4 questions).
- Vendor's red-team practice — internal, third-party, cadence, scope, findings disclosure (2–3 questions).
- Vendor's hallucination / factuality / grounding metric methodology and disclosure (2 questions).
- Vendor's jailbreak-resistance posture and evaluation (2 questions).
- Vendor's content-policy shape and enforcement (1–2 questions).
- Vendor's guardrail configurability where applicable (1–2 questions, conditional on class 2).
- Vendor's evaluation-benchmark disclosures and any independent benchmarks the vendor participates in (2–3 questions).
- Vendor's evaluation of updates before release — pre-release evaluation methodology, regression posture, release-notes cadence and depth (2–3 questions).
- Vendor's incident-history disclosure and any published post-mortems (1–2 questions).

Per-question, instantiate the chapter 03 question schema fully. Do not compress the schema; the drill's discipline is precisely that each question is schema-authored.

At least three of the 18 questions should be **hard-fail** questions the tier-3 template treats as gate-blocking absent an explicit executive-sponsor exception. Candidate hard-fails: no published safety-evaluation on customer-facing model versions; no third-party red-team engagement disclosed or offered; use of enterprise prompts and completions for the vendor's own model training without explicit opt-in.

At least three questions should have **auto-recheck on published URIs** discipline in the refresh cadence field — the vendor publishes to a URL the enterprise's pipeline hashes and monitors.

Every question's `cross_references.regulations` field cites at least one specific obligation the question supports (EU AI Act article, ISO/IEC 42001 Annex A control, NIST AI 600-1 activity, sector overlay); every question's `cross_references.controls` field cites at least one enterprise control library row (candidate rows: `CTRL-VENDOR-EVAL-01` evaluation-evidence-currency-per-tier; `CTRL-VENDOR-EVAL-02` adversarial-evaluation-evidence-per-tier; `CTRL-VENDOR-SAFETY-01` published-safety-commitment-currency).

### `ddq-categories-1-2-4-5-6-scaffold.yaml`

For each of the five remaining categories:

- Category scope in 3–5 sentences per chapter 03.
- Reviewer seat map for the category (per chapter 03's reviewer table).
- Target question count for tier-3 per the coverage matrix.
- SIG / CAIQ composition boundary — for category 2 specifically, name the SIG sections the DDQ references (candidate: SIG A – Enterprise Risk Management, SIG C – Security Policy, SIG G – Human Resources, SIG H – Operations Management, SIG I – Compliance, SIG J – Business Continuity Planning, SIG K – Physical and Environmental Security, SIG L – Communications, SIG M – Server Security, SIG N – Endpoint Security, SIG O – Cloud Security, SIG U – Privacy, SIG V – Third-Party Risk Management) <!-- needs-research: verify the current SIG section-letter taxonomy and which sections are load-bearing for AI-specific composition at the current SIG release --> and which AI-specific questions category 2 adds beyond SIG.
- At least three fully-authored questions per category (schema-instantiated) to establish the density.

Specific attention:

- **Category 1 (company and governance)** — include a question about the vendor's own AI governance programme (published policy, incident-response, evaluation practice) at tier-3 depth; include a question about the vendor's participation in voluntary commitments (US 2023 voluntary commitments, EU AI Pact, UK AISI testing agreements) and any published safety commitments; at tier-4 include the vendor's own third-party AI risk programme (recursive).
- **Category 2 (infosec, privacy, resilience)** — the AI-specific extensions beyond SIG include: retention posture on prompts and completions (SIG typically does not cover in this depth); telemetry data-classification; subprocessor list specifically for the vendor's AI inference chain (hyperscaler, secondary inference providers, third-party observability); prompt / completion access by vendor personnel (human review).
- **Category 4 (data-use, provenance, IP)** — include: training-data provenance disclosure to the extent the vendor discloses; opt-out mechanism for training use of enterprise inputs; IP-indemnity coverage on outputs; children's data posture; opt-out effective scope and how the enterprise verifies.
- **Category 5 (model / component version, change, update)** — include: version-pinning support (endpoint-level or SDK-level); material-change definition and notification commitment; deprecation cadence; version-hold capability; the vendor's own pre-release evaluation posture on the class of change; vendor's own post-market monitoring on outputs.
- **Category 6 (incident, exit, contract-adjacent)** — include: incident-severity taxonomy the vendor supports; notification timelines by severity; post-mortem publication commitment; regulator-cooperation commitment; data export shape on termination; fine-tune weight portability on termination; audit-rights posture (SOC 2 sub-service or on-site or none).

### `ddq-instance-<vendor-id>.yaml`

For the specific vendor from your scenario:

- **Front matter.** Vendor identification (vendor_id, display name, class, tier from exercise-01 register); template_version (references your ddq-template-tier-3.yaml); issued_at, issued_by, expected_return_date.
- **Federal-adaptation representation (preview of chapter 06).** Vendor's structured representations on: AI-incorporation yes/no; generative-AI yes/no; model / component identity and version at contract time; training-data category disclosure; supply-chain evidence commitment; safety-evaluation commitment; incident-notification commitment; data-use commitment on enterprise inputs. These are contract representations (bind under FED-01 to be authored in chapter 04 additions). Author illustrative vendor answers.
- **Sampled answers.** For at least five category-3 questions from your ddq-category-3-full.yaml, author illustrative vendor answers with a deliberate mix of scores — at least one score-3 answer, at least two score-2 answers, at least one score-1 answer, at least one score-0 answer. The score-0 case exercises the hard-fail path; the score-1 exercises the compensating-control path; the score-2 exercises the accept-with-conditions path; the score-3 is a clean accept.
- **Reviewer dispositions.** For each sampled answer, the reviewer's disposition (accept / accept-with-conditions / remediate / escalate) with a 3–5 sentence rationale. For the score-1 case, either propose a compensating enterprise-run evaluation or a substrate residual-risk carry; for the score-0 case, name the executive-sponsor exception or the exit path.
- **Aggregate scoring for category 3.** Show the weighted-average calculation for the sampled questions (assume the unsampled questions would score at the average of the sampled if you cannot afford to author every answer). Compare to the tier-3 threshold; state whether category-3 passes or fails threshold.
- **Overall gate disposition.** Accept / accept-with-conditions / remediate / escalate for the whole DDQ instance. State the conditions if any and their expiry dates.
- **Substrate binding.** URIs of the form `vendor-register/{vendor_id}/ddq/{template_version}/{completion_date}` and per-question `vendor-register/{vendor_id}/ddq/{template_version}/answers/{question_id}`.
- **Handoff to chapter 04.** For each answer that below-threshold-scored, name the specific contract-clause remediation the exercise-03 drill will pick up. For the score-0 case, name the hard-fail treatment (chapter 04 exception or exit).

### `sig-composition-note.md`

- Overview (2–3 paragraphs) of how the DDQ composes with your enterprise's SIG (or CAIQ) — one questionnaire base, AI-specific extensions layered.
- Table (or bullet list) mapping DDQ category 2 questions to SIG sections they reference (evidence passthrough) versus DDQ category 2 questions that add AI-specific content beyond SIG.
- Handling of vendor's answer inconsistency between SIG and DDQ (candidate: reviewer flags; joint-disposition; contract representation binding both).
- Handling of SIG refresh cadence (typically annual) versus DDQ refresh cadence (chapter 03 defaults) — how the two ingests coordinate.
- Substrate binding — the joint SIG + DDQ record lands in the vendor register under a single completed-diligence record with the two source references.

## Starter guidance

- Do not paraphrase the chapter 03 category descriptions verbatim into your template. The template is *your enterprise's* instantiation of the shape; make specific choices (weights, reviewer seats, refresh cadences) rather than reciting the chapter.
- Do not write single questions that ask two things. A question is either well-formed and scorable or a compound question; compound questions produce narrative answers no reviewer can score. Split compound questions.
- Every question must be *answerable in the schema*, not narratively. If the answer_format is structured-object, the required_fields must be concrete (e.g. `published_uri`, `evaluation_categories_covered`, `methodology_summary`, `metric_values_disclosed`, `independent_third_party`, `report_currency`).
- The scoring rubric per question must have all four levels of the 0–3 scale populated. A rubric with only "3 = good, 0 = bad" is not a rubric; it is a wish. Score 2 (partial-with-limitations) and score 1 (asserted-without-methodology) are where the drill lives.
- Reviewer seat is a *seat*, not a person's name — `ai-evaluation-engineer (level 35 peer)`, `ai-governance-analyst (level 15)`, `legal-contract-analyst`, `privacy-office-analyst`. The seat is what survives the person's rotation.
- Refresh cadence per question is either auto-recheck of a published URI (for evidence the vendor publishes) or full re-answer (for evidence that is vendor-private). Mix both across the 18 questions; a category of 18 questions with 18 full-re-answers is a maintenance failure.
- Cross-references to regulations must be *specific* — "EU AI Act Article 9" not "the AI Act." If the specific article cannot be verified at authoring time, mark `<!-- needs-research: ... -->` rather than invent.
- The illustrative vendor answers in the instance file are *drills*, not vendor-marketing paraphrases. Author them from what a real frontier-model vendor's published materials, DPA, and standard MSA typically say (or don't say); the score-1 and score-0 cases are what the drill really wants to teach against.
- The reviewer disposition rationale is the substrate artefact a subsequent auditor samples. Make it complete: what the reviewer saw, what threshold it fell against, what compensating control was proposed, what expiry attaches, who signs off.
- The federal-adaptation front-matter (FED-01) preview is an intentional handoff to exercise 05. If it feels premature, note that in a design comment and continue; the field will bind fully in chapter 06.

## Acceptance criteria

- [ ] Scenario (A / B / C) and specific vendor (from exercise-01 register) are stated at the top of the deliverable; tier is stated.
- [ ] All five artefacts (`ddq-template-tier-3.yaml`, `ddq-category-3-full.yaml`, `ddq-categories-1-2-4-5-6-scaffold.yaml`, `ddq-instance-<vendor-id>.yaml`, `sig-composition-note.md`) are present.
- [ ] Template carries all six categories with question-count targets in the tier-3 range and reviewer seat map per category.
- [ ] Category 3 has at least 18 fully-authored questions; every question instantiates the full chapter 03 question schema (no fields omitted).
- [ ] At least three category-3 questions are marked hard-fail; at least three carry auto-recheck-on-published-URI refresh cadence.
- [ ] Each of the five other categories has at least three fully-authored questions in the scaffold.
- [ ] Instance file carries at least five sampled category-3 answers spanning scores 3, 2, 1, and 0 (at least one of each of 0 and 1); reviewer dispositions and rationales are present.
- [ ] Aggregate scoring for category 3 is computed against the template's threshold; overall gate disposition is stated.
- [ ] Federal-adaptation front-matter (FED-01 preview) is present with at least eight structured representation fields.
- [ ] SIG composition note maps at least three category-2 questions to SIG sections and enumerates at least three AI-specific extensions.
- [ ] Every unverified citation — SIG section letters, EU AI Act article number, ISO clause number, NIST AI 600-1 activity ID — is marked `<!-- needs-research: ... -->`. No invented section letters, article numbers, or activity IDs.

## Stretch goals

- **Author the tier-4 delta.** For the same vendor scenario but at tier-4 (either your vendor is already tier-4, or you promote it for the drill), enumerate the tier-4-only questions category 3 adds (candidate: independent third-party safety-audit currency; sub-service organisation SOC-2 or equivalent on the vendor's safety-evaluation function; the vendor's own third-party AI governance programme evidence; sanctions and export-control posture on the vendor's own model access).
- **Draft the DDQ diff for a refresh.** Assume six months after the initial instance closes, the vendor publishes a new model version with a materially different safety-evaluation summary and adds a subprocessor in a jurisdiction not previously used. Author the DDQ refresh instance as a diff against the initial — which questions re-open, which auto-rechecks fire, which reviewer seats re-engage, what the substrate binds.
- **Sketch the reviewer training programme.** In one page, describe the training the level-15 governance analyst and the level-35 evaluation engineer need to score the DDQ consistently across a vendor population: rubric calibration exercises, sample-scoring workshops, escalation runbooks, second-line sampling of individual reviewer's dispositions to detect scoring drift.
- **Draft the DDQ-as-code representation.** Sketch how the DDQ template and instance would land in a machine-readable schema against JSON Schema, and how the DDQ instance's answers become substrate events per mod-108's substrate contract. This previews the mod-108 chapter 06 OSCAL work and the mod-111 GRC platform's DDQ-ingest interface.
- **Compose with the enterprise's own responsible-AI framework.** Sketch the mapping between the DDQ's category-3 questions and the enterprise's own responsible-AI framework (candidate frameworks: NIST AI RMF governance-function activities, IEEE 7000 series, a bespoke enterprise responsible-AI framework). The mapping is what allows the DDQ answers to feed the enterprise's *self-assessment* against the framework, in addition to being vendor diligence.
