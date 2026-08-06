# exercise-03: Observability Platform Wiring Drill

**Estimated effort:** 3 hours

## Objective

Produce the **observability-platform wiring architecture** for a specified enterprise scenario — the artefact you would take to the head of AI governance and the enterprise data-platform lead to ratify *before* the chapter 02 monitoring-to-risk-register contract, the mod-108 evidence-freshness updates, and the mod-107 ongoing-assurance triggers can each be wired against a shape that will hold under regulator inspection.

The deliverable is a decision document with a two-store convergence diagram, a canonical normalised event schema, per-signal-class boundary decisions between the data-lake and GRC tiers, a replay-and-retention plan that survives a regulator arriving fourteen months after an incident, and a swap-out runbook that proves the normalisation layer earns its keep. Downstream artefacts in this module (exercise-04 incident workflow, exercise-05 SOC handoff, exercise-06 external corpora calibration) and the adjacent mod-107 and mod-108 modules bind to the shape you draw here. Get the boundary right and every subsequent consumer composes; get it wrong and every subsequent consumer inherits either three sources of truth, platform lock, or replay-blindness.

## Prerequisites

- Chapter [`03-observability-platform-wiring-and-the-single-source-of-truth.md`](../03-observability-platform-wiring-and-the-single-source-of-truth.md) read once, with the three invariants, the three failure modes, and the schema fragment marked.
- Chapter [`01-eu-ai-act-article-72-and-the-enterprise-post-market-surveillance-shape.md`](../01-eu-ai-act-article-72-and-the-enterprise-post-market-surveillance-shape.md) — the enterprise-shape context the wiring answers into.
- Chapter [`02-monitoring-to-risk-register-contract-at-enterprise-scale.md`](../02-monitoring-to-risk-register-contract-at-enterprise-scale.md) — the register-write contract whose `register_write` fields your normalisation layer addresses.
- Mod-102's control catalogue shape (any of the mod-102 chapter references) — the `control_refs` your normalised events bind to.
- Mod-108's evidence-architecture shape — the freshness index the normalisation layer pings.
- Forward reference: mod-111 GRC platform architecture — the register tier will land there; specify the interface even though the platform selection is not this module's job.
- [`../resources.md`](../resources.md) for the primary references and vendor-documentation entry points.

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose observability wiring you are least familiar with; that is where the exercise will teach you most. State your choice at the top of the deliverable.

- **A US regional bank** (Northbrook Financial-style, per mod-101 exercise-02 and mod-102 exercise-01) with ~40 AI systems: fraud classifiers, document extraction, an internal RAG legal assistant, a customer-facing generative chat, third-party AI SaaS integrations. Colorado + NYC deployments; EU expansion planned. Existing SR 11-7-aligned MRM programme with a legacy tabular-drift monitoring capability; a data-science guild that adopted an open-source drift tool two years ago; a platform team that wired a Python-native evaluator into the CI pipeline; a generative-AI centre of excellence that recently procured a commercial LLM-quality observability vendor for the customer-facing chat.
- **A global healthcare payer / provider** with clinical-decision-support pilots, patient-facing chat, coding automation, and utilisation-management AI. US federal HIPAA scope; EU AI Act relevance for European insurance subsidiaries; multiple US state deployments including Illinois and California. A medical-device division brought in a commercial model-observability vendor under FDA-facing device-history commitments; a data-science division procured a different vendor for population-health analytics under an in-region deployment constraint; a payer analytics business unit is on an in-house telemetry stack; the generative pilots are on a fourth tool the CoE chose.
- **A B2B SaaS platform vendor** shipping GenAI-augmented HR-tech capabilities into enterprise customers across the US, UK, EU, and Singapore. Customers include public-sector deployments (subject to OMB M-25-21 shape) and financial-services enterprises (subject to SR 11-7 vendor-review shape). Two acquisitions in the last three years each brought in their own model-observability contract; the core platform team chose a fifth vendor for the flagship product's LLM-quality signal; the classical-ML footprint (résumé-parsing tabular classifiers) runs on an in-house Python evaluator wired into CI.

**Justify the platform mix.** Whichever scenario you pick, name the *realistic* multi-platform footprint your enterprise ends up with and *why*: which platform arrived through which acquisition, which was per-team choice, which was chosen for a capability the others do not provide (classical-ML tabular drift vs LLM-quality signals), which sits in a specific tenant for a data-residency reason. "Just standardise on one" is not architecturally available on the timescale post-market surveillance is being stood up; the exercise is to design *for* the mix you have, not against it.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`observability-wiring-architecture.md`** — the decision document, including an ASCII / text-diagram equivalent of the two-store convergence.
2. **`normalised-event-schema-v1.0.0.yaml`** — the canonical event schema every platform emits into.
3. **`store-boundary-decision-record.md`** — the per-signal-class decision record: data-lake tier only, GRC tier only, or both.
4. **`replay-and-retention-plan.md`** — how a regulator sampling twelve months after an incident is re-derived to the point-in-time state.
5. **`platform-swap-out-runbook.md`** — the mid-year deprecation walkthrough that proves invariant 1.

## Requirements

### `observability-wiring-architecture.md`

Decide and justify **each** of the following:

- **Platform mix.** Name the platforms your enterprise realistically operates. Use at least three of {Fiddler AI, Arthur AI, WhyLabs, Evidently AI, in-house telemetry}, and state for each: how it arrived (acquisition, per-team choice, capability fit, sector constraint), which business unit or tenant it currently serves, the signal classes it emits (drift, performance, fairness, adversarial-robustness, LLM-quality, data-quality). Any claim about a specific product feature — supported metric type, native retention window, alerting shape, deployment mode — carries `<!-- needs-research: verify against <vendor> documentation as of <date> -->` unless you have cited the vendor document directly.
- **Two-store convergence diagram.** An ASCII / text diagram showing the producers (platforms), the normalisation layer, and the two authoritative stores (data lake, GRC-for-AI register). Name the normalisation-layer boundary explicitly — what enters it (platform-native emissions) and what leaves it (canonical events per the schema you author in deliverable 2). Show the dual-write. Do not draw the platform UIs as sources; draw them as views only.
- **Normalisation-layer specification.** Name the ownership seat (typically enterprise data-platform team in coordination with the AI governance office — resolve which for your scenario), the SLA (delivery latency to both stores), the change-control process (how a schema evolution is ratified and communicated to downstream consumers), and the test suite (golden platform emissions with expected normalised outputs run on every schema change). State explicitly that the layer normalises **shape**, not **semantics** — it does not recompute a fairness metric or transform a value; it maps fields, joins system-ids, attaches taxonomy and control refs, and routes.
- **Three-invariant enforcement.** For each of the three chapter 03 invariants (single normalisation schema; two-store convergence; every signal catalogued or explicitly deprecated), name the specific design choice in your architecture that enforces it and the test that would detect its violation.
- **Three-failure-mode defence.** For each of the three chapter 03 failure modes (three-sources-of-truth; platform-lock; replay-blindness), name at least one architectural move you have made to prevent it in your scenario.
- **Downstream consumers.** Enumerate the named downstream consumers of the register-tier writes: the chapter 02 monitoring-to-risk-register contract; the mod-108 evidence-freshness index; the mod-107 chapter 02 pre-deployment gate re-affirmation trigger; the mod-107 chapter 03 ongoing-assurance re-assessment trigger; the chapter 04 incident-enrichment path. For each, name the specific normalised-event field(s) it reads and the cadence.
- **Non-scope.** At least three things you *chose not to* include and why. Candidates: turning the normalisation layer into a metric-recomputation service; sourcing the CRO's portfolio dashboard from a platform UI; adding a third store of authority to hold cross-platform aggregates; letting each platform write its own retention policy.

### `normalised-event-schema-v1.0.0.yaml`

Author the canonical schema every platform's adapter emits into. Mirror the chapter 03 schematic shape — `source`, `system`, `signal`, `taxonomy`, `routing`, `retention`, `captured_at` — and extend it as your scenario requires. Version the file (`1.0.0`). Include at least:

- A `source` block that isolates the platform-specific (platform id, platform stream id, tenant / workspace ref).
- A `system` block joining to the AI-inventory shape (system id, version, tier).
- A `signal` block with a canonical enum of signal kinds covering at minimum: `drift`, `performance`, `fairness`, `adversarial-robustness`, `llm-quality`, `data-quality`, `security-signal` (the last per chapter 05).
- A `taxonomy` block binding the event to mod-106 risk categories and mod-102 control refs.
- A `routing` block naming the register-write address and the chapter 02 contract edge.
- A `retention` block naming the hot-tier window and the cold-migration point per signal class and system tier.
- A `captured_at` timestamp and any `observed_at` / `emitted_at` distinctions your scenario needs for replay.

Where you specify a canonical metric name that a platform natively expresses differently (e.g. `disparate_impact_ratio` when one platform emits `dpr_24h`), mark the adapter-mapping assumption `<!-- needs-research: verify <platform> emits this metric under this exact name; consult vendor documentation -->`. Do NOT invent metric shapes to make the schema look complete.

### `store-boundary-decision-record.md`

For each of the following signal classes, decide whether the signal lives in the data-lake tier only, the GRC tier only, or both (register-tier aggregate + lake-tier raw), and defend the choice against the three invariants:

- **Drift** (feature-distribution drift, concept drift).
- **Performance** (accuracy, calibration, latency, error rates).
- **Fairness** (group-fairness metrics, disparate-impact signals).
- **Adversarial-robustness** (robustness-eval outcomes, red-team signals).
- **LLM-quality** (hallucination detection, retrieval-quality, tool-call correctness, refusal-rate signals).
- **SOC-sourced signals** (jailbreak detections, prompt-injection detections, credential-exfiltration attempts — the chapter 05 handoff category).

For each class, state:

- Which tier(s) hold it and why.
- The register-tier aggregation rule (if the class writes to the register): what raw signal becomes what register field, at what cadence, computed by whom (the normalisation layer's routing does not compute — say who does).
- The lake-tier retention (hot window, cold-migration point) with an explicit link to the regime obligation driving the window (see the retention plan).
- The consumer(s) that read from each tier for this class.

### `replay-and-retention-plan.md`

Assume the regulator arrives fourteen months after an EU AI Act Article 73 serious-incident report and asks for the point-in-time state of the monitoring signals around the incident window. Author the plan that satisfies the ask. It must cover:

- **Hot / cold tier boundaries per signal class and system tier.** State the operational access curve that drives the hot window (incident triage reach-back, drift investigation window, evaluation reproduction on recent versions) and the regulatory-retention obligation window that drives the cold window.
- **Retention windows tied to regime obligations at title level.** At minimum: EU AI Act document- and logging-retention obligations for high-risk systems; sector obligations for your scenario (FDA device-history for the healthcare medical-device division; SR 11-7 model-history expectations for the bank's MRM-covered classical models; contractual retention obligations in the B2B SaaS enterprise-customer DPAs). Every specific window carries `<!-- needs-research: verify against <regime> Article/section as of <date> -->` unless you have cited the primary source directly.
- **Replay procedure.** The step-by-step: how the sample request is received, how the lake is queried, how the normalisation-layer's `captured_at` and version metadata are used to reconstruct point-in-time state, how the register's aggregated state at that time is separately retrievable (register versioning or event-sourcing?), how the result is packaged for the regulator.
- **Freshness pings.** How the retention plan interacts with the mod-108 evidence-freshness index — expired artefacts in cold tier are still retrievable; expired *evidence* is not the same as expired *telemetry*.
- **Cost-vs-obligation trade-off.** A named place in the plan where the cold-tier retention window is *not* shortened for cost reasons — the obligation floor is the obligation floor.

### `platform-swap-out-runbook.md`

Walk the swap-out path for a chosen platform in your mix: the enterprise decides mid-year to deprecate one of the observability platforms (name which, and why — cost, capability gap, vendor viability, consolidation, sector-constraint change). The runbook proves invariant 1 (single normalisation schema): if your swap-out costs less than a full re-integration, the normalisation layer is doing its job.

Include:

- The **what-changes / what-does-not** ledger. Rows: normalisation adapter for deprecated platform (changes); normalisation adapter for replacement platform (changes / new); the canonical schema (does not change); the register schema (does not change); downstream consumers named above (do not change); historical events in the lake (preserved with old `source` values); portfolio queries across the boundary (continue to work).
- The **cutover sequence**: dual-run window, canary period, validation against golden emissions, decommission of the deprecated adapter, retention of the deprecated platform's raw event history in the lake for the regime window.
- The **regression tests**: portfolio-query results before and after cutover for a chosen set of tier-3 systems must be equivalent up to the deprecated platform's residual events; the ch 02 contract's register-write behaviour must be unchanged; the mod-108 freshness pings must not lose a beat.
- The **cost envelope**: a rough estimate (order of magnitude, quarter of engineering effort) that argues the swap is bounded. Compare against the point-to-point anti-pattern's multi-quarter register-schema-touching migration.
- The **rollback plan**: what happens if the replacement platform's adapter fails validation in the canary period.

## Starter guidance

- Draw the architecture diagram BEFORE authoring any schema. If you cannot draw the two-store convergence cleanly on one page, you have not decided the boundary and the schema will paper over the confusion.
- Do NOT let the normalisation layer become a translation service that also transforms values. It normalises **shape** — field names, system-id joins, taxonomy binding, routing addresses. It does not recompute a fairness metric, re-window a drift signal, or re-threshold a data-quality check. Semantic transformation belongs upstream in the platform or downstream in the consumer; the normalisation layer is the seam, not the compute.
- The replay plan is where certification-body inspection dies quietly if you do not design for it. Assume the regulator arrives fourteen months after the incident, the platform that emitted the signals has been swapped out in the interim, and the operator who last saw the console has left. The lake and the versioned schema are what remain.
- Do not invent vendor-specific product features to make the schema plausible. Write against the generic category (fairness metric, drift metric, LLM-quality signal) and mark any specific claim about a named vendor's feature `<!-- needs-research: ... -->`. The point of the architecture is that it holds regardless of which specific vendor sits in which tenant.
- The platform-swap-out runbook is the invariant-1 proof. If drafting the swap costs less than a full re-integration, your normalisation layer works. If drafting it looks like a multi-quarter register-schema project, revisit the schema.
- Distinguish register versioning from lake retention. The register holds aggregated state that is queryable at scale; reconstructing register state at a past point requires either register-side event sourcing or a separate append-only history. The lake holds raw and is where point-in-time signal state is reconstructed from. Decide both and say so.
- For the healthcare and bank scenarios especially, resolve the interface between the classical-MRM / medical-device observability heritage and the newer AI-specific observability layer. Do not double-write; do not leave signals uncatalogued.

## Acceptance criteria

- [ ] Scenario is stated at the top of `observability-wiring-architecture.md`; platform mix is enumerated with acquisition / per-team-choice / capability-fit provenance for each.
- [ ] Platform mix uses at least three of {Fiddler AI, Arthur AI, WhyLabs, Evidently AI, in-house telemetry}.
- [ ] The two-store convergence diagram is present and legible; platform UIs are drawn as views, not sources.
- [ ] The normalisation-layer specification names ownership, SLA, change-control, and test suite, and states explicitly that the layer normalises shape, not semantics.
- [ ] Each of the three chapter 03 invariants has a named design-choice-plus-detection-test.
- [ ] Each of the three chapter 03 failure modes has at least one architectural defence.
- [ ] Downstream consumers named explicitly, with the field(s) each reads and the cadence: ch 02 contract writes; mod-108 evidence freshness; mod-107 ch 02 pre-deployment gate re-affirmation; mod-107 ch 03 ongoing assurance triggers; ch 04 incident enrichment.
- [ ] `normalised-event-schema-v1.0.0.yaml` is versioned, mirrors the chapter 03 schematic shape, and covers at minimum the seven signal kinds enumerated in the requirements.
- [ ] `store-boundary-decision-record.md` decides all six signal classes (drift, performance, fairness, adversarial-robustness, LLM-quality, SOC-sourced) with tier, aggregation rule, retention, and consumer named per class.
- [ ] `replay-and-retention-plan.md` cites regime obligations at title level with `<!-- needs-research: ... -->` markers on every specific window not yet verified from primary source; includes the fourteen-month replay procedure end-to-end; names an obligation floor that is not shortened for cost.
- [ ] `platform-swap-out-runbook.md` includes the what-changes / what-does-not ledger, the cutover sequence, regression tests, cost envelope, and rollback plan.
- [ ] Every specific claim about Fiddler / Arthur / WhyLabs / Evidently product features (supported metrics, native retention, alerting shape, deployment mode) is either cited to vendor documentation or marked `<!-- needs-research: ... -->`. No invented feature claims.
- [ ] The `needs-research` discipline is applied throughout — no invented dates, article numbers, retention windows, or vendor capability claims. Every unverified specific carries the marker.

## Stretch goals

- Extend `normalised-event-schema-v1.0.0.yaml` and the wiring diagram to a **multi-tenant lake schema** that separates per-business-unit tenants under one enterprise (per the healthcare divisions, the bank's US-vs-EU expansion, the B2B SaaS's per-customer isolation). Show how the `source.tenant` field and lake partitioning compose without letting a business-unit query see another unit's raw telemetry.
- Sketch a **cost-model** for the two-tier lake retention: order-of-magnitude cost per byte per year at hot vs cold, multiplied by expected signal volume per system tier per platform, integrated over the regulator-obligation window. Name the point where the obligation floor and the storage-cost curve intersect and show that the plan holds above the floor.
- Author the **SIEM interface** with the SOC per chapter 05 as an appendix: which security signals traverse the normalisation layer into the enterprise stores, which are routed directly to the SIEM, which are dual-routed, and how the canonical `security-signal` event kind composes with the SOC's SIEM schema without producing a fourth store of authority.
