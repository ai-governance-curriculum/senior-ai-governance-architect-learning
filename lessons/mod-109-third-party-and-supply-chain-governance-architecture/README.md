# mod-109-third-party-and-supply-chain-governance-architecture: Third-Party and AI Supply-Chain Governance Program Architecture

The level-50 architect owns the enterprise third-party AI governance programme end to end: the tiering that decides which vendors get which depth of scrutiny, the due-diligence questionnaire attached to each tier, the contract-template controls that make evidence-access enforceable, the ongoing-monitoring schedule that catches vendor-side drift, the federal-acquisition shape (OMB M-24-18 / M-25-22) adapted to enterprise procurement, and the supply-chain evidence contract that gates ingestion of every third-party model artefact. This module designs each piece. Chapters compose left-to-right; each chapter's substrate is the anchor for the ones that follow.

**Estimated effort:** 16 hours

## Learning objectives

- Design the third-party AI governance programme — SR 23-4 shape at architect scope applied to foundation-model providers (Anthropic, OpenAI, Google, Meta, xAI, open-weights vendors), guardrail vendors, AI-observability vendors, GRC-for-AI platform vendors, dataset providers.
- Design the tiering criteria for third-party AI vendors — data sensitivity, decision materiality, replaceability, jurisdictional exposure — and the due-diligence questionnaire attached to each tier.
- Design the contract-template controls — data-use limits, evaluation-access rights, incident-notification obligations, evidence-access rights, exit / portability clauses — coordinated with procurement + legal (out-of-scope for authorship, in-scope for template design).
- Design the ongoing vendor-monitoring schedule — periodic re-attestation, drift-based re-assessment, vendor-incident routing into the enterprise risk register, contract-renewal review.
- Read OMB M-24-18 (2024) and M-25-22 (2025) as the federal-facing AI acquisition shape; adapt the shape to enterprise procurement.
- Design the AI supply-chain assurance slice at architect scope — CycloneDX ML-BOM + SPDX 3.0 AI profile + SLSA levels + Sigstore as the evidence contract for every third-party model artefact; coordinate with `ai-infra-security` (level 35) on runtime supply-chain / signing platform depth.

## Chapters

1. [`01-the-third-party-ai-governance-lens.md`](01-the-third-party-ai-governance-lens.md) — Why third-party AI governance is a distinct architectural concern; the vendor typology (foundation-model providers, guardrail vendors, AI-observability vendors, GRC-for-AI platforms, dataset providers); SR 23-4 as the process-shape reference; five architectural stances and seven invariants the programme holds; two failure modes to design against.
2. [`02-vendor-tiering-criteria-and-tier-definitions.md`](02-vendor-tiering-criteria-and-tier-definitions.md) — The four dimensions (data sensitivity, decision materiality, replaceability, jurisdictional exposure); scoring rubrics; the tier-derivation rule with auto-tiering carve-outs; the four tier definitions and their DDQ / contract / monitoring bundle scaffolds; the re-tiering discipline.
3. [`03-due-diligence-questionnaire-architecture.md`](03-due-diligence-questionnaire-architecture.md) — The schema of a well-formed question; the six DDQ categories; the tier-per-category coverage matrix; the reviewer model and aggregate scoring; composition with SIG / CAIQ so the programme extends rather than duplicates enterprise TPRM.
4. [`04-contract-template-controls.md`](04-contract-template-controls.md) — Six control families (data-use limits, evaluation-access, incident-notification, evidence-access, exit-and-portability, cross-cutting operational); tier-mandatory matrix; composition with MSA, DPA, order form, and sector schedules; the drafting-handoff contract with procurement and legal; DDQ-to-contract closure loop.
5. [`05-ongoing-vendor-monitoring-schedule.md`](05-ongoing-vendor-monitoring-schedule.md) — Periodic re-attestation cadences per tier; the drift-triggers catalog (vendor-side and enterprise-side); vendor-incident routing through triage, containment, AIMS non-conformity, risk register, and regulator-facing report; contract-renewal review at pre-renewal window; vendor register monitoring-state overlay and operating-rhythm queries.
6. [`06-federal-acquisition-shape-adaptation.md`](06-federal-acquisition-shape-adaptation.md) — OMB M-24-10 governance memorandum and M-24-18 / M-25-22 acquisition memoranda; the rights-impacting / safety-impacting designations; the vendor-representation regime; the mandatory-practices bundle; five federal-adaptation control shapes (FED-01 through FED-05); the federal-customer schedule for enterprises that resell to US federal customers.
7. [`07-ai-supply-chain-evidence-contract.md`](07-ai-supply-chain-evidence-contract.md) — Eight artefact-class taxonomy (open-weight checkpoints, vendor fine-tunes, datasets, guardrail models, evaluation sets, prompt libraries / agent templates, runtime containers, observability / GRC agents); minimum-evidence bundle per class × tier (hash, licence, signature, CycloneDX ML-BOM or SPDX 3.0 AI profile, SLSA level); registry ingestion gate; coordination contract with the level-35 `ai-infra-security` peer; substrate composition with mod-108.

## Exercises

Under [`exercises/`](exercises/):

- [`exercise-01-third-party-ai-vendor-tiering-drill.md`](exercises/exercise-01-third-party-ai-vendor-tiering-drill.md) — Score and tier a 10-vendor population against the chapter 02 rubric; author the tiering worksheet, the auto-tiering carve-out log, and the register-population artefact.
- [`exercise-02-due-diligence-questionnaire-authoring.md`](exercises/exercise-02-due-diligence-questionnaire-authoring.md) — Author a tier-3 DDQ instance for a specific vendor scenario across all six categories using the chapter 03 schema.
- [`exercise-03-contract-template-control-drill.md`](exercises/exercise-03-contract-template-control-drill.md) — Convert DDQ commitments into a tier-scaled contract-template control specification for legal drafting; author the DDQ-to-contract closure record and the residual-risk carry.
- [`exercise-04-ongoing-vendor-monitoring-schedule-design.md`](exercises/exercise-04-ongoing-vendor-monitoring-schedule-design.md) — Author the four-component monitoring schedule for a specific vendor at a specific tier; draft the drift-trigger catalog with detection wiring; rehearse a vendor-incident routing scenario.
- [`exercise-05-federal-acquisition-shape-adaptation-drill.md`](exercises/exercise-05-federal-acquisition-shape-adaptation-drill.md) — Derive the enterprise mapping from OMB M-24-18 / M-25-22 (or the currently-effective successor at reading time); extend the tiering scheme, DDQ, contract catalog, and monitoring schedule with the federal-adaptation additions.

## Structure

- `01-…md` … `07-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs.
- `quizzes/`: knowledge checks.
- `resources.md`: external references.

## What this module composes with

- **`mod-102`** — the enterprise AI control library carries the third-party governance controls (SR 23-4 shape, ISO/IEC 42001 Annex A third-party controls). This module is the implementation shape for those controls.
- **`mod-103`** — the third-party AI acceptance policy is a first-line-facing standard the policy hierarchy carries; the ingestion-gate policies (chapter 07) are policy-as-code artefacts.
- **`mod-105`** — the AIMS Clause 8 operational planning and Annex A controls on third-party-provided AI systems bind the AIMS to this module's programme.
- **`mod-106`** — vendor-risk categories are first-class nodes in the risk taxonomy; the programme's monitoring cadence and exception thresholds calibrate against the appetite statement.
- **`mod-107`** — the pre-deployment gate checks that every third-party dependency is tier-appropriately governed; the third-line audit samples the programme's operation.
- **`mod-108`** — vendor-emitted evidence (SOC 2 letters, model cards, ML-BOMs, SLSA attestations, incident notifications) lands in the substrate under the schema catalog; chapter 07 leans directly on mod-108 chapter 04's supply-chain evidence slice.
- **`mod-110`** — vendor incidents and vendor-version drift are post-market signals routed into the risk register through the shape mod-110 designs.
- **`mod-111`** — GRC-for-AI vendors are themselves a class-4 vendor under this module's typology; mod-111 walks the platform-selection depth.
- **`ai-infra-security` (level 35)** — the runtime supply-chain, signing platform, KMS, and registry admission controller are the level-35 peer's craft. Chapter 07 pins the coordination interface: architect owns the evidence-contract shape; level-35 peer owns the runtime enforcement platform.
- **`procurement`, `legal`, `information security`, `enterprise TPRM`** — the programme is coordinated through declared handoffs at each of chapters 03, 04, and 05.
