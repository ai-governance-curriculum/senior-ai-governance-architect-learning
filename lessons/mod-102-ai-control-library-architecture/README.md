# mod-102-ai-control-library-architecture: Enterprise AI Control Library Architecture

> Scaffolded by `aicg org execute-plan`. Lecture chapters and exercise content are authored on subsequent autonomous cycles.

**Estimated effort:** 20 hours

## Learning objectives

- Design an AI control library — control statements, applicability filters (system tier / use case / jurisdiction / risk-appetite band), implementation guidance, testing procedures, evidence contracts — that composes NIST AI RMF sub-categories, ISO/IEC 42001 Annex A controls, EU AI Act Articles 9-15 obligations, OWASP LLM Top 10 + MITRE ATLAS + Google SAIF + CISA/NCSC Secure AI System Development + ENISA multilayer good-practice into a single catalog
- Represent the control library in a machine-readable form — OSCAL catalog + profile, or an equivalent internal schema — so downstream automation (`ai-risk-engineer` at level 25, `ai-infra-mlops` at level 25) can consume it programmatically
- Design the control-inheritance model — how a group-level control satisfies a business-unit control, how a shared control is claimed, how compensating controls are documented — so audit-time traceability is defensible
- Design the control-authoring lifecycle — proposal → review → approval → publication → deprecation → sunset — with change-log discipline and back-compat rules
- Design the control-testing evidence contract — what evidence must be produced per control, at what cadence, by which owner, in what format — so `ai-governance-analyst` (level 15) can collect and `ai-evaluation-engineer` (level 35) can review without ambiguity
- Author the delegation contract to `ai-risk-engineer` (level 25) — the engineering-side controls (harm-model authoring, red-team, adversarial-ML, fairness, privacy, guardrail) hand off to that role for implementation; the architect signs off on the catalog design, not the implementation

## Structure

- `01-…md` … `0N-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs.
- `quizzes/`: knowledge checks.
- `resources.md`: external references.
