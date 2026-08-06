# Regulator-facing artefact templates — one source, many packagings

## Why this chapter exists

Every regulator ask triggers a hand-assembly project. A notified body under the EU AI Act requests the Article 11 technical documentation packet for a specific system: the compliance team empties out three shared drives, a senior analyst re-derives sections from the current model card, legal reviews the draft against the last submission, someone chases the evaluation engineer for the current eval report, the packet ships two weeks late and thirty pages differently structured than the previous submission. An FDA Q-Sub meeting requests the PCCP for the enterprise's AI/ML-enabled device software: a different subset of the compliance team empties out different shared drives, the packet ships on a different template with different-named sections. Internal audit samples across the two packets and finds inconsistencies that were entirely a function of the assembly method. The enterprise's evidence substrate held the right answers; the packaging did not consult the substrate directly.

The failure is not that the enterprise did not know what to submit. The failure is that the enterprise treats every regulator-facing packet as a *bespoke authored document* rather than as a *view over the substrate* driven by a versioned template. The result is expensive, inconsistent, and irreproducible.

Chapter 01 named the "one source, many packagings" stance. This chapter designs the packaging templates. Five templates cover the shapes the enterprise is likely to face: EU AI Act Article 11 technical documentation, EU AI Act Article 72 post-market surveillance, EU AI Act Article 73 serious-incident report, SR 11-7 model documentation, and FDA PCCP. Each template is a machine-executable view definition over the substrate content designed in chapters 02–04, with named signatures, an immutable filing discipline, and a reproducibility guarantee (re-emitting the same packet from the same substrate at T+30 days yields the same content). Exercise-04 walks the drill.

## What a packaging template is, structurally

A template is:

- **A view definition over the substrate.** Sections are declared; each section names its source content in the substrate (audit-log queries, card excerpts, ML-BOM references, SLSA attestations) and the projection logic that renders substrate content into the section shape the regulator expects.
- **A signature block.** Every packet has named signers whose roles are declared. The signers are typically drawn from a small set: general counsel or a designated legal seat, the head of AI governance, the AI-accountable executive, the head of MRM (for SR 11-7 packets), the head of regulatory affairs (for FDA PCCP submissions).
- **A version-controlled artefact.** Templates are versioned; every filed packet references the template version used; template updates go through second-line review and produce a governance-workflow log event.
- **An assembly pipeline.** The template does not sit on a laptop; it is executed by an assembly pipeline that produces the filed packet from the substrate at a specific point in time.
- **A reproducibility discipline.** Re-running the assembly against the substrate at the same point in time reproduces the same packet — bit-identical or content-identical, depending on redaction and formatting variance.

## What it is not

- **A Word file the compliance team fills in.** The template is executable-shape (Markdown-with-front-matter, YAML, or an OSCAL profile), rendered into the regulator's preferred format at assembly time. Word files that live on laptops are the failure the templates prevent.
- **A regulatory-affairs deliverable authored per regulator.** The template is authored once per regime; individual filings are instances of the template applied to a specific system at a specific time.
- **The full regulatory submission.** For most regimes the submission includes forms, cover letters, and administrative content the template does not carry. The template carries the *evidence content*; administrative composition happens at packaging time.
- **A substitute for the substrate.** The template renders substrate content; it does not hold content of its own. A template that starts to accumulate content the substrate does not carry has silently spawned a fourth evidence stack.

## The "one source, many packagings" pattern in detail

The pattern is architectural. Its five moves:

1. **The substrate is canonical.** Every claim of substance in every packet is sourced from the substrate — audit logs, cards, ML-BOMs, SLSA attestations, risk register entries, control-implementation evidence.
2. **Each template is a projection.** A template declares which substrate content maps into which packet section, with declared projection logic (excerpt, summary, transformation, redaction).
3. **Templates are versioned.** Every template has a semver; every filed packet references the template version used and the substrate snapshot version at assembly time.
4. **Assembly is pipeline-driven, not human-driven.** The assembly pipeline runs against the substrate at a defined point in time; the pipeline is triggered by the regulator ask or by scheduled cadence.
5. **Filed packets are immutable.** Filed packets land in WORM storage (chapter 02) with cosign signature and Rekor entry. Every regulator interaction produces an immutable filed version.

Regulator asks that require a bespoke response fall into two classes. Class one: the ask is a variant on an existing template (e.g., a notified-body follow-up question about a specific Article 11 section). The response is a *scoped assembly* that renders only the relevant section from the substrate; the response inherits the parent packet's template version. Class two: the ask is a genuinely novel shape (e.g., a state Attorney General's first inquiry under a new state AI Act). The response is the first instance of a new template — authored, versioned, then filed. Every future response of the same shape uses the new template. Bespoke never means starting from scratch.

## Template 1 — EU AI Act Article 11 technical documentation

Article 11 of Regulation (EU) 2024/1689 requires providers of high-risk AI systems to prepare technical documentation before placing the system on the market, kept up to date, structured per Annex IV of the Regulation. Annex IV's shape covers the categories of information a notified body will assess against the applicable harmonised standards during conformity assessment.

Template shape (structured to Annex IV; verify exact numbering against consolidated text): <!-- needs-research: verify Annex IV item numbering against the current consolidated Regulation (EU) 2024/1689 text -->

```yaml
template:
  id: eu-ai-act-article-11
  version: 2.1.0
  authored_by: senior-ai-governance-architect
  reviewed_by: head-of-ai-governance
  ratified_by: general-counsel-seat
  ratified_at: 2027-02-15
  applicable_to: high-risk AI systems under Article 6 / Annex III
  view_over_substrate:
    audit_log_families: [family-1, family-3]
    card_types: [model, system, dataset, risk]
    supply_chain_artefacts: [ml-bom, slsa-attestation, spdx-dataset-records]
  sections:
    - id: 1-general-description
      source: system-card.system-identity + system-card.system-composition
      content_type: markdown
    - id: 2-elements-and-development-process
      source: >
        system-card.system-composition + model-card.model-details + ml-bom +
        training-run family-1 log filtered by system_id
      content_type: markdown + attached-jsonl
    - id: 3-monitoring-functioning-control
      source: system-card.human-oversight-design + guardrails-and-safety-features
      content_type: markdown
    - id: 4-risk-management-system
      source: risk-card + risk-register-refs + mod-107 pre-deployment gate decision record
      content_type: markdown
    - id: 5-lifecycle-changes
      source: family-1 log filtered by system_id, changes over lifecycle window
      content_type: markdown
    - id: 6-standards-applied
      source: soa-standards-mapping + control-library rows applied
      content_type: markdown
    - id: 7-eu-declaration-of-conformity
      source: eu-declaration-of-conformity artefact (separately signed)
      content_type: pdf
    - id: 8-post-market-monitoring-plan
      source: template-eu-article-72-postmarket assembled at same point-in-time
      content_type: markdown
    # etc. per Annex IV items
  signers:
    - role: head-of-ai-governance
      seat: <named>
    - role: general-counsel-seat
      seat: <named>
    - role: ai-accountable-executive
      seat: <named>
      required_when: system.tier == 4 OR system.eu-annex-iii-category in [biometrics, critical-infrastructure]
  filing:
    format: pdf-a plus source-yaml
    immutability: s3-object-lock-compliance
    signing: cosign keyless
    transparency_log: sigstore-rekor
    retention: >= 10y from placing-on-market <!-- needs-research: verify duration -->
  reproducibility_guarantee: >
    Re-assembly of this packet against the substrate at the same snapshot_ts
    reproduces the section content byte-for-byte modulo formatting.
```

The template's sections are not sacred beyond what the Regulation requires; the *shape* is fixed by Annex IV. Every field a notified body will assess maps to substrate content. Where the substrate is missing content the template needs, the template's assembly pipeline emits a *substrate finding* rather than filling in placeholder text — the finding is a CAPA input, and the packet is not filed until the substrate is complete.

The signature block for Article 11 typically includes the head of AI governance and general counsel; for the highest-risk categories (biometrics; critical infrastructure; the Annex III categories the AI Act treats as most consequential <!-- needs-research: verify Annex III scoping and the sub-categories with elevated attention -->) the AI-accountable executive additionally signs. The EU Declaration of Conformity itself is a separately signed artefact referenced from Section 7.

## Template 2 — EU AI Act Article 72 post-market monitoring

Article 72 of the Regulation requires providers of high-risk AI systems to establish and document a post-market monitoring system to collect data on the performance of the system through its lifetime and ensure continuous compliance. The template consists of two parts: the *plan* (pre-registered before placing-on-market, filed alongside the Article 11 technical documentation) and the *ongoing record* (updated with monitoring data).

Template shape:

```yaml
template:
  id: eu-ai-act-article-72-postmarket
  version: 1.4.0
  applicable_to: high-risk AI systems under Article 6 / Annex III
  view_over_substrate:
    audit_log_families: [family-1, family-2, family-3]
    card_types: [system, risk]
    monitoring_signals: reference to mod-110 post-market surveillance architecture
  parts:
    plan:
      sections:
        - monitoring-objectives
        - monitored-signals
        - measurement-methods
        - performance-thresholds-and-triggers
        - responsibilities
        - reporting-cadence-and-audiences
        - review-and-update-cycle
      source: system-card + risk-card + monitoring-plan cross-reference (mod-110)
    ongoing_record:
      sections:
        - period-summary (by cadence)
        - measurement-results
        - trigger-events (with references to family-3 log)
        - corrective-actions-taken
        - open-issues-and-next-steps
      source: monitoring signals + family-3 log filtered to period
      cadence: quarterly for tier-3; monthly summary + quarterly consolidated for tier-4
  signers:
    plan: head-of-ai-governance + system-owner
    ongoing_record: head-of-ai-governance (per period)
  filing:
    immutability: s3-object-lock-compliance
    signing: cosign
    transparency_log: sigstore-rekor
    retention: same horizon as Article 11 packet
```

Article 72 records are the enterprise's ongoing regulator-facing evidence that the pre-launch claims are still true. Mod-110 designs the underlying monitoring architecture; this template packages the monitoring output for the regulator.

## Template 3 — EU AI Act Article 73 serious-incident report

Article 73 requires providers of high-risk AI systems to report serious incidents to the notifying authority within a bounded timeline. The template is the report shape; the underlying incident detection, triage, and response happens through the mod-110 post-market monitoring architecture and the mod-107 chapter 03 incident-driven re-assessment path.

Template shape:

```yaml
template:
  id: eu-ai-act-article-73-incident
  version: 1.2.0
  applicable_to: high-risk AI systems experiencing a serious incident per Article 3(49) definition
  timeline_discipline:
    initial_notification_max: 15 days from awareness  # <!-- needs-research: verify Article 73 timeline -->
    accelerated_notification_max: 2 days from awareness  # for widespread infringement / serious harm categories
    life-critical_notification_max: 10 days  # <!-- needs-research: verify categorised timelines -->
  sections:
    - id: 1-system-identity
      source: system-card.system-identity
    - id: 2-incident-description
      source: family-3 incident-triage-record + family-1 events during incident window
    - id: 3-harm-assessed
      source: risk-card + incident-triage-record.harm-classification
    - id: 4-immediate-corrective-actions
      source: family-3 incident-response events
    - id: 5-root-cause-analysis
      source: family-3 CAPA record (typically filed in a follow-up update; initial report may state
              "root-cause under investigation")
    - id: 6-affected-population-and-scale
      source: monitoring signals + system-card.deployment-scope
    - id: 7-provider-declaration-and-signatures
      source: signature block
  signers:
    - ai-accountable-executive
    - head-of-ai-governance
    - general-counsel-seat
  filing:
    immutability: s3-object-lock-compliance + rekor
    subsequent_updates: filed as versioned amendments; each amendment retains the prior record
    retention: >= 10y <!-- needs-research: verify -->
```

The critical architectural discipline for Article 73 is the *speed* of assembly. When a serious incident fires the template must be assembleable within the notification window without heroics. The reproducibility guarantee applies with a variant: re-assembly at T+7 days after the initial notification, using the substrate at incident-time-plus-2 (representing the state the enterprise knew at initial notification), reproduces the initial notification content.

## Template 4 — SR 11-7 model documentation

The Federal Reserve SR 11-7 supervisory letter and the OCC 2011-12 model risk management supervisory guidance impose model-documentation obligations on regulated financial institutions. The Fed / OCC expectations for AI models used in the regulated business are substantially the same as for classical models: purpose, theory, design, data, implementation, testing, ongoing monitoring, limitations, and independent validation, all sufficient to be reproduced by a person independent of the developer.

Template shape:

```yaml
template:
  id: sr-11-7-model-documentation
  version: 3.0.0
  applicable_to: models within scope of SR 11-7 / OCC 2011-12 (typically material models)
  view_over_substrate:
    audit_log_families: [family-1, family-2, family-3]
    card_types: [model, dataset, risk]
    supply_chain_artefacts: [ml-bom, slsa-attestation]
    mrm_validation_records: family-3 log entries filtered by role=mrm-validator
  sections:
    - id: 1-purpose
      source: model-card.intended-use + system-card.system-identity
    - id: 2-theory
      source: model-card.model-details + reference to underlying methodology docs
    - id: 3-design
      source: model-card.factors + model-card.metrics
    - id: 4-data
      source: dataset-cards + spdx-dataset-records + family-2 log
    - id: 5-implementation
      source: ml-bom + slsa-attestation + family-1 training-run log
    - id: 6-testing
      source: model-card.quantitative-analyses + family-1 evaluation-run log
    - id: 7-ongoing-performance-monitoring
      source: monitoring signals (mod-110) + family-3 log
    - id: 8-limitations
      source: model-card.caveats-and-recommendations + risk-card.residual-risk
    - id: 9-independent-validation
      source: mrm-validation-record (family-3, role=mrm-validator, independent-of-developer=true)
    - id: 10-usage-controls
      source: control-implementation-evidence for applicable soa rows
  signers:
    - mrm-head
    - head-of-ai-governance
    - model-owner (attestation-of-accuracy; distinct from sign-off authority)
  independence_discipline:
    section-9-authored-by: mrm-validator (independent of model-developer)
    other-sections-authored-by: model-owner (draft) + governance-analyst (assembly)
  filing:
    format: pdf-a plus source-yaml
    immutability: s3-object-lock-compliance
    signing: cosign + enterprise-pki
    retention: per bank records-retention schedule (typically 5-7 years; longer for models
      under ongoing supervision)
```

SR 11-7's independence discipline (validation independent of development) is enforced by the assembly pipeline — the pipeline refuses to produce section 9 unless the referenced MRM validation record is authored by a role in the MRM function with no crossover to the model's development team.

## Template 5 — FDA PCCP

The FDA Predetermined Change Control Plan (PCCP) allows manufacturers of AI/ML-enabled medical device software to describe anticipated modifications and the methods that will be used to evaluate them, so that anticipated modifications can be implemented without a new pre-market submission. The FDA finalised guidance on PCCPs for AI-enabled device software in 2024. <!-- needs-research: confirm exact title and finalisation date of the FDA PCCP guidance for AI/ML-enabled device software -->

Template shape:

```yaml
template:
  id: fda-pccp-ai-ml-samd
  version: 1.1.0
  applicable_to: AI/ML-enabled medical device software subject to FDA pre-market submission
  view_over_substrate:
    card_types: [model, system, dataset, risk]
    change_records: family-3 log filtered by system_id
    performance_evaluations: family-1 evaluation runs
    monitoring_signals: reference to mod-110 post-market monitoring
  sections:
    - id: 1-device-description
      source: system-card.system-identity + system-card.system-composition
    - id: 2-planned-modifications
      source: change-control-plan (authored artefact; declares anticipated modifications
        with references to controls; not a substrate projection but a controlled artefact
        of its own that lives in the substrate as a family-3 governance-artefact)
    - id: 3-modification-protocol
      source: modification-protocol-artefact (declares methods used to evaluate each
        modification class, including performance evaluation, labelling updates, and
        real-world monitoring)
    - id: 4-impact-assessment
      source: risk-card + impact-assessment-artefact
    - id: 5-cybersecurity-considerations
      source: control-implementation-evidence for cybersecurity soa rows
    - id: 6-labeling-considerations
      source: labeling-artefact
  signers:
    - regulatory-affairs-head
    - head-of-ai-governance
    - ai-accountable-executive
  filing:
    format: fda-submission-format (eSTAR / eCopy)
    immutability: s3-object-lock-compliance for the enterprise's copy of record;
      FDA electronic submission is the authoritative filing
    retention: for the device master record lifetime + FDA Part 11 records requirements
      <!-- needs-research: verify Part 11 record retention scope for AI/ML-enabled software -->
```

PCCP submissions are distinctive in that the *modification protocol* is itself a controlled artefact — the plan for what the enterprise will do when the model is retrained or the system is updated within the pre-registered envelope. Modifications within the PCCP envelope do not require re-submission; modifications outside require a new pre-market submission or a supplement. The template's assembly pipeline enforces the boundary by cross-referencing the family-1 log at submission time against the PCCP's declared envelope.

## The assembly discipline

The five templates each have an assembly pipeline. The pipeline's shape is:

1. **Substrate snapshot.** The assembly is triggered against the substrate at a specified point in time (usually "now" for cadence-driven assembly; a specified `snapshot_ts` for reproducibility runs).
2. **Section resolution.** For each section in the template, the projection logic resolves the source substrate content into the section's rendered form.
3. **Completeness check.** Missing content produces a substrate finding rather than a placeholder; the pipeline fails-closed by default.
4. **Redaction pass.** Where the packet is externally-facing (regulator, customer, public), the redaction pipeline runs against the assembled content (chapter 02 redaction discipline).
5. **Format render.** The pipeline renders the packet in the regulator's preferred format (PDF/A, eSTAR, XML, whatever).
6. **Signing.** Signers apply cosign or enterprise-PKI signatures; Rekor entries land.
7. **Filing.** The packet is filed in the packet registry (chapter 01 substrate.artefact_registries.packet-registry) with WORM policy applied, a family-3 log entry recorded, and any external submission channel triggered.

Reproducibility runs — assembly against a past `snapshot_ts` — are triggered by internal audit, by regulator follow-up (a plaintiff asks the enterprise to reproduce the packet the regulator saw), or by CAPA (a substrate finding after filing requires the enterprise to demonstrate what state the substrate was in at filing).

## The six invariants the templates hold

**Invariant 1 — every filed packet references its template version and substrate snapshot.** No packet is filed without recording (template_id, template_version, substrate_snapshot_ts). Failure mode: a packet is filed with no version reference; six months later the enterprise cannot say what it committed to.

**Invariant 2 — every packet is assembled by the pipeline, not by hand.** No packet is authored in Word and passed off as template output. Failure mode: the compliance team maintains a shadow Word template that diverges from the canonical template; every filing is authored twice.

**Invariant 3 — reproducibility is testable.** For any filed packet, assembly at the recorded snapshot_ts reproduces the content. Failure mode: the substrate has drifted after filing (retention aged content out; a redaction pipeline changed; a card was updated), and the enterprise cannot re-emit; the plaintiff argues the filed packet was authored, not derived.

**Invariant 4 — templates are versioned and change-controlled.** Template changes go through second-line review with a substrate log event. Failure mode: an analyst edits the template on the compliance drive between filings; the next filed packet is on a different shape and the drift is silent.

**Invariant 5 — signers sign the assembled artefact, not the template.** The signature binds to the specific packet, not to the template. Failure mode: a general counsel signs a template once and every subsequent packet inherits the signature; the signature carries no assertion about the specific packet.

**Invariant 6 — regulator interactions are logged.** Every packet filing, every regulator question, every enterprise response is a family-3 log entry with references to the packet(s) and template(s) involved. Failure mode: a regulator email exchange happens outside the substrate; the enterprise's answer to a question cannot be reconstructed later; the pattern of regulator concerns does not surface to the operating rhythm.

## Two failure modes to design against

**Failure mode 1 — the template that is a Word file on the compliance team's laptop.** The template *exists*; it is a Word file the compliance team has iterated over three years; every filing is an edit of the file. The template's version history is Word revisions; no substrate log entry records changes; the assembly pipeline is a compliance analyst typing content. When the analyst is replaced the template mutates; when the enterprise files two packets in the same month with two different analysts, the packets differ in ways no one intended. The fix is architectural: the template is a version-controlled artefact in the same repository the substrate configuration lives in; changes require review and produce log events; the assembly pipeline is executable and rendered content is what ships.

**Failure mode 2 — the packet that cannot be reproduced.** The enterprise files a packet on 2027-03-15 against Article 11 for one of its high-risk systems. On 2027-08-04 a notified body sends a follow-up asking the enterprise to confirm a claim in Section 3 of the March packet. The enterprise attempts to reproduce the packet from the substrate at 2027-03-15 and finds: the model card in Section 3 was updated on 2027-04-11 and the previous version is not directly retrievable; the redaction pipeline was changed on 2027-06-02 and the previous pipeline is not versioned; the eval-set referenced was superseded on 2027-05-19 and the manifest hash cited in Section 3 no longer resolves. The enterprise cannot answer the notified body's question with confidence; the enterprise's answer relies on human memory of what was in the March packet. The fix is architectural: the substrate itself retains all versions with hash references (chapter 02); the redaction pipeline is versioned and archived with a `version_active_at` window; the assembly pipeline can accept a `snapshot_ts` and reproduce the substrate state at that time. Reproducibility is a substrate-plus-pipeline property, not a memory property.

## Summary

The five regulator-facing artefact templates — EU AI Act Article 11 technical documentation, EU AI Act Article 72 post-market surveillance, EU AI Act Article 73 serious-incident report, SR 11-7 model documentation, FDA PCCP — are executable view definitions over the evidence substrate the module has been building since chapter 02. Each template is versioned, signed at instance-filing time, and produced by an assembly pipeline against a substrate snapshot. The "one source, many packagings" pattern refuses the hand-assembly failure mode; the reproducibility discipline refuses the "cannot re-emit" failure mode. Sector overlays and jurisdictional variants extend the pattern rather than re-authoring templates from scratch. Six invariants and two failure modes shape the design. The next chapter designs the OSCAL representation of the evidence-schema catalog that lets automation drive both packet assembly and the completeness / freshness / coverage queries the pre-deployment gate and internal audit depend on.
