# exercise-04: Article 73 Serious-Incident Reporting Workflow Design

**Estimated effort:** 2 hours

## Objective

Author the **standard operating procedure that discharges the enterprise's Article 73 serious-incident notification obligation** at a specified scenario — the repeatable workflow the enterprise will run against on the morning the first serious incident lands, not the bespoke content of any given filing. The deliverable is the SOP, its role matrix, its clock-start log schema, its parallel-obligations fan-out, and its post-incident-review template.

The role separation the SOP must respect is strict. The level-50 architect *writes* the SOP; the head of AI governance (level 60) *signs* each regulator filing produced against it; legal drafts the filing text; PR runs external comms; the AI governance analyst owns the single incident-fact record; SecOps runs any security-incident coordination. Every subsequent exercise-04 artefact in this module (the intake schema, the fan-out map, the review template) is designed to preserve this separation at execution time — the architect does not run any of the four downstream workstreams.

Get the design right and the enterprise files inside the statutory window with reconciled facts, a named signatory per obligation, and a closed-loop CAPA-plus-rescore that actually changes the residual-risk posture. Get it wrong and the enterprise discovers, on incident day, that it owns a scramble instead of a workflow — parallel filings diverge, the clock started at an ambiguous moment, and the regulator-facing stream was queued behind the operational fix until the statutory window had already expired.

## Prerequisites

- Chapter [`04-article-73-serious-incident-reporting-workflow-design.md`](../04-article-73-serious-incident-reporting-workflow-design.md) read once, with the four invariants and the three failure modes marked.
- Chapter [`01-eu-ai-act-article-72-and-the-enterprise-post-market-surveillance-shape.md`](../01-eu-ai-act-article-72-and-the-enterprise-post-market-surveillance-shape.md) — the PMS store is the upstream trigger for tier-4 classification and the substrate the evidence packet cites.
- The mod-105 chapter 09 walk-through of the corrective-and-preventive-action programme — every serious incident closes at least one CAPA record.
- The mod-106 taxonomy and appetite chapters — severity classification uses the taxonomy, and the post-incident review is required to trigger at least one risk rescore.
- The mod-107 chapter 03 walk-through of the ongoing-assurance programme — a serious incident is a named re-scoping trigger for the assurance plan.
- Access to primary references — the EU AI Act (Regulation (EU) 2024/1689) Article 73, GDPR Article 33, the SEC 2023 cybersecurity-disclosure rule (Item 1.05 8-K), and the sector-specific incident-notification regimes applicable to your scenario. See [`../resources.md`](../resources.md).

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose Article 73 exposure you understand least well; that is where the exercise will teach you most. State your choice at the top of the deliverable.

- **A US regional bank** (Northbrook Financial-style, per mod-101 exercise-02 and mod-102 exercise-01) with ~40 AI systems: fraud classifiers, document extraction, an internal RAG legal assistant, a customer-facing generative chat, third-party AI SaaS integrations. Colorado + NYC deployments; EU expansion planned — the EU expansion is what puts Article 73 on the map for the bank, alongside sector notification obligations to banking supervisors and GDPR obligations in Europe.
- **A global healthcare payer / provider** with clinical-decision-support pilots, patient-facing chat, coding automation, and utilisation-management AI. US federal HIPAA scope; EU AI Act relevance for European insurance subsidiaries; multiple US state deployments including Illinois and California. Article 73 exposure on European deployments; parallel obligations to HIPAA breach notification, FDA post-market safety reporting for any SaMD-classified AI, and state insurance commissioner notification regimes.
- **A B2B SaaS platform vendor** shipping GenAI-augmented HR-tech capabilities into enterprise customers across the US, UK, EU, and Singapore. Customers include public-sector deployments (subject to OMB M-25-21 shape) and financial-services enterprises (subject to SR 11-7 vendor-review shape). Article 73 exposure as a *provider* of high-risk AI systems into EU deployments, with heavy customer-contract notice-clause fan-out and public-sector customer-agency notification obligations layered on top.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`article-73-serious-incident-sop.md`** — the full SOP: intake, classification, escalation, roles, decisions, artefacts, timelines, external and internal communications. Written as an SOP the enterprise can run against on incident day — numbered steps, named seats, defined inputs and outputs — not narrative prose about what a good response would look like.
2. **`incident-role-matrix.md`** — a RACI-shape matrix naming every seat (architect, head-of-ai-governance, legal, PR, CISO/SecOps, `ai-risk-engineer`, `ai-evaluation-engineer`, `ai-governance-analyst`, product/business owner, and any scenario-specific seats — DPO, sector regulatory-affairs lead, clinical safety officer, customer-success lead) with responsibilities per SOP phase.
3. **`clock-start-log-schema.yaml`** — the schema for the *moment-of-awareness* log entry that the notification clock starts from. Timestamp, recorder-seat, evidence-of-awareness, source (PMS store / SOC / external notification / customer report / researcher disclosure), reproducibility hash to the raw telemetry or the external artefact.
4. **`parallel-obligations-fan-out.md`** — the fan-out map for simultaneous obligations. At minimum: EU AI Act Article 73, GDPR Article 33 (where personal data is involved), SEC materiality and Item 1.05 disclosure (where the enterprise is listed and the incident is material), a sector-regulator notification, customer-contract notifications. Shared incident-id contract; divergence-prevention discipline; per-obligation named signatory table.
5. **`post-incident-review-template.md`** — the template that closes the workflow: root cause, CAPA opened (mod-105 chapter 09), risk rescored (mod-106), taxonomy amendment considered (mod-106 chapter 07), assurance-programme trigger fired (mod-107 chapter 03), external-corpora review consideration (mod-110 chapter 06).

## Requirements

### `article-73-serious-incident-sop.md`

The SOP is a runnable document, not an essay. Structure it as numbered phases, each with named seats, defined inputs, defined outputs, and a defined close condition. At minimum, cover:

- **Intake.** The named triggers (PMS store classification, external notification, SOC severity-1 with AI nexus, researcher disclosure). The first-responder-on-call rota and its coverage discipline (follow-the-sun; no timezone stall). The mandatory-at-intake fields — incident id (single, sequential, authoritative), moment of awareness, source, initial-severity guess. State how the intake form makes the moment-of-awareness field impossible to skip.
- **Classification.** The `ai-risk-engineer` scoring step against the mod-106 taxonomy, the mapping from the taxonomy to Article 73's serious-incident categories, and the bounded time-from-intake within which the classification must land in the incident fact record.
- **Bridge convening.** The named seats on the standing bridge, the convene-within window, the timezone-coverage rota, and the standing-agenda template. State the delegation chain if the head of AI governance is unreachable.
- **Parallel-stream fan-out.** The operational-response stream, the regulatory-notification-prep stream, and the stakeholder-comms stream fire *simultaneously* from bridge-convene, not sequentially. State how the SOP prevents the regulatory-prep stream from being deferred.
- **Decisions.** The per-obligation named-signatory table — who decides *we file, on these facts, at this classification, at this time* for each obligation. No committees.
- **Evidence packet.** The template the architect owns and the analyst-owned incident fact record that supplies its substrate. State the version-citation discipline every filing must observe.
- **Filing.** Legal's drafting-and-submission step, the send-timestamp record, and the compliance-window closure event.
- **Follow-up filings.** The calendar-driven schedule for any preliminary-to-final follow-up obligation, cited to the specific regime and marked `<!-- needs-research: ... -->` if timeframe is not verified.
- **Post-incident review and close.** The close condition — regulator acks received, at least one CAPA record opened, at least one risk rescore filed, post-incident review artefact filed, stakeholder comms complete. The workflow does not close on the last regulator ack alone.

Every regulatory timeframe cited in the SOP is either verified from an authoritative primary source (with the source cited) or marked `<!-- needs-research: verify Article 73 timeframe for <serious-incident category> -->` (or the equivalent for the other regimes). Do *not* invent timeframes, clause numbers, or agency names.

### `incident-role-matrix.md`

A RACI-shape matrix (responsible / accountable / consulted / informed, or the R/A/C/I convention of your enterprise) with rows for every seat and columns for the SOP phases (intake, classification, bridge, ops-stream, regulatory-prep-stream, comms-stream, decisions, filing, follow-up, review, close). Every phase has *exactly one* Accountable seat. The A on the AI Act filing decision is the head of AI governance; the A on the incident fact record is the AI governance analyst; the A on the filing text is legal; the A on the SOP shape and evidence-packet template is the architect. The architect is C or I — not R or A — on the four downstream workstreams.

### `clock-start-log-schema.yaml`

A schema (YAML or JSON Schema) for the single log entry that starts the statutory clock. At minimum:

- `incident_id` — sequential, authoritative, unique across the enterprise.
- `moment_of_awareness_timestamp` — ISO-8601, UTC, with source-clock attribution.
- `recorder_seat` — the named on-call first responder who logged it.
- `recorder_person` — the individual, attributable.
- `evidence_of_awareness` — the artefact that establishes awareness (a specific PMS alert, an inbound customer email with its message-id, a SOC ticket reference, a media article URL).
- `source_channel` — enumerated: `pms_store` / `soc_alert` / `external_regulator` / `customer_notification` / `researcher_disclosure` / `media_report` / `internal_escalation`.
- `raw_evidence_hash` — content-hash reference to the raw telemetry or the external artefact stored in the data lake (chapter 03), so the moment-of-awareness claim is later reproducible against immutable evidence.
- `initial_severity_guess` — the first-responder's initial guess, replaced by the risk engineer's scoring at the classification step but preserved for audit.
- `mandatory_field_enforcement_note` — how the intake system prevents the entry from being submitted without this field populated.

Instrument the intake to force the field. State how (form validation, ticketing-system required-field, gated-workflow-transition).

### `parallel-obligations-fan-out.md`

For each regime in the fan-out table:

- The regime — regulation name, article or rule number, and jurisdictional scope.
- The trigger — what fact about the incident causes this obligation to fire.
- The signatory — the named seat that decides and signs.
- The clock — start-event and window, cited to primary source or marked `<!-- needs-research: ... -->`.
- The filing-content owner — typically legal, sometimes a sector regulatory-affairs lead.
- The fact-record binding — the specific version-cite convention that ensures this filing derives from the same incident fact record every other filing derives from.

Include, at minimum: EU AI Act Article 73, GDPR Article 33, SEC Item 1.05 8-K (where listed), one sector-regulator notification specific to your scenario, and customer-contract notice clauses. Add scenario-specific regimes as they apply (FDA post-market for healthcare SaMD; state insurance-commissioner regimes; banking-supervisor MRM notification expectations; public-sector customer-agency notification for the B2B SaaS scenario).

Document the shared-incident-id contract and the divergence-prevention discipline in prose at the end of the map. The rule is that every filing cites the fact record at a specific version, and no filing is permitted to source facts from any other place.

### `post-incident-review-template.md`

The template that closes the workflow. Sections at minimum:

- Incident id, dates, filed obligations, and signatories.
- Root-cause analysis, with evidence-of-analysis citations to the data lake.
- CAPA record(s) opened (mod-105 chapter 09) — CAPA-id, owner, target close date.
- Risk rescore filed (mod-106) — before-and-after residual-risk score, taxonomy category, evidence.
- Taxonomy-amendment consideration (mod-106 chapter 07) — did the incident reveal a category the taxonomy does not have; explicit decision, not silence.
- Assurance-programme trigger (mod-107 chapter 03) — has the ongoing-assurance plan been re-scoped; explicit decision.
- External-corpora review consideration (mod-110 chapter 06) — should this incident be considered for the enterprise's external calibration corpus, and should any external corpus entries change the enterprise's rescore.
- Lessons-learned distributed to first-line seats.
- Close-conditions checklist and the head-of-ai-governance sign-off block.

## Starter guidance

- Do not conflate the SOP with the filing. The SOP is repeatable; each filing is bespoke. The architect designs the SOP once and iterates it on version increments; the head of AI governance signs the individual filings against the SOP.
- The moment-of-awareness field is where clock-start ambiguity is designed away. Author its schema seriously; instrument the intake to force it; train the first-responder-on-call rota to populate it *before* any operational-response action.
- The parallel-obligations fan-out is where most enterprise incident workflows fail. Sequential filings produce divergent facts. The design move is one incident fact record, one owner, version-cited by every filing.
- Do not over-specify SOC-side detail. The interface between the AI Act workflow and the SOC-side runbook is chapter 05's territory; the SOC runbook depth is `ai-infra-security` (level 35). The architect specifies the interface — shared incident id, bridge co-attendance — not the SOC's internal steps.
- Do not over-specify legal-drafting detail. Legal owns filing-form content on the architect-designed evidence packet. The architect specifies the packet's schema and the fact-record-cite discipline; legal specifies the filing language, the regulator-counsel negotiation, and the follow-up correspondence.
- Do not invent Article 73 timeframes. Several tiers exist and they are easy to misremember. Verify from the regulation text or mark `<!-- needs-research: ... -->` and move on. The same discipline applies to GDPR Article 33's 72-hour clock (verify against current supervisory-authority guidance), the SEC Item 1.05 8-K window, and the sector-specific regimes.
- The four invariants (single incident id, clock-start logged, per-obligation named decision-authority, post-incident review feeds CAPA and rescore) each require an enforcing *design choice* in the SOP — not aspirational language. Name the choice and name the test that would detect a violation.

## Acceptance criteria

- [ ] Scenario is stated at the top; the SOP is coherent against it (Article 73 exposure is plausible; parallel obligations named match the sector and jurisdictions).
- [ ] `article-73-serious-incident-sop.md` covers every phase from intake to close as numbered runnable steps with named seats, inputs, and outputs — not narrative prose.
- [ ] `incident-role-matrix.md` names every seat listed in the deliverables section with exactly-one-Accountable per phase; the architect is C or I on the four downstream workstreams.
- [ ] `clock-start-log-schema.yaml` includes every required field and states how intake instrumentation forces the moment-of-awareness entry.
- [ ] `parallel-obligations-fan-out.md` covers at minimum Article 73, GDPR Article 33, SEC Item 1.05, one sector-regulator regime, and customer-contract notice clauses, each with signatory, clock, filing-content owner, and fact-record binding.
- [ ] `post-incident-review-template.md` requires an explicit CAPA record, an explicit risk rescore, an explicit taxonomy-amendment decision, an explicit assurance-programme trigger decision, and an external-corpora-review consideration.
- [ ] Each of the four chapter-04 invariants (single incident id, clock-start logged, per-obligation named decision-authority, post-incident review feeds CAPA and rescore) has a named enforcing design choice in the SOP.
- [ ] Each of the three chapter-04 failure modes (parallel filings with divergent facts, clock-start ambiguity, "we'll deal with the regulator part later") has a named architectural defence in the SOP.
- [ ] The regulator-notification-prep stream fires in PARALLEL with the operational-response stream from bridge-convene, not after; the SOP explicitly forbids deferral.
- [ ] Role separation is preserved: the architect writes the SOP; the head of AI governance signs the AI Act filing; legal drafts filing content; PR runs external comms; SecOps runs security-incident coordination. The architect does not run any of the four.
- [ ] Every Article 73, GDPR, SEC, sector-regulator, or agency-specific timeframe, clause number, or agency name is either verified from an authoritative primary source (cited) or marked `<!-- needs-research: verify <specific claim> -->`. No invented timeframes, clause numbers, or agency names.

## Stretch goals

- Add a **cross-border variant** for enterprises operating across multiple EU member states with different market surveillance authorities — the routing logic that determines which member-state authority receives the notification, the coordination discipline when the incident spans jurisdictions, and the interaction with any coordinating EU-level body.
- Author a **tabletop-exercise script** — a two-to-three-hour scripted incident (with injects) that walks the named seats through the SOP end-to-end, exercising the moment-of-awareness discipline, the bridge convening, the parallel-stream fan-out, and the close-condition checklist. Include the observer-scoring rubric.
- Specify the **integration with the SOC's incident-response bridge** — the shared incident id, the co-attendance seats, the fact-record-versus-SOC-timeline reconciliation contract, the hand-off point where the AI-specific workstream fires alongside the security-incident workstream. Cite chapter 05 as the interface owner and stop at the interface.
- Draft a **mock filing packet skeleton** — metadata only, no invented content — showing the sections a regulator filing produced against the architect's evidence packet would contain, the fact-record version-cite pattern, and the legal-review sign-off block. State clearly that this is a schema, not a filing.
