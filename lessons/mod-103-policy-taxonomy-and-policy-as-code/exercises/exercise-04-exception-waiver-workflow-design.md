# exercise-04: Exception / Waiver Workflow Design

**Estimated effort:** 3 hours

## Objective

Design the enterprise **waiver register** for Northbrook — the machine-readable schema, the approval matrix, and the tooling contract — then instantiate three worked waiver records across risk bands to prove the design does not silently permit the shape mistakes from chapter [`04-exception-and-waiver-workflow.md`](../04-exception-and-waiver-workflow.md). Close by classifying three plausible deviations as **waiver**, **exception**, or **compensating control** using the one-line test — the three words are used interchangeably in the wild, and the level-50 architect must not.

## Prerequisites

- Chapter [`04-exception-and-waiver-workflow.md`](../04-exception-and-waiver-workflow.md) read once, with the seven-field record, the approval matrix, and the three-way distinction absorbed.
- Skim of [`../../mod-102-ai-control-library-architecture/05-control-inheritance-and-compensating-controls.md`](../../mod-102-ai-control-library-architecture/05-control-inheritance-and-compensating-controls.md) — the compensating-control shape a waiver almost always routes to.
- Forward reference to mod-106 for the risk-appetite bands and risk-register shape; forward reference to mod-104 for the cross-jurisdictional escalation branch.
- The Northbrook Financial scenario carried through the track.

## Scenario

Northbrook's GRC-for-AI platform (mod-111) is being scoped and the level-50 architect must supply the *waiver-register schema and process* the platform will implement. The head-of-AI-governance has been clear: the two failure modes to design against are (a) waivers issued casually enough to become de facto policy amendments, and (b) waivers refused so uniformly that teams route around the register into shadow-exception territory the architect only discovers at audit. Both failures have been observed in peer institutions; the artefacts you produce are the fence against both.

## Deliverables

Author four artefacts in a working directory of your choice:

1. **`waiver-register-schema.yaml`** — a JSON Schema (or YAML-encoded schema) formalising the seven-field record.
2. **`approval-matrix.md`** — the Northbrook-tuned approval matrix.
3. **`worked-waivers/`** — three complete waiver records, one per risk band.
4. **`three-way-classification.md`** — the one-page waiver / exception / compensating-control drill.

## Requirements

### `waiver-register-schema.yaml`

Formalise the seven fields from chapter 04 — `waiver_id`, `scope`, `justification`, `compensating_controls`, `residual_risk`, `expiry`, `approvals` — as a validating schema (JSON Schema draft 2020-12, or an equivalent YAML dialect). Enforce, at minimum, the following invariants; the schema must refuse to open a record that violates any of them:

- `waiver_id` matches `^WVR-\d{4}-\d{4}$`, is unique across the register, and is never reused after closure. Renewals use a new ID with a `supersedes` back-pointer.
- `scope` requires both a `policy_ref` / `standard_ref` / `control_refs` triple *and* a `systems` array. Neither axis may be empty; wildcard scopes (`business_units` alone with no `systems`) are rejected — chapter 04 calls out that "a waiver against 'a business unit' is not a waiver".
- `justification` is a non-empty string. A pattern-match against a small list of forbidden justification phrases (`bandwidth`, `not applicable`, `TBD`) surfaces a warning; the schema's `errorMessage` explains why.
- `compensating_controls` is required *unless* `residual_risk.within_appetite: true` is set AND the `accepting_authority` is populated with a role authorised to accept the band. The default is that omission is not permitted.
- `residual_risk` requires `description`, `risk_register_id` (`^RR-\d{4}-[A-Z0-9-]+$`), and `accepting_authority` (a role name; free-text names alone are rejected).
- `expiry` is an ISO date. The maximum lifetime is a function of the resolved risk band (see the matrix below); the schema encodes the per-band cap and rejects expiries beyond it. Missing or open-ended (`null`, `"TBD"`, `"pending vendor timeline"`) expiries are rejected.
- `approvals` is a non-empty array. Each entry requires `role`, `name_at_time`, and `date`. Self-approval — an approver whose `role` resolves within the requesting `scope` — is a validation error.

Include a short `# comments` header stating which draft of the schema this is and how the tooling loads it.

### `approval-matrix.md`

The Northbrook-tuned matrix (chapter 04's default plus your delta). One row per risk band (low / moderate / high-critical / cross-jurisdictional). Each row names: the required approvers (roles, not names), the notification list (roles informed but not signing), the max initial term, and the max renewal count before the register forces either a policy amendment or a permanent exception.

Two rules must be explicit and testable:

- **Approvers scale with risk band, not political weight.** The row is set by the residual-risk band, not by the requesting business's flagship status.
- **No self-approval.** State the validation predicate the tooling encodes (e.g., "an approver whose role resolves inside `scope.business_units` fails validation"). The predicate must be concrete enough for the platform team to implement without ambiguity.

At least one row must exercise the cross-jurisdictional branch (waivers whose scope touches more than one jurisdiction routing to legal in addition to the risk-band chain — forward reference to mod-104).

### `worked-waivers/`

Three YAML files, one per risk band:

- **`WVR-2026-0101-low.yaml`** — low-risk / short-life. Line-manager + ai-governance-analyst approval. 30-day expiry. Compensating control assigned to a role outside the requesting team. Realistic scenario: a documentation-cadence tightening not yet reflected in a self-serve template.
- **`WVR-2026-0184-moderate.yaml`** — mirror or adapt the chapter 04 worked example (`SYS-CX-CHAT-07` vendor-SDK case). Adjust the systems and clauses for your Northbrook slice. Architect + head-of-AI-governance approval per matrix, 120-day expiry within the moderate cap.
- **`WVR-2026-0203-high.yaml`** — high / critical. A fairness or model-behaviour control temporarily bypassed on a regulated output; the justification is a concrete technical constraint (vendor telemetry gap, evaluation-pipeline dependency) — *not* an invented incident. Head-of-AI-governance + CRO approval per matrix. Expiry within the 365-day high-band cap; a plausible compensating control that is *materially better than nothing* even where equivalence is impossible.

Every waiver: all seven fields populated; `WVR-<year>-<seq>` ID; compensating-control owner role is NOT the requesting team; `residual_risk.risk_register_id` references a plausible `RR-2026-<bu>-<seq>` entry; `expiry` within the band cap.

### `three-way-classification.md`

One page. Invent three plausible deviations at Northbrook (either from the topic list in exercise-01 or from the AI-inventory in mod-105 forward-reference). Apply the chapter-04 one-line test to each and classify correctly:

- **Outcome not achieved, temporarily → waiver.**
- **Outcome does not apply to this class of system → exception** (an applicability filter change on the mod-102 control, not a workflow item).
- **Outcome achieved through a different mechanism → compensating control** (mod-102 chapter 05 shape, not a workflow item).

Your three deviations must cover each of the three answers — one waiver, one exception, one compensating control. For each, name the artefact that carries the classification (a waiver record; an applicability-filter amendment on the control; a compensating-control record on the control) and the approving authority for that artefact. Close with a note on the *over-escalation* failure mode (teams routing a compensating-control case through the waiver workflow) and the *under-escalation* failure mode (teams treating a real waiver as a "we'll just update the applicability filter" applicability question).

## Starter guidance

- The seven-field record from chapter 04 is not a suggestion. If the schema you author cannot reject an open-ended-expiry record, the register will fill with them within two quarters.
- The compensating control's owner role is the single most common thing designers get wrong. A compensating control assigned to the requesting team is a conflict — the team most incentivised for the waiver to succeed cannot be the team producing the assurance that it is working.
- "The team is busy" is not a justification. If the requesting team is bandwidth-constrained, the honest artefact is a re-prioritisation conversation with the ExCo, not a waiver.
- If the classification exercise sends you back to re-write a waiver as an applicability change or a compensating control, you have understood the chapter. If none of the three deviations rewrites, look again — one of them is almost certainly mis-classified.

## Acceptance criteria

- [ ] `waiver-register-schema.yaml` validates a well-formed record and rejects: open-ended expiries, expiries beyond band cap, self-approval, missing compensating control (except when `residual_risk.within_appetite: true` + accepting authority), wildcard business-unit scope, and non-`WVR-<year>-<seq>` IDs.
- [ ] `approval-matrix.md` names approvers per band, encodes the no-self-approval predicate as testable pseudocode, and includes a cross-jurisdictional escalation row.
- [ ] Three worked waivers, each with all seven fields, `WVR-*` IDs, compensating-control owner ≠ requesting team, and `residual_risk.risk_register_id` populated.
- [ ] `three-way-classification.md` correctly diagnoses one waiver, one exception, and one compensating control using the chapter-04 one-line test.
- [ ] Neither of chapter 04's shape mistakes is committed anywhere in the deliverables (open-ended expiry; blanket-BU waiver or missing compensating control). If a temptation was resisted, the schema or matrix has a comment naming which invariant catches it.
- [ ] Every compensating control referenced carries an owner role and an evidence contract (or a pointer to one) — no decorative compensating controls.
- [ ] The lifecycle diagram from chapter 04 is either embedded or referenced with a one-sentence note on where the pre-expiry review (expiry-30) fires in the register's automation.

## Stretch goals

- Sketch the five standing queries an ai-governance-analyst would run each Monday to keep the register healthy: pre-expiry sweep (expiring within 30d), policy-review sweep (which clauses are accumulating waivers — a policy-amendment signal), accepting-authority sweep (who is accepting how much residual risk), system-portfolio sweep (which systems are heaviest in waivers), and risk-band sweep. Half a page.
- Design the integration point between the waiver register and the CI/CD PaC gate from chapter [`03-policy-as-code-enforcement-tiers.md`](../03-policy-as-code-enforcement-tiers.md): a gated build that consults the register and either annotates the failure ("gated by `WVR-2026-0184`, expires 2026-12-04") or gates on it explicitly. Half a page.
- Draft the "waiver overrun" finding shape mod-106's risk register receives at expiry+0 when a waiver lapses without renewal, closure, or promotion. Include the SLA and the escalation chain.
- Propose a template for the *renewal request* — the form the requesting team files when they want to renew, including the mandatory reflection on "what would need to change for this waiver to close permanently at the next expiry rather than renewing again?" One page.
