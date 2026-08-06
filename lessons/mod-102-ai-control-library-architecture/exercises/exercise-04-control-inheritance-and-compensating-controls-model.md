# exercise-04: Inheritance Registry + Compensating-Control Worked Example

**Estimated effort:** 3 hours

## Objective

Design the two mechanisms that let one catalog serve heterogeneous systems without forking: **inheritance** (a parent scope satisfies the control on behalf of a child system) and **compensating controls** (an alternate mechanism achieves an equivalent outcome). You will draft (a) the initial *inheritance registry* Northbrook's platform team can register parent scopes into, and (b) one worked compensating-control entry in SSP shape for a specific real-shaped scenario.

The point is not to invent new mechanisms; the point is to build the *governance shape* around inheritance and compensation that keeps them defensible at audit time. Chapter 05's four inheritance-eligibility tests and six compensating-control fields are the substrate.

## Prerequisites

- Chapter [`05-control-inheritance-and-compensating-controls.md`](../05-control-inheritance-and-compensating-controls.md) read once.
- Chapter [`01-anatomy-of-a-control-library-entry.md`](../01-anatomy-of-a-control-library-entry.md) for the seven-field shape you will reference.
- Exercises 01-03 completed (you will re-use the catalog and profile from those exercises).
- Familiarity with the shape of a shared-responsibility inheritance model — the FedRAMP customer-responsibility matrix (`https://www.fedramp.gov/`) and the ISO 27001 group-certification pattern are useful reference shapes but should not be copied verbatim.

## Scenario

Two parallel Northbrook developments give you the raw material:

**Inheritance registry.** The Northbrook AI platform team has offered to publish attestations for two shared services: `ENT-PLATFORM-REG-08` (model registry, which captures training-data provenance for every registered model) and `ENT-PLATFORM-GRD-11` (guardrail service, which sits in front of every routed LLM call). Both teams are willing to commit to a change-propagation contract but neither has drafted one. You have been asked to publish the inheritance registry — the list of parent scopes that have passed the four eligibility tests and are consumable by downstream SSPs — with these two as the initial entries.

**Compensating-control worked example.** The retail-banking pricing team runs a legacy classical-ML pricing model on `Platform-X`, a bespoke on-prem stack. Platform-X does not accept the enterprise inference-log agent that satisfies `AIC-LOG-*` (audit-log controls from exercise-02 outcome #3). The team has proposed to compensate by piping Platform-X's native 15-minute-batched audit export into the enterprise SIEM and holding it in the compliance archive for the full retention window. The BU risk committee is willing to accept the residual reconstruction-window risk. You have been asked to draft the compensating-control entry so the pricing-team's SSP can pass architect review.

## Deliverables

Author three artefacts in a working directory of your choice:

1. **`inheritance-registry-2026-Q1.md`** — the initial inheritance registry, with two registered parent scopes.
2. **`ssp-fragment-pricing-legacy.yaml`** — the SSP fragment carrying the compensating-control block for the pricing team, in the shape chapter 05 defines.
3. **`review-memo.md`** — a two-page review memo to the pricing team and to the head-of-AI-governance covering the compensation decision, its residual risk, and the review cadence.

## Requirements

### `inheritance-registry-2026-Q1.md`

- A short preface stating the registry's purpose, who owns it (the architect), how a new parent scope is proposed for registration, and the change-propagation contract every registered scope must sign. The preface *is* the contract — write it as though it is being handed to the platform teams to countersign.
- For each of the two initial parent scopes (`ENT-PLATFORM-REG-08`, `ENT-PLATFORM-GRD-11`):
  - Parent-scope ID and owning team.
  - Which library controls the scope claims to satisfy (in-scope: at least `AIC-DAT-014` for the registry and `AIC-ENG-041` for the guardrail — reuse chapter 01 / chapter 03 IDs or your exercise-01 equivalents).
  - The inheritance `kind` per chapter 05 (`platform-provided` / `group-shared` / `contract-satisfied`).
  - The four eligibility-test results, each with a *specific* answer, not "yes":
    - **Test 1** — the scope's own control-satisfaction evidence (schema-URN + producer role + last-collected date). If the parent scope cannot yet produce this, note it and *do not register the scope yet* — mark it "pending eligibility".
    - **Test 2** — the applicability envelope of the parent scope, expressed with the same predicate shape as the child controls' applicability filters.
    - **Test 3** — the parent's evidence cadence and the strictest child-control cadence it needs to cover. If cadence is looser than any child, the row is a *fail*.
    - **Test 4** — the change-propagation contract text (notice period, notification channel, migration-guidance commitment). Write the paragraph, do not just say "TBD."
  - The **`delta_this_system`** template — the residual local work every inheriting SSP still owes, per chapter 05's "inheritance rarely means no local work" rule.
  - Registration date, next re-attestation date, and the architect who signed the registration.
- A **rejection log** section — at least one plausible parent scope you *did not* register (e.g., the ISMS awareness-training programme's blanket claim to satisfy every governance-family control), with the specific eligibility test it fails.

### `ssp-fragment-pricing-legacy.yaml`

The SSP fragment for the pricing team's control implementation of the audit-log control (the exercise-02 outcome #3 entry — call it `AIC-LOG-031` or your equivalent), in the shape chapter 05 defines. Must include:

- `control_id`, `implementation_status: compensating`.
- All six fields under `compensating`: `justification`, `alternate_mechanism`, `equivalence_argument`, `residual_risk` (with `accepted_by` resolving to `bu-risk-committee` or equivalent and a `review_cadence`), and `expiry`.
- The `equivalence_argument` must reference the control's *testing procedure* — chapter 05 says equivalence is tested against the testing procedure, not against the guidance. Cite the specific `AIC-LOG-031-Tn` test IDs the alternate mechanism satisfies.
- The `alternate_mechanism.artefacts` list must reference a real evidence artefact (a `platform-x-audit-export-config.md` file, a SIEM-ingestion attestation JSON) with a schema URN — even if the schema is a stub in exercise-03's registry.
- An explicit *comparison to inheritance*: one sentence stating why this is a compensating control and not an inheritance claim. (Hint: pattern C or pattern A? Neither, because …)

### `review-memo.md`

Two pages max. Structure:

- **The recommendation** — approve / approve-with-conditions / reject. State it in the first sentence.
- **The equivalence argument in your own words** — one paragraph. If you cannot summarise it to the head-of-AI-governance without reading the SSP, the SSP is not doing its job.
- **The residual risk in one paragraph** — with the specific size (a 15-minute reconstruction window on incident) and where it lives on Northbrook's risk register (preview of mod-106).
- **The three conditions** — expiry, review cadence, and the trigger that would force early re-review (e.g., a Platform-X vendor change, a regulator inquiry into audit-log completeness).
- **The pattern-check** — chapter 05 says: when two SSPs propose the same compensating control for the same reason, that is a signal to make the compensating mechanism a first-class control. Are there other Northbrook systems on Platform-X? Recommend a follow-up.
- **The escalation** — the specific escalation path if the pricing team refuses a condition. Not "escalate to the CRO"; a routed path with a named body.

## Starter guidance

- Write the inheritance-registry preface *first*. The registry entries are cheap once the preface commits to the specific eligibility test wording and the change-propagation contract.
- On Test 4 (change-propagation), a common failure mode is a contract that says "we will notify inheriting systems." Rewrite as: *what specific notice period, in what channel, with what migration guidance, at what point in the parent's change lifecycle*. Chapter 05's silent-failure warning is what this test defeats.
- For the compensating-control fragment, resist the temptation to weaken the `equivalence_argument` to "the alternate also captures logs." That is exactly the anti-pattern chapter 05 flags. Force the argument to describe *how the alternate satisfies each of the testing procedure steps*.
- On the review memo, do not paper over the residual risk. A 15-minute reconstruction window on tier-1 audit logs is a real risk. Naming it explicitly and routing it to the appetite framework is what makes the compensation defensible; hiding it is what makes it a finding.
- Do not create a new fifth eligibility test. Chapter 05 is deliberate about four. If the parent scope is legitimate, it will pass all four; if it fails one, the answer is not to add a test but to reject the registration.

## Acceptance criteria

- [ ] Inheritance-registry preface commits to the four-test bar and the change-propagation contract, written as a two-way agreement.
- [ ] Two parent scopes registered, each with all four eligibility-test answers, `delta_this_system` template, registration date, next re-attestation date, and architect signature line.
- [ ] Registry includes a rejection log with at least one rejected candidate and the specific test it failed.
- [ ] SSP fragment includes all six `compensating` fields per chapter 05, with `equivalence_argument` referencing specific testing-procedure test IDs.
- [ ] SSP fragment explicitly states why the situation is compensation and not inheritance.
- [ ] Review memo names a residual-risk owner, an expiry date, and a re-review trigger.
- [ ] Review memo includes the pattern-check follow-up (whether this compensation should become a first-class control).
- [ ] No entry uses "TBD," "operational reasons," or "similar mechanism" as a placeholder for concrete content.

## Stretch goals

- Add a *third* parent-scope candidate that is *contract-satisfied* (pattern C, per chapter 05): a foundation-model vendor whose SOC 2 report and ISO/IEC 42001 certification (assuming) claim to cover parts of `AIC-SEC-*` model-protection controls. Walk it through the same four tests — Test 4 in particular is where third-party contract shape (preview of mod-109) will bite.
- Extend the SSP fragment with a `waiver` block that would have been the *wrong* answer for the pricing team's situation, and note in one paragraph why compensating was the right taxonomy per chapter 05's three distinguishing questions.
- Draft the *dashboard* the head-of-AI-governance would look at once the registry is live: number of inheriting SSPs per parent, number of compensating controls per BU, number of parents pending re-attestation. One-paragraph description of the dashboard shape, not the actual chart.
