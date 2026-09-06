# exercise-01: GRC For AI Reference Architecture Drawing

**Estimated effort:** 3 hours

## Objective

Produce the **enterprise-specific GRC-for-AI reference architecture** for a specified scenario — the target-state artefact the level-50 architect takes to the CIO, the head of AI governance, and the ISO/IEC 42001 certification body when any of them asks *so is this a new system we are standing up, or is it an extension of what we already have, and what does it terminate on?* The deliverable set is a decision document, a machine-readable schematic mirroring the chapter-01 shape, an integration-edge catalog, a build-vs-buy scoping note, and an invariant-enforcement table.

The correctness spine is the six invariants (I1–I6) and the two failure modes fixed in chapter 01. Every design choice in the artefacts you produce here must be pinnable to one of the invariants (as the choice that enforces it) or to one of the two failure modes (as the choice that defends against it) — the *isolated-island* pattern where integrations are deferred and artefacts go stale within days, and the *parallel-platform duplication* pattern where the AI programme reproduces the enterprise GRC's shape inside a separate AI-GRC platform. The deliverable set composes downstream: exercise-02 (vendor evaluation matrix) reads rows from the integration edges and requirements you fix here; exercise-03 (workflow layer) binds to the coordination roles and ticketing edge; exercise-04 (RBAC + SoD) binds to the identity invariant and the IdP edge; exercise-05 (observability adjacencies) binds to the observability edge and the lake-vs-register-tier split. Get the shape right and the module composes. Get it wrong — smear observability into the system-of-record, leave the enterprise-GRC composition contract implicit, omit the owner on an integration edge — and every downstream exercise inherits the flaw.

Draft as a senior architect briefing an apprentice. The deliverables are contractual artefacts, not essays; the requirements below name what must be present, not how to phrase it.

## Prerequisites

- Chapter [`01-grc-for-ai-reference-architecture-and-enterprise-integration.md`](../01-grc-for-ai-reference-architecture-and-enterprise-integration.md) read once, with the two-store composition, the eight integration categories, the six invariants, and the two failure modes marked.
- Chapter [`02-grc-for-ai-vendor-evaluation-matrix.md`](../02-grc-for-ai-vendor-evaluation-matrix.md) skimmed — you need to know what rows the vendor-evaluation exercise will lift from your integration-edge catalog and build-vs-buy note.
- Chapter [`04-rbac-and-segregation-of-duties-model.md`](../04-rbac-and-segregation-of-duties-model.md) skimmed — the identity invariant you enforce here is what the RBAC + SoD model binds to.
- Chapter [`05-ai-runtime-security-adjacencies.md`](../05-ai-runtime-security-adjacencies.md) skimmed — the runtime-security edge you name here is what chapter 05 walks in detail.
- Chapter [`06-ai-observability-adjacencies.md`](../06-ai-observability-adjacencies.md) skimmed — the observability edge and the register-tier-vs-lake split you commit to here are what chapter 06 fills in.
- Chapter [`07-singapore-ai-verify-integration.md`](../07-singapore-ai-verify-integration.md) skimmed — one candidate integration seam the architecture may need to terminate on for the SaaS scenario.
- The mod-102 control-library work (system-of-record hosting), mod-105 AIMS documented-information work (scope + Statement of Applicability hosting), mod-106 risk-register work (taxonomy composition with enterprise GRC), mod-107 three-lines architecture (pre-deployment gate hosting), mod-108 evidence-artefact index (pointer-not-copy discipline), mod-109 third-party AI inventory (composed with enterprise TPRM), and mod-110 chapters 01 and 03 (PMS event store as a hosted artefact class; two-tier lake + register-tier split).
- Access to primary references — enterprise GRC / IRM vendor product pages (RSA Archer, ServiceNow GRC/IRM, MetricStream, OneTrust, LogicGate) `<!-- needs-research: verify current product-surface claims and any AI-governance-specific modules as of authoring date -->`; ML platform registry documentation for MLflow, Vertex AI Model Registry, SageMaker Model Registry, Databricks Model Registry, and Weights & Biases; identity provider references for Okta, Microsoft Entra ID, and Ping Identity (SAML / OIDC / SCIM surface); ISO/IEC 42001 clauses on documented information and operational planning; SR 11-7 on model risk management. See [`../resources.md`](../resources.md).

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose GRC-for-AI reference architecture you are **least familiar with**; that is where the exercise will teach you most. State your choice at the top of the decision document.

- **A US regional bank** (Northbrook Financial-style, per mod-101 exercise-02 and mod-102 exercise-01) with ~40 AI systems: fraud classifiers, document extraction, an internal RAG legal assistant, a customer-facing generative chat, third-party AI SaaS integrations. Colorado + NYC deployments; EU expansion planned. An SR 11-7-aligned MRM programme and an ISO/IEC 27001 ISMS already in place; an incumbent enterprise GRC / IRM platform is already in production for financial, operational, and cyber risk.
- **A global healthcare payer / provider** with clinical-decision-support pilots, patient-facing chat, coding automation, and utilisation-management AI. US federal HIPAA scope; EU AI Act relevance for European insurance subsidiaries; multiple US state deployments including Illinois and California. A classical clinical-safety oversight committee reports to the CMO; an incumbent enterprise GRC / IRM platform is in place with modules for operational, cyber, third-party, and clinical-quality risk.
- **A B2B SaaS platform vendor** shipping GenAI-augmented HR-tech capabilities into enterprise customers across the US, UK, EU, and Singapore. Customers include public-sector deployments (subject to OMB M-25-21 shape) and financial-services enterprises (subject to SR 11-7 vendor-review shape). Enterprise GRC footprint is lighter — a modern IRM / trust-management platform is in place for SOC 2 and ISO 27001 evidence, but not a full enterprise GRC of the bank / payer class.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`ref-arch-proposal.md`** — the decision document. The trade-offs live here; the other four artefacts instantiate them.
2. **`ref-arch-schematic-v1.0.0.yaml`** — the machine-readable schematic of the GRC-for-AI reference architecture, mirroring the chapter-01 schematic shape (`system_of_record`, `composed_with`, `integrations`, `invariants`, `coordination_roles`).
3. **`integration-edge-catalog.md`** — one entry per integration edge (ML platforms, AI observability, AI runtime security, identity, ticketing, communications, enterprise data lake, document management, and the enterprise GRC composition edge).
4. **`build-vs-buy-scoping-note.md`** — the fit-and-gap scoping of which components are build vs buy vs adopt-as-is, with requirements per build-side component.
5. **`invariant-enforcement-table.md`** — for each of the six chapter-01 invariants (I1 single system of record; I2 every edge intentional in direction; I3 IdP as only identity source; I4 enterprise GRC composed with, not replaced; I5 lake for raw and platform for register-tier; I6 documents referenced not copied), the specific design choice that enforces it and an auditable detection test.

## Requirements

### `ref-arch-proposal.md`

The decision document. Every subsection below is required.

- **Scenario declaration.** Name the chosen scenario and the two or three enterprise-specific facts (incumbent enterprise GRC platform, existing ML platform stack, jurisdictional footprint, provider-vs-deployer split) that most constrain the reference architecture.
- **Two-store composition statement.** Explicitly declare the GRC-for-AI platform as the *system of record* for AI-nexus governance artefacts (name each hosted artefact class from mod-102, mod-105, mod-106, mod-107, mod-108, mod-109, and the mod-110 register-tier), and the enterprise GRC platform as *composed with, not replaced*. State the composition-contract shape at the summary level here; the detailed field lists live in the schematic.
- **Coordination roles.** Name the enterprise seats that populate and ratify each face of the architecture. Use the level-numbered role names from the track's role tree (level-15 AI governance analyst, level-25 AI risk engineer, level-35 AI evaluation engineer and AI infra security, level-50 senior AI governance architect, level-60 head of AI governance) plus the adjacent enterprise roles named in chapter 01 (enterprise IAM lead, data platform lead, ML platform lead, enterprise ticketing platform owner, enterprise collaboration platform owner, CIO / enterprise-architecture board, third-line internal audit). Where the scenario adds a specialist seat (MRM function for the bank; clinical-safety-committee liaison for the healthcare scenario; customer-facing trust-office liaison for the B2B SaaS scenario), name it and place it.
- **Failure-mode defence.** For each of the two chapter-01 failure modes — (a) isolated-island (integrations deferred to "roadmap," artefacts stale within days) and (b) parallel-platform duplication (enterprise GRC's shape reproduced inside the AI-GRC platform) — name at least one concrete architectural move you have made in your scenario to prevent it. Each move must be tied to a design choice named elsewhere in the document (an integration made bidirectional-by-design; a composition-contract field explicitly enumerated; an artefact class explicitly hosted only once).
- **Non-scope.** Name at least three things you deliberately excluded from the GRC-for-AI platform's scope and why. Candidates: runtime enforcement of AI policies at inference time (a chapter-05 concern and a runtime-security-platform responsibility, not a GRC-for-AI-platform responsibility); per-team monitor authorship (the mod-110 chapter-02 monitoring-register contract is a *consumer* interface, not a monitor authoring surface); the IdP itself (owned by enterprise IAM — the architecture *consumes* identity, does not author it); the enterprise data lake (owned by the data platform team per mod-110 chapter 03); the enterprise document management platforms (owned by their respective content owners — the architecture references documents, does not host them).
- **Composition contract for downstream exercises.** A short subsection naming, per downstream exercise (`exercise-02` vendor-evaluation matrix; `exercise-03` workflow layer; `exercise-04` RBAC + SoD; `exercise-05` observability adjacencies), which fields of your schematic, integration-edge catalog, or invariant-enforcement table it binds to. This is the seam that lets a peer author the next exercise against your shape.

### `ref-arch-schematic-v1.0.0.yaml`

Machine-readable schematic of the GRC-for-AI reference architecture, mirroring the chapter-01 schematic shape. Version the file (`version: 1.0.0`) and include, at minimum:

- A `system_of_record` block naming the platform role, the identity source (must resolve to the enterprise IdP), the local user-table policy (must be `none`), and the list of hosted artefact classes (mod-102 control library, mod-105 AIMS documented information, mod-106 AI risk register, mod-107 assurance artefacts, mod-108 evidence artefact index, mod-109 third-party AI inventory, mod-110 PMS event store register-tier).
- A `composed_with` block naming the enterprise GRC platform's role in the scenario and a `contract` sub-block with two enumerated field lists — `inbound_from_grc_for_ai` and `outbound_to_grc_for_ai` — at the shape chapter 01 walks (aggregated risk exposure per enterprise-taxonomy node; AI-nexus issues in the enterprise issue schema; AI-nexus incidents crossing enterprise materiality; enterprise policy library; enterprise risk taxonomy version pins; audit committee cadence calendar).
- An `integrations` block with **one entry per category** — `ml_platforms`, `ai_observability`, `ai_runtime_security`, `identity`, `ticketing`, `communications`, `enterprise_data_lake`, `document_management`. Each entry carries `counterparty`, `direction` (must be either `bidirectional` or explicitly `one-way` with the direction stated — never omitted), `owner_role` (must be a named seat, never a placeholder), `contract` (the shape of the exchange in one line), and `category_examples` (the vendor-category enumeration from chapter 01, marked `<!-- needs-research -->` for any specific product-surface claim).
- An `invariants` block enumerating all six invariants (I1–I6) with stable identifiers the other artefacts cite.
- A `coordination_roles` block naming, per face of the architecture (architect, workflow, RBAC, security, observability, composition-contract ratifier), the seat that owns and the seat that ratifies.
- A `change_control` block naming who ratifies schematic version bumps and how.

Every identifier in the schematic (invariant IDs, integration IDs, artefact-class IDs) must be stable — the integration-edge catalog, the build-vs-buy note, and the invariant-enforcement table will cite them.

### `integration-edge-catalog.md`

One entry per integration edge — the eight integration categories above **plus** an entry for the enterprise GRC composition edge (nine entries total). Each entry must state:

- **Counterparty.** The named enterprise-side platform class (e.g. `enterprise model registry — MLflow / Vertex AI / SageMaker / Databricks / Weights & Biases class`), with any specific product-surface claim marked `<!-- needs-research -->`.
- **Direction.** Bidirectional-by-design or explicitly one-way with the direction stated. If the direction is one-way, name the reason it is not bidirectional (chapter 01 gives the pattern: communications is one-way outbound because the platform does not consume replies from Slack; identity is one-way inbound because the platform does not master identity).
- **Owner role.** The named enterprise seat responsible for the edge, both on the GRC-for-AI side (typically the level-50 architect co-owning) and on the counterparty side (the enterprise IAM lead, the ML platform lead, the ticketing platform owner, etc.). An edge without a named owner_role becomes an orphan within a quarter.
- **Contract shape.** The shape of the exchange — schema families, protocol (webhook, SCIM, SAML, deep-link, streaming event), and the versioning discipline. Where the shape depends on a specific vendor surface, mark the claim `<!-- needs-research -->`.
- **Open questions.** At least one open question per edge that must be answered before the edge can be provisioned. Candidates: which tenant / OU on the enterprise IdP is the platform bound to; which project / workspace on the ML platform is treated as authoritative; what happens when the ticketing platform is unavailable during a control-testing window.

### `build-vs-buy-scoping-note.md`

For each named component in the reference architecture — the system-of-record platform itself, the workflow layer, the RBAC layer, the evidence-artefact index, the risk-register data model, the third-party AI inventory data model, the AIMS documented-information store, and each of the eight (nine including the enterprise GRC composition edge) integration edges — decide **build**, **buy**, or **adopt-as-is** and state the reasoning in one paragraph per component.

For every component marked **build**, state the requirements the build-side component must satisfy:

- **Schema flexibility.** What the component's data model must accommodate — the mod-106 taxonomy at its required granularity, the mod-108 evidence-contract freshness properties, the mod-105 SoA structure, the mod-110 event schema. Point at the mod-X source that fixes each requirement.
- **Latency.** The response-time envelope the component must hold (register-tier events ingested within the mod-110 chapter 03 cadence; deployment-gate decisions written back to the model registry within the mod-107 chapter 02 gate window).
- **Integration surface.** The protocols the component must speak (webhook out, webhook in, SCIM, SAML / OIDC, streaming ingest, deep-link URL scheme).
- **RBAC.** The access-control granularity the component must expose (per-artefact, per-artefact-class, per-hosted-module), and the SoD constraints (mod-107 pre-deployment reviewer cannot be the same seat as the producing engineer for the artefact under review).

For components marked **buy**, state the fit-and-gap axes that the exercise-02 vendor-evaluation matrix will score against — one axis per requirement above.

For components marked **adopt-as-is** (the enterprise IdP, the enterprise data lake, the enterprise document management platforms, the enterprise ticketing platform, the enterprise communications platforms, the incumbent enterprise GRC platform), state the *reason* it is adopt-as-is (owned by another enterprise function; the composition contract, not the platform, is the architect's artefact) and any residual constraints the architect must impose on the counterparty (e.g. the IdP must expose SCIM; the ticketing platform must accept webhook-in for status updates).

Three anti-patterns from chapter 01 must be named explicitly, with the architectural move in this scenario that avoids each:

- The "we'll build it in ServiceNow" pattern (extending the enterprise GRC platform to host the AI programme rather than standing up a dedicated GRC-for-AI platform). Name the specific requirements from the schema-flexibility list above that the enterprise GRC platform would have to satisfy.
- The "we'll buy a vendor and integrate later" pattern (deferring integrations to "roadmap"). Name the specific integration edges you have insisted must be live at go-live in this scenario.
- The "we already have observability, we don't need GRC-for-AI" pattern (leaning on observability platforms as system of record). Name the hosted artefact classes that observability platforms cannot host and the invariants that fail if the enterprise tries.

### `invariant-enforcement-table.md`

For each of the six chapter-01 invariants, one row with three columns: **invariant** (identifier + one-line statement); **design choice** (the specific architectural move in your scenario that enforces the invariant, referencing the schematic identifier); **detection test** (the auditable check an internal auditor could execute in a working session).

Required rows, one per invariant:

- **I1 — single system of record for AI-nexus governance artefacts.** Design choice must name each hosted artefact class and its authoritative host; detection test must be executable against the schematic (e.g. "sample five randomly chosen AIMS documented-information records and confirm each has exactly one authoritative host per the schematic's `system_of_record.hosts` list").
- **I2 — every integration edge is bidirectional-by-design or explicitly one-way.** Design choice references the `direction` field in the integration block; detection test scans the integration table for any edge whose direction is omitted or ambiguous.
- **I3 — the enterprise IdP is the only source of identity.** Design choice references the `identity_source` and `user_table: none` fields; detection test enumerates local users on the platform and asserts the count is zero (or asserts an explicitly-named exception list with an owner and an expiry date).
- **I4 — enterprise GRC is composed with, not replaced.** Design choice references the `composed_with.contract` inbound and outbound field lists; detection test samples the audit-committee portfolio dashboard and confirms every AI-nexus row aggregates through the composition contract, not a parallel taxonomy.
- **I5 — the enterprise data lake holds raw telemetry; the platform holds register-tier state.** Design choice references the mod-110 chapter 03 two-tier retention configuration; detection test queries the platform's storage for any raw telemetry (payloads, feature values, prompt bodies) beyond the register-tier retention envelope and asserts the query returns empty.
- **I6 — documents live where produced; the platform holds pointers, not copies.** Design choice references the mod-108 evidence-index pointer schema; detection test samples five evidence-artefact records and confirms each is a pointer with a version-pin and a freshness contract, not an uploaded copy.

Every detection test must be executable by an internal auditor in a working session, not a philosophical assertion.

## Starter guidance

- Draft `ref-arch-proposal.md` **first**. The schematic, integration-edge catalog, build-vs-buy note, and invariant-enforcement table each instantiate a trade-off the decision document forced. Working the other order — starting with the schematic — produces a schematic that is internally consistent but that does not defend against the two failure modes the module is designed against.
- Do **not** reproduce the enterprise GRC platform's shape inside the AI-GRC platform. The parallel-platform failure mode (chapter 01 failure mode b) is the one senior architects fall into most often, because the enterprise GRC platform's taxonomies and dashboards are attractive to copy. The composition contract is the discipline that lets the two platforms coexist without the reconciliation tax the failure mode imposes.
- The identity invariant is load-bearing. Chapter 01 fixes it as the strongest predictor of audit-time defensibility — the third-line auditor who samples current access and finds users whose enterprise employment ended months ago has a finding that is very hard to close. Set the platform's user-table policy to `none` and mean it; any exception ("emergency access," "external auditor accounts") must be named, owned, and time-bounded in the schematic, not tolerated silently.
- Every integration edge must have a **named** `owner_role` on both sides of the edge. An edge with a placeholder owner ("TBD," "platform team") becomes an orphan within a quarter — the isolated-island failure mode in its most common form. If you cannot name an owner, the edge is not real; either remove it from the architecture or block on identifying the owner before the architecture ratifies.
- Do **not** mark integrations with `<!-- roadmap -->` or "phase 2." Either the integration exists in the architecture and is exercised in the scenario, or it is absent and the architecture states so explicitly. The chapter-01 isolated-island failure mode is exactly the pattern where "roadmap" integrations accumulate and the platform's assurance value collapses. Ambiguity here is not neutral; it is the failure mode.
- The `<!-- needs-research -->` discipline applies to every vendor-specific and standard-specific claim. Do not attribute a feature to a named commercial product without a citation you have verified, and do not invent clause numbers, hours-to-report, or product-surface capabilities. The exercise-02 vendor-evaluation matrix will lift your integration-edge contract rows directly; a fabricated contract shape here becomes a fabricated evaluation row there.
- Distinguish the GRC-for-AI platform from the platforms it terminates on. The observability platforms are producers, not authorities (per mod-110 chapter 03); the model registry is the authoritative model inventory, not the platform's AI inventory; the enterprise DMS holds the authoritative documents, not the platform's evidence store. The build-vs-buy note is where you make each of these distinctions concrete for the scenario.

## Acceptance criteria

- [ ] Chosen scenario is stated at the top of `ref-arch-proposal.md` and every artefact is coherent against it.
- [ ] `ref-arch-proposal.md` covers scenario declaration, the two-store composition statement, coordination roles (named to the level-15 / 25 / 35 / 50 / 60 canonical roster plus enterprise IAM lead, data platform lead, ML platform lead, ticketing platform owner, enterprise collaboration owner, CIO, and third-line audit), failure-mode defence for both chapter-01 failure modes, non-scope (at least three exclusions with rationale), and composition contract for downstream exercises 02, 03, 04, and 05.
- [ ] Each of the two chapter-01 failure modes has at least one named architectural defence tied to a specific design choice.
- [ ] `ref-arch-schematic-v1.0.0.yaml` mirrors the chapter-01 shape with `system_of_record`, `composed_with` (with enumerated `inbound_from_grc_for_ai` and `outbound_to_grc_for_ai` field lists), `integrations` (one entry per category, each with counterparty, direction, owner_role, contract, category_examples), `invariants` (all six, I1–I6, with stable identifiers), `coordination_roles`, and `change_control`.
- [ ] Every integration entry in the schematic has a `direction` field that is either `bidirectional` or an explicit one-way direction — never omitted or ambiguous.
- [ ] Every integration entry in the schematic has a named `owner_role` on both sides of the edge — no placeholders.
- [ ] `integration-edge-catalog.md` includes one entry per edge (ML platforms, AI observability, AI runtime security, identity, ticketing, communications, enterprise data lake, document management, and the enterprise GRC composition edge — nine entries), each with counterparty, direction, owner role, contract shape, and at least one open question.
- [ ] `build-vs-buy-scoping-note.md` decides build vs buy vs adopt-as-is for each named component, and for every build-side component states the schema-flexibility, latency, integration-surface, and RBAC requirements the component must satisfy.
- [ ] `build-vs-buy-scoping-note.md` names all three chapter-01 anti-patterns explicitly and states the architectural move in the scenario that avoids each.
- [ ] `invariant-enforcement-table.md` includes one row per invariant I1–I6 with a specific design choice (referencing the schematic identifier) and an auditable detection test executable by an internal auditor in a working session.
- [ ] Non-scope explicitly excludes runtime enforcement (a chapter-05 concern), per-team monitor authorship (a mod-110 chapter-02 concern), and the IdP itself (owned by enterprise IAM).
- [ ] The composition contract subsection specifies, per downstream exercise (`exercise-02`, `exercise-03`, `exercise-04`, `exercise-05`), which fields the exercise binds to.
- [ ] Every specific claim about a named commercial vendor (RSA Archer, ServiceNow GRC / IRM, MetricStream, OneTrust, LogicGate, MLflow, Vertex AI, SageMaker, Databricks, Weights & Biases, Fiddler, Arthur, WhyLabs, Evidently, Robust Intelligence, Lakera Guard, Calypso AI, HiddenLayer, Protect AI, Okta, Entra ID, Ping, Jira, ServiceNow ITSM, Azure DevOps Boards, Slack, Teams, SharePoint, Confluence, Google Workspace, Box) is marked `<!-- needs-research: ... -->` — no invented product surfaces, integration APIs, or capability claims.
- [ ] Every citation to an ISO/IEC 42001 clause, an SR 11-7 supervisory expectation, or any other regulatory or standard artefact is either verifiable against a primary source or marked `<!-- needs-research: ... -->` — no invented clause numbers or paragraph references.

## Stretch goals

- **Vendor-surface overlay.** Overlay the reference architecture against a specific enterprise-GRC vendor's documented product surface (pick one of RSA Archer, ServiceNow GRC / IRM, MetricStream, OneTrust, or LogicGate — whichever the scenario's enterprise most plausibly runs). Mark every product-surface claim `<!-- needs-research -->` and produce a fit-and-gap sketch — which of the schematic's requirements the vendor satisfies as-shipped, which require configuration, which require a custom module, and which are not satisfiable within the vendor's product envelope.
- **Certification-body walkthrough mock.** Sketch the 20-minute path an ISO/IEC 42001 certification auditor would traverse through the reference architecture — which artefacts the auditor is pointed at, in what order, and where each artefact resolves to a single authoritative host per invariant I1. The mock surfaces where the architecture is walk-through-ready and where the auditor's question would produce a "let me get back to you" answer.
- **Two-way composition contract for enterprise GRC.** Author the `composed_with` contract as a full two-way schema — the inbound field list (from GRC-for-AI to enterprise GRC) and the outbound field list (from enterprise GRC to GRC-for-AI), each with field name, type, cardinality, cadence (event-triggered vs scheduled), and the mapping to the mod-106 enterprise-taxonomy reference field. The contract is the artefact the CIO and the enterprise-architecture board ratify per the chapter-01 coordination-roles section.
