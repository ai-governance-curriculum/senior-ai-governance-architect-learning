# exercise-05: Role Engagement Contract Authoring

**Estimated effort:** 2 hours

## Objective

Author a fourth engagement contract — the architect ↔ **AI governance analyst (level 15)** interface — in the four-section shape chapter 06 establishes. Then stress-test all four contracts (the three from the chapter plus the one you authored) against six real-world request scenarios to prove they are executable.

Contracts you cannot execute against are decorative. This exercise turns decorative contracts into working ones.

## Prerequisites

- Chapter [`06-engagement-contracts-with-adjacent-roles.md`](../06-engagement-contracts-with-adjacent-roles.md) read once.
- Skim of [`CURRICULUM.md`](../../../CURRICULUM.md) — especially the ownership rule.
- Skim of the analyst-track scope: [`ai-governance-analyst-learning`](https://github.com/ai-governance-curriculum/ai-governance-analyst-learning) — read the README and the top-level curriculum plan to internalise what a level-15 analyst owns.

## Deliverables

Produce a directory containing:

1. **`contract-analyst.md`** — the architect ↔ AI governance analyst (level 15) contract.
2. **`contracts-stress-test.md`** — a scenario walk-through against all four contracts.

## Requirements

### `contract-analyst.md`

Follow the exact four-section shape from chapter 06:

1. **What the architect delivers to the analyst** — named artefacts + delivery cadence + which fields the analyst consumes.
2. **What the architect consumes from the analyst** — named artefacts + which fields the architect relies on + escalation if late / missing.
3. **What the architect defers to the analyst** — explicit non-scope. If the analyst is unhappy, they own the call.
4. **Shared artefacts and joint decisions** — named artefacts where the boundary is fuzzy on purpose. Both parties named. Written tiebreaker.

At a minimum, the contract must:

- Name the **schemas and templates** the architect delivers (intake, inventory, impact-assessment schema, model / system / dataset card schemas, framework-crosswalk template, jurisdictional tracker template, control-tracking template). Cadence and versioning explicit.
- Name the **draft artefacts** the architect consumes from the analyst (draft impact assessments, draft framework crosswalks, first-pass model / system cards, jurisdictional-tracker entries, control-tracking evidence links).
- Name at least **three explicit deferrals** — analyst-owned decisions the architect will not intervene in.
- Name at least **two shared / joint artefacts** with a written tiebreaker rule.
- Fit on **one page** rendered. Chapter 06's contracts do — yours must too.

### `contracts-stress-test.md`

Take the **six scenarios below**. For each, decide against all four contracts (analyst / risk engineer / evaluation engineer / head of AI governance) which route the request should follow, in what shape, and with what artefact.

If a scenario reveals a *gap* in one of the contracts (an artefact you had not listed, an ambiguity in the tiebreaker), name the gap and the contract patch you would make. Landing patches is the point.

**Scenarios:**

1. **The CRO asks for a monthly AI-risk dashboard.** Which contract(s) route this? Who drafts the dashboard schema, who populates it, who presents it to the CRO?
2. **A business unit wants to deploy a new customer-facing generative assistant next quarter and needs to know which controls apply.** Who runs intake, who fills in the impact assessment, who signs off, who owns the residual-risk call?
3. **A regulator (state DFS) sends an unsolicited questionnaire about the enterprise's use of AI in loan-underwriting decisions.** Who drafts the response, who reviews it, who signs and sends?
4. **A red team engagement finds a novel prompt-injection vector affecting three deployed systems.** Who owns the immediate remediation architecture, who updates the control library, who informs affected system owners, who informs the head of AI governance?
5. **An analyst finds three model cards that are 90 days out of date.** What is the architect's response? What is *not* the architect's response?
6. **The head of AI governance wants to commit to the audit committee that the enterprise will be ISO/IEC 42001-certified within 12 months.** What does the architect deliver into that commitment, what do they defer to the head, and what do they need from analyst / risk-engineer / evaluation-engineer?

For each scenario, output:

- **Primary contract routed against.**
- **Any secondary contracts implicated.**
- **Named artefact(s)** that satisfy the request.
- **Named owner(s)** per artefact.
- **Contract gap and patch** if the scenario surfaced one.

## Starter guidance

- The analyst contract is the **easiest to over-write**. Analysts execute schemas. The single sentence "the architect delivers the schema, the analyst fills it in" carries most of the contract. Everything else refines it.
- Deferrals are the hardest column. Force yourself to name three. If you cannot, you have probably specified the analyst's job too broadly.
- On stress-test scenarios 4 and 6, expect gaps. That is the *purpose* of the stress test.
- If a scenario cleanly runs against only one contract, that is a good sign. If it fans out across all four with no clear primary, either your primary contract is under-specified or the routing decision is a joint one that needs a written tiebreaker in the *primary* contract.
- Keep the contract to one page. Editing is the whole point.

## Acceptance criteria

- [ ] `contract-analyst.md` uses the four-section shape from chapter 06 with all four sections filled.
- [ ] Delivery section names at least six schemas / templates with cadence and versioning.
- [ ] Consumption section names at least four draft artefacts the architect consumes.
- [ ] Deferral section names at least three explicit deferrals.
- [ ] Shared / joint section names at least two artefacts with a named tiebreaker.
- [ ] Contract fits on one page rendered.
- [ ] `contracts-stress-test.md` routes all six scenarios against the correct contract(s) with named artefacts and named owners.
- [ ] At least one scenario surfaces a **gap and a patch** in one of the four contracts. If none of them do, you probably over-designed the contracts; revisit.
- [ ] Every scenario names a *primary* contract; joint routing is used only where genuinely required.

## Stretch goals

- Author a **fifth contract**: the architect ↔ agentic-safety-engineer (level 40) interface. This is the interface where the "architect defines the tripwire / rollback contract; that role owns the methodology" split matters most, and it is the most likely to matter at a frontier-model-consuming enterprise.
- For each of the six scenarios, add a **worst-case escalation branch** — what happens if the primary owner is on vacation or has resigned. Chapter 06's contracts do not include a fallback owner; adding one is a stretch discipline for real-org use.
- Convert the four contracts into a single **RACI matrix** (rows: canonical governance activities from CURRICULUM.md; columns: analyst / risk-engineer / evaluation-engineer / architect / head-of-governance). Note where the RACI conflicts with the contracts — RACI and prose contracts often drift and reconciling them is a real-org skill.
