# Contract-template controls — turning DDQ commitments into enforceable clauses

## Why this chapter exists

A DDQ commitment the vendor makes over email or in a marketing PDF is not enforceable. A commitment the enterprise's contract carries is. The seam is where governance actually lives. An enterprise with an excellent DDQ discipline and no matching contract-template control set has a filing cabinet of vendor assertions and no legal recourse when the vendor's posture drifts. An enterprise with strong contract-template controls but no DDQ to inform them signs the same MSA every time regardless of the vendor's actual risk shape and has clauses in place that the vendor's operations do not actually support.

The level-50 architect owns the *control shape* the contract templates carry. The architect does not draft the enforceable clause text — that is legal's craft and the drafter's judgement about jurisdiction, enforceability, common-law posture, and the specific parties. What the architect ships is the *control catalog for third-party AI contracts*: for each control, the intent, the vendor commitment the clause encodes, the evidence the clause references, the enforcement lever the clause creates, the tier at which the control is mandatory, and the composition with the enterprise's standard master agreement and DPA.

This chapter designs the control catalog. Six control families cover the AI-specific surface (data-use, evaluation-access, incident-notification, evidence-access, exit / portability, and cross-cutting operational controls including version-change notification, subprocessor management, sanctions and regulator cooperation). For each family: what the control does, what makes it enforceable, the tier at which it is mandatory, and the coordination model with procurement and legal. Exercise-03 walks the drill of authoring the control shapes for a specific vendor scenario.

## What the contract-template control set is, structurally

The set is:

- **A catalog of control shapes.** Each control shape names its intent, its vendor commitment, its evidence reference, and its enforcement lever.
- **A tier-mapping.** Which controls are mandatory at tier-1 through tier-4; which are optional; which are conditional on vendor class or vendor-offered capability.
- **A template-composition rule.** How the AI-governance addendum composes with the enterprise's standard MSA, DPA, order form, and any regulated-sector schedules.
- **A drafting-handoff contract with legal.** The architect's control shape hands off to legal as *specified intent*, not as prescribed clause language.
- **A version-controlled artefact.** The catalog is versioned; every contract executed references the catalog version its clauses derive from; catalog updates go through second-line review and produce a governance-workflow log event.

## What it is not

- **The enforceable clause text.** The clause text is legal's product; jurisdiction, common-law posture, and negotiation dynamics govern the language. The catalog specifies the control shape the clause must implement.
- **A one-size-fits-all addendum.** A single AI addendum with 40 clauses that every vendor signs is (i) unnecessary for tier-1 vendors, (ii) insufficient for tier-4 vendors, and (iii) unlikely to survive the tier-4 vendor's own legal-team review. The catalog is tier-scaled and vendor-class-scaled.
- **A substitute for the enterprise's DPA.** The DPA covers data-processing shape under GDPR / CCPA / sector-specific law; the AI addendum extends the DPA with AI-specific data-use commitments but does not replace it.
- **Static.** Vendor market practice on AI-specific clauses is evolving quarter-over-quarter. Foundation-model providers have added, revised, and retracted commitments (public-training-corpus opt-outs, IP indemnity coverage, elections-integrity commitments, minor-safety commitments) at a rate that a static template cannot track. The catalog carries a review cadence.

## The drafting-handoff contract with procurement and legal

Three functions collaborate. The architect's slice, procurement's slice, and legal's slice are declared:

- **Architect (level 50) — control shape.** Names the control, its intent, its vendor commitment, its evidence reference, its enforcement lever, the tier at which it is mandatory. Ratifies the catalog. Reviews vendor-proposed carveouts against the control's intent.
- **Legal — enforceable drafting.** Converts the control shape into clause text; carries jurisdictional and negotiation judgement; drafts fallback language for negotiation. Owns the standard-form MSA and DPA the addendum composes with.
- **Procurement — template issuance and negotiation.** Selects the tier-appropriate template bundle for a specific vendor; issues to the vendor's negotiation counterpart; runs the negotiation; escalates carveouts to legal and to the architect for material-shape issues.
- **AI-accountable executive — exception approval.** Approves the residual risk where the enterprise accepts a vendor-proposed carveout that breaks a mandatory control.

The handoff is *declared* — not because functions do not know their roles, but because it forms the substrate for reviewing what actually shipped. A contract executed with a mandatory control missing and no executive-approved exception is a governance failure the substrate records.

## The six control families

### Family 1 — Data-use limits

*What can the vendor do with the enterprise's inputs (prompts, fine-tuning corpora, uploaded documents, telemetry) and outputs (completions, generated artefacts, derived embeddings)?*

Control shapes in this family:

- **DU-01 — No training on enterprise data without explicit opt-in.** The vendor may not use the enterprise's prompts, completions, fine-tuning corpora, or telemetry to train, fine-tune, or evaluate the vendor's models available to other customers, absent explicit written opt-in.
  - Intent: prevent the enterprise's confidential inputs from influencing outputs the vendor delivers to other customers.
  - Evidence reference: DDQ category 4 answers; vendor's published data-use policy version at signing.
  - Enforcement lever: material-breach termination; audit right to verify.
  - Tier mandatory: tier-2 and above.
- **DU-02 — Retention window on inputs and outputs is defined and short.** The vendor's retention window on prompts, completions, and any derived data is defined in the contract; the default for tier-3+ is short (30 days or less absent specific abuse-monitoring exception) with the exception scope defined.
  - Intent: bound the confidentiality exposure and the incident-blast-radius of a vendor breach.
  - Evidence reference: DDQ category 4 answers on retention; vendor's published data-retention documentation.
  - Enforcement lever: contract right to require deletion evidence; audit right; material-breach termination.
  - Tier mandatory: tier-2 and above.
- **DU-03 — Fine-tuning corpora treated as confidential; fine-tune weights not shared.** The vendor treats enterprise-supplied fine-tuning corpora as confidential; fine-tune weights are not shared with other customers or made available in the vendor's general offering.
  - Intent: prevent the enterprise's fine-tune from becoming part of the vendor's competitive offering.
  - Evidence reference: DDQ category 4 answers on fine-tuning.
  - Enforcement lever: material-breach termination; injunctive-relief threshold.
  - Tier mandatory: tier-2 and above where fine-tuning is used.
- **DU-04 — Human review of enterprise inputs restricted.** Human review of enterprise prompts, completions, or fine-tuning corpora by vendor personnel is restricted to defined circumstances (abuse investigation, contractually-agreed evaluation) with logged access.
  - Intent: bound the human-eyes exposure on enterprise inputs.
  - Evidence reference: DDQ category 2 and 4 answers on human review.
  - Enforcement lever: audit right to review access logs.
  - Tier mandatory: tier-3 and above.
- **DU-05 — Subprocessor limitation on inputs and outputs.** The vendor may not disclose enterprise inputs or outputs to subprocessors other than those on the contractually-agreed subprocessor list; new subprocessors require notice and objection period.
  - Intent: bound the chain-of-processors that touches enterprise data.
  - Evidence reference: DDQ category 2 answers on subprocessors.
  - Enforcement lever: right to object; material-breach termination on unauthorised disclosure.
  - Tier mandatory: tier-2 and above.

### Family 2 — Evaluation-access rights

*Can the enterprise run its own evaluations against the vendor's model, at the enterprise's cadence, with the enterprise's evaluation set, and can the enterprise share the results as needed?*

Control shapes in this family:

- **EA-01 — Right to run enterprise evaluations against the vendor's model.** The enterprise has the right to run its own evaluation suite against the vendor's model version(s) offered under the engagement, at the enterprise's cadence, without additional per-run fees beyond the vendor's standard API metering.
  - Intent: the enterprise's pre-deployment gate (mod-107 ch02) and ongoing monitoring (mod-107 ch03) depend on the enterprise's own evaluations. This right makes those evaluations exercisable.
  - Evidence reference: DDQ category 3 answers on evaluation methodology.
  - Enforcement lever: contract right; breach triggers material-breach cure period.
  - Tier mandatory: tier-3 and above.
- **EA-02 — Rate-limit and abuse-monitoring accommodation for enterprise evaluations.** The vendor's rate-limits and abuse-monitoring do not block the enterprise's disclosed evaluation programmes, including red-team and adversarial-evaluation runs.
  - Intent: prevent the vendor's abuse-monitoring from silently degrading the enterprise's evaluation programme (jailbreak evaluations look like adversarial abuse to a naive classifier).
  - Enforcement lever: disclosed-evaluation whitelist mechanism; escalation channel; SLA on unblocking.
  - Tier mandatory: tier-3 and above.
- **EA-03 — Right to disclose evaluation results.** The enterprise has the right to disclose its own evaluation results to its regulators, its auditors, and its customers (subject to defined limitations around vendor's IP that the enterprise agrees not to reverse-engineer or publish).
  - Intent: evaluation results are enterprise evidence artefacts; the enterprise's regulator will ask for them.
  - Enforcement lever: contract right; carveout on vendor's IP is bounded by defined categories.
  - Tier mandatory: tier-3 and above.
- **EA-04 — Access to vendor's evaluation methodology and disclosed results.** The vendor delivers, on request, the methodology and disclosed results for the vendor's own pre-release evaluations of the model version(s) offered.
  - Intent: enterprise's diligence needs the vendor's own evidence.
  - Evidence reference: DDQ category 3 answers.
  - Enforcement lever: contract right on delivery; audit right on completeness.
  - Tier mandatory: tier-3 and above.
- **EA-05 — Sub-service organisation or SOC 2 attestation reference.** Where the vendor's safety-evaluation function is a sub-service organisation for SOC 2 purposes, the vendor delivers the sub-service SOC 2 letter or an equivalent attestation.
  - Intent: independent-attestation evidence for the vendor's evaluation function.
  - Tier mandatory: tier-4.

### Family 3 — Incident-notification obligations

*What is the vendor's obligation to tell the enterprise when something goes wrong?*

Control shapes in this family:

- **IN-01 — Severity taxonomy and timelines.** The contract defines a severity taxonomy (typically 3 or 4 levels: critical, high, medium, low; or the vendor's own if compatible) and attaches maximum notification timelines to each level. For AI-specific safety incidents at the highest severity, timelines are typically hour-scale (24 hours or less to a defined enterprise contact); for security incidents, timelines follow the enterprise's DPA (72 hours under GDPR is a common floor); for operational incidents, day-scale timelines apply.
  - Intent: the enterprise's post-market surveillance (mod-110) and regulator-facing incident-reporting obligations (EU AI Act Article 73 for serious incidents; sectoral obligations for supervised industries) depend on timely vendor notification.
  - Enforcement lever: material-breach cure period on repeated late notifications; termination for cause on egregious cases.
  - Tier mandatory: tier-2 and above (with tier-scaled severity coverage).
- **IN-02 — Notification content.** The notification includes the incident summary, the impacted enterprise footprint (which model versions, endpoints, features, geographic scope), the vendor's current mitigation, the vendor's remediation plan, and the vendor's next-update commitment.
  - Intent: notification without actionable content is a checkbox; the enterprise cannot triage without the specifics.
  - Enforcement lever: contract right on content; escalation path.
  - Tier mandatory: tier-2 and above.
- **IN-03 — Post-mortem delivery.** For high-severity incidents, the vendor delivers a post-mortem summary within a defined window (typically 30–60 days) covering root cause, remediation, and preventive measures.
  - Intent: the enterprise's own post-market surveillance and lessons-learned discipline needs the vendor's root-cause understanding.
  - Enforcement lever: contract right on delivery.
  - Tier mandatory: tier-3 and above.
- **IN-04 — Regulator-cooperation commitment.** Where the enterprise's regulator requests information about a vendor-side incident affecting the enterprise's regulated activity, the vendor commits to cooperate on defined terms (direct communications with the regulator through the enterprise; timely provision of documentation; witness availability).
  - Intent: EU AI Act Article 73, sectoral incident-reporting obligations, and litigation discovery all reach into vendor-held information.
  - Enforcement lever: contract right on cooperation.
  - Tier mandatory: tier-3 and above; explicit in tier-4.
- **IN-05 — Cross-customer incident disclosure.** For incidents affecting multiple customers where enterprise's exposure is direct (a vendor-wide security breach; a vendor-wide model-behaviour incident), the vendor notifies the enterprise on the same terms as if the incident were enterprise-specific.
  - Intent: prevent the enterprise from learning about material exposure through press coverage rather than through the vendor.
  - Enforcement lever: contract right; material-breach cure period.
  - Tier mandatory: tier-3 and above.

### Family 4 — Evidence-access rights

*What evidence about the vendor's controls is the enterprise entitled to receive, and on what cadence?*

Control shapes in this family:

- **EV-01 — Attestation currency.** The vendor delivers, at signing and at defined refresh cadences, current SOC 2 Type II (or ISO 27001 or equivalent, per the vendor's applicable attestation), and where available, ISO/IEC 42001 attestation.
  - Intent: the enterprise's evidence substrate (mod-108) holds the vendor's attestation evidence at freshness.
  - Enforcement lever: contract right on delivery; escalation on gaps; annual currency requirement.
  - Tier mandatory: tier-2 and above.
- **EV-02 — Third-party audit reports.** For tier-4 vendors, the vendor delivers, on request under NDA, third-party audit reports (safety, security, evaluation) or an equivalent redacted summary.
  - Intent: independent-attestation evidence beyond the vendor's own assertions.
  - Enforcement lever: contract right on delivery under defined terms.
  - Tier mandatory: tier-4; tier-3 where available.
- **EV-03 — SOC 2 sub-service treatment or right-to-audit.** For tier-4 vendors, the enterprise has either (i) SOC 2 sub-service treatment (the vendor is treated as a sub-service organisation for the enterprise's own SOC 2 audit, with the letters flowing through) or (ii) a contractual right to audit (on-site or remote formal engagement) exercised on defined cadence and terms.
  - Intent: the enterprise's own audit posture inherits the vendor's controls; without one of these mechanisms the enterprise's SOC 2 has an unaddressed sub-service gap.
  - Enforcement lever: contract right on delivery of letters or on audit engagement.
  - Tier mandatory: tier-4; increasingly common at tier-3 for AI-specific vendors.
- **EV-04 — Vendor's AI governance programme evidence.** For tier-3 and tier-4 vendors, the vendor delivers on request evidence of its own AI governance programme (published AI policy, risk-management framework, incident-response programme, safety-evaluation practice).
  - Intent: composition with the vendor's own governance is more defensible than reliance on the vendor's marketing pages.
  - Enforcement lever: contract right on delivery.
  - Tier mandatory: tier-3 and above.
- **EV-05 — Supply-chain evidence.** For vendors delivering model artefacts, dataset artefacts, or fine-tunes, the vendor delivers the supply-chain evidence bundle (ML-BOM, SPDX, SLSA, Sigstore signature) per chapter 07.
  - Intent: the enterprise's model-registry ingestion policy (chapter 07) checks the bundle before promotion.
  - Enforcement lever: contract right; registry ingestion refuses without.
  - Tier mandatory: tier-2 and above for artefact-delivery vendors; the specific SLSA level scales with tier.

### Family 5 — Exit and portability

*What happens when the engagement ends, and can the enterprise leave without stranding data, prompts, fine-tunes, or evaluations?*

Control shapes in this family:

- **EX-01 — Data-export shape and window.** On termination or on request, the vendor exports enterprise data (prompts, completions, telemetry, uploaded documents, fine-tuning corpora) in defined formats within a defined window (typically 30–90 days), and confirms deletion of enterprise data from vendor's systems within a further defined window.
  - Intent: portability at cost proportional to the tier; deletion evidence bounds residual confidentiality exposure.
  - Enforcement lever: contract right on export and deletion evidence; audit right on deletion.
  - Tier mandatory: tier-2 and above.
- **EX-02 — Fine-tune weight portability.** For fine-tuning engagements, the vendor delivers the fine-tune weights (or a functionally-equivalent artefact) on termination, in a format the enterprise can use with another compatible platform (typically the base-model checkpoint hash plus the delta weights or the merged checkpoint).
  - Intent: fine-tuning creates enterprise-specific IP that should not be trapped in the vendor's platform.
  - Enforcement lever: contract right; where the vendor cannot deliver a portable format the enterprise carries a documented exit residual.
  - Tier mandatory: tier-3 and above where fine-tuning is used.
- **EX-03 — Prompt / integration portability.** The enterprise's prompt library, evaluation set, and integration configurations remain enterprise IP and are exportable in formats the enterprise can use with alternative vendors.
  - Intent: even without proprietary fine-tunes, the enterprise's prompt-engineering and evaluation-set investment is portable.
  - Enforcement lever: contract right.
  - Tier mandatory: tier-2 and above.
- **EX-04 — Transition-services agreement (TSA) shape.** For tier-4 vendors, a TSA template is pre-agreed at signing covering migration assistance, extended-service window, price continuity, and cooperation with the enterprise's replacement vendor.
  - Intent: exit is a project; the TSA is the project's contract.
  - Enforcement lever: contract right; pre-negotiated template reduces exit-time friction.
  - Tier mandatory: tier-4.
- **EX-05 — Extended evidence retention.** On termination, the vendor retains the enterprise's evidence (evaluation results, incident post-mortems, audit letters) for a defined post-termination window matching the enterprise's regulator-facing evidence-retention obligations (EU AI Act's technical documentation window typically at 10 years; sectoral obligations may extend).
  - Intent: the enterprise's evidence obligations do not end with the vendor engagement.
  - Enforcement lever: contract right; substrate ingest of terminal evidence at termination time is the primary mitigation.
  - Tier mandatory: tier-3 and above.
- **EX-06 — Termination-for-cause enumeration.** The contract enumerates termination-for-cause triggers specifically for the AI engagement: material safety incident above threshold; unauthorised model change to a pinned version; licence breach on enterprise data; failure of the data-transfer regime the DPA relies on; sanctions or export-control violation; vendor's insolvency.
  - Intent: standard MSA termination triggers are typically insufficient; AI-specific triggers make the exit route defensible.
  - Enforcement lever: termination right on defined triggers.
  - Tier mandatory: tier-3 and above.

### Family 6 — Cross-cutting operational controls

*Version-change notification, subprocessor management, sanctions / export-control cooperation, insurance, indemnity, and other operational clauses that bind on the AI-specific engagement.*

Control shapes in this family:

- **OP-01 — Material-change notification.** The vendor commits to notify the enterprise, with defined advance notice, on material changes: model or classifier or dataset version updates (definition of "material" is contract-defined); subprocessor list changes; ownership or control changes; key-personnel changes (tier-4); policy changes that affect the enterprise's use.
  - Intent: version-drift as a first-class monitored signal (invariant 3 from chapter 01) is meaningless without contractual notification.
  - Enforcement lever: contract right; material-breach cure on repeated failures.
  - Tier mandatory: tier-2 and above.
- **OP-02 — Version pinning where offered.** Where the vendor offers pinned versions or dated-snapshot endpoints, the enterprise's engagement uses them for the classes of use the enterprise designates.
  - Intent: eliminate silent version drift on regulator-facing systems.
  - Enforcement lever: contract right on pinned-version availability during the notice window; extended-support commitment on pinned versions the enterprise's system depends on.
  - Tier mandatory: tier-3 and above.
- **OP-03 — Subprocessor consent-and-notification.** Additions to the subprocessor list require advance notice and a defined objection period; enterprise's objection triggers negotiation or termination.
  - Intent: the enterprise's data-flow surface does not silently expand.
  - Enforcement lever: contract right; substrate ingest of subprocessor-list changes.
  - Tier mandatory: tier-2 and above.
- **OP-04 — Sanctions and export-control cooperation.** The vendor cooperates with the enterprise's sanctions-screening and export-control diligence, including on the vendor's own subprocessor chain.
  - Intent: sanctions listings and export-control changes affect model access unpredictably; the enterprise's own exposure requires vendor cooperation.
  - Enforcement lever: contract right.
  - Tier mandatory: tier-3 and above.
- **OP-05 — Indemnity and liability posture.** The contract carries an indemnity from the vendor for defined AI-specific exposures where the vendor offers such (IP-indemnity on outputs is common at frontier-model providers; other indemnities are engagement-specific). The liability cap is calibrated to the exposure.
  - Intent: risk-transfer where the vendor is the appropriate risk-bearer; the enterprise's residual is what remains.
  - Enforcement lever: standard indemnity mechanics; cap negotiated per engagement.
  - Tier mandatory: tier-3 and above (with negotiation depending on vendor's standard offer).
- **OP-06 — Insurance.** For tier-4 engagements where the enterprise's exposure exceeds the vendor's balance sheet, the vendor carries defined insurance coverage on the AI-specific exposures.
  - Intent: financial protection commensurate with the exposure.
  - Enforcement lever: certificate of insurance delivered annually; contract right on maintenance.
  - Tier mandatory: tier-4 where exposure warrants.
- **OP-07 — Model / weights escrow where feasible.** For tier-4 engagements where the vendor's business-continuity failure would materially impair the enterprise, source-code / model-weights escrow is considered and, where legally supportable, implemented.
  - Intent: BC/DR against the vendor's entity-level failure.
  - Enforcement lever: escrow-agent arrangement; release triggers defined.
  - Tier mandatory: considered at tier-4; implemented where feasible.

## The tier-mandatory matrix

The catalog's control-to-tier map:

```yaml
tier_mandatory_matrix:
  version: 1.0.0

  tier-1:
    mandatory: [DU-01 (light), IN-01 (medium+ severity only), IN-02, EV-01, EX-01, OP-01]
    total_mandatory: ~6 controls (light form)

  tier-2:
    mandatory: [DU-01, DU-02, DU-03 (if fine-tuning), DU-05, IN-01, IN-02, EV-01, EV-05 (if artefact delivery),
                EX-01, EX-03, OP-01, OP-03]
    total_mandatory: ~12 controls

  tier-3:
    mandatory: [DU-01, DU-02, DU-03 (if fine-tuning), DU-04, DU-05,
                EA-01, EA-02, EA-03, EA-04,
                IN-01, IN-02, IN-03, IN-04, IN-05,
                EV-01, EV-02, EV-04, EV-05,
                EX-01, EX-02 (if fine-tuning), EX-03, EX-05, EX-06,
                OP-01, OP-02, OP-03, OP-04, OP-05]
    total_mandatory: ~28 controls

  tier-4:
    mandatory: all tier-3 mandatory PLUS
                [EA-05, EV-03, EX-04, OP-06, OP-07 (where feasible)]
    total_mandatory: ~32 controls; some conditional

  exception_policy:
    permitted_carveouts:
      - a vendor-proposed carveout on a mandatory control requires:
        - documented in the executed contract with a specific limitation
        - approved by the AI-accountable executive for tier-4
        - approved by the head of AI governance for tier-3
        - recorded as a residual-risk carry in the enterprise risk register (mod-106)
        - substrate log entry
```

The matrix is the *shape*. The number and identity of mandatory controls per tier will vary by enterprise (a HIPAA-regulated enterprise, an EU-primary enterprise, a US-federal-contractor enterprise each have overlays that mandate additional controls). The tier-4 shape typically ships with 30+ mandatory controls in an AI-governance addendum that is 15–25 pages of concise clause text in a mature enterprise.

## The composition with MSA and DPA

The AI-governance addendum composes with the enterprise's standard master agreement and DPA:

```yaml
contract_composition:
  layers:
    - master_services_agreement (MSA):
        owns: general commercial terms, standard indemnity, warranty, IP, termination, liability cap, governing law
        AI extensions: enumeration of AI-specific termination-for-cause triggers (OP-05, EX-06)
    - data_processing_addendum (DPA):
        owns: GDPR / CCPA / sector data-processing terms, subprocessor list, international transfer mechanism
        AI extensions: DU-01, DU-02, DU-04, DU-05, OP-03 anchor here (data-use-specific), extending the DPA rather than a separate document
    - ai_governance_addendum:
        owns: AI-specific control set (EA, IN, EV, EX, OP families that don't fit MSA or DPA), the DDQ commitments as contract obligations
        composition: incorporates DDQ answers by reference (the vendor's DDQ response becomes a contract schedule)
    - order_form / statement_of_work:
        owns: specific engagement scope, pricing, term, service metrics
        AI extensions: engagement-specific tier declaration, engagement-specific control overrides (e.g., "for this engagement, DU-01 is opt-in NO for all classes of enterprise data")
    - sector_or_regime_schedules:
        - HIPAA business-associate agreement (BAA) where PHI flows
        - FedRAMP / StateRAMP schedule where federal or state government relationships apply
        - EU AI Act deployer-provider schedule where the vendor is a provider and enterprise is a deployer (or vice versa)
        - PCI DSS schedule where cardholder data flows

  drafting_precedence:
    - order_form overrides addendum on engagement-specific terms
    - addendum overrides MSA on AI-specific terms
    - sector schedule overrides addendum on sector-specific terms where more restrictive
    - MSA governs where no more-specific instrument speaks

  versioning:
    - catalog version referenced in the addendum's preamble
    - the addendum executed with a specific catalog version is an artefact; catalog upgrades do not retroactively bind executed contracts
    - contract-renewal review (chapter 05) is the trigger to migrate an executed contract to a newer catalog version
```

The composition is legal's craft in a specific jurisdiction; the architect specifies which controls land where, and reviews to confirm no mandatory control was dropped in the drafting.

## The DDQ-to-contract closure loop

A control shape only takes effect if the vendor's DDQ answers and the contract clauses are cross-referenced. The closure loop:

- Every DDQ question with a scored below-threshold answer generates either (i) a remediation obligation that lands in the contract, (ii) a compensating-control commitment from the enterprise, or (iii) a residual-risk carry in the risk register with executive-sponsor sign-off.
- Every mandatory control in the tier-appropriate contract references the DDQ answers that support it — the vendor's assertion that they satisfy the control's intent, along with the evidence expected.
- The executed contract's *governance schedule* enumerates the vendor's specific commitments (severity taxonomy, retention windows, notification timelines, subprocessor list, insurance amounts) with concrete values, not references to the vendor's live policies.

This loop is what turns "the vendor made commitments" into "the enterprise has enforceable obligations backed by evidence."

## Two illustrative contract-clause shapes (specified, not drafted)

Two examples of what the architect hands to legal. These are *specifications*, not clause text.

### Example 1 — IN-01 severity-notification shape (tier-3)

*Specification for legal drafting:*

- Severity taxonomy: four levels — Critical, High, Medium, Low — with definitions from the enterprise's incident-response programme (attach as Schedule G).
- Notification timeline per level to the enterprise's designated Vendor Incident Contact:
  - Critical safety or security incident: within 24 hours of vendor's confirmation of the incident's occurrence.
  - High safety or security incident: within 72 hours.
  - Critical operational incident (availability): within 4 hours.
  - Medium and Low: within 5 business days.
- Notification content per IN-02 (see Schedule H).
- Notification channel: enterprise's designated technical incident intake plus enterprise's designated legal / governance channel.
- Cure period for repeated timeline breaches: three material breaches within a rolling 12-month window trigger termination-for-cause under Section [X] of the MSA.
- Escalation on disputed severity: the enterprise's Vendor Incident Contact and vendor's counterpart have a defined escalation path to their respective incident-response leads; unresolved disputes escalate to the AI-accountable executive on the enterprise side and vendor's equivalent.

Legal drafts the enforceable clause; the drafter's judgement governs the specific words that make the clause enforceable in the applicable jurisdiction.

### Example 2 — EA-01 evaluation-access shape (tier-3)

*Specification for legal drafting:*

- Right granted: the enterprise may run enterprise-authored evaluation suites against the vendor's model version(s) offered under the engagement, at the enterprise's cadence, without additional per-run fees beyond the vendor's standard API metering.
- Evaluation-set confidentiality: the vendor treats the enterprise's evaluation prompts and expected responses as enterprise confidential information; the vendor does not use them for training, evaluation, or benchmarking of the vendor's models.
- Rate-limit accommodation per EA-02: disclosed evaluation runs are whitelisted for defined-window rate exceptions; the enterprise pre-notifies with 5 business days' notice for large-batch runs.
- Result-disclosure right per EA-03: the enterprise may disclose its own evaluation results to its regulators, auditors, customers, and internal governance forums, subject to the enterprise not disclosing the vendor's proprietary model internals as such (which the enterprise does not have access to and thus cannot disclose).
- Vendor's counter-evidence right: the vendor may inspect the enterprise's disclosed methodology on request, may respond to enterprise-disclosed results with a vendor statement the enterprise agrees to include when the enterprise publishes results publicly, and may participate in a good-faith review of methodology disputes.
- Term of the right: for the duration of the engagement plus a defined tail-window post-termination sufficient for the enterprise to complete regulator-facing evaluations already in flight.

Legal drafts; the drafter's judgement on tail-window definition and vendor-counter-evidence procedural mechanics governs the words.

## The six invariants the control-set discipline holds

**Invariant 1 — every mandatory control appears in the executed contract or is exception-approved.** No silent drops. Failure mode: the tier-4 template has 32 mandatory controls; the executed contract has 27; nobody notices until an incident reaches for a control that was dropped in negotiation without approval.

**Invariant 2 — exceptions carry a residual-risk carry on the register.** An accepted vendor carveout is a documented residual, not an invisible compromise. Failure mode: the enterprise accepts a vendor-proposed carveout on DU-01 (vendor may retain enterprise prompts for 90 days for abuse monitoring); no register entry is created; twelve months later a subpoena reaches for the vendor's retained prompts and the enterprise's risk register did not reflect the exposure.

**Invariant 3 — the catalog is versioned and executed contracts reference their catalog version.** Failure mode: the catalog has evolved through three versions in eighteen months; executed contracts reference "the AI-governance addendum"; nobody knows which version of which control shapes each contract carries.

**Invariant 4 — the DDQ-to-contract loop is closed at execution.** Every DDQ answer material to a mandatory control has a clause or an exception. Failure mode: the vendor's DDQ answer to a data-use question was below threshold; the reviewer flagged it; the contract executed with the standard clause; the flagged issue never made it into the addendum's carveout schedule or the register.

**Invariant 5 — the addendum composes with MSA / DPA / sector schedules cleanly.** Precedence is declared; no conflict is silent. Failure mode: the DPA's data-retention window and the addendum's DU-02 window differ; the drafting did not resolve; a subsequent audit finds the ambiguity.

**Invariant 6 — the catalog is reviewed on a defined cadence.** Failure mode: the catalog was authored in 2027; foundation-model providers' standard commitments evolved through 2028; the enterprise's catalog is behind market; renewals miss commitments that would have been available.

## Two failure modes to design against

**Failure mode 1 — the addendum that ships with mandatory controls silently dropped in negotiation.** The tier-4 template has 32 mandatory controls. The vendor's legal team pushes back on 8 of them. Procurement, under close-cycle pressure, accepts the pushback. Legal drafts the final addendum without the 8 controls. The executed contract is 24 mandatory + optional controls. No exception was raised because no gate required the exception to be raised. Six quarters later an incident tests one of the dropped controls; the enterprise has no enforcement lever. The fix is architectural: the pre-execution gate compares the executed contract against the tier-mandatory matrix and blocks execution on unresolved drops; drops require an exception with a named executive sponsor; the exception carries a register entry.

**Failure mode 2 — the addendum that binds on a stale catalog version.** The catalog was authored in 2027 with the IP-indemnity language of that period. Frontier-model providers materially strengthened IP indemnities through 2028 (adding coverage for training-data copyright claims). The enterprise's executed contracts still reference the 2027 catalog. Renewal is not automatic; the enterprise's contract-renewal review (chapter 05) is what surfaces the gap. Where the enterprise's contract-renewal review is not disciplined, the enterprise is running behind market for indemnity coverage available to competitors. The fix is architectural: chapter 05's contract-renewal review checks catalog version and identifies the deltas; the renewal is the opportunity to migrate; where the enterprise chooses not to migrate, it records the rationale.

## Summary

The contract-template control set turns DDQ commitments into enforceable obligations. Six control families (data-use limits, evaluation-access rights, incident-notification, evidence-access, exit-and-portability, cross-cutting operational) cover the AI-specific surface; each family carries a set of control shapes with intent, vendor commitment, evidence reference, enforcement lever, and tier-mandatory disposition. A tier-mandatory matrix binds the set to the tier from chapter 02. The composition with MSA, DPA, order form, and sector schedules is layered and declared. Handoff: architect specifies shape; legal drafts enforceable text; procurement issues and negotiates; AI-accountable executive approves exceptions. Six invariants (no silent drops, exceptions carry residuals, catalog versioned, DDQ-to-contract loop closed, composition clean, catalog reviewed) and two failure modes (silent drops, stale catalog) shape the discipline. Exercise-03 walks the drill of authoring control shapes for a specific vendor scenario. The next chapter designs the ongoing-monitoring schedule that operates against these clauses after signing.
