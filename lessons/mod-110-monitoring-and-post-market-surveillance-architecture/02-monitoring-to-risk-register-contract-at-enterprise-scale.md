# The monitoring-to-risk-register contract at enterprise scale

## Why this chapter exists

An enterprise that stands up model-observability platforms, SOC detections for AI-specific attacks, and in-house telemetry for AI systems will, within a few quarters of shipping AI at scale, own tens of thousands of monitor emissions per day. Every one of those emissions is a signal about a system in production; a fair proportion is, or should be, a signal that the *risk posture* of that system has moved. And yet in most enterprises those signals never reach the risk register. Fiddler alarms fire into the MLOps team's Slack channel; Arthur's fairness drifts go to a dashboard someone looks at on Tuesday; WhyLabs data-quality checks page the platform-on-call; the SOC's prompt-injection detections open a security ticket that closes when the payload is blocked; the risk register sits on a GRC platform being read by the risk engineer for a system whose actual behaviour that engineer cannot see. The register and the runtime diverge silently. The portfolio view (mod-106 chapter 04) reports residuals that are stale. The audit committee sees a picture the systems in production have long since falsified.

The `ai-risk-engineer` (level 25) is authoring the *team-scope* contract for their own team's systems — *this* monitor writes *this* register field for *this* model at *this* cadence. That instance-level contract is necessary and, without a shape it must fit into, insufficient. Every risk engineer authors their contract in a slightly different shape, chooses their own thresholds, routes to their own SLAs, aggregates on their own windows. The portfolio-level aggregation the architect promised in mod-106 chapter 04 cannot execute because the shapes do not line up. The level-50 architect owns the *enterprise-scope meta-contract* — the shape that fixes the fields, the cadences, the threshold split, and the routing schema every team's instance-contract must conform to. The instance-contract belongs to the risk engineer; the meta-contract belongs to the architect. This chapter designs the meta-contract.

## What the meta-contract is, and what it is not

The monitoring-to-risk-register meta-contract is a versioned, ratified enterprise artefact that specifies the *shape* of the edge from any monitor to any risk-register field. It does not enumerate the edges themselves — that population belongs to the instance-contracts the risk engineers author, filed into the GRC-for-AI system of record per mod-111. It fixes:

- The **five fields** every monitor-to-register edge must bind (below).
- The **closed-world enumerations** those fields draw from — the list of accepted monitor platforms of origin, the list of accepted register field ids, the list of accepted cadence buckets, the threshold-type split, the owner-seat registry.
- The **invariants** the population of edges must hold, across all instance-contracts.
- The **change-control process** by which the shape itself is versioned and instance-contracts migrate.

It is *not*:

- **The monitors themselves.** Chapter 03 designs the wiring of the observability platforms; the meta-contract sits above and consumes their emissions.
- **The register schema.** Mod-106 fixed the register's field shape; the meta-contract references it.
- **The incident-classification scheme.** Mod-110 chapters 04 and 05 build the incident scheme for realised harms; monitors surface signals that *might* become incidents, and the register writes the meta-contract fixes are about residual, exposure, and freshness rather than incident logging.
- **The pre-deployment gate.** The gate (mod-107 chapter 02) reads the register the meta-contract keeps current; it does not write to it directly.

The meta-contract is the plumbing that makes the portfolio view honest. It is not the portfolio view.

## The five fields the contract binds per edge

Every monitor-to-register edge, in every instance-contract any team's risk engineer authors, must specify these five fields. The values are drawn from closed-world enumerations the architect maintains; free-text values are defects.

### Field 1 — monitor id and platform of origin

The edge names the specific monitor whose emissions feed the edge (a stable id inside the platform of origin) and the *platform of origin* itself. The platform-of-origin value is drawn from a closed-world list: the enterprise's registered model-observability platforms (chapter 03 pins these — Fiddler AI, Arthur AI, WhyLabs, Evidently AI, and equivalents in that category by capability class, without asserting any product's specific feature list), the enterprise Security Operations Center (chapter 05, for AI-specific detections routed from the SOC), and in-house telemetry emitters (typically MLOps-owned exporters over the platform team's metrics bus). New platforms of origin require a meta-contract amendment; a monitor emitting from an unregistered platform is a defect.

The purpose of the platform-of-origin binding is provenance. When the audit committee or a certification body asks "where did this residual score come from?", the edge traces back to a named monitor on a named platform whose reliability, uptime, and calibration are themselves auditable.

### Field 2 — risk-register field id it writes to

The edge names *exactly one* risk-register field id the monitor writes to. The register field id is drawn from the closed-world register schema (mod-106): residual likelihood, residual impact, residual composite score, exposure numerator, category coverage flag, control-effectiveness attestation freshness, tolerance-band position, and a small set of similar fields the register commits to. Free-text writes are prohibited; a monitor whose semantics do not map to any register field is either (a) driving an operational alarm and does not belong on the meta-contract, or (b) surfacing a signal the register does not yet know how to hold, which is a register-schema amendment request rather than a free-text write.

The edge also carries the *taxonomy reference* — the risk-category id (mod-106 invariant 1) the register field belongs to — and the *SoA control reference* — the AIMS Statement of Applicability control (mod-105 chapter 05) whose effectiveness this edge helps attest. Both are closed-world.

### Field 3 — cadence

Cadence is the interval at which the monitor's emissions produce a write to the register field. It is drawn from a small closed-world set: event-driven per emission, minute rollup, hour rollup, daily rollup, per-review-cycle rollup. The rollup is a *deliberate design choice per field* — not defaulted, not left to the platform's default push behaviour. Residual likelihood for a slow-drifting fairness metric typically rolls up daily or hourly; exposure numerator for a system whose call volume varies by two orders of magnitude across the day typically rolls up hourly; a control-effectiveness attestation freshness field typically writes per-review-cycle rather than continuously. A monitor whose cadence is defaulted to "every emission" swamps the register and produces failure mode (b) below; the meta-contract requires the risk engineer to state and justify the cadence per edge.

### Field 4 — threshold shape (operational alarm vs assurance trigger)

The threshold shape borrows directly from mod-107 chapter 03: every edge specifies, where applicable, both an *operational-alarm* threshold and an *assurance-trigger* threshold. The two are not alternatives.

- The **operational alarm** is what the *first line* (the MLOps team, the platform on-call, the model owner) responds to. It fires on routine drift, is handled inside first-line operations, and typically resolves through a first-line remediation (guardrail adjustment, retraining, canary rollback) without ever escalating.
- The **assurance trigger** is what the *second line* (the risk engineer, the ongoing-assurance function per mod-107 chapter 03) responds to. It fires when the drift, quality degradation, or exposure change is *material* to the launch decision — when the residual score against the tolerance table (mod-106) has moved enough that the pre-deployment gate's assumptions may no longer hold.

The operational-alarm threshold is typically looser (fires more often, handles routine); the assurance-trigger threshold is typically tighter on magnitude, with a longer sustain requirement (fires less often, handles material). An edge with only one threshold set is legitimate — some fields do not have a first-line remediation route (e.g., control-effectiveness attestation freshness has no first-line "fix," only a second-line "re-attest") — but the meta-contract requires the omission to be *stated* and justified, not silent.

### Field 5 — owner routing

The edge names, for each threshold, the *named seat* (drawn from the enterprise owner-seat registry — a role tag, not a person, so that seat vacancies are visible) that receives the threshold, the SLA within which that seat must acknowledge or act, and the escalation path if the SLA is missed. Typical shape:

- Operational-alarm responder: a first-line seat (e.g., `MLOps-lead-fraud`); SLA in hours-to-acknowledge; escalation-on-miss to a second-line seat.
- Assurance-trigger responder: a second-line seat (e.g., `ai-risk-engineer-fraud`); SLA in business-days-to-reassessment; escalation-on-miss to the head of AI governance.

Every threshold has an owner; every owner has an SLA; every SLA has an escalation path. Any threshold that fails to name one of these three is a defect — invariant 3 below.

## A worked contract-edge row

The following is a single instance-contract edge, authored by a fraud-team risk engineer against the enterprise meta-contract. Every field is drawn from a closed-world enumeration the architect maintains.

```yaml
edge:
  id: MRE-2027-fraud-classifier-drift-fairness
  monitor:
    id: MON-fiddler-fraud-clf-v3-drift-fairness-DIR
    platform_of_origin: fiddler-ai
  register_field:
    field_id: RR-2026-1442.residual.likelihood
    taxonomy_ref: RSK-CAT-fairness-disparate-outcome
    soa_control_ref: AIC-FAIR-004
  cadence: hourly-rollup
  threshold:
    operational_alarm:
      rule: DIR<0.85 sustain 24h
      responder: mlops-fraud-team
      sla: 4h-ack
    assurance_trigger:
      rule: DIR<0.80 sustain 24h OR DIR<0.85 sustain 7d
      responder: ai-risk-engineer-fraud
      sla: 5-business-days-reassessment
  owner_routing:
    first_line_seat: MLOps-lead-fraud
    second_line_seat: ai-risk-engineer-fraud
    escalation_on_sla_miss: head-of-ai-governance
  version_history:
    - v1.0.0
```

The row is small; the meta-contract's discipline is that every enterprise edge fits it, so the register can be queried across the population — "how many edges are past their assurance-trigger SLA?", "how many SoA controls have at least one edge writing their attestation-freshness field?", "how many taxonomy categories have no edge writing their residual?" — and the answers are truthful.

## Cardinality management — from tens of thousands of emissions to hundreds of writes

At enterprise scale the emissions cardinality is high and the register cardinality must remain low enough that named owners can actually read what is written. If tens of thousands of daily emissions produce tens of thousands of daily register writes, owners stop reading; the register becomes an alarm firehose and the meta-contract has failed on failure mode (b). The architecture pins the reduction mechanisms.

- **Aggregation windows.** The cadence choice (field 3) is itself the primary reduction — hourly rollups reduce sixty per-minute emissions to one write; daily rollups reduce twenty-four per-hour rollups to one. The meta-contract requires the risk engineer to state the aggregation function (max, mean, exceedance-count, sustained-breach-flag) per edge so that the semantics of the rolled-up value is unambiguous.
- **De-duplication rules.** Where multiple monitors surface variants of the same underlying signal (e.g., a fairness drift on subgroup A and a fairness drift on subgroup B, both feeding the same category-level residual), the meta-contract pins that only the *governing* edge writes to the register; the underlying subgroup monitors surface diagnostic detail addressable by the responder but do not each write independently.
- **Hysteresis on threshold flapping.** A monitor whose value oscillates around a threshold produces a flap — write-clear-write-clear — that swamps the register with no informational content. The meta-contract specifies that thresholds carry a *sustain* qualifier (already present in the worked example: `sustain 24h`) and a re-arm hysteresis (once cleared, must remain clear for a stated interval before re-firing). Flap suppression is a first-class field in the meta-contract, not a per-team convention.
- **Quiet-hours suppression for known-in-progress remediation.** When a CAPA (mod-105 chapter 09) or a remediation-in-progress is open on the affected system, the meta-contract permits suppression of further writes on the specific edge for the duration of the remediation window, with the suppression itself logged. Suppression without logging is a defect; suppression by default is a defect; suppression as an explicit, time-boxed, register-visible choice is legitimate and prevents the register from re-firing the finding the responder is already discharging.

The four mechanisms together are how tens of thousands of emissions per day reduce to hundreds of register writes per day, of which the vast majority are periodic re-freshes of unchanged residuals and a small minority are actionable movements.

## Composition with the taxonomy and the SoA

Two composition constraints keep the meta-contract coherent with the rest of the architecture.

- **Every edge references the taxonomy.** The taxonomy reference in field 2 is not optional and is drawn from the closed-world taxonomy (mod-106 invariant 1). The architect maintains the *inverse view*: for every taxonomy category the enterprise commits to, at least one monitor-to-register edge writes to a register field in that category. A taxonomy category with no monitor is a *coverage gap* — the residual on that category cannot be kept fresh, and the category enters the taxonomy backlog (mod-106 chapter 07) either for a monitor-authoring commitment or for explicit removal from the taxonomy.
- **Every SoA control's effectiveness attestation has at least one monitor.** The AIMS Statement of Applicability (mod-105 chapter 05) commits the enterprise to a set of controls. The meta-contract enforces that every SoA control has at least one edge writing its *attestation-freshness* field on the register — otherwise the attestation goes stale silently and mod-107's ongoing-assurance function has no signal to fire the re-attestation trigger. SoA controls without a monitor are a defect the architect and the head of AI governance address at the semi-annual SoA review.

Composition failures on either constraint are surfaced by simple population queries against the GRC-for-AI system's edge inventory (mod-111). The architect's operational review reads them monthly.

## The three invariants the contract holds

Three invariants must hold across the population of edges. Each is testable; each has an associated failure mode.

**Invariant 1 — every monitor writes somewhere or is deprecated.** A monitor emitting into the enterprise observability stack that does not appear on any meta-contract edge is an *orphan monitor* — its emissions may fire alarms, may consume responder attention, may show up at incident forensics, but the register does not know it exists. Orphans are a defect; the resolution is either to add the edge (bind the monitor to a register field) or to deprecate the monitor (retire it from the platform). Emissions with no register consequence and no first-line operational purpose are alarm-fatigue at a portfolio scale.

**Invariant 2 — every register field has a named freshness owner.** For every field on the register the enterprise commits to keeping current, at least one edge writes to it. A field with no edge is an *unattended field* — the residual on that field freezes at whatever value it last held, the tolerance-band position becomes fiction, and no one notices until an incident forces someone to read the value. Unattended fields are a defect; the resolution is either to bind a monitor or to remove the field from the register schema.

**Invariant 3 — every threshold has a routing target with a stated SLA.** For every threshold specified on an edge (operational or assurance), a named seat receives it, an SLA is stated, and an escalation path is defined. A threshold that fires into no owner, or into an owner with no SLA, is a defect; the threshold effectively does not exist because there is no one on the hook for responding to it.

## The failure modes to design against

**Failure mode (a) — monitor sprawl.** Teams stand up monitors on team-scoped platforms — a Grafana dashboard here, a bespoke evaluation-suite alerter there, a Slack integration wiring the fine-tuning provider's API webhooks — without registering the monitors under the meta-contract. Findings surface at post-incident forensics that neither the register nor the ongoing-assurance function ever knew about. The failure is architectural: the enterprise never mandated that every monitor emitting on a production AI system must resolve to a meta-contract edge. The fix is a *monitor registration gate* — an operational discipline in the GRC-for-AI platform (mod-111) that a monitor cannot be marked production-ready without a corresponding edge, and a quarterly reconciliation between the observability platforms' monitor inventories and the meta-contract's edge inventory that surfaces any drift.

**Failure mode (b) — alarm fatigue by mis-designed cadence.** The meta-contract permits raw event-driven writes to the register with no aggregation, no hysteresis, and no quiet-hours; risk engineers, under time pressure and unclear on the rollup they should choose, default to per-emission and write everything. The register becomes an alarm firehose; owners stop reading it; the mod-107 chapter 03 second failure mode (trigger-flooded) becomes structural rather than incidental. The fix is architectural: the meta-contract removes per-emission as a default and requires a stated justification when it is chosen, and the architect's monthly operational review reads the write-volume-per-edge distribution and flags edges whose write volume exceeds a defensible threshold for their field type.

**Failure mode (c) — stale-field silence.** A register field exists — a taxonomy category is committed, an SoA control is asserted — but no edge writes to it. The residual on that field freezes at its last-written value; the tolerance-band position becomes decorative; the portfolio view aggregates a value that is, in fact, unmeasured. No one notices until an incident on the affected system reveals that the "current" residual is months or years old. The fix is architectural: invariant 2 above, tested continuously by a query against the register-field inventory that surfaces any field with zero inbound edges.

## Change-control discipline

The meta-contract is itself versioned. New edge shapes (a new field added to the row schema, a new cadence bucket admitted, a new platform-of-origin registered), amendments to existing shapes, and deprecations of retired shapes are semver-shaped changes ratified by the `head-of-ai-governance` on the architect's recommendation. Instance-contracts migrate against a stated window when the meta-contract version they were authored against is superseded; the migration itself is tracked in the GRC-for-AI platform and reported in the architect's quarterly operational review.

New edges (authored by risk engineers against the current meta-contract version) are subject to the invariants at edge-creation time — an edge without a taxonomy reference (invariant 2 via composition), an edge without an owner and SLA (invariant 3), an edge writing to a register field that does not exist (register-schema violation) is rejected by the GRC platform's validation, not by human review after the fact. Machine-enforced invariants are how the meta-contract holds at scale.

Deprecated edges carry a *migration plan* for the register field they used to write. Simply removing the edge leaves the field unattended (failure mode c); the deprecation must either transfer the writing responsibility to a replacement monitor or remove the register field itself. The `ai-governance-analyst` (level 15) audits enterprise-wide conformance to these rules and files findings against edges and register fields whose bindings have drifted.

## Composition with adjacent modules

- **Mod-106** — the taxonomy the edges reference in field 2 and the appetite / tolerance table the residual reads against; the portfolio aggregation the meta-contract makes truthful.
- **Mod-107 chapter 02** — the pre-deployment gate consumes the register state the meta-contract keeps current; a gate reading a register with unattended fields cannot defensibly attest.
- **Mod-107 chapter 03** — the ongoing-assurance function consumes assurance-trigger fires from the meta-contract's threshold split; every second-line re-assessment is triggered by a meta-contract edge crossing its assurance threshold.
- **Mod-108** — evidence artefact freshness (evaluation reports, red-team artefacts, model cards) is itself a register field whose freshness the meta-contract keeps current; evidence-freshness monitors are first-class edges.
- **Mod-111** — the GRC-for-AI platform hosts the meta-contract itself, validates the invariants at edge-creation time, and surfaces the population queries the architect's operational review reads monthly.

## Summary

The monitoring-to-risk-register contract at enterprise scale is the meta-contract the level-50 architect authors — the shape every `ai-risk-engineer`'s instance-contract must fit so that portfolio-level aggregation actually works. It binds five fields per edge: monitor id and platform of origin (closed-world), register field id with taxonomy and SoA references (closed-world), cadence (closed-world, deliberate per field), threshold shape (operational-alarm vs assurance-trigger, per mod-107 chapter 03), and owner routing (named seat with SLA and escalation). Cardinality management — aggregation windows, de-duplication rules, hysteresis on flapping, quiet-hours suppression during remediation — is how tens of thousands of daily emissions reduce to hundreds of register writes without swamping owners. Three invariants hold across the population: every monitor writes or is deprecated, every register field has a named freshness owner, every threshold has a routing target with an SLA. Three failure modes — monitor sprawl, alarm fatigue by mis-designed cadence, stale-field silence — are common and each has an architectural fix. Peer roles bind: the `ai-risk-engineer` (level 25) writes the instance-contracts; the `ai-governance-analyst` (level 15) audits conformance; the `head-of-ai-governance` (level 60) ratifies the meta-contract version. The next chapter walks the wiring across the observability platforms and the SOC that produces the emissions the meta-contract binds.
