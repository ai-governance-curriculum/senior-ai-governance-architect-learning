# The audit-log substrate — architecture, retention, immutability, chain of custody

## Why this chapter exists

The pattern is familiar to any team that has faced a post-incident investigation. A model behaved badly on a Wednesday; by Friday the enterprise cannot say who touched the fine-tuning checkpoint that day, cannot show the pre-launch evaluation was the run cited in the model card, cannot prove the training-data manifest that a plaintiff's expert reconstructs from public metadata matches the manifest the launch record referenced. Each of those failures is *not* a failure of intent — the platform team logged something; the evaluation engineer emailed the run number; the data engineer versioned the manifest. Each is a failure of the *substrate*: the enterprise's audit logs were structured for engineering triage rather than for regulator-facing defensibility, retained on the wrong horizon, mutable in ways no one thought would matter, and stored in silos that no post-hoc query can compose.

Chapter 01 named the "provenance is enforceable" and "immutability holds at need" stances that the substrate must deliver. This chapter designs the substrate. It pins the three audit-log families, the event contract every family shares, the retention obligations from each in-scope regime and the sector overlays that compose above them, the immutability strategies (WORM object storage, hash-chained append-only logs, external notarisation), the chain-of-custody discipline that binds a regulator-shown artefact to the artefact that was produced, the storage tiering that reconciles the cost of long retention with the queryability internal audit demands, and the access model that separates producers, consumers, and operators. Exercise-01 walks the drill against a concrete scenario.

## What the audit-log substrate is, structurally

The audit-log substrate is the *append-only, provenance-attested, retention-controlled record* of every event across the AI lifecycle that a downstream consumer — internal audit, second-line assurance, external regulator, plaintiff's expert, incident-response team — will need to reconstruct what happened. It has four properties any of which, if missing, invalidates the substrate as an evidence layer.

- **Append-only.** Events are written and not modified. Corrections are new events referencing the corrected event; nothing overwrites in place.
- **Provenance-attested.** Every event's authorship (system component, identity, correlation ID) is captured and signed such that a downstream consumer can verify the event was produced by the claimed source.
- **Retention-controlled.** Every event is subject to a retention policy driven by its class, its subject system's tier, and the applicable regime and sector overlays. Retention beyond obligation is a cost decision; retention below obligation is a compliance failure.
- **Queryable at the horizon the consumer needs.** The substrate serves both the SIEM-style near-real-time query (analyst triage, drift alert) and the years-later reconstruction (subpoena, notified-body audit sampling). Two horizons require different storage tiers with different access mechanics.

## What it is not

- **An operational observability stack.** Prometheus / OpenTelemetry / your APM stack is engineering observability. The audit-log substrate is a distinct layer with different retention, different immutability, different access controls, and different consumers. Operational logs may *feed* the audit-log substrate; they are not the substrate.
- **A single log stream.** The substrate is a *family* of streams (three families designed below) that share a common event contract and interconnect via correlation IDs. Attempts to force all AI-relevant events into one stream produce either an unqueryable firehose or a stream that discards the correlations that make cross-cutting queries answerable.
- **A SIEM.** The SIEM is a consumer of the substrate for security-relevant events. It is not the storage of record for the substrate's own long-horizon retention obligations; those obligations exceed most SIEM retention windows and address a different consumer set.
- **Sufficient by itself for chain of custody.** The substrate holds events. Chain of custody requires signing and timestamping (below) plus custody-handoff records; the substrate is one input.

## The three log families

The substrate spans three families. Each family has a distinct schema extension, a distinct retention driver, a distinct primary consumer, and a distinct producer role.

### Family 1 — model and system lifecycle logs

Events across the AI system lifecycle: model artefact creation, training run start / complete / fail, evaluation run start / complete / fail, checkpoint promotion, model-registry ingestion, deployment, retirement, rollback. This is the family SR 11-7 model-documentation reproducibility depends on and the family EU AI Act Article 12 "automatically generated logs" most directly attaches to.

- **Producers:** MLOps, platform team, model owners via first-line tooling.
- **Consumers:** second-line evaluation engineer (release-assurance verification), risk engineer (residual reading), analyst (evidence-contract assembly, mod-107 chapter 02), internal audit sampling (mod-107 chapter 04), external regulator inquiry.
- **Retention driver:** the tier of the model system and the applicable regime. Tier-3 and tier-4 systems in EU AI Act scope retain against the Regulation's requirement; SR 11-7-scope models retain against the bank's records-retention schedule.
- **Interoperability:** correlation IDs tie training runs to the resulting model artefact, the model artefact to the model-registry ingestion event (chapter 04), the ingestion event to the pre-deployment gate decision-record (mod-107 chapter 02).

### Family 2 — data lifecycle logs

Events across the dataset lifecycle: ingest, transformation, redaction, licence change, DSAR-driven modification or deletion, dataset-registry promotion, dataset-registry retirement, dataset-manifest hash change, dataset lineage cross-reference.

- **Producers:** data engineering, MLOps for auto-triggered transformations, governance analysts for redaction / retention decisions, legal for licence changes.
- **Consumers:** evaluation engineer (dataset-eval cross-reference), governance analyst (dataset card assembly, chapter 03), third-party governance function for supplier-provided datasets (mod-109), incident-response for a licence-issue-driven quarantine.
- **Retention driver:** the class of data (personal data has DSAR-driven modification obligations; medical data has HIPAA-driven retention; children's data has stricter horizons under COPPA / age-appropriate design codes), the applicable regime, and the sector overlay.
- **Interoperability:** dataset-manifest hashes bind to the ML-BOM produced at training time (chapter 04) and to the model card's training-data section (chapter 03).

### Family 3 — governance workflow logs

Events across the second-line workflow: AIA drafts and reviews, risk register entries and updates, control-implementation-evidence collection, pre-deployment gate convene / conditional / block, sign-off application, exception request / approval, incident triage, CAPA open / close, internal audit engagement events, external regulator interaction.

- **Producers:** governance analysts, risk engineer, evaluation engineer, gate chair, incident-response coordinator, internal audit engagement lead.
- **Consumers:** internal audit sampling, external certification body audit sampling (mod-107 chapter 05), sector regulator examiner, general counsel for litigation-hold and subpoena response.
- **Retention driver:** ISO/IEC 42001 documented-information retention as declared in the SoA plus regime-specific overlays plus the enterprise's records-retention schedule. Signing evidence and sign-off records typically retain longer than the underlying activity records because they are the evidence of the *decision* not of the *activity*.
- **Interoperability:** every governance workflow event references the system(s) it applies to (correlating into family 1) and the data (correlating into family 2) where applicable.

## The event contract every family shares

Every event across all three families adheres to a common contract. This is what makes cross-family queries — "who touched this system in the 48 hours before the incident" — answerable at all.

```yaml
event_contract:
  version: 1.2.0
  required_fields:
    event_id: uuidv7                        # time-ordered, globally unique
    event_time: iso-8601-utc                # producer's clock
    ingest_time: iso-8601-utc                # substrate's clock
    schema_version: semver                   # for forward migration
    family: [lifecycle | data | governance]
    actor:                                   # who performed the action
      identity: <oidc-subject|service-account>
      role: <human role or service name>
      auth_context: <mfa|service-mesh|api-key-fingerprint>
    subject:                                 # what the action was performed on
      type: [model|dataset|system|control|risk-entry|packet|other]
      id: <registry id>
      version: <artefact version if applicable>
    action: <verb; controlled vocabulary per family>
    before_state_hash: sha256 | null         # for state-changing events
    after_state_hash: sha256 | null
    outcome: [success|failure|partial]
    correlation_ids:                         # for cross-family joins
      request_id: <top-level request>
      pipeline_run_id: <training/eval pipeline run>
      system_id: <the system this event affects>
      change_id: <the change-management record>
      incident_id: <if incident-driven>
    signature:                               # provenance chain
      producer_signature: <cosign|in-toto attestation payload>
      substrate_signature: <substrate-level receipt>
  optional_fields:
    reason_code: <controlled vocabulary for policy-driven events>
    references: [<artefact IDs cited>]
    payload: <family-specific extension>
    trace_id: <opentelemetry trace id if the event has an OTel context>
  forbidden_fields:
    pii: never in the payload; PII lives in the system of record with a pseudonymous
         reference here. See "The redaction discipline" below.
```

The contract has design consequences worth flagging. `event_time` (producer clock) and `ingest_time` (substrate clock) are both recorded because producer clocks skew; the substrate must be able to reason about ordering under skew. `before_state_hash` / `after_state_hash` allow a downstream consumer to reconstruct state without depending on the substrate to *store* state; the state lives in the artefact registry, and the log holds the hashes that let the consumer verify. `signature` binds authorship — a broken signature invalidates the event as evidence even if the fields look correct. `correlation_ids` is the field family that determines whether cross-family queries return sensible joins.

## Retention obligations — the composition rule

Retention is not a single number; it is a composition of obligations from each in-scope regime, each sector overlay, and the enterprise's own records-retention schedule. The architect authors the *composition rule* — for a given event class, what is the maximum of the applicable obligations — and the retention policy is derived from the rule.

**The regime-level obligations the composition draws from.**

- **EU AI Act Article 12 automatically generated logs.** Providers of high-risk AI systems ensure logs are automatically recorded and retained. The retention horizon is at least the horizon appropriate to the intended purpose — the Regulation states specific horizons for classes of high-risk systems. <!-- needs-research: verify Article 12 minimum retention (typically at least 6 months and possibly longer for regulated device software / financial services / other sector overlays) against final text -->
- **EU AI Act Article 18 record-keeping obligations.** Providers keep the technical documentation, EU declaration of conformity, and other documentation for the period after the system has been placed on the market. Typical horizon: 10 years from the last placing-on-market. <!-- needs-research: verify 10-year period and Article numbering -->
- **SR 11-7 model documentation.** Retained per the bank's records-retention schedule aligned with the Fed's / OCC's expectations; typically 5–7 years for the model documentation itself, longer for models under ongoing supervision or models material to safety-and-soundness.
- **ISO/IEC 42001 Clause 7.5.3 documented information under control.** Retention is *declared by the AIMS* per the Annex A control set the SoA declares applicable; the standard sets no absolute horizon but requires the retention be defined and controlled.
- **Sector overlays.** HIPAA requires 6-year retention on audit logs of PHI access. FDA Part 11 requires electronic records of regulated device software be preserved for the "device master record" horizon plus lifetime — often decades. PCI DSS requires 1-year hot retention with 3 months immediately queryable. NYC Local Law 144 requires the AEDT bias audit be preserved for the annual cycle. Colorado SB24-205 requires risk-management-programme records be preserved on a horizon the Attorney General regulates. <!-- needs-research: verify PCI DSS log retention (typically 1 year total, 3 months immediately available) against current PCI DSS 4.0 requirements -->

**The composition rule.**

```yaml
retention_composition:
  for event_class:
    applicable_regimes: [<list of in-scope regimes>]
    applicable_sector_overlays: [<list>]
    enterprise_records_schedule: <horizon>
  horizon = max(
    regime.retention for regime in applicable_regimes,
    sector.retention for sector in applicable_sector_overlays,
    enterprise_records_schedule,
    legal_hold.horizon if legal_hold_active else 0
  )
```

Where legal hold is active, retention is suspended-in-place (the artefact does not age out); the legal hold event itself is a governance-family log entry.

## Immutability strategies

Retention without immutability is retention on paper. Three strategies exist; most enterprises use a combination.

### WORM object storage

Cloud object stores expose Write-Once-Read-Many locks:

- **AWS S3 Object Lock in COMPLIANCE mode.** Objects cannot be deleted or modified until the retention period expires; even the root account cannot bypass. Used for the tier-4 and long-horizon regime obligations.
- **Azure Immutable Blob Storage (time-based retention or legal hold).** Comparable semantics.
- **Google Cloud Storage Bucket Lock.** Bucket-level policy; write-only guarantees for the policy's horizon.

WORM object storage is the correct default for the *raw event data* and for *filed regulator packets*. It is straightforward, verifiable, and the cloud provider's compliance certifications ride along.

### Hash-chained append-only logs

For a queryable log where WORM object storage is too coarse (an object per event is impractical at high event rates; an object per batch loses the append-only intra-batch guarantee), hash-chained logs deliver tamper-evidence without full object-lock semantics.

- **Trillian (Google).** A verifiable log implementation used by Certificate Transparency and by Sigstore Rekor. Merkle-tree structured, append-only, produces inclusion proofs for individual events.
- **Amazon QLDB.** A managed immutable ledger with cryptographic verification. AWS has announced plans to deprecate QLDB in favor of alternative patterns; enterprises considering it should confirm the roadmap. <!-- needs-research: confirm QLDB deprecation status and AWS-recommended migration target -->
- **Custom Merkle-tree append-only structures.** Written against an underlying store (Postgres, Kafka, S3), the enterprise operates the Merkle root maintenance and inclusion-proof issuance. Higher-effort; correctly done, comparable guarantees.

Hash-chained logs are the correct choice for the *governance workflow log family* where events are frequent, queried often, and where the enterprise wants to prove no post-hoc insertion between two adjacent events (which WORM does not directly guarantee).

### External notarisation

Where a downstream regulator or plaintiff might dispute the internal cryptographic guarantees, external notarisation binds the evidence to a third party the internal system cannot suppress.

- **RFC 3161 timestamping.** Cryptographic timestamps issued by a trusted TSA (time-stamping authority). Long-established, court-recognised in many jurisdictions.
- **Sigstore Rekor.** Public transparency log for software artefacts; extends naturally to evidence artefacts. Rekor entries are practically impossible to suppress.
- **Blockchain anchoring.** Periodic anchoring of the Merkle root to a public blockchain. Technically defensible; usually overkill and adds operational surface.

External notarisation is the correct choice for the *most consequential decision records* (pre-deployment gate decisions on tier-4 systems; regulator packet filings; serious-incident report submissions) where the enterprise wants a guarantee independent of its own key-management.

### The composition

Most enterprises land on:

- Raw event streams → hash-chained append-only log (Trillian / Merkle-tree / QLDB-alternative) with periodic Merkle-root notarisation to Rekor or an RFC 3161 TSA.
- Filed regulator packets and decision records → WORM object storage (S3 COMPLIANCE / Azure Immutable / GCS Bucket Lock) with cosign signature and Rekor entry.
- Signing keys → HSM or KMS with rotation policy; signing identities in OIDC / Sigstore keyless flow where feasible.

## Chain of custody

Chain of custody is the discipline that lets the enterprise prove *the artefact the regulator sees is the artefact that was produced*. It is not the same as immutability — immutability is about the artefact's own integrity; chain of custody is about the *handoff record* across each move.

The chain has four moments:

1. **Production.** The producer signs the artefact with their identity (Sigstore keyless bound to OIDC issuer, or cosign key). The signature is written to the transparency log at production time.
2. **Ingest.** The substrate ingests the artefact, generates its own substrate-signed receipt, and records the ingest event in the governance workflow log with a hash reference to the artefact.
3. **Query.** A consumer (analyst, auditor, legal) queries the substrate and receives the artefact plus its provenance chain (producer signature + substrate receipt). The query itself is logged.
4. **Handoff.** When the artefact leaves the substrate (regulator packet filed; subpoena response prepared; internal audit engagement pack shared), the handoff is a governance workflow log event with a hash reference to the exact artefact version handed over and a signature from the handing-over role.

Chain of custody makes the "we can't prove it wasn't tampered with post-hoc" argument unavailable. For the substrate to deliver, all four moments must record; a substrate that logs production and ingest but not query and handoff is defensible against tampering claims but not against manipulation-during-handoff claims.

## The redaction discipline

Two forces act against each other. Regulators, plaintiffs, and internal auditors want maximally complete records. Privacy law, trade-secret protection, and security-hardening posture argue for minimal records. The substrate resolves the tension with a *redaction discipline* the architect authors and defends.

- **PII never appears in the substrate.** Personal data lives in the system-of-record; the substrate references it by pseudonymous ID. When a DSAR modifies or deletes the personal data, the substrate does not itself modify — the reference stays; the referent changes.
- **Trade secrets and security-sensitive detail appear in a redaction-controlled layer.** The layer's access is gated by named role; the redaction event itself is logged.
- **The redacted view is versioned.** Redactions are not overwrites; the "with-redaction" view is a downstream artefact produced from the raw event by a versioned redaction pipeline. The raw event retains its full content behind the access gate.

The discipline prevents both failures: sensitive data leaking into a substrate queried by many, and post-hoc "cleanup" of embarrassing events by parties with substrate write access.

## Storage tiering

Long retention is expensive at hot-storage prices; queryable-at-any-time storage is impractical at cold-storage prices. The tiering reconciles.

- **Hot (last 30–90 days).** Queryable at interactive latency (seconds), fully indexed, backing the SIEM and analyst tooling. High cost per gigabyte.
- **Warm (last 1–2 years).** Queryable at semi-interactive latency (minutes), partial index (typically the correlation-ID fields plus a coarse-grained time bucket). Moderate cost.
- **Cold / archive (retention horizon).** Retrieve-on-request (hours to a day), unindexed by default, restored to warm for query. Low cost per gigabyte.
- **Legal hold override.** For any event class under legal hold, retention is extended and tier transitions are suspended if the current tier is warmer than cold. The hold itself is logged.

Tier transitions are *events*: each transition is a governance workflow log entry that a downstream consumer can verify against the retention policy. Silent tier transitions (a lifecycle policy that moves objects without emitting an event) invalidate the "retention-controlled" property of the substrate.

## Access model — producers, consumers, operators

Three distinct role classes touch the substrate. The architecture separates them.

- **Producers.** Write events. They cannot delete, cannot modify, cannot read at scale (they can read their own recent events for debugging; queries at scale are a consumer capability). Producer identity is bound to the event signature.
- **Consumers.** Read events. Analysts, auditors, evaluation engineers, incident responders, legal. Consumer queries are logged (a query is itself an event in the governance family log). Consumers cannot write to the substrate; findings that must be recorded are new events written through the producer path with the consumer's authorship.
- **Operators.** Own the substrate's own configuration — retention policies, WORM key management, tier transitions, break-glass execution. Operators are typically a small named group with distinct identity. Every operator action is a governance workflow log event.

Break-glass procedures exist for:

- **Legal hold implementation.** An operator applies a hold on demand from general counsel; the hold event is logged.
- **DSAR-driven personal-data modification / deletion.** The producer path handles the modification through the system of record; the substrate reflects the reference change without deletion.
- **Incident-driven quarantine.** For a compromised dataset or model artefact, operators may set a policy that the artefact is not served from the registry; the substrate retains all prior references.
- **Regulator-directed action.** An operator responds to a regulator order (production, destruction certification, delivery for inspection); every step is logged.

Break-glass without an audit trail is not break-glass; it is a bypass. The operator role's own actions are the audit-log substrate's most consequential entries.

## A schematic

```yaml
audit_log_substrate:
  version: 1.0.0
  families:
    - id: model-system-lifecycle
      producers: [mlops, platform, model-owners]
      retention_composition: max(article-12, article-18, sr-11-7, iso-42001-clause-7.5.3, sector-overlays)
      primary_stores:
        - hot: opensearch (30 days)
        - warm: iceberg-tables-on-s3 (18 months, indexed on system_id + correlation)
        - cold: s3-glacier-deep-archive (10 years)
      immutability:
        raw_stream: trillian-append-only + s3-object-lock-compliance on daily merkle-root anchor
        anchored_to: sigstore-rekor
    - id: data-lifecycle
      producers: [data-engineering, mlops, governance-analysts, legal]
      retention_composition: max(regime, hipaa-6y, ferpa-if-applicable, coppa-if-applicable, enterprise-schedule)
      primary_stores:
        - hot: opensearch (30 days)
        - warm: iceberg-tables-on-s3 (2 years)
        - cold: s3-glacier-deep-archive (regime-max)
      immutability:
        raw_stream: trillian-append-only + s3-object-lock-compliance on daily merkle-root anchor
        anchored_to: sigstore-rekor
    - id: governance-workflow
      producers: [analysts, risk-engineer, evaluation-engineer, gate-chair, incident-coordinator, internal-audit]
      retention_composition: max(article-18, iso-42001-clause-7.5.3, records-schedule, legal-hold)
      primary_stores:
        - hot: postgres + opensearch (90 days)
        - warm: iceberg-tables-on-s3 (5 years)
        - cold: s3-glacier-deep-archive (regime-max, minimum 10y for regulator-facing systems)
      immutability:
        raw_stream: trillian-append-only + s3-object-lock-compliance
        signing_evidence: cosign + rekor entry per decision record
        anchored_to: sigstore-rekor + rfc-3161-tsa
  access_model:
    producers:
      write: yes
      read_own_recent: yes
      read_all: no
      delete_modify: no
      identity: service-account per producer + oidc for humans
    consumers:
      read: yes (query is logged)
      write: no (findings written as new producer-path events)
      identity: rbac gated by role (analyst, evaluator, auditor, legal)
    operators:
      configure_retention: yes (logged)
      break_glass: yes (logged; multi-party approval for tier-4 impact)
      identity: hardware-token-bound; small named group
  chain_of_custody:
    production: cosign+in-toto attestation on artefact
    ingest: substrate signed-receipt in governance-family log
    query: query event in governance-family log
    handoff: handoff event in governance-family log + cosign countersignature by handing-over role
  redaction:
    pii: not in substrate; pseudonymous reference to system-of-record
    trade_secrets: access-gated layer; redaction event logged
    view_versioning: raw+redacted views distinct; redaction pipeline versioned
```

## The six invariants the substrate holds

**Invariant 1 — every material event is captured under the shared contract.** No lifecycle, data, or governance event material to future reconstruction is written outside the substrate; no material field is optional in the shared contract. Failure mode: a critical event class (say, "the fine-tuning parameter overrides applied at run-time") is captured only in application logs and does not appear in the substrate; six months later the enterprise cannot reconstruct.

**Invariant 2 — producer identity is bound to the event.** Every event is signed by a producer identity that a consumer can verify. Failure mode: a service account writes events with no signing; a downstream consumer cannot distinguish legitimate producer events from injected events during an incident-response investigation.

**Invariant 3 — retention obligation composition is authored, not inferred.** For every event class the retention rule is the maximum of applicable regime, sector overlay, enterprise schedule, and legal hold, and the rule is written down. Failure mode: retention is defaulted to "long enough" per engineering intuition; a regime obligation is silently violated for a subclass of events until an audit surfaces it.

**Invariant 4 — immutability is verified, not assumed.** WORM policies are audited on a defined cadence; hash-chain roots are re-verified; Rekor entries are cross-checked. Failure mode: the platform team's lifecycle policy quietly bypasses object-lock for objects "outside the compliance scope" and the certification body samples the bypass.

**Invariant 5 — chain of custody records at all four moments.** Production, ingest, query, and handoff each produce a substrate event. Failure mode: a regulator subpoena response is prepared and shipped without a handoff event; a plaintiff later claims the artefact was substituted en route and the enterprise cannot show otherwise.

**Invariant 6 — access-model separation is enforced by policy, not convention.** Producers cannot delete; consumers cannot write; operators cannot bypass without leaving a trail. Failure mode: an engineer with wide production access holds effective operator permissions on the substrate; the effective break-glass path has no logging; internal audit's competence gap prevents the finding.

## Two failure modes to design against

**Failure mode 1 — the log everyone writes to but no one can read.** The substrate is populated; producers dutifully emit events; the retention is set; but the query surface is not designed. Analyst queries against the raw event contract are painful, correlation IDs are inconsistent across producers, warm-tier indexes were designed for the wrong fields, and the operating rhythm never actually runs the queries the substrate was built to answer. The substrate becomes a museum. The fix is architectural: the query patterns the substrate must serve are declared in the substrate design itself (the queries in the OSCAL catalog's automation section, chapter 06, are one class); indexes are designed for those queries; the operating rhythm runs them regularly and treats "cannot answer this in T time" as a substrate defect.

**Failure mode 2 — the substrate that becomes queryable only after a subpoena.** The design is correct on paper; the queries are declared; but they are never executed until an external ask forces the enterprise's hand. Every query runs cold. Access controls have drifted. Indexes are behind. The evidence for the six-months-ago incident is retrievable at a delay measured in weeks. The fix is architectural: the operating rhythm exercises the substrate; internal audit's engagement sampling (mod-107 chapter 04) uses substrate queries as the primary evidence source; the pre-deployment gate's evidence-contract discharge (mod-107 chapter 02) sources evidence from the substrate rather than from ad-hoc collection.

## Summary

The audit-log substrate is the append-only, provenance-attested, retention-controlled record the evidence architecture rests on. Three log families (model / system lifecycle, data lifecycle, governance workflow) share an event contract with correlation IDs that make cross-family queries possible. Retention is a composition of applicable regime, sector overlay, enterprise records schedule, and legal hold. Immutability composes WORM object storage (for raw events and filed packets), hash-chained append-only logs (for the frequent governance workflow stream), and external notarisation (for the most consequential decision records). Chain of custody records production, ingest, query, and handoff. Storage tiering reconciles retention cost with query latency; break-glass procedures leave their own audit trail. The access model separates producers, consumers, and operators. Six invariants and two failure modes shape the design. The next chapter designs the card family — model, system, dataset, risk — the substrate carries as first-class disclosures.
