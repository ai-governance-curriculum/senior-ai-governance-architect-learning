# exercise-05: Integration With Observability And Runtime Security Drill

**Estimated effort:** 4 hours

## Objective

Produce the **sibling-integration artefact set** the level-50 architect commits the GRC-for-AI platform to when the runtime-security vendor (Robust Intelligence, Lakera Guard, Calypso AI, HiddenLayer, Protect AI) and the observability vendor (Fiddler, Arthur, WhyLabs / whylogs, Evidently) categories are already inside the enterprise procurement footprint. The deliverable is a sibling-tool inventory, a runtime-security enterprise contract per tool, a single observability consumption contract for the platform, a normalisation-adapter inventory naming the swap-out point when a vendor changes, and an auditable test plan that turns the sibling framing from an assertion into a repeatable proof. An optional sixth deliverable extends the shape to a Singapore AI Verify integration for one testable-attribute family.

The correctness spine is twelve items you must be able to point at line-by-line: the three chapter-05 invariants (runtime tools are producers not authorities; every runtime tool has a named enterprise contract owner; L25 / L35 / L50 role separation is preserved), the three chapter-05 failure modes (substitute mis-framing; refuse-to-integrate mis-framing; per-team fragmentation), the three chapter-06 invariants (the platform is a consumer never a competing source; the normalised event schema is the contract; the mod-108 evidence contract mediates the control binding), and the three chapter-06 failure modes (parallel drift monitoring inside the mod-111 platform; accepting vendor-native events that bypass normalisation; adopting a vendor dashboard as system of record). The mod-110 chapter 03 normalisation layer is the invariant pipeline both chapters bind through — every producer emits through the per-vendor adapter into the canonical event schema, and neither runtime-security nor observability signals reach any store of authority in the vendor's native shape. Design against that spine; each artefact you write should be pinnable to an invariant it enforces or a failure mode it defends against.

Draft as a senior architect briefing an apprentice — the artefacts are contractual, not narrative. The requirements below name what must be present, not how to phrase it. Solutions are held in a separate paired repository; do not write them here.

## Prerequisites

- Chapter [`05-ai-runtime-security-adjacencies.md`](../05-ai-runtime-security-adjacencies.md) read once, with the YAML runtime-security contract shape, the three invariants, and the three failure modes marked.
- Chapter [`06-ai-observability-adjacencies.md`](../06-ai-observability-adjacencies.md) read once, with the YAML consumption contract shape, the three invariants, and the three failure modes marked.
- Chapter [`07-singapore-ai-verify-integration.md`](../07-singapore-ai-verify-integration.md) skimmed — the optional deliverable references its five-point integration shape.
- Mod-110 chapter `03` (the normalisation layer, canonical event schema, two-store convergence) — the invariant pipeline both siblings bind through.
- Mod-108 (evidence-artefact contract, freshness window, state machine) — the binding all runtime-security and observability signals mediate through before touching a mod-102 control state.
- Mod-102 control library — specifically the `AIC-SEC-*` families the runtime-security tools substantiate, and the `AIC-ROB-*` / `AIC-FAIR-*` families the observability signals and the optional AI Verify integration bind to.
- Primary references: MITRE ATLAS (adversary tactics against ML systems), OWASP Top 10 for LLM Applications (LLM-specific attack surface vocabulary), ISO/IEC 42001 (AIMS documented-information requirements the runtime-tool configuration state is filed under). See [`../resources.md`](../resources.md).

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose sibling-tool footprint you are **least familiar with**; the exercise is calibrated to teach you most where you are weakest. State your choice at the top of `sibling-inventory.md`. Scenario choice constrains the sibling-tool inventory you author against and the coordination overlays each contract must accommodate.

- **A US regional bank** with an established SOC vendor stack, an SR 11-7-aligned MRM programme, and an emerging AI-observability adoption (typically one classical-ML observability platform in production against fraud and credit models; LLM-quality observability at pilot stage against internal RAG and customer-facing chat). Runtime-security adoption is procurement-led with heavy CISO input.
- **A global healthcare payer / provider** with a clinical-safety oversight committee overlaying every AI system, HIPAA scope on the entire portfolio, and a runtime-security posture that inherits from an already-mature medical-device software integrity programme. Observability is fragmented across clinical-decision-support pilots and a separate stack for payer-side utilisation-management AI.
- **A B2B SaaS platform vendor** shipping GenAI-augmented HR-tech into enterprise customers. Customer contracts commonly require a customer-facing incident-liaison seat, per-tenant guardrail configurability, and evidence exports the customer can present to *its* auditor. Runtime-security tools are deployed in-line at the tenant boundary; observability signal is emitted per tenant and rolled up at the platform level.

## Deliverables

Author five artefacts in a working directory of your choice; a sixth is optional.

1. **`sibling-inventory.md`** — the enterprise's sibling-tool inventory across runtime-security and observability categories. Every specific product-capability claim carries `<!-- needs-research: ... -->`.
2. **`runtime-security-contract-per-tool.yaml`** — one entry per runtime-security tool (at least **two**), following the chapter-05 YAML shape.
3. **`observability-consumption-contract.yaml`** — the mod-111-side consumption contract following the chapter-06 YAML shape. **One** contract covering all observability platforms in the inventory.
4. **`normalisation-adapter-inventory.md`** — one entry per per-vendor adapter (one per named tool). Source shape (vendor-native), target shape (canonical event schema per mod-110 chapter 03), enterprise owner. Adapters are the swap-out point when vendors change.
5. **`substitute-vs-sibling-tests.md`** — auditable test plan proving: (a) no vendor console is cited as authority in any register write; (b) every register write on runtime-security signal traces to a normalised event, not a vendor-native shape; (c) the mod-111 platform is not computing its own observability signal; (d) every runtime-security tool has a named enterprise contract owner.
6. **OPTIONAL** — **`ai-verify-integration-plan.md`** — the chapter-07 five-point integration shape applied to at least one AI Verify testable-attribute family (fairness / robustness / explainability). Stretch — not mandatory for a pass.

## Requirements

### `sibling-inventory.md`

At least **two runtime-security tools** and at least **two observability tools** relevant to the chosen scenario. Each entry carries:

- Tool name (vendor at category level; specifics such as product-name, marketed capability, integration surface all carry `<!-- needs-research: ... -->`).
- Enforcement scope (runtime-security) or emission scope (observability) at category level.
- Integration status in the scenario (in-production / at-pilot / at-procurement / not-adopted).
- Level-25 team-scope owner (`ai-risk-engineer` seat naming convention).
- Level-35 platform-scope owner (`ai-infra-security` for runtime-security; `ai-evaluation-engineer` or platform-observability seat for observability).
- Level-50 enterprise-contract owner (`senior-ai-governance-architect`).

State a per-scenario overlay note: SOC-vendor coupling for the bank; clinical-safety-committee overlay for healthcare; customer-facing incident-liaison and per-tenant configurability for the B2B SaaS scenario. Name explicitly out-of-scope: SOC-side detection engineering (mod-110 chapter 05) and per-team monitor-to-register edges (mod-110 chapter 02).

### `runtime-security-contract-per-tool.yaml`

One block per runtime-security tool, following the chapter-05 YAML shape. Required fields per block:

- `tool_id`, `vendor`, `product`, `category` (one of `inference-time-enforcement` or `build-time-scanning`).
- `enterprise_contract_owner`, `operational_owner`, `team_engineer_seat`.
- `aims_documented_information_ref` — the AIMS documented-information identifier the tool's configuration state is filed under (mod-105).
- `enforcement_scope` — the runtime enforcement points the tool sits at.
- `signal_types_emitted` — a list, each entry carrying `kind`, `severity`, `lake_tier`, `register_contract_edge`, `mod102_controls_substantiated`, `mod106_categories_scored`, and where applicable `pms_to_soc_handoff` or `triggers_workflow` or `appetite_alarm_on_sustained_outage` or `aims_documented_information_update`.
- `evidence_contract` — `mod108_evidence_artefact_shape`, `freshness_window`, `substantiation_scope`.
- `integration_shape` — `normalisation_adapter_ref` (points into the adapter inventory below), `canonical_event_schema` (mod-110 ch-03), `two_store_write: [ enterprise-data-lake, grc-for-ai-register ]`.
- `role_coordination` — three sub-blocks naming the L25 / L35 / L50 responsibilities specifically for this tool.
- `deprecation_policy` — trigger and workflow when the tool is replaced.

### `observability-consumption-contract.yaml`

**One** contract, following the chapter-06 YAML shape. Required fields:

- `id`, `version_ratified_by` (list including `senior-ai-governance-architect`, `head-of-ai-governance`, and the enterprise data-platform lead).
- `upstream_schema_ref` — the mod-110 ch-03 normalised event schema identifier at a specific version.
- `accepts_event_kinds` — the exhaustive list, each entry carrying `kind` (e.g. `drift-metric`, `performance-metric`, `fairness-metric`, `data-quality-metric`, `llm-quality-metric`), `updates_evidence_artefact` (mod-108 artefact class), and `register_write_field_ref` (mod-110 ch-02 address).
- `raw_telemetry_reference` — `holds_locally: false`, `references_by_pointer_to: enterprise-data-lake`.
- `dashboard_read_path` — `source: this-platform-register-tier`, `permitted_secondary_sources: none`.
- `refuses` — a list naming `platform-native-event-shapes`, `vendor-console-as-source-of-truth`, and `locally-computed-observability-signal`.

### `normalisation-adapter-inventory.md`

One entry per per-vendor adapter (one adapter per named runtime-security tool and per named observability tool). Each entry:

- Adapter identifier and current version.
- Source vendor shape — the fields the adapter reads from the vendor's native emission (naming the vendor field vocabulary at category level; specifics carry `<!-- needs-research: ... -->`).
- Target canonical shape — the mod-110 ch-03 canonical event schema at its ratified version, and the specific `signal.kind` values the adapter emits.
- Enterprise adapter owner (a level-35 seat) and enterprise contract owner (level 50).
- Deprecation criteria — the condition under which a vendor version bump requires a new adapter version, and the workflow that ratifies the swap.

### `substitute-vs-sibling-tests.md`

Each test carries: name, precondition, action, expected outcome, and evidence capture. The test set must cover all six failure modes:

- Chapter 05 (a) — substitute mis-framing — test that no register write, no attestation, and no evidence-artefact citation names the vendor console as source.
- Chapter 05 (b) — refuse-to-integrate mis-framing — test that the runtime-security signal stream is landing in the lake and rolling into the register (a stale register on the highest-signal category is the visible symptom).
- Chapter 05 (c) — per-team fragmentation — test that every runtime-security tool present in any team's stack maps to an entry in `runtime-security-contract-per-tool.yaml` with a named `enterprise_contract_owner`.
- Chapter 06 (a) — parallel drift monitoring — test that the mod-111 platform is not computing its own drift, performance, fairness, data-quality, or LLM-quality signal (the `refuses.locally-computed-observability-signal` line is what the test verifies).
- Chapter 06 (b) — accepting vendor-native events — test at the platform's ingestion boundary that a vendor-native payload is refused and referred back to the normalisation-layer schema-evolution process.
- Chapter 06 (c) — vendor dashboard as system of record — test that no executive dashboard, no report export, and no API response cites the vendor console as source; the mod-111 register is the sole source of truth.

Cross-cutting requirements applying to every deliverable:

- The L25 / L35 / L50 three-vertex triangle must appear in every contract with **specific** responsibilities named for that tool or platform — not generic role descriptions.
- Every event kind in every contract must bind to specific mod-102 control identifiers (e.g. `AIC-SEC-PROMPT-INJECTION-GUARDRAIL`, `AIC-FAIR-004`) and mod-106 category identifiers.
- Explicit non-scope: SOC-side detection engineering (owned by `ai-infra-security` at level 35, referred to mod-110 chapter 05); per-team monitor-to-register edges (a mod-110 chapter 02 concern). Both are named as out-of-scope in `sibling-inventory.md`.

### OPTIONAL `ai-verify-integration-plan.md`

If attempted, apply the chapter-07 five-point integration shape (workflow trigger; run execution with reproducibility inputs; framework-native output; evidence-index write via schema-mapping adapter; control-attestation reference) to at least one AI Verify testable-attribute family. The plan cites `AIC-FAIR-*`, `AIC-ROB-*`, or the explainability control family it substantiates and shows how the resulting mod-108 evidence artefact composes with the observability signal already covered by the consumption contract without creating an unrationalised competing test (chapter-07 failure mode c).

## Starter guidance

Author `sibling-inventory.md` first. The contracts depend on the inventory — you cannot commit an enterprise contract for a tool that is not on the inventory, and you cannot write the consumption contract's `accepts_event_kinds` block coherently until you know which observability platforms it is accommodating. The inventory is short but load-bearing.

Do **not** let a vendor's native field shape enter any register write. Chapter 06 failure mode (b) is the fastest way to lock the enterprise into a vendor by mistake, and it happens most often when a well-supported vendor integration ships a plug-and-play push into the register-tier that seems to save effort. The normalisation adapter is not a nice-to-have; it is the invariant. If the adapter is missing, the contract is not enforceable.

The L25 / L35 / L50 triangle is where per-team fragmentation is prevented. If any vertex in a contract is unassigned or vaguely named ("the platform team owns this"), failure mode 05(c) is guaranteed within a quarter of standing the tool up. Name the seat, name the specific responsibility for this tool.

A runtime-security tool without a named enterprise contract owner is not a governance oversight; it is a level-50-invariant violation. Chapter 05 invariant 2 is not aspirational. The contract owner is the accountable seat for the tool's integration into the enterprise stores, the AIMS documented information state, the mod-108 evidence-contract binding, and the swap-out when the vendor changes. If no one owns it, the invariant is broken and the runtime tool is running as a shadow governance surface.

The substitute-vs-sibling tests are what make the sibling framing *enforceable*. Writing the contracts without the tests leaves the invariants aspirational. The tests should be executable by an internal auditor in a working session — not philosophical assertions. If a test reads "verify that governance is aligned with the runtime tool", rewrite it.

The optional AI Verify plan is a stretch: attempt it only after the five mandatory artefacts hold together. It is the composition drill that shows the reference architecture accommodates a third sibling class (open-source assurance testing) without the two-store convergence or the mod-108 mediation invariants having to bend.

## Acceptance criteria

- [ ] Chosen scenario is stated at the top of `sibling-inventory.md` and every artefact is coherent against it.
- [ ] `sibling-inventory.md` names at least two runtime-security tools and at least two observability tools with the six required fields per entry and a scenario overlay note.
- [ ] `runtime-security-contract-per-tool.yaml` contains at least two blocks, each with all chapter-05 YAML fields present (tool_id through deprecation_policy).
- [ ] `observability-consumption-contract.yaml` is a single contract carrying the chapter-06 YAML shape end-to-end, including the `refuses` block naming all three chapter-06 failure modes explicitly.
- [ ] `normalisation-adapter-inventory.md` names one adapter per tool with source shape, target canonical shape, enterprise owner, and deprecation criteria.
- [ ] `substitute-vs-sibling-tests.md` covers all six failure modes (three chapter-05, three chapter-06) with an executable test each — name, precondition, action, expected outcome, evidence capture.
- [ ] The L25 / L35 / L50 three-vertex triangle is named in every runtime-security contract block and in the observability consumption contract with specific per-tool or per-platform responsibilities.
- [ ] Every event kind in every contract binds to specific mod-102 control identifiers and mod-106 category identifiers.
- [ ] SOC-side detection engineering and per-team monitor-to-register edges are explicitly named as out-of-scope in `sibling-inventory.md` with references to mod-110 chapters 05 and 02 respectively.
- [ ] Every specific vendor product-capability claim carries `<!-- needs-research: ... -->`. No invented product names, marketed features, or integration surfaces.
- [ ] If attempted, `ai-verify-integration-plan.md` follows the chapter-07 five-point shape for at least one testable-attribute family and names its rationalisation against the observability signal on the same control family.

## Stretch goals

- **Second AI Verify family.** Extend the optional integration plan to a second testable-attribute family (fairness *and* robustness, or robustness *and* explainability) and show the rationalised evidence set on any control family the two families both touch — primary evidence source per attestation, corroborating sources, deprecated tests explicitly listed.
- **Vendor-swap runbook.** Author the runbook for replacing one named runtime-security tool with another under the same enterprise contract. The runbook exercises the adapter swap-out invariant — the contract stays, the `normalisation_adapter_ref` changes, the register-side attestation history is preserved in the lake, no re-attestation is required for the historical window — and lands the mod-105 AIMS documented-information update and the mod-108 evidence-contract freshness re-run at the correct steps.
- **Joint SOC + governance runbook.** Show a single inbound runtime-security signal traversing both the mod-110 chapter 05 SOC handoff and the mod-111 register write in one workflow, without duplication and without either side claiming primacy over the other. The runbook should demonstrate the L35 SOC seat and the L50 governance seat coordinating through the normalisation layer as the shared substrate, not through direct point-to-point handoffs that re-introduce the failure-mode 05(b) pattern.
