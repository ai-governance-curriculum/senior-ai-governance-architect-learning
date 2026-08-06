# mod-105-aims-and-ai-management-system-architecture: ISO/IEC 42001 AIMS Architecture and Integration

**Estimated effort:** 18 hours

## Why this module exists

An AI Management System (AIMS) built to ISO/IEC 42001:2023 is not a compliance checklist and not a folder of policies. It is a *management system* — a set of interlocking artefacts (scope statement, Statement of Applicability, risk-and-impact-assessment process, risk-treatment plan, competence and communications plan, internal audit programme, management-review cadence, non-conformity and corrective-action process) whose composition is what a third-party certification body auditing under ISO/IEC 42006 comes to test. The level-50 architect designs the shape of that system so that it *(a)* discharges the enterprise's obligations under the standard, *(b)* integrates cleanly with the sibling ISO/IEC 27001 ISMS the enterprise almost certainly already runs, *(c)* composes with the AI impact-assessment process under ISO/IEC 42005 and the AI risk-management guidance in ISO/IEC 23894 (both of which sit on ISO 31000 as parent), and *(d)* is *auditable* — a certification body can arrive on day one and find the evidence they need to sample against.

The head-of-AI-governance (level 60) *operates* this system day to day; the architect *designs* it. The two roles collaborate at fixed interfaces described in mod-101 chapter 06 and reiterated across this module. This is not a role in which the architect writes the meeting minutes of the management-review forum. It is a role in which the architect designs the forum's charter, the inputs it demands, the outputs it must produce, and the interfaces to internal audit, to Annex A control ownership, and to the risk-treatment plan.

## Learning objectives

- Design a certifiable ISO/IEC 42001 AI Management System — scope statement, Statement of Applicability (SoA) mapping each Annex A control to inclusion/exclusion + justification, risk-treatment plan, internal audit programme, management-review cadence, competence + communications plan, non-conformity + corrective-action process.
- Integrate the AIMS with an existing ISO/IEC 27001 ISMS — shared documentation, shared audit programme, shared management review — so the enterprise runs one integrated management system rather than two disconnected ones.
- Integrate ISO/IEC 42005 (AI system impact assessment) into the AIMS — when an AIA fires, who authors it, who reviews it, how the outputs feed the risk-treatment plan.
- Read ISO/IEC 42006 (audit / certification body requirements) and design an AIMS that a third-party certification body can audit — evidence completeness, sampling methodology, auditor competence expectations.
- Position ISO/IEC 23053 (framework for AI systems using ML) as the reference architecture the AIMS scope is written against; position ISO 31000 as the parent risk-management framework the AIMS risk process composes with.
- Design the AIMS operations calendar — internal audit rounds, management reviews, risk-treatment plan refreshes, competence assessments, communications updates — and locate the operations owner (typically `head-of-ai-governance` at level 60, informed by architect at level 50).

## Chapters

1. [`01-the-aims-as-architectural-artefact.md`](01-the-aims-as-architectural-artefact.md) — Why the AIMS is an architectural artefact, the six-part shape it always takes under Annex SL, and how it composes with the sibling standards (27001, 42005, 42006, 23053, 23894, 31000, 38507).
2. [`02-scope-context-and-the-23053-reference-architecture.md`](02-scope-context-and-the-23053-reference-architecture.md) — Clause 4. Context of the organisation, interested parties, the scope statement as the anchor artefact, and ISO/IEC 23053 as the ML-system reference architecture the scope is written against.
3. [`03-leadership-policy-and-planning-clauses-5-and-6-1.md`](03-leadership-policy-and-planning-clauses-5-and-6-1.md) — Clauses 5 and 6.1. Top-management commitment, the AI policy, and the shape of the risk-and-opportunity planning process.
4. [`04-risk-and-impact-assessment-composition.md`](04-risk-and-impact-assessment-composition.md) — Composing ISO 31000 (parent framework) with ISO/IEC 23894 (AI risk guidance) with ISO/IEC 42005 (AI impact assessment) with 42001 Clause 6.1 (risk assessment). Who authors, who reviews, how outputs feed the risk-treatment plan.
5. [`05-statement-of-applicability-and-annex-a.md`](05-statement-of-applicability-and-annex-a.md) — The SoA as the certifiable artefact; the Annex A control walk; the inclusion / exclusion / justification discipline; the SoA-to-control-library binding.
6. [`06-risk-treatment-plan-and-operational-clauses.md`](06-risk-treatment-plan-and-operational-clauses.md) — Clauses 6.1.3 and 8. The risk-treatment plan artefact; residual-risk acceptance; operational planning and control; change-management.
7. [`07-support-competence-awareness-communications-and-documented-information.md`](07-support-competence-awareness-communications-and-documented-information.md) — Clause 7. Resources, competence, awareness, communications, documented information. The plans the architect writes and hands to the head of AI governance to run.
8. [`08-performance-evaluation-internal-audit-and-management-review.md`](08-performance-evaluation-internal-audit-and-management-review.md) — Clause 9. Monitoring, measurement, analysis, evaluation; the internal audit programme; the management review. This is where the AIMS operations calendar lives.
9. [`09-non-conformity-corrective-action-and-continual-improvement.md`](09-non-conformity-corrective-action-and-continual-improvement.md) — Clause 10. The non-conformity and corrective-action process, the CAPA record, and the continual-improvement flow into risk treatment.
10. [`10-integrating-the-aims-with-the-iso-27001-isms.md`](10-integrating-the-aims-with-the-iso-27001-isms.md) — Shared documentation, shared audit programme, shared management review. How to run one integrated management system rather than two disconnected ones without collapsing the AI-specific clauses into ISMS boilerplate.
11. [`11-designing-for-third-party-audit-iso-42006.md`](11-designing-for-third-party-audit-iso-42006.md) — ISO/IEC 42006 read as the auditor's rulebook. Evidence completeness, sampling methodology, auditor competence expectations, the shape of the stage-1 and stage-2 audits, and the pre-certification readiness review.

## Exercises

- [`exercises/exercise-01-aims-scope-statement-drill.md`](exercises/exercise-01-aims-scope-statement-drill.md) — draft a defensible AIMS scope statement for a specified enterprise.
- [`exercises/exercise-02-statement-of-applicability-authoring.md`](exercises/exercise-02-statement-of-applicability-authoring.md) — author a full SoA against ISO/IEC 42001 Annex A with inclusion / exclusion / justification per control.
- [`exercises/exercise-03-risk-treatment-plan-shape-drill.md`](exercises/exercise-03-risk-treatment-plan-shape-drill.md) — produce a risk-treatment plan that composes ISO 31000, 23894, 42005, and 42001 Clause 6 without collapsing them.
- [`exercises/exercise-04-aims-plus-isms-integration-design.md`](exercises/exercise-04-aims-plus-isms-integration-design.md) — design the integrated AIMS+ISMS documentation, audit programme, and management review.
- [`exercises/exercise-05-iso-42005-integration-into-aims.md`](exercises/exercise-05-iso-42005-integration-into-aims.md) — wire ISO/IEC 42005 AI impact assessment into the AIMS as a first-class input to the risk-treatment plan.
- [`exercises/exercise-06-iso-42006-audit-body-readiness-drill.md`](exercises/exercise-06-iso-42006-audit-body-readiness-drill.md) — walk the AIMS through a stage-1 readiness review from an ISO/IEC 42006-conformant certification body's perspective.

## Structure

- `01-…md` … `11-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs (planned).
- `quizzes/`: knowledge checks (planned).
- `resources.md`: external references.
