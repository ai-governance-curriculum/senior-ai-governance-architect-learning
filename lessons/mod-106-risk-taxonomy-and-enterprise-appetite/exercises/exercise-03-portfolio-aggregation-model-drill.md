# exercise-03: Portfolio Aggregation Model Drill

**Estimated effort:** 3 hours

## Objective

Design the enterprise's AI risk aggregation model and produce a worked quarterly portfolio view from a supplied per-system risk register. The deliverable exercises the three legitimate aggregation shapes (worst-case, exposure-weighted, dependency-graph), the three risk stances (inherent / residual / control-defeated), and the three portfolio-view artefacts (residual heatmap, top-N list, tail-risk register) — and forces you to defend the aggregation-shape choice per (category, view) cell rather than picking one shape globally.

The deliverable is the specification the risk engineer (level 25) will implement against and the worked example the head of AI governance will take to the audit committee to show how the portfolio view actually reads.

## Prerequisites

- Chapter [`04-aggregation-and-the-portfolio-view.md`](../04-aggregation-and-the-portfolio-view.md) — the three shapes, the three stances, the three artefacts, and the failure modes.
- Chapter [`02-harm-categories-capability-tiers-and-the-dependency-map.md`](../02-harm-categories-capability-tiers-and-the-dependency-map.md) — the dependency map you will read.
- Chapter [`06-the-quantification-contract-with-the-ai-risk-engineer.md`](../06-the-quantification-contract-with-the-ai-risk-engineer.md) — the scoring axes producing the residuals.
- Exercise-01's taxonomy and exercise-02's tolerance table as the substrate. If you did not do the earlier exercises, use the chapter 02 / 03 fragments as your working versions.

## Scenario

You continue as the level-50 architect at your chosen enterprise. The head of AI governance has given you a *synthetic per-system risk register* covering 20 AI systems across the enterprise footprint — a mix of tier-1 through tier-4 systems, with per-(system, category) inherent / residual / control-defeated scores filed against your taxonomy. The register is your input; the portfolio view is your output.

Because this is a training exercise, you author the synthetic register yourself as part of the deliverable. The register should be *coherent* against the scenario (a healthcare-payer register looks nothing like a bank register) and *representative* of the exposure shape a real enterprise carries — some categories have a single hot system, some categories are broadly distributed across the fleet, at least one category has an active cascade path via dependency edges, at least one category has a large residual-to-control-defeated gap on a tier-4 system.

## Deliverables

Author four artefacts in a working directory of your choice:

1. **`aggregation-model-spec-v1.0.0.yaml`** — the aggregation model specification.
2. **`synthetic-register-v1.0.0.yaml`** — the 20-system synthetic register you will aggregate against.
3. **`portfolio-view-2026Q3.yaml`** — the worked quarterly portfolio view produced from the register per the specification.
4. **`portfolio-narrative.md`** — the accompanying audit-committee narrative that walks the three artefacts.

## Requirements

### `aggregation-model-spec-v1.0.0.yaml`

Specify:

- **Per-category aggregation-shape assignment.** For each category in your taxonomy, which shape (worst-case / exposure-weighted / dependency-graph) applies to which portfolio view (residual heatmap / top-N / tail-risk register). Justify the choice per (category, view) cell in one sentence — chapter 04 argues worst-case for tolerance comparison, exposure-weighted for narrative, dependency-graph for cascade.
- **Exposure denominators.** For each category that uses exposure-weighted aggregation, name the exposure denominator (e.g., transactions decided, consequential-employment-decisions, patients-affected) and the source of the denominator (which mod-108 evidence artefact or metric feed provides it).
- **Dependency-graph traversal rules.** For each `causes` / `escalates` edge in your dependency map, specify how the aggregation composes the residuals. The chapter 04 guidance is: `causes` edges de-duplicate; `escalates` edges add a conditional risk. Pin the specific rule per edge kind so the risk engineer's implementation is deterministic.
- **Stale-residual handling.** How the aggregation treats residuals with stale evidence (per chapter 06's freshness gates) — do they enter the heatmap with a stale label, are they excluded from the top-N, are they treated as control-defeated for the tail view?
- **Version stamping.** The taxonomy / tolerance / contract version stamps that every score and every view carries, per chapter 07's traceability guarantee.
- **Quality checks.** The self-tests the aggregation must pass before publication (e.g., every heatmap cell has ≥ 1 driving system named; top-N counts sum consistently; every tail-view entry has a mod-110 monitoring-signal reference).

### `synthetic-register-v1.0.0.yaml`

A 20-system register. For each system:

- `system_id`, `system_name`, `capability_tier`, `deployment_context` attributes.
- `active_categories` — the subset of the taxonomy that applies to this system (not every category applies to every system; the applicability filter from mod-102 tells you which).
- For each active category, an inherent / residual / control-defeated triple with the sub-axis defence prose from chapter 06.
- At least three systems where the residual is `stale` under your freshness thresholds.
- At least two systems that participate in a dependency cascade (one system produces outputs that another consumes; realising the upstream risk causes the downstream).
- At least one system whose residual-to-control-defeated gap on some category is ≥ 4 on your ordinal scale.

The synthetic register is *your invention*, but it must be defensible — a system with tier-4 autonomy and a residual of 1 across all categories is not credible.

### `portfolio-view-2026Q3.yaml`

Apply the aggregation model to the register and produce the three artefacts:

- **View A — residual heatmap.** All (category, tier) cells with the aggregation shape labelled, the residual score, the tolerance from exercise-02, the breach flag, and the driving system(s).
- **View B — top-N list.** N = 10. Ranked by residual delta from tolerance. Each entry with system, category, residual, tolerance, delta, escalation status, and the chapter 03 escalation trigger it fires (if any).
- **View C — tail-risk register.** Top 5 categories by aggregate residual-to-control-defeated gap. Each with the largest-gap systems, the control(s) whose defeat opens the gap, the AIID / OECD.AI historical incident lineage (chapter 02 requires the mapping; use it), and the current monitoring signal freshness.

### `portfolio-narrative.md`

An audit-committee-facing narrative, roughly 3–4 pages, that walks the three views. Must contain:

- A one-paragraph "state of AI risk" opening: what the portfolio looks like today, at high altitude.
- A per-view walkthrough of the two or three most consequential entries.
- A tail-view walk of the single largest residual-to-control-defeated gap: what would have to fail for the tail to realise, what the enterprise would do if it did, and what monitoring signal would detect it early.
- A cascade section: one worked cascade path from the dependency graph, showing how an upstream risk feeds a downstream one and how the aggregation reads it without double-counting.
- A quarter-over-quarter section: two or three sentences (invented for the exercise, marked as illustrative) on how the portfolio has moved. This exercises the two-way traceability requirement chapter 07 argues for.

## Starter guidance

- Design the aggregation-model spec *before* you author the register. If you design the spec to produce a pretty-looking view from an arbitrary register, you have inverted the discipline.
- Do not paper over the tail view. The single most common failure mode in enterprise portfolio views is a smoothed tail. If your tail register produces uncomfortable-looking numbers, that is the exercise working — the discomfort belongs in the audit-committee's attention, not in an averaging step.
- Do use worst-case for anything compared against the tolerance table. Chapter 04 is emphatic on this and there is no scenario where averaging against tolerance is defensible.
- The cascade walk is where dependency-graph aggregation earns its keep. If your register does not produce at least one cascade, add the systems and edges that would.
- The stale-residual handling is where the aggregation shows whether it takes evidence freshness seriously. A view that treats stale residuals as if they were fresh is a view that misleads the audit committee.

## Acceptance criteria

- [ ] The aggregation-model spec assigns a shape per (category, view) cell with a one-sentence rationale each.
- [ ] The synthetic register covers 20 systems with the tier / context / active-categories fields populated.
- [ ] The register contains at least three stale-residual systems, at least two cascade participants, and at least one large residual-to-control-defeated gap.
- [ ] The portfolio view's heatmap covers every (category, tier) combination for which the register carries a score.
- [ ] The top-N list is ranked correctly by delta from tolerance and identifies the driving systems.
- [ ] The tail-view register carries the historical-incident lineage from AIID / OECD.AI for each entry, or flags where the mapping is not established.
- [ ] The narrative walks all three views and demonstrates one cascade path.
- [ ] Every score and every view carries the taxonomy / tolerance / contract version stamps per chapter 07.
- [ ] The aggregation-model spec passes its own quality checks against the register (e.g., every heatmap cell names at least one driving system).
- [ ] The tail view does not average control-defeated scores — chapter 04 forbids smoothing the tail; violation of this rule fails the criterion.

## Stretch goals

- Add a *FAIR-shaped monetisation stretch view* for two categories where the data would support it. Present it as a *co-existing* view, not a replacement for the ordinal portfolio view, per chapter 04's guidance.
- Author a *scenario overlay*: recompute the portfolio view under an assumed adversarial event (one large-blast-radius system's control-defeat scenario realises). Show how the top-N and tail views change and what the appropriate escalation would look like.
- Preview *mod-110 post-market surveillance*: for the tail-view register, identify which category needs a monitoring signal that does not yet exist and specify what the mod-110 architecture would have to add.
- Sketch what changes in the aggregation model if the enterprise expands to a second business unit that carries a *different* taxonomy version. Preview of chapter 07's cross-version traceability.
