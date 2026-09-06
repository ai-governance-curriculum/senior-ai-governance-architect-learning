# exercise-05: External Audit Interface Authoring

**Estimated effort:** 3 hours

## Objective

Author the **external audit and certification interface** for a specified enterprise scenario — the per-provider engagement contracts, the evidence-packaging convention, the sampling-access pathway, and the remediation-plan template that compose with the enterprise's assurance architecture and turn every external engagement from a scavenger hunt into a rehearsed production.

The deliverable is the interface charter, a per-provider engagement contract for each provider your scenario actually faces, an evidence-package skeleton (directory structure plus manifest schema), a sampling-access pathway document, and a remediation-plan template that composes with the enterprise CAPA process.

## Prerequisites

- Chapter [`05-external-audit-and-certification-interface.md`](../05-external-audit-and-certification-interface.md) read once, with the five external-provider types, the packaging discipline, the sampling-access pathway, and the two failure modes marked.
- Exercises 01, 02, 03, 04 in this module complete (or your scenario architecture, gate charter, ongoing programme, and third-line audit programme available in a form you can reference). The external-provider interface *feeds off* the third-line audit programme (exercise-04) — the internal audit engagement packages are analogous to the external-provider packages; a well-designed internal engagement rehearses external delivery.
- Mod-105 chapter 11 (Designing for third-party audit — ISO 42006) walked once, so your interface with the ISO 42001 certification body composes with the pre-certification readiness review from mod-105 rather than duplicating it.
- Mod-105 chapter 09 (Non-conformity and corrective action) walked once, so your remediation-plan template composes with the enterprise CAPA process from mod-105, does not fork it.
- Chapter [`04-third-line-independent-audit-programme.md`](../04-third-line-independent-audit-programme.md) available for cross-reference — the third-line audit-artefact contract is the internal analog to the packaging discipline you author here.
- Access to primary references — ISO/IEC 42006:2025 (title-level enough if paywalled), IAF Multilateral Recognition Arrangement (MLA), ForHumanity IAAIS, BABL AI Algorithmic Bias Auditor, Regulation (EU) 2024/1689 Articles 43 (conformity assessment), 72 (post-market monitoring), 73 (serious incident), 74 (information-request powers), 99 and 101 (enforcement), SR 11-7 (US federal banking supervisor examination shape), NAIC Model Bulletin on AI (state insurance departments), NYC LL144 (bias-audit requirement for AEDTs), PCAOB / ISAE 3402 / ISAE 3000 shapes for statutory financial auditor engagements. See [`../resources.md`](../resources.md).

## Scenario

Use the same scenario you chose in exercise-01 (US regional bank, global healthcare payer/provider, or B2B SaaS HR-tech vendor). Restate the scenario at the top of the deliverable. The scenario determines *which* of the five external-provider types are in scope:

- **The bank scenario** faces (typically): the ISO 42001 certification body if the enterprise is pursuing certification; the Fed / OCC / FDIC MRM examiner under SR 11-7; a state banking supervisor where applicable; state insurance department if the bank has insurance-adjacent operations; the enterprise's statutory financial auditor (Big Four) for AI systems in ICFR scope; a NYC LL144 bias auditor if any employment-decision AI is deployed in NYC.
- **The healthcare scenario** faces: the ISO 42001 certification body; FDA for medical AI/ML devices in the SaMD pathway; potentially a notified body for European insurance subsidiary systems in AI Act high-risk scope; state medical-board and insurance-department engagements per jurisdiction; the statutory financial auditor; potentially a ForHumanity IAAIS auditor for enterprise-customer trust reporting.
- **The B2B SaaS scenario** faces: the ISO 42001 certification body (customer-facing assurance); SOC 2 auditors (customer-facing; the interface with the SOC 2 auditor is a variant of the statutory financial auditor interface); a NYC LL144 bias auditor for HR-tech AEDT deployments; potentially a ForHumanity IAAIS auditor commissioned for enterprise-customer due diligence; potentially an EU AI Act notified body if deploying high-risk-classified systems into EU-based enterprise customers.

Enumerate the in-scope providers at the top of the deliverable. The rest of the exercise instantiates the interface per enumerated provider.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`external-audit-interface-charter.md`** — the charter document that specifies the packaging discipline, sampling-access pathway, remediation-plan discipline, and the audit-liaison seat's authority.
2. **`per-provider-engagement-contracts/`** — a directory with one file per in-scope provider type. Filenames like `iso-42001-certification-body.md`, `sr-11-7-mrm-examiner.md`, `fda-samd-reviewer.md`, `nyc-ll144-bias-auditor.md`, etc. Each specifies the engagement cadence, scope, package shape, sampling access, remediation window, and named liaison.
3. **`evidence-package-skeleton/`** — a directory structure plus a `package-manifest-schema.yaml` that specifies the fields the manifest carries, checksum discipline, and signature discipline. Chapter 05's schematic package structure is the shape to instantiate.
4. **`sampling-access-pathway.md`** — the sampling-access pathway document; who has read-only production access under what pre-clearance, how reproducibility access is discharged, how interviews are scheduled, how access denials are escalated.
5. **`remediation-plan-template.yaml`** — the remediation-plan template composing with the mod-105 chapter 09 CAPA process; per-finding fields for root cause, corrective action, preventive action, effectiveness review, target closure date, owner, and reporting-back-to-provider discipline.

## Requirements

### `external-audit-interface-charter.md`

Decide and justify **each** of the following:

- **Audit-liaison seat.** The named seat (or seats) that owns each provider relationship; the authority to schedule; the authority to commit the enterprise to production timelines; the authority to negotiate scope with the provider counterpart; the escalation path to the head of AI governance for scope disputes and to legal for privilege discipline. Distinguish where the audit-liaison seat is (a) the head of AI governance directly, (b) a dedicated regulatory-affairs seat, (c) general counsel or a designated regulatory-counsel seat, (d) the chief audit executive.
- **Packaging discipline as a formal contract clause.** The five packaging invariants from chapter 05 (scope-first, complete-on-delivery, traceable-across-boundaries, immutable-at-delivery, privileged-where-required, signed-and-hashed) instantiated for your scenario. The contract must state that packages delivered under this charter carry the invariants; deviations require named authorisation before delivery.
- **Sampling-access pre-clearance discipline.** The pre-cleared read-only production-access pathway (or the deliberate decision not to offer such access, with the reason). The interview-scheduling discipline (pre-planned; interviewees briefed; substantive answers are the interviewees' own). The denial-escalation pathway (audit-liaison to legal to head of AI governance; the escalation happens *before* the auditor treats the denial as a finding).
- **Remediation-plan discipline.** The response window per provider type; the composition with the mod-105 chapter 09 CAPA process (every finding produces a CAPA entry); the systemic-finding escalation to the architect and the audit committee; the publication-where-required discipline (NYC LL144 audit summary publication; certain regulator enforcement outcomes) under legal-managed clearance.
- **Rehearsal discipline.** The rehearsal cadence — the internal audit engagements (chapter 04) produce analogous packages; the pre-certification readiness review (mod-105 chapter 11) is a full-scale rehearsal; the enterprise assembles no external-audit package for the first time when the external auditor arrives.
- **Package-delivery ceremony.** The delivery signature discipline; the checksum manifest; the delivery-log entry; the post-delivery immutability commitment (corrections are additive notes-to-file, not modifications).
- **Anti-negotiation discipline.** Chapter 05 failure mode 2 (negotiation-shaped engagement) is common. The charter must specify the *response-not-negotiation* discipline as a formal clause: factual clarifications are provided during the engagement; severity is not argued during the engagement; management response accompanies the finding post-engagement but does not modify it.
- **Non-scope.** At least three things the interface deliberately excludes. Candidates: a marketing-language review of finding text before delivery to the provider (the finding language is the auditor's, not the enterprise's); a general-counsel privilege blanket on all engagement communications (privilege is granular and specific, not blanket); a "commissioning of one external provider to audit another" (external providers do not audit each other under this charter).

### `per-provider-engagement-contracts/`

One file per in-scope provider type. Each file walks:

- **Cadence.** ISO 42001 certification body: stage 1 → stage 2 → annual surveillance → year-3 recert. SR 11-7 examiner: risk-based; typically 12- to 24-month examination cycles. FDA SaMD reviewer: at 510(k) or De Novo submission and ongoing post-market. NAIC / state insurance department: risk-based; NAIC Model Bulletin adoption pace varies by state. AI Act market surveillance: risk-based; incident-triggered; complaint-triggered. NYC LL144 bias auditor: annual per AEDT in scope. IAAIS auditor: per-engagement (typically annual or biennial). Statutory financial auditor: annual. Notified body under Article 43: per system in scope of the conformity-assessment procedure requiring notified-body involvement.
- **Scope.** The AIMS clauses / regulatory articles / control-family scope the provider samples. Where the scope is enterprise-negotiable (ISO 42001 stage-2 scope), the scope statement the enterprise commits to. Where the scope is regulator-defined (SR 11-7 examination scope; AI Act Article 74 information-request scope), the enterprise's obligation is production against the scope as defined.
- **Package shape.** The section of the evidence-package skeleton the provider engagement consumes. ISO 42001 gets the full AIMS package (chapter 05 schematic). SR 11-7 examiner gets the model-file plus the MRM programme plus the internal-audit-of-MRM records. FDA SaMD gets the technical file plus the predetermined change-control plan plus the post-market surveillance plan. NYC LL144 bias auditor gets the test set plus the methodology plus the protected-characteristic analysis. IAAIS gets the IAAIS-criteria-mapped package with the crosswalk from the enterprise's internal organisation.
- **Sampling access.** Which artefacts are delivered in the package vs. which are read-only-production-access sampled vs. which require reproducibility access vs. which require interview access.
- **Remediation window.** The standard window for the provider type (ISO certification: 90 days for major NC as a typical accreditation-body-set expectation; sector regulators: as specified in engagement documentation; statutory auditor: pre-financial-statement-issue; NYC LL144: pre-publication of the audit summary). Deviations require named authorisation.
- **Named liaison and enterprise interlocutors.** The audit-liaison seat, the head of AI governance as substantive interlocutor, the interview subjects the provider is expected to want (model owners, second-line reviewers, evaluation engineer, risk engineer, internal audit lead, head of AI governance).
- **Termination or transition.** For provider relationships the enterprise contracts (ISO certification body, IAAIS auditor, statutory financial auditor, NYC LL144 bias auditor, notified body): the transition-out discipline if the enterprise changes provider (independence preservation on the succeeding relationship; records-handover; historical-finding continuity).

At least three provider contract files must be authored at full depth. If your scenario's in-scope-provider count exceeds three, author all remaining files at outline depth (cadence, scope, package shape, named liaison — the sampling-access and remediation sections can be summarised with "per charter" cross-references where the charter's default applies).

### `evidence-package-skeleton/`

A directory structure plus a manifest schema:

- **The directory structure** mirroring chapter 05's schematic. Show the top-level directories (00-scope, 01-index, 02-crosswalk-map, 10-aims, 20-controls-and-evidence, 30-pre-deployment-decisions, 40-ongoing-assurance, 50-internal-audit, 60-incident-management, 70-third-party, 90-notes-to-file, 99-checksum-and-signature); populate at least one directory with an example artefact list appropriate to your scenario.
- **The `package-manifest-schema.yaml`** specifying the manifest fields: package id, package version, scope reference, provider recipient, delivery date, signature (audit-liaison seat + timestamp), checksum algorithm and per-file hashes, artefact index with per-artefact source-reference (mod-108 evidence contract entry, risk register entry, AIA reference, SoA row), immutability clause.
- **The `crosswalk-map.md`** exemplar showing how the enterprise's internal organisation (mod-108 evidence contract, risk register, control library) maps to the provider's external criteria (ISO 42001 Annex A clauses, IAAIS criteria, AI Act Article 9–15 obligations, FDA SaMD documentation expectations, NYC LL144 bias-audit criteria). Instantiate the crosswalk for at least one provider from your scenario.

### `sampling-access-pathway.md`

Decide and justify:

- **Read-only production access.** Which providers get pre-cleared read-only production access, to which systems, under what discipline (SSO with named-account auditor identity; read-only role with logging; time-bound access). Which providers do *not* get such access and why (e.g., NYC LL144 bias auditor typically works on test-set-only samples; statutory financial auditor's ICFR-scope access is separately governed under audit-firm confidentiality).
- **Reproducibility access.** The eval-set version, seed, model version, and artefact-hash discipline that allows an auditor to re-run a specific evaluation. Where mod-108's reproducibility discipline is not yet mature, name the interim compensating controls (audit re-runs a specific evaluation with the enterprise's evaluation engineer's supervision, rather than independently).
- **Interview scheduling.** The pre-scheduled interview list per engagement; the interviewee-briefing discipline (production discipline; legal privilege where applicable; the substantive answers are the interviewee's own); the interviewer's questions in advance where the provider agrees.
- **Denial-escalation.** Where the auditor requests access to an artefact and the enterprise's default access pathway is not available (privilege-protected material; another-provider's confidential IP; ongoing litigation privilege), the discipline for handling the denial: audit-liaison receives the request, consults legal, either approves alternative access (redacted version; summary; interview substitution) or documents the denial with reason before the auditor treats it as a finding.

### `remediation-plan-template.yaml`

The template that composes with the mod-105 chapter 09 CAPA process. Per finding, the fields:

```yaml
finding:
  id: <FND-<engagement-id>-<seq>>
  engagement_id: <ENG-...>
  provider: <provider name>
  provider_finding_reference: <as cited by provider>
  finding_summary: <one-paragraph description>
  severity_as_cited_by_provider: <major NC / minor NC / observation / etc>
  affected_scope:
    systems: [<system ids>]
    aims_clauses_or_regulatory_articles: [<references>]
    control_library_rows: [<row ids>]
  root_cause_analysis:
    method: <e.g. 5-whys / fishbone / other>
    output: <root cause statement>
    author: <seat>
  corrective_action:
    description: <what will be done>
    owner: <seat>
    target_closure_date: <YYYY-MM-DD>
    dependencies: [<other CAPAs, resource asks>]
  preventive_action:
    description: <how recurrence is prevented across scope>
    owner: <seat>
    target_closure_date: <YYYY-MM-DD>
  effectiveness_review:
    method: <how effectiveness is measured>
    scheduled: <YYYY-MM-DD>
    owner: <seat>
  provider_report_back:
    initial_response_due: <YYYY-MM-DD>
    corrective_action_plan_due: <YYYY-MM-DD>
    closure_evidence_to_be_delivered: <what artefact confirms closure>
  systemic_flag: <true if finding indicates a systemic issue>
  systemic_escalation:
    escalated_to: <architect / head of AI governance / audit committee>
    architecture_version_impact: <version bump if any, per chapter 01 invariant 6>
  publication_required: <true / false>
  publication_pathway: <legal-managed clearance path if applicable>
```

Include one *worked example finding* — a plausible finding for your scenario, with all fields populated to a coherent state. The finding must be non-trivial (a real defect, not a paperwork nit) and demonstrate the systemic-flag and architecture-version-impact pathways.

## Starter guidance

- Draft the charter *before* the per-provider contracts. If you cannot decide the packaging and access disciplines in prose, the per-provider contracts will diverge and the enterprise will end up with a different discipline per provider.
- The audit-liaison seat is the interface's single point of coordination; do not diffuse the seat. If your scenario's audit-liaison seat is split (regulatory-affairs owns regulator engagements; head of AI governance owns certification-body engagements; general counsel owns statutory-auditor engagements), name the split explicitly and specify how coordination across the split happens without ambiguity.
- The response-not-negotiation discipline is where organisational pressure lands, exactly parallel to chapter 04's management-response discipline for internal audit findings. Under-competent audit-liaison seats under commercial or reputational pressure attempt to soften findings during the draft cycle. The charter must name the discipline as a formal clause and the audit-liaison seat must be held to it.
- The reproducibility access section will show whether mod-108's evidence architecture is actually mature or is a paper commitment. If your enterprise's evaluation runs are not reproducible from filed artefacts, do not paper over it — name the interim compensating discipline and the maturity path.
- For the bank scenario, the SR 11-7 examiner interface is the most legally consequential; the enterprise's obligation is production against the examiner's scope as defined and the examiner's authority is substantial. Design the interface for full and timely production, not for scope contest. The statutory financial auditor interface is the second most consequential, particularly for ICFR-material AI systems.
- For the healthcare scenario, the FDA SaMD interface follows a *very* specific regulatory shape (submission-time reviews plus post-market surveillance); the enterprise's obligation includes the predetermined change-control plan discipline for AI/ML-enabled devices. The interface with the notified body for EU insurance-subsidiary systems under AI Act Article 43 is a parallel discipline for EU-scope systems.
- For the B2B SaaS scenario, the customer-facing SOC 2 auditor interface is the *most frequent* external engagement; the AI-scope of the SOC 2 attestation is a distinct package section that likely repeats across customer engagements. The NYC LL144 bias auditor interface for AEDT deployments is annual per system in scope.
- Composition with ForHumanity IAAIS is optional in most scenarios (an assurance product distinct from ISO 42001, commissioned where market value exists); where it is in scope, the crosswalk map from the enterprise's internal organisation to the IAAIS criteria is the deliverable's centrepiece.

## Acceptance criteria

- [ ] Scenario is stated at the top; in-scope providers are enumerated with reasoning.
- [ ] `external-audit-interface-charter.md` decides all eight requirements bullets, each with stated rationale.
- [ ] Audit-liaison seat is named (or the split is named and coordination discipline specified); authority scope is stated.
- [ ] Packaging discipline is a formal contract clause with the five invariants; deviations require named authorisation.
- [ ] Sampling-access pre-clearance discipline is stated; interview-scheduling discipline is stated; denial-escalation pathway is stated.
- [ ] Remediation-plan discipline composes with mod-105 chapter 09; systemic-finding escalation is stated; publication-where-required discipline is legal-managed.
- [ ] Rehearsal discipline is stated; anti-negotiation discipline is a formal clause.
- [ ] Non-scope section names at least three things deliberately excluded.
- [ ] `per-provider-engagement-contracts/` includes at least three contract files at full depth for the in-scope providers; any additional in-scope providers have outline-depth files.
- [ ] `evidence-package-skeleton/` includes the directory structure, `package-manifest-schema.yaml`, and a `crosswalk-map.md` exemplar for at least one provider.
- [ ] `sampling-access-pathway.md` decides read-only-production, reproducibility, interview, and denial-escalation disciplines with named seats.
- [ ] `remediation-plan-template.yaml` covers every field; worked example finding is coherent and non-trivial.
- [ ] Every unverified citation to ISO/IEC 42006 clauses, IAF MLA provisions, IAAIS criteria, BABL AI curriculum, EU AI Act Articles, SR 11-7 provisions, FDA guidance, NAIC Model Bulletin sections, NYC LL144 rules, or PCAOB/ISAE clauses is marked `<!-- needs-research: ... -->` — no invented section numbers or article labels.

## Stretch goals

- Add a *provider-portfolio view* — a chart showing all in-scope providers, cadences, packaged sections shared vs. unique, and the total annual external-audit hours the enterprise commits to. Preview of the mod-112 operating-model chapter's resource-planning discipline.
- Sketch the *transition playbook* for changing ISO 42001 certification bodies mid-cycle (the enterprise's certification body's accreditation is suspended; the enterprise moves to a new certification body). The playbook covers records-handover, gap-audit at transition, independence-preservation, and audit-committee ratification.
- Extend the sampling-access-pathway document with a *privilege-management addendum* — the discipline for producing to sector regulators under legal privilege (typically work-product privilege in the US; comparable frameworks in other jurisdictions). Preview of the mod-104 multi-jurisdiction reconciliation chapter's legal-privilege topic.
- Add a *SOC-report leverage exemplar* — a specific instance of a frontier-model provider's or MLOps platform vendor's SOC 1 / SOC 2 or ISAE 3402 / 3000 report leveraged for statutory financial auditor confidence on an AI capability. Composition with mod-109 chapter 06 third-party assurance.
- Compose the interface charter with the *NIST SP 800-37 Authorize step's independent-assessment* framing from chapter 07 — where the external providers, particularly the ISO 42001 certification body and the SR 11-7 examiner, play a role analogous to the RMF's independent assessor for federal-facing customers.
