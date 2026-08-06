# The evidence architecture lens — why one surface must satisfy three regimes

## Why this chapter exists

An enterprise AI programme without an *evidence architecture* fails in a distinctive way. The programme has evidence — model owners keep evaluation notebooks, governance analysts collect model cards, the platform team logs training runs, the compliance team files SR 11-7 model documentation and an ISO/IEC 42001 documented-information register and, when the EU AI Act comes into scope, an Article 11 technical documentation dossier. Each of those artefacts is defensible in isolation. In aggregate they tell the enterprise's story three different ways, and at the seam where two regulators compare notes the divergence is what the enterprise gets asked about. The certification body finds an eval report the SR 11-7 file cites but the ISO 42001 documented-information register does not list. The plaintiff's expert reconstructs a training-data manifest that does not match the manifest cited in the notified-body-facing technical documentation. Internal audit samples the Clause 9.2 evidence pack and cannot find the specific evaluation run that anchored the pre-deployment gate. Every layer of the enterprise is producing evidence and no layer owns the *surface* the evidence sits on.

The level-50 architect owns that surface. This chapter names the failure the surface prevents, the three convergent regimes the surface has to satisfy in the same schema, the architectural stances the module will design against, and the invariants the surface must hold. It is the anchor for the module — chapters 02–04 design the substrate (audit logs, cards, supply-chain evidence), chapter 05 designs the regulator-facing packagings, chapter 06 shows the machine-readable catalog that makes automation possible, and chapter 07 pins the coordination contracts that keep the substrate populated. Every subsequent chapter is an instance of the shape this chapter fixes.

Two adjacent architectural concerns motivate this framing. First, evidence architecture is *not* a byproduct of good compliance operations any more than data architecture is a byproduct of good engineering — treating it that way produces the same class of failure (silos, drift, three stories for the same fact). Second, evidence architecture at AI scale is a *design* concern the enterprise's classical governance shops usually do not carry — the volume of automatically generated evidence, the reproducibility discipline, the supply-chain slice, and the machine-readable catalog together move the problem outside what a records-management function can solve.

## What "evidence architecture" is, structurally

Evidence architecture is the *authored design* of the surface across which the enterprise's AI-governance evidence is produced, stored, provenance-attested, versioned, queried, and packaged for internal and external consumers. It consists of:

- **A substrate** — the technical stores that hold the raw evidence (audit logs, immutable object stores, model registries, dataset registries, artefact repositories).
- **A schema catalog** — the controlled definitions of each evidence artefact type (an audit-log event, a model card, a dataset card, a risk card, an ML-BOM, an SLSA attestation, an EU AI Act Article 11 technical documentation packet, an SR 11-7 model documentation packet).
- **A provenance and immutability layer** — signatures, transparency logs, timestamping, chain-of-custody records that make the evidence *enforceable* against a subsequent claim of tampering or reconstruction.
- **A packaging layer** — the views that render subsets of the substrate as regulator-facing packets, certification-body sampling responses, plaintiff subpoena responses, internal audit engagement packs.
- **A role model** — the assignment of authorship, review, sign-off, publication, and consumption responsibilities across the roles in the AI governance stack (analyst, risk engineer, evaluation engineer, architect, head, accountable executive).
- **A machine-readable catalog** — the OSCAL or equivalent representation that lets automation reason about "what evidence exists for what system at what freshness against what obligations."

## What it is not

- **A GRC platform.** A GRC platform is a tool. The architecture is the design the tool implements. Enterprises that buy a GRC platform without first authoring the architecture buy a container for the wrong shape.
- **A records-management extension.** Records management is a discipline; it does not, on its own, carry the schema catalog, the provenance layer, the machine-readable representation, or the AI-specific artefact types this architecture holds.
- **A compliance function's private concern.** The evidence is produced by first-line MLE / MLOps / data engineering (chapter 02 audit logs, chapter 04 supply-chain evidence) and by second-line analysts / evaluation engineers / risk engineers (chapter 03 cards, chapter 05 regulator packagings). If any of those roles do not treat the substrate as first-class, the architecture is hollow.
- **Sufficient by itself.** Evidence architecture is one architectural layer alongside the control library (mod-102), the policy hierarchy (mod-103), the AIMS (mod-105), the risk taxonomy (mod-106), the assurance architecture (mod-107). Compose or fail.

## The three convergent regimes

The architecture must satisfy three regulatory / standards regimes in the same schema, with sector overlays as compositions. The three regimes are the reason "evidence architecture" is a distinct concern — no single regime forces this architecture; the requirement that *all three* be satisfied against the same underlying evidence is what does.

### Regime 1 — the EU AI Act

Regulation (EU) 2024/1689 imposes an evidence obligation on providers of high-risk AI systems that is unusually specific for a horizontal regulation. The obligations most relevant to the architecture:

- **Technical documentation (Article 11).** A structured dossier following the Annex IV shape covering the system's intended purpose, elements, monitoring, functioning, control, risk-management system, changes over the lifecycle, standards applied, EU declaration of conformity, and post-market monitoring plan. Chapter 05 designs the template. <!-- needs-research: confirm Annex IV numbering against the consolidated Regulation (EU) 2024/1689 text -->
- **Record-keeping / automatically generated logs (Article 12).** High-risk systems must be designed to automatically record events over the lifecycle; the logs must cover specific events sufficient to identify situations that may present a risk. Retention obligations attach — typically at least 6 months, though the specific horizon depends on the deployment context and sectoral rules. <!-- needs-research: verify Article 12 retention duration and specific event categories against final text -->
- **Record-keeping obligations for providers and deployers.** Records must be kept for the period the Regulation specifies (in the range of 10 years for the technical documentation and EU declaration of conformity, and specific horizons for automatically generated logs). <!-- needs-research: confirm 10-year period against the final Regulation text and align with Article 18 / Article 19 numbering -->
- **Post-market monitoring (Article 72).** Ongoing records of monitoring against the pre-registered plan; chapter 05 fixes the template shape and chapter 07 pins the interface to mod-110 (post-market monitoring architecture).
- **Serious-incident reporting (Article 73).** Time-bounded reporting to the notifying authority; the timeline and severity classification are fixed in the Regulation. Chapter 05 fixes the template. <!-- needs-research: verify Article 73 timelines (72 hours / 15 days / 2 days variants) against final text -->

### Regime 2 — SR 11-7 / OCC 2011-12 model risk management

The Federal Reserve SR 11-7 and the OCC 2011-12 supervisory guidance on model risk management remain the dominant US federal shape for model documentation and validation. Where a bank uses AI as a "model" under SR 11-7 the evidence obligations attach independently of whether the AI Act is in scope. The relevant obligations:

- **Model documentation.** A structured record covering purpose, theory, design, data, implementation, testing, ongoing performance monitoring, limitations, and independent validation. The documentation is expected to be sufficient for a person independent of the developer to reproduce.
- **Independence of validation from development.** The validation records must show the validator was independent of the developer.
- **Ongoing monitoring records.** Periodic re-validation, monitoring against pre-registered performance thresholds, and documentation of remediation when the model deviates.
- **Model inventory and materiality classification.** The bank must maintain a model inventory with materiality classification driving documentation depth.

### Regime 3 — ISO/IEC 42001:2023 documented information

ISO/IEC 42001 as an AI management system standard imposes a documented-information regime under Clauses 7.5, 9.1, 9.2, and 10 that the enterprise's AIMS must operate against. The relevant obligations:

- **Clause 7.5 — documented information.** The AIMS defines what information must be documented, controls its creation and update, controls its availability and retention, and controls its destruction.
- **Clause 9.1 — monitoring, measurement, analysis, and evaluation.** Records of the AIMS's own performance against its objectives.
- **Clause 9.2 — internal audit records.** Records of the AIMS internal audit programme (mod-105 Clause 9.2, mod-107 chapter 04).
- **Clause 10 — nonconformity and CAPA records.** The record of the enterprise's response to findings, incidents, and drift.
- **Annex A control records.** Each Annex A control the SoA declares applicable produces evidence.

### The convergence problem

The three regimes ask for the *same underlying evidence* in three different packagings. All three want to know what the model does, how it was trained, what it was evaluated against, how it is monitored, and what the enterprise's controls around it are. All three ask *slightly different questions*, expect *slightly different section shapes*, and impose *different retention and independence obligations*.

An enterprise that treats each regime as a separate compliance stream produces three parallel evidence stacks. Each stack is expensive to maintain; the stacks drift; the drift is what the certification body, the sector examiner, and the plaintiff each surface. The architectural move is to treat the substrate as canonical (one story about the model, the training data, the eval, the monitoring, the controls) and the regulator-facing packagings as *views* over the substrate. This is the "one source, many packagings" pattern the module will design.

Sector overlays compose above the three regimes. HIPAA imposes 6-year audit-log retention on covered entities. FDA imposes Part 11 electronic records / electronic signatures requirements on regulated device software. PCI DSS imposes logging retention. Colorado's SB24-205 AI Consumer Protection Act imposes risk management programme records on developers and deployers of high-risk AI. New York City Local Law 144 imposes a bias-audit record for automated employment decision tools. The architecture must accept overlays without re-authoring — the substrate does not change; the view definitions extend.

## The five architectural stances

The module will design the substrate, cards, supply-chain evidence, regulator packagings, catalog, and coordination against five stances. The stances are architectural commitments the architect makes and defends.

1. **Evidence is a first-class artefact, not a byproduct.** Every evidence artefact has an owner, a schema, a version, a signature, an immutability posture, a retention obligation, a query surface, and a downstream consumer. Evidence produced without those attributes is not evidence — it is a wiki page waiting to become a finding.
2. **One authoritative source, many regulator-facing packagings.** The substrate is canonical. Article 11 packets, SR 11-7 documentation, FDA PCCP submissions, and ATRS records are *views* — assembled from the substrate by defined view logic, not hand-authored per submission. Chapter 05 designs the templates; chapter 06 designs the OSCAL profile that drives assembly.
3. **Provenance is enforceable.** Every material evidence artefact carries a signature (Sigstore cosign, in-toto attestations), a timestamp (RFC 3161 or Rekor entry), and a chain-of-custody record sufficient to prove the artefact the regulator sees is the artefact that was produced. Chapters 02 and 04 fix the mechanisms.
4. **Schemas are machine-readable.** The evidence-schema catalog is expressed in OSCAL (or equivalent) so downstream automation can reason about completeness, freshness, drift, and coverage. Chapter 06 designs it.
5. **Roles are assigned per artefact-type.** Every artefact type has a named author (typically first-line for control-implementation evidence; second-line for cards; second-line for regulator packagings), a named reviewer, a named signer, and a named consumer. Chapter 07 pins the coordination with the level-15 analyst, the level-25 risk engineer, and the peer level-35 evaluation engineer.

## A schematic of the evidence surface

The substrate, catalog, provenance layer, packaging layer, and role model, expressed as a topology the architect authors and defends against the AI-accountable executive.

```yaml
evidence_architecture:
  version: 1.0.0
  owner: senior-ai-governance-architect (level 50)
  ratifies: head-of-ai-governance (level 60); ai-accountable-executive (annually)

  substrate:
    audit_log_stores:  # chapter 02
      - id: model-system-lifecycle-log
        family: lifecycle
        immutability: worm+hash-chain
        retention: tier-based (see chapter 02)
      - id: data-lifecycle-log
        family: data
        immutability: worm+hash-chain
        retention: tier-based + sector overlay
      - id: governance-workflow-log
        family: governance
        immutability: hash-chain+notarised
        retention: 10y minimum for regulator-facing systems
    artefact_registries:
      - id: model-registry
        content: model artefacts + ML-BOM + SLSA attestation + signatures  # chapter 04
      - id: dataset-registry
        content: dataset artefacts + SPDX 3.0 AI record + signatures
      - id: card-registry
        content: model/system/dataset/risk cards + versions + publication state  # chapter 03
      - id: packet-registry
        content: filed regulator packets + version + signatures  # chapter 05
    catalog:
      format: OSCAL 1.x (or equivalent)  # chapter 06
      content: evidence-schema definitions, profiles per tier/jurisdiction, assessment results

  provenance_and_immutability:
    signing: sigstore-cosign + in-toto attestations
    transparency_log: sigstore-rekor
    timestamping: rfc-3161 + rekor
    immutability_stores: s3-object-lock (compliance) OR azure-immutable-blob OR gcs-bucket-lock
    hash_chain_ledger: trillian OR merkle-tree append-only  # chapter 02

  packagings:
    - id: eu-ai-act-article-11
      view_definition_ref: view-defs/article-11.yaml  # chapter 05
      signers: [head-of-ai-governance, general-counsel-seat]
    - id: eu-ai-act-article-72-postmarket
      view_definition_ref: view-defs/article-72.yaml
      signers: [head-of-ai-governance]
    - id: eu-ai-act-article-73-incident
      view_definition_ref: view-defs/article-73.yaml
      signers: [ai-accountable-executive, general-counsel-seat]
    - id: sr-11-7-model-documentation
      view_definition_ref: view-defs/sr-11-7.yaml
      signers: [mrm-head, model-owner-attest]
    - id: fda-pccp
      view_definition_ref: view-defs/fda-pccp.yaml
      signers: [regulatory-affairs-head, ai-accountable-executive]
    - id: atrs-record
      view_definition_ref: view-defs/atrs.yaml
      signers: [head-of-ai-governance]

  role_model:  # chapter 07
    authors:
      cards: level-15 analyst (draft) + first-line model owner (contribution)
      risk-card: level-25 risk engineer
      eval sections: level-35 evaluation engineer (peer)
      control-implementation evidence: first-line (model owner / platform / MLOps)
      regulator packagings: assembled from substrate; sign-off per view
    reviewers:
      cards: second-line reviewer (governance office)
      supply-chain evidence: model-registry ingestion policy + second-line spot-check
    signers:
      per packaging: as declared above
    consumers:
      internal: pre-deployment gate (mod-107 ch02); ongoing assurance (mod-107 ch03); internal audit (mod-107 ch04)
      external: certification body; sector regulator; notified body; plaintiff (via legal); customer (via customer-facing DPAs)

  invariants:
    - id: I1
      description: schema-completeness against each regime
      test: for each in-scope regime, every declared obligation maps to at least one substrate store or catalog schema
    - id: I2
      description: provenance is enforceable
      test: every material evidence artefact has an in-toto or cosign signature verifiable against a transparency log
    - id: I3
      description: immutability holds at need
      test: WORM policy verified quarterly; hash-chain replay verified on demand; break-glass leaves audit trail
    - id: I4
      description: one source, many views
      test: no regulator packaging is authored in a wiki or hand-assembled Word file; every packaging traces to substrate content and a versioned view definition
    - id: I5
      description: machine-readable catalog exists and is queried
      test: OSCAL catalog present; at least three automation queries run against it in the operating rhythm
    - id: I6
      description: roles are assigned per artefact-type
      test: for every substrate content type, named author + reviewer + signer + consumer
```

Exercise-05 walks the OSCAL catalog. Exercise-04 walks the packagings. Exercise-01 walks the audit-log substrate. Exercise-02 walks the cards. Exercise-03 walks the supply-chain evidence.

## The six invariants the architecture holds

**Invariant 1 — schema completeness against each in-scope regime.** For every applicable regime (EU AI Act, SR 11-7 where the enterprise runs models under it, ISO/IEC 42001, sector overlays), the architecture names the schema that carries each declared obligation. Failure mode: a regime's obligation exists on paper and no substrate artefact carries it; the enterprise discovers this the day the regulator asks.

**Invariant 2 — provenance is enforceable.** Every material evidence artefact — model card, evaluation run, ML-BOM, SLSA attestation, regulator packet — carries a signature verifiable against a public or private transparency log, and a timestamp from a defensible source. Failure mode: the enterprise's evidence is a set of PDFs on a compliance drive; a plaintiff's expert claims the artefact was edited post-hoc; the enterprise cannot prove otherwise.

**Invariant 3 — immutability holds at need.** For every artefact class the retention obligation demands, the substrate enforces immutability — WORM object storage, hash-chained logs, external notarisation, or a combination. Break-glass procedures exist for legal hold overrides and DSAR-driven deletion; break-glass itself leaves an audit trail. Failure mode: the substrate has "immutable" storage that the platform team's break-glass procedure quietly bypasses on Fridays; the certification body samples for immutability and finds the bypass.

**Invariant 4 — one source, many views.** Every regulator-facing packaging is a *view* over the substrate driven by a versioned view definition; nothing is hand-authored per submission. Failure mode: every regulator ask triggers a hand-assembly project; the response is late, inconsistent with previous submissions, and expensive; the enterprise's compliance team spends 60% of its capacity on packaging rather than on actual compliance work.

**Invariant 5 — machine-readable catalog exists and is queried.** The evidence-schema catalog is expressed in OSCAL or an equivalent machine-readable format, and the enterprise's operating rhythm includes queries against it (freshness, completeness, coverage). Failure mode: the catalog exists as a compliance museum — the files are correct, no one queries them, drift accumulates silently until pre-certification review.

**Invariant 6 — roles are assigned per artefact type.** Every substrate content type has a named author, reviewer, signer, and consumer. When one of the roles is vacant (no reviewer designated for the dataset-card schema; no signer for the risk-card publication gate) the artefact is *out of scope of the architecture* until the assignment lands. Failure mode: evidence is produced without a reviewer; the reviewer role is invented ex-post when an audit finding lands; the ex-post reviewer has no rehearsed practice and misses systematic problems.

## Two failure modes to design against

**Failure mode 1 — the three-stories failure.** The enterprise runs three parallel evidence stacks — one for EU AI Act, one for SR 11-7, one for ISO/IEC 42001 documented information. Each stack has its own authoring toolchain, its own retention policy, its own reviewer population. At the seam where any two stacks are compared (an internal auditor cross-checks the SR 11-7 file against the ISO 42001 register; a notified-body auditor questions why the Article 11 packet cites an eval report the SR 11-7 file does not) the enterprise cannot reconcile. Every reconciliation is a hand-driven exercise that produces a fourth version of the story. The fix is architectural: the substrate is canonical; every regime's evidence lives in the same substrate under the same schema; the packagings are views. The three-stories failure is prevented by refusing to author the packagings directly.

**Failure mode 2 — the hand-assembled packet.** The substrate is canonical; the schemas are catalog-defined; the regulator-facing templates exist. But the enterprise's operating culture treats "assembling the packet" as a bespoke project for each regulator ask. The template is a Word file that a compliance analyst fills in. The re-assembly is not reproducible from the substrate at T+30 days — the source data has drifted, the analyst who did the last assembly is on parental leave, the enterprise cannot re-emit the same packet. The fix is architectural: view definitions are versioned and executable; assembly is scripted; every filed packet is re-emit-testable at any future point from the substrate at that point. Chapter 06 shows the OSCAL profile driving assembly.

## Summary

Evidence architecture is a distinct architectural concern the level-50 architect owns. Its substrate carries the raw evidence; its schema catalog names the artefact types; its provenance layer makes signatures enforceable; its packaging layer renders regulator-facing views; its role model assigns authorship, review, and sign-off. The three convergent regimes (EU AI Act, SR 11-7, ISO/IEC 42001), overlaid with sector regimes (HIPAA, FDA Part 11, PCI DSS, state Acts), demand *the same underlying evidence* in different packagings — which the architecture accepts by making the substrate canonical and the packagings views. Five stances (first-class artefact, one source many views, enforceable provenance, machine-readable catalog, role per artefact type) shape the design. Six invariants (schema completeness, provenance enforceable, immutability at need, one source many views, catalog queried, role assigned) are testable. Two failure modes (three stories, hand-assembled packet) are common. The next chapter designs the audit-log substrate that grounds provenance and immutability across the surface.
