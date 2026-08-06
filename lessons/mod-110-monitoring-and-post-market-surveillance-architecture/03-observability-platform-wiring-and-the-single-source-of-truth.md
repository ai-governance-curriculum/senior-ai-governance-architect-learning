# Wiring the observability platforms — Fiddler, Arthur, WhyLabs, Evidently — without producing three sources of truth

## Why this chapter exists

An enterprise that has stood up model observability at scale — drift monitors on its classical-ML footprint, LLM-quality monitors on its generative footprint, fairness and data-quality signals on its higher-tier systems — almost always ends up with more than one observability platform. Fiddler AI is under contract to one business unit from an earlier procurement; Arthur AI came in with an acquisition; WhyLabs was chosen by the data-science guild for open-source lineage; Evidently is the default the platform team wired into the CI pipeline because it composes with their Python stack. Each has its own console, its own alerting shape, its own retention defaults, its own idea of what a "system" is and what a "signal" is. Each is emitting into the enterprise, and each is a plausible answer when the head of AI governance asks "how many tier-3 systems currently show above-appetite drift on the fairness axis?"

The failure mode is predictable and it is the one this chapter exists to prevent. The chief risk officer reads Fiddler's dashboard, the CFO's AI-materiality reader reads WhyLabs' monthly export, the head of AI governance reads the GRC-for-AI register that a mix of platforms wrote into over the last six months in inconsistent shapes, and each of the three gets a different number. All three are speaking the truth their view can see. None of the three is speaking the enterprise's truth, because no such truth has been architected — three views became three sources.

The level-50 architect owns the wiring that prevents this. The chapter names the two stores of authority the architecture converges on, the normalisation layer that is the price of the convergence, the point-to-point anti-pattern that swallows enterprises that do not build the normalisation layer, and the three invariants and three failure modes the design turns on. Naming the four platforms above is orientation to the category of tooling — model-observability platforms typically emitting drift, performance, data-quality, fairness, and increasingly LLM-quality signals — and is not an endorsement of any one; the same architecture applies to Datadog Model Monitoring, Weights & Biases, Aporia, Superwise, or any successor. Specific vendor-feature claims in the chapter carry a `needs-research` marker; the architecture is what generalises.

## Why the enterprise ends up with more than one platform

"Just standardise on one" is the first thing an executive says to this problem and the first thing that is not architecturally available on the timescale the enterprise needs a working post-market surveillance system. Several structural forces push toward multi-platform:

- **Acquisitions bring tenants.** The acquired business had its own observability vendor under a multi-year contract with data-residency commitments; ripping it out on day one is neither commercially nor operationally realistic.
- **Per-team choice was the previous norm.** Before there was a level-50 architect owning the shape, individual data-science and platform teams chose the tool that fit their stack. Undoing those choices requires migration work no team has bandwidth for.
- **Capability differences are real.** The tooling category has bifurcated: platforms strong at classical-ML tabular drift are not always the platforms strong at LLM-quality signals (hallucination detection, retrieval-quality, tool-call correctness), and vice versa. An enterprise with both classical-ML and generative footprints may legitimately need coverage from more than one vendor. `<!-- needs-research: verify which named vendors currently market first-class LLM-quality observability vs classical-ML observability, and any explicit segmentation in their product literature -->`
- **Sector deployment constraints intervene.** A regulated business unit in a jurisdiction with data-sovereignty rules may require an in-region deployment the enterprise-preferred vendor does not offer.
- **Existing tenants and contracts have inertia.** Even where a single vendor is technically preferable, the switching cost — re-instrumentation, re-alerting, retention migration, re-training of operators — is often multi-quarter work.

The architect designs *for* multi-platform rather than against it. The question is not "which vendor wins" but "what shape holds when we have three or four of them emitting concurrently for the next several years?"

## The two stores of authority

The architecture converges on exactly two enterprise stores. Neither is any observability platform.

**Store (a) — the GRC-for-AI system of record.** This is the risk register, the incident store, the AIMS documented information, the evidence artefact index — the store the monitoring-to-risk-register contract (chapter 02 of this module) writes into, the store the ISO/IEC 42001 certification body inspects, the store the third-line internal audit samples, the store the sector regulator's examiner asks to see. Its schema is stable, its retention is set by regime obligation and enterprise policy, its writes are auditable, and its state at any point in time is defensible under external audit. Mod-111 pins the platform layer for this — commercial GRC or purpose-built AI-GRC, either shape must satisfy the same properties.

**Store (b) — the enterprise data lake.** This is where raw telemetry is retained in its native shape — the point-in-time signal streams the observability platforms emitted, before aggregation, before roll-up, before enterprise categorisation was overlaid. Raw retention is what makes replay possible: the certification body asks for the fairness metric state on the day of the serious incident, the third-line audit asks for the drift trajectory in the week before the model was retrained, the external independent auditor asks for a sample of raw evaluation runs to reproduce. Only the lake can answer. Retention windows are set per regime obligation — the EU AI Act imposes document- and logging-retention windows for high-risk systems that the lake must satisfy; FDA guidance for AI/ML-enabled medical devices imposes device-history retention; sector regulators impose their own. `<!-- needs-research: verify the specific retention window Article 12 / Article 19 (logging) of the EU AI Act imposes on high-risk system providers and deployers, and the analogous FDA device-history retention window for AI/ML-enabled devices -->`

**Neither store is the observability platform itself.** This is the invariant the whole chapter turns on and the sentence to hold onto through the rest of the design. The observability platform is a *producer* — it emits signals into the enterprise. The two stores are *authorities* — they hold state the enterprise commits to as truth. The platform's own UI, its alerting console, its retention layer, its dashboards — all of them are *views*. A view can be useful; it can be operationally indispensable to the model owner debugging a drift alarm at 2am. It is not authority. When the CRO's number and the platform UI's number disagree, the CRO's number wins because it references the register; the platform UI has drifted or is scoped differently.

## The wiring pattern

The shape the wiring takes:

```
observability platform → normalised event schema → dual write:
   (data lake for raw retention) + (GRC for register-writes per ch 02 contract)
```

The **normalisation layer** in the middle is the enterprise-owned component that maps every platform's native emission into one canonical event shape before it hits either store. It is where enterprise categories from the mod-106 taxonomy are attached (an incoming fairness signal becomes an event tagged with the enterprise's `RSK-CAT-fairness-disparate-outcome` category, not the platform's native metric name); where system-id joins are performed against the AI inventory (an incoming platform-stream reference becomes an event carrying the enterprise `system_id` and version); where owner tags are added (the first-line model owner, the second-line assurance reviewer, the incident triage queue); where taxonomy classifications and control references are overlaid (the signal is bound to the mod-102 controls whose attestations it substantiates).

The normalisation layer is a **first-class enterprise component**. It is not a script glued together by whoever integrated the first vendor. It has a versioned schema (see the fragment later in this chapter), a change-control process (schema evolutions ratified by the architect, communicated to downstream consumers), an ownership seat (typically the enterprise data-platform team in coordination with the AI governance office), an SLA (delivery latency to both stores under a stated bound, so that alerting-critical signals do not sit in a backlog), and a test suite (a set of golden platform emissions with expected normalised outputs, run on every schema change). Treating it as anything less than a first-class component is what produces the anti-pattern the next section names.

## The point-to-point anti-pattern

The alternative to a normalisation layer is that each observability platform writes directly into the register in its own shape. Three consequences follow, each of them corrosive:

1. **The register schema grows to be the union of every platform's shape.** Fiddler's fairness event has one set of fields; Arthur's has another; WhyLabs' has a third; Evidently's has a fourth. Each is added to the register schema as a separate optional-fields group. Six months in, the register has a hundred fields, most of them null for any given entry, and no analyst can write a portfolio query without knowing which platform each row came from.
2. **Swapping a platform requires a register migration.** The enterprise decides to consolidate away from vendor X. But vendor X's fields have been written into the register for two years, and the register's schema, downstream reports, board metrics, and regulator packaging all reference those fields. The swap becomes a multi-quarter project. The vendor knows this; pricing negotiations reflect it. This is *platform lock*, and it is not accidental — it is what happens when the enterprise fails to invest in the normalisation seam.
3. **Portfolio queries across platforms fail because fields are not comparable.** Fiddler's fairness metric is `demographic_parity_ratio` on a 24-hour rolling window; WhyLabs' is `disparate_impact` on a 7-day tumbling window; Arthur's is a proprietary fairness index; Evidently's is a threshold-crossing count. The head of AI governance asks "which of our tier-3 systems currently show a fairness signal above appetite?" The register cannot answer, because the four numbers are not the same number and the register never architected a canonical fairness expression the four platforms map into.

Point-to-point integration is architecturally cheap on day one and structurally expensive for the life of the programme. The normalisation layer is the opposite — expensive on day one, cheap for the life of the programme.

## Retention and reproducibility — the two-tier lake

The certification body, the third-line internal audit, the external independent auditor, and the sector regulator's examiner all share one demand the register alone cannot satisfy: **replay to a point in time.** When a serious incident is reported under EU AI Act Article 73 obligations, the regulator will ask for the point-in-time state of the monitoring signals around the incident window. When the third-line audit samples a system, it will ask for the raw drift trajectory over the sampled quarter, not the register's aggregated status. When the external auditor reproduces an evaluation, it will need the raw evaluation-run artefacts the observability platform captured at the time.

The register does not hold raw telemetry; it holds aggregated / rolled-up state suitable for querying at scale. The lake is what holds raw. Within the lake, a two-tier retention scheme is the shape the architecture typically converges on:

- **Hot tier — weeks to months.** Recent windows kept for operational access patterns: incident triage that reaches back over the last several weeks, drift investigations that need daily-granularity signal for a quarter, evaluation reproduction on recently-deployed versions. Latency is low; storage cost is higher per byte; retention window is set by the operational access curve.
- **Cold tier — years.** The regulatory-retention obligation window, kept at low cost with higher retrieval latency. Sampling from cold tier is a batch operation, not an interactive one. Migration from hot to cold is scheduled per-event by the normalisation layer's retention policy (see the `retention` block in the schema fragment below).

The hot / cold cutover is a design decision the architect makes per system tier and per regime obligation. A tier-3 high-risk system under EU AI Act obligations may hold a longer hot window than a tier-1 low-risk classifier; a system under FDA device-history obligations may hold a longer cold window than one that is not. The point is that the two-tier shape is an architectural choice, not a storage-cost afterthought.

## Downstream consumers of the register writes

The register-tier writes the normalisation layer produces feed a set of downstream consumers that this module and adjacent modules have already defined. The wiring is not complete until each of them is named as a first-class consumer, with the specific field(s) it reads and the cadence it reads them.

- **Incident record enrichment.** An incident record in the GRC store references the raw signals in the lake by pointer; the register-tier fields on the incident are populated by the normalisation layer's writes. When the incident-response team opens the record, the enriched fields are already there. Ad-hoc post-incident data collection from the observability platform UIs is the failure mode the enrichment path prevents.
- **Risk-register score writes.** The chapter 02 monitoring-to-risk-register contract specifies which monitor signals feed which risk-register fields — likelihood, exposure, control-effectiveness proxies. The normalisation layer's `routing.register_write` field is the address the contract writes to.
- **Evidence-contract artefact freshness updates.** Mod-108 established that evidence artefacts have a freshness contract — a fairness attestation stale beyond its refresh window is no longer valid evidence. The normalisation layer emits freshness pings against the evidence index; the mod-108 index marks artefacts fresh, stale, or expired.
- **Pre-deployment gate re-affirmation input.** Mod-107 chapter 02's pre-deployment gate re-affirms — periodically, on drift, on incident — the readiness decisions it made at launch. The normalisation layer's writes are the input to the re-affirmation trigger.
- **Ongoing-assurance re-assessment trigger.** Mod-107 chapter 03's ongoing assurance programme fires re-assessments on threshold-crossing events; the normalisation layer is where the threshold-crossing is detected in a canonical shape and the trigger is emitted to the second-line queue.

Each of these consumers reads the register (or the lake, via the register's pointer) — not the observability platform directly. This is what makes the platform swappable and the register authoritative.

## The three invariants

**Invariant 1 — single normalisation schema across all platforms.** Every platform's emission is mapped into one canonical event before it hits either store. No platform's native shape enters the register or the lake unmodified. The schema is versioned; adapters are per-platform; schema evolution is change-controlled by the architect. This is the invariant that makes platform swap-out tractable and cross-platform portfolio queries meaningful.

**Invariant 2 — two-store convergence, never three.** The register and the lake are the only stores of authority. The platform's own UI is a view — useful, operationally consulted, but not authoritative. A dashboard the CRO reads that is not sourced from the register is a governance defect. A retention layer other than the lake that the third-line audit is asked to sample from is an architectural defect.

**Invariant 3 — every platform's signal is either in-scope for the ch 02 contract or explicitly deprecated.** No platform emits into the enterprise and is un-catalogued. When a platform starts emitting a new signal type (a new LLM-quality metric, a new fairness measure), either the ch 02 contract is extended to cover it and the normalisation schema is evolved to carry it, or the signal is explicitly out-of-scope and the platform is configured not to emit it into the enterprise stores. Silent signal accretion — signals flowing into the lake that no consumer reads, no contract addresses, and no owner knows exist — is the invariant this closes off.

## The three failure modes

**Failure mode (a) — three sources of truth.** The register says one thing, the lake's ad-hoc query says another, each platform's UI says a third, and the CRO, the CFO's AI-materiality reader, and the head of AI governance disagree on how many tier-3 systems currently sit in above-appetite residual on the fairness axis. This is the failure mode the two-store convergence and the normalisation layer exist to prevent. It typically appears when the normalisation layer was treated as a script rather than a component, or when platform UIs continued to be cited as sources after the register was stood up.

**Failure mode (b) — platform lock.** Ad-hoc integrations have proliferated. The register schema references vendor-native field names; downstream reports are hard-coded to vendor concepts; the third-line audit's sampling procedures assume the vendor's console. The enterprise cannot swap the vendor without a multi-quarter migration, and the vendor's pricing leverage grows with that knowledge. The invariant-1 normalisation schema is what prevents this; the failure mode is what happens when invariant 1 is skipped for expediency.

**Failure mode (c) — replay-blindness.** Raw telemetry was aggregated on the way in — either at the platform, or at a lightweight ingestion layer that only kept rolled-up state. When the regulator asks for the point-in-time signal state at the moment of a serious incident, the enterprise cannot reconstruct it. The register's aggregated state is defensible for portfolio queries but insufficient for regulator sampling; the lake's raw retention was skipped or its cold-tier window was set too short. This is the invariant-2 payoff — the lake tier is not optional infrastructure, and its retention windows are set by regime obligation and not by storage cost.

## The normalised event — a schema fragment

The following is illustrative of the shape the normalisation layer emits. Enterprises adapt field names and enumerations; the shape is what generalises.

```yaml
normalised_event:
  event_id: NEV-2027-08-14T09:22:03Z-fiddler-fraud-clf-v3-fairness
  source:
    platform: fiddler-ai
    platform_stream_id: <platform-native stream ref>
    tenant: <tenant/workspace ref>
  system:
    system_id: SYS-2027-0042
    version: v3.4.1
    tier: tier-3
  signal:
    kind: fairness-metric
    metric: disparate_impact_ratio
    value: 0.82
    window: 24h-rolling
  taxonomy:
    category_ref: RSK-CAT-fairness-disparate-outcome
    control_refs: [ AIC-FAIR-004 ]
  routing:
    register_write: RR-2026-1442.residual.likelihood
    contract_edge: MRE-2027-fraud-classifier-drift-fairness
  retention:
    lake_tier: hot
    cold_migration_at: 90d
  captured_at: <timestamp>
```

The fields carry the invariants. `source` isolates the platform-specific — everything downstream is platform-agnostic. `system` joins against the AI inventory. `signal` carries the canonical metric name and value the enterprise has committed to, not the platform's native metric. `taxonomy` binds the event to the mod-106 risk category and the mod-102 control(s) whose attestation the signal substantiates. `routing` addresses the register field the chapter 02 contract writes to and the monitor-registry edge the signal traverses. `retention` carries the two-tier hot/cold policy. `captured_at` fixes the reproducibility anchor for point-in-time replay.

## The platform swap-out path

The payoff of invariant 1 is what happens when the enterprise deprecates a platform. Because normalisation isolates the platform-specific (the `source` block) from the enterprise-canonical (everything else), the swap requires re-wiring only the platform-to-normalisation adapter. The register schema does not change. Downstream consumers — the ch 02 contract, the incident enrichment, the pre-deployment gate re-affirmation, the ongoing-assurance trigger, the evidence-freshness updates — do not change. Historical events in the lake are preserved as they were emitted (with the old platform's `source` values); new events flow from the new platform's adapter with the same canonical shape downstream. Portfolio queries continue to work across the boundary because the canonical signal names, taxonomy references, and system joins are unchanged.

The migration is bounded, tractable, and completable within a single quarter for a single platform. Compare against the point-to-point anti-pattern's multi-quarter, register-schema-touching migration, and the case for the normalisation layer's day-one investment writes itself.

## Coordination — peer and adjacent roles

The wiring is not the architect's to build alone. Two adjacent role interfaces matter:

- **`ai-infra-security` (level 35)** co-owns any security-signal routing — jailbreak detections, prompt-injection detections, credential-exfiltration attempts observed through the observability layer. Chapter 05 of this module walks the SOC interface in detail; the point here is that security signals traverse the same normalisation layer as governance signals, and the architect ensures the security team's canonical events are first-class in the schema.
- **`ai-evaluation-engineer` (peer, level 35)** uses the lake tier for evaluation-run reproduction. The lake's raw retention is what makes second-line re-derivation of first-line evaluations possible; the evaluation function's requirements on retention window, granularity, and metadata are inputs to the lake's design.

The wiring also crosses several adjacent module boundaries: mod-102 controls whose attestations the freshness monitors implement; mod-105 chapter 07's documented-information handling that the AIMS layer of the register inherits; mod-108's evidence architecture that the freshness updates feed; mod-111's GRC platform layer that will host the register tier.

## Summary

The enterprise ends up with multiple observability platforms — by acquisition, by per-team choice, by capability difference between classical-ML and LLM observability, by sector deployment constraint — and the level-50 architect designs for multi-platform rather than against it. The architecture converges on exactly two stores of authority: the GRC-for-AI system of record and the enterprise data lake. Neither is any observability platform; each platform is a producer, not a source. A first-class normalisation layer maps every platform's native emission into one canonical event before it hits either store. Point-to-point integration is the anti-pattern — it grows the register schema to the union of every platform's shape, produces platform lock, and defeats cross-platform portfolio queries. The lake carries two-tier retention (hot for operational replay, cold for regulator-retention obligations) sized against regime windows. The register-tier writes feed a defined set of downstream consumers: incident enrichment, ch 02 risk-register scoring, mod-108 evidence freshness, mod-107 pre-deployment gate re-affirmation, and mod-107 ongoing-assurance re-assessment. Three invariants hold — single normalisation schema, two-store convergence, every signal catalogued — and three failure modes are what happens when they do not: three sources of truth, platform lock, and replay-blindness. Chapter 04 walks the incident aggregation and near-miss handling that the two-store shape enables; chapter 05 walks the SOC handoff for security-shaped signals; chapter 06 walks the Article 73 serious-incident reporting workflow the register's single-source-of-truth property makes tractable.
