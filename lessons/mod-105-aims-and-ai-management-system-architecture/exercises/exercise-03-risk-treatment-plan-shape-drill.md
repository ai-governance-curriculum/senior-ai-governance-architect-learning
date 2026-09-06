# exercise-03: Risk treatment plan shape drill

**Estimated effort:** 3 hours

## Objective

Produce a **risk-treatment plan (RTP) shape** that composes ISO 31000 (parent risk-management framework), ISO/IEC 23894 (AI-specific risk guidance), ISO/IEC 42005 (AI system impact assessment), and ISO/IEC 42001:2023 Clause 6.1 (AIMS risk requirements) *without collapsing them into one another*. The deliverable is the schema for an RTP entry, the operating procedure that governs the RTP over time, and a seed set of ten-to-fifteen populated entries that exercise every treatment option and every failure-pattern countermeasure named in chapter 06.

The trap this drill teaches you to avoid is the *single-standard-flattening* mistake — writing an RTP that reads only against 42001 Clause 6.1.3 and loses the ISO 31000 framework mandate, the 23894 AI-specific taxonomy, and the 42005 impact-assessment linkage. The RTP is where the four-standard composition designed in chapter 04 actually lands in an operational artefact. If the RTP does not visibly compose them, the composition is not real.

This is the artefact the level-50 architect designs, the head of AI governance operates, and internal audit tests every audit round. Every RTP entry is an implicit promise about what the AIMS will do; every closed entry is an implicit assertion that the promise was kept and that residual risk is within appetite.

## Prerequisites

- Chapter [`04-risk-and-impact-assessment-composition.md`](../04-risk-and-impact-assessment-composition.md) read and internalised — the four-standard composition is designed there.
- Chapter [`06-risk-treatment-plan-and-operational-clauses.md`](../06-risk-treatment-plan-and-operational-clauses.md) read and internalised — this exercise implements what that chapter designs.
- Chapter [`05-statement-of-applicability-and-annex-a.md`](../05-statement-of-applicability-and-annex-a.md) read — the SoA-to-RTP linkage runs both ways and is tested here.
- Exercise [`exercise-02-statement-of-applicability-authoring.md`](exercise-02-statement-of-applicability-authoring.md) completed — some RTP entries in this exercise close SoA `partially-in-place` rows from exercise-02.
- Primary references: ISO 31000:2018; ISO/IEC 23894:2023; ISO/IEC 42005:2025; ISO/IEC 42001:2023 Clause 6.1 (Annex A referenced but not re-authored). Links in [`../resources.md`](../resources.md).

## Scenario

Continue with **Halden Insurance Group** from exercises 01 and 02. Assume:

- The scope statement (exercise-01) and SoA (exercise-02) are approved.
- The first-cut AI risk register (~30 risks) sits in the GRC-for-AI platform (mod-111 shape). The top ten risks are approved by the risk-and-impact review committee and have been through initial analysis.
- The AIA process (chapter 04, chapter 04's shape) has produced two AIAs — one for the claims-triage assistant (AIA-2026-034) and one for the broker-facing conversational assistant (AIA-2026-041). The pricing-and-reserving family AIA is in draft.
- The enterprise ERM function has an ISO 31000-shaped risk-management framework with a 1-5 likelihood/impact scale and a documented enterprise risk appetite. The AI risk appetite has been drafted as an extension (mod-106 will deepen this; assume the draft exists for this exercise).
- The Solvency II model-governance overlay applies to the pricing and reserving models. The AI-Act deployer and provider obligations apply to the two high-risk systems.

Assume the RTP is being *established* — this is the first shape submitted to the audit programme.

## Deliverables

1. **`rtp-entry-schema.yaml`** — the canonical schema for one RTP entry.
2. **`rtp-operating-procedure.md`** — the one-to-two-page operating procedure that governs the RTP over time.
3. **`rtp-seed.yaml`** — ten to fifteen populated RTP entries exercising every schema field and every treatment option.
4. **`rtp-standards-composition-brief.md`** — a two-page brief mapping RTP fields to the four contributing standards, so a reviewer can see that no standard collapsed the others.

## Requirements

### `rtp-entry-schema.yaml`

Extend the chapter-06 sketch into a *complete* schema. Every field named, every enumerated value listed, every optional-vs-required distinction stated, every foreign-key relationship named. At minimum the schema carries:

- `rtp_entry_id` (format: `RTP-YYYY-Qn-nnn`).
- `title` (short — one line the auditor can scan).
- `risk_register_links` (list of `AI-RISK-YYYY-nnn` ids). At least one required.
- `aia_links` (list of `AIA-YYYY-nnn` ids). Required when the risk was surfaced or amplified by an AIA per Clause 6.1.4 / ISO/IEC 42005.
- `soa_row_links` (list of Annex A control refs). Required when the RTP entry closes an SoA `partially-in-place`, `planned`, or `deferred` row.
- `enterprise_controls_implemented` (list of `AIC-*` ids with version). Required for `reduce` and `transfer` treatments.
- `treatment_option` (enumerated: `avoid` | `reduce` | `transfer` | `retain`) — the ISO 31000 / 23894 canonical four.
- `description` (paragraph — the specific action the RTP entry commits to).
- `owner_role` (from the AIMS role register per Clause 5.3) and `owner_person` (named).
- `support_roles` (list) — the roles the owner is authorised to draw on for delivery.
- `timing` (nested: `planned_start`, `planned_completion`, `progress_review_cadence`).
- `evidence_to_close` (list) — the artefacts whose existence at agreed quality closes the entry. Required at entry creation; countermeasure to chapter-06 failure pattern 1.
- `residual_risk` (nested: `pre_treatment`, `post_treatment_estimated`, `post_treatment_actual`, `acceptance_authority_if_above_appetite`). Required for every entry; countermeasure to failure pattern 2. Include the scale reference (`1-5 enterprise scale` per the ERM framework, per ISO 31000 Layer 1 in chapter 04).
- `treatment_31000_step` — which ISO 31000 process step this entry serves (typically `risk_treatment`; sometimes `monitoring_and_review` where the entry closes a monitoring gap).
- `treatment_23894_source_ref` — which 23894 AI-risk-source category the addressed risk sits in (bias in training data; model drift; adversarial manipulation; over-reliance on AI outputs; opacity of decision-making; misuse of AI capabilities; dependency on third-party AI; data leakage from generative models; unsafe emergent behaviour; and the enterprise-added categories).
- `regulatory_obligation_refs` (list) — the specific regulatory articles / memoranda / clauses the entry helps discharge (EU AI Act Article X; Solvency II model-governance expectation Y; NAIC Model Bulletin 2023 section Z; and so on). Optional but recommended.
- `dependencies` (list of other RTP-entry ids) — cross-links where one entry cannot close until another closes.
- `status` (enumerated: `open` | `in-progress` | `awaiting-review` | `closed` | `paused` | `abandoned`) with transition rules — `closed` requires evidence-to-close complete and residual-risk actual assessed; `abandoned` requires named authority and rationale.
- `change_log` (list of dated entries recording material changes to the RTP entry: retiming, scope change, owner change, closure).
- `review_at_management_review` (boolean) — true if the entry is on the management-review dashboard by design (all `avoid`, all `retain` above appetite, all `abandoned`, all `paused >6 months`, and material `reduce`/`transfer` entries). Countermeasure to top-management-visibility gaps.

For each field, state whether required or optional; for enumerated fields, enumerate the current allowed values; for extensible enumerations (23894 source categories; treatment options if the enterprise ever extends beyond the ISO 31000 four — which it should not), state the extension process.

### `rtp-operating-procedure.md`

One-to-two pages. Must name:

- **The entry-creation trigger** — the events that create a new RTP entry (risk newly identified above threshold; risk re-evaluated after AIA and now above threshold; SoA row marked `partially-in-place` / `planned` / `deferred`; audit finding requiring corrective action per chapter 09; management-review directive; regulatory change opening a new obligation).
- **The pre-creation checklist** — the mandatory fields at creation (all except post-treatment-actual and status transitions), and the pre-creation sign-off (who signs to open a new entry; usually the head of AI governance for material entries, the ai-governance-analyst for maintenance entries).
- **The progress-review cadence** — monthly at the AIMS operational review; quarterly at the risk-and-impact review committee for material entries; at every management review for the entries flagged `review_at_management_review`.
- **The evidence-to-close discipline** — how evidence is submitted, who verifies quality, who marks the entry closed. Independent verification (the head of AI governance, or a delegate not in the owner's reporting line, not the owner) is required for entry closure. Countermeasure to self-attestation.
- **The residual-risk-acceptance discipline** — when post-treatment actual exceeds pre-treatment estimated or the pre-agreed post-treatment estimate, the entry may not close without explicit re-acceptance by the accepting authority; where above appetite, escalation to the AI-accountable executive; where at appetite ceiling, ratification at the next management review.
- **The pruning discipline** — the quarterly review that closes entries whose original driver no longer applies (with note), re-authors entries whose scope has changed (with links back), and flags entries paused >6 months for management-review attention. Countermeasure to chapter-06 failure pattern 3.
- **The tooling and register** — the RTP lives in the mod-111 GRC-for-AI platform with defined access, versioning, and integration to the risk register and the SoA. Countermeasure to failure pattern 4 (personal spreadsheet).
- **The auditor interface** — how the RTP is presented to the certification body at surveillance and at recertification; how the auditor samples entries and traces to closure evidence; how findings against the RTP feed back into the CAPA process.
- **The 31000 / 23894 / 42005 traceability** — how the RTP demonstrates composition on inspection. Concretely: every entry names the 31000 step, the 23894 source category, and (where applicable) the AIA that surfaced or amplified the risk.

### `rtp-seed.yaml`

Populate ten to fifteen RTP entries against the schema, drawn from the Halden portfolio. Every entry fully populated — no `TBD`s, no bare fields.

Cover at minimum:

- **At least one `avoid` entry** — a decision to *not* undertake an activity (e.g. a pilot proposal that the risk-and-impact review committee declined to green-light because the residual risk exceeded appetite; explicit avoid decisions are what make the option visible).
- **At least six `reduce` entries** — the workhorse. Drawn from the Halden portfolio and spanning: the pricing/reserving family (Solvency II overlay drives some entries; EU AI Act Article 9 risk-management-system requirement drives others), the claims-triage assistant (foundation-model dependency; hallucination monitoring), the fraud-detection classifier fleet (fairness monitoring; false-positive impact), the broker assistant (transparency; UK FCA guidance), and the group-wide governance layer (competence plan gap; documentation gap).
- **At least one `transfer` entry** — a contractual or insurance-based transfer of a portion of risk (typical: the third-party foundation model provider's contractual warranties and indemnifications for a portion of the model-integrity risk). Note explicitly what is *not* transferred (accountability, evidence obligations).
- **At least one `retain` entry** — an explicit residual-risk-acceptance decision by named authority (typical: some part of hallucination residual risk after all reasonable controls, accepted at appetite ceiling by the head of AI governance with a scheduled review).
- **At least one entry that closes an exercise-02 SoA `partially-in-place` row** — cited by Annex A ref, with the evidence-to-close list matching the row's implementation gap.
- **At least one entry surfaced by an AIA** — with the AIA link populated and the treatment addressing an impact the AIA identified.
- **At least one entry with a `dependencies` link** — where one entry cannot close until another closes (typical: the RAG-corpus drift monitor cannot deploy until the corpus registry is stood up).
- **At least one `paused` entry** with a stated reason, and one entry due for management-review attention flagged `review_at_management_review: true`.

Every entry must carry `treatment_31000_step` and `treatment_23894_source_ref` — this is where the composition shows.

### `rtp-standards-composition-brief.md`

Two pages. Map the RTP schema fields to the four contributing standards so a reviewer can see the composition. Structure the brief in three parts:

- **The composition map** — a table with rows being RTP schema fields and columns being ISO 31000 / ISO/IEC 23894 / ISO/IEC 42005 / ISO/IEC 42001 Clause 6.1. Each cell either states which specific clause of the standard the field serves, or `n/a`. The `treatment_option` field serves 31000 and 23894 (both); `aia_links` serves 42005 and 42001 Clause 6.1.4; `residual_risk.acceptance_authority` serves 31000 and 42001 Clause 6.1.3; and so on. Every field must trace to at least one standard.
- **The composition narrative** — one page of prose walking the four-layer stack from chapter 04 and demonstrating each layer's presence in the RTP. Cite specific seed entries as worked examples ("entry RTP-2026-Q2-021 shows the 23894 source-category tagging in action — the addressed risk sits in the 'model drift' category, and the treatment applies the AIC-MON-062 corpus-drift monitor").
- **The failure-pattern countermeasures** — a table of chapter-06's four failure patterns and the specific RTP schema field(s) or procedure step(s) that counter each. Each countermeasure must reference a real seed entry that demonstrates it.

## Starter guidance

- **Every RTP entry is an *action*, not a *description*.** The risk register describes the risk; the AIA describes the impact; the SoA states the applicability decision. The RTP entry says *what will be done, by whom, by when, and what evidence closes it*. If your entry reads as a paragraph about the risk, you have the wrong artefact — refactor.
- **Do not fake the four treatment options.** If ten of your ten entries are `reduce`, either the enterprise is not making the `avoid` and `retain` decisions or you have not surfaced them. Include at least one `avoid` and at least one `retain` in the seed set; if the scenario does not obviously call for them, invent a plausible one from the Halden portfolio (a declined pilot; an explicit residual-risk acceptance after mitigation).
- **The 23894 source-category tagging is the composition trace.** Every entry must sit in one of the AI-specific risk-source categories from 23894. If you find yourself using "other", the taxonomy is not being applied; return to the source and re-classify.
- **AIA-linked entries must trace back cleanly.** The AIA process (Clause 6.1.4 / 42005) is not a parallel track — its outputs feed the risk register (which the RTP references) or directly feed new RTP entries. Every AIA-surfaced entry cites the AIA id in `aia_links`.
- **Residual-risk fields are load-bearing.** The chapter-06 failure pattern 2 is the RTP that closes without an actual residual-risk assessment. Every closed entry in your seed set must carry `post_treatment_actual` and — where above appetite — an acceptance record with named authority and date.
- **Do not use the RTP for the SoA's job.** The SoA is the AIMS-scoped applicability register; the RTP is the action tracker. RTP entries reference SoA rows via `soa_row_links` but never re-state the applicability decision. If you find yourself justifying an Annex A inclusion inside an RTP entry, the material belongs on the SoA row.
- **Use `<!-- needs-research: ... -->` for regulatory specifics you cannot verify.** Solvency II article numbers, EU AI Act paragraph refs, NAIC Model Bulletin section numbers — verify or mark.

## Acceptance criteria

- [ ] The RTP entry schema names every required and optional field, enumerates every closed enum, and states the extension process for extensible enumerations.
- [ ] Every schema field traces to at least one of the four contributing standards on the composition map, and the map is *complete* (no field without a trace).
- [ ] The operating procedure names the entry-creation trigger, the pre-creation checklist, the progress-review cadence, the evidence-to-close discipline, the residual-risk-acceptance discipline, the pruning discipline, the tooling and register, and the auditor interface — each of chapter 06's failure patterns has a named procedural countermeasure.
- [ ] The seed set carries at least ten fully-populated entries covering `avoid`, `reduce`, `transfer`, and `retain` treatments; at least one entry closes an exercise-02 SoA row; at least one entry is surfaced by an AIA; at least one entry has a `dependencies` link; every entry carries `treatment_31000_step` and `treatment_23894_source_ref`.
- [ ] No entry re-describes the risk itself — the entry is an action tracker, not a risk register.
- [ ] Every entry marked `closed` in the seed set carries `post_treatment_actual`; where above appetite, the acceptance authority and date are recorded.
- [ ] The standards-composition brief maps every schema field to at least one standard, includes a narrative walking the four-layer stack with worked seed entries, and cites a seed entry for each failure-pattern countermeasure.
- [ ] Every regulatory citation is verifiable against the primary source in `../resources.md` or is marked `<!-- needs-research: ... -->`.
- [ ] The RTP entries compose with the exercise-02 SoA — every `soa_row_links` reference points at a real SoA row from that exercise's shape.

## Stretch goals

- **Author a *pruning-review script*** — the ten-item checklist the ai-governance-analyst runs on each quarterly pruning review (which entries have been `paused` >6 months? which have `planned_completion` in the past without status update? which have never had an `evidence_to_close` verification? and so on). Half a page.
- **Sketch the *management-review RTP dashboard*** — the one-page dashboard the head of AI governance presents at the management review, drawing from the entries flagged `review_at_management_review`. State the sections, the metrics, and the decisions the dashboard is designed to elicit from top management.
- **Add a *treatment-effectiveness measure* to the schema** — the field that captures, after closure, whether the treatment actually reduced the risk as estimated. State how the measure is populated (independent post-closure review at the next annual risk-register refresh) and what happens when a treatment is judged ineffective (re-open as new entry; do not silently mark closed and forget).
- **Author a *cross-linked-obligation view*** — a small YAML fragment showing three RTP entries whose `regulatory_obligation_refs` all include EU AI Act Article 9 (risk management system for high-risk providers) and how the aggregation of the three entries would be presented to a regulator asking "how does Halden discharge Article 9 for the pricing and reserving models?"
- **Sketch the *RTP-to-audit-finding roundtrip*** — one paragraph on how a stage-2 audit finding (from chapter 09's non-conformity process) becomes an RTP entry, and how RTP entry closure produces the evidence the certification body's follow-up sampling looks for.
