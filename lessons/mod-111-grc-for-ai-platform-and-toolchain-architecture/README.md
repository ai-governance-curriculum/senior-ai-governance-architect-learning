# mod-111-grc-for-ai-platform-and-toolchain-architecture: GRC-for-AI Platform and Toolchain Reference Architecture

> Scaffolded by `aicg org execute-plan`. Lecture chapters and exercise content are authored on subsequent autonomous cycles.

**Estimated effort:** 16 hours

## Learning objectives

- Author the GRC-for-AI reference architecture — how the AI governance system of record sits inside the enterprise stack, integrations with ML platforms, security tooling, enterprise GRC (Archer, ServiceNow GRC, MetricStream), identity, communications, ticketing — so a build-vs-buy conversation has a concrete target-state
- Evaluate the GRC-for-AI vendor landscape — Credo AI, Holistic AI, ModelOp, Monitaur, ServiceNow AI Control Tower, IBM watsonx.governance, Fairly AI, Enzai, Trustible, Verify AI, Cranium, HydroX AI — against the enterprise reference architecture; author the vendor-evaluation matrix (workflow coverage, RBAC, integration surface, evidence-schema flexibility, jurisdiction coverage, audit-body track record, price)
- Position AI-runtime security adjacencies — Robust Intelligence, Lakera Guard, Calypso AI, HiddenLayer, Protect AI — as sibling platforms the GRC-for-AI system of record integrates with, not competes with; coordinate with `ai-risk-engineer` (level 25, guardrail engineering) and `ai-infra-security` (level 35, platform-scale defence)
- Position AI observability adjacencies — Fiddler AI, Arthur AI, WhyLabs / whylogs, Evidently AI — as sibling platforms the GRC-for-AI system of record consumes signal from
- Design the workflow layer — intake, impact-assessment, control-testing, evidence-collection, exception-handling, incident-routing, audit-facing packaging — as first-class flows the platform must support
- Design the RBAC + segregation-of-duties model — analyst / engineer / architect / auditor / executive personas — that satisfies SR 11-7 validation-vs-development independence and ISO/IEC 42001 competence requirements
- Position Singapore AI Verify as the reference open-source assurance testing framework the reference architecture may integrate for testable-attribute reporting

## Structure

- `01-…md` … `0N-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs.
- `quizzes/`: knowledge checks.
- `resources.md`: external references.
