# The ongoing assurance programme — periodic, drift-driven, and incident-driven re-assessment

## Why this chapter exists

The pre-deployment gate (chapter 02) decides that a system is fit to launch *at a moment in time*. The moment passes. Data distributions shift. Adversaries adapt. The regulatory posture changes. The frontier-model provider ships a new version of the base model with different behaviour. A new state Act arrives. An incident happens on a comparable system in another enterprise and reveals a failure mode no one had scored for. The launch decision, correct on day one, is worth less every subsequent day until it is re-taken. Regulatory obligations that name the mechanism — the EU AI Act's Article 72 post-market monitoring, the Colorado AI Act's risk-management-programme cadence <!-- needs-research: verify the specific cadence obligations of Colorado SB24-205 against final rulemaking as adopted by the Colorado AG. -->, the FDA guidance on predetermined change-control plans for AI/ML-enabled medical devices, the OCC and Fed expectations under SR 11-7 that model validation is ongoing not one-shot — are all pointing at the same architectural gap: an assurance system that does not re-assess is not an assurance system.

The ongoing assurance programme is what closes the gap. It is the second-line-owned programme that specifies *when a system is re-assessed*, *what the re-assessment covers*, and *what outputs it produces* — periodic re-assessments on a tiered cadence, drift-driven re-assessments on quantitative triggers, incident-driven re-assessments on qualitative triggers, and regulatory-change re-assessments when the regime changes. This chapter designs it. It composes with the pre-deployment gate (chapter 02) — every re-assessment either re-affirms the gate decision or re-opens it — and with the risk register (mod-106) and the AIMS operations calendar (mod-105 chapter 08).

## What "ongoing assurance" is, structurally

The ongoing assurance programme is:

- A **second-line-owned programme** parallel in structure to the pre-deployment gate. The governance office designs and runs it; the evaluation engineer executes the methodology inside it; the risk engineer re-scores; the analysts collect evidence; the platform team surfaces telemetry; the model owner responds to findings.
- **Trigger-based** — re-assessments fire on periodic cadence, on drift thresholds, on incidents, and on regulatory changes. The four trigger types are not alternatives; they compose.
- **Tiered** — the re-assessment shape depends on the system's tier and its regulator-facing profile; not every re-assessment produces the same artefacts.
- **Coupled to the risk register** — every re-assessment writes back into the risk register (mod-106) and, where applicable, updates the SoA status and the risk-treatment plan (mod-105 chapters 05 and 06).

It is *not*:

- **Continuous monitoring in the operational sense.** That is a first-line responsibility — the model owner, the platform team, and MLOps run drift monitors, latency dashboards, and quality alarms as a matter of routine operations. Ongoing assurance is *second-line* — it *consumes* the operational monitoring outputs and decides what those outputs mean for the launch decision the pre-deployment gate took.
- **Optional.** Regulatory regimes with post-market monitoring obligations (EU AI Act, FDA AI/ML guidance, sector regulator MRM programmes) do not treat it as optional; certification bodies (ISO/IEC 42006) will find its absence a material nonconformity; independent auditors (ForHumanity IAAIS) will find the same.

## The four trigger types

### Trigger type 1 — periodic re-assessment on a tiered cadence

Every system in scope re-assesses on a defined cadence set by tier. The cadence balances two competing pressures: too-frequent re-assessments consume second-line capacity to no assurance benefit; too-infrequent re-assessments leave stale launch decisions in effect for periods long enough that the enterprise's risk posture drifts materially. The architecture fixes the cadence per tier:

| Tier | Periodic cadence | Rationale |
|---|---|---|
| Tier-1 (lowest-risk, internal-only) | Annually | Regulatory exposure minimal; drift risk contained by scope |
| Tier-2 (moderate-risk, internal or bounded external) | Annually, with light-touch mid-cycle check | Drift risk present; regulatory exposure limited |
| Tier-3 (consequential decisions, regulator-facing) | Semi-annually | Regulatory obligations require ongoing evidence; drift risk material |
| Tier-4 (highest-risk, safety-critical, or highly regulated) | Quarterly, plus continuous drift monitoring | Regulator expectation; the gate decision decays fastest at this tier |

The cadence is a *floor*, not a ceiling. A system may re-assess more frequently for cause; the cadence is what the second line commits to as a minimum discharge of its ongoing-assurance obligation.

Periodic re-assessments produce a *re-affirmation record* — a lighter artefact than the pre-deployment decision-record but structurally analogous: evidence-contract discharge state re-verified, residual scores re-read, control state re-checked, oversight and rollback re-checked, regulator-packaging currency re-checked. The re-affirmation either states "the launch decision holds unchanged" or specifies the changes (residual now higher; a control now stale; an artefact now overdue). Where the re-affirmation cannot be issued cleanly, the re-assessment produces a *conditional re-affirmation* (findings that must be closed by a stated date) or a *withdrawal* (the launch decision is withdrawn; the system is either taken out of service or subjected to a new pre-deployment gate).

### Trigger type 2 — drift-driven re-assessment

Drift is the quantitative degradation of the model's behaviour or of the environment it operates in, measured against pre-registered thresholds. The architecture pins:

- **What drift metrics are monitored.** Model-output drift (distribution of predictions), data-input drift (distribution of features or of prompts and their entities), performance drift on labelled ground truth where available, fairness drift on protected characteristics, adversarial-robustness drift where measured on periodic red-team samples, calibration drift on decision-boundary systems. The specific set depends on the system's category and tier; the pre-deployment gate's evidence contract pinned the set at launch.
- **What thresholds trigger a re-assessment.** The architecture specifies thresholds at two levels: an *operational-alarm* threshold that the first line responds to (routine drift, first-line remediates), and an *assurance-trigger* threshold that the second line responds to (material drift, ongoing assurance re-assessment fires). The two levels are chosen so that the operational alarm normally handles routine drift without escalating; the assurance trigger fires only when drift materially changes the residual-risk reading against the tolerance table.
- **How the trigger is filed.** The platform team's monitoring stack (mod-110) writes to a defined ingestion point; the second-line ongoing-assurance function receives the trigger and opens a re-assessment; the re-assessment convenes within a stated interval (typically 5 to 10 business days from trigger receipt).

Drift-driven re-assessments are *not lighter* than periodic re-assessments — they are typically deeper, because the trigger itself signals that the launch-decision-relevant assumptions have moved. A drift-driven re-assessment typically re-runs the evaluation suite (evaluation-engineer-authored), re-scores the affected risk-register entries (risk-engineer-authored), re-reads the residual against tolerance (analyst plus risk-engineer), and produces a decision: the launch decision holds, the launch decision requires conditions, or the launch decision is withdrawn pending remediation.

A typical drift trigger specification:

```yaml
drift_trigger:
  id: DT-2026-cust-facing-chat-fairness
  system: cust-facing-chat-v2.3.x
  metric: disparate_impact_ratio_by_protected_class
  operational_alarm:
    threshold: DIR < 0.85 sustained over 24h
    responder: first-line MLOps
    typical_response: investigate; adjust guardrail; if quick fix, remediate; if not, escalate
  assurance_trigger:
    threshold: DIR < 0.80 sustained over 24h, or DIR < 0.85 sustained over 7 days
    responder: second-line ongoing-assurance
    convene_by: T+5 business days
    minimum_scope: reassess risk register entries RR-2026-1442 and RR-2026-1443; re-run
                   fairness eval; re-read residual vs tolerance
    trace_upstream_to: pre-deployment gate PDG-2026-04-0087
```

### Trigger type 3 — incident-driven re-assessment

An incident is a realised harm — a serious-incident under the EU AI Act Article 73 shape, an incident classified against the taxonomy (mod-106) as tier-2 or above, an incident that fires the enterprise's post-market surveillance obligation (mod-110). Every incident triggers a re-assessment of the system on which it fired *and* re-assessment consideration for comparable systems in the portfolio. The architecture specifies:

- **The trigger filing.** The incident-management function (mod-110) files the trigger to the ongoing-assurance function at incident classification. The classification determines the re-assessment scope.
- **The re-assessment scope by classification.**
  - *Minor incident (tier-1 category, contained).* Re-assess the system's residual on the affected category only; typically a light re-assessment that either re-affirms or updates one risk-register entry.
  - *Material incident (tier-2 or tier-3 category, contained or with recovery).* Re-assess residual across the affected categories; re-check control effectiveness; consider whether comparable systems in the portfolio require a portfolio-wide re-assessment.
  - *Serious incident (tier-4 category; safety, life, rights, or material regulatory obligation).* Full re-assessment analogous to a pre-deployment gate. Regulator-facing packaging updated; sector regulator or supervisory authority notified per applicable regime (AI Act Article 73 requires notification of serious incidents to the market surveillance authority; sector-specific regimes carry their own notification obligations); audit committee informed at next scheduled meeting (or emergency meeting if the incident requires).
- **The portfolio consideration.** For material and serious incidents, the second-line function assesses whether other systems sharing controls, data provenance, or the frontier-model provider's base model with the affected system carry the same failure mode. Portfolio-wide re-assessments — even if each individual system's re-assessment is light — are how a single-system incident becomes a portfolio-scale corrective action rather than a per-system patch.
- **The corrective-action route.** Every incident-driven re-assessment produces at least one CAPA entry (mod-105 chapter 09) — root cause, corrective action, preventive action, effectiveness review. The CAPA closes when the effectiveness review passes, not when the corrective action is implemented.

### Trigger type 4 — regulatory-change re-assessment

The enterprise's regulatory posture is not static. New Acts pass; new implementing regulations arrive; sector regulators publish new expectations; case law changes what a private right of action means. The architecture must fold these into the ongoing programme:

- **A regulatory-horizon-scanning function.** Not the architect's role in operations — the head of AI governance (or a designated legal-and-regulatory seat) scans the horizon; the architect designs the intake by which horizon-scan output triggers ongoing assurance.
- **The re-assessment scope by change type.**
  - *Interpretive change* (new guidance clarifies an existing obligation). Typically a light re-assessment: confirm the enterprise's current practice aligns with the new interpretation; where not, open a CAPA to align.
  - *Additive obligation* (a new artefact, a new disclosure, a new filing). Re-assessment scopes the new obligation into the affected systems' evidence contracts; the pre-deployment gate contracts and the ongoing-assurance re-affirmations for affected systems are updated at the next cycle.
  - *Substantive change* (a new prohibition, a new risk classification, a materially different technical requirement). Full re-assessment analogous to a pre-deployment gate, potentially triggering system-level changes (rollback, retraining, redesign of the human-oversight layer). The Colorado AI Act, once its final rulemaking lands, is expected to require substantive-change-shaped re-assessments across affected US financial-services and employment-technology AI systems. <!-- needs-research: track the final Colorado AG rulemaking under Colorado Revised Statutes 6-1-1701 et seq. and update the substantive-change footprint. -->
- **The migration window.** Regulatory changes typically carry effective dates. The architecture pins that ongoing-assurance re-assessments for regulatory changes are *phased* — an initial re-assessment against the new obligation on publication (posture-alignment); a mid-cycle re-assessment as the enterprise's implementations land (progress-verification); a final re-assessment at effective date (readiness). The pre-deployment gate for any new launch after the change incorporates the change immediately.

## The routing into the risk register and the AI risk portfolio view

Every re-assessment writes back into the risk register (mod-106). The architecture specifies the writes explicitly, so no re-assessment produces findings that go nowhere:

- **Residual score updates.** Where the re-assessment changes a residual score (drift, incident, control state change), the score is re-filed with the effective date and the re-assessment id.
- **Category coverage adjustments.** Where a new category is being scored (a taxonomy amendment landed since last re-assessment; a new incident revealed a category that was missing), the register grows.
- **Control-defeated scenario updates.** Where a new attack, a new incident on a comparable system, or a new regulator expectation identifies a new plausible defeat scenario, the control-defeated score for that scenario is added.
- **Stale-residual escalation.** Where the periodic cadence lapses (an intended re-assessment did not occur), the residual is labelled stale and the chapter-03 tolerance-table stale-residual escalation from mod-106 fires.

The AI risk portfolio view (mod-106 chapter 04) is *not* a separate artefact — it is the aggregation over the risk register the ongoing programme keeps current. A portfolio view that has drifted from the register is a failure of the ongoing programme, not a rendering issue.

## The programme's operations calendar

The ongoing assurance programme sits inside the AIMS operations calendar (mod-105 chapter 08). The architecture publishes the calendar so first-line, second-line, third-line, and the board's audit committee can all read it without ambiguity:

- **Monthly.** Ongoing-assurance operational review (second-line internal meeting) — open re-assessments, drift triggers received, incident triggers received, regulatory triggers received, upcoming periodic re-assessments in the next 60 days. Second-line only; head of AI governance chairs or delegates.
- **Quarterly.** Ongoing-assurance status readout at the quarterly AIMS operational review — trend on re-assessment throughput, findings closure, stale-residual count, systemic patterns in drift and incidents, regulatory-change pipeline. Presented alongside the mod-105 chapter 08 quarterly package.
- **Semi-annually.** Portfolio-view refresh — the AI risk register aggregated to portfolio; presented to the audit committee alongside the third-line audit findings from chapter 04. This is where the audit committee gets the ongoing-assurance signal.
- **Annually.** Ongoing-assurance programme review — is the cadence right? are the drift thresholds still tuned? is the incident-driven scope adequate? are the regulatory-change response times materially responsive? The architect leads this review; the head of AI governance ratifies; findings feed the RTP.

## Composition with post-market monitoring (mod-110) and MRM

Two nearby programmes overlap materially with ongoing assurance; the architecture must specify the boundaries so effort is not duplicated and nothing falls between:

- **Post-market monitoring (mod-110).** The operational monitoring stack that surfaces drift, quality, safety, and incident signals. It is a first-line function (owned by MLOps / platform / product with second-line requirements-setting). Its outputs *feed* ongoing assurance — the drift triggers of trigger-type 2 come from mod-110; the incident triggers of trigger-type 3 come from mod-110's incident-management flow. Ongoing assurance does not build its own monitoring stack; it consumes mod-110.
- **Model risk management (SR 11-7-shaped MRM).** Where the enterprise runs a classical MRM programme under SR 11-7 (US banking) or a sector-specific analogue, the ongoing model-validation cadence prescribed by the MRM programme *is* the periodic re-assessment for models in MRM scope. The architecture composes the two — the MRM cadence does not run separately from ongoing assurance; ongoing assurance for MRM-in-scope models routes through the MRM function as the second-line executor, with the architect pinning the interface. Chapter 06 walks the analogous interface with the AI evaluation function.

## A schematic — the ongoing assurance programme

```yaml
ongoing_assurance_programme:
  version: 1.2.0
  owner: senior-ai-governance-architect (level 50)
  operator: head-of-ai-governance (level 60)
  triggers:
    periodic:
      cadence_by_tier:
        tier_1: annual
        tier_2: annual (with light mid-cycle check)
        tier_3: semi-annual
        tier_4: quarterly
      artefact: re-affirmation record
    drift:
      metric_registry_ref: monitoring-programme (mod-110)
      threshold_types:
        - operational_alarm  # first-line responds
        - assurance_trigger  # second-line responds
      convene_by: T+5 to T+10 business days
      artefact: drift re-assessment record
    incident:
      trigger_source: incident-management (mod-110)
      scope_by_classification:
        minor: single-category, single-system
        material: multi-category, portfolio consideration
        serious: full re-assessment; regulator notification; audit committee
      artefact: incident-driven re-assessment record + CAPA
    regulatory_change:
      horizon_scan_source: legal-and-regulatory function
      scope_by_change_type:
        interpretive: light
        additive: contract-updating
        substantive: full re-assessment
      phased_response: posture-alignment → progress-verification → readiness
  writes_back_to:
    - risk register (mod-106)
    - SoA (mod-105 chapter 05)
    - risk-treatment plan (mod-105 chapter 06)
    - CAPA register (mod-105 chapter 09)
    - portfolio view (mod-106 chapter 04)
  calendar:
    monthly: ongoing-assurance operational review
    quarterly: readout at AIMS operational review
    semi-annually: portfolio view to audit committee
    annually: programme review; RTP feed
  composition:
    consumes: post-market monitoring (mod-110)
    coexists_with: MRM function under SR 11-7 (via interface pinned by architect)
    feeds: third-line audit (chapter 04); external audit interface (chapter 05)
```

## Two failure modes to design against

**Failure mode 1 — periodic-only.** The ongoing programme runs on the calendar but never on triggers. Drift alarms fire and remediate at the first line without ever escalating; incidents are classified and closed by first-line teams without a second-line re-assessment; regulatory changes are handled by the legal function without an assurance re-assessment. The periodic re-affirmations proceed on schedule and re-affirm decisions that any of the untriggered events would have re-opened. The certification body's stage-2 audit finds the systemic pattern: re-affirmations that should have been re-assessments. The fix is architectural: the trigger definitions must be defined *quantitatively* (drift metric thresholds), *taxonomically* (incident classifications), and *contractually* (regulatory-change scope) so that a first-line team cannot suppress a trigger without leaving a trace the second-line function's monthly review catches.

**Failure mode 2 — trigger-flooded.** The opposite pathology. Every alarm is a second-line trigger; every incident, however minor, produces a full re-assessment; every regulatory change fires the substantive-scope response. The ongoing programme cannot execute at the volume; the second line becomes a queue that never drains; the periodic cadence lapses because the second line is buried in triggers. The audit committee sees a semi-annual portfolio view that is out of date because the second line has not had capacity to update it. The fix is the *operational-alarm vs assurance-trigger* separation, tuned carefully at drift-monitor registration time. First-line teams keep the operational alarms; the second-line assurance triggers fire only when the drift or incident materially changes the launch decision's assumptions.

## Summary

Ongoing assurance is the second-line-owned programme that keeps the pre-deployment gate's launch decision fresh. Four trigger types compose: periodic re-assessment on a tiered cadence, drift-driven re-assessment on quantitative thresholds, incident-driven re-assessment on taxonomy classifications, regulatory-change re-assessment on horizon-scan outputs. Each writes back into the risk register, the SoA where applicable, the risk-treatment plan, and the CAPA register — nothing falls off the second-line record. The programme's operations calendar sits inside the AIMS operations calendar, with monthly, quarterly, semi-annual, and annual cadences serving different consumers up to the audit committee. Composition with post-market monitoring (mod-110) and MRM under SR 11-7 is pinned at defined interfaces; ongoing assurance consumes monitoring outputs rather than replicating them. Two failure modes — periodic-only, trigger-flooded — are common and both prevented by tuning the trigger thresholds so the second-line function fires when it should and only when it should. The next chapter walks the third-line independent audit programme that samples the whole assurance system, including the ongoing programme, for the board.
