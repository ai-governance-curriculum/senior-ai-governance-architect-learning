# AI observability adjacencies — Fiddler, Arthur, WhyLabs, Evidently as signal sources

## Why this chapter exists

This chapter is the twin of chapter 05. Chapter 05 fixed the interface between the GRC-for-AI system of record and the runtime-security adjacencies — Robust Intelligence, Lakera Guard, CalypsoAI, HiddenLayer, Protect AI — and the coordination edges that keep those platforms sibling producers into the register rather than competing sources. The present chapter does the same shape of work for the *other* class of adjacency the mod-111 architect has to bind against: the AI-observability tooling category — Fiddler AI, Arthur AI, WhyLabs / whylogs, Evidently AI, and the wider set of successor and adjacent products — whose signal class is drift, model performance, fairness, data-quality, and LLM-quality rather than jailbreak-and-attack-pattern.

The wiring itself is not this chapter's job. Mod-110 chapter 03 has already designed the two-store convergence, the normalisation layer, the point-to-point anti-pattern, the two-tier retention scheme in the lake, and the register-tier consumer set that the register writes feed. That chapter is the authority on how the observability platforms are wired *into* the enterprise. What mod-110 chapter 03 does not, and cannot, specify is the *consumption contract at the GRC-for-AI end* — the shape the platform mod-111 is designing commits to as a consumer, the binding it holds against the mod-108 evidence contract and the mod-102 control library, and the invariant that the platform's own dashboard is a reader of the register rather than a competing view. That is this chapter's scope, and it is intentionally shorter than chapter 05 because it reads chapter 05 as the sibling and points at mod-110 chapter 03 for the wiring rather than re-deriving it.

The chapter that follows names the vendor category at orientation level, fixes the consumption contract the GRC-for-AI platform offers, walks the level-25 / level-35 / level-50 coordination edges, names the three failure modes the contract exists to design against, gives a YAML-shaped fragment of the contract, and closes on the invariants.

## The observability tool category at a glance

The category is the set of platforms whose primary emission is model-behaviour signal against a deployed system — drift against a reference distribution, performance against a labelled or proxy ground truth, fairness against a protected-attribute partitioning, data-quality against schema and validity rules, and increasingly LLM-quality signals such as hallucination-rate, retrieval-quality, and tool-call correctness. The category has bifurcated somewhat between platforms strong at classical-ML tabular and vision drift and platforms strong at LLM-quality; enterprises typically end up with more than one, per the multi-platform argument mod-110 chapter 03 made. `<!-- needs-research: verify which named vendors currently market first-class LLM-quality observability vs classical-ML observability, and any explicit segmentation in their product literature -->`

The four named at orientation:

- **Fiddler AI** — commercial model-observability platform; markets model-performance, drift, explainability, and LLM-quality features. `<!-- needs-research: verify current Fiddler product surface area for LLM-quality features, list of first-class metrics emitted, and any managed export/streaming shape -->`
- **Arthur AI** — commercial model-observability platform; markets model-performance monitoring and (per current product literature) LLM-focused evaluation and monitoring. `<!-- needs-research: verify current Arthur product surface area, including LLM-side offering and split between hosted and self-hosted deployment shapes -->`
- **WhyLabs / whylogs** — commercial observability platform (WhyLabs) built on an open-source data-logging library (whylogs); markets classical-ML and LLM observability with a data-profiling lineage. `<!-- needs-research: verify current WhyLabs commercial product surface, whylogs licence, and any platform-native LLM-quality metric list -->`
- **Evidently AI** — open-source Python library for model and data monitoring with a commercial platform tier; strongly composable with Python-native pipelines and CI. `<!-- needs-research: verify current Evidently open-source vs commercial-tier split, licence, and metric-family coverage -->`

The list is orientation, not endorsement, and it is not exhaustive; Datadog Model Monitoring, Weights & Biases, Aporia, Superwise, and successors sit in the same category and are governed by the same architecture. The mod-111 architect specifies the shape the platform holds; specific vendor selection is a mod-111 vendor-evaluation exercise elsewhere in the module.

## The consumption contract the GRC-for-AI platform offers

The GRC-for-AI platform mod-111 is designing enters the observability picture as a **consumer**, not a producer, and not a normaliser. Mod-110 chapter 03 established a normalised event stream — one canonical event shape all observability platform emissions are mapped into by the enterprise-owned normalisation layer before they hit either store of authority. The mod-111 platform binds to that stream downstream of the normalisation layer. Three properties follow, and together they define the contract.

**Property 1 — the platform consumes normalised events; it does not re-normalise, and it does not hold raw telemetry.** The register-tier writes described in mod-110 chapter 03 land in the GRC-for-AI platform in the shape the normalisation layer has already committed to. The platform accepts the `signal.kind`, `signal.metric`, `taxonomy.category_ref`, `taxonomy.control_refs`, `routing.register_write`, and the rest of the canonical schema; it does not receive Fiddler's raw fairness event, Arthur's raw performance event, WhyLabs' raw profile summary, or Evidently's raw report object. Raw telemetry is the lake tier's responsibility and stays there; when the platform needs to reference the raw evidence behind a register-tier update — for a Board packet, for an examiner sample, for an internal-audit re-derivation — it references the lake by pointer rather than caching a copy. This keeps the platform's storage footprint bounded to the register-tier shape and the two-store convergence intact.

**Property 2 — the platform binds normalised events to mod-108 evidence artefacts via the mod-108 evidence contract.** Every normalised event that carries a `taxonomy.control_refs` list corresponds to one or more evidence artefacts in the mod-108 index — a fairness-attestation artefact, a data-quality attestation, a drift-monitor result, an LLM-quality evaluation record. The platform's write path takes the incoming event and updates the freshness index and, where applicable, the current-state field on the referenced artefact(s). A fairness signal exceeding a threshold refreshes the freshness timestamp on the fairness-attestation artefact for the affected system and marks the artefact's current-state field per the mod-108 evidence-contract state machine. The platform does not invent a parallel evidence structure; it *drives* the mod-108 structure with the incoming stream.

**Property 3 — the platform's own dashboard is a reader of the register, not a competing view.** The mod-111 platform will ship a dashboard — a portfolio view of AI systems, a residual-position view against appetite, a control-attestation freshness view, an incident-and-signal view. Every number on that dashboard is sourced from the register the platform itself holds; no number is computed from a separate observability-platform pull, a cached normalisation-layer copy, or a shadow store. When the platform's dashboard number and the register number disagree, the ruling is that the platform's read path is defective, not that two versions of the truth exist. The property is the mod-111 analogue of the mod-110 chapter 03 two-store convergence invariant: within the mod-111 platform, the *register-tier state* is the one and only source, and the dashboard, the report exports, the API responses, and the workflow-layer state transitions all read from it.

Taken together the three properties are the consumption contract. The platform is downstream of a well-defined stream; it holds no raw and no shadow; its evidence binding is mediated by mod-108; its dashboard reads its own register and nothing else. Everything else the platform does — the workflow layer, the RBAC model, the audit-facing packaging, the intake and impact-assessment surfaces — sits on top of that consumption contract without changing its shape.

## The role-coordination

The observability-adjacency wiring crosses three of the track's role scopes, and the mod-111 architect has to keep the seams clean at each boundary.

- **`ai-risk-engineer` (level 25, monitor-to-register edges at team scope).** The level-25 engineer, within a single team's scope, authors the concrete monitor-to-register edges — the mod-110 chapter 02 contract instances that map a specific observability-platform monitor on a specific system to a specific register-field write. The mod-111 architect's contract accepts the level-25 outputs by consuming the normalised events that the level-25's edges emit. The seam is: the level-25 owns the per-team edges; the level-50 owns the platform-side contract those edges compose against; neither owns the other's shape. Where the two disagree — the level-25 needs a new `signal.kind` the normalisation schema does not yet carry — the resolution runs through the mod-110 chapter 03 schema-evolution change control, ratified by the level-50 architect, not by the platform team quietly extending the schema in place.

- **`ai-evaluation-engineer` (peer, level 35, data-lake consumer for reproduction).** The level-35 evaluation engineer consumes the lake tier — the raw retention side of the mod-110 chapter 03 two-store convergence — for reproduction of first-line evaluations, for re-derivation of monitor thresholds, and for point-in-time reconstruction of evaluation state. The evaluation engineer does *not* consume the mod-111 platform's register-tier as authoritative for re-derivation; the platform's register is aggregated and rolled-up and insufficient for reproduction. The seam is: the evaluation engineer works from the lake; the mod-111 platform works from the normalised stream; the two views are consistent because both derive from the same two-store shape, and neither is trying to be the other.

- **`senior-ai-governance-architect` (level 50, this contract).** The mod-111 architect owns the consumption contract itself — the schema the platform accepts from the normalisation layer, the mod-108 evidence-binding rules, the invariant that the platform's dashboard reads the register, and the change control on the consumption contract's own version. The architect does *not* own the normalisation layer itself (that is mod-110 chapter 03's, co-owned with the enterprise data-platform team) and does *not* own the per-team monitor edges (level-25). The architect ratifies the consumption contract's versions jointly with the head of AI governance and, where the normalisation-layer schema changes affect the consumption contract, with the enterprise data-platform lead.

The three-role picture is the same shape as chapter 05's runtime-security coordination — one role owns the enterprise interface (level 50), one role owns the platform-scale operational depth (level 35), one role owns the team-scale contract edges (level 25) — with the class of signal being the only substantive difference between the two chapters.

## The three failure modes

**Failure mode (a) — the mod-111 platform stands up its own drift monitoring.** The GRC-for-AI platform's product surface tempts an implementation team into adding a parallel drift-monitoring capability inside the platform — a pull from the model registry, a comparison against a reference distribution, an alarm on threshold crossing. Six months in, the enterprise now has: Fiddler emitting fairness and drift, WhyLabs emitting drift, Evidently emitting drift in the CI pipeline, the normalisation layer emitting canonical drift events into the register, *and* the mod-111 platform emitting its own drift computation into its own register-tier field. Three sources of truth. The failure is the exact one mod-110 chapter 03 designed against, re-introduced by the mod-111 platform violating its own consumption contract. The architect closes it off by fixing the contract: the mod-111 platform is a consumer of the normalised stream and does not compute observability signal itself.

**Failure mode (b) — the mod-111 platform accepts platform-native events.** An observability vendor ships a well-supported integration that pushes its native event shape directly into the mod-111 platform, bypassing the normalisation layer. The integration is fast to stand up and demos well; six months in, the register carries vendor-native field names, downstream reports reference vendor-native metric identifiers, the mod-108 evidence contract is bound against fields that are one vendor's shape, and the enterprise has locked to the vendor by the same platform-lock mechanism mod-110 chapter 03 named — this time inside the mod-111 register rather than in the enterprise register upstream. The failure is a normalisation-layer bypass; the fix is that the mod-111 consumption contract accepts *only* the normalised event schema, and vendor-side integrations must land in the normalisation layer first.

**Failure mode (c) — an observability vendor is treated as a substitute for the system of record.** An observability vendor's dashboard is portfolio-shaped and looks like a register — a list of systems, a status per system, an owner, a last-updated timestamp, an alert count, an appetite-shaped colour code. An executive sees the dashboard and asks why the enterprise is standing up a separate GRC-for-AI platform when the vendor's UI seems to do the same thing. If the answer is that the vendor's dashboard is adopted as the enterprise view, the enterprise has lost: the mod-108 evidence contract (the vendor has no notion of evidence artefact freshness or contract state), the mod-102 control library binding (the vendor has no notion of the enterprise's `AIC-*` control identifiers), the workflow layer (intake, impact-assessment, exception-handling, audit-facing packaging), and the register-side of the mod-110 chapter 03 convergence. The observability platform is a *view over one class of signal on one class of system*; the GRC-for-AI system of record is the enterprise's authoritative register with a documented control library, evidence contract, and workflow layer bound to it. They are not substitutes and the architect must be prepared to hold that line explicitly.

## YAML-shaped consumption contract

The following is illustrative of the shape the mod-111 platform's consumption contract commits to. Field names and enumerations are enterprise-adaptable; the shape is what generalises. The event kinds named are the ones drawn from the observability category; the register-write addresses reference the mod-110 chapter 02 monitor-to-register contract addresses; the evidence artefact identifiers reference the mod-108 evidence-artefact index.

```yaml
consumption_contract:
  id: GRC-AI-OBS-CONS-v1.2.0
  version_ratified_by: [ senior-ai-governance-architect, head-of-ai-governance, enterprise-data-platform-lead ]
  upstream_schema_ref: mod-110-ch-03-normalised-event-v2.x
  accepts_event_kinds:
    - kind: drift-metric
      updates_evidence_artefact: mod-108-EA-drift-monitor-result
      register_write_field_ref: mod-110-ch-02-MRE.*.residual.likelihood
    - kind: performance-metric
      updates_evidence_artefact: mod-108-EA-performance-attestation
      register_write_field_ref: mod-110-ch-02-MRE.*.control_effectiveness_proxy
    - kind: fairness-metric
      updates_evidence_artefact: mod-108-EA-fairness-attestation
      register_write_field_ref: mod-110-ch-02-MRE.*.residual.category-fairness
    - kind: data-quality-metric
      updates_evidence_artefact: mod-108-EA-data-quality-attestation
      register_write_field_ref: mod-110-ch-02-MRE.*.control_effectiveness_proxy
    - kind: llm-quality-metric
      updates_evidence_artefact: mod-108-EA-llm-quality-evaluation
      register_write_field_ref: mod-110-ch-02-MRE.*.residual.category-llm-quality
  raw_telemetry_reference:
    holds_locally: false
    references_by_pointer_to: enterprise-data-lake
  dashboard_read_path:
    source: this-platform-register-tier
    permitted_secondary_sources: none
  refuses:
    - platform-native-event-shapes
    - vendor-console-as-source-of-truth
    - locally-computed-observability-signal
```

The `accepts_event_kinds` block is the exhaustive list of what the platform ingests; anything outside the list is refused at the boundary and referred back to the normalisation-layer schema-evolution process. The `updates_evidence_artefact` field is the mod-108 binding; the `register_write_field_ref` is the mod-110 chapter 02 address. The `raw_telemetry_reference` block fixes property 1 of the contract — the platform does not hold raw, it points to the lake. The `dashboard_read_path` block fixes property 3 — one source, no secondary. The `refuses` block names the three failure modes back as explicit non-goals, so an implementation team reading the contract cannot claim the contract was ambiguous about them.

## Invariants

The consumption contract turns on three invariants; they are the mod-111 restatement of the mod-110 chapter 03 invariants viewed from the consumer end.

**Invariant 1 — the platform is a consumer, never a competing source.** The mod-111 platform reads the normalised event stream, writes into its own register-tier fields per the mod-110 chapter 02 contract, and does not compute observability signal itself. Any capability the platform product surface offers that would produce a parallel signal is out of scope for the mod-111 architecture and disabled at configuration. This invariant is what makes failure mode (a) impossible by construction.

**Invariant 2 — the normalised event schema is the contract.** The platform's ingestion surface accepts only events conforming to the mod-110 chapter 03 normalised schema at its current ratified version. Platform-native vendor events are refused at the boundary. Schema evolution runs through the mod-110 chapter 03 change-control process, ratified by the level-50 architect; the mod-111 consumption contract carries a schema-version reference and is re-ratified when the upstream schema major-version bumps. This invariant is what makes failure mode (b) impossible by construction.

**Invariant 3 — the mod-108 evidence contract mediates the binding to specific control artefacts.** No normalised event writes directly into a mod-102 control state; the write path always runs through the mod-108 evidence artefact that substantiates the control. A fairness signal updates the fairness-attestation artefact per the mod-108 state machine; the artefact's state then flows into the mod-102 control's attestation status per the mod-108 evidence contract. This preserves the separation between *signal* (what the observability platform produces), *evidence* (what mod-108 governs), and *control state* (what mod-102 defines), and it means a change to how signal maps to evidence (a new mod-108 artefact type, a change to an evidence-freshness rule) does not require a change to the mod-102 control library — the seam holds.

## Summary

The AI-observability tooling category — Fiddler AI, Arthur AI, WhyLabs / whylogs, Evidently AI, and adjacent successors — is a sibling to the GRC-for-AI system of record, not a competitor with it. Mod-110 chapter 03 designed the wiring — normalisation layer, two-store convergence, two-tier lake retention, register-tier consumer set. This chapter fixed the consumption contract at the GRC-for-AI end: the mod-111 platform consumes normalised events downstream of the normalisation layer, binds those events to mod-102 controls via the mod-108 evidence contract, and treats its own dashboard as a reader of its own register rather than a competing view. Three failure modes were designed against — the platform standing up parallel drift monitoring, the platform accepting vendor-native event shapes, the vendor's dashboard being adopted as the enterprise system of record — and the consumption contract's `refuses` block names each back explicitly. The role-coordination runs across level-25 (per-team monitor-to-register edges), level-35 (data-lake consumer for reproduction), and level-50 (this contract). The three invariants — the platform is a consumer never a competing source, the normalised event schema is the contract, the mod-108 evidence contract mediates the control binding — are the mod-111 analogue of the mod-110 chapter 03 invariants and hold together with them. Chapter 05 is the sibling for the runtime-security signal class; chapter 07 will move on to the enterprise-GRC integration shape — Archer, ServiceNow GRC, MetricStream — that governs how the mod-111 register interoperates with the broader enterprise risk store.
