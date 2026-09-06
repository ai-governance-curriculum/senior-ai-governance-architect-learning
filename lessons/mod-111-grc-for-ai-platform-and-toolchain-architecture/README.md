# mod-111-grc-for-ai-platform-and-toolchain-architecture: GRC-for-AI Platform and Toolchain Reference Architecture

This is the level-50 architect's module for the enterprise platform layer that hosts the AI-governance system of record — the target-state architecture where the mod-102 control library, mod-105 AIMS documented information, mod-106 risk register, mod-107 assurance artefacts, mod-108 evidence index, mod-109 third-party inventory, and mod-110 PMS register-tier state all live and are queried together. It delivers the GRC-for-AI reference architecture composed with (not replacing) the enterprise GRC platform, the nine-dimension vendor evaluation matrix, the seven-flow workflow layer, the RBAC + segregation-of-duties model that discharges SR 11-7 independence and ISO/IEC 42001 competence together, the runtime-security sibling positioning (Robust Intelligence, Lakera Guard, Calypso AI, HiddenLayer, Protect AI), the observability sibling positioning (Fiddler AI, Arthur AI, WhyLabs / whylogs, Evidently AI), and the Singapore AI Verify integration as the reference open-source assurance testing framework. Downstream modules mod-112 (program design) and mod-113 (sector blueprints) bind to the artefacts this module authors.

**Estimated effort:** 16 hours

## Learning objectives

- Author the GRC-for-AI reference architecture — how the AI governance system of record sits inside the enterprise stack, integrations with ML platforms, security tooling, enterprise GRC (Archer, ServiceNow GRC, MetricStream), identity, communications, ticketing — so a build-vs-buy conversation has a concrete target-state
- Evaluate the GRC-for-AI vendor landscape — Credo AI, Holistic AI, ModelOp, Monitaur, ServiceNow AI Control Tower, IBM watsonx.governance, Fairly AI, Enzai, Trustible, Verify AI, Cranium, HydroX AI — against the enterprise reference architecture; author the vendor-evaluation matrix (workflow coverage, RBAC, integration surface, evidence-schema flexibility, jurisdiction coverage, audit-body track record, price)
- Position AI-runtime security adjacencies — Robust Intelligence, Lakera Guard, Calypso AI, HiddenLayer, Protect AI — as sibling platforms the GRC-for-AI system of record integrates with, not competes with; coordinate with `ai-risk-engineer` (level 25, guardrail engineering) and `ai-infra-security` (level 35, platform-scale defence)
- Position AI observability adjacencies — Fiddler AI, Arthur AI, WhyLabs / whylogs, Evidently AI — as sibling platforms the GRC-for-AI system of record consumes signal from
- Design the workflow layer — intake, impact-assessment, control-testing, evidence-collection, exception-handling, incident-routing, audit-facing packaging — as first-class flows the platform must support
- Design the RBAC + segregation-of-duties model — analyst / engineer / architect / auditor / executive personas — that satisfies SR 11-7 validation-vs-development independence and ISO/IEC 42001 competence requirements
- Position Singapore AI Verify as the reference open-source assurance testing framework the reference architecture may integrate for testable-attribute reporting

## Chapters

- [`01-grc-for-ai-reference-architecture-and-enterprise-integration.md`](./01-grc-for-ai-reference-architecture-and-enterprise-integration.md) — the target-state reference architecture; the GRC-for-AI platform as system of record, composed with the enterprise GRC (Archer / ServiceNow GRC / MetricStream / OneTrust / LogicGate) rather than replacing it; the two failure modes (isolated-island; parallel-platform duplication); six invariants; the integration edges every enterprise deployment must terminate on.
- [`02-grc-for-ai-vendor-evaluation-matrix.md`](./02-grc-for-ai-vendor-evaluation-matrix.md) — the nine-dimension evaluation matrix (workflow coverage, RBAC + SoD flexibility, integration surface, evidence-schema flexibility, jurisdiction coverage, audit-body track record, deployment model, roadmap velocity, TCO) authored BEFORE any vendor demo; the POC-vs-demo boundary; the build-vs-buy decision instrument.
- [`03-workflow-layer-intake-through-audit-packaging.md`](./03-workflow-layer-intake-through-audit-packaging.md) — the seven first-class flows (intake, impact-assessment, control-testing, evidence-collection, exception-handling, incident-routing, audit-facing packaging); the workflow-as-first-class-object discipline (versioned, ratified, deprecatable).
- [`04-rbac-and-segregation-of-duties-model.md`](./04-rbac-and-segregation-of-duties-model.md) — capability bundles, personas, seats, scopes; the SoD conflict matrix enforced at both assignment time and action time; break-glass discipline; discharges SR 11-7 validation-independence and ISO/IEC 42001 Clause 7.2 competence simultaneously.
- [`05-ai-runtime-security-adjacencies.md`](./05-ai-runtime-security-adjacencies.md) — Robust Intelligence, Lakera Guard, Calypso AI, HiddenLayer, Protect AI as SIBLINGS (not competitors) to the system of record; the L25/L35/L50 coordination triangle; enterprise-contract shape per tool.
- [`06-ai-observability-adjacencies.md`](./06-ai-observability-adjacencies.md) — Fiddler AI, Arthur AI, WhyLabs / whylogs, Evidently AI as sibling signal-sources; the mod-111-side consumption contract; three invariants (consumer never competing source; normalised event schema is the contract; mod-108 evidence contract mediates the control binding).
- [`07-singapore-ai-verify-integration.md`](./07-singapore-ai-verify-integration.md) — Singapore AI Verify as the reference open-source assurance testing framework; the five-point integration shape; the testing-toolkit-not-policy distinction.

## Exercises

- [`exercises/exercise-01-grc-for-ai-reference-architecture-drawing.md`](./exercises/exercise-01-grc-for-ai-reference-architecture-drawing.md) — author the enterprise reference-architecture schematic, integration edge catalog, build-vs-buy scoping note, and invariant-enforcement table for a chosen scenario.
- [`exercises/exercise-02-vendor-evaluation-matrix-drill.md`](./exercises/exercise-02-vendor-evaluation-matrix-drill.md) — author the matrix, score three candidates against the nine dimensions, produce the POC plan, build-vs-buy recommendation, and five-year TCO model.
- [`exercises/exercise-03-workflow-layer-shape-drill.md`](./exercises/exercise-03-workflow-layer-shape-drill.md) — author the seven-flow catalog, state-machine diagrams, intake-and-impact-assessment runbook, exception-handling policy, and audit-facing-bundle spec.
- [`exercises/exercise-04-rbac-plus-sod-model-design.md`](./exercises/exercise-04-rbac-plus-sod-model-design.md) — author the RBAC model YAML, SoD conflict matrix, competence-profile catalog, break-glass runbook, and SoD enforcement test plan.
- [`exercises/exercise-05-integration-with-observability-and-runtime-security-drill.md`](./exercises/exercise-05-integration-with-observability-and-runtime-security-drill.md) — author the sibling-tool inventory, runtime-security enterprise contracts, observability consumption contract, normalisation-adapter inventory, and substitute-vs-sibling test plan.

## Structure

- `01-…md` … `07-…md`: lecture chapters.
- `exercises/`: per-exercise prompts. Solutions live in the paired `-solutions` repo.
- `labs/`: long-form hands-on labs (scaffolded; content lands on a subsequent cycle).
- `quizzes/`: knowledge checks (scaffolded; content lands on a subsequent cycle).
- `resources.md`: external references.
