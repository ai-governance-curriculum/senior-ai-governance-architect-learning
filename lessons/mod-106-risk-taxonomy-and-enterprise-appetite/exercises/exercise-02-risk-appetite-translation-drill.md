# exercise-02: Risk Appetite Translation Drill

**Estimated effort:** 3 hours

## Objective

Translate a board-ratified AI risk-appetite statement into the three operationalising artefacts an enterprise can actually run against: a per-tier tolerance table, an escalation-trigger catalog, and a stop-shipping-threshold catalog. Ship them alongside a translation record that names, per category, exactly which sentence of the board statement drives each cell — and enumerates the open interpretation questions the architect refuses to close unilaterally.

The deliverable is the artefact you would take to a joint session with the head of AI governance, the chief risk officer, and general counsel to ratify the translation before it becomes the operating machinery for the enterprise's AI governance council.

## Prerequisites

- Chapter [`03-appetite-translation-from-board-statement-to-bench-triggers.md`](../03-appetite-translation-from-board-statement-to-bench-triggers.md) — the four artefacts, the parent frameworks, the ownership matrix.
- Chapter [`02-harm-categories-capability-tiers-and-the-dependency-map.md`](../02-harm-categories-capability-tiers-and-the-dependency-map.md) — you will attach tolerances to categories × tiers.
- Chapter [`06-the-quantification-contract-with-the-ai-risk-engineer.md`](../06-the-quantification-contract-with-the-ai-risk-engineer.md) — the scoring scale the tolerances normalise against.
- The output of exercise-01 as the taxonomy and quantification contract you attach to. If you did not do exercise-01, use the chapter 02 worked schema and chapter 06 contract fragment as your working versions.
- ISO 31000:2018, ISO/IEC 27005:2022, and COSO ERM as the parent framework references (title level is enough).

## Scenario

You continue as the level-50 architect at the enterprise you chose in exercise-01. The board of directors has approved the following risk-appetite statement (adapt lightly to your scenario — sector-specific language is fine; do not change the substance):

> **AI Risk Appetite Statement (draft — to be operationalised)**
>
> The [enterprise] deploys AI systems in the pursuit of its strategic objectives while accepting *low* levels of operational, compliance, and reputational risk arising from those deployments. We accept *no willingness* to expose customers, employees, or third parties to unlawful discrimination, to physical harm, or to loss of access to services on which they materially rely, arising from the deployment of AI systems under our control. We accept *moderate* innovation risk in exploratory internal pilots of generative-AI capabilities; we require material risk mitigation and demonstrable evaluation before customer-facing production deployment of any generative-AI capability. We accept *no willingness* to deploy AI capabilities on which the enterprise cannot demonstrate accountable human oversight commensurate with the capability's autonomy. We reserve any right to pause or discontinue deployment of AI capabilities on which residual risk cannot be maintained within these tolerances. This statement will be reviewed by the audit committee semi-annually and by the full board annually.

Additional context the head of AI governance has given you:

- The board would like to see specific stop-shipping thresholds for the highest-scrutiny categories (physical-safety-adjacent, unlawful-discrimination, loss-of-service-access) before the next audit committee meeting in six weeks.
- The chief risk officer wants the tolerance table to be *compatible* with the ISMS's ISO/IEC 27005-shaped risk-acceptance criteria the enterprise already runs.
- General counsel has asked for open questions to be surfaced explicitly rather than closed by architectural judgement.

## Deliverables

Author three artefacts in a working directory of your choice:

1. **`tolerance-table-v1.0.0.yaml`** — the per-tier tolerance table.
2. **`escalation-and-stop-shipping.yaml`** — the escalation-trigger catalog and stop-shipping-threshold catalog.
3. **`translation-record.md`** — the translation from the board statement to the tables, with open questions for legal and for the head of AI governance.

## Requirements

### `tolerance-table-v1.0.0.yaml`

For each category × tier combination in your taxonomy (from exercise-01), populate a cell with:

- `tolerance_score` — the maximum acceptable residual under the chapter 06 scale.
- `rationale` — one sentence pinning the cell to the driving sub-axis (why *this* score at *this* tier for *this* category).
- `jurisdiction_override` — where a specific jurisdiction (EU AI Act high-risk, Colorado, NYC LL 144, HIPAA, sector-specific) attaches a stricter tolerance, enumerate it per chapter 03's cell schema.
- `sector_override` — where a specific sector amplifier applies.
- `user_population_override` — where vulnerable populations attach stricter tolerances.
- `hard_red_line: true` — where the cell is a stop-shipping trigger regardless of scoring (these categories also appear in `escalation-and-stop-shipping.yaml`).

Enforce the three chapter 03 invariants — monotonicity in tier, explicit overrides with no implicit inheritance, hard-red-line separation.

### `escalation-and-stop-shipping.yaml`

Two sections:

**Escalation triggers.** For each of the three chapter 03 trigger kinds (approach-of-tolerance, breach-of-tolerance, aggregate-portfolio), enumerate at least two concrete triggers with:

- `threshold_definition` — the specific quantitative or qualitative rule that fires the trigger.
- `addressee_list` — the roles who receive the escalation.
- `response_cadence` — how quickly a response is required and to whom.
- `artefact_produced` — the record the response leaves (chapter 03: escalations without artefacts do not survive).

**Stop-shipping thresholds.** At least four thresholds addressing the categories the board statement makes hard limits on. For each:

- `threshold_definition` — the categorical property of the system that triggers refusal.
- `driving_board_sentence` — quote or paraphrase the sentence in the appetite statement that motivates this threshold.
- `override_authority` — the level at which override may (or may not) occur; the board-ratified thresholds should have no manager-level override.
- `evidence_required_for_release` — what would have to be produced to release the system for deployment despite the threshold.

### `translation-record.md`

For each category in your taxonomy, a translation record with:

- `board_statement_reference` — the specific sentence or clause that drives the tolerance for this category.
- `tolerance_table_row_ids` — the cells produced in `tolerance-table-v1.0.0.yaml`.
- `tolerance_derivation` — the reasoning that maps the sentence to the specific tolerance numbers.
- `escalation_triggers` and `stop_shipping_thresholds` — the identifiers of the corresponding entries.
- `open_questions_for_legal` — interpretation questions where the tolerance depends on a legal reading (e.g., "does the enterprise's understanding of 'materially rely' cover a customer-service chatbot that some customers rely on to reach human support?"). At minimum three across the whole translation record.
- `open_questions_for_head_of_ai_governance` — enterprise-scope questions where the tolerance depends on a policy decision (e.g., "does the enterprise treat internal-employee-facing systems as customer-facing for the purposes of the moderate-innovation-risk clause?"). At minimum two.

Additionally, a **compatibility-with-ISMS section**: one page describing how the tolerance table interoperates with the enterprise's existing 27005-shaped risk-acceptance criteria on the categories that overlap (information-security-adjacent AI risks). Where the two frameworks would reach different conclusions on a shared risk, document the reconciliation.

## Starter guidance

- Draft the translation record *first*, category by category. If you cannot defend a tolerance in prose against the board statement, the YAML will be prettier but arbitrary.
- Do not use a stricter tolerance than the board statement demands just because "more careful is safer." The board's appetite is *the* answer to what tolerance is enforced; over-strictness is not conservative, it is a governance failure that reduces the credibility of the whole architecture.
- Where the board statement is genuinely ambiguous, resist the temptation to close the ambiguity yourself. Log the open question and let the head of AI governance / legal / audit committee close it.
- The hard-red-line cells are the ones the audit committee will read most carefully. Do not proliferate them — every hard-red-line is a real stop-shipping commitment. If you have more than a dozen across your whole tolerance table, the architecture is probably wrong.
- The ISMS compatibility section is where the CRO will pay the closest attention. Concrete alignment on shared risks is what makes the AI governance function credible to the sibling ISMS function.

## Acceptance criteria

- [ ] The tolerance table covers every category × tier combination from your taxonomy (missing cells fail the criterion).
- [ ] Every cell has a one-sentence rationale — no bare numbers.
- [ ] Overrides are explicit per cell; no implicit "if EU tighten by two" global rules.
- [ ] Hard-red-line cells are separately enumerated in the stop-shipping catalog.
- [ ] The escalation-trigger catalog has ≥ 2 triggers per kind (approach, breach, portfolio) with all four required fields populated.
- [ ] The stop-shipping catalog has ≥ 4 thresholds each with the driving-board-sentence quote.
- [ ] Every category in the taxonomy has a translation record with board-sentence reference, derivation, and open questions where applicable.
- [ ] At least three open questions for legal and at least two for the head of AI governance are surfaced.
- [ ] The ISMS compatibility section describes concrete reconciliation for at least three overlapping risk categories.
- [ ] Monotonicity-in-tier holds (test: sort tolerances per category by tier; scores should be non-increasing).

## Stretch goals

- Add a *quantitative sensitivity analysis* section: for the three most consequential tolerance cells, describe how they would move if the board statement's "low" / "moderate" language were tightened or relaxed by one band.
- Author a *board-facing one-pager* that summarises the four artefacts in language the audit committee reads, with three chart-shaped renderings (heatmap of tolerances, top-N escalations, stop-shipping catalog with driving statements).
- Preview *mod-107 assurance*: identify at least three cells where the tolerance depends on the second-line assurance function's confidence in the residual score. Note where the audit function's independence would strengthen the tolerance's defensibility.
