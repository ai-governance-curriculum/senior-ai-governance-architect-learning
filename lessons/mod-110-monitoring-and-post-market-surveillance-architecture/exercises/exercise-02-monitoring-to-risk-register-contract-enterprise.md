# exercise-02: Monitoring-to-Risk-Register Contract at Enterprise Scale

**Estimated effort:** 3 hours

## Objective

Produce the **enterprise-scope meta-contract** that fixes the shape every team-scope monitoring-to-risk-register instance-contract must fit at a specified enterprise scenario. This is the artefact you would take to the `head-of-ai-governance` to ratify *before* any `ai-risk-engineer` (level 25) is allowed to author or version a team-scope contract against it, and *before* the mod-106 portfolio aggregation can defensibly claim that residuals across the population have coherent semantics.

The deliverable is a decision document, a machine-readable schema, at least twelve worked edge fragments, a cardinality-and-aggregation decision record, and a monitor-sprawl guard process. Downstream artefacts — the observability wiring in chapter 03, the Article 73 workflow in chapter 04, the SOC interface in chapter 05, and every team-scope contract the risk engineers author — all bind to the shape you fix here. Get it right and portfolio queries return truthful answers; get it wrong and every instance-contract inherits a shape mismatch that surfaces at the audit committee months later, when the register and the runtime have already diverged.

You are *not* authoring an instance-contract. That is the risk engineer's job at level 25 and it is deliberately out of scope. You are authoring the shape they must conform to.

## Prerequisites

- Chapter [`02-monitoring-to-risk-register-contract-at-enterprise-scale.md`](../02-monitoring-to-risk-register-contract-at-enterprise-scale.md) read once, with the five fields, the three invariants, and the three failure modes marked.
- Chapter [`01-eu-ai-act-article-72-and-the-enterprise-post-market-surveillance-shape.md`](../01-eu-ai-act-article-72-and-the-enterprise-post-market-surveillance-shape.md) for the post-market-surveillance obligations that make the contract non-optional for high-risk systems.
- Chapter [`03-observability-platform-wiring-and-the-single-source-of-truth.md`](../03-observability-platform-wiring-and-the-single-source-of-truth.md) for the platform-of-origin closed-world list and the single-source-of-truth stance.
- The mod-106 chapter 04 walk-through of portfolio aggregation — the meta-contract's reason for existing is that this aggregation must be honest.
- The mod-107 chapter 03 walk-through of ongoing assurance — every assurance-trigger threshold you specify feeds a re-assessment scope defined there.
- The `ai-risk-engineer` (level 25) role scope note — the peer whose instance-contracts your meta-contract governs. You fix the shape; they fill the instances. Do not do their job.
- [`../resources.md`](../resources.md) for primary references (EU AI Act Article 72, ISO/IEC 42001 Clause 9, and the observability-platform vendor documentation to the extent you cite it).

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose monitoring-to-register shape you are least familiar with; that is where the exercise will teach you most. State your choice at the top of the deliverable.

- **A US regional bank** (Northbrook Financial-style, per mod-101 exercise-02 and mod-102 exercise-01) with ~40 AI systems: fraud classifiers, document extraction, an internal RAG legal assistant, a customer-facing generative chat, third-party AI SaaS integrations. Colorado + NYC deployments; EU expansion planned. An SR 11-7-aligned MRM programme and an ISO/IEC 27001 ISMS already in place; a Fiddler-style model-observability platform and a mature SOC already stood up.
- **A global healthcare payer / provider** with clinical-decision-support pilots, patient-facing chat, coding automation, and utilisation-management AI. US federal HIPAA scope; EU AI Act relevance for European insurance subsidiaries; multiple US state deployments including Illinois and California. A clinical-safety oversight committee reports to the CMO; observability is a mix of Arthur-style clinical-model monitoring and in-house telemetry.
- **A B2B SaaS platform vendor** shipping GenAI-augmented HR-tech capabilities into enterprise customers across the US, UK, EU, and Singapore. Customers include public-sector deployments (subject to OMB M-25-21 shape) and financial-services enterprises (subject to SR 11-7 vendor-review shape). WhyLabs-style LLM-quality monitoring, a co-managed SOC, and a customer-facing per-tenant telemetry surface already exist.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`monitoring-register-contract-v1.0.0.md`** — the decision document for the enterprise-scope meta-contract that fixes the shape every team-scope contract must fit. Justifies the deviation from the team-scope contract shape at level 25.
2. **`contract-schema-v1.0.0.yaml`** — the enterprise schema every team-scope contract must conform to. Fields per edge: monitor id + origin platform, register field id, cadence, threshold shape (operational vs assurance-trigger), owner routing with SLA, taxonomy reference (closed-world to mod-106), SoA-control reference (closed-world to mod-105 chapter 05).
3. **At least TWELVE `edges/edge-*.yaml` fragments** illustrating the shape across categories: drift, performance, fairness, adversarial-robustness, LLM-quality, and SOC-sourced. Each edge references a plausible mod-106 taxonomy category and a plausible mod-102 control id. At least one edge is SOC-sourced (per chapter 05) writing to a governance register field.
4. **`cardinality-and-aggregation.md`** — the decision record for how tens of thousands of daily emissions roll up to hundreds of register writes without swamping owners (aggregation windows, de-duplication rules, hysteresis on threshold flapping, quiet-hours suppression during remediation).
5. **`sprawl-guard.md`** — the process for preventing "monitor sprawl" (teams standing up monitors on team-scoped platforms without registering under the enterprise contract). Names the discovery cadence, the enforcement mechanism, and the deprecation path for orphan monitors.

## Requirements

### `monitoring-register-contract-v1.0.0.md`

Decide and justify **each** of the following:

- **Scope statement.** What the meta-contract governs (the *shape* of the edge, the closed-world enumerations, the invariants, the change-control) and what it explicitly does not (the monitors themselves, the register schema, the incident-classification scheme, the pre-deployment gate). Draw the boundary against chapter 02's "what the meta-contract is not" list and defend any deviation.
- **Level-50 vs level-25 contract boundary.** Explain, in the voice of a person who has to defend this to both the head of AI governance and to the risk engineers whose work you are constraining, *why* the meta-contract exists at level 50 and the instance-contract at level 25. Name the specific failure that would occur if the shape were left to each team.
- **The five fields.** For each of the five fields chapter 02 pins (monitor id + platform of origin, register field id with taxonomy + SoA references, cadence, threshold shape, owner routing), state the closed-world enumeration your enterprise commits to at v1.0.0 and the process for amending it.
- **The three invariants.** For each of the three chapter-02 invariants (every monitor writes or is deprecated; every register field has a named freshness owner; every threshold has a routing target with a stated SLA), name the specific design choice that enforces it *and* the detection test that would surface a violation. Machine-enforced beats after-the-fact review — say so where it applies.
- **Composition with adjacent modules.** State the specific interfaces to mod-106 (taxonomy reference, appetite/tolerance table), mod-105 chapter 05 (SoA control reference), mod-107 chapter 03 (assurance-trigger consumption), mod-108 (evidence-freshness fields), and mod-111 (where the meta-contract itself lives and validates). Interfaces are named, not hand-waved.
- **Change-control.** How the meta-contract is versioned (semver-shaped), who ratifies (`head-of-ai-governance` on the architect's recommendation), how instance-contracts migrate when the meta-contract supersedes the version they were authored against, and how the migration is tracked.
- **Non-scope.** At least three things you *chose not to* include in the meta-contract and why. Candidates: the register schema itself (mod-106 owns it, not you); the incident-classification scheme (chapters 04 and 05 own it); a duplicate copy of the SoA (mod-105 owns it); per-team dashboard shapes (team-scope, not enterprise-scope).

### `contract-schema-v1.0.0.yaml`

The schema every team-scope contract must conform to. Every edge must bind **all five** fields. Every enumeration is closed-world.

At minimum, the schema declares:

- The top-level `edge` object shape with a stable `id` pattern, a required `monitor` block (nested `id` + `platform_of_origin`), a required `register_field` block (nested `field_id` + `taxonomy_ref` + `soa_control_ref`), a required `cadence` value, a `threshold` block with optional `operational_alarm` and `assurance_trigger` sub-blocks (at least one required), a required `owner_routing` block, and a `version_history` list.
- The closed-world enumerations for `platform_of_origin`, `cadence`, `taxonomy_ref` prefix, `soa_control_ref` prefix, and the seat-registry pattern for `owner_routing` seats.
- The `threshold` sub-block shape (rule expression, responder seat, SLA, escalation-on-miss seat).
- Where an `assurance_trigger` is present, the required `assurance_rescope_ref` field naming the ongoing-assurance re-assessment scope (per mod-107 chapter 03) that fires when the trigger crosses.
- The validation rules the mod-111 GRC platform enforces at edge-creation time — no free-text values in any closed-world field; every threshold has a responder, SLA, and escalation; every `register_field.field_id` exists in the register schema; every `assurance_trigger` names an `assurance_rescope_ref`.

The schema is machine-parseable. A team-scope contract that does not conform is rejected by the GRC platform, not by human review after the fact.

### `edges/edge-*.yaml` fragments

At least twelve edge fragments demonstrating the schema across categories. Each fragment binds all five fields and references a plausible mod-106 taxonomy category and a plausible mod-102 control id — invented control ids are acceptable so long as they follow a defensible naming pattern (e.g., `AIC-FAIR-004`) and the fragment states the taxonomy and control anchor.

Cover, at minimum:

- Two **drift** edges (feature-drift and prediction-drift, feeding residual-likelihood or residual-composite fields).
- Two **performance** edges (accuracy/AUC-style degradation feeding residual-composite or exposure).
- Two **fairness** edges (disparate-outcome or subgroup-metric drift feeding a fairness-category residual).
- Two **adversarial-robustness** edges (evasion or extraction-attempt rate feeding a security-category residual or exposure).
- Two **LLM-quality** edges (groundedness/toxicity/prompt-injection-attempt-rate feeding LLM-category residuals; adapt to your scenario's LLM footprint).
- At least one **SOC-sourced** edge (per chapter 05) — an AI-specific SOC detection routed as a governance-register write, not merely a security ticket, demonstrating the SOC-to-governance interface interoperability.
- At least one **evidence-freshness** or **attestation-freshness** edge (per mod-108 composition) — no first-line remediation available, only a second-line re-attestation route, so `operational_alarm` is legitimately absent and the omission is stated.

Every edge names its cadence deliberately with a stated reason (not a defaulted per-emission write). Every `assurance_trigger` names the ongoing-assurance re-assessment scope it fires.

### `cardinality-and-aggregation.md`

The decision record for cardinality management. Cover:

- **A plausible traffic profile for your scenario.** State the assumed daily emission volume across the population (order of magnitude, not a fictitious precise count). Show the reduction math — how the aggregation windows collapse tens of thousands of emissions per day into hundreds of register writes per day. Do not hand-wave; a plausible arithmetic chain is what forces the architecture to be defensible.
- **Aggregation windows.** For each cadence bucket in the schema, state the aggregation function (max, mean, exceedance-count, sustained-breach-flag) the meta-contract permits and the rule for choosing between them per field type. Cadence-plus-aggregation-function is the primary reduction mechanism.
- **De-duplication rules.** For monitors that surface variants of the same underlying signal (e.g., subgroup-level fairness drifts rolling to a category-level residual), state the governing-edge rule so that only the governing edge writes and the underlying monitors surface diagnostic detail without independent writes.
- **Hysteresis on threshold flapping.** State the `sustain` and re-arm rules the meta-contract requires per threshold. A monitor whose value oscillates around a threshold is prevented from producing write-clear-write-clear noise by design, not by per-team convention.
- **Quiet-hours suppression during remediation.** State the rule for suppressing further writes on an edge while a CAPA (mod-105 chapter 09) or a remediation-in-progress is open on the affected system, and the register-visible logging that makes the suppression auditable. Suppression without logging is a defect; suppression by default is a defect.
- **Cadence-by-field-type table.** Attestation-freshness fields tolerate daily or per-review-cycle rollups; residual-composite fields on tier-3+ systems tolerate less. Publish the table so risk engineers do not have to guess.

### `sprawl-guard.md`

The process for preventing monitor sprawl (chapter 02 failure mode a).

- **Definition.** Name what counts as an orphan monitor (an emitter on any observability surface — Grafana, a bespoke Slack integration, a fine-tuning provider webhook, an ad-hoc evaluation-suite alerter — that emits on a production AI system without a corresponding meta-contract edge).
- **Discovery cadence.** How and how often the enterprise reconciles the observability platforms' monitor inventories against the meta-contract's edge inventory. Name the seat that runs the reconciliation and the report it produces.
- **Enforcement mechanism.** The operational discipline that prevents a monitor from reaching production-ready status without a corresponding edge — the mod-111 GRC platform's monitor-registration gate. State the machine-enforced check.
- **Deprecation path for orphans.** The bounded workflow that discovers, triages, and resolves an orphan monitor — the orphan is either bound to a new or existing edge (regularised) or retired from the platform (deprecated). State the SLA.
- **The three invariants restated.** Show how the sprawl guard specifically enforces invariant 1 (every monitor writes or is deprecated) and interoperates with invariants 2 and 3.

## Starter guidance

- Design the schema before drafting any edge. A schema drift discovered at edge-11 forces rework of edges 01–10. The decision document and the schema come first; the edges instantiate the schema, they do not reveal it.
- Do NOT hand-wave the cardinality math. Show a plausible traffic profile and how the aggregation windows collapse it. "Hourly rollups will handle it" is not an answer; sixty per-minute emissions to one hourly write, times the population, times the aggregation function, is.
- Cadence is not a taste choice. Field type determines cadence: attestation-freshness fields tolerate daily or per-review-cycle rollups; residual-score fields on tier-3+ systems need faster. Publish the table before you author the edges.
- Do not centralise ownership on the head of AI governance. Every threshold has ONE named routing seat with a stated SLA, and the meta-contract is broken by design if it defaults upward. The head of AI governance is the escalation-on-miss target, not the responder.
- Distinguish the enterprise-scope contract you write from the team-scope contract the `ai-risk-engineer` (level 25) writes. Yours fixes the shape; theirs fills instances. If you find yourself writing thresholds for a specific model, you have descended a level and the meta-contract has lost its universality.
- Every `assurance_trigger` must name the ongoing-assurance re-assessment scope (mod-107 chapter 03) that fires when it crosses. A trigger with no re-scope is decorative — the second line has nothing to re-evaluate.
- The twelve edges are a stress test on the schema. If any of the six categories forces you to invent a new field, the schema is not v1.0.0 yet — go back and revise.

## Acceptance criteria

- [ ] Scenario is stated at the top; the meta-contract is coherent against it.
- [ ] `monitoring-register-contract-v1.0.0.md` decides all requirements bullets (scope, level-50-vs-25 boundary, the five fields with closed-world enumerations, the three invariants with enforcing design choices and detection tests, composition with adjacent modules, change-control, non-scope) with stated rationale.
- [ ] `contract-schema-v1.0.0.yaml` declares the top-level edge shape, every closed-world enumeration, the threshold sub-block shape, the `assurance_rescope_ref` requirement, and the mod-111 validation rules.
- [ ] At least twelve `edges/edge-*.yaml` fragments cover drift (2), performance (2), fairness (2), adversarial-robustness (2), LLM-quality (2), plus at least one SOC-sourced edge and at least one evidence- or attestation-freshness edge. Every edge binds all five fields; every taxonomy and SoA reference is closed-world.
- [ ] Every `assurance_trigger` names the ongoing-assurance re-assessment scope it fires per mod-107 chapter 03.
- [ ] Each of the three chapter-02 invariants has a named enforcing design choice and a detection test.
- [ ] `cardinality-and-aggregation.md` shows a plausible traffic profile with reduction math, the aggregation-function rules per cadence bucket, the de-duplication governing-edge rule, the sustain and re-arm hysteresis rule, the quiet-hours-suppression logging discipline, and the cadence-by-field-type table.
- [ ] `sprawl-guard.md` defines the orphan-monitor failure mode, the discovery cadence with a named seat, the machine-enforced registration gate, and the bounded deprecation-or-regularisation path with an SLA.
- [ ] Every unverified citation to EU AI Act Article 72/73, ISO/IEC 42001 clauses, SR 11-7, an observability-vendor product feature, or a scenario-specific regulation is marked `<!-- needs-research: ... -->` — no invented dates, clause numbers, feature names, or agency positions.

## Stretch goals

- Design a **portfolio-level SLA-breach report shape** — a single artefact the architect reads at the monthly operational review that surfaces, across the population of edges, which SLAs are trending toward breach, which have breached, and which responder seats are consistently missing them. Include the specific query patterns against the mod-111 edge inventory.
- Sketch a **migration plan for existing team-scoped contracts** to the new enterprise meta-contract shape — the discovery-plus-classify-plus-regularise-plus-deprecate workflow, the timeline, the seat that owns each step, and the deprecation path for contracts that cannot be regularised.
- Draft a **conformance-testing harness sketch** — the property-based test set that runs against any candidate instance-contract before it is admitted to the mod-111 registry (schema conformance, closed-world enumeration conformance, invariant conformance, SLA-shape conformance). Not the tests themselves; the shape of the harness.
- Design the **interoperability with a third-party frontier-model provider's own signals** — how the meta-contract admits a monitor whose platform-of-origin is a vendor's telemetry surface (e.g., a foundation-model provider's usage-signals API) without breaking the closed-world discipline. Compose with mod-109 third-party governance.
