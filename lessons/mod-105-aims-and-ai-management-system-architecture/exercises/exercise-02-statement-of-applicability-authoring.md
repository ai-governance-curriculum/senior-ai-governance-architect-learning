# exercise-02: Statement of Applicability authoring

**Estimated effort:** 3 hours

## Objective

Author a **full Statement of Applicability (SoA)** against ISO/IEC 42001:2023 Annex A — a row per Annex A control, each carrying the inclusion/exclusion decision, a defensible justification, the implementation reference to the enterprise AI control library (mod-102), and the implementation status — together with the SoA operating procedure that governs how the SoA is refreshed, versioned, approved, and audited over time. The SoA is the most-sampled artefact in a stage-2 AIMS audit; getting it right is the difference between "the auditor uses the SoA to structure their sampling" and "the auditor finds a nonconformity in the SoA before they finish reading it."

The point of this drill is the *discipline* — inclusion justifications that trace to identified risks or interested-party requirements, exclusion justifications that follow one of the three defensible patterns from chapter 05, `partially-in-place` rows that always carry a linked risk-treatment-plan entry and a residual-risk acceptance record, and every row bound to a real enterprise control library entry (not to the Annex A control itself, which is the *standard's* vocabulary, not the enterprise's).

This is the artefact the level-50 architect authors, the head of AI governance signs, and top management commits to under Clause 6.1.3. Legal and internal audit both review it. Every downstream operational activity — the internal audit programme, the management review inputs, the CAPA process — refers back to it.

## Prerequisites

- Chapter [`04-risk-and-impact-assessment-composition.md`](../04-risk-and-impact-assessment-composition.md) read — the risk register that feeds the SoA inclusion justifications is designed there.
- Chapter [`05-statement-of-applicability-and-annex-a.md`](../05-statement-of-applicability-and-annex-a.md) read and internalised — this exercise implements what that chapter designs.
- Chapter [`06-risk-treatment-plan-and-operational-clauses.md`](../06-risk-treatment-plan-and-operational-clauses.md) skimmed — the RTP linkage from partially-in-place SoA rows lands there.
- Exercise [`exercise-01-aims-scope-statement-drill.md`](exercise-01-aims-scope-statement-drill.md) completed — the SoA's scope-based exclusions cite the scope statement authored there.
- mod-102 chapters 01 (anatomy of a control-library entry) and 02 (family taxonomy) read — the SoA rows bind to control-library entries in the mod-102 shape.
- Primary reference: ISO/IEC 42001:2023 Annex A (the normative control set) and Annex B (implementation guidance). Paywalled — verify control identifiers and titles against the published standard; do not paraphrase.

## Scenario

Continue with **Halden Insurance Group** from exercise-01. Assume:

- The scope statement authored in exercise-01 is now approved.
- Halden's enterprise AI control library exists in the mod-102 shape, with the families `AIC-GOV-*`, `AIC-DAT-*`, `AIC-DOC-*`, `AIC-LOG-*`, `AIC-HOV-*`, `AIC-ROB-*`, `AIC-SEC-*`, `AIC-TRP-*`, `AIC-RSK-*`, `AIC-INC-*`, `AIC-MON-*`, `AIC-3PT-*` — well populated but not gap-free.
- A first-cut AI risk register exists (from the composition designed in chapter 04) with roughly 30 identified risks across the in-scope portfolio; the top ten are approved by the risk-and-impact review committee.
- The AI-and-ISMS integrated internal-audit programme is being planned for the FY next; the SoA is the audit's opening artefact.
- Halden's AIMS role register is stood up (from chapter 03 / mod-101), with clear owners for governance, risk, evaluation, and operations.

Assume the SoA is being authored for the first time — this is the artefact submitted to the certification body's stage-1 readiness review.

## Deliverables

1. **`soa.yaml`** — the full Statement of Applicability, one row per Annex A control.
2. **`soa-authoring-procedure.md`** — the one-page operating procedure that governs how the SoA is refreshed, versioned, approved, and used.
3. **`soa-audit-defence-brief.md`** — a two-page brief pre-answering the auditor's most likely challenges to the SoA.

## Requirements

### `soa.yaml`

Author one row per Annex A control. The SoA must be *complete* — Annex A control identifiers you do not include are audit findings by omission, so every published Annex A control must appear in the SoA, either as `included` (with a full justification and implementation reference) or as `excluded` (with a justification following one of chapter 05's three defensible exclusion patterns).

Because the exact Annex A control set is paywalled, walk the annex organised by *control objective* (per chapter 05):

- Policies related to AI
- Internal organisation
- Resources for AI systems
- Impact assessment of AI systems
- AI system life cycle
- Data for AI systems
- Information for interested parties of AI systems
- Use of AI systems
- Third-party and customer relationships

For each control objective, enumerate the specific controls it contains against the published standard (mark unverified identifiers `<!-- needs-research: verify against ISO/IEC 42001:2023 Annex A -->` and continue the row anyway using the standard's numbering as you have it). Do not skip a control because you cannot remember its identifier; use a placeholder and continue.

Each row (per chapter 05's five-column shape, in YAML):

```yaml
- annex_a_ref: A.<clause>.<subclause>          # verify against published standard
  short_title: <as-published>
  decision: included | excluded
  justification: >
    <paragraph — for included rows, cite the risk-register ids and / or the interested-party
    requirements the control addresses; for excluded rows, follow one of the three defensible
    exclusion patterns from chapter 05, name the trigger under which the exclusion would be
    revisited, and cite the artefact that carries the exclusion decision.>
  implementation_ref: AIC-<family>-<n> | n/a
  implementation_status: in-place | partially-in-place | planned | deferred | n/a
  # for partially-in-place / planned / deferred, additional required fields:
  rtp_link: RTP-YYYY-Qn-nnn
  residual_risk_accepted_by: <role or role-holder>
  residual_risk_acceptance_date: YYYY-MM-DD
  residual_risk_review_date: YYYY-MM-DD
  # optional but recommended
  operational_owner_role: <from Clause 5.3 role register>
```

**Inclusion-justification requirements**:

- Every included row's justification must name at least one of: a specific risk-register id (`AI-RISK-YYYY-nnn`); an interested-party requirement id (`IP-<category>-nnn`) or the regulatory obligation the control discharges (article / memorandum / clause); or an internal-issue id from the Clause 4.1 register.
- A justification of "we include this because Annex A says so" is not accepted.

**Exclusion-justification requirements**:

- Every excluded row's justification must follow one of chapter 05's three defensible patterns: **role-based** (the enterprise does not occupy the role the control applies to), **scope-based** (the activity falls outside the AIMS scope statement), or **not-applicable-in-context** (the activity type is genuinely absent from the enterprise, with an evidenced basis).
- Every exclusion must name (a) the source of the exclusion decision (legal memo, scope statement clause, risk-committee minute), (b) the trigger under which the exclusion would be revisited, and (c) the review cadence for the exclusion itself.
- Blanket exclusions ("this doesn't apply to us") are not accepted. Exclusion for convenience is not a defensible pattern.

**Implementation-reference requirements**:

- Every `included` row must reference an enterprise control library entry (`AIC-<family>-<n>`) that implements the Annex A control. Where the enterprise library does not yet carry a matching entry, the SoA row should either point at a *planned* library entry (with the entry-id reserved) or mark the implementation status as `planned` and link to the RTP entry that owns creating the library entry.
- One Annex A control may be implemented by more than one enterprise control library entry; state each. One enterprise control library entry may implement more than one Annex A control; that is fine, and the auditor benefits from seeing the reuse.

**Portfolio-specific requirements for Halden**:

The SoA must accommodate at minimum the following portfolio realities. Where a control's applicability depends on which system it applies to, use the *applicability filter* pattern (mod-104 chapter 07) at the enterprise control library level — the SoA row references the control-library entry, not the per-system evaluation.

- The pricing and reserving models — high-risk under EU AI Act Annex III insurance-and-life-and-health; Solvency II model-governance overlay.
- The claims-triage assistant — deployer role, third-party foundation model backend, EU and Norwegian deployments.
- The fraud-detection classifier fleet — group-wide P&C.
- The broker-facing conversational assistant — UK pilot, FCA guidance in view.
- The US policy-administration platform's embedded AI feature — per exercise-01, either in scope (with third-party governance overlay) or excluded (with the exclusion following the scope-based pattern and naming the interim governance).

### `soa-authoring-procedure.md`

One page. The operating procedure that governs the SoA over time. Must name:

- **The refresh cadence** — at minimum annually, and event-driven on any of: new system in scope; material change to an existing system; portfolio expansion into a new jurisdiction or a new AI product family; publication of an ISO/IEC 42001 or 42005 amendment; a nonconformity finding that revealed a missing control; a management-review decision to change coverage.
- **The versioning discipline** — how versions are numbered, how the version history is retained (immutable prior versions), and how the auditor is given traceability from the current SoA back to the SoA-that-was-in-effect at a given point in time (crucial for evidence sampling across a period, not a point).
- **The approval workflow** — who authors changes (the architect at level 50; the ai-governance-analyst at level 15 for maintenance), who reviews (the head of AI governance; the risk-and-impact review committee for material changes), who signs (top management commitment stays with the AI-accountable executive for material changes; the head of AI governance for maintenance changes), and what constitutes a "material" change.
- **The audit interface** — how the SoA is presented to the certification body at stage-1 (as-of-date snapshot; supporting artefacts identified per row); how the SoA is used in stage-2 sampling; how findings against the SoA feed back into the CAPA process (chapter 09).
- **The binding to the enterprise control library** — how a change to an enterprise control library entry (rename, split, deprecation) propagates to the SoA rows that reference it; who owns the propagation; what the SLA is.
- **The residual-risk-acceptance discipline** — every `partially-in-place`, `planned`, and `deferred` row carries a residual-risk acceptance with an accepting authority and a review date; the procedure names how those reviews are scheduled, who ratifies acceptance renewal, and what happens if a residual risk sits `partially-in-place` for two consecutive review cycles without progress (typically: escalation to the AI-accountable executive and re-open in the RTP).

### `soa-audit-defence-brief.md`

Two pages. The pre-written defence of the SoA against the auditor's most likely challenges. Must contain:

- **The three exclusions the auditor is likeliest to challenge** — the ones you would defend first, with your defence pre-written. Common candidates: GPAI-provider-scoped controls (excluded because Halden is a deployer, not a provider — the same role-decision pattern chapter 05 walks); training-data-lifecycle controls the enterprise does not fully own for the third-party foundation model (excluded on scope-based grounds with a compensating third-party-governance overlay); certain public-transparency controls scoped narrowly against the enterprise's non-public-facing use cases.
- **The three inclusions where the implementation is *thin*** — controls where you marked `partially-in-place` and the auditor will ask when the RTP entry closes. State the RTP entry, the accepting authority, and the review date, and pre-write the two-sentence answer.
- **The two rows where the enterprise control library does not yet exist** — where you have marked `planned` and the auditor will ask what "planned" means. State the planned control library entry id, the target authoring quarter, and the RTP entry.
- **The trace from a sample risk to a sample control** — pick one risk from the risk register that made it into the SoA's inclusion justifications, and walk in one paragraph how the auditor could trace risk → Annex A control → SoA row → enterprise control library entry → operational evidence (the audit trail chapter 05's pattern produces).
- **The refresh evidence** — a one-paragraph statement of when the SoA was last refreshed, what changed, and what the next scheduled refresh is.

## Starter guidance

- **Do not renumber Annex A.** The SoA's `annex_a_ref` must be the identifier the published standard uses. Renumbering breaks the auditor's ability to walk the standard alongside the SoA. Where you cannot confirm the exact identifier at authoring time, use the identifier you have and mark `<!-- needs-research: verify against ISO/IEC 42001:2023 Annex A -->` — but do not silently drop the row.
- **Every inclusion is a *promise*, not a checkmark.** Marking a row `included / in-place` is the enterprise's assertion that the control is fully implemented today. If it is only partially implemented, mark `partially-in-place` and link to the RTP entry that owns closing the gap. The auditor will sample against the assertion; a `included / in-place` row that turns out to be `partially-in-place` on sample is a *nonconformity*.
- **Every exclusion is a *decision*, not a shortcut.** The chapter-05 failure mode is exclusion for convenience. The three defensible patterns are role-based, scope-based, and not-applicable-in-context — every exclusion must fit one of these and cite the artefact that carries the decision.
- **Bind to the enterprise control library, not to Annex A itself.** The implementation reference is an `AIC-*` id, not the Annex A id repeated. The whole point of the SoA is to be the *bridge* between the standard's vocabulary and the enterprise's operational library.
- **`partially-in-place` without an RTP link is a nonconformity.** Every partially-in-place row must have `rtp_link`, `residual_risk_accepted_by`, `residual_risk_acceptance_date`, and `residual_risk_review_date`. Auditors specifically look for this.
- **Use the mod-104 applicability filter, not per-system SoA rows.** If a control applies only to the pricing models and not the fraud fleet, that per-system evaluation lives on the enterprise control library entry's applicability filter, not on the SoA row. The SoA is *AIMS-scoped* — one row per Annex A control, per AIMS.
- **Do not invent Annex A control identifiers.** Where the exact identifier is not to hand at authoring time, use the identifier you have and mark it for verification. Do not skip a control because you cannot remember its identifier.

## Acceptance criteria

- [ ] Every published Annex A control appears in the SoA — no silent omissions. Where identifiers were not verifiable at authoring time, the row is present with a `<!-- needs-research -->` marker on the identifier.
- [ ] Every `included` row's justification traces to at least one risk-register id, one interested-party requirement id, or one regulatory obligation. No "included because Annex A says so".
- [ ] Every `excluded` row's justification fits one of chapter 05's three defensible patterns (role-based / scope-based / not-applicable-in-context), names the source of the decision, and states the trigger for revisit.
- [ ] Every `included` row carries an `implementation_ref` to an enterprise control library entry (`AIC-*`) or a `planned` reservation. No blank refs.
- [ ] Every `partially-in-place`, `planned`, or `deferred` row carries `rtp_link`, `residual_risk_accepted_by`, `residual_risk_acceptance_date`, and `residual_risk_review_date`.
- [ ] The portfolio realities of Halden are represented — the pricing and reserving models, the claims-triage assistant, the fraud fleet, the broker assistant, and the embedded feature all show up in inclusion justifications, exclusion justifications, or applicability filter references.
- [ ] The SoA authoring procedure names the refresh cadence, the versioning discipline, the approval workflow (with the material-vs-maintenance distinction), the audit interface, the control-library binding propagation, and the residual-risk review discipline.
- [ ] The audit defence brief pre-writes the defence of three challenged exclusions, three thin inclusions, and two planned-implementation rows, and includes a risk-to-control-to-evidence trace paragraph.
- [ ] The SoA composes with the exercise-01 scope statement — no scope-based exclusion cites a scope clause the scope statement does not carry.

## Stretch goals

- **Author a *SoA-diff-report shape*** — the diff-report the ai-governance-analyst produces at each refresh, showing rows added, rows changed (with before/after), rows whose implementation status changed, and rows whose residual-risk review is coming due. Half a page plus a YAML sketch.
- **Sketch the *SoA machine representation*** — one paragraph on how the SoA lands in the mod-111 GRC-for-AI toolchain (schema; the join to the enterprise control library; how the tool renders the SoA to the certification body).
- **Handle the *acquisition case*** — what changes in the SoA when Fjord Analytics is brought into scope in FY2027? Sketch two-to-three new rows, one changed row, and any exclusions that stop holding. Half a page.
- **Author a *stage-1 walkthrough script*** — the fifteen-minute walkthrough the head of AI governance uses to present the SoA to the certification body's stage-1 reviewer. Bullet list of what to say, in what order, and which artefacts to have open.
- **Add an *AIMS glossary bridge*** — the two-column table that bridges Annex A control-objective titles as published in the standard to the enterprise's AI control library family names. Useful for the auditor and for internal onboarding.
