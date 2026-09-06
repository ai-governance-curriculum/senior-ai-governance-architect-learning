# exercise-04: Ongoing Vendor Monitoring Schedule Design

**Estimated effort:** 3 hours

## Objective

Author the **ongoing-monitoring schedule** for a specific vendor (the exercise-01 through -03 vendor at hand) at its assigned tier, plus a **drift-trigger catalog** with wired detection, plus the enterprise's **vendor-incident routing playbook** applied to a concrete incident scenario, plus the **contract-renewal review** artefact you would run at renewal-minus-window for the same vendor. The deliverable is the machine-processable schedule artefact, the trigger catalog, the incident-rehearsal walk-through, and the renewal-review artefact.

The schedule is what turns the exercise-01–03 governance artefacts from a T=0 filing into a *living substrate*. If the schedule is under-specified, the vendor's twelve-month posture drifts silently; if the trigger catalog lacks wired detection, "as-needed" monitoring reverts to no monitoring; if the incident routing lacks an AIMS-non-conformity hook, the ISO 42001 auditor's Clause 10 sampling surfaces the gap.

## Prerequisites

- Chapter [`05-ongoing-vendor-monitoring-schedule.md`](../05-ongoing-vendor-monitoring-schedule.md) read once, with the four components (periodic re-attestation, drift-based re-assessment, vendor-incident routing, contract-renewal review), the per-tier default cadence, the trigger catalog (V1–V7 vendor-side, E1–E4 enterprise-side), the vendor register monitoring-state overlay, the operating-rhythm queries, and the six invariants marked.
- Chapter [`04-contract-template-controls.md`](../04-contract-template-controls.md) read once for the contract clauses that fire in monitoring (IN family, OP family).
- Exercises 01 (register), 02 (DDQ instance), and 03 (executed contract shape) as inputs — the vendor is the same across the four exercises.
- The mod-105 walk-through of ISO/IEC 42001 Clause 10 (nonconformity and corrective action) — the vendor-incident routing composes with the AIMS's non-conformity flow.
- The mod-106 chapter on the risk register — vendor incidents and drift signals feed risk-register updates.
- The mod-107 chapter 03 walk-through of ongoing assurance — the first-line evaluation signals (E3 trigger) originate here.
- The mod-108 chapter 02 walk-through of the audit-log substrate — monitoring events land in the substrate under specified event types.
- Access to the primary references — SR 23-4 (ongoing-monitoring phase); ISO/IEC 42001 Clause 9 (monitoring, measurement, analysis, evaluation) and Clause 10 (nonconformity and corrective action); EU AI Act Article 72 (post-market monitoring plan) and Article 73 (serious-incident reporting) <!-- needs-research: verify Article 73 timelines and categories against final text -->; GDPR Article 33 (72-hour notification); sector overlays. See [`../resources.md`](../resources.md).

## Scenario

Continue the enterprise scenario (A / B / C) and the vendor from exercises 01–03. In addition, the drill uses a **concrete vendor-incident scenario** for the incident-routing walk-through. Choose one of the following based on your vendor:

- **(I1) Vendor-model safety incident.** The vendor releases an out-of-band model update that regresses on a class of prompts the enterprise's use overlaps. The vendor's public disclosure is 8 hours after the enterprise's own monitoring detected degradation; the vendor's private notification to the enterprise arrives 14 hours after the enterprise had already engaged.
- **(I2) Vendor-side data breach.** The vendor discloses a security incident affecting the vendor's environment where enterprise prompts and completions may have been exposed; the vendor's notification arrives 68 hours after the vendor confirmed the incident, cutting close to the GDPR 72-hour deadline and beyond the enterprise's contractual expectation from exercise-03's IN-01 severity taxonomy.
- **(I3) Vendor subprocessor and jurisdictional change.** The vendor discloses that a subprocessor has been added in a jurisdiction the enterprise's DPA does not currently accommodate; the change becomes effective 30 days from notice; the enterprise's chapter 04 OP-03 subprocessor consent-and-notification clause is invoked.

State the scenario and the specific incident choice at the top of the deliverable.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`monitoring-schedule-<vendor-id>.yaml`** — the full monitoring schedule instance for the vendor at their tier: periodic re-attestation cadence and scope per the tier (with per-question refresh mapping from exercise-02's DDQ instance), drift-based re-assessment triggers active for this vendor, incident-routing pathway, contract-renewal review dates and pre-renewal window.
2. **`drift-trigger-catalog-v1.yaml`** — the enterprise-authored trigger catalog per chapter 05's shape (V1 through V7 vendor-side, E1 through E4 enterprise-side, plus any scenario-specific additions), each trigger with detection wiring (specific pipeline, subscription, sensor, or contract-obligated notification), scope, reopened artefacts, seat, and substrate event mapping. Detection wiring is the discipline test — no trigger without a wired detection.
3. **`incident-routing-walkthrough-<incident-id>.md`** — the vendor-incident routing playbook applied to the chosen incident scenario (I1, I2, or I3). Walk the incident from vendor notification (or detected divergence) through intake, triage, containment, AIMS non-conformity opening, risk-register update, regulator-facing report decision, vendor-engagement, and closure. Include the concrete timestamps, seats, artefacts produced at each step, and the closure output with lessons-learned.
4. **`renewal-review-<vendor-id>.md`** — the contract-renewal review artefact you would run at the tier-appropriate pre-renewal window (tier-3: renewal minus 6 months; tier-4: renewal minus 9 months). Include: inputs assembled (contract as-is, catalog delta, DDQ delta, monitoring history, risk register, business input, market scan, regulatory scan); decision options walked (continue-as-is / renegotiate / exit); recommended decision with rationale; sign-off seats.
5. **`register-monitoring-fields-<vendor-id>.yaml`** — the vendor register monitoring-state overlay populated for the vendor per chapter 05's field shape (attestation currency, open triggers, open incidents, contract-lifecycle dates, catalog-version delta, supply-chain evidence currency, register-health-score), and a companion **`operating-rhythm-queries.yaml`** file authoring the specific queries (daily, weekly, monthly, quarterly) with SQL-like pseudocode or YAML condition shapes that would surface the register state to the operating rhythm.

## Requirements

### `monitoring-schedule-<vendor-id>.yaml`

For the vendor at their tier:

- **Periodic re-attestation section.**
  - Cadence per tier per chapter 05 (tier-3: quarterly light + annual full; tier-4: monthly signals + quarterly deep-dive + annual comprehensive).
  - Quarterly light scope: enumerate the specific fields to re-attest (SOC 2 currency; ISO 42001 currency where applicable; model / classifier / dataset version currency; subprocessor list currency; evaluation-results snapshot; supply-chain evidence for any artefact ingested since last check).
  - Annual full scope: enumerate the full-DDQ-refresh reference (the exercise-02 template); contract-shape review against catalog version delta; independent-attestation review; vendor's own AI governance programme delta.
  - Seats per re-attestation event: which analyst runs it, which architect ratifies at what threshold, which head-of-governance signs off at what tier.
  - Substrate binding per event.
- **Per-question refresh mapping.** For at least eight of the tier-3 DDQ questions from exercise-02, specify the refresh cadence (per-question override to the category default) and the auto-recheck wiring (which URI, which hash monitoring, which vendor-published channel).
- **Drift-based re-assessment section.** Enumerate which triggers from the trigger catalog are active for this vendor and why (some triggers apply universally; some apply only to specific vendor classes; some apply only where a specific commitment was made).
- **Vendor-incident routing section.** Reference the incident-routing-walkthrough artefact; pin the primary Vendor Incident Contact, the escalation path, the AIMS non-conformity opening authority.
- **Contract-renewal review section.** The pre-renewal window opening date (renewal minus 6 or 9 months); the review inputs the enterprise will assemble; the decision cadence and seats.
- **Composition with enterprise TPRM.** State how the AI monitoring schedule composes with the enterprise's TPRM monitoring — shared attestation ingest, AI-specific overlays for the AI-specific fields.

### `drift-trigger-catalog-v1.yaml`

Per chapter 05's shape:

- **V1 — vendor model / classifier / dataset version change.**
  - Detection wiring: name the specific mechanism. Candidates: vendor RSS feed ingest into the enterprise's regulatory-and-vendor-change monitoring pipeline; vendor SDK's version-header monitoring at inference-time; contract-obligated notification per OP-01; vendor bulletin subscription; vendor-published changelog hash-recheck.
  - Scope: material change definition. Cite chapter 05's suggested shape (any change altering behaviour on the enterprise's evaluation set beyond a threshold; any change altering published safety commitments; any change altering rate-limits or pricing on the enterprise's usage tier).
  - Reopened artefacts: enumerate.
  - Seat and substrate binding.
- **V2 through V7 vendor-side, E1 through E4 enterprise-side.** Author per chapter 05's shape with specific detection wiring for each.
- **Scenario-specific additions.** At least two triggers your enterprise adds beyond chapter 05's catalog motivated by scenario-specific exposures. Candidates:
  - Scenario (A) bank: `V8_sec_or_occ_or_fed_supervisory_action` — a supervisory action affecting the vendor's regulated status or a peer bank's use of the same vendor triggers re-assessment.
  - Scenario (B) healthcare: `V8_fda_or_ocr_action` — an FDA warning letter or an OCR HIPAA enforcement action affecting the vendor triggers re-assessment.
  - Scenario (C) B2B SaaS: `V8_customer_regulator_action` — an action by a *customer's* regulator affecting a peer SaaS with the same vendor dependency triggers re-assessment.
- **Composition with the mod-108 substrate event schema.** Each trigger's substrate_events field references the specific event types in the audit-log substrate (per mod-108 chapter 02).

### `incident-routing-walkthrough-<incident-id>.md`

For the chosen incident scenario, walk the routing from vendor notification (or enterprise-observed signal) through closure. Use concrete timestamps (relative to a T=0 for the incident's detection or notification receipt) and named seats.

- **T=0.** Notification received (or enterprise signal detected). Substrate log entry: `family-1: vendor-incident-received` (or `family-1: vendor-public-incident-observed` for I1 if the enterprise detects before the vendor discloses). Seat: incident-response function.
- **T+1 hour.** Ack to vendor per contract commitment. Substrate log entry.
- **T+4 hours.** Triage complete. Impact assessment on enterprise systems, users, data. Regulator-facing assessment — is this reportable under EU AI Act Article 73, GDPR Article 33, sector-specific reporting? Seats: incident-response lead, AI governance analyst, technical assessment (sponsoring team + evaluation engineer), legal assessment, external-communications assessment (for tier-3+).
- **T+ variable hours.** Containment plan executed. For I1: failover to prior model version; enterprise-run additional evaluation to characterise the regression. For I2: preserve evidence; assess exposure of PII in vendor-held prompts and completions; potentially restrict enterprise usage of the vendor pending vendor's forensic. For I3: subprocessor objection assessment against OP-03; potential DPA amendment or engagement re-scoping.
- **T+ 24 hours to 72 hours.** AIMS non-conformity opened (per Clause 10) — head-of-AI-governance opens; analyst drafts. Risk-register update — risk engineer with analyst. Regulator-facing report drafted if reportable — legal lead, regulatory-affairs, head-of-AI-governance, accountable-executive sign-off. For I2 specifically, the GDPR Article 33 72-hour clock and the enterprise's contractual timeline may diverge; walk the specific timing carefully.
- **T+ ongoing.** Vendor engagement per IN-02 updates cadence. Post-mortem delivery per IN-03 within 30-60 days. Regulator-cooperation invocation if regulator requests.
- **T+ closure.** Root cause, contributing factors, remediation, preventive measures. Lessons-learned for DDQ (candidate: add a question to category 5 about out-of-band model updates), for contract catalog (candidate: tighten IN-01 timeline for critical safety incidents), for monitoring trigger catalog (candidate: extend E3 first-line signal detection to include the specific regression pattern the incident presented). Vendor-relationship disposition (continue / renegotiate / put-on-watchlist / exit). Substrate close event.
- **T+ 30 to 90 days.** Post-close review at head-of-AI-governance level: pattern detection against prior incidents; class-wide vendor-issue detection; monitoring-schedule updates.

For the incident-routing walk-through, be specific about *which* contract clauses fire, *which* substrate events emit, *which* AIMS Clause references bind, and *which* regulator-facing timelines attach. Where a specific timeline cannot be verified from primary source at authoring time, mark `<!-- needs-research: ... -->` rather than invent.

### `renewal-review-<vendor-id>.md`

Per chapter 05's shape at the tier-appropriate pre-renewal window:

- **Inputs assembled.** For each input from chapter 05 (executed contract, catalog delta, DDQ delta, monitoring history, risk register, business input, market scan, regulatory scan), author a paragraph on what the enterprise assembles for this specific vendor at this specific renewal.
- **Decision-options walked.** For each of continue-as-is, renegotiate, exit, state whether it applies here and why or why not; if it applies, sketch the specific move.
- **Recommended decision with rationale.** Pick one; justify against the criteria in chapter 05.
- **Sign-off seats.** Per chapter 05 (ai-governance-analyst + procurement lead as leads; architect reviews for tier-3+; accountable executive signs off on tier-4 renewals).
- **Substrate binding.** Where the renewal review record lands; the catalog-version-migration record if renegotiating.

If the recommended decision is *renegotiate*, sketch the specific catalog-version migration — which controls from your exercise-03 catalog v1.0.0 the executed contract already carries at v1.0.0 shape versus which controls at v1.2.0 (assume a hypothetical v1.2.0 with an improved IP-indemnity and a supply-chain-evidence tier tightening) the enterprise now wants. If the recommended decision is *exit*, sketch the exit-plan activation with chapter 04 EX-family clauses.

### `register-monitoring-fields-<vendor-id>.yaml` and `operating-rhythm-queries.yaml`

Register monitoring fields per chapter 05:

- Attestation: last_full_ddq_refresh, next_full_ddq_refresh, last_partial_reattestation, next_partial_reattestation, days_overdue_on_reattestation.
- Triggers: open_triggers, closed_triggers_last_90d, time_to_close_average.
- Incidents: open_incidents, closed_incidents_last_180d, time_to_ack_average, late_notifications_count.
- Contract lifecycle: contract_start, initial_term_end, current_renewal_end, days_to_renewal, pre_renewal_window_open, catalog_version_executed, catalog_version_current, catalog_delta_material.
- Supply chain: last_artefact_ingest, last_ml_bom_refresh, slsa_level_current, signature_verification_last.
- Register health score: attestation_currency_ok, open_trigger_count_ok, open_incident_count_ok, renewal_window_managed_ok, aggregate traffic-light.

Populate with defensible synthetic values consistent with your vendor's tier and the exercise-01 through -03 artefacts.

Operating-rhythm queries per chapter 05:

- **Daily.** Any tier-4 vendor with open incident? Any tier-4 vendor with open trigger older than 5 days?
- **Weekly.** All vendors with re-attestation overdue? All vendors with late notification in last 30d? All vendors with pre-renewal window open?
- **Monthly.** Tier-3+ vendors with catalog delta material? Vendors with incident pattern in last 180d (>=3 incidents)?
- **Quarterly.** Full population by tier and health score.
- **Scenario-specific additions.** At least two additional queries motivated by the scenario. Candidates:
  - Bank: tier-3+ vendors whose executed contract lacks the SEC 8-K-aligned notification shape.
  - Healthcare: tier-3+ vendors whose executed contract lacks the HIPAA breach-notification-timeline alignment.
  - B2B SaaS: tier-3+ vendors whose executed contract lacks the customer-flowdown obligations required by a specific customer segment.

Author each query as SQL-like pseudocode or a YAML condition shape (whichever fits your enterprise's tooling). The specific dialect matters less than that the query is *executable* — a query that reads as English but does not resolve to actual conditions against actual fields is not a query.

## Starter guidance

- The schedule is *tier-driven*, not *vendor-relationship-driven*. Do not adjust the cadence downward because the vendor is a friendly partner; the tier is what earns the cadence.
- Detection wiring is where the trigger catalog earns its keep. "The team monitors industry bulletins" is not a wired detection; "the enterprise subscribes to the vendor's release RSS at URI X, hashes the changelog daily, and files a substrate event on hash change" is a wired detection.
- The incident-routing walk-through is the most substantial artefact. Be specific about timestamps, seats, contract clauses, substrate events, AIMS Clause bindings, and regulator-facing timelines. The value of the drill is precisely the concrete rehearsal — a walk-through that reads as generic incident-response is not rehearsing the AI-specific overlay.
- The GDPR 72-hour and EU AI Act Article 73 timelines are the two most-cited regulator-facing timelines; both are worth being specific about in I2 or I3. Where a specific timeline cannot be verified from primary source, mark `<!-- needs-research: ... -->` rather than invent.
- The AIMS non-conformity opening (Clause 10) is where third-party incidents most frequently drop out of AIMS coverage. Enterprises open Clause 10 non-conformities for their own operational incidents but forget for vendor incidents. The drill's discipline is precisely: every AIMS-touching vendor incident opens a Clause 10 record, always.
- Renewal review at renewal-minus-6-months (tier-3) or renewal-minus-9-months (tier-4) sounds early; the discipline exists because renewal negotiations at renewal-minus-30-days lose leverage. Author the review for the *real* pre-renewal window; the "we'll get to it in the last month" pattern is the failure the discipline prevents.
- Operating-rhythm queries are what make the register a living substrate. Author queries that would surface state to a real operating rhythm (a Monday morning stand-up review, a monthly governance council, a quarterly executive-committee report). A query that never runs is a field that never populates.
- Do not conflate the AI-monitoring schedule with the SIEM's security monitoring or the SRE's uptime monitoring. Both live at day-to-day operational cadences; the AI-monitoring schedule lives at governance cadences (weekly to annual). Both feed the substrate; neither is the substrate.

## Acceptance criteria

- [ ] Scenario (A / B / C), vendor from exercises 01-03, and chosen incident (I1 / I2 / I3) are stated at the top of the deliverable.
- [ ] All five artefacts (`monitoring-schedule-<vendor-id>.yaml`, `drift-trigger-catalog-v1.yaml`, `incident-routing-walkthrough-<incident-id>.md`, `renewal-review-<vendor-id>.md`, `register-monitoring-fields-<vendor-id>.yaml` + `operating-rhythm-queries.yaml`) are present.
- [ ] Monitoring schedule instantiates the tier-appropriate cadence with specific field-level scope per re-attestation event and per-question refresh mapping for at least eight DDQ questions from exercise-02.
- [ ] Drift-trigger catalog authors detection wiring (specific pipeline, subscription, sensor, contract obligation) for every trigger; at least two scenario-specific triggers beyond V1–V7 / E1–E4.
- [ ] Incident-routing walk-through uses concrete relative timestamps, names seats per step, cites specific contract clauses that fire (from exercise-03 catalog), enumerates AIMS Clause bindings, and states regulator-facing timeline decisions.
- [ ] AIMS Clause 10 non-conformity opening is explicit in the incident routing.
- [ ] Renewal review artefact assembles all chapter 05 inputs for the specific vendor, walks all three decision options, recommends one with rationale, and identifies sign-off seats appropriate to the tier.
- [ ] Register monitoring fields populated with defensible values consistent with exercises 01-03 artefacts; register-health-score computed per your rule.
- [ ] Operating-rhythm queries authored as executable pseudocode; at least two scenario-specific queries added beyond chapter 05's defaults.
- [ ] Every unverified citation — EU AI Act Article 73 timeline, GDPR breach-notification specifics, SEC 8-K cybersecurity timeline, HIPAA breach-notification timeline, sector-rule identifier — is marked `<!-- needs-research: ... -->`. No invented timelines or article numbers.

## Stretch goals

- **Author the exit-execution playbook.** For the specific vendor at hand, sketch the exit-execution playbook if the exit posture from chapter 04 EX family fires. Include: data-export shape and window, fine-tune weight portability if applicable, TSA invocation for tier-4, replacement-vendor evaluation timeline, transition-services with parallel-operation window, cutover, retention of enterprise data at the departing vendor pending regulator-facing retention obligations, deletion evidence, substrate-recorded exit-plan artefact.
- **Rehearse a second incident scenario.** Pick a second incident from the (I1, I2, I3) options and walk it through the routing. Compare the two walk-throughs — where the routing shape is the same, where the specific timelines and seats differ (a safety incident engages the evaluation engineer more heavily than a data-breach incident; a jurisdictional change engages the privacy office more heavily than a version change).
- **Sketch the third-line audit engagement sampling this vendor's monitoring.** In one page, describe the third-line audit engagement that samples the enterprise's monitoring discipline for this vendor: sampling frame (attestation records, trigger closures, incident closures, renewal review artefacts), what the auditor checks per sample (calendar adherence, closure discipline, substrate integrity, exception discipline), what a finding versus a systemic finding is.
- **Compose with mod-110 post-market surveillance.** Sketch the specific interface between this vendor's monitoring schedule and mod-110's enterprise-wide post-market surveillance shape. The vendor's monitoring signals feed post-market surveillance; the post-market surveillance's regulator-facing reporting feeds the vendor's contract-remedy path if the vendor is the root cause.
- **Author a machine-readable representation of the register.** Draft the JSON Schema for `vendor-register-with-monitoring-fields.json` and validate your register instance against it. This previews the mod-111 GRC platform's vendor-register-ingest shape and the mod-108 chapter 06 OSCAL representation discipline.
