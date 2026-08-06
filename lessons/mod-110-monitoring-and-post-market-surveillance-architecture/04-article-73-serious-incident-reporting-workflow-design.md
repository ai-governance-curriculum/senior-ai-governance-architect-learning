# The Article 73 serious-incident reporting workflow — internal escalation and coordination

## Why this chapter exists

The morning the first serious incident lands is the morning the enterprise discovers whether it has an architected workflow or a scramble. The pattern of the scramble is predictable and depressingly consistent. The first-line team detects the problem at 03:14 UTC on a Tuesday. Somebody pages the head of engineering. The head of engineering pages the CTO. The CTO — around 07:00 — asks whether legal is involved yet; legal is not. Legal is looped in around 09:30 and asks whether the governance office knows; the governance office finds out around 11:00 from a colleague forwarding a customer-success email. The head of AI governance asks the level-50 architect whether Article 73 has fired; the architect asks the risk engineer to classify the severity; the risk engineer asks what the actual facts are; nobody can produce an authoritative fact record because operational response has been running in parallel across three chat channels and a shared document. By the end of Tuesday there is a regulatory-notification clock running against a "moment of awareness" that nobody logged, a customer-communications draft that says one thing about root cause, a legal-drafted regulator filing that says another thing about root cause, and a board briefing being prepared that will say a third thing about root cause when it goes out on Thursday.

Nothing about that pattern is a competence failure. Every named role acted responsibly. The failure is architectural. The absence of a pre-designed workflow — a standard operating procedure that fires on the intake signal, names the seats, defines the artefacts, sets the clock, and fans obligations out in parallel — is the level-50 architect's design gap. This chapter closes it. Article 73 of the EU AI Act obligates providers of high-risk AI systems to report serious incidents to the market surveillance authority of the member state where the incident occurred, within specific timeframes tied to severity <!-- needs-research: verify the specific Article 73 timeframes for serious-incident categories — they differ by severity (death/serious harm vs infrastructure disruption vs other) and there may be preliminary vs final report distinctions -->. The workflow that discharges that obligation is what the architect authors. The signing of the filing itself is not the architect's — it belongs to `head-of-ai-governance` at level 60. The filing content is not the architect's — it belongs to legal. The architect designs the shape everyone else executes inside.

## The role separation the architect must not conflate

Serious-incident response mobilises a large cast, and each seat has a specific accountability that the SOP must respect. Collapsing any two of these into one role is where architected workflows degrade into scrambles.

- **The level-50 architect** designs the workflow itself — the SOP document, the intake schema, the escalation graph, the evidence-packet template, the parallel-obligation fan-out, the close-condition. The architect does not sign filings and does not draft filings. The architect is on the incident bridge as a standing seat because the workflow's design intent is often the fastest way to answer procedural questions in the moment, but the architect's authored artefact is the SOP, not any given filing.
- **The head of AI governance (level 60)** owns the regulator-facing filing decision. This role signs the AI Act notification to the market surveillance authority, is accountable to the board and the audit committee for the filing, and carries the personal reputational weight of that accountability. The decision "we file the AI Act notification, on these facts, at this severity classification" is this role's, informed by legal advice, on the evidence packet the architect's SOP produces.
- **Legal** owns the filing-form and regulatory-communication content. Legal drafts the actual submission — chooses the language, negotiates the categorisation with regulator counsel where applicable, manages the follow-up correspondence — on the architect-designed evidence packet as the factual substrate.
- **PR and Comms** own the external communications *outside* the regulator. Customer letters, press statements, social-media posture, employee comms. PR and Comms coordinates with the regulator-facing workstream but does not initiate the regulatory filing and, critically, does not release external comms that contradict the regulator filing's facts.
- **SecOps / CISO** owns any security-incident coordination. If the AI incident involves attack, exfiltration, prompt-injection succeeding in a tool-calling agent, or model-supply-chain compromise, the SOC-side runbook (chapter 05) fires alongside the AI Act workflow. The two workstreams share the same incident id and coordinate through the incident bridge; they do not fork the fact record.
- **`ai-risk-engineer` (level 25)** supplies the risk-scoring input. Given the facts of the incident, this seat classifies the incident's severity against the taxonomy from mod-106 and against the appetite tolerance, so the head-of-ai-governance decision has a scored basis.
- **`ai-evaluation-engineer` (peer, level 35)** supplies any technical re-derivation input. Was the model behaving as validated? Did any of the pre-deployment evaluations predict this behaviour and get dismissed? Is the incident inside or outside the validated operating envelope? The evaluation engineer's technical read grounds the "what happened, in evaluation-programme terms" question the regulator will ask.
- **`ai-governance-analyst` (level 15)** collects and packages the documented information. The analyst is the single owner of the incident fact record — the authoritative document of record that every downstream filing and communication derives from. This concentration of ownership in a single named seat is the architected defence against the parallel-filings-diverge failure mode this chapter names.

Emphasise: the architect writes the SOP; the head of AI governance signs each filing; the analyst owns the fact record; legal drafts the filing text. Four seats, four distinct accountabilities, no overlap. Every additional filing (GDPR, SEC, sector) adds a further named-decision-authority seat, never a committee.

## The internal-escalation flow as a graph

The workflow the architect fixes is a directed graph with named seats on each node and named artefacts on each edge. The graph shape does not vary by incident; only the content flowing through it does. This is what turns the response from a scramble into an SOP.

1. **Intake.** The trigger fires from the PMS store (chapter 01) — a classification of tier-4 severity by the monitoring pipeline, an external notification received (customer, regulator, media, researcher), or a SOC severity-1 alert with an AI-system nexus. The intake step is owned by a designated first-responder-on-call rota and produces the *incident id* (single, authoritative, sequential) and the *moment of awareness* timestamp (see invariant ii below).
2. **Classification.** The `ai-risk-engineer` scores the severity against the mod-106 taxonomy and maps to Article 73's serious-incident categories. This classification determines which downstream obligations fire and which timeframes apply. The classification is written to the incident fact record within a bounded interval of intake, not "when convenient."
3. **Convene the incident-response bridge.** The bridge is a standing forum with named seats and defined coverage — a primary chair (typically the head of AI governance or a delegate), a technical lead (evaluation engineer or platform lead), a risk seat (risk engineer), a legal seat, a comms seat, a SOC seat, the architect as workflow-shape reference, and the analyst as fact-record owner. Timezone coverage is designed — the bridge convenes within a defined window of intake regardless of the hour, with a follow-the-sun rota so that no incident stalls waiting for business hours in a particular geography.
4. **Decision to notify regulator.** The head of AI governance makes the decision on legal advice, informed by the architect's evidence packet and the risk engineer's scoring. This is a named-seat decision, not a committee vote. The decision is logged with its facts, its reasoning, and its signatory in the incident record.
5. **Notification prepared.** Legal drafts the actual submission text. The architect's evidence-packet template supplies the factual substrate — the system identifier, the incident timeline, the affected populations, the initial root-cause hypothesis, the containment and corrective actions in flight, the data-lake reference (chapter 03) where the raw evidence lives. Legal is drafting on a scaffold, not from a blank page.
6. **Notification sent.** The filing lands with the market surveillance authority through legal's regulatory-communications channel. The send-timestamp is recorded against the moment-of-awareness clock and the compliance-window calculation is closed.
7. **Follow-up filings on schedule.** Article 73 contemplates follow-up reporting as the investigation matures <!-- needs-research: verify the specific Article 73 timeframes for serious-incident categories — they differ by severity (death/serious harm vs infrastructure disruption vs other) and there may be preliminary vs final report distinctions -->. The SOP schedules these as calendar-driven obligations from the notification-sent event, not as ad-hoc deliverables.
8. **Post-incident review.** The workflow does not close on the last regulator ack. The close-condition (see invariant iv) includes at least one CAPA record filed against the corrective and preventive action programme in mod-105 chapter 09, at least one risk-register rescore against the mod-106 taxonomy, and a post-incident review artefact that feeds the ongoing-assurance re-assessment in mod-107 chapter 03. Without those, the incident stays open on the register.

## Coordinating with parallel obligations

A serious AI incident rarely fires only Article 73. It usually fires several regimes simultaneously, and each has its own clock, its own decision-authority, and its own filing content. The architect's design move is to fan out across obligations *without duplicating fact-collection*. One incident-fact record, owned by the analyst, feeds every filing.

- **GDPR Article 33** — the 72-hour breach notification to the lead supervisory authority when personal data is involved. Cited at title level; specific timeframe stated in the text of Article 33. The Data Protection Officer owns this decision and this filing. If the AI incident touched personal data (in training, in inference, in logs, in a leaked prompt), the DPO seat activates on the bridge and the GDPR filing runs in parallel to the Article 73 filing.
- **SEC 10-K materiality assessment and Item 1.05 8-K disclosure**, for listed enterprises where the incident is material to investors. Reference the SEC 2023 cybersecurity disclosure rule at title level <!-- needs-research: verify current SEC Item 1.05 8-K disclosure rule scope for AI incidents specifically — the 2023 rule addresses cybersecurity incidents; whether and how it extends to AI-specific incidents that are not cyber-nexus is jurisdictionally and factually specific -->. The CFO and general counsel jointly own the materiality determination; the SEC filing itself is legal's execution on that determination.
- **Sector-regulator notifications.** Banking supervisors, insurance commissioners, healthcare regulators, and telecommunications regulators each carry sector-specific AI or model-risk notification expectations <!-- needs-research: verify the specific incident-notification obligations under the relevant sector regime — the Fed / OCC MRM examination programme, state insurance commissioner model-audit regimes, and equivalent regimes in EU and UK vary substantially and require jurisdiction-and-sector-specific research -->. The relevant sector regulatory-affairs seat owns each of these filings; the architect's SOP names which seat, per sector, so no obligation lands in a queue with no owner.
- **Customer-contract notifications.** Enterprise-B2B AI contracts commonly carry incident-notification clauses in the DPA appendix and in the AI-specific rider. Current-generation contracts frequently obligate notice within short windows for AI-specific incidents (misclassification affecting a class of end users, unauthorised training-data use, model degradation below contracted performance). The customer-success and contracts seats own the fan-out to affected customers; the architect's evidence packet supplies the substrate; PR coordinates tone.
- **Sector safety-authorities where safety-critical AI is touched.** FDA for medical-device AI incidents (under the SaMD and 510(k) predicate obligations) <!-- needs-research: verify FDA post-market safety reporting obligations for AI/ML-enabled devices — MDR and MedWatch obligations plus the FDA's evolving AI/ML action plan may create additional AI-specific reporting duties -->, aviation regulators for AI in flight-critical systems, transport regulators for autonomous-driving stacks, energy regulators for AI in grid-critical infrastructure. Each has a dedicated regulatory-affairs seat; each is named in the architect's SOP fan-out table.

The design principle is that a single incident-fact record — one authoritative document of record, owned by the analyst, versioned, timestamped, and citation-referenced by every filing — feeds all of these. The AI Act filing cites the fact record at version 1.2. The GDPR filing cites the fact record at version 1.2. The customer letter cites the fact record at version 1.2. When the fact record moves to version 1.3 because a subsequent investigation revised the root-cause hypothesis, all filings scheduled after that moment cite 1.3, and all prior filings are followed up with revised statements as their own regime requires. Divergence between filings is architecturally prevented because there is no other place from which any filing can source facts.

## The four invariants

Every SOP-shape decision the architect makes preserves these four invariants. Any change to the workflow (a new obligation added, a role reassigned, a bridge shape reshaped) is tested against them before ratification.

**Invariant i — single incident id.** Every parallel obligation, every artefact, every log, every filing binds to one authoritative id. The SOC ticket references it. The PMS store references it. The regulator filings reference it. The legal case-management system references it. The customer-communications tracker references it. The CAPA record references it. The risk-rescore references it. Cross-system traceability is a first-class requirement of the workflow architecture, not a nice-to-have. Failure mode: parallel filings on the same incident go under different case ids in different systems and the enterprise cannot, six months later, produce a coherent record of what it did.

**Invariant ii — clock-start time is explicitly logged and traceable.** The moment of awareness is defined in the SOP ("the earliest point at which any named seat inside the enterprise became aware of facts that would, on reasonable interpretation, indicate a serious incident"), recorded as a first-class timestamped attributable field at intake, and every regulatory-clock calculation derives from it. Ambiguity about when the enterprise first became aware is a designed-away failure mode. The first-responder-on-call is trained and instructed to log this field first, before any operational-response action, because retroactive reconstruction of the moment of awareness is where the enterprise loses days of the statutory window.

**Invariant iii — designated decision-authority per obligation, not a committee.** For each filing, exactly one seat signs. The head of AI governance signs the AI Act filing. The DPO signs the GDPR filing. The CISO signs cyber-regulator filings. The CFO and general counsel jointly own the SEC materiality determination with the general counsel signing the 8-K. Sector regulator filings each have a named signatory in the SOP's obligation-to-signatory table. No filing lands on "the committee" because committees do not sign — committees debate, and debate under a 72-hour clock is where enterprises miss statutory windows. The bridge advises; the named seat decides.

**Invariant iv — post-incident review feeds CAPA and re-scores the risk.** Every serious incident produces at least one CAPA record (per mod-105 chapter 09) and at least one risk-register score change against the mod-106 taxonomy. The workflow does not close until both exist and the post-incident review artefact is filed. This invariant is what turns Article 73 compliance from a paper-trail exercise into a feedback loop. Failure mode: the enterprise files thirty AI Act notifications over five years and the residual-risk profile of the estate looks unchanged because none of the incidents ever caused a rescore or a CAPA.

## The three failure modes

**Failure mode (a) — parallel filings with divergent facts.** The AI Act filing says the model was retrained on 2027-02-14. The GDPR filing says 2027-02-15. The customer letter says early February. All three come from the same incident and none reconcile. Six months later, the market surveillance authority, the lead supervisory authority under the GDPR, and a plaintiff's counsel in a class action are comparing notes and the enterprise cannot explain the discrepancies. Architected defence: single incident-fact record, single owner (the analyst), all filings derived from that record with version citation. Divergence is impossible unless a filing goes out that did not cite the fact record — which the SOP forbids and the legal-review step verifies.

**Failure mode (b) — clock-start ambiguity.** The statutory window becomes wider than the regulator intended because the moment of awareness was never logged. When the regulator later asks when the enterprise first became aware, the enterprise cannot produce an authoritative answer, and the regulator's assumption about the clock start is unfavourable. Architected defence: the intake step MUST log the moment of awareness as a first-class field, timestamped and attributable to a named individual, before any other action is taken. The first-responder-on-call rota is trained on this discipline and the SOP's intake form makes the field mandatory-with-justification if it cannot be filled in the first minute.

**Failure mode (c) — "we'll deal with the regulator part later".** The operational incident-response consumes the whole first week — engineers are rolling back the model, disabling the tool-calling permission, drafting the customer-facing hotfix. The regulatory-notification obligation is remembered at day nine, when the statutory window has expired. Architected defence: the SOP fires the regulator-notification-preparation stream in parallel with the operational response, not after. The bridge convenes with legal and the analyst on-seat from the start; the evidence packet begins accumulating at intake; the decision-to-notify point falls well inside the statutory window because everything upstream of it has been running in parallel with the fix.

## The workflow schematic

```yaml
workflow:
  id: SOP-INC-A73-v2.0.0
  owner: senior-ai-governance-architect (level 50)
  ratifies: head-of-ai-governance (level 60); audit-committee (annually)
  trigger:
    any_of:
      - pms_classification: tier-4 (serious) [chapter 01]
      - external_notification_received: customer | regulator | media | researcher
      - soc_severity: sev-1 with ai-system nexus [chapter 05]
  clock:
    start_field: moment_of_awareness
    start_recorder: first-responder-on-call (rota)
    definition: earliest point at which any named seat became aware of facts
      that would, on reasonable interpretation, indicate a serious incident
  bridge:
    convene_within: <needs-research: window from intake>
    seats:
      chair: head-of-ai-governance (or delegate)
      technical_lead: ai-evaluation-engineer OR platform-lead
      risk: ai-risk-engineer
      legal: incident-response-counsel
      comms: pr-and-comms-lead
      soc: ciso-delegate
      workflow_reference: senior-ai-governance-architect
      fact_record_owner: ai-governance-analyst
      dpo: on-call (activates if personal-data nexus)
    coverage: follow-the-sun rota; no timezone-driven stall
  parallel_streams:
    - id: STREAM-OPS
      name: operational_response
      seats: secops + first-line + platform
    - id: STREAM-REG
      name: regulatory_notification_prep
      seats: legal + architect-designed-packet + analyst
    - id: STREAM-COMMS
      name: stakeholder_comms
      seats: pr + comms + customer-success
  decisions:
    - id: DEC-notify-a73
      obligation: EU AI Act Article 73
      owner: head-of-ai-governance
      informed_by: legal, architect-evidence-packet, risk-engineer-scoring
      deadline: <needs-research: A73 timeframe by severity category>
    - id: DEC-notify-gdpr-a33
      obligation: GDPR Article 33
      owner: dpo
      fires_if: personal_data_nexus
      deadline: 72h from awareness (verify against DPA guidance)
    - id: DEC-sec-materiality
      obligation: SEC cybersecurity disclosure rule (2023)
      owner: cfo + general-counsel (jointly)
      fires_if: listed_enterprise AND material
      deadline: <needs-research: Item 1.05 8-K trigger and window>
    - id: DEC-sector-notify
      obligation: sector-regulator regime (per sector)
      owner: sector-regulatory-affairs-lead
      fires_if: sector_regime_applies
      deadline: <needs-research: per sector>
    - id: DEC-customer-notify
      obligation: enterprise-B2B contract clauses
      owner: customer-success + contracts
      fires_if: contract_notice_clause_applies
      deadline: per contract
  artefacts:
    single_incident_id:
      generator: intake-step
      referenced_by: all systems, all filings, all logs
    incident_fact_record:
      owner: ai-governance-analyst
      versioned: semver
      cited_by_version: every filing
    regulator_evidence_packet:
      template_owner: senior-ai-governance-architect
      content_owner: legal (approves prior to filing)
      substrate: incident_fact_record
      data_lake_reference: [chapter 03]
    capa_record:
      obligation: mod-105 chapter 09
      required: >= 1 per serious incident
    risk_rescore:
      obligation: mod-106 taxonomy score change
      required: >= 1 per serious incident
    post_incident_review:
      feeds: mod-107 chapter 03 ongoing-assurance re-assessment
  close_condition:
    all_of:
      - regulator_ack: received for every filed obligation
      - capa_record: opened
      - risk_rescore: filed
      - post_incident_review: filed
      - stakeholder_comms: completed per plan
```

## Cross-references

The workflow this chapter designs sits inside the broader architecture the module and the track fix. Intake is the PMS store from chapter 01. The rescore feeds the risk-register writes downstream of the aggregation model in chapter 02. The evidence packet cites the enterprise data-lake reference established in chapter 03. When the incident involves attack, exfiltration, or model-supply-chain compromise, the SOC-side workstream in chapter 05 fires alongside this SOP and shares the same incident id. CAPA discharge follows the corrective-and-preventive-action architecture in mod-105 chapter 09. Severity classification uses the taxonomy authored in mod-106. The post-incident review's re-assessment output flows into the ongoing-assurance programme in mod-107 chapter 03, where a serious incident is one of the named triggers for re-scoping the assurance plan.

## Summary

Article 73 obligates providers of high-risk AI systems to notify the market surveillance authority of the member state where a serious incident occurred, on timeframes tied to severity that the architect must verify at authoring time. The workflow that discharges this obligation is a level-50 architect's design artefact — an SOP that names the intake trigger, defines the moment of awareness as a first-class field, convenes an incident bridge with named seats and timezone coverage, fires operational-response and regulatory-notification-prep and stakeholder-comms as parallel streams, drives named-seat decisions per obligation, and closes only when a CAPA record and a risk rescore both exist. Role separation is strict: the architect designs the SOP; the head of AI governance signs the AI Act filing; the analyst owns the incident fact record; legal drafts the filing text. Parallel obligations — GDPR Article 33, SEC materiality and 8-K disclosure, sector-regulator regimes, customer-contract notice clauses, sector-safety authorities — fan out across the same single fact record so filings cannot diverge. Four invariants hold: single incident id, explicit clock-start log, per-obligation named decision-authority, post-incident review feeds CAPA and rescore. Three failure modes are designed against: divergent parallel filings, clock-start ambiguity, "regulator part later." The workflow schematic is the architect's authored artefact — the anchor for exercise-04 in this module and the interface the certification body, the market surveillance authority, and internal audit will each ask to walk through.
