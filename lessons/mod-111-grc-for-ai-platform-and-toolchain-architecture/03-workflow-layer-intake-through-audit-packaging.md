# The workflow layer — the seven first-class flows the platform runs on

## Why this chapter exists

Every enterprise the level-50 architect has ever stood a GRC-for-AI platform up inside already has a workflow engine somewhere in its stack — the ticketing system's state machines, the enterprise GRC platform's approval flows, the ITSM change-management pipeline, the compliance team's spreadsheet-with-conditional-formatting. The temptation, when the GRC-for-AI platform's vendor deck shows a generic workflow builder with drag-and-drop transitions, is to treat governance workflows as another instance of the same primitive: open a ticket, assign it, route it through some approvers, close it. The architect who yields to that temptation ships a platform where the seven flows the AI governance programme actually depends on — intake, impact assessment, control testing, evidence collection, exception handling, incident routing, and audit-facing packaging — are each re-invented by each business unit in email, Confluence, and Jira, with no consistent state model, no ratified transitions, and no audit trail the ISO/IEC 42001 certification body can replay. The single-source-of-truth invariant chapter 01 ratified — that the GRC-for-AI platform is the authoritative home for AI-nexus governance artefacts — collapses because the state transitions those artefacts undergo happen outside the platform, in tools whose logs the platform never reads.

The sibling failure mode is the opposite: the vendor's product bakes the seven flows into hard-coded C# (or Java, or a proprietary rule DSL) with no versioning surface exposed to the customer, so when the vendor pushes a routine platform upgrade the audit-critical state transitions silently move — an approval that used to require two seats now requires one, an expiry timer that used to fire at ninety days now fires at one-eighty, a pre-close gate that used to demand attached evidence now accepts a text note. The customer's third-line internal audit function discovers the drift six months later when the sample they pull no longer matches the workflow-shape their walkthrough documentation was written against. `<!-- needs-research: verify which enterprise GRC platforms in the class named in chapter 01 expose workflow definitions as versioned, exportable, ratifiable artefacts vs which hold them as vendor-managed internal state -->` Both failure modes have the same root: workflows were not treated as first-class, versioned, ratified objects on the same footing as the mod-102 control library, the mod-106 taxonomy, or the chapter-04 RBAC bundles.

This chapter designs against both. It names seven flows the platform runs on as first-class objects, walks each one's inputs, state transitions, artefacts, sibling handoffs, RBAC bundle, and SLA, then commits the workflow-layer discipline: workflows are versioned artefacts, ratified under the same authority that ratifies the reference architecture (chapter 01) and the RBAC model (chapter 04), pinned per instance for audit-time replay, and terminated on the enterprise integration edges chapter 01 already defined — the ticketing platform, the communications platform, the identity provider, and the enterprise document management.

The material assumes chapter 01's stance on enterprise integrations as the notification and task surface (the platform does not build a competing task queue or notification channel), and chapter 04's stance on RBAC as attribute-driven, seat-and-scope shaped, with SoD enforced at both assignment and action time. The workflow layer sits on top of both: every transition consults the RBAC layer for permitted-transition proof, and every terminal transition emits into the enterprise integration edge that owns the downstream surface.

## Flow 1 — INTAKE

Intake is the flow that fires when a new AI system arrives from any of three legitimate upstream sources: the enterprise procurement platform (a vendor-supplied AI system passing through the mod-109 third-party inventory pipeline), the ML platform's model registry (a first-party model version registered per chapter 01's ML-platform integration), or a business-unit proposal submitted through the platform's intake form (a system whose build-vs-buy decision is still open). The flow accepts as input the source event plus a minimal payload — a proposed `system_id`, an owning business unit, a proposed nature-of-processing summary — and it advances the system's record from `proposed` through `classified-provisional` to `intake-complete`.

The flow produces three artefacts. First, the mod-102 initial control classification — the shape mod-102 exercise 05 defines, applied here to a system rather than a control, which pre-selects the control families the system will be measured against. Second, the mod-106 tier-assessment seed — the tier field is initialised to `unset` and a task is opened on the impact-assessment flow (Flow 2) with the classification-provisional record as its input. Third, the mod-108 evidence-index seed — the index is opened with the artefact classes the initial classification implies (model cards, DPIA references, evaluation-run pointers), each in `pending` state with a freshness contract stub.

The flow fires four sibling handoffs. Ticketing (per chapter 01) receives a ticket for the intake owner with the governance-record identifier tagged; the enterprise IdP is consulted for the group-membership that resolves the owning-business-unit's `ai-governance-analyst` seat and provisions the ownership assignment; the mod-109 third-party inventory is written to when the source is procurement, joining the intake record to the third-party record; and the mod-106 register is opened with a stub entry the impact-assessment flow will complete.

The RBAC bundle authorising transitions here is `bundle:intake-and-triage` (chapter 04), held by the `ai-governance-analyst` seat, scoped to the owning business unit. The SLA discipline: `proposed → classified-provisional` within five business days of the source event; `classified-provisional → intake-complete` within ten business days, extendable once by the `head-of-ai-governance` seat with documented rationale. SLA breach fires an escalation into the enterprise communications platform routed by the same IdP-group-driven pattern chapter 01 established.

## Flow 2 — IMPACT ASSESSMENT

The impact-assessment flow implements the ISO/IEC 42005-shaped impact assessment the AI programme owes on every in-scope system. `<!-- needs-research: verify ISO/IEC 42005 clause references for the impact-assessment discipline and the specific documented-information obligation the standard names -->` The flow is *conditional on tier* from the mod-106 tier assessment — a tier-1 system runs the full assessment battery (fundamental-rights scan, affected-populations mapping, foreseeable-misuse enumeration, harm-severity scoring against the mod-106 taxonomy), a tier-2 system runs the compressed battery (affected-populations mapping and harm-severity scoring only), and a tier-3 system runs the light-touch battery (documented-consideration record without the fundamental-rights scan). The tier gate is read from the mod-106 register at flow entry and pins the assessment shape at that value for the duration of the assessment; a mid-flow re-tier is a separate event that either completes the current shape or restarts with the new shape, never silently upgrades.

The flow accepts as input the intake-complete record plus the mod-106 tier reading. It advances state from `impact-assessment-scheduled` through `impact-assessment-in-progress` and `impact-assessment-under-review` to `impact-assessment-ratified`. It produces the impact-assessment artefact itself, which becomes the mod-105 documented-information record `DI-IA-{system_id}-{version}`; the mod-105 index is updated with the pointer, the version pin, and the freshness contract (impact assessments carry a mandatory re-assessment cadence per tier — tier-1 annually, tier-2 every eighteen months, tier-3 every two years). It also produces an appetite-implication note that triggers a review on the mod-106 register; if the assessment surfaces a residual-risk projection above the appetite threshold the mod-106 chapter on appetite established, the flow blocks at `impact-assessment-under-review` pending appetite adjustment or additional treatment design.

Sibling handoffs: ticketing receives the review-cycle work-item for the `ai-evaluation-engineer` seat (second-line challenge on the assessment); the mod-105 documented-information index is written; the mod-106 register receives the appetite-implication note and, if the threshold is crossed, the appetite-alarm workflow is invoked on the register side. Communications routes an executive briefing to the `head-of-ai-governance` seat on ratification of tier-1 assessments.

The RBAC bundle authorising transitions: authorship of the assessment sits with `bundle:intake-and-triage` extended by an `impact-assessment-authorship` bundle held by the business-system-owner seat; second-line review sits with `bundle:evaluation-validation` held by the `ai-evaluation-engineer` seat; ratification sits with the `head-of-ai-governance` seat for tier-1 systems and with a delegated `bundle:tier-2-3-ratification` seat for lower tiers. The SoD matrix (chapter 04) blocks the authoring seat from also holding the second-line-review seat. SLA: tier-1 systems complete `impact-assessment-scheduled → impact-assessment-ratified` within forty-five business days of the intake-complete event; tier-2 within thirty; tier-3 within twenty.

## Flow 3 — CONTROL TESTING

The control-testing flow drives periodic or event-triggered testing of the mod-102 control library's applicable controls against each in-scope system. Periodic testing is scheduled from the control's ratified test cadence (each mod-102 control carries a `test_cadence` attribute — monthly, quarterly, semi-annually, or on-event); event-triggered testing fires from an operational-signal event that the mod-110 chapter 02 monitor authorship recognises as a test-invalidating change (a new model version deployed, a training-data refresh, a runtime configuration change to a control's substantiating tool).

The flow accepts as input the control-and-system pair plus the trigger (`schedule:{cadence-timestamp}` or `event:{event-id}`). It advances state from `test-scheduled` through `test-in-progress` and `test-under-review` to `test-effective` or `test-ineffective-remediation-opened`. When the terminal state is `test-effective`, the flow refreshes the mod-108 evidence-index entry for the control-and-system pair with the new test-run artefact, the fresh substantiation timestamp, and the pointer to the test-run detail. When the terminal state is `test-ineffective-remediation-opened`, the flow opens a remediation ticket in the ticketing platform and, if the ineffectiveness crosses a severity threshold the mod-106 chapter on residual-risk established, it also opens the exception-handling flow (Flow 5) to catch the gap while remediation runs.

The flow drives the sibling toolchain: the chapter 07 Singapore AI Verify integration for controls whose substantiation is an AI-Verify-run; the chapter 06 observability adjacency for controls whose substantiation is an observability-derived metric; and the chapter 05 runtime-security adjacency for controls whose substantiation is a security-signal (adversarial-input rejection rate, prompt-injection block-rate, model-extraction detection). The flow's design is to invoke the substantiating tool through the chapter-01 integration edge, receive the tool's output as evidence per the mod-108 evidence contract, and then advance state on the strength of the evidence rather than on the tester's opinion alone.

RBAC bundle: test execution sits with `bundle:monitor-authorship` extended by a `bundle:test-execution` held by the `ai-risk-engineer` seat (first-line); the second-line review that advances `test-under-review → test-effective` sits with `bundle:evaluation-validation` held by the `ai-evaluation-engineer` seat, and the SoD action-time enforcement (chapter 04) blocks the seat that executed the test from also signing its effectiveness. SLA: monthly-cadence tests complete within five business days of the schedule fire; quarterly within ten; semi-annual within twenty; on-event tests within the event's runbook-defined window (typically two to five business days). Breach fires escalation into communications and, on repeated breach, escalates to a control-family review by the `senior-ai-governance-architect` seat.

## Flow 4 — EVIDENCE COLLECTION

Evidence collection is the mod-108 evidence-artefact write path expressed as a first-class flow rather than an ambient "someone attaches a file to a record" behaviour. The flow accepts as input a substantiation-need event — either scheduled (the mod-108 freshness contract on an existing evidence artefact is approaching expiry and the artefact must be re-produced) or unscheduled (a control test needs a fresh evidence artefact, an impact-assessment update needs a new supporting document, an audit-facing packaging request needs an artefact refresh). It advances state from `evidence-need-open` through `evidence-in-production` and `evidence-under-substantiation-review` to `evidence-substantiated` or `evidence-refused`.

The flow implements the producer→adapter→index pattern the mod-108 chapter on the evidence pipeline established. The *producer* is the tool or role that authors the evidence — an evaluation-harness run (chapter 07), an observability-metric extract (chapter 06), a security-signal aggregate (chapter 05), a model card produced by the ML team in Confluence, a DPIA produced by the privacy office in SharePoint. The *adapter* is the platform-side component that normalises the producer's output into the mod-108 evidence-artefact shape, resolves the document-pointer to the enterprise document management (chapter 01's DMS integration), and attaches the version pin and freshness contract. The *index* is the mod-108 evidence artefact index the platform holds as the pointer-and-substantiation registry — never the document copy.

The flow produces the evidence-artefact record with its stable identifier, the document pointer (not a copy — chapter 01's invariant 6), the producer reference, the freshness contract (the timestamp beyond which the artefact is considered stale), and the substantiation binding to the mod-102 control the evidence is offered for. The flow refuses to advance to `evidence-substantiated` when the substantiation binding fails — an evidence artefact offered against a control whose mod-102 substantiation-shape the artefact does not satisfy is bounced back into `evidence-in-production` with the deficiency named.

Sibling handoffs: the mod-108 index is the primary write target; ticketing receives the production work-item for the producer role; the enterprise DMS is the pointer target; and when the evidence is intended for an in-flight audit-facing packaging (Flow 7), the packaging flow is notified of the substantiation event so its bundle-assembly can pick up the fresh artefact.

RBAC bundle: production sits with the producer role's own bundle (varies by artefact class — `bundle:monitor-authorship` for observability-derived evidence, `bundle:intake-and-triage` for administrative-record evidence, `bundle:external-attestation-signature` for signed-attestation evidence); substantiation review sits with `bundle:evaluation-validation` held by the `ai-evaluation-engineer` seat. SoD action-time enforcement blocks the producer seat from signing the substantiation review on its own artefact. SLA: scheduled refreshes complete before the outgoing artefact's freshness contract expires — the flow opens the refresh at freshness-expiry-minus-thirty-days for artefacts on annual cadence, minus-fourteen-days for artefacts on quarterly cadence; unscheduled events driven by control-testing or audit-facing packaging complete within the driving flow's SLA.

## Flow 5 — EXCEPTION HANDLING

Exception handling fires when a control cannot be met on time or when a policy exception is required — the control-testing flow has advanced to `test-ineffective` and remediation will take longer than the appetite window permits, a newly-introduced obligation applies to a system whose control coverage is not yet complete, a vendor system in the mod-109 inventory cannot satisfy a control the enterprise's policy requires and the business-unit is willing to argue the compensating-controls case. The flow accepts as input the exception request payload — the specific control, the specific system, the requested exception window, the proposed compensating controls, the business justification, and the residual-risk projection.

It advances state from `exception-proposed` through `exception-under-review`, `exception-ratifying`, and `exception-active` to `exception-expired` or `exception-remediated-and-closed`. The mandatory attributes on every exception: a time-box (no exception is open-ended — the maximum window is calibrated to severity, with a policy-level ceiling of twelve months and a working default of ninety days), a set of compensating controls that carry their own substantiation contract for the duration of the exception, a ratifying seat calibrated to severity (low-severity exceptions ratifiable by the business-system-owner seat with `ai-evaluation-engineer` concurrence, medium-severity by the `senior-ai-governance-architect`, high-severity by the `head-of-ai-governance`), and a mandatory pre-expiry review scheduled at expiry-minus-thirty-days that either closes the exception through remediation-completion or opens a renewal exception (which is itself a new exception with a fresh ratification cycle — never a silent roll-forward).

The flow produces the exception artefact as a mod-105 documented-information record, the compensating-controls entry as a mod-102 register annotation with the exception's stable identifier, and the residual-risk uplift as an entry on the mod-106 register carrying the exception reference. Every state transition writes an immutable audit-log event; the audit log is a first-class artefact the third-line audit samples directly. Sibling handoffs: ticketing receives the ratification work-item routed to the severity-appropriate seat; communications routes the exception-active notification to the business-unit stakeholders and the head-of-ai-governance; the mod-108 evidence index carries the compensating-controls substantiation for the duration of the exception; the pre-expiry review is scheduled into ticketing at ratification time (not at expiry-minus-thirty — the scheduling itself is an artefact of ratification).

RBAC bundle: proposal sits with the business-system-owner seat's bundle; review sits with `bundle:evaluation-validation`; ratification sits with the severity-calibrated seat named above; expiry-management sits with a dedicated `bundle:exception-lifecycle-management` held by the `ai-governance-analyst` seat but restricted to non-substantive actions (scheduling the review, capturing the review outcome, closing the exception on remediation) — the substantive re-ratification is always the severity-calibrated seat. SoD action-time enforcement blocks the proposing seat from being the ratifying seat. SLA: `exception-proposed → exception-active` within ten business days for low-severity, five for medium, three for high; the pre-expiry review is not SLA-driven but calendar-driven — thirty days before expiry, no exceptions.

## Flow 6 — INCIDENT ROUTING

Incident routing is the flow that fires when an operational-signal event arrives that may constitute an incident — the mod-110 chapter 04 Article 73 serious-incident classifier flags an event, the mod-110 chapter 05 SOC handoff routes an AI-nexus security event, a user-report submitted through the enterprise support channel arrives with an AI-related complaint, or a vendor advisory from a third-party AI provider in the mod-109 inventory arrives with a disclosed defect. The flow accepts as input the incident-candidate event with its source, its provisional severity, and its provisional AI-nexus classification.

The flow advances state from `incident-candidate` through `incident-classified`, `incident-triaged`, and `incident-in-response` to `incident-resolved` or `incident-escalated-to-external-notification`. Classification maps the candidate against the incident taxonomy the mod-106 register and the mod-110 chapter 04 workflow jointly maintain, and against the AI-nexus flag that determines whether the event routes to the AI-programme SOP set or into the enterprise's general-purpose incident-response track. Triage assigns severity per the joint mod-106 + mod-110 taxonomy and routes to the appropriate SOP: an Article 73 serious-incident SOP for events crossing the EU AI Act materiality threshold (the mod-110 chapter 04 detail), a SOC-owned AI-security SOP for events routed through the chapter 05 interface (mod-110 chapter 05), a user-report SOP for events arriving through the enterprise support channel with a defined acknowledgement window, or a vendor-advisory SOP for events arriving through the mod-109 third-party interface.

The flow produces the incident record as a first-class entry on the mod-110 PMS event store (per chapter 01's invariant 5, the register-tier resides on the GRC-for-AI platform while the raw telemetry resides in the enterprise data lake), the classification-and-triage decisions as annotations, the response actions as their own audit-logged transitions, and — when the incident escalates to external notification — the notification package as a mod-105 documented-information record. Sibling handoffs: ticketing receives the response work-item routed by SOP; communications routes the escalation notice through the IdP-group-driven pattern; the mod-110 register is the primary write target; the mod-106 register receives a residual-risk uplift entry when the incident modifies the running risk profile; and, for Article 73 events, the regulator-facing counsel seat is notified through communications with a deep link to the incident record.

RBAC bundle: classification and triage sit with `bundle:monitor-authorship` extended by an `bundle:incident-triage` held by the `ai-risk-engineer` seat (first-line); response actions run under the SOP-appropriate bundle (SOC-owned actions under the `ai-infra-security` seat's bundle, business-owner actions under the business-system-owner seat's bundle); external-notification approval sits with the `head-of-ai-governance` seat. SoD action-time enforcement blocks the seat that authored the offending model version from being the seat that triages or closes the resulting incident. SLA: `incident-candidate → incident-classified` within four hours for events crossing the Article 73 threshold, within one business day for other AI-nexus events; classified → triaged within twenty-four hours for high-severity, within three business days for medium; response and resolution follow the SOP-specific windows.

## Flow 7 — AUDIT-FACING PACKAGING

Audit-facing packaging is the flow that materialises the audit bundle a certification body (an ISO/IEC 42001 auditor), a regulator (an EU AI Act competent-authority Article 72 post-market-surveillance evidence request, an SR 11-7 model-validation supervisor's sample), or the third-line internal audit function requires. The flow's core technical challenge is that the bundle must be traceable to a *point-in-time state* of the platform — the auditor asks for the state as it stood on a specific date, not as it stands today, and every artefact in the bundle must be pinned to its version-as-of-that-date rather than its current version.

The flow accepts as input the audit request payload — the requesting party, the scope (which systems, which controls, which time window), the specific evidence classes requested, and the point-in-time timestamp against which the bundle is to be assembled. It advances state from `packaging-requested` through `packaging-scope-ratified`, `packaging-in-assembly`, and `packaging-under-review` to `packaging-delivered` or `packaging-refused-out-of-scope`.

The assembly joins across the mod-102 control library (the controls in scope at the requested point-in-time, at their version-as-of-that-date), the mod-105 documented-information index (the AIMS artefacts referenced by the in-scope controls, at their version-as-of-that-date), the mod-106 register (the risk entries associated with the in-scope systems, at their version-as-of-that-date), the mod-107 assurance artefacts (the assurance opinions and their supporting evidence, at their version-as-of-that-date), the mod-108 evidence index (the evidence artefacts substantiating the in-scope controls, at their version-as-of-that-date), the mod-109 third-party inventory (the third-party AI systems in the request scope, with their vendor-assurance state as-of-that-date), and the mod-110 chapter 04 and chapter 05 event stores (the incidents and security events touching the in-scope systems within the request time window). Every artefact carries its version pin and its document pointer resolves to the DMS-held original at that pin — the bundle is a pointer-collection with substantiating metadata, not a document dump.

The flow produces the audit bundle as a mod-105 documented-information record `DI-AUDIT-PACK-{request-id}-{version}`, a bundle-manifest that names every included artefact with its version pin and the join keys that establish traceability, a chain-of-custody log capturing every seat that touched the bundle during assembly and review, and — for external requests — a delivery-envelope through the regulator-facing counsel seat that carries the legal-review confirmation. Sibling handoffs: ticketing receives the assembly work-items for the analyst seat; communications routes the delivery-ready notification to the requesting party's contact; the mod-108 evidence index is queried at point-in-time; and the mod-110 chapter 05 SOC interface is queried when the request scope includes security events.

RBAC bundle: assembly sits with a dedicated `bundle:audit-packaging-assembly` held by the `ai-governance-analyst` seat under the enterprise-scoped `senior-ai-governance-architect`'s supervision; review sits with `bundle:evaluation-validation`; delivery sign-off sits with the `head-of-ai-governance` seat with regulator-facing counsel countersigning for external deliveries. External assurance auditor seats (chapter 04's read-only, time-bounded, sampling-scoped persona) may be provisioned to consume the bundle in-platform rather than as a static export. SLA: `packaging-requested → packaging-delivered` within twenty business days for scheduled audits, five for regulator-driven urgent requests, two for third-line internal-audit sampling.

## The workflow-as-first-class-object discipline

Everything above only holds if workflows themselves are first-class objects on the same footing as the mod-102 control library, the mod-106 taxonomy, and the chapter-04 RBAC bundles. Treating a workflow as an admin-configured runbook — a set of transitions someone with the right platform-admin role can adjust from the web UI without going through a ratification cycle — is what re-introduces both failure modes the chapter opened with. The runbook drifts silently, the audit trail cannot replay the transitions the record actually went through at the time it went through them, and the third-line audit's walkthrough documentation stops matching the platform's behaviour.

The discipline: each workflow is a versioned artefact with a `WFL-{flow-id}-v{semver}` identifier. Definition changes follow a semver-shape versioning discipline — a patch increment for editorial changes that do not alter transitions or gates, a minor increment for adding an optional transition or extending an SLA, a major increment for any change to a permitted-transition, a ratifying-seat requirement, an SLA that shortens, or an artefact-emission that adds a downstream obligation. Every major-version change carries a deprecation window during which both the outgoing and incoming versions are supported, and every workflow instance is pinned to the workflow version it started under — a record intake-opened under WFL-INTAKE-v2.1 completes under v2.1 even if v3.0 is ratified mid-flight. The pin is what makes audit-time replay possible: the audit-facing packaging flow (Flow 7) reads the pinned workflow version alongside the pinned artefact versions and reconstructs the state transitions as they were legitimate at the time.

Ratification sits with the same authorities the reference architecture (chapter 01) and the RBAC model (chapter 04) sit under: the `senior-ai-governance-architect` proposes and ratifies; the `head-of-ai-governance` co-ratifies at the major-version boundary and for any change to a ratifying-seat requirement; the third-line internal audit function reviews at the major-version boundary and holds a documented veto on changes that reduce audit-log discipline. Deprecation windows are ratified alongside the incoming version; the outgoing version is not removed until every open instance under it has terminated or been migrated with documented rationale.

Vendors whose products hold workflows as vendor-managed internal state — invisible-to-customer, upgraded-by-vendor, unversioned-to-audit — fail this discipline structurally. `<!-- needs-research: verify which enterprise GRC and GRC-for-AI vendors expose workflow definitions as first-class, versioned, exportable, customer-ratifiable artefacts, and which hold them as internal state whose upgrade is vendor-driven -->` The vendor-evaluation matrix (chapter 02) scores this capability explicitly; the architect who accepts a platform without it accepts the sibling failure mode the chapter opened with.

## The seven-flow schematic

The schematic below is illustrative of the shape. Enterprises adapt field names and enumerations; the shape is what generalises.

```yaml
workflow_catalog:
  id: WFL-CATALOG-v1.0
  ratified_by: [ senior-ai-governance-architect, head-of-ai-governance ]
  audit_reviewed_by: third-line-internal-audit
  flows:
    - id: WFL-INTAKE-v2.1
      owner_role: ai-governance-analyst (level 15)
      inputs:
        - source: procurement | ml-platform-registry | business-unit-proposal
        - payload: proposed_system_id, owning_bu, nature_of_processing_summary
      outputs:
        - mod-102 initial control classification
        - mod-106 tier-assessment seed
        - mod-108 evidence-index seed
        - ownership-seat provisioning event
      sla: proposed→classified-provisional 5bd; classified-provisional→intake-complete 10bd
      ratification: senior-ai-governance-architect + head-of-ai-governance
      dependencies: [ mod-102, mod-106, mod-108, mod-109, chapter-01-idp ]
      sibling_handoffs: [ ticketing, comms, mod-109-inventory, mod-106-register ]

    - id: WFL-IMPACT-ASSESSMENT-v3.0
      owner_role: business-system-owner (co-authored) + ai-evaluation-engineer (review)
      inputs:
        - intake-complete record
        - mod-106 tier reading (pins assessment shape)
      outputs:
        - impact-assessment artefact (mod-105 DI-IA-{system_id}-{version})
        - appetite-implication note into mod-106 register
        - freshness contract (tier-1 annual / tier-2 18mo / tier-3 2yr)
      sla: tier-1 45bd / tier-2 30bd / tier-3 20bd end-to-end
      ratification: senior-ai-governance-architect + head-of-ai-governance
      dependencies: [ mod-105, mod-106, ISO-IEC-42005 ]
      sibling_handoffs: [ ticketing, comms, mod-105-index, mod-106-register ]

    - id: WFL-CONTROL-TESTING-v2.4
      owner_role: ai-risk-engineer (execute) + ai-evaluation-engineer (review)
      inputs:
        - control-and-system pair
        - trigger: schedule:{cadence} or event:{event-id}
      outputs:
        - test-run artefact into mod-108 evidence index
        - test-effective | test-ineffective terminal state
        - remediation ticket + optional Flow 5 invocation on ineffective
      sla: monthly 5bd / quarterly 10bd / semi-annual 20bd / on-event 2-5bd
      ratification: senior-ai-governance-architect + head-of-ai-governance
      dependencies: [ mod-102, mod-108, chapter-05, chapter-06, chapter-07 ]
      sibling_handoffs: [ ticketing, comms, mod-108-index, mod-110-monitors ]

    - id: WFL-EVIDENCE-COLLECTION-v2.2
      owner_role: producer-varies + ai-evaluation-engineer (substantiation)
      inputs:
        - substantiation-need event (scheduled or unscheduled)
      outputs:
        - evidence-artefact record (pointer, not copy)
        - freshness contract
        - substantiation binding to mod-102 control
      sla: scheduled refresh before freshness expiry (annual: -30d / quarterly: -14d); unscheduled per driving flow
      ratification: senior-ai-governance-architect + head-of-ai-governance
      dependencies: [ mod-108, mod-102, chapter-01-dms ]
      sibling_handoffs: [ ticketing, mod-108-index, enterprise-dms, Flow-7-if-in-flight ]

    - id: WFL-EXCEPTION-HANDLING-v2.0
      owner_role: business-system-owner (propose); severity-calibrated ratifier
      inputs:
        - exception request: control, system, requested-window, compensating-controls, justification, residual-risk projection
      outputs:
        - exception artefact (mod-105 DI-EXCEPT-*)
        - compensating-controls annotation on mod-102 register entry
        - residual-risk uplift on mod-106 register
        - mandatory pre-expiry review scheduled at ratification (expiry-minus-30d)
      sla: proposed→active 10bd low / 5bd medium / 3bd high; pre-expiry review calendar-driven (no exceptions)
      ratification:
        low-severity: business-system-owner + ai-evaluation-engineer concurrence
        medium-severity: senior-ai-governance-architect
        high-severity: head-of-ai-governance
      dependencies: [ mod-102, mod-105, mod-106, mod-108 ]
      sibling_handoffs: [ ticketing, comms, mod-105-index, mod-106-register, mod-108-index ]

    - id: WFL-INCIDENT-ROUTING-v3.1
      owner_role: ai-risk-engineer (classify/triage) + SOP-appropriate responder
      inputs:
        - incident-candidate event from mod-110 ch 04 Article 73 classifier | mod-110 ch 05 SOC handoff | user-report channel | mod-109 vendor advisory
      outputs:
        - incident record on mod-110 PMS event store (register-tier)
        - classification + triage annotations
        - response-action audit-logged transitions
        - external-notification package (mod-105 DI-INC-EXT-*) on escalation
      sla:
        Article-73 candidate→classified: 4h
        other AI-nexus candidate→classified: 1bd
        classified→triaged: 24h high / 3bd medium
        response/resolution: per SOP
      ratification: senior-ai-governance-architect + head-of-ai-governance (SOP set); ai-infra-security co-ratifies chapter-05 SOPs
      dependencies: [ mod-106, mod-110-ch-04, mod-110-ch-05, mod-109, chapter-05 ]
      sibling_handoffs: [ ticketing, comms, mod-110-register, mod-106-register, regulator-facing-counsel-on-Article-73 ]

    - id: WFL-AUDIT-FACING-PACKAGING-v2.0
      owner_role: ai-governance-analyst (assemble) under senior-ai-governance-architect supervision
      inputs:
        - audit request: requesting-party, scope, evidence-classes, point-in-time timestamp
      outputs:
        - audit bundle (mod-105 DI-AUDIT-PACK-*)
        - bundle manifest with version-pins and join keys
        - chain-of-custody log
        - delivery envelope (external requests) via regulator-facing counsel
      sla: scheduled audit 20bd / regulator-urgent 5bd / third-line internal-audit sample 2bd
      ratification: senior-ai-governance-architect + head-of-ai-governance; regulator-facing counsel countersigns external delivery
      dependencies: [ mod-102, mod-105, mod-106, mod-107, mod-108, mod-109, mod-110-ch-04, mod-110-ch-05 ]
      sibling_handoffs: [ ticketing, comms, external-assurance-auditor-seats-per-chapter-04 ]

  versioning:
    scheme: semver
    patch: editorial (no transition change)
    minor: optional transition added; SLA extended
    major: permitted-transition changed | ratifying-seat changed | SLA shortened | new downstream artefact obligation
    deprecation_window: both versions supported until every open instance terminates or is migrated with documented rationale
    instance_pinning: every flow instance pinned to workflow version at start; audit-time replay uses the pinned version

  invariants:
    - every flow instance carries a stable identifier + workflow-version-pin
    - every state transition writes an immutable audit-log event (seat, timestamp, version-pin, permitted-transition proof)
    - flows terminate on chapter-01 integration edges; no in-platform notification competes with enterprise stack
    - workflow versions are ratified artefacts under senior-ai-governance-architect + head-of-ai-governance
```

## Invariants

Four invariants the workflow layer holds; each is testable, and the layer is defective when any of them fails.

**Invariant 1 — every flow instance carries a stable identifier plus a workflow-version-pin.** The identifier is the join key every artefact the flow produces references; the version pin is the guarantee that audit-time replay uses the workflow shape the instance actually ran under. Failure mode: instances carry a workflow reference that resolves to "current version", the workflow is upgraded mid-flight to a version with a different permitted-transition set, and the audit-time walkthrough shows the record advancing through transitions the current workflow does not permit — the auditor cannot tell whether the transition was legitimate at the time or a defect after the fact.

**Invariant 2 — every state transition writes an immutable audit-log event with seat, timestamp, workflow-version-pin, and permitted-transition proof.** The permitted-transition proof is the record that the acting seat's RBAC bundle at the time (chapter 04) authorised the transition under the pinned workflow version — not "authorises now", but "authorised then". Failure mode: transitions are logged with the acting seat's current permissions rather than their permissions-at-time-of-action, and a subsequent RBAC change (a bundle upgraded, a seat's persona replaced) rewrites the historical authorisation chain. The third-line audit can no longer independently verify that the transition was permitted at the time.

**Invariant 3 — flows terminate on the chapter-01 integration edges.** No in-platform notification channel competes with the enterprise communications stack (Slack/Teams/email); no in-platform task queue competes with the enterprise ticketing platform (Jira/ServiceNow ITSM/Azure DevOps Boards); no in-platform document store competes with the enterprise document management (SharePoint/Confluence/Google Workspace/Box). Failure mode: the platform grows its own notification channel "for governance-critical alerts only", stakeholders whose primary attention is on the enterprise channels miss the alerts, and the platform-native alerts silently accumulate unacknowledged. Chapter 01's isolated-island failure mode returns through the notification surface.

**Invariant 4 — workflow versions are ratified artefacts under the level-50 architect and the level-60 head of AI governance.** No workflow change becomes effective without the ratification cycle; every major-version change carries a deprecation window; every instance is pinned at start; the third-line audit reviews at the major-version boundary. Failure mode: workflow definitions are held as vendor-managed internal state, upgraded silently on platform release, and the audit-critical transitions move without ratification. The customer's third-line audit function discovers the drift only through walkthrough-vs-observed-behaviour discrepancies, months after the drift began.

## Failure modes

Four failure modes the invariants close against.

**Failure mode (a) — generic workflow engine with no first-class flows.** The platform ships with a drag-and-drop workflow builder and no ratified catalog of the seven flows; each business unit and each enterprise deployment re-invents intake, control-testing, exception-handling, and audit-packaging in its own shape. Cross-BU aggregation is impossible because no two units' workflows share a state model; the audit-facing packaging flow cannot exist because there is no consistent state to package. The single-source-of-truth invariant chapter 01 ratified collapses into a per-BU-single-source-of-truth, which is not a single source at all. Prevented by ratifying the seven-flow catalog as a first-class artefact and refusing per-BU divergence beyond the versioning discipline.

**Failure mode (b) — exception handling as email.** The exception-handling flow (Flow 5) is not stood up as a first-class workflow; instead, exceptions are requested by email to the head of AI governance, granted by reply-all, tracked in a shared spreadsheet, and forgotten at expiry. There is no audit-log discipline, no compensating-controls substantiation, no pre-expiry review, no residual-risk uplift on the mod-106 register. The chapter-04 SoD action-time enforcement is bypassed entirely because the platform is not the authorising surface. Prevented by treating exception-handling as one of the seven first-class flows with the ratified compensating-controls-and-expiry discipline.

**Failure mode (c) — audit-facing packaging as ad-hoc export.** The audit-facing packaging flow (Flow 7) is not stood up; when the certification body arrives, the analyst pulls a CSV from the platform, screenshots dashboards, downloads documents from the DMS, and assembles the bundle by hand in a shared drive. Point-in-time reconstruction is impossible because no artefact carries its version-pin-as-of-audit-date; the join across mod-102, 105, 106, 107, 108, 109, and mod-110 is done by hand and drifts against the register; chain-of-custody is undocumented. The audit finding writes itself. Prevented by treating audit-facing packaging as the seventh first-class flow with the point-in-time join discipline and the manifest+chain-of-custody artefacts.

**Failure mode (d) — exception expiry silently rolled forward.** The exception-handling flow is stood up, but the pre-expiry review mechanism is soft — the analyst seat with the lifecycle-management bundle nudges the expiry date forward "administratively" when the responsible remediation has not completed, and no fresh ratification cycle is triggered. The originally-ninety-day exception silently persists for eighteen months. The residual-risk uplift on the mod-106 register becomes permanent without ever being reconsidered by the ratifying seat. Prevented by pinning the pre-expiry review as calendar-driven rather than SLA-driven, and by disallowing any lifecycle-management action that alters the expiry date without a fresh proposal-and-ratification cycle.

## Summary

The workflow layer designs seven first-class flows the GRC-for-AI platform runs on — intake, impact assessment, control testing, evidence collection, exception handling, incident routing, and audit-facing packaging — as versioned, ratified artefacts on the same footing as the mod-102 control library, the mod-106 taxonomy, and the chapter-04 RBAC bundles, rather than as generic workflow-engine configurations or vendor-managed internal state. Each flow names its inputs, its state transitions, the artefacts it produces, the sibling handoffs it fires into the chapter-01 integration edges (ticketing, communications, IdP, DMS, mod-108 index, mod-106 register), the chapter-04 RBAC bundle that authorises each transition under the SoD action-time enforcement, and its SLA discipline calibrated to the flow's regime obligations (ISO/IEC 42005 for impact assessment, ISO/IEC 42001 for AIMS documented-information, EU AI Act Articles 72 and 73 for post-market-surveillance and serious-incident notification, SR 11-7 for model-validation independence). The workflow-as-first-class-object discipline holds workflows as semver-versioned artefacts ratified by the `senior-ai-governance-architect` and `head-of-ai-governance` seats, pinned per instance for audit-time replay, and deprecated with windows during which both versions are supported. The seven-flow schematic `WFL-CATALOG-v1.0` names each flow's shape; four invariants (stable identifier plus version-pin per instance, immutable audit-log per transition with permitted-transition proof, termination on chapter-01 integration edges, ratified-artefact discipline) are testable; four failure modes (generic workflow engine without first-class flows, exception-handling as email, audit-facing packaging as ad-hoc export, exception-expiry silently rolled forward) are common enough that the layer is specifically designed against them. Chapter 04 fills in the RBAC bundles the transitions consult; chapter 05, chapter 06, and chapter 07 fill in the sibling toolchains the control-testing and evidence-collection flows compose with; the vendor-evaluation matrix (chapter 02) scores each candidate platform's workflow-as-first-class-object capability directly against this catalog.
