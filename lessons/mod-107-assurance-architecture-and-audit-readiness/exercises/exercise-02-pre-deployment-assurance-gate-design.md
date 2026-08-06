# exercise-02: Pre-Deployment Assurance Gate Design

**Estimated effort:** 4 hours

## Objective

Author the **pre-deployment assurance gate charter and decision-record template** for a specified enterprise scenario. This is the artefact the second-line function operates against every time an AI system approaches launch — the evidence contract, the six sequenced reviews, the signature block, the escalation paths, and the decision-record shape from chapter 02, all instantiated for your scenario with a concrete tier scheme and named seats.

The deliverable is the charter document, a decision-record template, and one worked decision-record for a specific tier-3 or tier-4 system in your scenario. Downstream exercises (exercise-03 ongoing cadence, exercise-05 external interface, exercise-06 coordination contracts) will consume the gate design you author here.

## Prerequisites

- Chapter [`02-pre-deployment-assurance-gate-design.md`](../02-pre-deployment-assurance-gate-design.md) read with the six-review sequence, the escalation paths, and the two failure modes marked.
- Exercise-01 in this module complete (or your scenario architecture in a form you can reference).
- Chapter [`01-the-three-lines-of-defence-for-ai.md`](../01-the-three-lines-of-defence-for-ai.md) read for the second-line-vs-first-line boundaries the charter enforces.
- Familiarity with the mod-102 control library shape (SoA rows, applicability filter) and the mod-108 evidence contract (the artefact the gate reviews).
- Access to primary references — ISO/IEC 42001 Clause 8 (operational planning and control), Regulation (EU) 2024/1689 Articles 9–15, SR 11-7, NIST AI RMF Generative AI Profile (AI 600-1) for evaluation-methodology reference. See [`../resources.md`](../resources.md).

## Scenario

Use the same scenario you chose in exercise-01 (bank, healthcare, or B2B SaaS). Restate it at the top of the deliverable so the exercise is self-contained.

## Deliverables

Author three artefacts in a working directory of your choice.

1. **`gate-charter.md`** — the charter document.
2. **`decision-record-template.yaml`** — the machine-readable template for a gate decision-record.
3. **`worked-decision-record.yaml`** — one instance of the template for a specific system in your scenario, at tier 3 or tier 4.

## Requirements

### `gate-charter.md`

Decide and justify **each** of the following:

- **Tier scheme.** State your enterprise's tier scheme (default to the four-tier shape from chapter 02 unless your scenario justifies a different structure). For each tier: representative examples from your scenario, definition of what makes a system that tier, cadence of periodic re-assessment (previewing exercise-03).
- **Evidence-contract base bundle.** The set of artefacts required for every system every tier. Enumerate against the table in chapter 02, adapted for your scenario. State the owner and reviewer per artefact.
- **Evidence-contract tier overlays.** The additional artefacts required at tier-3 and at tier-4. Include the AI-Act Article 11–15 overlay if the scenario has EU exposure. Include the sector-specific overlay (SR 11-7 model-file for the bank; FDA-adjacent evidence for the healthcare CDS; SOC-2-adjacent evidence for the B2B SaaS).
- **Evidence-quality gates.** State how the five quality gates (completeness, freshness, traceability, reproducibility, independence-of-authorship) are checked before the gate convenes. Name the seat that runs the pre-gate completeness check and defend why it is not the same seat that chairs the gate.
- **Sequenced reviews.** Walk each of the six reviews from chapter 02, adapted for your scenario. For each: reviewer seat, standard duration, pass / conditional / block outcome criteria, and the specific challenge questions the review asks.
- **Signature block.** The signatures required per tier at your scenario. Extend to any additional signatures your scenario requires (e.g. general counsel for legal-sensitive systems; CMO for clinical-decision-support in healthcare; enterprise-customer-facing product owner for B2B SaaS releases).
- **Escalation paths.** Walk each of the four escalation types from chapter 02, adapted for your scenario. For each: destination, time bound, reversal rules, and the specific artefacts the escalation produces.
- **Sampling quotas.** The specific sampling percentages the gate charter commits to per tier. Chapter 02 asserts the quota is small enough to be executable and large enough to change gate culture; pick concrete numbers and defend.
- **Non-scope.** At least three things you *chose not to* include in the charter and why. Candidates: a first-line self-attestation as a signature (belongs on model card, not on gate); a required marketing-launch sign-off (belongs in go-to-market, not in assurance); a security-only gate for high-risk systems (security is one dimension; the gate is multi-dimension).

### `decision-record-template.yaml`

Author a machine-readable template that instantiates chapter 02's decision-record shape. Every field the chapter's schematic carries plus any scenario-specific extensions must be present. Fields the template surfaces:

- `id`, `system`, `system_tier`, `scope_of_decision`
- `evidence_contract_discharge` block: base bundle, tier overlay, five quality gates
- `reviews` block: six sequenced reviews with outcome and reviewer per review
- `overall_decision`, `conditions_attached`
- `signatures` block with seat, role, timestamp, and scope-of-signature
- `cross_references` block: risk register entries, AIA id, SoA version, control library versions, evidence artefact ids
- `next_reassessment` block: scheduled date and event-driven triggers

The template's structure must support machine ingestion by a GRC-for-AI platform (mod-111). YAML anchors and aliases are permitted where they reduce redundancy but must not obscure the record's semantic content on human read.

### `worked-decision-record.yaml`

Instantiate the template for a specific system in your scenario. Pick a tier-3 or tier-4 system where the exercise will exercise the interesting parts of the charter — a system with a residual near the tolerance boundary, or a system in scope for at least one external regime (AI Act Article-11 system, an SR 11-7 material model, an FDA SaMD candidate).

The worked record must be *coherent* — every field internally consistent, all cross-references pointing to sensible-shape identifiers (you may invent identifiers for downstream artefacts but the identifier scheme must match your enterprise conventions from exercise-01). Signatures may be simulated (SEAT-A, SEAT-B, ...) but the signature block must reflect the tier's requirements from the charter.

Include at least one *interesting* feature: a conditional outcome, a near-miss residual, a scope carve-out, or an escalation reference. The point is to show the template working under a real (non-toy) load.

## Starter guidance

- Draft the charter *before* the template. If you cannot decide the shape in prose, the YAML template will be prettier but inconsistent.
- The evidence-contract base bundle is where first-time gate designers under-specify. Enumerate against chapter 02's table plus your scenario's specifics; do not leave "additional artefacts as needed."
- Sequenced-review challenge questions are the discipline that prevents rubber-stamping. Under review 4 (residual against appetite), the challenge question is not "is the residual score green?" — it is "walk me through the residual reading against the tolerance table and the three-stance decomposition." Write the challenge questions concretely.
- Sampling quotas require calibration. A quota of "the evaluation engineer will independently reproduce 100% of first-line eval runs" is unrealistic and will not survive contact with delivery pressure. A quota of "one run per gate meeting" is too low to change culture. Aim for a defensible middle — 10-25% of first-line runs, or a fixed number per meeting scaled to the meeting's throughput.
- Do not include first-line signatures on the decision-record. The gate is second-line-authoritative; the first line's ship-readiness decision is a separate artefact. Confusing the two is invariant-1 failure from chapter 01.
- For the bank scenario, the SR 11-7 model-file overlay is where the tier-3 and tier-4 overlays differ most from the base bundle; walk it explicitly. For the healthcare scenario, the FDA-adjacent evidence and the clinical-safety-committee interaction are the interesting overlay. For the B2B SaaS scenario, the customer-facing SOC-2 leverage and the customer DPA appendix currency are the interesting overlay.

## Acceptance criteria

- [ ] Scenario is stated at the top; charter is coherent against it.
- [ ] `gate-charter.md` decides all nine requirements bullets, each with stated rationale.
- [ ] Evidence-contract base bundle enumerates every artefact with owner and reviewer.
- [ ] Tier-3 and tier-4 overlays enumerate additional artefacts.
- [ ] Five quality gates each specify the check, the seat that runs it, and the failure consequence.
- [ ] Six sequenced reviews each specify reviewer seat, duration, pass / conditional / block criteria, and challenge questions.
- [ ] Signatures per tier match chapter 02, extended for scenario specifics.
- [ ] Four escalation paths each specify destination, time bound, reversal rule, and produced artefact.
- [ ] Sampling quotas are concrete numbers with defence.
- [ ] `decision-record-template.yaml` covers all fields from chapter 02's schematic plus scenario extensions.
- [ ] `worked-decision-record.yaml` is coherent and includes at least one non-trivial feature (conditional outcome, near-miss residual, scope carve-out, or escalation reference).
- [ ] Every unverified citation to AI Act articles, ISO clauses, SR 11-7 provisions, FDA guidance, or sector regulations is marked `<!-- needs-research: ... -->` — no invented article numbers or clause labels.

## Stretch goals

- Sketch what changes in the charter if the enterprise stands up a *frontier-model provider substitution capability* — the ability to migrate a system from one frontier-model provider to another mid-lifecycle. What additional evidence and reviews are required at the substitution's gate?
- Draft the *anti-shipping-pressure* discipline paragraph — the section of the charter that specifies how the gate protects itself when the first-line function is pushing to ship before the evidence contract is discharged.
- Add a *gate-quality-review* addendum — a self-review the gate does quarterly on its own past decisions, sampling for whether the gate's decisions correlated with subsequent incidents. Preview of exercise-04 (third-line audit of the gate).
- Compose the charter with the *NIST SP 800-37 Authorize step* from chapter 07 — where does the RMF vocabulary land in the charter, and where do you retain ISO 42001 vocabulary?
