# Aggregation and the portfolio view — rolling risk up without hiding the tail

## Why this chapter exists

The per-system view of AI risk is not the view the audit committee reads. The audit committee reads a *portfolio view*: "what are our top ten AI risks across the enterprise?", "how much residual risk are we carrying in the `discriminatory-decision` category across the fraud, credit-decisioning, and customer-service systems combined?", "have we crossed our aggregate portfolio tolerance in the `confidential-information-exfiltration` category?", "are we accumulating dependent risks (chapter 02's dependency map) that would cascade if any one of them realised?" No single system's risk register answers any of these questions. Rolling per-system risks into a portfolio view is a distinct architectural problem — done well, it makes real portfolio-level risks visible; done naïvely, it hides the tail, double-counts dependent risks, and creates a misleading green-yellow-red dashboard that is worse than no dashboard.

The level-50 architect owns the *aggregation model* — the choice of how per-system residuals compose into portfolio-level residuals, how inherent-vs-residual-vs-control-defeated risks stack, how the dependency map from chapter 02 factors into cascade views, and how tail-risk is preserved rather than smoothed away. The risk engineer (level 25) implements the aggregation on the actual risk register. The head of AI governance (level 60) uses the portfolio view to run the AI governance council and to report to the board. Chapter 06 details the per-system scoring contract; this chapter details what happens when scores compose.

## The three risk stances the aggregation reads

Every risk on the register is a triple: *inherent risk* (the risk score before any controls), *residual risk* (the score after the deployed controls are assumed to work as designed), and *control-defeated risk* (the score if the deployed controls are defeated by an adversary, by a failure mode, or by an operational lapse). The three are useful for different purposes:

- **Inherent risk** answers "what would the exposure be with no controls?" It is the *unmitigated* view. The board and the risk committee use it to reason about strategic exposure — categories with high inherent risk require serious control investment regardless of current mitigations. Inherent risk is also the baseline against which control effectiveness is measured (inherent minus residual is the *control contribution*).
- **Residual risk** answers "what is our current exposure assuming the controls work?" It is the *nominal* view — the number that gets compared against the tolerance table (chapter 03). Most day-to-day governance operates against residual risk.
- **Control-defeated risk** answers "what is our exposure if the controls fail?" It is the *tail* view. Adversarial defeat, operational lapse, and dependency failures are the failure modes; the control-defeated score assumes the identified controls do not, in a specific scenario, function. Control-defeated risk is what the audit committee cares about most for the highest-priority categories, because that is where the enterprise-level catastrophe lives.

The three views are not alternatives; they are three lenses on the same portfolio. The aggregation model must render all three. A portfolio dashboard that shows only residual risk implies false comfort — the residuals look manageable because the controls are assumed to work. A dashboard that shows only inherent risk implies false alarm — the exposure looks unmanageable before the controls that mitigate it are accounted for. A dashboard that shows only control-defeated risk implies a paranoia the audit committee will discount within a quarter. The three-view rendering is the discipline.

### The residual-to-control-defeated gap is the tail

The gap between residual risk and control-defeated risk is the *tail* — the exposure the enterprise takes on by relying on the controls. Categories where the gap is small are categories where the enterprise is not really exposed to control failure. Categories where the gap is large are categories where a single control defeat produces enterprise-scale harm. The tail is where every historical AI risk catastrophe lives. The aggregation model must foreground the tail — the audit committee should be able to ask "which categories have the largest residual-to-control-defeated gap?" and get an answer without the head of AI governance having to compute it manually.

## The three legitimate aggregation shapes

There are three defensible ways to compose per-system risk into a portfolio view. Each has a different use case and different failure modes. The architect chooses per (category, view) which shape is applied and documents the choice in the aggregation model specification.

### Shape 1 — sum-of-worst-case

For each category, the portfolio view carries the *worst-case per-system score* across all systems in scope. If any tier-4 system carries a residual of 7 on `confidential-information-exfiltration`, the portfolio residual on that category is at least 7. Additional systems at lower scores do not lower the portfolio number, and the worst-case is not averaged away.

The worst-case shape is the *only* defensible shape for the **residual-vs-tolerance** comparison. The tolerance table's cells are per-tier; a portfolio residual on a category can breach tolerance if any single system breaches it. Averaging across systems creates the pathology where three low-scoring systems and one high-scoring system produce a portfolio "moderate" that hides the one system that is actually above appetite.

Worst-case is also the correct shape for the **stop-shipping-threshold view** — a stop-shipping trigger on any one system stops that system regardless of other systems' scores.

### Shape 2 — exposure-weighted

For each category, the portfolio view aggregates per-system residuals weighted by *exposure* — a proxy for how much of the enterprise's actual activity flows through the system. Exposure weights are usually chosen from a small closed-world set: transaction count, active-user count, revenue impact, safety-critical-user count. The exposure-weighted aggregate answers "what is the portfolio-average exposure a customer / transaction / user experiences on this category?"

Exposure-weighted aggregation is defensible for the **portfolio-scale reporting** view — the audit committee reading "aggregate exposure in the discriminatory-decision category across all consequential-decision systems" wants a weighted view, not the worst-case-per-system view. It is *not* defensible for the tolerance comparison — averaging washes out the outlier that is the actual risk. The two views are rendered side by side; neither replaces the other.

Exposure-weighted aggregates need a defined *exposure denominator* per category. For `discriminatory-decision-in-employment-context`, the denominator might be "number of consequential employment decisions made by AI in the last 90 days." The denominator is enumerated in the aggregation model specification; the risk engineer's implementation reads from the mod-108 evidence architecture (which tracks per-system metrics) to compute it.

### Shape 3 — dependency-graph aggregation

For each category, the portfolio view accounts for the dependency map from chapter 02 — the `causes`, `escalates`, `subsumes`, and `mitigation-conflicts` edges. Dependency-graph aggregation is what prevents the naïve failure of double-counting and the naïve failure of missing cascade paths.

Two concrete uses:

- **Cascade view.** If risk `A` on system `S1` *causes* risk `B` on system `S2` (because `S1` produces outputs that `S2` consumes), the portfolio view carries a cascade path `A@S1 → B@S2` with a joint likelihood computed against the individual probabilities and the cascade transfer coefficient. The cascade view is what makes multi-system exposures visible; the naïve per-system view misses them entirely because no single system's register carries the cascade.
- **De-duplication view.** If risk `B` is *caused* by unmitigated risk `A` and the enterprise is already carrying the `A` residual, the aggregation model does *not* additively sum an independent `B` residual — the `B` residual is conditional on `A`, and summing produces double-counting. The dependency edges tell the aggregation model to compute the marginal contribution of `B` given `A` rather than the independent contribution.

Dependency-graph aggregation is the most sophisticated of the three shapes and the most likely to be omitted from a first-cut aggregation model. It is the reason chapter 02 argued for the dependency map as a first-class artefact rather than a nice-to-have. Without the dependency edges, the aggregation model runs blind on the cross-system exposures that are, in practice, where enterprise-scale AI risk lives.

## What the aggregation model produces — the portfolio-view artefacts

Three artefacts, produced on a cadence the head of AI governance and the audit committee ratify:

**Artefact A — the portfolio-level residual heatmap.** Rows: taxonomy harm categories. Columns: capability tiers. Cells: the aggregate residual across all systems in that (category, tier) combination, with the aggregation shape (worst-case / exposure-weighted / dependency-graph) labelled per cell. Cells that breach the tolerance table's corresponding cell are highlighted; cells that are close to breach (per the escalation triggers of chapter 03) are separately highlighted. The heatmap is the audit committee's *what is currently the state*.

**Artefact B — the top-N portfolio risks by category and by system.** A ranked list of the enterprise's currently-top-N residual risks — typically N=10 or N=20 — with each entry showing the harm category, the system(s) driving the risk, the residual score, the corresponding tolerance, the delta, and the escalation status. The top-N view is what the audit committee reads as narrative; the heatmap is what they read as scan. The two together are the standing report; individually neither is sufficient.

**Artefact C — the tail-risk register.** For the top-K categories (usually K=5 or K=10) by residual-to-control-defeated gap, a per-category record of (a) the largest gap systems, (b) the control(s) whose defeat opens the gap, (c) the historical incidents in AIID / OECD.AI that realised this category's tail, and (d) the current tail-monitoring signal (mod-110 will bind here). The tail-risk register is what the audit committee reads when they want to reason about *what could go really wrong*; the residual heatmap and the top-N list do not, by design, render this well because they are averaged or ranked views.

The three artefacts are rendered together in the standing AI risk portfolio report — quarterly to the AI governance council, semi-annually or annually to the audit committee, depending on the enterprise's governance cadence.

## What the aggregation model must *not* do

**Do not compress the three risk stances into one score.** A "combined risk score" that averages or otherwise blends inherent, residual, and control-defeated collapses the exact distinctions the audit committee reads. Render them separately; let the reader compose. This is the same discipline chapter 01's invariant 3 (separation of what from how much) enforced at the category level; here it applies to the stance level.

**Do not compress the three aggregation shapes into one number.** A "portfolio score" that is a single number per category is either worst-case (in which case it is the same as shape 1 and the exposure-weighted and dependency-graph views are lost) or an average (in which case it hides the worst-case and defeats the tolerance comparison). The three shapes are three views. The dashboard renders three; the report walks three.

**Do not smooth the tail.** Averaging control-defeated scores across systems, showing "average tail exposure," or computing a "portfolio-wide expected loss" that discounts the low-probability high-consequence corner *are* legitimate quantitative techniques in some risk methodologies (they are how insurance-industry portfolio risk usually reads). They are *not* the primary view for AI risk aggregation because the categories the audit committee cares most about are the low-probability high-consequence ones — the tail *is* the story, not a discountable region of the distribution. FAIR-shaped monetisation (see below) can be a stretch view for the categories that admit it; it is not the default.

**Do not confuse the aggregation model with the taxonomy.** The aggregation model reads the taxonomy (categories, tiers, dependency edges) but does not change it. If aggregation is producing weird numbers because a category is not MECE against another, the fix is a taxonomy amendment (chapter 07), not an aggregation-model workaround. Adding aggregation-model kludges to work around a taxonomy defect is how governance systems ossify.

## The FAIR question — quantitative monetisation as a stretch, not a default

FAIR (*Factor Analysis of Information Risk*) — an Open Group standard for quantitative information-risk analysis — offers a well-developed methodology for expressing risk in monetary terms (annualised loss expectancy, loss event frequency × loss magnitude, with distributions rather than point estimates). It is popular in mature information-security risk functions and has been applied to AI risk with mixed success.

The architect's position on FAIR-shaped monetisation for AI risk:

- **Do not adopt FAIR as the *primary* portfolio-view methodology.** FAIR requires calibrated frequency and magnitude estimates that AI risk categories, especially the emerging ones, do not yet have. Confabulating those estimates to get a monetary number produces false precision and, worse, a false portfolio ranking driven by which categories happen to have quantitative research behind them (some financial-fraud AI categories have very good data; agentic-tool-misuse categories have almost none). The audit committee will make decisions that are worse than the ones they would make with a well-executed qualitative aggregation.
- **Do adopt FAIR-shaped monetisation as a *stretch* view for the small subset of categories where the data supports it.** Categories with well-calibrated historical loss data — fraud-model errors in specific products, some cybersecurity-adjacent AI risks where the ISMS already runs FAIR — can carry a FAIR view alongside the qualitative aggregation. Label the FAIR view clearly; do not let it substitute for the tolerance comparison; do not let it drive the top-N view for categories without the data.
- **Do use the FAIR *decomposition* — likelihood × magnitude — even where the monetary quantification is not warranted.** Chapter 06's impact × likelihood × exposure scoring contract is a FAIR-influenced decomposition rendered qualitatively (or semi-quantitatively on an ordinal scale). This is the sound part of FAIR for AI risk today; the monetary quantification is the part to hold in reserve.

<!-- needs-research: link to current FAIR Institute / Open Group C20 publication references for FAIR; note any newer AI-specific FAIR extensions the FAIR Institute has published. -->

## The mod-102 control library binding

The aggregation model reads the taxonomy (chapter 02) for categories, capability tiers, and dependency edges. It also reads the mod-102 control library for the per-system control set and the evidence contract that establishes whether the controls are working. The binding is important: a residual score assumes a control state, and the aggregation model must be told the control state per system for the residual to be trustworthy. The evidence architecture (mod-108) provides the per-system control-state signal; the aggregation model queries it.

The specific binding:

- Each residual score is annotated with the control-set version and the evidence-freshness stamp used to compute it.
- Control state that has aged beyond a threshold (e.g., no test artefact in the last quarter) demotes the residual to a *stale-residual* label and escalates per the trigger chapter 03 defines.
- Control-defeated scores are computed against the taxonomy's *documented mitigation-conflict* edges and the risk engineer's per-system defeat scenarios (chapter 06 details the process).

## A schematic portfolio-view record

```yaml
portfolio_view:
  reporting_period: 2026-Q3
  aggregation_model_version: 3.1.0
  taxonomy_version: 1.2.0
  tolerance_table_version: 2.1.0
  view_a_heatmap:
    - category: confidential-information-exfiltration-via-extraction-attack
      tier: 3
      aggregation_shape: worst-case
      residual: 6
      tolerance: 5
      breach: yes
      breach_since: 2026-08-01
      driving_systems: [S-104-fraud-classifier, S-207-legal-rag]
      escalation_status: breach-triggered escalation open; review 2026-09-15
      inherent: 8
      control_defeated: 8
      residual_to_defeated_gap: 2
    - category: discriminatory-decision-in-employment-context
      tier: 3
      aggregation_shape: exposure-weighted
      exposure_denominator: consequential-employment-decisions-90d
      exposure_denominator_value: 4820
      residual: 3
      tolerance: 3
      breach: no
      approach_trigger: yes
      approach_since: 2026-07-12
      driving_systems: [S-311-resume-screen, S-402-promotion-recommender]
  view_b_top_n:
    - rank: 1
      category: confidential-information-exfiltration-via-extraction-attack
      driving_system: S-104-fraud-classifier
      residual: 6
      tolerance: 5
      delta: +1
      status: breach
    - rank: 2
      category: unsafe-agentic-action
      driving_system: S-511-customer-service-agent
      residual: 5
      tolerance: 4
      delta: +1
      status: breach
  view_c_tail:
    - category: unsafe-agentic-action
      gap: 5
      largest_gap_system: S-511-customer-service-agent
      defeated_by_control: AIC-HOV-014 (human-in-the-loop-tool-invocation)
      historical_incident_reference: AIID-2024-numbered-incident-here
      tail_monitoring_signal:
        source: mod-110-tool-invocation-audit-log
        freshness: 2026-08-05
```

Exercise-03 requires you to author a portfolio-view record like this for a hypothetical enterprise footprint and defend the aggregation-shape choices per cell.

## Summary

Portfolio-level AI risk is a distinct architectural problem: rolling per-system residuals into a view the audit committee reads without hiding the tail, without double-counting dependent risks, without smoothing away the worst-case, and without collapsing inherent / residual / control-defeated into a single misleading number. The aggregation model uses three legitimate shapes — worst-case (the only defensible shape for tolerance comparison and stop-shipping), exposure-weighted (for the audit committee's per-customer / per-transaction narrative view), and dependency-graph (for cascade detection and de-duplication of dependent risks) — and renders three artefacts (the residual heatmap, the top-N list, the tail-risk register). It renders the three risk stances (inherent, residual, control-defeated) side by side, foregrounding the residual-to-control-defeated gap because that gap is where enterprise-scale catastrophe lives. It reads from the taxonomy (chapter 02), applies against the tolerance table (chapter 03), and consumes control-state signal from the mod-102 control library and mod-108 evidence architecture. FAIR-shaped monetisation is a stretch view for the small set of categories with the data to support it, not the default. Chapter 05 studies the frontier-lab frameworks where analogous aggregation lives at very different scale; chapter 06 details the quantification contract that produces the residuals the aggregation reads; chapter 07 walks the versioning of the aggregation model alongside the taxonomy.
