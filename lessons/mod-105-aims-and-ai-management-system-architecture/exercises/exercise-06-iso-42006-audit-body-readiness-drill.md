# exercise-06: ISO/IEC 42006 audit body readiness drill

**Estimated effort:** 3 hours

## Objective

Walk the Halden AIMS through a **stage-1 readiness review from an ISO/IEC 42006-conformant certification body's perspective**, and produce the artefact set that carries Halden through it — the certification-body-brief documentation pack, the stage-2 sampling plan the architect *predicts* the certification body will build, and the self-assessment against the eight audit behaviours from chapter 11. Then produce the twenty-item pre-certification readiness checklist the head of AI governance runs the week before the certification body arrives.

The trap this drill teaches you to avoid is the *documentation-without-operation* trap. Stage 1's classic failure modes (per chapter 11) are (a) missing artefacts, (b) artefacts that exist but have never been operated, and (c) scope-statement issues. This exercise makes you check for each in the artefact set you have built across exercises 01-05, and gives you the pre-certification-readiness discipline that lets the architect say — hand on the table — "we are ready for stage 2 in six weeks, not eighteen months".

This is the artefact the level-50 architect produces for the head of AI governance's use at the pre-audit gate. The head of AI governance runs the actual readiness review; the CRCO ratifies proceeding to stage 1; the certification body then conducts the review under its own methodology, which is what this exercise anticipates.

## Prerequisites

- Chapter [`11-designing-for-third-party-audit-iso-42006.md`](../11-designing-for-third-party-audit-iso-42006.md) read and internalised — this exercise implements what that chapter designs.
- Chapters [`08-performance-evaluation-internal-audit-and-management-review.md`](../08-performance-evaluation-internal-audit-and-management-review.md) and [`09-non-conformity-corrective-action-and-continual-improvement.md`](../09-non-conformity-corrective-action-and-continual-improvement.md) read — the internal audit programme, management review, and CAPA register are load-bearing at stage-1 review.
- Exercises 01-05 completed — this exercise operates on the artefact set those produced (scope statement, SoA, RTP, IMS integration design, AIA process and worked AIA).
- Working understanding of ISO/IEC 17021-1 (the parent conformity-assessment standard) — 42006 is its AI specialisation. Read at overview depth only.
- Primary references: ISO/IEC 42006:2025; ISO/IEC 17021-1; IAF Mandatory Documents relevant to management-system certification. Links in [`../resources.md`](../resources.md).

## Scenario

Continue with **Halden Insurance Group** from exercises 01-05. Assume:

- All exercise 01-05 deliverables are approved and in the IMS documented-information register.
- The AIMS is nearing certification target. The certification body has been selected (`<selected-body>` — for the exercise, assume DNV, BSI, or an equivalent IAF-accredited body; use `<CB>` as a placeholder if you prefer).
- The certification-body engagement letter is signed; stage 1 is booked for six weeks out; stage 2 is booked for four months out.
- Halden has run at least one full internal-audit round covering all AIMS clauses; the internal audit report is filed. At least one management review has been held. The CAPA register carries a modest but healthy set of open, in-progress, and closed items.
- One material AIMS deficiency is known to the head of AI governance and not yet closed: the third-party AI provider register (mod-109 shape) has three providers assessed but two more identified providers not yet assessed. The gap is on the RTP; whether it clears stage 1 or blocks it is a *live* question this exercise makes you address.

## Deliverables

1. **`stage-1-documentation-pack-index.md`** — the annotated index of the documentation pack Halden hands to the certification body ahead of stage 1.
2. **`predicted-stage-2-sampling-plan.md`** — the two-page prediction of the stage-2 sampling plan the certification body will build, based on the scope statement and SoA.
3. **`eight-behaviours-self-assessment.md`** — the self-assessment against the eight audit behaviours from chapter 11, marking each `strong`, `adequate`, `weak`, or `blocker` with evidence.
4. **`pre-certification-readiness-checklist.md`** — the twenty-item checklist the head of AI governance runs the week before the certification body arrives.
5. **`stage-1-defence-brief.md`** — the two-page brief on how Halden handles the known deficiency (the two unassessed vendors) at stage 1, and the anticipated stage-1 preconditions Halden expects.

## Requirements

### `stage-1-documentation-pack-index.md`

The certification body's stage-1 review begins from the documentation pack. Produce the annotated index Halden delivers. Structure:

- **Section 1 — Governance and Clause 4/5 artefacts.** The AIMS scope statement (exercise-01), the Clause 4.1 issues register, the Clause 4.2 interested-parties register, the AI policy, the top-management-commitment record (Clause 5.1), the AIMS role register (Clause 5.3), the IMS charter (exercise-04). For each artefact: filename in the register, version, date, approving authority, one-sentence description, and a pointer to the source-artefact deliverable in exercises 01-05.
- **Section 2 — Planning artefacts (Clause 6).** The risk-and-opportunity register, the AI risk register (initial + top-ten), the RTP (exercise-03), the SoA (exercise-02), the AIA process specification (exercise-05), the sample AIA (exercise-05 worked example), the AI objectives register (Clause 6.2 — assume it exists per the shape chapter 03 sketches; state its shape if you have not authored it).
- **Section 3 — Support artefacts (Clause 7).** The resources plan, the competence matrix, the awareness programme evidence (delivered modules; completion tracking), the communications plan and evidence (executed communications), the documented-information procedure, the retention policy.
- **Section 4 — Operational artefacts (Clause 8).** The operational-control procedures for the AI-system lifecycle (design, development, evaluation, deployment, operation, decommissioning), the change-management procedure, the third-party AI provider register (mod-109 shape) — with the deficiency flagged, the incident-management procedure (shared with ISMS per exercise-04) and evidence of at least one worked incident, the AIA record for the pricing model plus the two existing AIAs.
- **Section 5 — Performance evaluation (Clause 9).** The monitoring-and-measurement plan and evidence, the internal audit programme and completed report(s), the management-review agenda, minutes, and decisions record.
- **Section 6 — Improvement (Clause 10).** The nonconformity-and-corrective-action procedure, the CAPA register (open + closed items), the continual-improvement narrative (typically written into the management-review record).
- **Section 7 — Integration artefacts.** The IMS charter (exercise-04), the facet map, the shared-apparatus specifications, the operating-model diagram, the integrated-audit programme plan.

For each artefact, mark its *state* honestly: `implemented` | `partially-implemented` | `documented-not-yet-operated` | `documented-with-known-gap`. Any artefact not `implemented` needs a paragraph explaining the state and the RTP entry (or CAPA record) that owns closing the gap.

At the top of the index, a one-page **cover letter** — signed by the CRCO (top management) and the head of AI governance — that transmits the pack, states the certification scope requested, names the point of contact, and commits to timely response on stage-1 preconditions.

### `predicted-stage-2-sampling-plan.md`

Two pages. Predict the stage-2 sampling plan the certification body will build from the scope statement and SoA. The point is not to script the certification body's actual plan — the certification body owns that — but to anticipate it well enough that Halden is not surprised. Structure:

- **Systems sampled.** From the AIMS scope, the certification body will sample across product families, risk tiers, deployment jurisdictions, and organisational units. Predict at least eight systems the body will sample and why (drawn from the pricing and reserving family, the claims-triage assistant across DE and NO deployments, the fraud fleet across P&C sub-lines, the broker assistant, and one instance from the exclusion list to test that the exclusion holds). For each, note the specific SoA rows and RTP entries the sampling will touch.
- **Controls sampled deeper than others.** The chapter-11 sampling model is risk-based, not random. Predict which Annex A control objectives the body will sample deeper (typical: impact assessment of AI systems for the pricing family; data for AI systems for the fraud fleet; third-party and customer relationships across the portfolio given the mod-109 gap).
- **Interviews the body will conduct.** The head of AI governance; the CRCO (top-management commitment); the CISO (integration); the AI-risk engineer(s) responsible for the sampled systems; the AI-evaluation engineer(s); one or two random Tier-1 awareness targets (chapter 11 audit behaviour 6); internal audit lead; a sample business-unit head whose systems are sampled.
- **Artefacts the body will walk in real time.** The scope statement first; then the SoA row-by-row for the sampled controls; then the RTP entries for the partially-in-place rows; then the AIAs for the sampled systems; then the risk register linkages; then the monitoring evidence per mod-110; then the CAPA register.
- **The likely on-site duration.** State the auditor-day estimate you would predict for Halden's scale (mid-size multinational insurer, two-facet IMS, 30+ risks in scope, portfolio of ~10 systems with several high-risk classifications). Cite the 42006 duration model factors qualitatively (mark `<!-- needs-research: verify the specific 42006 auditor-day tables against the published standard -->`) and derive a *range* rather than a single number.
- **The likely audit-team competence composition.** ISO/IEC 42001 lead auditor + AI-competence specialist (per 42006) + insurance / financial services technical expert as a non-auditor specialist for the Solvency II overlay. State the team-composition posture Halden expects.

### `eight-behaviours-self-assessment.md`

The self-assessment against the eight audit behaviours from chapter 11:

1. **Reading the scope statement first, sampling by it thereafter.**
2. **Walking the SoA row by row.**
3. **Tracing risks to treatments to controls to evidence.**
4. **Reading the AIA process and sampling AIAs.**
5. **Sampling the internal audit programme and management review.**
6. **Interviewing role holders across the workforce.**
7. **Sampling third-party governance and incident-response.**
8. **Reading the CAPA register for trend and maturity.**

For each behaviour:

- Mark `strong` / `adequate` / `weak` / `blocker`.
- State the evidence — which artefact from exercises 01-05 (or from the scenario's stated existing state) provides it, and where the auditor will find it.
- Where marked `weak` or `blocker`, state what would need to close before certification and by when.

The mod-109 provider-register gap surfaces at behaviour 7 and needs an honest handling here — either it is a `weak` we mitigate with an RTP entry the auditor will accept, or it is a `blocker` we close before stage 2. Make and defend the call.

### `pre-certification-readiness-checklist.md`

Twenty items the head of AI governance walks the week before the certification body arrives. Each item: *what to check*, *what "ready" looks like*, *what to do if not ready*.

At minimum, cover:

- Scope statement version currency and approval signature.
- SoA version currency, approval signature, and every partially-in-place row's RTP link + residual-risk acceptance currency.
- RTP freshness — no `planned_completion` in the past without status update; no entry paused >6 months without management-review flag.
- Risk register currency — annual refresh completed or event-driven refresh triggered where needed.
- AIA coverage — every in-scope system that meets the AIA trigger has a current AIA on file; sampled AIAs open cleanly.
- Third-party AI provider register — every provider assessed; open assessments (the scenario's two) tracked and honestly disclosed.
- Incident register — every open incident is being worked; every closed incident has a post-incident review; the incident-management procedure has been executed in the last cycle.
- Internal audit programme — the last cycle's audit has been completed and reported; every finding has a CAPA record; no finding has been silently closed.
- Management review — the last review was held with real attendance, real inputs, real decisions, and real actions.
- CAPA register — healthy flow of open, in-progress, and closed items; no CAPA record has been abandoned without an authority note; effectiveness reviews are being conducted on closed items.
- Documented-information retention — the retention rules are being applied; superseded versions are retained per the policy.
- Awareness evidence — Tier-1 completion is at or above the target; Tier-2 and Tier-3 evidence exists for the sampled roles; a sample random-workforce interview would succeed on the "what would you do if you saw an AI concern" question.
- Competence records — the competence matrix is current; competence-record entries for the role holders the auditor will interview are up to date.
- Communications plan evidence — communications the plan committed to have been executed; the evidence is retrievable.
- Third-party governance — the vendor register carries every provider in scope, no discovered-and-not-registered providers.
- IMS integration — the facet map is current; the shared-apparatus specifications are being operated as specified (the joint management review has actually been held; the integrated audit programme is running).
- Regulator-facing documented information — the technical documentation for high-risk EU AI Act systems is at the required completeness; the Article 73 serious-incident channel is warm; any regulator correspondence in the current cycle has been logged.
- Physical / logical access — the certification body's auditors have access to systems, documentation, and role holders during the on-site week; NDAs are in place; secure-review facilities are available.
- Escalation path — the CRCO's calendar shows availability; a decision-taker is reachable if a stage-2 finding requires immediate response.
- Post-audit corrective-action capacity — the AIMS operating team has capacity in the 90-day post-audit window to respond to any minor / major nonconformities.

Where a check *would* fail today, name the RTP entry or CAPA record that owns closing it and the target date.

### `stage-1-defence-brief.md`

Two pages. Handle the known deficiency and anticipate the stage-1 preconditions.

- **The known deficiency — two unassessed third-party AI providers.** Frame it honestly: what the providers are (invent plausible names and roles — perhaps a translation-service AI used by the German claims operation and a licensed dataset provider used by the fraud fleet — but state that the exercise invents them), when they were identified, why they are not yet assessed, what the RTP entry that owns closing them says, and what the target closure date is. State whether Halden is (a) willing to defer stage 1 six weeks to close the gap, (b) willing to proceed to stage 1 and accept it as a stage-1 precondition to be closed before stage 2, or (c) willing to escalate to management review for a documented residual-risk acceptance and proceed as-is. Make and defend the call.
- **The three preconditions Halden most likely receives from stage 1.** Pre-write them, in the certification body's likely voice, and pre-write Halden's response to each. Common candidates: (i) the provider-register gap above; (ii) a request that Halden formalise the interface between the ISMS incident process and the AIMS Article 73 overlay in a specific procedure clause the current documentation does not carry; (iii) a request that the SoA justifications on a couple of role-based exclusions cite the specific legal memo rather than "legal advice".
- **The scope-statement-challenge scenario.** Anticipate the auditor challenge to the scope decisions from exercise-01 that were most consequential (the US policy-administration platform's embedded AI feature; the Fjord Analytics deferral; the R&D-prototype exclusion). Pre-write the two-sentence defence of each.
- **The stage-2 sampling posture Halden asks for.** State any preferences Halden wishes to communicate to the certification body about stage-2 planning — e.g. sequencing the DE and NO on-site visits, allowing remote interviews for role holders in the US entity, avoiding a week the enterprise has a scheduled all-hands. Distinguish between preferences the body may accommodate and preferences the body will refuse — do not assume the body will bend to convenience.

## Starter guidance

- **Read 42006 as the *auditor's rulebook*, not as compliance requirements for Halden.** Halden does not implement 42006; the certification body does. What Halden does is design the AIMS such that the auditor's required activities find evidence. The chapter-11 pattern.
- **Do not fake readiness.** The whole point of this drill is that the pre-certification readiness checklist catches deficiencies with time to fix them. If your self-assessment against the eight behaviours is all `strong`, either Halden is a magical enterprise or you are marking to the checklist rather than to reality. Rating things `weak` is honest and appropriate here.
- **Handle the mod-109 gap explicitly, do not paper over it.** Auditors have seen thousands of AIMS pre-audit packs. A honestly-flagged gap with a credible RTP entry is a far better story than a silent one the auditor discovers at stage 2. The stage-1 defence brief is where this discipline lives.
- **Predict the sampling plan; do not script the auditor's day.** The prediction is a *readiness tool*, not a compliance product. Its value is that it forces you to check every likely sampling target has evidence behind it — not that the certification body will actually use your prediction.
- **The scope statement is where the auditor starts.** If the exercise-01 scope statement has drifted since it was authored, this is the moment to detect that. The readiness checklist item on scope currency is not decorative.
- **Twenty checklist items is the target, not the ceiling.** If you find you need thirty, add them. If you find you have fewer than twenty and you are covering everything the eight behaviours require, you are missing coverage.
- **Use `<!-- needs-research: ... -->` for 42006 specifics you cannot verify** — the auditor-day tables, the exact competence-domain enumeration, the specific certification-decision-function requirements.

## Acceptance criteria

- [ ] The documentation-pack index is complete across all seven sections, every artefact marked with its state (`implemented` / `partially-implemented` / `documented-not-yet-operated` / `documented-with-known-gap`), and non-`implemented` artefacts have a RTP-or-CAPA link.
- [ ] The documentation-pack index has a top-of-pack cover letter naming the certification scope, the point of contact, and the top-management commitment to timely response.
- [ ] The predicted stage-2 sampling plan names at least eight systems, calls out the likely deeper-sampled Annex A control objectives, lists the interviews, walks the artefact-sampling order, and states an auditor-day range with the qualitative rationale.
- [ ] The eight-behaviours self-assessment marks each behaviour honestly and, for `weak` / `blocker` marks, names what would need to close and by when. The mod-109 gap surfaces at behaviour 7 and is handled explicitly.
- [ ] The pre-certification readiness checklist has at least twenty items covering the areas listed, and where any item would fail today, an RTP-or-CAPA link and target date is named.
- [ ] The stage-1 defence brief handles the known deficiency with a stated choice (defer / precondition / accept-as-residual) and defends it; pre-writes at least three likely stage-1 preconditions with responses; and pre-writes the scope-statement-challenge defence for the three consequential exercise-01 scope decisions.
- [ ] Every artefact reference in the pack index resolves to a real deliverable from exercises 01-05 (or to the scenario's stated existing artefact).
- [ ] Every 42006-specific claim (auditor-day tables, competence domains, certification-decision-function specifics) is either verifiable or marked `<!-- needs-research: ... -->`.

## Stretch goals

- **Author the *stage-1-opening-meeting agenda*** — the ninety-minute agenda the head of AI governance runs with the certification body on stage-1's on-site morning. Bulleted; ten to twelve items.
- **Sketch the *stage-2-nonconformity-response playbook*** — the procedure Halden uses on receipt of a major nonconformity: who convenes, how root-cause analysis is conducted, how the 90-day corrective-action plan is authored, who signs, how it is communicated to the certification body. One page.
- **Design the *surveillance-audit prep discipline*** — the once-per-cycle preparation the head of AI governance runs before each surveillance audit. What changed since the last audit, which findings from the last audit closed effectively, which artefacts have been refreshed. Half a page.
- **Author a *joint-audit MOU sketch*** — if Halden wants the ISMS and AIMS surveillance audits combined for efficiency, the memorandum the two certification bodies (or the same body if it holds both accreditations) would sign. Half a page.
- **Handle the *re-certification-year scope-expansion decision*** — three years hence, at re-certification, Halden considers bringing Fjord Analytics into scope. Sketch the one-page decision brief on whether to include Fjord in the re-certification cycle or run its integration off-cycle.
