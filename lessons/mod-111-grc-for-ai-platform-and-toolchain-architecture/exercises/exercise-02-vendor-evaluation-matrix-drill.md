# exercise-02: Vendor Evaluation Matrix Drill

**Estimated effort:** 4 hours

## Objective

Produce the **enterprise vendor evaluation matrix** — the instrument through which the build-vs-buy decision for the GRC-for-AI platform is made honestly against the chapter-01 reference architecture, rather than shopped on the strength of a curated vendor demo. The deliverable set is a version-locked matrix schematic, one scorecard per candidate for three candidates (a purpose-built AI-GRC platform, an enterprise-GRC extension, and a build-in-house sketch), a proof-of-concept plan scoped to the highest-criticality integrations, a build-vs-buy recommendation that reads the pattern of scores rather than the aggregate rank, and a five-year total-cost-of-ownership model that includes the migration-at-contract-end term the initial-year quote almost always omits.

The correctness spine is the invariants chapter 02 fixed: the matrix is authored and version-locked *before* any vendor demo is scheduled; every dimension carries a testable rubric; and claims that cannot be exercised in an enterprise-controlled proof-of-concept do not score above **unverified**, no matter how compelling the demo footage was. The two failure modes the matrix is designed against — picking on a demo without a reference-architecture-scored evaluation, and treating the matrix as a spreadsheet exercise rather than as a build-vs-buy decision instrument — are the tests each deliverable must survive. A matrix authored after demos begin fails invariant 1; a scorecard that promotes a vendor claim from marketing footage to a **verified** band without a POC observation fails invariant 3; a recommendation that reads "the top-weighted aggregate wins" without commentary on the pattern of scores across dimensions fails failure-mode-(b).

Draft as a senior architect briefing an apprentice. The deliverables are contractual artefacts, not essays; the requirements below name what must be present, not how to phrase it.

## Prerequisites

- Chapter [`02-grc-for-ai-vendor-evaluation-matrix.md`](../02-grc-for-ai-vendor-evaluation-matrix.md) read once, with the three vendor categories, the nine dimensions, the three invariants, and the two failure modes marked.
- Chapter [`01-grc-for-ai-reference-architecture-and-enterprise-integration.md`](../01-grc-for-ai-reference-architecture-and-enterprise-integration.md) — the reference architecture the matrix scores candidates against. Every dimension's rubric binds to a component or seam this chapter fixed.
- Chapter [`03-workflow-layer-intake-through-audit-packaging.md`](../03-workflow-layer-intake-through-audit-packaging.md) — the workflow spine the matrix's first dimension (workflow-coverage) scores against.
- Chapter [`04-rbac-and-segregation-of-duties-model.md`](../04-rbac-and-segregation-of-duties-model.md) — the persona set and SoD invariants the matrix's second dimension scores against.
- The mod-108 evidence-contract chapters — the evidence-schema-flexibility dimension binds to the evidence shape (provenance chain, freshness window, control binding, authoring/review personas, hash-based integrity) the enterprise's contract fixes.
- The mod-110 chapter [`03-observability-platform-wiring-and-the-single-source-of-truth.md`](../../mod-110-monitoring-and-post-market-surveillance-architecture/03-observability-platform-wiring-and-the-single-source-of-truth.md) — the normalisation-layer integration is one of the two highest-criticality integrations on the integration-surface dimension.
- Access to primary references — vendor product documentation for every candidate scored (mark all such references with `<!-- needs-research: ... -->`); ISO/IEC 42001 for the audit-body-track-record dimension; the enterprise's own CMK, residency, and data-classification policies for the deployment-model non-negotiables; enterprise budget envelope for the total-cost-of-ownership disqualifier. See [`../resources.md`](../resources.md).

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose GRC-for-AI vendor selection you are **least familiar with**; that is where the exercise will teach you most. State your choice at the top of the matrix schematic and every scorecard. Scenario choice constrains the deployment-model non-negotiables (residency regions, CMK posture, tenancy isolation for tier-1 data, embedded-AI-inference posture) and the jurisdiction-coverage weighting the matrix applies.

- **A US regional bank** (Northbrook Financial-style, per mod-101 exercise-02 and mod-102 exercise-01) with ~40 AI systems: fraud classifiers, document extraction, an internal RAG legal assistant, a customer-facing generative chat, third-party AI SaaS integrations. Colorado + NYC deployments; EU expansion planned. An SR 11-7-aligned MRM programme and an ISO/IEC 27001 ISMS already in place; internal audit function reports to the audit committee.
- **A global healthcare payer / provider** with clinical-decision-support pilots, patient-facing chat, coding automation, and utilisation-management AI. US federal HIPAA scope; EU AI Act relevance for European insurance subsidiaries; multiple US state deployments including Illinois and California. A classical clinical-safety oversight committee reports to the CMO; internal audit reports to the audit committee.
- **A B2B SaaS platform vendor** shipping GenAI-augmented HR-tech capabilities into enterprise customers across the US, UK, EU, and Singapore. Customers include public-sector deployments (subject to OMB M-25-21 shape) and financial-services enterprises (subject to SR 11-7 vendor-review shape). Internal audit is a small function co-sourced with a Big Four provider.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`evaluation-matrix-v1.0.0.yaml`** — the versioned matrix schematic, ratified jointly by the level-50 architect, the level-60 head of AI governance, the CISO, and the CIO's procurement lead per chapter 02. Version-locked before any vendor demo.
2. **`per-vendor-scorecards/`** — one markdown file per candidate for **three** candidates: one purpose-built AI-GRC vendor from the chapter-02 category list; one enterprise-GRC extension (or a ServiceNow AI Control Tower / IBM watsonx.governance-class incumbent); one build-in-house sketch. Each scorecard scores the candidate against **all nine** chapter-02 dimensions using the six-band rubric.
3. **`poc-plan.md`** — the proof-of-concept plan for the top-scoring candidate (or the build option, if the pattern of scores points there), scoped so it exercises the highest-criticality integrations (mod-108 evidence index; mod-110 normalisation layer) and the SoD invariants of the RBAC dimension.
4. **`build-vs-buy-recommendation.md`** — the decision document. Not a pick-highest-score; a reasoned reading of the pattern of scores against the two chapter-02 failure modes.
5. **`total-cost-of-ownership.yaml`** — the five-year TCO model per candidate across list price, integration engineering, professional services, ongoing maintenance, and migration at contract end. Migration cost is not optional.

## Requirements

### `evaluation-matrix-v1.0.0.yaml`

The versioned matrix schematic. Every block below is required.

- **Identity and version.** `id` (a stable identifier, e.g. `GRC-AI-VEM-<scenario>-v1.0.0`); `version: 1.0.0`; `authored_by: senior-ai-governance-architect`; `ratified_by: [ head-of-ai-governance, ciso, cio-procurement-lead ]`; `authored_before_vendor_demos: true`.
- **Scoring cadence.** `scoring_cadence` enumerating the three chapter-02 triggers: annually as vendors ship (Q1); before every contract renewal (T-minus-6-months); and within one quarter of any material change in the enterprise reference architecture.
- **Scoring bands.** `scoring_bands` enumerating all six chapter-02 bands with the numeric mapping: `native-first-class-verified` (5), `native-verified` (4), `extensible-verified` (3), `partial-verified` (2), `unverified` (1), `absent-or-disqualifying` (0). The meaning of each band must be stated inline so the scorecards cite a stable definition.
- **Dimensions.** `dimensions` enumerating all nine, each with a weight and a rubric block:
  - `workflow-coverage` — per-flow rubric enumerating all seven flows (`intake`, `impact-assessment`, `control-testing`, `evidence-collection`, `exception-handling`, `incident-routing`, `audit-facing-packaging`) with a weighted-mean aggregation.
  - `rbac-sod-flexibility` — rubric requiring all eight chapter-04 personas expressible and SoD violations blocked at the workflow layer (not the UI layer).
  - `integration-surface` — per-target rubric enumerating the reference-architecture integration targets (mod-102 control library; mod-105 AIMS documented information; mod-106 risk register; mod-108 evidence index; mod-110 normalisation layer; ML platform / model registry; runtime-security tooling; enterprise-GRC system of record; identity / SSO; communications; ticketing). Mark the mod-108 evidence index and the mod-110 normalisation layer as highest-criticality. Include an `api_shape_bonus` ordering (`api-first > ui-with-api > ui-only-with-export`).
  - `evidence-schema-flexibility` — rubric enumerating the four positions (`contract-bending`, `adaptable-with-effort`, `schema-imposing`, `partial`).
  - `jurisdiction-coverage` — per-regime rubric enumerating at least six regimes (`EU-AI-Act`, `NIST-AI-RMF`, `ISO-IEC-42001`, `SR-11-7`, `Colorado-SB24-205`, plus at least one sector regime scoped to the scenario). `bring-your-own-catalogue: required`.
  - `audit-body-track-record` — rubric on confirmable references across multiple certification bodies.
  - `deployment-model` — `non_negotiables` block enumerating residency, CMK for data at rest and in transit, tenancy isolation for tier-1 data, and CMK for embedded AI inference the platform itself performs.
  - `roadmap-velocity-and-viability` — rubric on 12-month-shipped-vs-committed, absorptive capacity for enterprise-driven requests, and runway posture.
  - `total-cost-of-ownership` — `five_year_model` enumerating list price, integration engineering, professional services, ongoing maintenance, and migration at contract end; `migration_cost_uncertainty_flag: true`.
- **Disqualifying conditions.** `disqualifying_conditions` enumerating at minimum: deployment-model fails a non-negotiable; five-year TCO exceeds the enterprise budget envelope; any dimension scores `unverified` across all POC attempts (vendor refused POC or POC did not proceed). Disqualifying conditions apply *before* the weighted aggregate; a candidate that trips one does not proceed to aggregation.

The matrix identifiers must be stable — the per-vendor scorecards will cite the dimension IDs, the band definitions, and the disqualifying-condition IDs from this file.

### `per-vendor-scorecards/*.md`

One file per candidate — three files total. Each file must include:

- **Vendor / candidate name** and **category** (purpose-built AI-GRC, enterprise-GRC extension, or build-in-house). Vendor-specific product-feature claims must carry `<!-- needs-research: ... -->` markers; do not paste marketing copy as verified fact.
- **Scenario declaration** — the same scenario chosen for the matrix; the deployment-model non-negotiables and jurisdiction-coverage weights the matrix applied.
- **Matrix version citation** — `scored_against: evaluation-matrix-v1.0.0` — so the scorecard is pinnable to a specific matrix version.
- **A scored row per dimension** — nine rows, one per chapter-02 dimension. Each row states: the band assigned (from the six-band rubric); the numeric equivalent; a short rubric-satisfaction note (what specifically about the candidate justifies the band); and either a POC observation reference (the test in `poc-plan.md` whose outcome supports the band) OR the explicit note `did not POC — remains unverified`. A band above `unverified` without a POC observation reference is an invariant-3 violation.
- **Aggregate weighted score** — the sum of `weight × numeric` across the nine dimensions, computed *after* the disqualification test.
- **Disqualification test outcome** — a named-and-numbered check against each disqualifying condition in the matrix. A candidate that trips any disqualifier does not receive an aggregate; the aggregate section states `disqualified — {condition-id}` instead.
- **Pattern-of-scores commentary** — a short prose subsection reading the shape of the scores across dimensions. Where does the candidate concentrate strengths (which dimensions cluster in the top two bands)? Where does it concentrate weaknesses? Does the pattern support a buy decision, a partial-buy-with-build-against-weak-dimensions, or a build decision? This is the input the recommendation document reads from; a scorecard without this subsection reduces to a spreadsheet row and fails failure-mode-(b).

The build-in-house scorecard scores the *same* nine dimensions — the internal engineering shape must be evaluated by the same rubric a vendor would be. Workflow-coverage for a build option is "which flows would the enterprise build first, and which stay manual until later phases"; integration-surface is "which integrations the enterprise's own platform team can terminate cleanly"; roadmap-velocity is "the enterprise's own delivery velocity on comparable platforms"; TCO is the five-year internal-engineering-plus-ops model.

### `poc-plan.md`

The proof-of-concept plan for the top-scoring candidate (or the build option, if the pattern of scores directs there). Required subsections:

- **Named test scenarios** — at minimum: (a) an evidence artefact of each mod-108 shape (fairness attestation; red-team finding-plus-productionisation record; LLM-quality evaluation run) written into the platform's evidence store and retrieved without loss; (b) a mod-110 normalisation-layer signal consumed through the platform's integration surface and surfaced as an observation on the appropriate register record; (c) a ticketing round-trip (an exception raised in the platform, routed to the enterprise ticketing system, resolved, and the resolution reflected back in the platform's state); (d) an IdP off-boarding revocation flow (a user disabled at the identity provider loses access to the platform within the SLA the CISO fixes); (e) SoD invariant tests — self-attestation refused (an author cannot mark their own evidence attested); validator-writing-development-artefact refused (a validator role cannot author the development-side artefacts the SR 11-7-aligned personas separate). Each test must be executable against the enterprise's own systems or a sandbox mirror; a test the enterprise cannot execute is not a POC.
- **Named success criteria per test** — the observable outcome that constitutes pass. "The workflow completed" is not a success criterion; "the evidence artefact retrieved from the platform matches the input byte-for-byte and the retrieval carries the provenance chain the mod-108 contract requires" is.
- **Time-box** — the calendar window the POC runs in (typical range: four to eight weeks); the seats the enterprise commits to the POC (level-50 architect, level-35 evaluation engineer, level-35 AI infra security, level-25 risk engineer); the vendor-side seats committed reciprocally.
- **Vendor-side deliverables required in advance** — the tenant configuration the vendor must provide (empty enterprise-shaped tenant, not a pre-loaded demo tenant); API credentials scoped to the integration targets under test; documentation for the SoD-relevant permissions surface; escalation contact for POC-blocking issues.
- **POC-refusal position** — the explicit statement that a vendor that declines the POC or scopes it away from the highest-criticality integrations does not score above `unverified` on the dimensions those integrations govern, per chapter-02 invariant 3.

### `build-vs-buy-recommendation.md`

The decision document. Required subsections:

- **Named decision.** One of: `buy vendor X`; `build`; `partial-build with vendor X covering specific dimensions {D1, D2, ...} and enterprise-built coverage of {D3, D4, ...}`. State the decision at the top; the reasoning below justifies it.
- **Reasoning against the pattern of scores.** Not the aggregate rank alone. Name the pattern: which dimensions the recommended candidate concentrates strengths in; which dimensions it concentrates weaknesses in; whether the weakness-pattern is one the enterprise can build against, integrate around, or must accept. Name the trade-off explicitly.
- **Failure-mode defence.** For each of the two chapter-02 failure modes: (a) picking on a demo without a reference-architecture-scored evaluation — name the specific evidence in the scorecards that this recommendation was reference-architecture-scored, not demo-scored; (b) treating the matrix as a spreadsheet — name the specific reading of the pattern of scores that this recommendation performs, beyond the weighted aggregate.
- **Migration-cost-at-contract-end risk position.** State the enterprise's exposure at year three or year five if the recommended vendor is not renewed; cite the TCO model's migration line; state the data-portability commitments the enterprise will negotiate into the contract to reduce the exposure.
- **Re-score commitment.** A named commitment to re-score against the matrix at year-end and before the first contract renewal, per the scoring-cadence block in the matrix. The commitment names the seat responsible and the trigger.

### `total-cost-of-ownership.yaml`

Five-year TCO model per candidate — three entries, one per scorecard. Each entry must include:

- **Per-year breakdown across the five terms** — `list_price`, `integration_engineering`, `professional_services`, `ongoing_maintenance`, `migration_at_contract_end`. Migration is modelled on the assumption the vendor is not renewed at year five (or year three for a break clause); the cost is the extraction of the register, the evidence index, the workflow state, the RBAC configuration, and the historical audit trail into a shape a successor platform can ingest.
- **Envelope check** — the enterprise's five-year budget envelope for the GRC-for-AI platform seat, and a comparison of each candidate's five-year total against it. A candidate whose total exceeds the envelope trips the TCO disqualifier in the matrix.
- **`migration_cost_uncertainty_flag`** — set true on any entry where the year-five migration cost is materially uncertain (vendor's data-portability posture unspecified in the contract, the enterprise has not verified the export shape in the POC, or the successor-platform market is unpredictable). A flagged entry carries a matrix flag on the total-cost-of-ownership dimension regardless of the numeric total.

Explicit ordering constraint: the matrix MUST be authored (and `version: 1.0.0` locked) *before* any scorecard is written. The scorecards must cite the matrix version in their header. This is chapter-02 invariant 1; the version-lock is what makes the discipline auditable. Every specific product-feature, integration, or roadmap claim in the scorecards MUST carry a `<!-- needs-research: ... -->` marker; the exercise does not require the learner to verify vendor documentation, but does require the learner to be honest that they have not.

## Starter guidance

- Author the matrix first, in full, and version-lock it before scheduling a single demo. The temptation to schedule "one exploratory demo" before the matrix is finished is what invariant 1 is designed against — the demo will reshape the rubric silently. The rubric is the instrument through which the demo is scored, not a document the demo helps produce.
- The proof-of-concept versus demo boundary is what separates **verified** from **unverified** across every dimension. A vendor that refuses a bounded, enterprise-controlled POC scoped to the matrix's highest-criticality integrations stays at **unverified** on every dimension those integrations govern — and a scorecard whose rows are almost all **unverified** is itself scoring signal about the vendor's fit for a level-50-serious enterprise.
- Disqualifying conditions apply *before* the weighted aggregate. A candidate that fails the deployment-model non-negotiables, exceeds the TCO envelope, or lands at **unverified** across all POC attempts does not receive an aggregate; the scorecard states the disqualifier and stops. Weighting a disqualified candidate against surviving candidates is a category error that the two failure modes both feed into.
- The pattern of scores is the real output of the exercise, not the aggregate rank. Two candidates with the same aggregate can point to very different decisions — one whose strengths cluster in workflow and RBAC and whose weaknesses cluster in integration and evidence-schema is a candidate the enterprise can build against; one whose strengths and weaknesses are inverted is a candidate the enterprise cannot. The recommendation document reads the pattern; the scorecard's pattern-of-scores subsection is where the reading is done.
- Do NOT invent vendor features to make a scorecard readable. A `<!-- needs-research -->` cell is a valid cell. A scorecard that fills every row with confident-sounding claims sourced from marketing copy is worse than a scorecard with honest research gaps — the confident scorecard mispresents the maturity of the evaluation, and the mispresentation is the mechanism by which failure-mode-(a) infiltrates the exercise.
- The build-in-house scorecard is not a rhetorical device. Score it as seriously as the vendor candidates, against the same nine dimensions, using the same six-band rubric. In some enterprise shapes the pattern of scores across the vendor candidates points to build; the exercise is valuable only if that outcome is a real possibility that the build scorecard can surface.
- For the healthcare scenario, weight jurisdiction-coverage against the sector-specific regime shape the enterprise carries (FDA guidance for AI/ML-enabled medical device software functions where applicable, HIPAA for health data, plus the US-state overlay); for the bank scenario, weight against SR 11-7 and OCC 2011-12 in addition to the AI-specific regimes; for the B2B SaaS scenario, weight against the customer-contract shape (some of the enterprise's obligations are inherited from public-sector or financial-services customers under OMB M-25-21 or SR 11-7 vendor-review shape).

## Acceptance criteria

- [ ] Chosen scenario is stated at the top of `evaluation-matrix-v1.0.0.yaml` and every scorecard, and every artefact is coherent against it.
- [ ] `evaluation-matrix-v1.0.0.yaml` includes identity/version, `authored_before_vendor_demos: true`, `ratified_by` covering the level-50 architect, level-60 head of AI governance, CISO, and CIO procurement lead, the three-trigger scoring cadence, all six scoring bands with numeric equivalents, all nine dimensions with per-dimension rubrics, and the disqualifying-conditions block.
- [ ] The workflow-coverage dimension enumerates all seven flows; the integration-surface dimension enumerates the reference-architecture integration targets and marks mod-108 evidence index and mod-110 normalisation layer as highest-criticality; the jurisdiction-coverage dimension enumerates at least six regimes; the deployment-model dimension names non-negotiables including CMK for embedded AI inference.
- [ ] Three scorecards exist in `per-vendor-scorecards/` covering one purpose-built AI-GRC vendor, one enterprise-GRC extension (or ServiceNow AI Control Tower / IBM watsonx.governance-class incumbent), and one build-in-house sketch. Each cites `scored_against: evaluation-matrix-v1.0.0`.
- [ ] Every scorecard row scores against all nine dimensions with band + numeric + rubric-satisfaction note + POC observation reference OR explicit `did not POC — remains unverified` note. No band above `unverified` lacks a POC reference.
- [ ] Every scorecard runs the disqualification test explicitly and reports outcome; a disqualified candidate carries `disqualified — {condition-id}` in place of an aggregate.
- [ ] Every scorecard includes a pattern-of-scores commentary subsection reading the shape of the scores across dimensions.
- [ ] `poc-plan.md` names test scenarios covering mod-108 evidence-index write; mod-110 normalisation-layer consumption; ticketing round-trip; IdP off-boarding revocation; SoD invariant tests (self-attestation refused; validator-writing-development-artefact refused). Each test has a named success criterion, and the plan states the POC-refusal position.
- [ ] `build-vs-buy-recommendation.md` names one decision (buy vendor X / build / partial-build with vendor X covering {D1, D2, ...}); justifies it against the pattern of scores; defends explicitly against both chapter-02 failure modes; states the migration-cost-at-end risk position; commits to re-score at year-end and before renewal.
- [ ] `total-cost-of-ownership.yaml` breaks five years per candidate across list price, integration engineering, professional services, ongoing maintenance, and migration at contract end; runs the envelope check; sets `migration_cost_uncertainty_flag` where year-five cost is materially uncertain.
- [ ] The matrix is version-locked before the scorecards are written; the scorecards cite the matrix version; no scorecard silently re-weighted the matrix.
- [ ] Every specific product-feature, integration, or roadmap claim about a named vendor carries a `<!-- needs-research: ... -->` marker. No marketing copy is pasted as verified fact.
- [ ] The recommendation document explicitly names the evidence that failure-mode-(a) (picking on a demo) and failure-mode-(b) (spreadsheet-not-decision-instrument) were both defended against.

## Stretch goals

- **Fourth-candidate sensitivity analysis.** Score a fourth candidate (from a different chapter-02 category than the three primary scorecards) against the same matrix and use it as a sensitivity check on the pattern-of-scores reading. If adding the fourth candidate would flip the recommendation, the pattern-reading in the primary recommendation was under-tested; the sensitivity analysis surfaces that.
- **Vendor-landscape drift test.** Run the matrix against the current-year vendor list and against a hypothetical next-year vendor list (adding one plausible new entrant per category and dropping one plausible exit). Report the dimensions where the drift most changes the pattern of scores — those are the dimensions where the matrix's weighting is most sensitive to market movement, and where the scoring-cadence commitment matters most.
- **RFP annex extracted from the matrix rubrics.** Author a one-page RFP annex that translates each of the nine dimensions' rubrics into vendor-answerable questions, scoped so a shortlisted vendor's written response gives the enterprise enough signal to decide whether a POC is worth scheduling. The annex is a downstream artefact of the matrix; if the rubrics are testable (invariant 2), the annex writes itself.
