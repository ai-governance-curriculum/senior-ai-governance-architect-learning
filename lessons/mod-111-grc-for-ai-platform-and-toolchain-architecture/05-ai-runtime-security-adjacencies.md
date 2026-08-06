# AI-runtime security adjacencies — Robust Intelligence, Lakera Guard, Calypso AI, HiddenLayer, Protect AI as siblings

## Why this chapter exists (the sibling-vs-competitor mis-framing; the double-buy and the gap)

An enterprise standing up its GRC-for-AI system of record almost always meets, in the same procurement window, a vendor from the AI-runtime security category. The account executive from the runtime-security vendor is persuasive: their product blocks prompt-injection attempts before they reach the model, screens outputs before they leave the model, and scans model artefacts for backdoors before they are deployed. It has a dashboard the CISO likes, a policy language the security team understands, and integration hooks into the inference path that the platform engineers can wire in a sprint. Somewhere in the demo the account executive says the phrase that starts the failure mode: "you don't really need a separate GRC layer once you have this."

The mis-framing runs in both directions and produces two symmetric failures. The first is the *double-buy*: the enterprise procures both a GRC-for-AI system of record and a runtime-security vendor, but no one architects the interface between them. The runtime tool blocks a hundred prompt-injection attempts a day; the GRC register carries no record that the attempted-violation rate against a given system tripled over the quarter. The runtime tool scans an incoming model and finds a suspicious weight-signature; the mod-108 evidence index has no artefact citing the scan as substantiation for the control that requires pre-deployment model-integrity checks. The two systems are running in parallel and each is doing its job in isolation, but the enterprise has spent twice for coverage it is not consolidating and cannot answer a single audit question about.

The second failure is the *gap*: the enterprise buys the runtime-security vendor, treats its dashboard as sufficient governance, and does not stand up (or defers) the GRC-for-AI system. When the certification body asks for the control-effectiveness history on the prompt-injection guardrail across the last twelve months of production traffic, the runtime tool can show current policy state and a rolling-window incident count from its console; it cannot produce the point-in-time control-attestation record, tied to the mod-102 control id, referencing the mod-108 evidence contract, with the sign-off chain the mod-107 second-line assurance flow requires. The runtime tool's console is a *view* onto its own operational state; it is not the enterprise's register.

The level-50 architect fixes the framing before either failure mode has a chance to root. The runtime-security tools are **siblings** to the GRC-for-AI system of record. They are producers of AI-nexus signal into the enterprise stores (per mod-110 chapter 03's normalisation seam). They are substantiating sources for control attestations (per mod-108's evidence architecture). Their own configuration state is documented information under the AIMS (per mod-105). They are not the register. They do not compete with the register; they feed it. This chapter walks the shape.

## The runtime-security tool category at a glance

The AI-runtime security vendor category has emerged over the last several years as a distinct segment from both classical application security and model observability. The tools in the category share a common shape: they enforce something *at inference time* or *at build time* against an AI system, and they emit signal about what they enforced. The category subdivides into two shapes.

**Inference-time enforcement — the LLM firewall / guardrail shape.** The tool sits in the request/response path between the caller and the model, or between the model and a downstream tool. It inspects inbound prompts for injection patterns, PII, policy-forbidden content, jurisdiction-restricted material; it inspects outbound completions for hallucinated citations, exfiltrated internal document content, tool-call misuse, jailbreak-success signatures; it enforces a policy that blocks, redacts, degrades, or logs based on what it sees. Vendors positioning in this shape include:

- **Lakera Guard** — prompt-injection and content-safety guardrail library, positioned as a policy-driven filter over LLM inputs and outputs. `<!-- needs-research: verify current Lakera Guard product scope (guardrail SDK vs hosted API), the enumerated policy categories, and the named platform integrations against Lakera's current product documentation -->`
- **Calypso AI** — LLM firewall and enterprise policy enforcement layer, positioned around inbound/outbound content policy for enterprise assistant deployments. `<!-- needs-research: verify Calypso AI's current product name, enforcement points (proxy vs SDK vs sidecar), and integration surface against vendor documentation -->`
- **Robust Intelligence** — AI validation gateway and model firewall, positioned to enforce both pre-deployment validation gates and runtime input/output policy. `<!-- needs-research: verify Robust Intelligence's current product scope, split between AI Validation and AI Firewall, and named platform integrations against vendor documentation -->`

**Build-time and continuous scanning — the model-integrity / MLSecOps shape.** The tool inspects model artefacts, training pipelines, model registries, and inference infrastructure for supply-chain compromise, backdoors, malicious serialisation payloads, adversarial-perturbation vulnerability, and policy violations against the enterprise model catalogue. Vendors positioning in this shape include:

- **HiddenLayer** — model-integrity monitoring and adversarial-attack detection, positioned to detect model theft, adversarial input patterns, and integrity violations against deployed models. `<!-- needs-research: verify HiddenLayer's current product split (Model Scanner, AISec Platform, MLDR) and named runtime-detection categories against vendor documentation -->`
- **Protect AI** — MLSecOps platform bundling model scanning (Guardian / ModelScan lineage), policy enforcement, and runtime protection. `<!-- needs-research: verify Protect AI's current product portfolio (Guardian, Layer, Recon, Sightline, ModelScan) and how build-time vs runtime enforcement points are split against vendor documentation -->`

The naming is orientation to the category shape, not endorsement of any specific vendor, and the vendor landscape shifts. Names shift too — a vendor with an LLM-firewall product this year may add a model-integrity scanner next year, or vice versa. The architecture does not depend on which vendor sits in which cell; it depends on the sibling framing and the integration shape this chapter names.

The point common to the whole category: these tools **enforce** and **detect** at runtime or at build time. That is what makes them siblings to the GRC-for-AI system — the register does not enforce and does not detect; it records and adjudicates. Two different jobs; two different components.

## Why the GRC-for-AI system does NOT enforce runtime (and what it does instead)

The clearest way to fix the sibling framing is to be explicit about what the GRC-for-AI system does not do. It does not sit in the inference path. It does not inspect prompts. It does not block outputs. It does not scan model weights. It does not run continuously against production traffic. It has no runtime enforcement point.

The reasons are architectural, not accidental.

**Latency budgets.** A GRC-for-AI platform is workflow-shaped — intake, assessment, evidence collection, attestation, audit packaging. The p99 latency budget for a GRC workflow step is measured in minutes to hours; the latency budget for an inference-path enforcement point is measured in milliseconds. The two are not the same component and cannot be collapsed without breaking one or the other.

**Failure-mode separation.** An enforcement point that fails to enforce means an attack succeeds; an enforcement point that fails-closed means the system stops serving traffic. Either failure is an operational incident with a short response window. A register that misses a control-attestation update is a governance defect with a longer response window and a very different escalation shape. Combining the two into one system tangles the failure modes and forces the enterprise to pick which one it will run at incident-response cadence — and whichever loses, loses badly.

**Authority separation.** The runtime tool's output is *observations* — it saw an attempt, it blocked a completion, it flagged a model. The register's output is *adjudicated state* — the residual on this risk category is above appetite, the control is effective, the incident is a serious incident under Article 73. Adjudication requires the second-line assurance flow (mod-107), the evidence contract (mod-108), and the risk-appetite reference (mod-106). Runtime tools do not carry those references natively and should not.

What the GRC-for-AI system does *instead*:

- It **consumes signal** from the runtime tools via the mod-110 chapter 03 normalisation layer, and turns those signals into register writes per the mod-110 chapter 02 monitoring-to-risk-register contract.
- It **holds control-attestation artefacts** (per mod-108) that cite the runtime tool's telemetry as substantiation for controls in the mod-102 library — `AIC-SEC-PROMPT-INJECTION-GUARDRAIL`, `AIC-SEC-MODEL-INTEGRITY-SCAN`, `AIC-SEC-OUTPUT-CONTENT-FILTER`, and so on.
- It **documents the runtime tool's configuration state** — which policies are active, which are in tuning, which have been deprecated — as AIMS documented information (per mod-105 chapter 07's handling of documented information).
- It **triggers control-testing re-runs** in the workflow layer (chapter 03 of this module) when a runtime tool's policy drifts, is deprecated, or is materially changed.
- It **packages the runtime tool's evidence** into the audit-facing bundles the certification body and the third-line internal audit sample against.

The runtime tool cannot do those things. The GRC-for-AI system cannot enforce runtime. Neither is a substitute for the other. Both are necessary; each does what only it can do.

## The role-coordination triangle — level-25 ai-risk-engineer, level-35 ai-infra-security, level-50 architect — and who owns what

The runtime-security tool category sits inside a three-role coordination shape the level-50 architect must fix, or per-team fragmentation is the default outcome. The three roles this track shares peers with are all in play here, and each owns a different altitude of the same problem.

**`ai-risk-engineer` (level 25) — the guardrail configuration at team scope.** The engineer on a single AI system's team writes the concrete Lakera policy set, the Calypso rule bundle, the Robust Intelligence validation-test suite, the HiddenLayer model-scan configuration, the Protect AI Guardian policy for their model registry. They pick thresholds, tune false-positive rates against their traffic, and iterate the policy against their production evaluation loop. Their scope is *this system, this model, this policy set*. They implement what the enterprise contract says they must implement.

**`ai-infra-security` (level 35) — the platform-scale defence and the SOC interface.** The security engineer at platform altitude runs the SIEM integration (per mod-110 chapter 05), owns the detection engineering that consumes the runtime tools' event streams, runs the incident-response playbook when the runtime tool escalates, and coordinates threat-intel updates back into the runtime tools' policy sets. Their scope is the platform footprint — every system that runs under the platform's runtime-security enforcement, and the incident response across them.

**`ai-governance-architect` (level 50, this track) — the enterprise contract, the sibling interface, and the audit-defensible shape.** The architect chooses which runtime tools sit in the enterprise sibling set, writes the enterprise contract each level-25 engineer composes to, designs the integration shape into the mod-110 stores, ties the runtime tools' signals to the mod-102 control library and the mod-108 evidence architecture, and owns the AIMS documentation of the runtime tools as controlled documented information. Their scope is the whole footprint plus the external-audit-facing shape.

The triangle fails predictably when any vertex tries to do another vertex's job. If the level-25 engineer picks the runtime tool without the level-50 enterprise contract, per-team fragmentation results (failure mode (c) below). If the level-50 architect writes the policy set itself, the tuning loop is too slow to be operationally useful and the level-25 engineer disengages. If the level-35 security engineer treats the runtime tool as an internal SOC concern and does not surface signal into the register, the register goes stale on its highest-signal categories (failure mode (b) below). The three roles are not interchangeable; each owns a distinct altitude of the same set of tools.

Cross-references keep this discipline honest: chapter 03 of this module (the workflow layer) is where the architect's contract turns into workflow-tickets the level-25 engineer answers; chapter 04 (the RBAC / segregation-of-duties model) is where the three-role separation is enforced in the platform itself; mod-110 chapter 05 is where the level-35 role's SOC interface is designed.

## The integration shape — how signals enter the enterprise stores

The runtime-security tools' signals enter the enterprise stores through exactly the same pathway that the observability platforms' signals enter through — the mod-110 chapter 03 two-store, normalisation-layer shape. This is not incidental. The reason the shape is the same is that the invariants are the same: single canonical event schema across all producers, two authoritative stores (the GRC register and the enterprise lake), no producer's native shape entering either store unmodified, every signal catalogued or explicitly out-of-scope.

The runtime tool is a **producer** into the normalisation layer. Its native emission shape — a Lakera Guard block-event with its own field vocabulary, a HiddenLayer scan-finding with its own severity scale, a Calypso AI policy-violation with its own category enumeration — is mapped by a per-vendor adapter into the canonical event schema before the event reaches either store. The `source` block on the canonical event carries the vendor-specific reference; every field downstream is platform-agnostic.

The signal types the category emits, and where each lands:

- **Attempted-violation events** (prompt-injection detected, PII in input, jailbreak-signature match, output-content flag) — high-volume, low-severity individually, meaningful in aggregate. They land in the enterprise lake for raw retention (hot tier for weeks to months, cold tier for the regime-obligation window) and are rolled up into the register on the monitor-to-risk-register contract (mod-110 chapter 02) — likelihood-shift on the corresponding mod-106 category, exposure adjustment on the affected system's residual, control-effectiveness proxy for the mod-102 guardrail control the tool substantiates.
- **Blocked-output events** — the runtime tool prevented an unsafe or policy-violating response from leaving the model. These land as both a lake write (raw retention) and a register write (aggregated block-rate against the mod-102 output-filter control's attestation), and — if the blocked output content itself carries security implications (an exfiltration attempt) — trigger the PMS→SOC handoff (mod-110 chapter 05).
- **Model-scan findings** — a scanner has flagged a model artefact for a suspicious signature, a serialisation-payload risk, a backdoor candidate, an adversarial-example vulnerability. These land in the lake with the full scan report, in the register as a finding tied to the affected `system_id` and the pre-deployment gate (mod-107 chapter 02) it feeds, and are packaged into the mod-108 evidence artefact for the corresponding pre-deployment control.
- **Policy-drift events** — a runtime tool's active policy set has changed (a policy was added, tuned, or deprecated by the level-25 engineer). These land in the register as documented-information updates against the AIMS (mod-105), and trigger a workflow-layer control-testing re-run (chapter 03 of this module) so the register's attestation on the corresponding control reflects the new policy state and is re-substantiated.
- **Tool-health events** — the runtime tool itself failed, degraded, or was bypassed. These land in the register as control-effectiveness alarms — a runtime enforcement point that is not enforcing is a failed control until it is restored — and fire the appetite-alarm on the corresponding mod-106 category if the outage is sustained.

The GRC-for-AI system does not read the runtime tool's console directly. It reads the register writes the normalisation layer produced. This is what preserves the two-store invariant: even when the register carries a residual-shift on the "prompt-injection-succeeded" category that was driven by Lakera Guard telemetry, the register's cited source is *the register-write from the normalisation layer that referenced the lake retention that referenced the Lakera adapter's emission* — not the Lakera dashboard. Swapping Lakera for another vendor changes the adapter, not the register.

The register's writes flow into the downstream consumers mod-110 chapter 03 already named: incident enrichment, chapter 02 contract writes, mod-108 evidence freshness pings, mod-107 pre-deployment gate re-affirmation, mod-107 ongoing-assurance re-assessment triggers. Each of those consumers reads the register (or the lake, via the register's pointer) and never reads the runtime tool directly. The runtime tool becomes swappable; the register stays authoritative.

## The failure modes (three: substitute mis-framing, refuse-to-integrate mis-framing, per-team fragmentation)

**Failure mode (a) — substitute mis-framing.** The enterprise treats a runtime-security vendor as a replacement for the GRC-for-AI system of record. The vendor's dashboard becomes the CISO's default view, the CFO's AI-materiality reader is pointed at the vendor's monthly export, and the head of AI governance is told the register can wait because "we already have coverage." Six months later the ISO/IEC 42001 certification body asks for the point-in-time control-attestation history for the prompt-injection guardrail control across the audit period. The vendor's console can show current policy state and a rolling incident count; it cannot produce the control-id-referenced, second-line-signed-off, evidence-artefact-backed attestation history the certification body samples against. No mod-108 evidence artefacts landed in the index. No sign-off chain exists. The audit finding is that the control is not evidenced; the runtime tool's telemetry is described in the vendor's format and is not the enterprise's evidence. The remediation is standing up the GRC-for-AI system after the fact and back-populating a year of missing attestations — expensive, contested, and often audit-fatal for the current cycle.

**Failure mode (b) — refuse-to-integrate mis-framing.** The GRC-for-AI programme treats the runtime-security tools as competitive threats to its own remit and declines to integrate. The runtime tool's signal stream stays inside the SOC's perimeter; the register carries a residual on the "prompt-injection-succeeded" category that reflects only the manual reviews and the point-in-time evaluations, not the tens of thousands of blocked attempts the runtime tool is seeing per day. The register is stale on its highest-signal category — the runtime tool's telemetry *is* the leading indicator on prompt-injection risk, and the register has severed itself from it. When the first successful injection lands, the appetite-alarm fires against a residual that never reflected the true threat surface, and the incident-response retrospective (mod-107 second-line) finds that the runtime tool had been detecting the precursor pattern for months without the register ever seeing it. This is the mirror of failure mode (a): where (a) is the register missing because the runtime tool was mis-framed as a substitute, (b) is the runtime tool's signal missing because the register was mis-framed as autonomous.

**Failure mode (c) — per-team fragmentation.** The level-25 `ai-risk-engineer` on each team picks the runtime tool that fits their stack, without a level-50 enterprise contract to compose to. Team A uses Lakera; team B uses Calypso; team C wrote its own guardrail library and thinks that is fine; team D uses Protect AI Guardian for model scanning but nothing at inference time; team E uses Robust Intelligence for both. The mod-102 `AIC-SEC-PROMPT-INJECTION-GUARDRAIL` control's attestation across the portfolio is a patchwork of five different substantiation shapes with five different signal vocabularies, and the enterprise cannot answer a portfolio-scale question about prompt-injection posture without hand-work. The level-50 architect unwinding this ends up rationalising toolchain choices under a written enterprise contract long after the choices were made — a multi-quarter piece of work whose commercial and organisational cost is entirely the price of not writing the contract on day one. The contract does not need to mandate a single vendor; it needs to name the enforced integration shape (adapter to the normalisation layer, canonical event schema, mod-102 control mapping, mod-108 evidence contract) that every runtime tool in the portfolio composes to, so that vendor plurality does not become integration fragmentation.

## YAML-shaped integration schematic

The following is illustrative of the enterprise contract shape a level-50 architect writes for a single runtime-security tool in the sibling set. Every tool in the portfolio has one of these; the schemas across tools are consistent by design.

```yaml
runtime_security_sibling:
  tool_id: RST-2027-LAKERA-GUARD
  vendor: lakera
  product: guard
  category: inference-time-enforcement
  enterprise_contract_owner: senior-ai-governance-architect
  operational_owner: ai-infra-security-lead
  team_engineer_seat: ai-risk-engineer (per-system)
  aims_documented_information_ref: AIMS-DOC-2027-RST-LAKERA-v1.2
  enforcement_scope:
    - inbound-prompt-inspection
    - outbound-completion-inspection
  signal_types_emitted:
    - kind: prompt-injection-attempted
      severity: low-individually
      lake_tier: hot-30d then cold-obligation-window
      register_contract_edge: MRE-2027-prompt-injection-attempted-rate
      mod102_controls_substantiated: [ AIC-SEC-PROMPT-INJECTION-GUARDRAIL ]
      mod106_categories_scored: [ RSK-CAT-prompt-injection-succeeded ]
    - kind: output-content-blocked
      severity: variable-per-policy
      lake_tier: hot-90d then cold-obligation-window
      register_contract_edge: MRE-2027-output-block-rate
      mod102_controls_substantiated: [ AIC-SEC-OUTPUT-CONTENT-FILTER ]
      mod106_categories_scored: [ RSK-CAT-confidential-info-exfiltration ]
      pms_to_soc_handoff: conditional-on-exfiltration-signature
    - kind: policy-drift-event
      severity: control-attestation-affecting
      lake_tier: cold-obligation-window
      register_contract_edge: MRE-2027-runtime-policy-drift
      triggers_workflow: control-testing-re-run (mod-111 ch-03)
      aims_documented_information_update: required
    - kind: tool-health-event
      severity: control-effectiveness-alarm
      lake_tier: hot-30d
      register_contract_edge: MRE-2027-runtime-tool-health
      appetite_alarm_on_sustained_outage: true
  evidence_contract:
    mod108_evidence_artefact_shape: RST-TELEMETRY-EXPORT-v1.4
    freshness_window: 30-days
    substantiation_scope: per-control per-system per-attestation-window
  integration_shape:
    normalisation_adapter_ref: ADP-2027-LAKERA-v1.3
    canonical_event_schema: normalised_event (mod-110 ch-03 v2.1)
    two_store_write: [ enterprise-data-lake, grc-for-ai-register ]
  role_coordination:
    level_25_ai_risk_engineer:
      owns: per-system-policy-tuning
      composes_to: this-contract
    level_35_ai_infra_security:
      owns: detection-engineering + soc-interface
      references: mod-110-ch-05
    level_50_governance_architect:
      owns: this-contract + adapter-schema + control-mapping
  deprecation_policy:
    trigger: vendor-swap or product-eol
    workflow: control-testing-re-run + adapter-re-wire + attestation-history-preserved-in-lake
```

The schema binds the invariants: a named enterprise contract owner, an explicit adapter to the normalisation layer, canonical event schema shared across all sibling tools, and role-coordination fields that keep the three-vertex triangle intact.

## Invariants

**Invariant 1 — runtime security tools are producers into the mod-110 event store, not authorities in themselves.** No runtime tool's console is a source of truth for the enterprise. Its dashboard is a view onto its own operational state. The enterprise's truth for what the tool observed is the normalisation-layer event, the lake retention, and the register write. When the vendor's console and the register disagree, the register wins because the register carries the audit-defensible substantiation chain.

**Invariant 2 — every runtime tool has a named enterprise contract owner and a written contract of the shape above.** No runtime tool enters the enterprise's inference path or model registry without a level-50-owned contract that names the integration shape, the signal-type mappings, the mod-102 controls substantiated, the mod-108 evidence contract, and the AIMS documented-information reference. Silent adoption by a single team, without the contract, is the failure-mode-(c) attack surface.

**Invariant 3 — role separation between level-25, level-35, and level-50 is preserved in the contract and in the platform.** The level-25 engineer tunes the policy set for their system; the level-35 security engineer runs the SIEM integration, the detections, and the incident response; the level-50 architect writes and maintains the enterprise contract. The RBAC model in chapter 04 of this module enforces the separation in the GRC-for-AI platform itself; the contract enforces it at the process level.

## Summary

The AI-runtime security tool category — Lakera Guard, Calypso AI, Robust Intelligence, HiddenLayer, Protect AI, and the successor and adjacent vendors that will follow them — is architecturally a **sibling set** to the GRC-for-AI system of record. The runtime tools enforce and detect at inference time and build time; the GRC-for-AI system records, adjudicates, evidences, and packages for audit. Neither is a substitute for the other; both are necessary; the interface between them is what the level-50 architect designs. The interface reuses the mod-110 chapter 03 shape: a per-vendor adapter into a canonical normalised event schema, dual-write into the enterprise data lake and the GRC register, no vendor console cited as authority, no vendor field entering either store unmodified. The three-role triangle — level-25 `ai-risk-engineer` (per-team policy tuning), level-35 `ai-infra-security` (platform-scale defence and SOC interface), level-50 architect (enterprise contract and integration shape) — is the coordination discipline that keeps the sibling set coherent across the portfolio. Three failure modes are common enough that the architecture is designed against them: the substitute mis-framing (runtime tool treated as replacement for the register, no mod-108 evidence lands, audit failure follows), the refuse-to-integrate mis-framing (register kept autonomous from runtime signal, highest-signal category stays stale, incident lands against a wrong residual), and per-team fragmentation (level-25 engineers pick tools without a level-50 contract, portfolio attestation becomes patchwork). The YAML contract binds the invariants — producers not authorities, named enterprise contract owner per tool, role separation preserved — and is the artefact the architect writes once per sibling in the set. Chapter 06 walks the observability adjacencies — Fiddler, Arthur, WhyLabs, Evidently — as the second sibling category the GRC-for-AI system consumes signal from; chapter 07 walks the RBAC and segregation-of-duties model that enforces the three-role separation inside the platform itself.
