# exercise-05: Taxonomy Versioning and Migration Plan

**Estimated effort:** 2 hours

## Objective

Produce the **release packet** for a proposed *major* release of the joint (taxonomy + tolerance table + aggregation model + quantification contract) versioned artefact set — the packet that would land on the head of AI governance's desk for ratification and, because it is a major release, then on the audit committee's agenda. The packet exercises chapter 07's semver-shaped scheme, the amendment-proposal workflow, the seven-section migration-plan template, the two-way traceability guarantee, and the change-communications contract against a concrete, plausibly-motivated proposed change.

The deliverable is the artefact the architect ships with every release; getting it right is the difference between a taxonomy that stays operational for years and one that silently rots.

## Prerequisites

- Chapter [`07-taxonomy-versioning-and-migration-plan.md`](../07-taxonomy-versioning-and-migration-plan.md) — the joint-versioning discipline, semver semantics, amendment workflow, seven-section migration-plan template, two-way traceability guarantee, retirement pathway.
- Chapter [`06-the-quantification-contract-with-the-ai-risk-engineer.md`](../06-the-quantification-contract-with-the-ai-risk-engineer.md) — the calibration studies that surface many amendment proposals.
- Chapter [`02-harm-categories-capability-tiers-and-the-dependency-map.md`](../02-harm-categories-capability-tiers-and-the-dependency-map.md) — the schema you will be editing.
- The output of exercises 01, 02, and 03 as the versions you are shipping *from* (release 1.x) and *to* (release 2.0.0). If you did not do the earlier exercises, use the chapter fragments as your working v1.x baseline.
- The change-communications discipline sketched in mod-103 chapter 05 as prior art the enterprise's release process inherits.

## Scenario

You continue as the level-50 architect at your chosen enterprise. Twelve to eighteen months of operational experience on release 1.x of the taxonomy and its sister artefacts has produced a queue of amendment proposals from all three legitimate sources (chapter 07). You have consolidated them into a proposed **release 2.0.0** — a major release. The head of AI governance has asked for the full release packet before she takes it to the AI governance council and, subsequently, to the audit committee.

Your release 2.0.0 must incorporate **at least three amendments across three amendment kinds**. Choose from the following pool (or invent equivalents that are consistent with the pool's shape) — the goal is to exercise the migration-plan template against a genuine variety of change kinds:

- **Amendment A — calibration-driven category split.** A calibration study during Q2 of the current operating year returned an inter-rater agreement statistic (Krippendorff's α or Cohen's κ per chapter 06) below the enterprise's threshold on one existing category. The evaluation engineer recommends splitting the category into two successors whose distinction is testable.
- **Amendment B — environment-driven new category.** A regulatory development (EU AI Act Article 50 transparency implementing act; NIST AI RMF profile update; a new NAIC insurance model bulletin adoption in additional states; the appearance of a new dangerous-capability category in the most recent frontier-lab framework revision) requires the enterprise taxonomy to carry a category it currently lacks. Pick a specific regulatory or research development.
- **Amendment C — operations-driven dependency-edge revision.** The risk engineer has encountered enough real cases in which category X's residual is being scored as if it were independent of category Y — when in fact the causal relationship makes double-counting likely — that a `causes` or `escalates` edge in the dependency map must be added, changed, or removed.
- **Amendment D — deprecation and retirement of an under-used category.** A category from release 1.0.0 has been used in fewer than three risk-register entries across all systems over the past year. The AI governance analyst recommends deprecation with a suggested successor.
- **Amendment E — tolerance-table banding renormalisation.** Because amendment A (or an equivalent change) modifies the quantification-contract composition function, the tolerance-table banding must be re-normalised to preserve the semantics of the "low / moderate / high / severe" labels.
- **Amendment F — quality-gate addition to the contract.** A near-miss in the aggregation model (a stale residual entered the top-N view because the freshness gate did not have a hard-fail behaviour) requires the contract's quality gates to add a specific new check.

Your release 2.0.0 must include *at least one* amendment that requires *judgemental* re-mapping of historical entries (not just mechanical rename). That is where the migration plan earns its keep.

## Deliverables

Author four artefacts in a working directory of your choice:

1. **`amendment-proposals-v2.0.0.md`** — the amendment records for each proposed amendment, following chapter 07's amendment-record schema.
2. **`release-notes-v2.0.0.yaml`** — the release-notes fragment, following the schematic in chapter 07 (release header, change enumeration, migration-plan reference, two-way-traceability-guarantee statement, communications record).
3. **`migration-plan-v2.0.0.md`** — the full seven-section migration plan for the release.
4. **`communications-packet-v2.0.0/`** — a directory (represented as a single index file plus stubbed appendices) containing the per-audience communication artefacts named in chapter 07's change-communications contract.

## Requirements

### `amendment-proposals-v2.0.0.md`

For each amendment (at least three, from at least three amendment kinds from the pool above), an amendment record with:

- **`amendment_id`** — a stable identifier the release notes and migration plan will reference.
- **`source_and_driver`** — which of chapter 07's three sources (operations / calibration / environment change); what specifically prompted the proposal; who proposed it.
- **`proposed_change`** — the concrete text of what changes in the taxonomy, tolerance table, aggregation model, or quantification contract. For a new category, the full chapter 02 schema entry. For a split, the successor definitions and the rule that distinguishes them. For a dependency-edge change, the before-and-after fragment. For a banding renormalisation, the before-and-after band table.
- **`impact_assessment`** — which existing categories are affected; which tolerance-table cells need to be authored, retired, or edited; which aggregation-model dependency edges change; which risk-register entries need to be re-mapped (rough count).
- **`cross_corpus_mapping`** — for new categories, the composition against NIST GenAI Profile, ISO/IEC 23894, AIRO, MIT Repository, AIID / OECD.AI. Where a category has no reference-corpus mapping, defend the invention explicitly per chapter 02.
- **`migration_effort_estimate`** — how many historical risk-register entries are affected; whether the migration is mechanical or judgemental; the estimated hours or days to complete migration; who does the work.
- **`communication_plan_sketch`** — who is told; through what channels; what training or refresher content is required. The full communications packet is the fourth deliverable; this stanza sketches what will land in it.
- **`review_and_ratification_path`** — reviewers, ratifiers, target ratification date.

For at least one amendment, include the amendment-proposal being *rejected* on review with a rationale — the amendment workflow is not a rubber stamp, and demonstrating the rejection path is part of exercising the discipline.

### `release-notes-v2.0.0.yaml`

Follow the schematic release-notes fragment in chapter 07. Populate:

- **`release` header** — the four synchronised version numbers, release type (major), effective-from date, ratifiers with dates.
- **`changes` list** — one entry per change with `artefact`, `change_kind`, predecessor / successor identifiers, and a rationale that references the amendment record.
- **`migration_plan_reference`** — the filename of your migration-plan deliverable.
- **`two_way_traceability_guarantee`** — the version-migration-graph filename plus a one-sentence assertion that the guarantee holds for this release.
- **`communications`** — the audience → artefact → training-required → delivered-by table drawn from your fourth deliverable.

The release notes are the durable machine-readable record; the migration plan is the human-readable operating document. Both must be internally consistent.

### `migration-plan-v2.0.0.md`

The full seven-section template from chapter 07:

- **Section 1 — Change enumeration.** What changed, at what altitude, in each of the four artefacts. Not the full release notes; the enumeration of *migration-consequential* changes.
- **Section 2 — Historical risk-register entry re-mapping.** For each retired or renamed category and each split category, the mechanical / judgemental / no-remapping determination. For the judgemental re-mappings, the review cadence, the deadline, and who owns the queue. For the mechanical re-mappings, the tooling to be run and the completion criterion.
- **Section 3 — Two-way traceability guarantee.** How the version-migration graph is constructed for this release. What the audit-committee query "how has our exposure on category X evolved from Q1-2025 to Q3-2026?" would return under this release, worked through a specific X. This is the guarantee made concrete.
- **Section 4 — Tolerance-table migration.** Whether historical breach records are re-computed under the new bands or left under the old bands with a version stamp. Chapter 07's default is re-compute; if you re-compute, defend the trend-appearance change; if you do not, defend the temporal-discontinuity in the portfolio views.
- **Section 5 — Aggregation-model migration.** Whether historical portfolio views are re-computed under the new dependency edges. The default is *do not re-compute*; historical views under version N stay at version N; current views under version N+1 use the new edges. Defend any deviation.
- **Section 6 — Quantification-contract migration.** For contract changes (composition function, quality-gate additions, calibration threshold movement), the historical-score re-scoring plan: which entries, on what cadence, by whom, how the intermediate state (some old-contract, some new-contract) is rendered on the portfolio view during the migration.
- **Section 7 — Communication and training.** The delivery plan and the effective-date discipline: the release is not effective until the required communications are delivered and training is completed. This section references the communications packet.

Additionally, the migration plan must include a **coordination note** (chapter 07's closing section) covering mod-102 control library, mod-104 obligation register, and mod-108 evidence architecture — which sister artefacts need coordinated updates, in what sequence, before the release becomes effective.

### `communications-packet-v2.0.0/`

Represent the packet as a single index file plus stub files for each audience artefact from chapter 07's change-communications contract table:

- **`index.md`** — the table of audiences, artefacts, cadences, and delivery dates. Consistent with the `communications` block in the release notes.
- **`release-notes-2.0.0-engineer.md`** — a stub with the section headings the risk engineer's release notes carry (full release notes, migration checklist, training plan).
- **`analyst-executive-summary-2.0.0.md`** — a stub with the section headings the AI governance analyst's executive summary carries (new categories with definitions, changed workflow steps).
- **`sponsor-brief-head-of-ai-governance-2.0.0.md`** — a stub with the section headings the head-of-AI-governance sponsorship brief carries (why this release, what changes at the audit-committee altitude, what the head of AI governance is signing on for).
- **`audit-committee-brief-2.0.0.md`** — a stub with the section headings the audit-committee major-release brief carries (new stop-shipping thresholds, changes to the appetite architecture, request for ratification).
- **`internal-audit-completeness-report-plan.md`** — a stub describing the release-plus-60-days completeness report internal audit will receive: what is measured (percentage of historical entries re-mapped; queue burn-down; any deferred re-mappings with rationale).
- **`peer-team-summary-2.0.0.md`** — a stub for peer teams (data-risk, cyber-risk) on dependency-map changes that touch shared risks.

Stubs may be short (a heading list plus a paragraph of purpose) — the exercise is on the delivery *architecture*, not on writing every artefact end-to-end.

## Starter guidance

- **Draft the migration plan *before* the release notes.** The release notes are a summary of the plan's operative facts; the plan is where the thinking happens. Writing the notes first tempts you to hide inconvenient migration realities behind clean-looking version bumps.
- **Choose amendments that stress-test the plan, not amendments that flatter it.** An all-additive minor release does not test the seven-section template; a category retirement plus a banding renormalisation plus a contract composition change does. Pick amendments where the traceability guarantee has to work under real pressure.
- **The two-way traceability worked example is where the audit committee's confidence is won or lost.** Do not paraphrase it in the abstract. Walk it through one specific historical category, from a real (or plausibly-real) Q1-2025 name and definition to a Q3-2026 mapped category with a re-scored residual under the new contract. Show the round-trip: current-view-back-to-history and history-forward-to-current.
- **Rejected amendments are not a failure of the workflow; they are the workflow.** Include one on review. A workflow that ratifies every proposal it receives is a workflow that ratifies noise.
- **Coordination with mod-102 / mod-104 / mod-108 is where releases die.** A release that ships the taxonomy but leaves the control library referencing retired categories produces silent breakage. The coordination note is a real deliverable; do not treat it as a footnote.
- **Communications-packet stubs do not need to be full artefacts.** The exercise is on the *architecture* of the delivery — who gets what, when, in what shape — not on producing every audience artefact end-to-end. Each stub with the right headings suffices.
- **Do not skip section 3 of the migration plan.** The two-way traceability guarantee is the operational feature that separates the versioned taxonomy from the un-versioned one that silently rots. A migration plan without a concrete section 3 is chapter 07's failure mode 1 in shape if not in name.

## Acceptance criteria

- [ ] The release incorporates at least three amendments across at least three amendment kinds.
- [ ] At least one amendment requires judgemental (not mechanical) re-mapping of historical entries; the plan specifies the queue, cadence, and deadline.
- [ ] At least one amendment is walked through *rejection* on review, with a rationale.
- [ ] The four version numbers are synchronised across the release notes, and the release header cites all four along with the release type, effective-from date, and ratifiers.
- [ ] The migration plan has all seven sections populated; skipping any fails the criterion.
- [ ] Section 3 (two-way traceability guarantee) walks one specific category round-trip: current-view-to-history and history-forward-to-current.
- [ ] Section 4 explicitly states the re-compute-or-not decision for tolerance-table bands and defends the choice.
- [ ] Section 5 explicitly states the re-compute-or-not decision for historical portfolio views and defends the choice.
- [ ] The coordination note names the specific mod-102 controls, mod-104 obligations, and mod-108 evidence artefacts affected by the release.
- [ ] The communications packet index covers every audience in chapter 07's change-communications contract table (all rows represented).
- [ ] The release is not marked effective until the communications delivery and training completion dates are satisfied; the effective-from date is consistent with the delivery dates.
- [ ] Every amendment record cites its source (operations / calibration / environment change) explicitly; unsourced amendments are absent from the release.
- [ ] For new categories, the cross-corpus mapping to at least two reference corpora is populated, or the invention is defended explicitly.

## Stretch goals

- **Cross-version portfolio-view rendering.** Produce a two-page mock of what the portfolio view (from exercise-03) looks like on the day before and the day after release 2.0.0 becomes effective, for the specific category walked through in section 3 of the migration plan. Show how the two-way traceability guarantee makes the transition intelligible to the audit committee.
- **Retirement-pathway walk-through.** For amendment D (the deprecation), walk the full three-stage retirement pathway from chapter 07: the release in which deprecation is announced, the deprecation-period cadence and communications, the subsequent major release in which the retirement is completed. Timeline in months.
- **Amendment-workflow throughput sketch.** Author a one-page description of the enterprise's release cadence: how many amendments the workflow processes per quarter, how the review committee is chaired, what the queue-burn-down expectation is. Distinguish the amendment workflow's steady-state cadence from the major-release cadence. Preview of mod-112's org-shape work.
- **Preview mod-107 assurance.** Identify at least two places in the release where the second-line assurance function would independently verify the release *before* effective date (e.g., independent verification that the two-way-traceability graph is complete; independent verification that the historical re-scoring under the new contract preserves the appetite comparability that the tolerance-table migration promised). Name the specific assurance test.
- **Preview mod-108 evidence architecture.** Author the *release-record schema* the evidence architecture will hold for this release: what fields, what retention period, what access controls, what audit-defensibility properties. The release record is the artefact that lets an external auditor three years hence reconstruct why release 2.0.0 shipped the way it did.
