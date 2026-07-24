# mod-102 — Enterprise AI Control Library Architecture

**Estimated effort:** 20 hours

This module designs the enterprise AI control library — the machine-readable catalog of control entries the rest of the program dereferences into. It builds the anatomy of a single entry (chapter 01), then composes the governance-family sources (chapter 02), the threat-family sources (chapter 03), serialises the result in OSCAL (chapter 04), layers inheritance and compensating controls (chapter 05), governs the authoring lifecycle (chapter 06), hardens the evidence contract (chapter 07), and delegates the engineering-side entries to `ai-risk-engineer` at level 25 (chapter 08). Every downstream module in the track (policy taxonomy in mod-103, jurisdictional reconciliation in mod-104, AIMS in mod-105, risk taxonomy in mod-106, assurance in mod-107, evidence architecture in mod-108, third-party in mod-109, post-market surveillance in mod-110, GRC-for-AI in mod-111, operating model in mod-112, sector blueprints in mod-113) hangs off the catalog authored here.

## Learning objectives

- Design an AI control library — control statements, applicability filters (system tier / use case / jurisdiction / risk-appetite band), implementation guidance, testing procedures, evidence contracts — that composes NIST AI RMF sub-categories, ISO/IEC 42001 Annex A controls, EU AI Act Articles 9-15 obligations, OWASP LLM Top 10 + MITRE ATLAS + Google SAIF + CISA/NCSC Secure AI System Development + ENISA multilayer good-practice into a single catalog.
- Represent the control library in a machine-readable form — OSCAL catalog + baseline profiles, plus a companion evidence-schema registry — so downstream automation (`ai-risk-engineer` at level 25, `ai-infra-mlops` at level 25, the GRC-for-AI platform at mod-111) can consume it programmatically.
- Design the control-inheritance model — how a group-level or platform-provided control satisfies a business-unit control, how a shared control is claimed, how compensating controls are documented — so audit-time traceability is defensible.
- Design the control-authoring lifecycle — proposal → architect review → adoption → modification → deprecation → sunset — with change-log discipline, semver-ish versioning, and back-compat rules that let downstream artefacts pin against a release.
- Design the control-testing evidence contract — what evidence must be produced per control, at what cadence, by which owner, in what format, at what retention, with what immutability — so `ai-governance-analyst` (level 15) can collect and `ai-evaluation-engineer` (level 35) can review without ambiguity.
- Author the delegation contract to `ai-risk-engineer` (level 25) — the engineering-side controls (harm-model authoring, red-team, adversarial-ML, fairness, privacy, guardrail) are co-authored through a structured three-round negotiation; the architect signs off on the catalog design, not on the mechanism-specific tuning.

## Chapters

1. [`01-anatomy-of-a-control-library-entry.md`](01-anatomy-of-a-control-library-entry.md) — the seven fields every AI-library entry carries and why each earns its place.
2. [`02-composing-nist-rmf-iso-42001-eu-ai-act.md`](02-composing-nist-rmf-iso-42001-eu-ai-act.md) — one entry per outcome, multi-parented across the three governance-family sources.
3. [`03-composing-owasp-mitre-saif-cisa-enisa.md`](03-composing-owasp-mitre-saif-cisa-enisa.md) — the same composition move for the five threat-family sources, plus coverage-map as a first-class deliverable.
4. [`04-oscal-representation.md`](04-oscal-representation.md) — serialising the catalog and baseline profiles in OSCAL, cross-catalog composition with SP 800-53 and CSF 2.0, and evidence-schema references outside OSCAL.
5. [`05-control-inheritance-and-compensating-controls.md`](05-control-inheritance-and-compensating-controls.md) — the three legitimate inheritance patterns, the four inheritance-eligibility tests, the compensating-control shape, and the boundary with waivers.
6. [`06-control-authoring-lifecycle.md`](06-control-authoring-lifecycle.md) — the five statuses, the release cadence, the change-classification rules, and the release-notes contract that make the catalog pinnable.
7. [`07-evidence-contract-per-control.md`](07-evidence-contract-per-control.md) — the six base sub-fields plus three maturity sub-fields, the SSP-level evidence binding, and the architect's review discipline on this field.
8. [`08-delegation-to-ai-risk-engineer.md`](08-delegation-to-ai-risk-engineer.md) — which controls are engineering-side, the three-round negotiation, and the two shared artefacts that keep the delegation honest.

## Exercises

- [`exercises/exercise-01-control-catalog-shape-proposal.md`](exercises/exercise-01-control-catalog-shape-proposal.md) — propose the shape of the enterprise catalog.
- [`exercises/exercise-02-nist-ai-rmf-to-iso-42001-annex-a-composition.md`](exercises/exercise-02-nist-ai-rmf-to-iso-42001-annex-a-composition.md) — compose NIST AI RMF sub-categories with ISO/IEC 42001 Annex A controls into single entries.
- [`exercises/exercise-03-oscal-catalog-plus-profile-drill.md`](exercises/exercise-03-oscal-catalog-plus-profile-drill.md) — serialise a slice of the catalog + one baseline profile in OSCAL.
- [`exercises/exercise-04-control-inheritance-and-compensating-controls-model.md`](exercises/exercise-04-control-inheritance-and-compensating-controls-model.md) — draft the inheritance registry and one compensating-control worked example.
- [`exercises/exercise-05-control-authoring-lifecycle-flow.md`](exercises/exercise-05-control-authoring-lifecycle-flow.md) — walk a proposal through the five stages.
- [`exercises/exercise-06-evidence-contract-per-control-drill.md`](exercises/exercise-06-evidence-contract-per-control-drill.md) — harden the evidence contract on three controls of different families.

## Other assets

- [`labs/`](labs/) — long-form hands-on labs (planned).
- [`quizzes/`](quizzes/) — knowledge checks (planned).
- [`resources.md`](resources.md) — curated primary references.
