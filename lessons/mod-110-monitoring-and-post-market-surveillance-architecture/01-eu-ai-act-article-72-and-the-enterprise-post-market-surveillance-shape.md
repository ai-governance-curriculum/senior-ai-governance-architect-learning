# Post-market surveillance as enterprise architecture — Article 72 and the shape

## Why this chapter exists

An enterprise that has passed a pre-deployment assurance gate believes, for a period, that the risk on the shipped system is understood. That belief is defensible for exactly as long as the pre-deployment evidence remains representative of the deployed reality. In every enterprise AI programme large enough to matter, the belief starts decaying the day after launch — the input distribution shifts, the prompt patterns evolve, users find failure modes the red team did not, a downstream integration changes, the frontier-model provider updates the endpoint under the enterprise, an incident in another organisation reveals a class of attack the enterprise has not tested for. Somebody, somewhere in the enterprise, sees the first anomaly. Whether that anomaly becomes a *signal that reaches the risk register, the regulator, and the design of the next release* — or dies in a Slack channel — is not an operational question. It is an architectural one.

Post-market surveillance (PMS) is the architectural discipline that answers it. In the EU AI Act, Article 72 makes PMS a codified obligation for providers of high-risk systems. Outside the EU, an accumulating set of parallel regimes — FDA guidance on AI/ML-enabled medical devices, US federal model-risk-management expectations under SR 11-7, Colorado's SB24-205 risk-management-programme requirements — impose PMS-shaped obligations under different names. The regulatory pressure is real, but the deeper point is that PMS is what turns a deployed AI portfolio from a snapshot at launch into a governed system over its life. Without it, the AIMS the enterprise stood up under mod-105 has an ISO 42001-shaped hole where the operational feedback loop should live; the risk register the enterprise built under mod-106 is a lagging document that memorialises last quarter's understanding; the control library from mod-102 has no evidence of *ongoing* operation to feed back into the mod-108 evidence contract.

This chapter fixes the enterprise PMS shape as an architectural artefact: what Article 72 actually obligates, why parallel regimes converge on the same shape, the four things a defensible enterprise PMS system holds, why near-miss capture is a first-class discipline distinct from incident capture, and the six invariants the architecture must preserve. The level-50 architect owns the shape. The chapters that follow bind the shape to the monitoring-to-register contract (chapter 02), the observability-platform wiring (chapter 03), the Article 73 serious-incident workflow (chapter 04), the SOC interface (chapter 05), and the external-corpora calibration (chapter 06). This chapter is the anchor.

## What Article 72 actually obligates

Article 72 of the EU AI Act imposes a *post-market monitoring system* obligation on providers of high-risk AI systems. Read as an architect and not as a paralegal, the article obligates four coupled things:

- **A documented post-market monitoring system.** Not a policy stating that monitoring will happen; a system that is designed, described, and proportionate to the nature of the AI technology and the risks of the high-risk system in question. The obligation is on the *design* of the system, discoverable by an auditor as a documented artefact, not on the fact of monitoring itself.
- **Active and systematic data collection on the system's performance throughout its lifetime.** The word *systematic* rules out ad-hoc dashboards and voluntary bug reports. The word *lifetime* rules out monitoring that ends at some fixed post-launch horizon.
- **Analysis of the collected data.** The obligation is not to *hold* data; it is to *analyse* it — to detect trends, deviations, and emerging risks that a raw stream of telemetry does not surface on its own.
- **Feedback of findings into design, risk management, and — where applicable — the technical documentation.** The loop closes. What is learned in the field re-enters the design and risk-management artefacts the enterprise already maintains under Articles 9, 11, 15, and elsewhere.

<!-- needs-research: verify the exact wording of Article 72 obligations for enterprise providers vs deployers, including which sub-obligations attach to deployers under Article 26 and where the post-market monitoring plan template referenced in Article 72 is specified (implementing act vs annex). -->

Two features of Article 72 matter architecturally more than the surface text. First, the obligation attaches to *providers* — the enterprise-as-provider when the enterprise puts a high-risk system on the market, distinct from the enterprise-as-deployer when it uses a third-party system. A single enterprise is typically both, for different systems in its portfolio, and the PMS architecture must handle both roles cleanly. Second, Article 72 is *linked* to Article 73 (serious-incident reporting) — the PMS system is what surfaces the events Article 73 requires the provider to report, on the cadence Article 73 sets. Chapter 04 details the reporting workflow; this chapter fixes the surveillance system that feeds it.

<!-- needs-research: verify the provider vs deployer split for post-market monitoring obligations under Articles 72 and 26, and the precise linkage between Article 72 monitoring outputs and Article 73 reporting triggers. -->

## Why parallel regimes impose the same shape

A programme designed only for Article 72 is under-designed for the enterprise reality. Any organisation of a scale that requires this role tree is subject to more than one PMS-shaped regime at once, and the architecture must satisfy all of them from a single shape rather than standing up separate stacks per regulator.

- **FDA guidance on AI/ML-enabled medical devices** treats post-market performance monitoring, real-world evidence, and predetermined change-control plans (PCCPs) as core to the lifecycle of software-as-a-medical-device that adapts. The obligation shape is the same: designed data collection over the deployed life, systematic analysis, and controlled feedback into design changes. <!-- needs-research: verify current FDA final guidance vs draft-guidance status for AI/ML-enabled device software functions, PCCP framework, and any 21st-Century-Cures-updated citations. -->
- **US federal model-risk-management expectations under SR 11-7 and OCC 2011-12** treat *ongoing monitoring* as an integral component of model risk management, distinct from initial validation. The expectation is that model performance is tracked against defined benchmarks over the life of the model, that thresholds trigger revalidation, and that outcomes analysis feeds back into the model inventory and validation cycle. <!-- needs-research: verify SR 11-7 language on ongoing monitoring frequency expectations and the interaction with OCC 2011-12 for national-bank supervised entities. -->
- **Colorado SB24-205 (Colorado AI Act)** imposes risk-management-programme obligations on developers and deployers of high-risk AI systems, including impact-assessment updates and disclosure obligations that presume an ongoing programme rather than a one-time exercise. <!-- needs-research: verify SB24-205 effective date, the precise programme obligations imposed on deployers vs developers, and whether the AG-issued rules specify PMS-cadence requirements. -->
- **Sector regulators** (state insurance commissioners under the NAIC Model Bulletin approach, EU financial supervisors under EBA guidance where AI is used in credit decisioning, GDPR supervisory authorities where automated decision-making is in scope) increasingly presume ongoing monitoring in their examination playbooks. <!-- needs-research: verify NAIC Model Bulletin on the Use of AI Systems by Insurers ongoing-monitoring language and any EBA guidance on model monitoring for credit-scoring AI. -->

The composition move is the same one mod-104 fixed for policy reconciliation: design the enterprise PMS system to the strictest applicable regime's shape, then map each regime's specific obligations onto slots of that shape. A separate PMS stack per regulator produces divergence, drift, and the multi-store failure mode this chapter names below.

## The four things a defensible enterprise PMS system holds

A PMS system that meets Article 72's obligation and satisfies the parallel regimes above holds four coupled things. The architect designs all four; the operations roles below the architect run them.

### (a) A single source of truth aggregating incidents, near-misses, and material operational signals

There is exactly one authoritative store of *events* in the enterprise PMS system. An event is an incident (something realised harm or a control failure), a near-miss (a precursor that did not realise harm because of luck, defence-in-depth, or a compensating control), or a material operational signal (a drift breach, a fairness-metric excursion, a jailbreak-detection spike). The store references the raw telemetry — the observability platform's time-series, the SIEM alert, the model card version — but it does not duplicate them; the event record is the authoritative *narrative* about what happened, linked to the raw evidence. The store is what the risk register, the Article 73 reporting workflow, the CAPA process, and the board dashboard all *read from*.

### (b) A defined regulatory-reporting cadence

The PMS system carries a calendar. Article 73 serious-incident reporting has a cadence. The FDA quality-system reports run on a cadence. State-insurance-department examination cycles run on a cadence. The Colorado impact-assessment update obligation runs on a cadence. The calendar names, per regime, what report is due, on what trigger, to whom, populated from which slots of the event store, and signed by which enterprise seat. Chapter 04 walks the Article 73 workflow specifically; this chapter fixes that the *calendar itself* is an architectural artefact under the PMS system's ownership, not a spreadsheet on the head of AI governance's laptop.

### (c) Deviation-from-appetite alarms wired to the risk register

The mod-106 risk taxonomy and appetite table define, per capability tier and harm category, the tolerance the enterprise commits to. The PMS system holds the alarms that fire when a monitored metric crosses a threshold that maps to a *deviation from that appetite*. Every alarm is pre-registered against a mod-106 category and a specific tolerance-table row; the alarm's fire is not an ad-hoc alert but a *typed event* that writes into the risk register through the monitoring-to-register contract (chapter 02 of this module, extending the team-scope contract the `ai-risk-engineer` at level 25 already authors under mod-106 chapter 04). Ad-hoc alarms — the ones a product team set up in Grafana last quarter and never registered — are not part of the PMS system by definition.

### (d) A documented analysis and feedback loop that closes into design, risk-treatment, and CAPA

Article 72's fourth obligation is the feedback loop, and it is the one enterprises most commonly under-design. Every closed event in the PMS store produces a downstream artefact: a risk-treatment plan update (per mod-105 chapter 06), a CAPA record (per mod-105 chapter 09), a control-library amendment (per mod-102), a taxonomy amendment (per mod-106 chapter 07), a policy update (per mod-103), or an evidence-contract change (per mod-108). *No event closes into a shared drive.* The closing artefact is the audit-visible proof that the feedback loop functioned. Where no downstream artefact is warranted, the closure record explicitly says so and names who ratified the no-change decision — a null feedback is a decision, and the decision is logged.

## Near-miss capture is a first-class discipline

Realised-incident logging on its own is systematically blind to the precursor pattern. Every mature safety-critical discipline outside AI has learned this the hard way and codified near-miss reporting as a separate architectural artefact:

- **Aviation** operates the Aviation Safety Reporting System (ASRS), administered by NASA at arm's length from the FAA specifically to solicit *voluntary* reports of events that did not become accidents. The confidentiality architecture is deliberate — the reporter is protected from enforcement so that the precursor data flows. <!-- needs-research: verify current ASRS scope, immunity provisions, and any parallel systems (ASAP, ASIAS) that supplement it. -->
- **Healthcare** operates patient-safety event-reporting systems (the AHRQ Common Formats for patient-safety reporting; state-level PSO frameworks under the Patient Safety and Quality Improvement Act) that explicitly capture near-misses alongside adverse events, on the same well-established finding that realised-harm-only logging misses the precursor signal. <!-- needs-research: verify current AHRQ Common Formats versions and PSO framework structure. -->
- **Industrial process safety** under the CCPS / OSHA PSM regime treats near-miss investigation as a required element of a process-safety-management programme, on the finding — repeatedly re-derived across chemical, nuclear, and oil-and-gas incidents — that serious incidents are almost always preceded by a pattern of near-misses that were logged badly, aggregated badly, or investigated not at all. <!-- needs-research: verify current CCPS and OSHA PSM near-miss guidance references. -->

The AI-specific analogue is direct. A jailbreak that was caught by an output filter is a near-miss for the same category of harm the equivalent uncaught jailbreak would have realised. A prompt-injection that failed only because the target tool required an authorisation step is a near-miss for the class of agentic misuse the enterprise's risk taxonomy names. A model-extraction attempt that was rate-limited before it succeeded is a near-miss for confidential-information-exfiltration. The enterprise that logs only the realised incidents will discover, after the eventually-realised serious incident, that the precursor pattern was visible in the near-miss data — had the near-miss data existed. The PMS architecture must give near-miss capture its own schema, its own reporting cadence, and its own analytical treatment. Parity of treatment with incident capture is the invariant; folding near-misses into the incident register as a severity=low tier is not architectural parity, it is administrative erasure.

## The multi-store failure mode

The most common shape enterprise PMS ends up in — absent explicit architecture — is one where each observability platform (Fiddler AI, Arthur AI, WhyLabs, Evidently AI, or the in-house equivalent), the SOC's SIEM, an AIID-style external-incident tracker the risk team subscribes to, and the risk register itself each hold a *fraction* of the picture. The observability platform holds drift and performance excursions; the SIEM holds jailbreak-detection alerts; the risk register holds quarterly risk-review entries; the incident tracker holds public incidents from AIID and OECD.AI that the risk team found interesting. None of them can answer a portfolio-level question. The certification body's first question at an ISO 42001 audit — *what is the current residual on this system, and what is the trend since last quarter?* — has as many answers as there are stores, none of them authoritative. Chapter 03 walks the observability-platform wiring that prevents this; the invariants below make the prevention testable.

## The six invariants of a defensible PMS architecture

Every design decision in the chapters that follow — the monitoring-to-register contract (chapter 02), the observability wiring (chapter 03), the Article 73 workflow (chapter 04), the SOC interface (chapter 05), the external-corpora calibration (chapter 06) — is a choice about preserving these invariants. Each has a testable failure mode.

**Invariant 1 — single source of truth.** Every incident, near-miss, and material operational signal has exactly one authoritative record in the PMS event store; the store references but does not duplicate the raw observability streams. *Detection test:* pick a recent incident at random and ask three audiences (the SOC, the model-owner's product team, the risk office) to produce the event record; they must all return the same identifier from the same store.

**Invariant 2 — near-miss capture is first-class.** Near-misses have their own schema, their own reporting cadence, and their own analytical treatment; the near-miss stream reaches parity with the incident stream in ownership, review, and closure. *Detection test:* the near-miss volume, per system per quarter, is non-zero for any system under active use; systems with zero near-misses logged are treated as *under-reported*, not as *safe*.

**Invariant 3 — regulatory-reporting cadence is scheduled and versioned.** The PMS calendar names each regime's reporting cycle, populated fields, and signing seat; changes to the cadence are versioned; a late report produces its own PMS event (a control failure in the reporting-cadence control). *Detection test:* the calendar file has a version history; the last three amendments each cite a ratifying seat; the audit log of last quarter's reports shows on-time filing or an explicit late-report event.

**Invariant 4 — deviation-from-appetite alarms are pre-registered.** Every alarm binds to a mod-106 taxonomy category and a specific tolerance-table row; ad-hoc alarms outside the pre-registration list are not part of the PMS system. *Detection test:* the alarm registry cross-references cleanly to the appetite table; sampling ten alarms at random shows each has a taxonomy category and tolerance-row citation.

**Invariant 5 — the PMS feeds back.** Every closed event produces an artefact somewhere downstream — risk-treatment update, CAPA record, control-library amendment, taxonomy amendment, policy update, evidence-contract change, or an explicit no-change ratified closure. *Detection test:* pick ten closed events at random; each closure record cites a specific downstream artefact by identifier, or a ratified no-change decision with a named seat.

**Invariant 6 — end-to-end auditability.** Every event has a traceable lineage from the raw observability signal, through classification, aggregation, decision, and remediation; sampling access is available to third-line audit (per mod-107 chapter 04) and to external assurance providers (per mod-107 chapter 05). *Detection test:* the internal-audit team can, without special access, reconstruct a chosen event's full lineage in one working session.

## The two failure modes to design against

**Failure mode 1 — three-sources-of-truth divergence.** The risk register, the SOC SIEM, and each observability platform's UI each report different numbers to different audiences. The head of AI governance briefs the board from the risk register; the CISO briefs the audit committee from the SIEM's AI-tagged tickets; the head of engineering briefs the platform steering committee from Fiddler's dashboard. None of the three sets of numbers reconciles to any other. The ISO 42001 certification body asks *what is the current residual on this system?* and gets three answers. The finding is systemic. The fix is invariant 1 designed in from the start — the single event store, referenced by all downstream consumers, with the observability platforms as *feeds* and the risk register and SIEM as *readers*, never as competing systems of record. Chapter 03 walks the wiring specifically.

**Failure mode 2 — near-miss blindness.** The enterprise logs only realised incidents. The precursor pattern for what will eventually be a serious incident — the jailbreak that the output filter caught seventy times in the six months before it broke through, the prompt-injection whose payload the retrieval layer stripped in a way the attacker eventually worked around, the drift excursion that self-corrected three times and then didn't — sits unrecorded and unanalysed. When the serious incident does realise, the Article 73 disclosure is worse than it needed to be, the regulator's follow-up questions have no defensible answers, and internal post-incident review discovers the precursor pattern was in the enterprise's own telemetry all along. The fix is invariant 2 designed in from the start — separate schema, separate cadence, separate analytical treatment, and an explicit under-reporting hypothesis for systems that log no near-misses.

## A schematic of the enterprise PMS system

```yaml
post_market_surveillance_system:
  version: 1.0.0
  owner: senior-ai-governance-architect (level 50)
  ratifies: head-of-ai-governance (level 60)
  scope:
    - enterprise-as-provider systems (Article 72 direct obligation)
    - enterprise-as-deployer systems (Article 26 obligations, parallel regimes)
    - internal AI systems (voluntary parity of shape)
  inputs:
    observability_platforms: see chapter 03 (Fiddler, Arthur, WhyLabs, Evidently, in-house)
    soc_signals: see chapter 05 (jailbreak detections, prompt-injection alerts, ai-tagged tickets)
    third_party_incident_feeds: vendor status pages, frontier-model provider incident notices
    external_corpora: see chapter 06 (AIID, OECD.AI, MIT AI Risk Repository)
    user_reports: in-product feedback, support tickets tagged ai-quality
    internal_reports: red-team findings, model-owner self-reports, ai-governance-analyst (level 15) triage
  stores:
    pms_event_store:
      role: single authoritative record of incidents, near-misses, operational signals
      schema_families: [incident, near_miss, operational_signal]
      references: [raw_telemetry_uri, observability_platform_id, siem_alert_id]
    raw_retention_tier:
      role: data-lake tier holding raw observability streams for lineage and re-analysis
      retention: per regulatory regime, defaulting to the longest applicable
  outputs:
    regulatory_reporting_feeds: see chapter 04 (Article 73 workflow; FDA MDR-equivalent; sector filings)
    risk_register_writes: per chapter 02 monitoring-to-register contract; per mod-106
    control_library_amendments: per mod-102
    taxonomy_amendments: per mod-106 chapter 07
    capa_records: per mod-105 chapter 09
    risk_treatment_updates: per mod-105 chapter 06
    board_and_executive_reporting: periodic PMS report to ai-accountable executive
  cadences:
    regulatory_reporting: per regime (see calendar artefact)
    deviation_alarm_evaluation: per alarm (streaming for critical; periodic for trend-based)
    near_miss_review: no less frequent than incident review; recommended weekly or per-sprint
    taxonomy_coverage_review: per chapter 06 (against AIID/OECD.AI/MIT repository)
    periodic_pms_report: to ai-accountable executive on stated cadence
    pms_system_change_control: versioned; ratified by head-of-ai-governance
  coordinating_roles:
    designs: senior-ai-governance-architect (level 50)
    ratifies: head-of-ai-governance (level 60)
    populates: ai-risk-engineer (level 25); ai-evaluation-engineer (peer, level 35)
    triages: ai-governance-analyst (level 15)
    soc_interface: ai-infra-security (level 35)
    audit_sampling: third-line internal audit (per mod-107 chapter 04)
  invariants:
    - id: I1
      description: single source of truth for events
    - id: I2
      description: near-miss capture is first-class
    - id: I3
      description: regulatory-reporting cadence scheduled and versioned
    - id: I4
      description: deviation-from-appetite alarms pre-registered
    - id: I5
      description: PMS feeds back into design, treatment, CAPA, control library, taxonomy
    - id: I6
      description: end-to-end auditability to third-line and external providers
```

Exercise-01 asks you to produce the enterprise-specific instance of this schematic and defend each invariant against its detection test. The schematic is what a certification body wants to see when it asks *walk me through your post-market monitoring system* — a designed artefact, not a screenshot of a dashboard.

## Summary

Post-market surveillance is the architectural discipline that keeps the enterprise's understanding of its deployed AI portfolio current — the thing that lets the pre-deployment gate's assurance decay into an *ongoing* assurance rather than a snapshot at launch. Article 72 of the EU AI Act codifies the obligation for providers of high-risk systems, and a set of parallel regimes (FDA guidance on AI/ML-enabled devices, SR 11-7 ongoing monitoring, Colorado SB24-205, sector supervisors) impose the same shape under different names. A defensible enterprise PMS system holds four coupled things: a single source of truth for incidents and near-misses and operational signals, a defined regulatory-reporting cadence, deviation-from-appetite alarms wired to the risk register, and a documented feedback loop that closes into design, risk-treatment, and CAPA. Near-miss capture is a first-class discipline — the aviation, healthcare, and process-safety analogues have all learned the same lesson about precursor blindness, and the AI-specific analogue is direct. Six invariants (single source of truth, near-miss parity, versioned cadence, pre-registered alarms, feedback closure, end-to-end auditability) make the architecture testable. Two failure modes (three-sources-of-truth divergence, near-miss blindness) are the shapes the architecture must specifically design against. The chapters that follow bind the shape to the monitoring-to-register contract (chapter 02), the observability wiring (chapter 03), the Article 73 workflow (chapter 04), the SOC interface (chapter 05), and the external-corpora calibration (chapter 06) — and land in a PMS system the rest of the stack (mod-107 assurance, mod-108 evidence, mod-111 GRC platform) can bind to.
