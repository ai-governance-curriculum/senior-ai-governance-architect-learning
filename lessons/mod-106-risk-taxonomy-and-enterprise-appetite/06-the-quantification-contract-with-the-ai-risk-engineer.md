# The quantification contract — the architect / risk-engineer / evaluation-engineer interface

## Why this chapter exists

The taxonomy (chapter 02) names the categories; the appetite architecture (chapter 03) says what tolerance applies; the aggregation model (chapter 04) rolls per-system scores into a portfolio view; the frontier-lab shape lessons (chapter 05) name the pre-registration and eval-set-naming moves. All of that requires a *score* per (system, category) — an actual number that says "this system carries a residual of 5 on this category" — for any of the machinery to work. The score is what the level-25 `ai-risk-engineer` produces and what the peer level-30 `model-evaluation-engineer` calibrates the methodology for. The level-50 architect does not produce the score. The architect *designs the contract* that says how the score is produced, what its axes are, how the three risk stances (inherent / residual / control-defeated) are computed against the same axes, and what quality gates the score must clear to be accepted onto the portfolio view.

This chapter details that contract. It is not a scoring manual — the scoring manual is what the `ai-risk-engineer` learning track authors and calibrates, and the `model-evaluation-engineer` track calibrates the statistical methodology for. It is the *architect's contract*: the boundary the architect draws, the interface the two engineering roles fill in, the quality gates the architect owns. Getting this contract right is how the architect keeps the taxonomy usable without being drawn into the day-to-day scoring judgements the taxonomy exists to make defensible in the first place.

## The three roles and the interface

**The architect (level 50, this role).** Owns: the taxonomy shape, the appetite architecture, the aggregation model, the quantification *contract*. Does *not* own: the scoring judgement per system, the calibration methodology, the individual quality-gate decisions on scores.

**The AI risk engineer (level 25).** Owns: the scoring judgement per system — assessing a given (system, category) triple against the impact × likelihood × exposure axes the contract defines, producing an inherent / residual / control-defeated triple, and filing the score into the risk register with the required evidence. Reads the taxonomy the architect designs; scores against it; escalates to the architect when the taxonomy does not fit a case (chapter 07's amendment process is the response). Coordinates with the evaluation engineer on how the calibration works.

**The evaluation engineer (peer level 30, Governance family).** Owns: the *statistical methodology* that keeps scores comparable — inter-rater calibration, drift monitoring, agreement measurement, the choice of ordinal scales, the treatment of uncertainty in scoring. Reads the taxonomy; designs the calibration studies that determine whether the taxonomy is being applied consistently; hands calibration outputs back to both the architect (for taxonomy amendments where MECE-ness is failing — invariant 2) and the risk engineer (for scoring-practice adjustments).

The three roles compose. The architect draws the boundaries; the risk engineer scores within them; the evaluation engineer calibrates the scoring. Confuse any of the three and the interface degrades: an architect who insists on scoring produces a taxonomy that ossifies to whatever the architect happens to have scored recently; a risk engineer who insists on redrawing the taxonomy per-scoring-case produces the free-text failure mode chapter 01 warned against; an evaluation engineer who redesigns the taxonomy in the name of methodological rigour produces a MECE-perfect taxonomy that no operational team can actually use.

## The impact × likelihood × exposure decomposition

The score is always a decomposition. The architect's contract is to fix the axes; the risk engineer applies them per case; the evaluation engineer calibrates. The three-axis decomposition — impact × likelihood × exposure — is the shape the contract uses. It is FAIR-influenced (chapter 04 discussed the FAIR question), tuned for qualitative or semi-quantitative ordinal application in most enterprises, and richly extensible to quantitative FAIR-shaped monetisation for categories where the data supports it.

### Axis 1 — Impact

*Impact* is what the harm would cost in the specific instance under evaluation, if realised. It composes several sub-axes the contract enumerates per category (chapter 02's `residual_scoring_hint` block in the taxonomy schema starts this):

- **Reach.** How many parties are affected? A single individual? A protected class? An entire customer base? An ecosystem?
- **Severity per party.** How bad is the harm to each affected party? Reversible inconvenience? Financial loss? Rights violation? Physical injury? Loss of life?
- **Reversibility.** Can the harm be undone once realised? Is the process for undoing it well-defined and available to the enterprise?
- **Regulatory exposure.** Does realising the harm produce a regulatory notification obligation, a fine exposure, a private right of action, or an enforcement action?
- **Enterprise-strategic exposure.** Does realising the harm materially affect the enterprise's ability to conduct its core business? Its ability to enter new markets? Its ability to retain customers or talent?

The contract does not require the risk engineer to score all five sub-axes as independent numbers — that produces false precision. It requires that the risk engineer's overall impact score be *defended against* the sub-axes, with the driving sub-axes named in the score-record. A score of 8 on impact for a specific `discriminatory-decision-in-consequential-context` instance might be defended as "driven by reach (enterprise-wide customer base at scale) and regulatory exposure (Colorado SB24-205 private right of action attaches at the observed disparity level)"; the score-record carries that defence.

### Axis 2 — Likelihood

*Likelihood* is the probability that the harm is realised over a defined time horizon (the aggregation model specifies the horizon — quarterly, annually, per-deployment-cycle — depending on the category). Sub-axes:

- **Realisation pathway plausibility.** Given the system's current controls and deployment context, how plausible is the sequence of events that would realise the harm? For categories with named realisation scenarios (e.g., prompt-injection followed by tool-call), plausibility is scored against the scenario. For categories without a well-defined scenario (some emerging categories), the contract requires the risk engineer to *author the scenario* as part of the score defence.
- **Adversary model fitness.** Where the realisation pathway involves an adversary, how well-resourced and motivated is the assumed adversary? The contract carries a small set of adversary profiles (opportunistic outsider, motivated outsider, motivated insider, well-resourced adversary, nation-state) and the risk engineer names the profile per scoring case.
- **Observed base rates.** Where AIID / OECD.AI or industry incident data provides a base rate for the category on comparable systems, the risk engineer references it. The contract carries the *presumption* that base rates from AIID are a floor for likelihood; scoring likelihood below the observed base rate on comparable systems requires an explicit defence.

### Axis 3 — Exposure

*Exposure* is the *fraction of the enterprise's activity in scope for this system that would materially participate in the harm*. Exposure is what turns a per-instance score into a portfolio-relevant number. A system with high impact and high likelihood but that runs only in a bounded pilot with 50 users produces less portfolio-relevant exposure than the same system deployed enterprise-wide.

Sub-axes:

- **Deployment scope.** How much of the enterprise's activity currently flows through the system? A number and a denominator (queries per day, customers exposed, transactions decided, and so on) — the risk engineer records both.
- **Time horizon.** How long is the system deployed at the current scope? Systems in short-duration pilots have less exposure than systems running for years. The aggregation model (chapter 04) uses the time horizon to compute the exposure-weighted view.
- **Blast-radius attenuation.** Does the deployment context reduce blast radius (e.g., enterprise-internal-only, revocable session tokens, reviewable-before-action) or amplify it (customer-facing at scale, autonomous execution)? Chapter 02's axis 3 (deployment context) carries the coarse attributes; the risk engineer refines them into an exposure score.

### The composition

For most enterprises, impact × likelihood × exposure is composed on an ordinal scale (1-3, 1-5, or 1-9 are all common; the architect chooses one and the contract fixes it) and the composition is a defined function — often multiplicative (`impact × likelihood × exposure`, giving 1-27 or 1-125 or 1-729 depending on the base scale), sometimes a weighted sum with weights the architect ratifies. The contract *does not* leave the composition to per-scorer discretion — a defined function is used enterprise-wide, so scores are comparable.

Where the enterprise has the data and the discipline to graduate a specific category to FAIR-shaped quantitative monetisation, the composition is replaced for that category with a loss-event-frequency × loss-magnitude computation producing a monetary distribution. The two shapes co-exist in the risk register; the aggregation model (chapter 04) knows which is which and does not attempt to average across shapes.

## Inherent, residual, control-defeated — the three stances against the same axes

Chapter 04 argued that inherent, residual, and control-defeated are three lenses on the portfolio view. Here we specify how they are scored *against the same three-axis decomposition*.

- **Inherent risk.** The impact × likelihood × exposure score assuming *no controls*. The scoring imagines the system as designed and deployed without the enterprise's control library applied. Realisation pathways are scored with default control state (i.e., the countermeasures are absent). This is a *thought experiment* score; it is not comparing against the tolerance table. Its purpose is to characterise the exposure the enterprise is *taking on* by choosing to deploy the system at all.
- **Residual risk.** The impact × likelihood × exposure score assuming the *actually-deployed* controls (per mod-102 control library and per the system's current state, tracked in mod-108 evidence architecture) work as designed. This is the number that gets compared against the tolerance table (chapter 03). Residual assumes controls are operating correctly and evidence of their operation is fresh; if the evidence is stale, chapter 04's stale-residual escalation fires.
- **Control-defeated risk.** The impact × likelihood × exposure score assuming a specified control-defeat scenario. The scenario is *specific*: the risk engineer names which control(s) are assumed to fail and in what mode. Different defeat scenarios produce different control-defeated scores; the risk engineer typically produces a small set (three to five) of the most plausible defeat scenarios per (system, category) and the aggregation model reads the *worst* of them as the control-defeated view.

The three stances share the impact and exposure axes almost entirely (the harm reach, severity, and blast radius do not depend on whether the controls worked); the likelihood axis is where they differ (inherent likelihood is high because nothing stops the pathway; residual likelihood is lower because the controls stop most pathways; control-defeated likelihood is at the level assumed when a specific defeat scenario is realised). Scoring the three consistently is the risk engineer's discipline; the contract requires all three to be filed per (system, category) or the score is not accepted onto the portfolio view.

## Quality gates the architect owns

The architect does not judge individual scores. The architect owns *the quality gates that must be cleared for a score to be accepted onto the portfolio view*. Five gates:

**Gate 1 — Category assignment.** The score is filed against a valid taxonomy category at the current taxonomy version. If the risk engineer cannot find a category that fits, the response is a taxonomy amendment request (chapter 07), not a free-text `Other` filing. This gate is what enforces chapter 01's invariant 1 (closed-world enumeration).

**Gate 2 — Three-stance completeness.** The score record carries inherent, residual, and control-defeated scores. Missing any of the three sends the record back. This is what makes the aggregation model's three-view rendering (chapter 04) possible.

**Gate 3 — Sub-axis defence.** The impact and likelihood scores name the driving sub-axes in prose. A bare "impact = 7, likelihood = 5" without defence is not accepted. The defence is what makes the calibration study (below) tractable — the evaluation engineer can only measure inter-rater agreement on axes the scorers were reasoning against.

**Gate 4 — Evidence freshness.** The residual score references the evidence artefacts (per mod-108) that establish the control state assumed in the score. If the evidence is older than the category-specific freshness threshold (typically quarterly for most categories; monthly or weekly for high-turnover categories; ad hoc for regulatory-notification categories), the residual is *labelled stale* and the escalation trigger from chapter 03 fires.

**Gate 5 — Calibration currency.** The scoring methodology in use is the current calibrated methodology (per the evaluation engineer's ongoing calibration studies). Scores filed against an obsolete methodology are re-scored during the migration window; the aggregation model marks them as `re-calibration-pending` in the interim.

The five gates are enforced by the mod-111 GRC-for-AI platform tooling once the platform is stood up; the architect designs the gates, the platform implementation enforces them at intake, and the risk engineer scores against them at authorship.

## The calibration methodology — the peer-level-30 interface

Consistent scoring across systems and across scorers is not automatic. The evaluation engineer (peer level 30, Governance family) owns the methodology that keeps it consistent. The architect's interface to the evaluation engineer is what this section pins.

**Calibration study — the standard shape.** Periodically (typically quarterly during taxonomy roll-out, semi-annually once stable), the evaluation engineer runs a calibration study:

- A sample of representative (system, category) scoring cases is drawn from the risk register.
- Two or three risk engineers score the sample independently against the current methodology.
- Inter-rater agreement is measured (Krippendorff's alpha or an equivalent chance-corrected statistic; the exact choice is the evaluation engineer's — the architect's contract fixes that an agreement statistic is used, not which one).
- Agreement below a stated threshold (the architect ratifies the threshold — typically alpha ≥ 0.7 for the residual score, alpha ≥ 0.6 for the more speculative control-defeated score) is a *methodology defect* and triggers one or more of: taxonomy amendment (categories are not MECE — invariant 2), scoring-practice refinement (sub-axis definitions need sharpening), scoring-training programme (the risk engineers are drifting), aggregation-model adjustment (the aggregation is exposing inconsistencies the per-system scoring did not).

**The architect's role in calibration.** The architect *ratifies the agreement threshold*, *reviews the methodology-defect findings*, and *decides which findings drive taxonomy amendments*. The architect does not run the calibration study, does not choose the statistic, does not judge individual scoring disagreements.

**The escalation for methodology drift.** If calibration studies show agreement declining over time, the response is not "instruct the risk engineers to try harder." The response is a scheduled review of the taxonomy, the scoring contract, and the aggregation model together — one or more of the three is producing the drift. This is why chapter 07's versioning process is a joint versioning of taxonomy + tolerance table + aggregation model + contract, not four independent version streams.

<!-- needs-research: verify the current NIST US AISI methodology documents on AI system evaluation calibration, and any published methodological guidance from the model-evaluation profession (e.g., Stanford CRFM, MLCommons AILuminate) that the evaluation engineer role would reference. -->

## The boundary with `model-evaluation-engineer` — what the architect does *not* do

The evaluation engineer role at peer level 30 owns:

- The statistical methodology for the calibration studies.
- The choice of agreement statistics and the interpretation of their values.
- The methodology depth for evaluating models against the categories (the enterprise's system-level eval sets, chapter 05 adaptation 2).
- The design of the sampling strategies for the calibration studies.
- The integration with published external evaluation methodologies (US AISI, UK AISI, NIST AI 100-3 and similar) — bringing external methodology into the enterprise's calibration practice.

The architect does *not* do these things and should not attempt to. When the audit committee asks about the statistical rigour behind the score comparability, the answer is "the evaluation engineer has run calibration study Q3-2026 with Krippendorff's alpha of 0.74 on residual scoring, above our ratified threshold; the underlying methodology is documented at [reference]; here is the study report." The architect defends the *shape* of the calibration practice; the evaluation engineer defends the *methodology* the shape holds.

Similarly, when the calibration study surfaces a methodology defect — say, that risk engineers systematically score the `unsafe-agentic-action` category with low agreement because the category's definition is ambiguous about whether it covers *attempted* unsafe actions or only *successful* ones — the response is a taxonomy amendment the architect owns, informed by the methodology finding the evaluation engineer produced. The architect does not diagnose the methodology defect; the architect fixes the taxonomy.

## A schematic scoring-contract fragment

```yaml
quantification_contract:
  version: 1.3.0
  taxonomy_version: 1.2.0
  scale:
    axis: [1, 3, 5, 7, 9]  # semi-quantitative ordinal
    composition: multiplicative  # impact * likelihood * exposure
    resulting_range: [1, 729]
    tolerance_normalisation:
      # tolerance table cells reference this normalisation
      band_low: [1, 27]
      band_moderate: [28, 125]
      band_high: [126, 343]
      band_severe: [344, 729]
  three_stances:
    inherent:
      assumes: no controls in place
      purpose: strategic exposure characterisation; input to control-investment prioritisation
    residual:
      assumes: currently-deployed controls operating as designed with fresh evidence
      purpose: comparison against tolerance table
      staleness_thresholds:
        default: 90 days
        category_overrides:
          - category: regulatory-notification-obligation
            threshold: 30 days
    control_defeated:
      assumes: specified control-defeat scenarios realised
      purpose: tail-risk view
      minimum_scenarios_per_score: 3
      worst_case_used_for_aggregation: true
  quality_gates:
    - id: G1
      description: Category assignment against current taxonomy version
      enforced_at: scoring intake
    - id: G2
      description: Three-stance completeness
      enforced_at: scoring intake
    - id: G3
      description: Sub-axis defence in prose per axis
      enforced_at: scoring intake
    - id: G4
      description: Evidence freshness per residual staleness threshold
      enforced_at: scoring intake and portfolio rebuild
    - id: G5
      description: Calibration currency (current methodology version)
      enforced_at: scoring intake
  calibration:
    study_cadence: semi-annually
    statistic: krippendorff-alpha (default; evaluation engineer may substitute equivalent)
    agreement_thresholds:
      residual: 0.70
      inherent: 0.65
      control_defeated: 0.60
    methodology_defect_response: joint taxonomy + contract + aggregation review
```

Exercise-01 requires you to author a contract like this alongside the taxonomy. Exercise-02 requires you to defend the tolerance-normalisation banding against a specific board statement.

## Summary

The quantification contract is the architect / risk-engineer / evaluation-engineer interface. The architect fixes the three-axis decomposition (impact × likelihood × exposure), the composition function, the three-stance requirement (inherent / residual / control-defeated all scored against the same axes), the quality gates that must be cleared for a score to reach the portfolio view, and the calibration cadence and threshold. The risk engineer scores against the contract per system. The evaluation engineer calibrates the methodology and hands drift findings back to the architect for taxonomy amendment (chapter 07). The three roles compose; each is dependent on the other two for the whole system to be defensible. The contract is versioned alongside the taxonomy, the tolerance table, and the aggregation model — chapter 07 walks the joint versioning discipline that keeps all four in step and prevents the drift that turns the appetite architecture into a compliance-checkbox artefact.
