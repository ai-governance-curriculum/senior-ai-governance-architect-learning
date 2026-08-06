# The external audit and certification interface — packaging, sampling access, remediation

## Why this chapter exists

The enterprise AI assurance architecture faces outward through *external assurance providers*: the ISO/IEC 42001 certification body (accredited under IAF Multilateral Recognition and operating under ISO/IEC 42006), the ForHumanity-listed independent auditor operating under IAAIS, the sector regulator's examiner (federal banking supervisor under SR 11-7, state insurance department examiner, EU market surveillance authority under the AI Act, FDA reviewer for medical AI, notified body conducting Article 43 conformity assessment for high-risk EU AI Act systems), and the enterprise's statutory financial auditor where AI touches material financial-reporting processes. Each arrives with a different scope, competence baseline, reporting audience, and enforcement authority. Each will succeed or fail depending on the *interface* the enterprise has architected between itself and them.

An enterprise without a designed external-audit interface degrades every external engagement into an ad-hoc scavenger hunt — the auditor emails whoever they met last time, the enterprise scrambles evidence, findings are dominated by "we could not obtain" rather than substance. An enterprise with a designed interface presents a rehearsed evidence-packaging discipline, a defensible sampling-access pathway, and a remediation-plan template — the auditor spends their time on the actual assurance question, and the findings that emerge are substantive on both sides.

This chapter designs the interface. It is the fifth architectural artefact of the module (after the three lines, the pre-deployment gate, the ongoing programme, and the third-line audit programme) and it is the *externally-visible* face of everything the previous chapters built.

## Who arrives — the external-provider taxonomy

The architecture pins the interface per provider type. The five types have distinct engagement shapes.

### Type 1 — the ISO/IEC 42001 certification body

The 42001 certification body is the enterprise's own contracted certification-audit partner, accredited by a national accreditation body under IAF Multilateral Recognition. Its scope is the AIMS. Its audit shape (stage 1 readiness, stage 2 certification, annual surveillance, three-year recertification) was designed for in mod-105 chapter 11 — this chapter's contribution is the *interface* the enterprise operates against the certification body.

Key interface responsibilities:

- **Contract-scope negotiation.** The certification body's contract specifies scope, cycle, surveillance frequency, on-site days, and audit-team composition. The architect informs the negotiation (the AIMS scope statement drives what the certification body will sample) but does not sign the contract (that is the AI-accountable executive with legal review).
- **Stage-1 readiness support.** The pre-certification readiness review from mod-105 chapter 11 is what the enterprise runs internally; the certification body's stage-1 audit is what the certification body runs. The interface between the two is the *stage-1 pre-brief*: the enterprise packages the readiness-review outputs, the AIMS documented information, and the risk / SoA / RTP state as the certification body's stage-1 auditor arrives.
- **Stage-2 execution support.** During the on-site stage-2 audit the enterprise's audit-liaison seat coordinates access — the audit team meets the interview subjects on the plan, sees the documents on the plan, samples the systems on the plan, and does not spend on-site time on access negotiation.
- **Surveillance and recertification cadence.** The three-year cycle produces a rolling package the enterprise assembles at each surveillance and the full recertification package at year three.

### Type 2 — the ForHumanity-listed independent auditor (IAAIS)

ForHumanity operates a listing of independent auditors qualified under its IAAIS framework. An enterprise may engage an IAAIS auditor for an *independent-auditor engagement* — an external assurance product parallel to but distinct from ISO 42001 certification, aimed at attesting the enterprise's AI-system practices against IAAIS criteria across ethics, bias, cybersecurity, privacy, trust, and related domains. Enterprises deploying AI in domains where IAAIS attestation carries market value (public-sector procurement, insurer or reinsurer risk-transfer, ESG reporting, enterprise-customer due diligence in B2B SaaS) commission such engagements as an assurance product distinct from ISO 42001.

Key interface responsibilities:

- **Engagement scoping.** The IAAIS auditor scopes against IAAIS criteria. The enterprise's audit-liaison and the head of AI governance negotiate scope, timing, and deliverable form.
- **Criteria mapping.** The enterprise packages evidence against the criteria the engagement scopes. Where the enterprise's evidence is organised against ISO 42001 clauses or an internal risk taxonomy, the audit-liaison authors a *crosswalk map* from the internal organisation to the IAAIS criteria.
- **Attestation drafting.** The IAAIS auditor's attestation language is negotiated draft-by-draft; the enterprise's legal function reviews attestation language before publication.

<!-- needs-research: verify the current IAAIS attestation product taxonomy — full audit vs limited-scope attestation vs specific-criteria review — against the ForHumanity publication as versioned. -->

### Type 3 — the sector regulator's examiner

Sector regulators arrive under the authority of their supervisory mandate. The engagement shape varies dramatically by regulator:

- **US federal banking supervisors (OCC, Fed, FDIC).** Under SR 11-7 the supervisors sample the model risk management programme, including AI models under MRM scope. The engagement typically includes model-inventory review, individual model-file review, MRM-programme review, and the internal audit of MRM. Examiners have subpoena-adjacent authority to compel access; the enterprise's obligation is timely and complete production.
- **US state insurance departments.** Under NAIC Model Bulletin on the Use of AI Systems and state-specific model-audit expectations, insurance regulators review AI usage in underwriting, pricing, claims, and marketing. Engagement is typically less frequent than banking examinations but carries the same production obligation. <!-- needs-research: verify the current NAIC Model Bulletin on AI (adopted 2023) status and which states have implemented in law or regulation. -->
- **EU market surveillance authorities under the AI Act.** Under Regulation (EU) 2024/1689, market surveillance authorities have information-request powers (Article 74), remote-access powers to high-risk system logs, and enforcement powers (Article 99, Article 101) up to and including corrective-action orders and fines. Engagement is triggered by risk-based selection, incident reports, or complaints.
- **FDA for medical AI/ML devices.** The FDA reviews SaMD (Software as a Medical Device) and the AI/ML-specific pathway including the predetermined change-control plan for AI/ML-enabled devices. Engagement is at 510(k) or De Novo submission time and ongoing post-market. <!-- needs-research: verify the current FDA Total Product Lifecycle for AI/ML-enabled device final guidance status. -->
- **Notified bodies conducting Article 43 conformity assessment.** Under the EU AI Act, high-risk systems in the Annex III use cases that require conformity assessment involving a notified body have a notified-body engagement roughly analogous to the ISO 42001 certification body's engagement but scoped to conformity with the AI Act rather than to conformance with a management-system standard. <!-- needs-research: verify the Article 43 conformity-assessment procedures list against the final Regulation (EU) 2024/1689 text. -->

Key interface responsibilities across regulator engagements:

- **Regulator-liaison ownership.** A named enterprise seat owns each regulator relationship — typically general counsel or a specialist regulatory-affairs seat, with the head of AI governance as the substantive interlocutor. The seat has authority to schedule, to commit the enterprise to production timelines, and to negotiate scope with the regulator's counterpart.
- **Production discipline.** Regulator requests carry legally-defensible production timelines. The enterprise's response — the actual document, the actual data, the actual record — is authored under legal privilege discipline (typically produced under a work-product framework agreed with counsel).
- **Substantive engagement.** Regulator interviews with model owners, with the head of AI governance, with the level-50 architect, with the risk engineer, and with internal audit are common. Preparation is deliberate: the interviewees see the topic areas in advance; legal briefs them on production discipline; the substantive answers are the interviewees' own.

### Type 4 — the enterprise's statutory financial auditor

Where AI touches material financial-reporting processes — revenue recognition on AI-generated forecasts, allowance for credit losses on AI-scored portfolios, valuation-model outputs feeding financial statements, internal controls over financial reporting (ICFR) that depend on AI-controlled processes — the enterprise's statutory financial auditor (Big Four or comparable) has a scope that includes the AI system's control state and reliability. Under SOX in the US, PCAOB standards apply; under UK / EU statutory audit regimes, comparable audit standards apply. The engagement is annual and heavily standardised.

Key interface responsibilities:

- **ICFR-scope mapping.** The enterprise's compliance function maps AI systems into ICFR scope. The architect informs the mapping (which control-library entries apply to ICFR-material AI systems) and the audit-liaison packages evidence accordingly.
- **SOC-report leverage.** Where the AI capability is provided by a third party (frontier-model provider, MLOps platform vendor), the vendor's SOC 1 / SOC 2 (or ISAE 3402 / 3000) reports are leveraged for auditor confidence. Chapter 06 of mod-109 walks the third-party assurance shape; this chapter's contribution is that the SOC reports are *filed in the audit-liaison's packaging system* and produced to the financial auditor on request.
- **Independence discipline.** The statutory auditor cannot provide advisory services that would compromise their independence. The audit-liaison discipline prevents the auditor's engagement scope from expanding into advisory work.

### Type 5 — the certification-body-adjacent bias auditor

Under NYC LL144 (bias-audit requirement for automated employment decision tools), under Colorado AI Act (bias-audit-adjacent obligations under SB24-205's risk-management-programme requirement), and under sector-specific requirements (some state insurance departments; some federal contractor rules under EEOC guidance), the enterprise engages a specialist independent auditor to conduct a *bias audit* — a formal, publishable attestation of the bias metrics of a specific AI system against a defined set of protected characteristics. BABL AI's Algorithmic Bias Auditor programme is one specialisation; other independent bias-audit firms have their own methodologies.

Key interface responsibilities:

- **Auditor selection and scope.** The audit-liaison confirms the auditor's qualification against the applicable regime (an NYC LL144 bias audit must be by an *independent auditor* as defined by the DCWP rules).
- **Test-data-set packaging.** The bias auditor requires test data with protected-characteristic labels. The enterprise packages the test data under privacy discipline (data-minimisation; anonymisation where required; DPA where personal data is transferred).
- **Attestation publication.** NYC LL144 requires publication of the audit summary; Colorado AI Act's risk-management-programme obligations may require similar publication. The audit-liaison manages the publication.

## What packaging looks like — the evidence packaging discipline

The recurring architectural artefact across all five provider types is the *evidence package*. The architect designs the package's shape so that each external provider receives an artefact that is:

- **Anchored to a scope statement.** The package's first artefact is the scope of the engagement — what systems, what AIMS clauses, what regulatory obligations, what time window. All later artefacts are indexed against the scope.
- **Complete on the pre-defined evidence list.** The package includes every artefact the engagement scope names. Missing artefacts are surfaced early — the auditor is told before fieldwork "artefact X is not on the package because Y" rather than discovering the gap during sampling.
- **Traceable across composition boundaries.** Every artefact cites its source (the mod-108 evidence contract entry, the risk register entry, the AIA, the SoA row). The traceability is what makes the package *sample-able* — the auditor pulls one artefact and can trace its provenance and dependencies.
- **Immutable at delivery.** Once delivered to the external provider, the package is immutable. Corrections are additive: a *note-to-file* accompanies the correction; the original artefact remains in the package for audit provenance.
- **Privileged where legal discipline requires.** Some engagements (particularly financial-audit and regulator engagements) involve legal privilege on portions of the evidence base. The audit-liaison seat, working with legal, discipline what is delivered privileged and what is not.

**A schematic evidence-package structure.**

```
package-<engagement-id>/
  00-scope.md                     # the engagement scope statement
  01-index.md                     # the ordered list of every artefact
  02-crosswalk-map.md             # internal-organisation-to-external-criteria map
  10-aims/                        # AIMS documented information
    aims-scope-statement.pdf
    ai-policy.pdf
    soa-v3.pdf
    rtp-current.pdf
    risk-register-extract.csv     # extract of scope-relevant entries
  20-controls-and-evidence/       # control library + evidence per applicable control
    control-library-v2026.03.15.oscal.xml
    evidence-index.csv
    evidence-artefacts/           # per-artefact directory
      AIC-DAT-013-2026-Q1/
        ...
  30-pre-deployment-decisions/    # sampled decision-records
    PDG-2026-04-0087/
      decision-record.yaml
      evidence-referenced/
      review-workpapers/
  40-ongoing-assurance/           # sampled re-assessment records
    RA-2026-08-...
  50-internal-audit/              # third-line records the external provider may sample
    engagement-reports/
    capa-status/
    audit-committee-decks/
  60-incident-management/         # sampled incident records (where in scope)
    ...
  70-third-party/                 # sampled third-party AI provider records (where in scope)
    ...
  90-notes-to-file/               # additive corrections, clarifications
    ...
  99-checksum-and-signature/      # SHA-256 hashes; audit-liaison signature; delivery date
    package-manifest.yaml
    package-signature.sig
```

Package construction is *rehearsed* — the pre-certification readiness review (mod-105 chapter 11) and the internal audit engagement (chapter 04) both produce packages that are analogous. When the external provider arrives, the audit-liaison seat is not building the package for the first time.

## Sampling access — the negotiated pathway

External providers sample. The architecture specifies the *sampling-access pathway* so sampling is quick, deep, and unambiguous.

- **Read-only production access.** Some artefacts (logs, evaluation-run records, monitoring dashboards) are more efficiently sampled by giving the external provider read-only access to the production system than by extracting samples into the package. The architecture specifies where this pathway is used (typically for regulator engagements with information-request authority; for certification-body stage-2 engagements where the log-review is more efficient on-system).
- **Reproducibility access.** Where the auditor asks to reproduce an evaluation run, the eval-set version, seed, and model version must be re-runnable at engagement time. This is where mod-108's reproducibility discipline matters practically.
- **Interview scheduling.** Interviews with model owners, second-line reviewers, evaluation engineer, risk engineer, internal audit lead, head of AI governance are pre-scheduled on the engagement plan. Interviewees are briefed on production discipline and on legal privilege where applicable.
- **Escalation for access denials.** Where the external provider is denied access to a specific artefact, the denial reason is documented. The certification body and the sector regulator both treat unexplained access denials as material findings; the audit-liaison seat pre-clears access disputes with legal before they become findings.

## The remediation plan — what happens when findings land

Every external engagement produces findings. The architecture specifies how findings are turned into remediation:

- **Response window.** Certification bodies typically require an initial written response to findings within 30 to 60 days, with a corrective-action plan for major nonconformities within 90 days. Sector regulators specify their own windows in engagement documentation. The architecture pins the standard window per provider type and the enterprise's default; deviations are exceptions with named approver.
- **Remediation-plan template.** The architect designs a template that composes with the CAPA process (mod-105 chapter 09). Each finding produces a CAPA entry; the CAPA carries root cause, corrective action, preventive action, effectiveness review, target closure date, and owner. The external provider's finding is closed when the CAPA closes; the enterprise reports closure back to the external provider with evidence.
- **Escalation for systemic findings.** Where a finding indicates a systemic architectural issue (not a per-system defect but a defect in the assurance architecture itself), the remediation route runs through the head of AI governance to the AI-accountable executive and to the audit committee. The architect participates in the remediation design; systemic findings often prompt architecture-version increments (mod-107 chapter 01 invariant 6).
- **Publication where required.** Some engagements produce publishable findings (NYC LL144 bias-audit summary; certain regulator enforcement outcomes). The audit-liaison seat manages the publication under legal discipline.

## A schematic — the external-audit interface

```yaml
external_audit_interface:
  version: 1.3.0
  owner: senior-ai-governance-architect (level 50)
  operator: head-of-ai-governance + audit-liaison seat
  providers:
    - id: iso-42001-cert-body
      cadence: stage-1 pre-cert; stage-2 cert; annual surveillance; year-3 recert
      packaging_shape: full-aims package
      sampling: on-site stage-2; document-review surveillance
      remediation_window: 90d for major NC
    - id: forhumanity-independent-auditor
      cadence: per-engagement (typically annual or biennial)
      packaging_shape: iaais-criteria-mapped package
      sampling: document + interview; system-level as scope requires
      remediation_window: engagement-specific
    - id: sector-regulator-examiner
      instances: [fed-sr-11-7, state-insurance, eu-market-surveillance, fda, notified-body]
      cadence: risk-based; incident-triggered; scheduled per regime
      packaging_shape: regime-specific
      sampling: regime-specific; production-access common
      remediation_window: regime-specific; enforceable
    - id: statutory-financial-auditor
      cadence: annual
      packaging_shape: icfr-scope + soc-reports
      sampling: risk-based per audit methodology
      remediation_window: pre-financial-statement-issue
    - id: bias-auditor-nyc-ll144-or-babl-ai
      cadence: annual per system in scope
      packaging_shape: test-set + methodology + protected-characteristic-analysis
      sampling: test-set; system-instrumentation
      remediation_window: pre-publication
  packaging_discipline:
    scope_first: true
    complete_on_delivery: true
    traceable_across_boundaries: true
    immutable_at_delivery: true
    privileged_where_required: true
    signed_and_hashed: true
  sampling_access:
    read_only_production_access: pre-cleared
    reproducibility_access: eval-set + seed + model version discipline
    interview_scheduling: pre-planned
    denial_escalation: legal + audit-liaison
  remediation:
    response_window_per_provider: defined
    remediation_plan_template: composes with CAPA (mod-105 chapter 09)
    systemic_finding_escalation: architect + head-of-ai-governance + audit committee
    publication_where_required: legal-managed
  invariants:
    - id: E1
      description: every engagement has a named audit-liaison seat
      test: engagement roster shows liaison; liaison signs delivery
    - id: E2
      description: no engagement package is negotiated after delivery
      test: package signature date precedes engagement fieldwork start
    - id: E3
      description: every finding produces a CAPA entry
      test: CAPA register cross-references every finding by engagement id
    - id: E4
      description: publication is legal-cleared before release
      test: publication log shows legal sign-off timestamp before public release
```

## Two failure modes to design against

**Failure mode 1 — the last-minute package.** The certification body's stage-2 audit is scheduled for a Monday. The audit-liaison seat starts assembling the package the previous Wednesday. Artefacts are missing; the audit-liaison seat improvises; the head of AI governance signs off on the package Friday night; the auditor arrives Monday and discovers the improvisations by lunch. Findings are dominated by "the package appears assembled recently and shows internal inconsistencies." The fix is architectural: package construction is a *rehearsed process* running throughout the year — the internal audit engagements (chapter 04) produce analogous packages; the pre-certification readiness review (mod-105 chapter 11) is a full-scale rehearsal. The package the audit-liaison seat delivers Monday morning was assembled Friday from artefacts that were already in the correct shape.

**Failure mode 2 — the negotiation-shaped engagement.** Findings are negotiated during the engagement — the second-line function argues that a nonconformity is "really an observation"; the audit-liaison seat argues that a nonconformity's severity should be downgraded; the head of AI governance argues the finding is out of scope. The external provider's independence is compromised; the audit committee receives a softened report. Certification bodies detect this pattern quickly (their own quality assurance under 17021-1 will find the pattern in the reporting language); regulators do not tolerate it. The fix is the *response-not-negotiation* discipline: the enterprise responds to findings during the engagement with factual clarification (the finding cites artefact X but the artefact is actually at reference Y); the enterprise does not argue finding severity during the engagement. Post-engagement, the enterprise's management response accompanies the finding but does not modify it.

## Summary

The external audit and certification interface is the outward-facing artefact of the assurance architecture. Five external-provider types (ISO 42001 certification body, ForHumanity IAAIS auditor, sector regulator's examiner across banking / insurance / EU market-surveillance / FDA / notified body, statutory financial auditor, and specialist bias auditor under NYC LL144 or BABL AI) each carry a distinct engagement shape; the architect pins the interface per type. Evidence packaging is disciplined — scope-first, complete on delivery, traceable, immutable, privileged where legal requires, signed and hashed. Sampling access is a designed pathway with pre-cleared read-only production access, reproducibility discipline, pre-scheduled interviews, and legal-managed denial escalation. Remediation composes with the CAPA process; systemic findings escalate to the architect and the audit committee; publication is legal-cleared. Two failure modes — last-minute package, negotiation-shaped engagement — are common and both architectural. The next chapter walks the coordination contracts with the peer evaluation engineer and the analyst that make the interior of the assurance architecture executable.
