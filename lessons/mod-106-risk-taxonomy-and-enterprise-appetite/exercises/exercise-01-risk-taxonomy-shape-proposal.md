# exercise-01: Risk Taxonomy Shape Proposal

**Estimated effort:** 3 hours

## Objective

Produce the **enterprise AI risk taxonomy shape proposal** for a specified enterprise scenario — the artefact you would take to the head of AI governance to ratify *before* the taxonomy is used to score any actual risk. The deliverable is a decision document plus a starter set of 12–15 categories fully composed from the reference corpora plus a companion quantification contract fragment.

Downstream artefacts in this module (exercise-02 tolerance table, exercise-03 aggregation model, exercise-05 migration plan) and downstream modules in this track (mod-107 assurance, mod-108 evidence, mod-110 monitoring, mod-111 GRC platform) all bind to the shape you propose. Get it right and everything composes; get it wrong and every downstream artefact inherits the flaw.

## Prerequisites

- Chapter [`01-the-risk-taxonomy-problem-and-the-architects-lens.md`](../01-the-risk-taxonomy-problem-and-the-architects-lens.md) — the invariants and the failure modes.
- Chapter [`02-harm-categories-capability-tiers-and-the-dependency-map.md`](../02-harm-categories-capability-tiers-and-the-dependency-map.md) — the composition move and the schema fragment.
- Chapter [`06-the-quantification-contract-with-the-ai-risk-engineer.md`](../06-the-quantification-contract-with-the-ai-risk-engineer.md) skimmed for the scoring axes.
- The mod-102 chapter 01 anatomy-of-a-control-library-entry as reference — your capability-tier axis will bind to the applicability filter shape.
- Access to the primary reference corpora: NIST AI RMF 1.0 + Generative AI Profile (AI 600-1); ISO/IEC 23894:2023 (Annex A/B — title level is enough if you do not have the paywalled standard); AIRO / VAIR ontology publications; MIT AI Risk Repository; AI Incident Database and OECD.AI Incidents Monitor. See [`../resources.md`](../resources.md).

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose risk shape you are least familiar with; that is where the exercise will teach you most. State your choice at the top of the deliverable.

- **A US regional bank (Northbrook Financial-style, per mod-101 exercise-02 and mod-102 exercise-01)** with ~40 AI systems: fraud classifiers, document extraction, an internal RAG legal assistant, a customer-facing generative chat, and third-party AI SaaS integrations. Colorado + NYC deployments; EU expansion planned. SR 11-7-aligned MRM programme and ISO/IEC 27001 ISMS already in place.
- **A global healthcare payer / provider** with clinical-decision-support pilots, patient-facing chat, coding automation, and utilisation-management AI. US federal HIPAA scope; EU AI Act relevance for its European insurance subsidiaries; multiple US state deployments including Illinois and California.
- **A B2B SaaS platform vendor** shipping GenAI-augmented HR-tech capabilities into enterprise customers across the US, UK, EU, and Singapore. Customers include public-sector deployments (subject to OMB M-25-21 shape) and financial-services enterprises (subject to SR 11-7 vendor-review shape).

## Deliverables

Author three artefacts in a working directory of your choice:

1. **`taxonomy-shape-proposal.md`** — the decision document.
2. **`taxonomy-v1.0.0.yaml`** — the starter set of 12–15 categories in the chapter 02 schema.
3. **`contract-v1.0.0.yaml`** — the quantification contract fragment paired to the taxonomy.

## Requirements

### `taxonomy-shape-proposal.md`

Decide and justify **each** of the following:

- **Axis-1 (harm category) design.** Enumerate the categories the enterprise adopts. For each: parent NIST GenAI Profile category, parent ISO/IEC 23894 risk source(s), MIT Repository subdomain, AIID category (if mapped), rationale for adoption. Categories that are on the enterprise footprint but not in any reference corpus require an explicit *invented* flag and a defence.
- **Axis-2 (capability tier) design.** Adopt or adapt the four-attribute tier definition from chapter 02 (modality access × action authority × data blast radius × autonomy horizon). Enumerate 4 or 5 tiers with concrete definitions and illustrative example systems from the scenario. State the closed-world convention (a system is exactly one tier).
- **Axis-3 (deployment context) design.** Enumerate the sector, user-population, and jurisdiction attribute values the enterprise carries. State any additional deployment-context attributes the enterprise's footprint requires.
- **Dependency map.** For your starter set of categories, enumerate at least six `causes` / `escalates` / `subsumes` / `mitigation-conflicts` edges you defend. The map should be small on release 1 (dozens of edges max, not hundreds).
- **Composition against reference corpora.** For at least three categories, produce the composition worksheet: the crosswalk to all five reference corpora and a one-sentence rationale per edge.
- **Reference-corpus omission defence.** For at least two categories that appear in the MIT Repository or the NIST GenAI Profile that you *decided not to adopt*, defend the omission — footprint mismatch, sector irrelevance, or premature (research-only) status are all legitimate reasons.
- **Invariant enforcement.** For each of the six invariants from chapter 01, name the specific design choice in your proposal that enforces it and the test that would detect its violation.
- **Non-scope.** At least three things you *chose not to* include in the taxonomy and why. Candidates: severity-hard-coded categories (invariant 3 violation), full-text descriptions in the register (closed-world violation), regime-specific labels (belong in the obligation register, not the taxonomy).

### `taxonomy-v1.0.0.yaml`

Populate the chapter 02 schema (the `RSK-CAT-042` fragment as reference) for 12–15 categories drawn from your enterprise scenario. Each entry must include:

- The three axes (`axis_harm`, `axis_capability`, `axis_deployment`) with the enumerated values.
- The `parents` block covering NIST AI 600-1, ISO 23894, MIT Repository subdomain, AIRO term (if used), AIID category (if mapped).
- At least one `dependency_edges` entry where the category has known dependencies; if you decline to add any, state so explicitly in the entry's notes.
- The `residual_scoring_hint` block with the impact / likelihood / exposure sub-axes the category emphasises.
- The `version_history` block starting at 1.0.0.

Where a citation cannot be verified from a primary source at authoring time, use `<!-- needs-research: ... -->` inline. Do not invent article numbers, section numbers, or subdomain names.

### `contract-v1.0.0.yaml`

Companion quantification contract with:

- The ordinal scale (1-3, 1-5, or 1-9 — chapter 06 lists the trade-offs).
- The composition function (multiplicative, weighted-sum, or FAIR-shaped for selected categories).
- The three-stance definitions (inherent / residual / control-defeated) with the freshness thresholds for residuals.
- The five quality gates from chapter 06 with the specific evidence artefacts each gate demands.
- The calibration cadence and the agreement threshold you propose.

## Starter guidance

- Draft the axis-1 categories *before* the schema fragments. If you cannot name the categories in prose, the YAML will be prettier but incoherent.
- Start from the NIST GenAI Profile section 2 list and the MIT Repository's 23 subdomains as your candidate pool. Cut ruthlessly — the enterprise's operational taxonomy should be far smaller than either source.
- Resist the urge to invent categories. If a proposed category maps to no reference corpus at all, re-check whether the reference corpora carry a category you missed. Genuinely-novel categories exist (some agentic-tool-misuse categories are recent) but are the exception.
- Do not conflate capability tier with harm category. If you find yourself proposing "tier-4 hallucination risk" as a distinct category from "tier-2 hallucination risk," you are violating invariant 4 — the tier is the axis, not the category name.
- The dependency map should reflect actual causal understanding, not aesthetic completeness. An edge you cannot defend in one sentence should not be added.
- For the healthcare and B2B SaaS scenarios, the *sector* attribute drives more of the taxonomy than for the bank. Do not paper this over.

## Acceptance criteria

- [ ] The scenario is stated at the top and the taxonomy is coherent against it (no categories that make no sense for the chosen scenario).
- [ ] 12–15 categories in the YAML, each with the full chapter 02 schema populated (missing fields fail the criterion).
- [ ] Every category has ≥ 2 parent references across NIST, ISO, MIT, AIRO, AIID (chapter 02's minimum for composition defensibility).
- [ ] At least six dependency edges, each with a one-sentence defence.
- [ ] At least three categories are walked through a composition worksheet in the proposal document.
- [ ] At least two reference-corpus categories are explicitly rejected with rationale.
- [ ] Each of the six chapter-01 invariants has a design-choice-plus-detection-test entry.
- [ ] The quantification contract has all sections from the chapter 06 schema fragment populated.
- [ ] Every unverified citation is marked `<!-- needs-research: ... -->` — no invented section numbers, subdomain labels, or AIID category names.
- [ ] `catalog-shape-proposal.md`-style non-scope section names at least three things deliberately excluded and why.

## Stretch goals

- Add a *sensitivity analysis* to the proposal: for the three categories where your composition choices are most consequential, describe how the taxonomy shape changes if the enterprise's scenario is modified (e.g., healthcare payer expands into direct-to-consumer telemedicine, adding a `vulnerable-population` amplifier).
- Sketch what changes in the taxonomy if the enterprise adds a *sector overlay* (mod-113 preview) — one paragraph per adjacent sector the enterprise might expand into.
- Add a *MITRE ATLAS* mapping to at least three of your categories (adversarial-tactic references from the ATLAS matrix). Preview of mod-108 evidence architecture.
- Take the *AI Incident Database* and randomly sample 20 recent incidents; walk each against your 12–15 categories and record the coverage (how many fit; which do not). Report the coverage number in the proposal and defend it.
