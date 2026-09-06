# exercise-02: Three-Lines-for-AI RACI Drill

**Estimated effort:** 3 hours

## Objective

Produce the **operating-model RACI** for a specified enterprise — the artefact chapter 02 designs against, ratified by the AI governance council (chapter 01), composed with the mod-107 assurance architecture and the mod-102 control library. The deliverable set is a machine-readable seat inventory across four lines, a control-family RACI that terminates on the `AIC-*` families, artefact-level RACIs for the pre-deployment gate and the AIMS Clause 9.3 management review, an X-counterparty coordination register, and an invariant-enforcement test plan the internal audit function could sample against.

The correctness spine is chapter 02's six invariants and its two failure modes — the shape-without-seats operating model and the R-equals-A collapse. Every design choice must be pinnable to an invariant (as the choice that enforces it) or to a failure mode (as the choice that defends against it). The RACI's discipline is that the shape is line-typed but every row is seat-named; a "the second line" cell is an unfilled cell.

## Prerequisites

- Chapter [`02-three-lines-of-defence-for-ai-operating-model.md`](../02-three-lines-of-defence-for-ai-operating-model.md) read once, with the RACI convention, the AI-specific X designation, the seat inventory, and the invariants marked.
- Chapter [`01-ai-governance-council-charter-and-decision-forum.md`](../01-ai-governance-council-charter-and-decision-forum.md) skimmed — the council ratifies the operating model as a reserved matter; charter amendments include seat additions and cross-boundary moves.
- Chapter [`03-role-descriptions-and-hiring-plan.md`](../03-role-descriptions-and-hiring-plan.md) skimmed — the hiring plan is what discharges invariant 4 (every seat staffed or has a plan-to-staff); this exercise pairs closely with exercise-03.
- The mod-102 control-library `AIC-*` family list — the RACI's control-family rows terminate on these identifiers.
- The mod-107 assurance-architecture design — the three-lines shape and the independence invariants the RACI enforces at seat level.
- The mod-105 chapter-05 documented-information index — invariant 5 (RACI composes with mod-105 documented information) tests against this index.

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose seat topology is **least familiar** to you.

- **A US regional bank** with a mature SR 11-7-aligned MRM function reporting to the CRO. The MRM function already carries validation-independence for the model-risk perimeter. Your RACI must place the MRM function inside the second-line seat inventory without duplicating or contradicting the AI-evaluation-engineer's A on `AIC-FAIR-*` / `AIC-ROB-*`. Where MRM's authority ends and the AI-evaluation-engineer's begins is the design decision; both sit on the second line, and the RACI must reconcile them without collapsing either.
- **A European B2B SaaS platform vendor** whose customers include public-sector deployers. There is a customer-facing seat (per the operating model's X designation for customer-facing assurance recipients) that consumes the vendor's assurance material. The RACI must name the enterprise-side coordinator per invariant 3. There is no MRM function; the AI-risk-engineer and AI-evaluation-engineer seats carry the full second-line quantification and assurance load.
- **A global healthcare payer / provider** with a clinical-safety committee reporting to the CMO. Clinical-safety review of AI-enabled clinical-decision-support is a first-line-or-second-line safety function per the mod-107 exercise-01 pattern. The RACI must position clinical safety inside the control-family map without collapsing the committee into a second-line seat. Where clinical safety is R vs C vs A is the design decision; the CMO is the accountable executive for clinical decisions and cannot be inside the second line.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`seat-inventory-v1.0.yaml`** — the machine-readable seat inventory across first / second / third / external lines, with the scenario-specific seat included.
2. **`control-family-raci-v1.0.md`** — the RACI table across all mod-102 `AIC-*` families the enterprise's control library carries. Every row has a single A and R != A for second-line-quality-gated rows.
3. **`gate-and-review-raci-v1.0.md`** — the artefact-level RACIs for the pre-deployment gate (mod-107 chapter 02) and the AIMS Clause 9.3 management review (mod-105 chapter 07).
4. **`x-counterparty-register-v1.0.md`** — the register of external counterparties (X seats) with the enterprise-side coordinator named per counterparty, per invariant 3.
5. **`raci-invariant-tests-v1.0.md`** — the test plan the internal audit function could sample against, one test per chapter-02 invariant plus one test per failure mode.

## Requirements

### `seat-inventory-v1.0.yaml`

The machine-readable inventory. Structure:

- **First-line seats.** At minimum: model-owner, product-owner, platform-lead, MLOps-lead, data-engineering-lead, enterprise-IAM-lead. Add scenario-specific first-line seats (e.g., clinical safety liaison for the healthcare scenario) as required.
- **Second-line seats.** At minimum: senior-AI-governance-architect (level 50), head-of-AI-governance (level 60), AI-governance-analyst (level 15), AI-risk-engineer (level 25), AI-evaluation-engineer (level 35), agentic-safety-engineer (level 40), AI-infra-security (level 35), CPO/DPO office. Add MRM function for the bank scenario; add clinical-safety-committee liaison as a second-line-adjacent seat for the healthcare scenario.
- **Third-line seats.** Internal-audit lead (AI scope), internal-audit engagement team, co-source partner where required.
- **External seats.** ISO/IEC 42001 certification body, ForHumanity independent auditor (where engaged), sector regulator examiner, statutory financial auditor, frontier-model-provider (attestation-consumer interface), customer-facing assurance recipient (B2B SaaS scenario).

Every seat carries: `id`, `level` (where the seat maps to a role-tree level), `line` (first / second / third / external), `role_packet_ref` (into the parent curriculum, per chapter 03), `staffed` (true / partial / plan-to-staff — invariant 4), `staffing_plan_ref` (into the exercise-03 hiring plan where `staffed != true`).

### `control-family-raci-v1.0.md`

The RACI table with one row per mod-102 `AIC-*` family. At minimum, the chapter-02 default families (`AIC-DATA-*`, `AIC-FAIR-*`, `AIC-ROB-*`, `AIC-SEC-*`, `AIC-EXPL-*`, `AIC-PRIV-*`, `AIC-HITL-*`, `AIC-AGENT-*`, `AIC-3PP-*`, `AIC-PMS-*`, `AIC-DOC-*`, `AIC-IAM-*`) with every column populated. Add scenario-specific families where they apply (e.g., `AIC-CLINSAFE-*` for the healthcare scenario if the enterprise's control library carries one).

Columns:

- **R (does the work).** One or more seat ids from the inventory. Prefer single R; where shared, state the split.
- **A (accountable).** Exactly one seat id. Multiple As is a defect.
- **C (consulted).** Seat ids the R consults during the work.
- **I (informed).** Seat ids the R informs on completion.
- **X (external).** External counterparties from the X register that participate; each X entry cross-references its enterprise-side coordinator in `x-counterparty-register-v1.0.md`.

Below the table, state per-row rationale where the R / A split is non-obvious or where the scenario-specific tailoring departs from the chapter-02 default. Every `A = R` cell for a second-line-quality-gated row is a defect the table must not carry; if the enterprise has a specific reason for such a collapse, state it and flag it as an invariant-2 exception the council must ratify.

### `gate-and-review-raci-v1.0.md`

Two artefact-level RACI tables.

**Pre-deployment gate RACI** — one row per gate artefact (evidence-contract discharge check; sequenced review — technical; sequenced review — legal / privacy; sequenced review — residual against appetite; sequenced review — third-party posture; gate decision-record signature). Columns: R, A, C, I. The gate decision-record signature is the exception to single-A per chapter 02; state it explicitly.

**AIMS Clause 9.3 management review RACI** — one row per management-review artefact (input pack; management-review meeting; management-review record; management-review-triggered changes). Columns: R, A, C, I.

### `x-counterparty-register-v1.0.md`

A register with one row per external counterparty. Each row carries:

- **Counterparty id.** Stable identifier.
- **Counterparty name and type.** (Certification body / independent auditor / sector regulator examiner / statutory auditor / frontier-model provider / customer-facing recipient).
- **Enterprise-side coordinator.** The named seat from the second-line inventory that coordinates the interface, per invariant 3.
- **Coordination cadence.** How often the coordinator engages the counterparty.
- **Contract / engagement reference.** Where the enterprise-side contractual or regulatory basis for the engagement is documented.
- **Chapter-08 boundary reference.** For counterparties the head-of-AI-governance carries at the leadership tier (regulator engagements, 42001 certifier), the reference into chapter 08's carrier discipline.

### `raci-invariant-tests-v1.0.md`

A test plan structured as one test per chapter-02 invariant and one test per failure mode. Each test carries:

- **Test id and name.**
- **Precondition.** The state that must exist for the test to run.
- **Action.** The specific sampling action the internal auditor takes.
- **Expected outcome.** The evidence the auditor should find (invariant held) or the defect the auditor would find (invariant broken).
- **Evidence capture.** What the auditor exports as evidence of the sample.

The plan must include:

- **Invariant 1 (single A per row).** Sample the RACI; find any row with multiple As.
- **Invariant 2 (A ≠ R for second-line-quality-gated rows).** Sample `AIC-FAIR-*`, `AIC-ROB-*`, `AIC-HITL-*`; check A ≠ R.
- **Invariant 3 (every X has enterprise-side coordinator).** Sample X entries in the RACI; find the coordinator in the X register.
- **Invariant 4 (every seat staffed or has plan-to-staff).** Cross-check seat inventory `staffed` field against the exercise-03 hiring plan; find unstaffed seats without a plan-to-staff reference.
- **Invariant 5 (RACI composes with mod-105 documented information).** Sample work products named in the RACI; find the corresponding AIMS documented-information item.
- **Invariant 6 (versioned and council-ratified).** Check version history; each amendment cites a council minute id.
- **Failure mode 1 (shape-without-seats).** Sample any RACI row; every R, A, C, I, X cell resolves to a seat id in the inventory (not "the second line" or "internal audit").
- **Failure mode 2 (R-equals-A collapse).** Verify invariant 2 test plus a review of any documented exceptions and their council-ratification records.

## Starter guidance

The seat inventory is the RACI's terms of reference. Draft it first, and draft it against the scenario's actual (or plausibly staffed) enterprise. A pristine inventory with every seat at target headcount is not credible — the RACI's invariant 4 tests against the `staffed` field, and the artefact must show which seats are currently staffed, which are partially staffed, and which are plan-to-staff. The exercise-03 hiring plan is what discharges the plan-to-staff cases; this exercise cross-references but does not author the hiring plan.

Multiple-A rows are the easy defect to spot. A row that reads "AI-evaluation-engineer and CPO/DPO are both A" is a drafting error where the author was trying to signal joint importance. Joint importance is C or C-with-veto, not joint A. Chapter 02's invariant 1 is uncompromising: exactly one A per row. If the enterprise cannot pick between two candidate As, the enterprise has not yet decided which seat owns the work product, and the RACI is not ready for ratification.

R = A for second-line-quality-gated rows is the harder defect and the one chapter 02's invariant 2 is designed against. The seat that does the fairness engineering (R) is not the seat that attests to the fairness engineering's independence-of-review (A). Where the enterprise's second-line has small headcount and one seat carries both, the RACI's invariant 2 test flags it as a defect; the correction is to grow the second line (chapter 03 hiring plan) or to split the seat's scope so that the same person's roles are R on system X but A on system Y.

X counterparties are easy to miss because they are external. The RACI's cells that read "42001 certifier" or "sector regulator examiner" look like completed cells but chapter 02's invariant 3 requires an enterprise-side coordinator for every X. The X register is where the coordinator is named; the RACI cell references the register. Without the register, the counterparty's engagement is coordinated ad-hoc — the failure mode chapter 02 warns against.

For the bank scenario, do not collapse MRM into the AI-evaluation-engineer. MRM is a distinct second-line function with its own regulatory grounding (SR 11-7); the AI-evaluation-engineer's A on `AIC-FAIR-*` and `AIC-ROB-*` is compatible with MRM's validation authority on model-risk-scoped systems if the two are cleanly reconciled. State the reconciliation explicitly: which systems are model-risk-scoped (MRM validates), which are AI-scoped-but-not-model-risk (AI-evaluation-engineer validates), and how the RACI handles systems that are both (MRM validates for the model-risk perimeter; AI-evaluation-engineer validates for the AI-specific perimeter; joint C between the two on the composition).

For the healthcare scenario, do not fold clinical safety into the second-line seat inventory. The clinical-safety committee reports to the CMO; the CMO is the accountable executive for clinical decisions and is not inside the AI-governance second line. The RACI's design decision is where clinical safety is R (on clinical-safety attestations for `AIC-HITL-*` and any `AIC-CLINSAFE-*` family), where it is C (on adjacent AI-governance decisions), and where the CMO is I (on non-clinical AI-governance items). The liaison seat inside the second-line inventory is the bridge, not the substitute.

## Acceptance criteria

- [ ] Chosen scenario is stated at the top of `seat-inventory-v1.0.yaml` and every artefact is coherent against it.
- [ ] `seat-inventory-v1.0.yaml` includes at minimum the chapter-02 default seats across four lines plus the scenario-specific additions; every seat carries `id`, `level`, `line`, `role_packet_ref`, `staffed`, and `staffing_plan_ref` where applicable.
- [ ] `control-family-raci-v1.0.md` covers at minimum the chapter-02 default `AIC-*` families plus scenario-specific additions; every row has exactly one A; A ≠ R for second-line-quality-gated rows; below-table rationale is present where the R / A split is non-obvious.
- [ ] `gate-and-review-raci-v1.0.md` includes the six-row pre-deployment gate RACI and the four-row AIMS Clause 9.3 management review RACI; the gate decision-record signature's multi-signatory exception is stated explicitly.
- [ ] `x-counterparty-register-v1.0.md` names every X seat from the inventory with the enterprise-side coordinator; every counterparty carries the chapter-08 boundary reference where the head-of-AI-governance carries the leadership-tier interface.
- [ ] `raci-invariant-tests-v1.0.md` includes a test per chapter-02 invariant (I1–I6) and a test per failure mode (shape-without-seats, R-equals-A collapse); every test names precondition, action, expected outcome, and evidence capture.
- [ ] Every design choice is pinnable to a chapter-02 invariant as enforcer or a failure mode as defence; a `pinning:` block or footnote in the seat-inventory YAML makes this explicit.
- [ ] The scenario-specific reconciliation (MRM composition; customer-facing X coordination; clinical-safety liaison shape) is authored explicitly, not left implicit.
- [ ] Every unverified specific — mod-102 family identifier wording, SR 11-7 clause language, EU AI Act article numbers — carries `<!-- needs-research -->` rather than a guessed value.

## Stretch goals

- **Second-line-scale sensitivity analysis.** Author a short analysis: what does the RACI look like when the enterprise's second-line headcount is halved from target? Which invariants break first, which seats become dual-hatted, and which control families lose an independent A? The analysis informs the exercise-03 hiring plan's sequencing.
- **X-counterparty-onboarding runbook.** Take one X counterparty (e.g., the 42001 certification body) and author the enterprise-side onboarding runbook the coordinator uses: initial engagement, contract shape, information-request-handling protocol, escalation path when the counterparty's information request cannot be satisfied within cadence.
- **Council reserved-matter test.** Take a plausible cross-line-boundary move (e.g., moving A on `AIC-3PP-*` from architect to head-of-AI-governance) and author the council reserved-matter proposal per chapter 01's charter-amendment discipline. The proposal is what invariant 6 tests against.
