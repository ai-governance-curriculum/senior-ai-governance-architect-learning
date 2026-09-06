# mod-103 — AI Policy Taxonomy and Policy-as-Code Architecture

**Estimated effort:** 16 hours

This module designs the enterprise AI policy taxonomy — the five-layer instrument shape (Responsible AI principles → binding policy → standards → procedures → work instructions) that sits atop the mod-102 control library — and the lateral policy-as-code slice that decides which policies are enforced at runtime, which at CI/CD gate time, and which by attestation only. It builds the hierarchy with authority bodies, verb strengths, review cadences, and exception paths per layer (chapter 01); adds the traceability discipline — the mapping matrix and the orphan / widow detectors — that keeps Responsible AI principles from being decorative (chapter 02); places the policy-as-code slice using OPA + Rego or AWS Cedar and reasons about the developer-experience cost of each tier (chapter 03); designs the exception / waiver workflow that keeps concessions rare, time-bounded, and visible (chapter 04); governs the change-communications flow so policy updates propagate cleanly to `ai-governance-analyst` and `ai-risk-engineer` through a deprecation window (chapter 05); and composes the taxonomy with ISO/IEC 22989 (vocabulary), ISO/IEC 38507 (governance-body positioning), and the IEEE 7000-series (ethics methodology) rather than accidentally competing with them (chapter 06). Every downstream module in the track (mod-104 multi-jurisdiction reconciliation, mod-105 AIMS, mod-106 risk taxonomy, mod-107 assurance, mod-108 evidence, mod-109 third-party, mod-110 post-market surveillance, mod-111 GRC-for-AI, mod-112 operating model, mod-113 sector blueprints) hangs off the taxonomy authored here.

## Learning objectives

- Design the AI policy hierarchy — Responsible AI principles → binding policy → standards → procedures → work instructions — with clear authority levels, review cadences, and exception paths.
- Map every enterprise Responsible AI principle to at least one binding policy, at least one standard, and at least one testable control in the control library — so the principle is not decorative.
- Design the policy-as-code slice — which policies are enforced at runtime (via Open Policy Agent + Rego or Cedar), which are enforced at CI/CD gate time, which are enforced by attestation only — and reason about the developer-experience cost of each.
- Design the exception / waiver workflow — who requests, who approves, how long the waiver lives, what compensating controls apply, how the waiver is tracked in the risk register.
- Design the policy-change communications flow — who gets a heads-up before publication, how policy changes route to `ai-governance-analyst` (level 15) for control-tracking updates and to `ai-risk-engineer` (level 25) for engineering-side updates, and how deprecation windows are enforced.
- Position ISO/IEC 38507 (governance implications of AI) + ISO/IEC 22989 (concepts and terminology) + IEEE 7000-series as the reference frames the policy taxonomy composes with.

## Chapters

1. [`01-ai-policy-hierarchy-authority-and-cadence.md`](01-ai-policy-hierarchy-authority-and-cadence.md) — the five-layer taxonomy with authority body, verb strength, review cadence, and exception path per layer; and the "human oversight, all the way down" worked scenario that threads a single topic through every layer.
2. [`02-principle-to-policy-to-standard-to-control-traceability.md`](02-principle-to-policy-to-standard-to-control-traceability.md) — the traceability chain as a DAG, the six-column mapping matrix, the four traceability rules, and the orphan / widow detectors that stop Responsible AI principles from being decorative.
3. [`03-policy-as-code-enforcement-tiers.md`](03-policy-as-code-enforcement-tiers.md) — the three tiers (runtime OPA/Rego or Cedar; CI/CD gate; attestation-only), the five-question placement framework, the PDP / PEP split, and the DX-cost profile of each tier.
4. [`04-exception-and-waiver-workflow.md`](04-exception-and-waiver-workflow.md) — the seven-field waiver record, the approval matrix by risk band, the max-lifetime cap, the risk-register integration, the lifecycle, and the three-way distinction between waiver / exception / compensating control.
5. [`05-policy-change-communications-and-deprecation-windows.md`](05-policy-change-communications-and-deprecation-windows.md) — the five change classes, the deterministic routing list, the seven-part notice packet, the deprecation-window discipline, the routing SLAs, and the coordinated cadence with the mod-102 control-library release.
6. [`06-iso-38507-22989-and-ieee-7000-composition.md`](06-iso-38507-22989-and-ieee-7000-composition.md) — the composition rule that sequences vocabulary (22989) → governance-body (38507) → management-system (42001) → ethics-methodology (IEEE 7000) → enterprise normative → control library, and the shape mistakes that follow from citing the wrong frame at the wrong altitude.

## Exercises

- [`exercises/exercise-01-policy-hierarchy-authoring-drill.md`](exercises/exercise-01-policy-hierarchy-authoring-drill.md) — instantiate all five taxonomy layers end-to-end on one topic and self-audit each layer for altitude.
- [`exercises/exercise-02-principle-to-policy-to-control-crosswalk.md`](exercises/exercise-02-principle-to-policy-to-control-crosswalk.md) — author the mapping matrix for five Responsible AI principles and the orphan / widow detector queries.
- [`exercises/exercise-03-opa-rego-and-cedar-policy-as-code-slice.md`](exercises/exercise-03-opa-rego-and-cedar-policy-as-code-slice.md) — place ten policies across the three tiers with the five-question framework, and hand-author one runtime, one gate, and one attestation artefact.
- [`exercises/exercise-04-exception-waiver-workflow-design.md`](exercises/exercise-04-exception-waiver-workflow-design.md) — author the waiver-register schema, approval matrix, three worked waivers across risk bands, and a three-way classification drill.
- [`exercises/exercise-05-policy-change-communications-flow.md`](exercises/exercise-05-policy-change-communications-flow.md) — walk one real policy change through the seven-part notice packet, the release notice, and the deprecation-window plan.

## Other assets

- [`labs/`](labs/) — long-form hands-on labs (planned).
- [`quizzes/`](quizzes/) — knowledge checks (planned).
- [`resources.md`](resources.md) — curated primary references.
