# Ongoing vendor-monitoring schedule — cadence, drift triggers, incident routing, renewal review

## Why this chapter exists

Onboarding is the discipline the enterprise gets to rehearse. Ongoing monitoring is where most third-party programmes silently degrade. A vendor onboarded at tier-3 with a discharged DDQ, a fully-executed AI-governance addendum, and a defensible supply-chain evidence bundle is a governance win at T=0. Twelve months later, if the enterprise has done nothing — no re-attestation, no drift check, no incident sweep, no renewal review — the vendor is at a completely different posture and the enterprise's evidence stack still describes T=0.

The failure is not that the enterprise did not intend to monitor. The failure is that "ongoing monitoring" was left as a sentence in the DDQ template and never turned into a schedule with dates, owners, triggers, and enforcement. Every vendor's monitoring calendar becomes a matter of the sponsoring team remembering; every renewal negotiation happens from a stale evidence position; every vendor incident gets triaged from scratch because no routing shape exists.

The level-50 architect designs the *schedule*. Not the calendar — the calendar is what the head of AI governance's team operates against; the schedule is the machine-readable template the calendar is generated from. The schedule has four components: periodic re-attestation (a fixed-cadence sweep of what has changed since the last attestation); drift-based re-assessment (event-triggered re-opening of specific worksheets); vendor-incident routing (the pathway from vendor notification to enterprise risk register, AIMS non-conformity, and if applicable, regulator-facing incident report); and contract-renewal review (the pre-renewal gate that decides continuation, renegotiation, or exit). Each component has a tier-scaled cadence, an owner, an evidence expectation, and a substrate binding.

This chapter designs the schedule. Exercise-04 walks the drill of authoring one for a specific vendor.

## What the monitoring schedule is, structurally

The schedule is:

- **A per-tier default cadence template.** Tier-1 through tier-4 each have a default cadence bundle the architect ships; specific vendors inherit the tier's cadence and can carry per-vendor overrides.
- **A trigger catalog.** Named events (vendor-side and enterprise-side) that reopen specific artefacts (worksheets, DDQ questions, contract clauses).
- **An incident-routing shape.** The pathway a vendor incident notification takes through enterprise triage, containment, AIMS non-conformity, risk register, and regulator-facing incident report where applicable.
- **A renewal-review shape.** The pre-renewal artefacts, the decision path (continue as-is / renegotiate / exit), and the pre-renewal window each tier requires.
- **A machine-readable vendor register overlay.** The register from chapter 02 carries monitoring-state fields (last attestation, next attestation, open triggers, open incidents, renewal date, days-to-renewal) that the operating rhythm queries.
- **A named operations owner.** The head of AI governance (level 60) operates the schedule; the architect designs it; specific analysts (level 15) run individual artefacts against the schedule; the AI evaluation engineer (level 35 peer) and the AI infrastructure security peer (level 35 peer) run specialist checks; the AI-accountable executive receives escalations.

## What it is not

- **A one-artefact-per-year attestation letter.** Annual attestation is a floor for tier-1 only. Higher tiers demand a mix of continuous, quarterly, and event-triggered monitoring; a static annual review does not catch drift.
- **A live-dashboard substitute for evidence.** A vendor's trust portal or live-metrics dashboard is *supplemental* to the substrate evidence. Point-in-time evidence is what the substrate holds, refreshed on cadence.
- **A vendor-relationship-manager's calendar.** The relationship manager may operate parts of the schedule; the schedule itself is a governance artefact, not a relationship function.
- **The whole exit posture.** Contract-renewal review is the *decision point*; the exit posture (chapter 04, EX family) is the *capability*. The schedule fires the decision; the capability enables the follow-through.

## The four components

### Component 1 — Periodic re-attestation

The vendor confirms, on a fixed cadence, the continued accuracy of a subset of the DDQ answers, the continued currency of specified evidence, and any material changes since the last attestation.

Tier-scaled default cadence:

```yaml
periodic_reattestation:
  version: 1.0.0

  tier-1:
    cadence: annual
    scope: >
      Confirm continued accuracy of DDQ; refresh SOC 2 (or equivalent) currency;
      update subprocessor list; disclose any incidents in the period.
    seat: enterprise-tprm-analyst with ai-governance-analyst sign-off
    substrate_binding: vendor-register/{vendor_id}/attestation/{iso_date}

  tier-2:
    cadence: semi-annual
    scope: >
      Tier-1 scope plus: refresh vendor model card / usage guide currency;
      re-answer DDQ questions with per-question refresh_cadence <= 6 months;
      disclose material changes in vendor's own subprocessor / hosting / DPA schedule.
    seat: ai-governance-analyst with second-line reviewer
    substrate_binding: as tier-1

  tier-3:
    cadence: quarterly (light) + annual (full)
    quarterly_scope: >
      Refresh currency of attestation letters; verify published-URI hashes for
      auto-recheckable DDQ answers; refresh evaluation-results snapshot;
      verify subprocessor list; refresh model / classifier / dataset versions
      the enterprise depends on; refresh supply-chain evidence for any artefact
      ingested since last check.
    annual_scope: >
      Full DDQ refresh (all questions re-answered); full contract-shape review
      against catalog version delta; independent-attestation review;
      vendor's incident history and post-mortem review; vendor's own AI
      governance programme delta.
    seats: ai-governance-analyst + ai-evaluation-engineer (level 35 peer) for
      evaluation-specific fields + ai-infrastructure-security (level 35 peer)
      for supply-chain and version fields
    substrate_binding: as tier-1

  tier-4:
    cadence: monthly (signals) + quarterly (deep-dive) + annual (comprehensive)
    monthly_scope: >
      Auto-recheck of published URIs; version-change ingest; incident sweep
      (public incident history + vendor bulletins); subprocessor delta;
      sanctions / export-control screening; regulatory-change scan for the
      vendor's jurisdiction.
    quarterly_scope: >
      Deep-dive re-attestation on defined fields; DDQ delta review; contract
      compliance review (were IN, EV, EX clauses exercised as expected in
      the quarter?); financial-condition sanity check.
    annual_scope: >
      Full DDQ refresh; full contract-shape review; independent-audit engagement
      or SOC-2-sub-service-organisation refresh; on-site (or remote formal
      equivalent) review; executive-relationship check-in; TSA currency
      review; escrow-arrangement currency review (where applicable).
    seats: ai-governance-analyst (lead) + ai-evaluation-engineer + ai-infrastructure-security
      + enterprise-tprm-analyst + legal-contract-analyst + head-of-ai-governance sign-off
    substrate_binding: as tier-1
```

Two design choices the architect defends:

- **Cadence is fixed, not "as needed."** "As needed" is where monitoring dies. The cadence lands as calendar events; slippage produces a substrate log entry the operating rhythm sees.
- **Cadence is *layered*.** Tier-4 has monthly signals *and* quarterly deep-dives *and* annual comprehensive reviews. Deeper reviews do not substitute for lighter reviews at higher frequency; each layer catches a different class of drift.

### Component 2 — Drift-based re-assessment

Drift-based re-assessment is *event-triggered* rather than fixed-cadence. Named events reopen specific artefacts. The trigger catalog is what makes drift-based monitoring executable — without a named trigger, the "reassess if something material changes" instruction is a wish.

The trigger catalog:

```yaml
drift_triggers:
  version: 1.0.0

  # Vendor-side triggers
  V1_model_version_change:
    detection: >
      Vendor bulletin ingestion (subscribed to vendor's model / release RSS
      or API); API-response version-header monitoring (where exposed);
      contract-obligated notification (OP-01 from chapter 04).
    scope: >
      Material model / classifier / dataset version updates as defined in
      contract (typically: any change altering behaviour on the enterprise's
      evaluation set beyond a threshold; any change altering the model's
      published safety commitments; any change altering rate-limits or
      pricing on the enterprise's usage tier).
    reopens:
      - DDQ category 3 (safety-evaluation-quality) — full re-answer
      - DDQ category 5 (version-change-update) — full re-answer
      - the enterprise's own evaluation run against the new version (mod-107 ch03)
      - supply-chain evidence bundle refresh (chapter 07) if artefact-delivered
    seat: ai-evaluation-engineer + ai-governance-analyst
    substrate_events: [family-1: vendor-version-change-detected]

  V2_subprocessor_list_change:
    detection: >
      Vendor's DPA-obligated notification (OP-03 from chapter 04); vendor's
      published subprocessor page hash-recheck.
    scope: any addition, removal, or change to the vendor's subprocessor list
    reopens:
      - privacy-office review of the added subprocessor
      - DDQ category 2 subprocessor question refresh
      - contract objection right assessment (OP-03)
      - jurisdictional-exposure dimension refresh (chapter 02 tiering)
    seat: privacy-office-analyst + ai-governance-analyst
    substrate_events: [family-1: vendor-subprocessor-change-detected]

  V3_incident_notification_received:
    detection: >
      Vendor's IN-01 notification landing in the enterprise's designated
      Vendor Incident Contact.
    scope: any severity level
    reopens:
      - vendor-incident routing (component 3 below)
      - DDQ category 6 (incident-exit-contract) refresh on close of incident
    seat: incident-response function; ai-governance-analyst
    substrate_events: [family-1: vendor-incident-received]

  V4_material_public_incident:
    detection: >
      Enterprise's monitoring of vendor's public incident channels, security
      research disclosures, industry bulletins, and press coverage.
    scope: incidents the vendor has not yet notified where enterprise's
      exposure is plausible
    reopens:
      - immediate vendor engagement to confirm exposure
      - IN-05 (cross-customer incident disclosure) enforcement
      - if the vendor withholds material notification: contract-remedy path
    seat: information-security-analyst; ai-governance-analyst
    substrate_events: [family-1: vendor-public-incident-observed]

  V5_ownership_or_control_change:
    detection: >
      Vendor's OP-01 notification; enterprise's news / regulatory-filing monitoring.
    scope: change of control, acquisition, ownership above defined threshold, key-personnel departures for tier-4
    reopens:
      - full tier-recompute (chapter 02) as jurisdictional and financial-condition
        dimensions may have shifted
      - contract's change-of-control provisions (MSA)
      - re-diligence
    seat: enterprise-tprm-analyst; treasury / risk analyst (tier-4); ai-governance-analyst
    substrate_events: [family-1: vendor-ownership-change-detected]

  V6_jurisdictional_regime_change:
    detection: >
      Enterprise's regulatory-change scan; privacy-office monitoring; legal
      monitoring for sanctions / export-control lists.
    scope: any material change in the transfer regime the DPA relies on
      (adequacy decision revoked or issued; new SCC-equivalent required);
      sanctions listing affecting vendor or vendor's parent / subsidiaries;
      export-control change affecting model access; new AI-specific law
      in scope for the vendor's operations.
    reopens:
      - jurisdictional-exposure dimension refresh (chapter 02)
      - DPA schedule review
      - contract's export-control provisions (OP-04)
      - possibly a supply-chain sanctions review
    seat: legal-compliance-analyst; privacy-office-analyst; ai-governance-analyst
    substrate_events: [family-1: jurisdictional-regime-change-detected]

  V7_third_party_audit_finding_or_certification_change:
    detection: >
      Vendor's attestation letter refresh (SOC 2, ISO 27001, ISO 42001); vendor's
      independent third-party audit report if delivered; vendor's certification
      lapse or downgrade.
    scope: any material finding in the vendor's attestations, or a lapse
      / downgrade of a certification the enterprise's diligence relied on
    reopens:
      - DDQ category 1 and 2 refresh
      - evidence-access refresh (EV family clauses)
      - risk register update if a compensating-control was based on the
        certification
    seat: information-security-analyst; ai-governance-analyst
    substrate_events: [family-1: vendor-attestation-change-detected]

  # Enterprise-side triggers
  E1_use_pattern_expansion:
    detection: >
      Sponsoring-team-declared change (new use case, new data class flowing,
      new geography, new decision-materiality); enterprise's usage-monitoring
      detecting undeclared expansion.
    scope: any enterprise-side change that could push a tier dimension up
    reopens:
      - full tier-recompute (chapter 02)
      - DDQ delta review at new tier
      - contract-shape review (may require addendum amendment)
    seat: ai-governance-analyst + sponsoring-team lead
    substrate_events: [family-1: use-pattern-expansion-declared]

  E2_regulatory_change_on_enterprise_side:
    detection: >
      Enterprise's regulatory-tracking programme (mod-104 multi-jurisdictional
      reconciliation carries the scan discipline).
    scope: new regulation in scope for the enterprise's use of the vendor
      (e.g., a new state AI act with third-party-diligence obligations;
      an EU AI Act general-purpose model rule change with cascading deployer
      obligations)
    reopens:
      - DDQ delta review to add questions the new regulation requires
      - contract-shape review to add clauses the new regulation supports
    seat: ai-governance-analyst + legal-compliance-analyst
    substrate_events: [family-3: regulatory-change-affecting-vendor-relationship]

  E3_first_line_evaluation_signal:
    detection: >
      First-line ongoing monitoring (mod-107 ch03) flags vendor-side
      degradation: rising hallucination rate, safety-eval regression, latency
      / availability degradation, cost / metering anomaly.
    scope: any signal that could indicate vendor-side change the vendor has
      not disclosed
    reopens:
      - immediate vendor engagement to confirm or refute suspected change
      - if confirmed: full V1 flow (model-version-change)
    seat: ai-evaluation-engineer; ai-governance-analyst
    substrate_events: [family-1: first-line-vendor-degradation-signal]

  E4_incident_on_enterprise_side_implicating_vendor:
    detection: >
      Enterprise's own incident-response programme; a customer-facing incident
      whose root cause involves the vendor.
    scope: any enterprise-side incident where the vendor is a contributing
      or root cause
    reopens:
      - vendor-incident routing to notify vendor and request corroborating
        investigation
      - contract's remedy provisions
      - risk register update
    seat: incident-response function; ai-governance-analyst
    substrate_events: [family-1: enterprise-incident-implicating-vendor]
```

The catalog is not exhaustive — the enterprise adds triggers as its exposure evolves. What matters is that the catalog is *authored*, *versioned*, and *wired* — not that it starts complete.

### Component 3 — Vendor-incident routing

Vendor-incident routing is the pathway a vendor's notification takes through the enterprise from receipt to closure. The pathway is designed once and rehearsed; the alternative — hand-improvising every incident — is where the enterprise loses hours on triage and blows regulator-facing timelines.

```yaml
vendor_incident_routing:
  version: 1.0.0

  intake:
    channel:
      primary: enterprise Vendor Incident Contact intake (defined per contract IN-02)
      alternates: incident-response function; ai-governance-analyst; legal
    initial_actions:
      - timestamp the notification receipt
      - assign a case id
      - substrate log entry (family-1: vendor-incident-received) with vendor id,
        severity as declared by vendor, and content summary
      - notify the designated on-call: incident-response function primary, ai-governance-analyst secondary
    time_to_ack_to_vendor: within 1 business hour (24×7 for critical)

  triage:
    seats:
      lead: incident-response function
      ai_governance: ai-governance-analyst (level 15 or 25 depending on complexity)
      technical_assessment: sponsoring team + ai-evaluation-engineer (level 35 peer)
      legal_assessment: legal-compliance-analyst
      external_communications: communications function (tier-3+ / high severity)
    outputs:
      - impact_assessment: which enterprise systems, users, or data are affected;
        the enterprise-tier of the affected systems; the classes of decisions or data at risk
      - regulator_facing_assessment: is this a reportable incident under EU AI Act Article 73,
        GDPR Article 33, sectoral obligations (BSA / OCC / HIPAA / SEC / other)?
      - containment_plan
      - communications_plan

  containment:
    lead: sponsoring team (with incident-response function support)
    typical actions:
      - failover to alternate vendor / fallback path if available
      - restrict enterprise usage of vendor's affected feature / version
      - preserve evidence (logs, prompts, completions, telemetry) for downstream forensic
      - substrate log entries per containment action (family-1)

  aims_non_conformity:
    trigger: >
      Any vendor incident that touches an AIMS-controlled activity requires
      a non-conformity record under ISO/IEC 42001 Clause 10. The head of AI
      governance opens a CAPA record; the mod-105 non-conformity flow runs.
    seat: head-of-ai-governance (formal opening); ai-governance-analyst (drafting)
    substrate_events: [family-3: aims-nonconformity-opened]

  risk_register:
    trigger: >
      Any vendor incident that changes the enterprise's residual risk exposure
      is a risk-register update event (mod-106).
    seat: risk engineer (level 25) with ai-governance-analyst
    substrate_events: [family-3: risk-register-update]

  regulator_facing_report:
    trigger: >
      Incidents meeting the reportable threshold under EU AI Act Article 73,
      GDPR Article 33/34, sectoral incident-reporting obligations, or specific
      state / sector AI-incident-reporting requirements.
    timeline:
      - EU AI Act Article 73: per Article 73 timelines (verify against final text). <!-- needs-research: confirm Article 73 timelines and severity categories against the current consolidated Regulation (EU) 2024/1689 text -->
      - GDPR Article 33: 72 hours to supervisory authority where notifiable.
      - Sectoral: per applicable rule (SEC, OCC, HIPAA, etc.).
    seats:
      legal_seat: legal-compliance-analyst + general-counsel
      regulator_communications: regulatory-affairs function
      ai_governance: head-of-ai-governance
      accountable_executive: ai-accountable-executive (final sign-off on filing)
    substrate_events: [family-3: regulator-facing-incident-filed]

  vendor_engagement:
    ongoing:
      - vendor commits to updates on defined cadence (IN-02 clause)
      - post-mortem delivery per IN-03 within defined window
    escalation:
      - repeated missed updates: escalate to vendor's executive counterpart
      - deficient post-mortem: contract remedy path
      - regulator-cooperation invocation per IN-04 if regulator requests

  closure:
    outputs:
      - closure record: root cause, contributing factors, remediation, preventive measures
      - lessons-learned: enterprise-side changes (DDQ question additions, contract-clause
        additions, monitoring-trigger additions)
      - vendor-relationship disposition: continue / renegotiate / put on watchlist / exit
      - substrate close event (family-1: vendor-incident-closed)
      - CAPA closure or continuation per Clause 10

  post_close_review:
    cadence: quarterly review of closed incidents at head-of-ai-governance level
    scope: pattern detection (repeated incidents at a vendor; class-wide vendor issues);
      lessons-learned application; monitoring-schedule updates
```

The routing is not the incident-response function's runbook — it is the *AI-specific overlay* on the enterprise's incident-response programme. The incident-response function owns the general shape; the AI programme adds the AIMS non-conformity, the AI-specific regulator-facing timelines (Article 73), and the AI-specific vendor-engagement pathway.

### Component 4 — Contract-renewal review

The pre-renewal review is the disciplined decision to continue, renegotiate, or exit. It happens at a defined pre-renewal window (chapter 04 specifies the window per tier), not at renewal-minus-30-days. Renewing under time pressure is where enterprises lose contract-shape leverage.

```yaml
renewal_review:
  version: 1.0.0

  pre_renewal_windows:
    tier-1: renewal - 60 days
    tier-2: renewal - 90 days
    tier-3: renewal - 6 months
    tier-4: renewal - 9 months (with executive engagement)

  inputs:
    - executed contract as-is (identify clauses, catalog version, exceptions carried)
    - catalog version delta: what the current template offers that the executed contract does not
    - DDQ delta: what has changed in vendor's answers since last full refresh
    - monitoring history: incidents in the period, drift triggers fired, attestation gaps observed
    - risk register: current residuals on this vendor
    - business-side input: sponsoring team's use pattern current, planned expansions, alternatives evaluated
    - market scan: alternative vendors' current offering and pricing
    - regulatory scan: new obligations affecting the engagement since signing

  decision_options:
    continue_as_is:
      criteria: >
        Vendor's posture is unchanged or improved; no material incidents;
        catalog delta is not material; sponsoring team's use unchanged;
        no better alternative.
      output: continuation with existing addendum; scheduled re-tier if triggers accumulated

    renegotiate:
      criteria: >
        Catalog delta is material (new commitments available); DDQ delta shows
        the vendor's posture has drifted (either better or worse); enterprise's
        use pattern has expanded; new regulatory obligation requires clause
        additions.
      output: renegotiation of the addendum against the current catalog version;
        renegotiation of the order form; substrate log entry on the shift

    exit:
      criteria: >
        Vendor's posture is materially worse and the vendor will not remediate;
        material incident with insufficient vendor response; alternative vendor
        materially better and migration is executable within the renewal window;
        strategic decision independent of the vendor's quality.
      output: exit-plan activation (chapter 04 EX family clauses fire);
        TSA invocation for tier-4; substrate log entries on exit plan

  seats:
    lead: ai-governance-analyst + procurement-lead
    architect: ai-governance-architect (level 50) reviews decision for tier-3+
    accountable_executive: sign-off on tier-4 renewal decisions
    legal: contract-renewal drafting
    finance: pricing / commercial review

  substrate_binding:
    - renewal review record: substrate://vendor-register/{vendor_id}/renewal/{renewal_year}
    - decision record with rationale
    - if renegotiation: catalog-version migration record
    - if exit: exit-plan artefact

  gate:
    - the renewal decision is a gated artefact; a renewal that closes without
      a substrate-recorded review record is a governance failure the third-line
      audit samples for
```

The renewal review is a *smaller decision than pre-deployment* but a *larger one than day-to-day monitoring*. It is where the enterprise's exit posture (chapter 04) actually gets exercised or refreshed.

## The vendor register — the monitoring-state overlay

The register from chapter 01 carries monitoring-state fields. The operating rhythm queries these fields; the queries are what surface the state of the vendor population without a manual sweep.

```yaml
vendor_register_monitoring_fields:
  # per vendor
  attestation:
    last_full_ddq_refresh: <date>
    next_full_ddq_refresh: <date>
    last_partial_reattestation: <date>
    next_partial_reattestation: <date>
    days_overdue_on_reattestation: <int>  # queryable metric

  triggers:
    open_triggers: [<list of trigger-ids fired but not yet closed>]
    closed_triggers_last_90d: [<list>]
    time_to_close_average: <duration>

  incidents:
    open_incidents: [<list of case ids>]
    closed_incidents_last_180d: [<list>]
    time_to_ack_average: <duration>
    late_notifications_count: <int>  # queryable for contract remedy triggers

  contract_lifecycle:
    contract_start: <date>
    initial_term_end: <date>
    current_renewal_end: <date>
    days_to_renewal: <int>  # queryable
    pre_renewal_window_open: <bool>
    catalog_version_executed: <version>
    catalog_version_current: <version>
    catalog_delta_material: <bool>  # queryable

  supply_chain (if applicable):
    last_artefact_ingest: <date>
    last_ml_bom_refresh: <date>
    slsa_level_current: <int>
    signature_verification_last: <status>

  register_health_score:
    attestation_currency_ok: <bool>
    open_trigger_count_ok: <bool>
    open_incident_count_ok: <bool>
    renewal_window_managed_ok: <bool>
    aggregate: <traffic-light>

operating_rhythm_queries:
  daily:
    - any_tier-4_vendor_with_open_incident_ok?
    - any_tier-4_vendor_with_open_trigger_older_than_5_days?
  weekly:
    - all_vendors_with_reattestation_overdue?
    - all_vendors_with_late_notification_in_last_30d?
    - all_vendors_with_pre_renewal_window_open?
  monthly:
    - tier-3+_vendors_with_catalog_delta_material?
    - vendors_with_incident_pattern_in_last_180d (>=3 incidents)?
  quarterly:
    - full_population_by_tier_and_health_score?
```

The queries are what make the register a *living substrate artefact* rather than a filing cabinet. If the enterprise's operating rhythm does not query them, the fields decay to lies-of-omission.

## Coordination with the enterprise TPRM and other functions

The AI programme's monitoring composes with the enterprise TPRM function's monitoring rather than replacing it. Two coordinations the architect defends:

- **Shared incident intake, AI-specific overlay.** The enterprise's incident-response function receives vendor incident notifications through the standard intake; the AI-specific overlay (AIMS non-conformity opening, AI-specific regulator-facing timelines, AI-specific technical assessment) attaches to AI-relevant incidents.
- **Shared attestation ingest, AI-specific extensions.** The enterprise TPRM's annual attestation cycle carries the SOC 2 / ISO 27001 refresh; the AI programme's quarterly / annual cycle carries the AI-specific attestations (safety-evaluation refresh, model-card refresh, supply-chain evidence refresh).

The composition is the practical form of invariant 1 (TPRM integration, not TPRM duplication) from chapter 01.

## The six invariants the monitoring schedule holds

**Invariant 1 — cadence is calendared, not "as needed."** Every tier's periodic re-attestation has a fixed cadence that produces calendar events; slippage is observable. Failure mode: the enterprise says vendors are monitored quarterly; a query against the register shows 40% of tier-3 vendors have not been re-attested in over a year; no one saw the slippage because no one queried.

**Invariant 2 — drift triggers are wired to detection.** Named triggers have concrete detection mechanisms — vendor RSS ingestion, API version-header monitoring, contract-obligated notification, published-URI hash recheck. Failure mode: the trigger catalog says "V1 fires on material model version change" but nothing subscribes to the vendor's release channel; the trigger fires when a data scientist notices the model behaves differently, months later.

**Invariant 3 — vendor incidents route into AIMS and the risk register.** Every AIMS-touching vendor incident produces a Clause 10 non-conformity; every material-exposure vendor incident produces a risk-register update. Failure mode: the incident is triaged, contained, and closed by the incident-response function without a governance touch; the CAPA never opens; the risk register never reflects; the ISO 42001 auditor samples the Clause 10 records and finds vendor incidents missing.

**Invariant 4 — the renewal review happens at the pre-renewal window.** Renewal decisions are not made at renewal minus 30 days. Failure mode: the pre-renewal window opened 6 months before renewal; no review was scheduled; the review starts at T-30; the enterprise's leverage is zero; renewal ships at the vendor's terms.

**Invariant 5 — the register is queryable and queried.** The operating rhythm actually runs the queries; queries surface state; state drives action. Failure mode: the register is a spreadsheet updated by one analyst; the "queries" are the analyst re-reading rows on request.

**Invariant 6 — exceptions carry expiry and revisit.** Exceptions granted at onboarding (tier-4 vendor with a mandatory control carveout) or during monitoring (accepting a vendor's inability to remediate a finding) have an expiry date and a revisit trigger. Failure mode: exceptions accumulate without expiry; the vendor population's risk residuals grow silently; the third-line audit surfaces the accumulation.

## Two failure modes to design against

**Failure mode 1 — the annual sweep that catches nothing.** The enterprise has an annual vendor-review cycle. Every tier-3 and tier-4 vendor gets a review letter in the same week each fiscal year; the review is a checklist confirming the vendor's continued existence and current SOC 2 letter; the review takes 30 minutes per vendor for a large analyst team over 6 weeks. The output is 40 reviews with 40 "no material change" dispositions. Meanwhile, half the vendors had material model-version changes in the year, a quarter had subprocessor changes, three had public incidents, and one had an ownership change. The annual sweep caught none of them because it asked only the questions the sweep was designed to ask. The fix is architectural: the layered cadence (monthly signals + quarterly deep-dive + annual comprehensive) *plus* the drift-triggered re-assessment together are what catch drift; the annual sweep alone is not the discipline.

**Failure mode 2 — the vendor incident that never becomes an AIMS non-conformity.** A tier-3 foundation-model vendor notifies the enterprise of a material safety incident: the model version the enterprise's customer-facing chatbot depends on had a documented reasoning failure in a class of prompts that overlaps with the enterprise's use. The enterprise's incident-response function receives the notification, opens a case, triages, contains (falls back to a prior model version), and closes within a business day. The head of AI governance is informed but no CAPA is opened; no risk-register update is filed; no lessons-learned prompt lands in the DDQ catalog for next cycle. Six months later the ISO 42001 auditor samples the Clause 10 records; the vendor incident is absent; the finding is a systemic non-conformity because the AIMS's own incident-to-CAPA loop is not operating for third-party incidents. The fix is architectural: the vendor-incident routing (component 3) always opens the AIMS non-conformity for AIMS-touching incidents; the head of AI governance's operating rhythm queries the register for open non-conformities against vendor incidents; the routing is drilled on rehearsals.

## Summary

The ongoing-monitoring schedule is four components: periodic re-attestation (tier-scaled cadence: annual for tier-1, layered monthly/quarterly/annual for tier-4), drift-based re-assessment (event-triggered against a named trigger catalog with wired detection), vendor-incident routing (intake → triage → containment → AIMS non-conformity → risk register → regulator-facing report → closure), and contract-renewal review (pre-renewal-window decision to continue / renegotiate / exit). The vendor register carries monitoring-state fields queryable by the operating rhythm. Coordination with the enterprise TPRM and incident-response functions is shared intake with AI-specific overlays. Six invariants (cadence calendared, triggers wired, incidents routed, renewals in-window, register queried, exceptions expire) and two failure modes (annual sweep catches nothing, incident never becomes non-conformity) shape the discipline. Exercise-04 walks the drill. The next chapter adapts the federal AI acquisition shape (OMB M-24-18 and M-25-22) to enterprise procurement.
