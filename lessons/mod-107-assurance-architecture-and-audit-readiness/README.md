# mod-107-assurance-architecture-and-audit-readiness: AI Assurance Architecture and Audit Readiness

**Estimated effort:** 18 hours

## Why this module exists

An enterprise AI programme without an *assurance architecture* collapses into two failure modes with predictable speed. In the first, everyone thinks they own assurance and every layer inspects the layer above until no independent view is ever taken. In the second, no one owns assurance and every layer defers to the next until the certification body arrives and finds nothing was actually challenged. Both failures are architectural, not operational — and both are the level-50 architect's to design against.

This module designs the architecture. The three lines of defence specialised for AI (chapter 01) fix the shape. The pre-deployment assurance gate (chapter 02) is the second-line-authoritative launch decision. The ongoing assurance programme (chapter 03) keeps that decision fresh through periodic, drift-driven, incident-driven, and regulatory-change re-assessments. The third-line internal audit programme (chapter 04) is what the audit committee sees. The external audit and certification interface (chapter 05) is what the certification body, the sector regulator, the ForHumanity independent auditor, the statutory financial auditor, and the specialist bias auditor see. The coordination contracts with the peer-level evaluation engineer and the analyst (chapter 06) are what makes it all execute. NIST SP 800-37 RMF (chapter 07) is the reference process shape the whole system composes with.

The architect *does not run* the internal audit function (that reports to the audit committee), *does not execute* release-assurance methodology (the peer-level `ai-evaluation-engineer` at level 35 does), *does not sign* the ISO 42001 certification body's contract (the AI-accountable executive with legal review does), and *does not collect the day-to-day evidence* (the `ai-governance-analyst` at level 15 does). The architect designs the *shape* each of them operates against — the gate charter, the programme charter, the audit plan template, the interface contract, the coordination contracts — and ratifies changes through the invariants and change-control discipline chapter 01 pins.

## Learning objectives

- Design the enterprise AI assurance architecture — three lines of defence for AI, pre-deployment assurance, ongoing assurance, third-line audit — aligned to the IIA Three Lines Model, SR 11-7 validation-vs-development independence, and ISO/IEC 42006 audit-body requirements.
- Design the pre-deployment assurance gate — what evidence must exist, what reviews must pass, what stakeholders must sign, what escalation paths open — coordinating with `ai-evaluation-engineer` (peer, level 35) on the release-assurance methodology that runs inside the gate.
- Design the ongoing assurance programme — periodic re-assessment cadence per risk tier, drift-based re-assessment triggers, incident-driven re-assessment triggers — and route into the AI risk register.
- Design the third-line independent audit programme — audit-plan authoring, audit-scope definition, audit-artefact contracts, audit-committee reporting cadence — using ForHumanity IAAIS + BABL AI Algorithm Bias Auditor + ISO/IEC 42006 as shape references.
- Design the interface with external audit / certification bodies (ISO 42001 certification body, ForHumanity independent auditor, sector regulator examiner) — evidence packaging, sampling access, remediation plans.
- Coordinate with `ai-evaluation-engineer` (peer, level 35) — that peer executes release-assurance inside the architecture this role designs — and with `ai-governance-analyst` (level 15) — that role collects the analyst-tier evidence audit consumes.
- Position NIST SP 800-37 RMF as the reference process shape the assurance system composes with.

## Chapters

1. [`01-the-three-lines-of-defence-for-ai.md`](01-the-three-lines-of-defence-for-ai.md) — The IIA Three Lines Model specialised for AI: first-line model owners and platform; second-line governance, evaluation, MRM where applicable, risk-engineering; third-line internal audit. Four AI-specific pressures (specialist competence; model-as-product boundary; post-deployment surveillance obligation; external-audit-body proliferation). Six invariants and the two failure modes (collapsed lines, theatrical lines) the architecture must design against.
2. [`02-pre-deployment-assurance-gate-design.md`](02-pre-deployment-assurance-gate-design.md) — The second-line-authoritative launch decision. The evidence contract (base bundle plus tier overlays), the five quality gates, the six sequenced reviews, the signature block per tier, the four escalation paths, and the decision-record shape the gate produces.
3. [`03-ongoing-assurance-cadence-and-triggers.md`](03-ongoing-assurance-cadence-and-triggers.md) — Keeping the launch decision fresh. Four trigger types (periodic tiered cadence; drift-driven quantitative thresholds; incident-driven taxonomy scope; regulatory-change horizon scan) compose into a programme that writes back into the risk register, the SoA, the RTP, the CAPA register, and the portfolio view. Composition with post-market monitoring (mod-110) and MRM under SR 11-7. Failure modes: periodic-only, trigger-flooded.
4. [`04-third-line-independent-audit-programme.md`](04-third-line-independent-audit-programme.md) — The internal audit function's AI scope. Multi-year programme; annual plan; engagement-scope-of-work template; six-phase engagement procedure; audit-artefact contract; audit-committee reporting cadence. Three competence patterns (development, co-sourcing, external contract). Composition with ForHumanity IAAIS, BABL AI, and ISO/IEC 42006. Failure modes: AIMS-clauses-only audit, friendly audit.
5. [`05-external-audit-and-certification-interface.md`](05-external-audit-and-certification-interface.md) — The outward-facing artefact of the assurance architecture. Five external-provider types (ISO 42001 certification body, ForHumanity IAAIS auditor, sector regulator examiners across banking / insurance / EU market surveillance / FDA / notified body, statutory financial auditor, specialist bias auditor). Packaging discipline; sampling-access pathway; remediation-plan template composing with CAPA. Failure modes: last-minute package, negotiation-shaped engagement.
6. [`06-coordination-contracts-with-evaluation-and-analyst.md`](06-coordination-contracts-with-evaluation-and-analyst.md) — What makes the architecture executable. Peer-to-peer coordination contract with the level-35 `ai-evaluation-engineer` (three-round proposal-response-reconciliation; reciprocal boundaries; standing forums). Role-scope contract for the level-15 `ai-governance-analyst` (six primary outputs; competences held and not held; escalation discipline). Interface between the two roles. Failure modes: analyst-as-fallback, architect-as-manager.
7. [`07-nist-sp-800-37-rmf-as-reference-process-shape.md`](07-nist-sp-800-37-rmf-as-reference-process-shape.md) — Composing the assurance architecture with the RMF process shape. Step-by-step mapping from Prepare / Categorize / Select / Implement / Assess / Authorize / Monitor onto the module's artefacts. Where the AI-specific extension is required and where the mapping is direct. When to cite the RMF composition (federal-facing customers; enterprises whose assurance system needs a process-shape reference).

## Exercises

- [`exercises/exercise-01-three-lines-for-ai-diagramming.md`](exercises/exercise-01-three-lines-for-ai-diagramming.md) — Draw the three-lines-of-defence architecture for a specified scenario: decision document, per-system ownership schematic, and independence matrix. Anchor artefact for the module — downstream exercises bind to the shape this exercise fixes.
- [`exercises/exercise-02-pre-deployment-assurance-gate-design.md`](exercises/exercise-02-pre-deployment-assurance-gate-design.md) — Author the pre-deployment gate charter, the decision-record template, and one worked decision-record for a tier-3 or tier-4 system in the scenario.
- [`exercises/exercise-03-ongoing-assurance-cadence-drill.md`](exercises/exercise-03-ongoing-assurance-cadence-drill.md) — Author the ongoing assurance programme charter, the per-system trigger registry, and one worked re-assessment record per trigger type (periodic, drift, incident, regulatory-change).
- [`exercises/exercise-04-third-line-audit-programme-design.md`](exercises/exercise-04-third-line-audit-programme-design.md) — Author the multi-year audit programme, one year's plan, one full engagement-scope-of-work, the audit-artefact contract, and the audit-committee reporting shape.
- [`exercises/exercise-05-external-audit-interface-authoring.md`](exercises/exercise-05-external-audit-interface-authoring.md) — Author the external-audit interface charter, per-provider engagement contracts, an evidence-package skeleton, the sampling-access pathway, and the remediation-plan template.
- [`exercises/exercise-06-assurance-owner-contracts-with-evaluation-engineer.md`](exercises/exercise-06-assurance-owner-contracts-with-evaluation-engineer.md) — Author the peer-to-peer coordination contract with the level-35 evaluation engineer and the role-scope contract for the level-15 analyst; walk three worked coordination scenarios.

## Structure

- `01-…md` … `07-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs (planned).
- `quizzes/`: knowledge checks (planned).
- `resources.md`: external references.
