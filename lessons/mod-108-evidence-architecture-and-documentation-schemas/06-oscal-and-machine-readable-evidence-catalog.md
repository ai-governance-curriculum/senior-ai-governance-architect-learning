# The machine-readable evidence catalog — OSCAL as the query surface over the substrate

## Why this chapter exists

Enterprises produce evidence. Enterprises cannot *query* it. That is the failure this chapter names.

At month eighteen of a serious AI-governance programme, the substrate designed in chapters 02–04 is holding real content — audit-log families across three stores, hundreds of cards under version control, ML-BOMs and SLSA attestations in the model registry, filed packets in the packet registry. And yet:

- The compliance analyst who owns the enterprise's Article 11 coverage picture spends the first three days of every notified-body engagement running tag hunts across shared drives and asking Slack channels "does anyone know if there is a current dataset card for the retrieval corpus behind the underwriting-assist system." The evidence exists; the analyst has no query surface.
- The pre-deployment gate reviewer (mod-107 chapter 02) is presented a system for review 2 and is asked to confirm the evidence contract has been discharged. The reviewer has no automated answer to "for a tier-3 system in the EU, does every required artefact exist at the required freshness with the required signatures?" The reviewer defaults to eyeball inspection of a link farm and to trusting the model owner's assertion that "everything is there."
- The internal auditor (mod-107 chapter 04) is sampling for coverage. The auditor wants to express: *for every high-risk system in the operational population, does a signed, current-within-twelve-months model card exist, is the signer a person still holding the head-of-AI-governance seat, and are the cited evaluation runs discoverable in the audit-log substrate?* The auditor cannot express that as a query. The auditor samples by pulling six systems out of a spreadsheet and inspecting them by hand.
- The regulator-facing assembly pipeline designed in chapter 05 assembles Article 11 packets by running section-projection logic against the substrate. When the pipeline hits a missing section it emits a *substrate finding* — but only for the sections declared in the template. There is no independent inventory the pipeline can cross-check to say "you asked me to project section 3 from the model card; the enterprise has declared that a system of this tier requires an *independent-evaluation attestation* attached to the model card; the current card does not carry one." That coverage claim requires an authoritative machine-readable statement of *what should exist for this system*, and the pipeline has none.

Every one of those failures has the same root: the enterprise has evidence content but no *machine-readable catalog* of what evidence classes exist, what obligations they satisfy, what freshness they demand, what signature discipline they carry, and which systems are in scope for which. Chapter 01 named this as invariant 5 — the machine-readable catalog must exist and must be queried in the operating rhythm. Chapter 06 designs the catalog itself.

The default candidate is NIST OSCAL. The chapter explains what OSCAL is, why it maps onto this problem shape, where it must be extended to carry AI-governance evidence classes it was not originally authored for, and how the catalog composes with the substrate (chapter 02), the cards (chapter 03), the supply-chain evidence (chapter 04), the regulator-facing packagings (chapter 05), and the SoA the AIMS carries (mod-105). Exercise-05 walks the drill.

## What OSCAL is, structurally

OSCAL — the Open Security Controls Assessment Language — is a NIST-authored suite of XML / JSON / YAML schemas for representing security and privacy control catalogs, control implementations, assessments, and remediation plans in a machine-readable form. NIST develops OSCAL as an open specification; the schemas and models live under `pages.nist.gov/OSCAL` and are versioned as OSCAL 1.x. The version at the time of writing this chapter is in the OSCAL 1.1.x series; verify the current minor against the NIST release notes before locking any specific field names. <!-- needs-research: verify current OSCAL 1.x minor version and pin the exact model-layer field names cited in later snippets -->

OSCAL is opinionated about what "a controls-based governance regime" looks like structurally. Its model layer decomposes the problem into seven models, each with its own top-level document:

- **Catalog.** A collection of *controls* grouped into *control families* (groups), each control carrying an id, a title, parameters (`params`), structured `parts` (statements, guidance, objectives, assessment methods), properties (`props`), and links. NIST publishes the SP 800-53 Rev 5 control catalog in OSCAL as the canonical example.
- **Profile.** An overlay on one or more catalogs that *selects* a subset of controls (an `import` with `include-controls` / `exclude-controls`), *parameterises* them (`set-parameter`), *modifies* them (`modify.alters`), and *combines* controls across catalogs. Profiles are the mechanism by which the same catalog is instantiated differently for different scopes.
- **Component-definition.** A description of a component (a technology, a service, a role) and how it *satisfies* controls. A component-definition names the controls the component can implement and how.
- **System-Security-Plan (SSP).** The per-system authored instance that names the system, its boundary, its components, and how each in-scope control is implemented on this specific system.
- **Assessment-Plan.** A plan describing what will be assessed, against which controls, with which methods, on which system.
- **Assessment-Results.** The output of an assessment run — findings, observations, evidence references, risks.
- **Plan-of-Action-and-Milestones (POA&M).** The corrective-action tracker — what findings remain open, what remediation is planned, who owns it, when it is due.

Every OSCAL document shares a *metadata block* — `metadata` with title, published date, last-modified date, version, and OSCAL version — and a set of controlled parties and roles. The `metadata.parties` array declares the organisations and individuals the document references; the `metadata.roles` array declares role identifiers (e.g. `role/head-of-ai-governance`) that responsibility-assignments elsewhere in the document bind to. Every OSCAL document also carries a `back-matter` block containing `resources` — externally-referenced artefacts (documents, evidence files, URLs) each with a UUID, a title, a description, and typed hashes (`rlinks` with `hashes.algorithm` and `hashes.value`).

The load-bearing OSCAL primitives for the rest of this chapter are: **controls** as the statements of what must exist / hold true; **profiles** as the tier-and-jurisdiction overlay mechanism; **component-definitions** as the description of the enterprise's AI systems; **SSPs** as the per-system authored instance; **assessment-plans** as the expression of the pre-deployment gate and ongoing-assurance runs; **assessment-results** as their output; **POA&M** as the CAPA tracker; and **back-matter resources** as the mechanism by which the catalog references substrate artefacts by hash.

## What it is not

OSCAL is a *schema and interchange format*. It is not a great many things it is sometimes mistaken for.

- **Not a GRC platform.** OSCAL does not run workflows, host UIs, send notifications, or execute assessments. It defines the shape the data takes; the platform (whether an off-the-shelf GRC tool, a bespoke build, or a hand-authored Git repository of YAML files) is separate.
- **Not a substitute for authoring the evidence schemas.** OSCAL carries *references* to the enterprise's evidence artefacts (cards, ML-BOMs, filed packets) through back-matter resources; it does not carry the evidence content itself. The card schemas are authored in chapter 03; the supply-chain evidence schemas in chapter 04; the packaging templates in chapter 05. OSCAL is where those schemas' *existence, obligations, and coverage* are named, not where their content lives.
- **Not a magic bullet against evidence-architecture drift.** The catalog is only true if the operating rhythm updates it, queries it, and monitors catalog-substrate agreement. A dead catalog is a compliance museum (failure mode 1 below).
- **Not the only choice.** JSON Schema authored per artefact class is a viable lighter-weight substitute where OSCAL's model surface is over-scoped. SCAP (Security Content Automation Protocol) is older and narrower. XBRL is the machine-readable-financial-statement lineage and shows up for financial-facing artefacts. Some enterprises use a domain-specific format expressed in Cue or Rego and hand-author the composition. The reason this chapter defaults to OSCAL is composition: the same catalog serves security controls, privacy controls, and AI-governance controls in one representation the enterprise's security and privacy functions already understand.

The architect is not required to adopt OSCAL. The architect is required to adopt *a* machine-readable catalog and to design the substrate against it. Where the enterprise's security programme already runs on OSCAL, adopting it for AI-governance evidence is the compositional choice that avoids two parallel catalogs.

## Why OSCAL specifically for AI evidence architecture

OSCAL was authored to represent security control catalogs, principally NIST SP 800-53. The shape maps onto AI-governance evidence architecture more directly than it may first appear.

- **Controls become the enterprise's controls.** The enterprise's control library (mod-102) — the AI-specific and AI-adjacent controls the enterprise has authored across risk categories — is the AI equivalent of an SP 800-53 baseline. The controls express what must exist or hold true (e.g. *every high-risk system has a current signed model card*; *every training run produces an ML-BOM*; *every regulator packet references a substrate snapshot timestamp*). OSCAL's control shape (id, statements, guidance, objectives, assessment methods) fits.
- **Profiles instantiate a subset with parameters.** Different tiers and jurisdictions need different subsets of controls with different parameters. Tier-4 EU high-risk demands more controls at tighter parameter values than Tier-1 unregulated internal. OSCAL profiles express that overlay natively.
- **Component-definitions express the AI systems.** Every AI system in the operational population is a component (in some cases a system-of-components). The component-definition captures what capabilities the component provides and, for each control the component is in scope for, how the component contributes to the implementation.
- **System-Security-Plans express per-system evidence binding.** The SSP — better read here as *system-governance-plan* for the AI use — is the per-system authored instance that the pre-deployment gate reviewer runs against. Every in-scope control is either implemented on the system with a named implementation and referenced evidence, or explicitly out of scope with justification.
- **Assessment-plans express the pre-deployment gate and ongoing-assurance runs.** The gate's evidence-contract review (mod-107 chapter 02, review 2) is an assessment-plan: which controls will be assessed, with which methods, against which system, producing what evidence references.
- **Assessment-results express findings.** The gate's output — pass, conditional pass, fail — with the specific control-level findings, becomes an assessment-results document. Ongoing-assurance re-assessments produce further assessment-results over time.
- **POA&M expresses CAPA.** Findings that remain open (a card is over-age; a system is missing an ML-BOM; a control implementation has drifted) live in POA&M until closed. Mod-105 and mod-107 CAPA processes drop into this shape.

The composition argument is that these mappings are not stretches — they are the mappings OSCAL's authors intended for security controls, applied to a domain where the shape of the problem (a controls-based governance regime with per-system implementation, assessment cadence, and remediation tracking) is the same.

## The AI-evidence OSCAL profile — the extension the enterprise authors

OSCAL out-of-the-box does not know that a model card exists, that ISO/IEC 42001 Annex A defines an AI-specific control set, or that an ML-BOM has a distinct provenance shape from a classical SBOM. The enterprise extends OSCAL along three surfaces.

**Extension surface 1 — additional control catalogs.** The enterprise authors (or ingests) OSCAL catalogs for control regimes OSCAL does not ship natively. NIST publishes SP 800-53 in OSCAL. The enterprise adds:

- The ISO/IEC 42001 Annex A control catalog, expressed in OSCAL. ISO does not publish this in OSCAL form; the enterprise authors the mapping. <!-- needs-research: check whether any community project has published ISO/IEC 42001 Annex A as an OSCAL catalog; if so, cite it -->
- The enterprise's own AI-specific controls that go beyond SP 800-53 and Annex A — evaluation-methodology controls, red-team controls, model-monitoring controls, cards-and-documentation controls, supply-chain-evidence controls — expressed as an OSCAL catalog with the enterprise as the source.
- The NIST AI RMF playbook controls, expressed in OSCAL, where the enterprise has adopted them.
- A jurisdiction-specific catalog for each regime the enterprise is subject to — an EU AI Act obligations catalog, an SR 11-7 obligations catalog, an FDA PCCP obligations catalog — expressed as controls where each control's statements are the regulatory obligation and its objectives are the assessable clauses. Some regulatory-obligation content is authoritative from the regulator's text; the enterprise's OSCAL representation is a *shape* over that authoritative text, and the shape is what OSCAL adds.

**Extension surface 2 — artefact classes not native to OSCAL.** OSCAL knows about *evidence* as an assessment-results concept (an assessment observation references evidence via a `related-observations` reference or a back-matter resource). It does not know about the artefact *classes* the AI-evidence substrate holds. The enterprise declares these classes and represents them as back-matter resources with typed properties.

The classes to declare (each is an entry in the enterprise's authored artefact-class inventory, itself expressed as a controlled document referenced from the catalog):

| Artefact class | Where authored | OSCAL representation |
|---|---|---|
| Model card | chapter 03 | `back-matter/resource` with `props/name=artefact-class` `value=model-card` plus typed hash of the published card |
| System card | chapter 03 | same shape, `value=system-card` |
| Dataset card | chapter 03 | same shape, `value=dataset-card` |
| Risk card | chapter 03 | same shape, `value=risk-card` |
| CycloneDX ML-BOM | chapter 04 | `back-matter/resource` with `value=ml-bom-cyclonedx` |
| SPDX 3.0 AI-profile record | chapter 04 | `value=spdx-3-ai` |
| SLSA build attestation | chapter 04 | `value=slsa-attestation` |
| Sigstore signature / Rekor entry | chapter 04 | `value=sigstore-signature` |
| Regulator packet (Article 11, 72, 73, SR 11-7, FDA PCCP) | chapter 05 | `value=regulator-packet-<template-id>` |
| Substrate log entry references (family 1/2/3) | chapter 02 | typically via `oscal-link` from an assessment observation into a log-store query URI |

Where OSCAL 1.x does not have a first-class field for "artefact class taxonomy," the enterprise uses `props` — OSCAL's typed extension mechanism — with an enterprise-owned namespace (a `ns` URI the enterprise controls) and named properties. This is the intended extension pathway. <!-- needs-research: verify OSCAL's guidance on `props/ns` naming conventions for enterprise extensions and pin the recommended URI shape -->

**Extension surface 3 — profile parameterisation for freshness, retention, and signature discipline.** Each artefact class has properties that vary by tier and jurisdiction — freshness threshold (a model card is "current" if signed within 12 months for tier-3, 6 months for tier-4), retention duration, minimum SLSA level, required-signer role. OSCAL profiles carry parameterisation via `set-parameter`; the enterprise expresses these thresholds as parameters on the controls that assert them, and each tier-and-jurisdiction profile sets the parameter to the appropriate value.

The enterprise does *not* invent an OSCAL extension standard. There is no widely-adopted community-owned OSCAL AI-evidence profile at the time of writing. <!-- needs-research: monitor NIST OSCAL working groups, the OpenControl / ComplianceAsCode communities, and the CNCF supply-chain SIG for an emerging community OSCAL profile for AI-evidence artefacts; adopt when one lands -->

## A concrete OSCAL catalog snippet

The following is a small illustrative OSCAL catalog snippet showing five controls the enterprise's AI-evidence catalog would carry. The snippet uses OSCAL 1.x YAML syntax. Field names correspond to OSCAL 1.1.x where I am confident; uncertain field names are marked. Do not copy this into production without verifying against the current NIST OSCAL specification.

```yaml
catalog:
  uuid: 3a1f5c8e-2b64-4b7f-9d1e-01c3b8f4e2a6
  metadata:
    title: Enterprise AI-Evidence Control Catalog
    published: 2027-09-06T00:00:00Z
    last-modified: 2027-09-06T00:00:00Z
    version: "1.2.0"
    oscal-version: "1.1.2"  # <!-- needs-research: verify current 1.1.x point release -->
    parties:
      - uuid: 8b7a3e2d-5f01-4c9a-a3b6-2d1e5c7f8a90
        type: organization
        name: Enterprise AI Governance Office
    roles:
      - id: head-of-ai-governance
        title: Head of AI Governance
      - id: ai-governance-analyst
        title: AI Governance Analyst
      - id: ai-evaluation-engineer
        title: AI Evaluation Engineer (peer, level 35)
      - id: model-owner
        title: Model Owner (first line)
      - id: mrm-head
        title: Model Risk Management Head
      - id: ai-accountable-executive
        title: AI-Accountable Executive
  groups:
    - id: ai-doc
      title: AI Documentation and Disclosure Controls
      controls:
        - id: ai-doc-01
          title: Model card currency and signature
          params:
            - id: ai-doc-01_max-age-months
              label: Maximum permitted age of a signed model card, in months
              values: ["12"]
            - id: ai-doc-01_required-signer-role
              label: Role that must sign the published model card
              values: ["head-of-ai-governance"]
          parts:
            - id: ai-doc-01_smt
              name: statement
              prose: >
                Every AI system in the operational population shall have a
                published model card, signed by the role identified in
                {{ insert: param, ai-doc-01_required-signer-role }}, whose
                signature timestamp is no older than
                {{ insert: param, ai-doc-01_max-age-months }} months as of
                assessment date.
            - id: ai-doc-01_obj
              name: assessment-objective
              prose: >
                Determine whether every operational system's model card exists,
                is signed by the required-signer role's current seat, and is
                current within the max-age threshold.
            - id: ai-doc-01_gdn
              name: guidance
              prose: >
                A card whose signer no longer holds the required role at
                assessment date is treated as unsigned. See chapter 03 for
                the card lifecycle and re-review triggers; see chapter 07
                for the responsibility assignment across analyst, evaluation
                engineer, and head of AI governance.
          props:
            - name: artefact-class
              ns: https://oscal.enterprise.corp/ns/ai-evidence
              value: model-card
            - name: substrate-store
              ns: https://oscal.enterprise.corp/ns/ai-evidence
              value: card-registry
          links:
            - href: "#ref-chapter-03-card-family"
              rel: reference
            - href: "#ref-ch07-role-model"
              rel: reference

        - id: ai-doc-02
          title: Training run has attested ML-BOM
          params:
            - id: ai-doc-02_min-slsa-level
              label: Minimum SLSA level for the training-run provenance
              values: ["2"]
          parts:
            - id: ai-doc-02_smt
              name: statement
              prose: >
                Every training and fine-tuning run whose output artefact is
                promoted through the model registry shall produce a
                CycloneDX ML-BOM and a SLSA attestation at level not less
                than {{ insert: param, ai-doc-02_min-slsa-level }}, both
                signed via Sigstore and recorded in Rekor.
            - id: ai-doc-02_obj
              name: assessment-objective
              prose: >
                Determine, per training-run pipeline execution over the
                assessment window, whether an ML-BOM and SLSA attestation
                of sufficient level exist and are signature-verified.
          props:
            - name: artefact-class
              ns: https://oscal.enterprise.corp/ns/ai-evidence
              value: ml-bom-cyclonedx
            - name: substrate-store
              ns: https://oscal.enterprise.corp/ns/ai-evidence
              value: model-registry

        - id: ai-doc-03
          title: Regulator packet cites substrate snapshot
          parts:
            - id: ai-doc-03_smt
              name: statement
              prose: >
                Every filed regulator-facing packet (Article 11, Article 72,
                Article 73, SR 11-7 documentation, FDA PCCP, ATRS) shall
                record the substrate snapshot timestamp against which it
                was assembled, the assembly-pipeline version, and the
                template version.
            - id: ai-doc-03_obj
              name: assessment-objective
              prose: >
                Determine whether the packet-registry entry for each filed
                packet includes (template_id, template_version,
                pipeline_version, snapshot_ts) as required.
          props:
            - name: artefact-class
              ns: https://oscal.enterprise.corp/ns/ai-evidence
              value: regulator-packet
            - name: substrate-store
              ns: https://oscal.enterprise.corp/ns/ai-evidence
              value: packet-registry

    - id: ai-log
      title: AI Audit-Log Substrate Controls
      controls:
        - id: ai-log-01
          title: Lifecycle log retention against tier
          params:
            - id: ai-log-01_min-retention-tier3
              values: ["5y"]
            - id: ai-log-01_min-retention-tier4
              values: ["10y"]
          parts:
            - id: ai-log-01_smt
              name: statement
              prose: >
                The family-1 lifecycle log shall retain events for
                {{ insert: param, ai-log-01_min-retention-tier3 }} for
                tier-3 systems and
                {{ insert: param, ai-log-01_min-retention-tier4 }} for
                tier-4 systems, under WORM object-lock in compliance mode.
          props:
            - name: substrate-store
              ns: https://oscal.enterprise.corp/ns/ai-evidence
              value: family-1-lifecycle-log

    - id: ai-sup
      title: AI Supply-Chain Evidence Controls
      controls:
        - id: ai-sup-01
          title: Third-party ingest carries a recorded provenance disposition
          parts:
            - id: ai-sup-01_smt
              name: statement
              prose: >
                Every third-party model ingest (open-weight checkpoint,
                vendor-supplied fine-tune, frontier-model API binding)
                shall carry a recorded provenance disposition — one of
                {vendor-verified, hub-hash-only, exception-with-executive-sponsor}.
                Exception dispositions require a named executive sponsor
                and a corresponding risk-register entry.
          props:
            - name: artefact-class
              ns: https://oscal.enterprise.corp/ns/ai-evidence
              value: third-party-ingest-disposition
            - name: substrate-store
              ns: https://oscal.enterprise.corp/ns/ai-evidence
              value: family-1-lifecycle-log
  back-matter:
    resources:
      - uuid: 5c2d8e91-3f47-4b0a-9c1e-6d3f8a2b7e50
        title: Chapter 03 card family reference
        rlinks:
          - href: "https://governance.enterprise.corp/lessons/mod-108/03-cards"
        props:
          - name: reference-type
            value: internal-doc
      - uuid: 9e7b4d31-8a25-4f6c-b1d0-2e5a9c3f8b16
        title: Chapter 07 role model reference
        rlinks:
          - href: "https://governance.enterprise.corp/lessons/mod-108/07-coordination"
```

Several deliberate choices are visible in the snippet. Every control declares one or more `props` in the enterprise's OSCAL extension namespace naming the artefact class and the substrate store — this is what makes the catalog queryable by artefact class (see the three killer queries below). Every control statement uses OSCAL's parameter-substitution syntax so profiles can set tier-and-jurisdiction-specific values without editing the control text. Every assessable clause has an `assessment-objective` part that assessments can bind findings to. Back-matter resources with typed hashes are the mechanism for referencing substrate artefacts (in a real catalog every referenced card, ML-BOM, and packet would have a back-matter entry with `hashes.algorithm=SHA-256` and `hashes.value=<hex>`).

## Profiles — tier and jurisdiction as OSCAL profile overlays

The catalog above is the *superset*. Individual tier-and-jurisdiction combinations are OSCAL profiles that import the catalog, select the applicable controls, and parameterise them.

An enterprise operating across four tiers and three jurisdictions might carry twelve profiles (tier × jurisdiction), each a distinct OSCAL profile document. A profile for tier-4 EU high-risk systems, sketched:

```yaml
profile:
  uuid: 7f2b91e0-8d54-4c3a-a1b6-9e4d2f5c8a70
  metadata:
    title: Tier-4 EU High-Risk AI System Profile
    version: "2.0.0"
    oscal-version: "1.1.2"
    published: 2027-09-06T00:00:00Z
    last-modified: 2027-09-06T00:00:00Z
    parties:
      - uuid: 8b7a3e2d-5f01-4c9a-a3b6-2d1e5c7f8a90
        type: organization
        name: Enterprise AI Governance Office
    roles:
      - id: head-of-ai-governance
        title: Head of AI Governance
      - id: ai-accountable-executive
        title: AI-Accountable Executive
  imports:
    - href: "https://catalogs.enterprise.corp/ai-evidence-catalog-v1.2.0.yaml"
      include-controls:
        - with-ids:
            - ai-doc-01
            - ai-doc-02
            - ai-doc-03
            - ai-log-01
            - ai-sup-01
    - href: "https://catalogs.enterprise.corp/eu-ai-act-obligations-catalog-v1.0.0.yaml"
      include-all: {}
    - href: "https://catalogs.enterprise.corp/iso-42001-annex-a-catalog-v1.0.0.yaml"
      include-all: {}
  modify:
    set-parameters:
      - param-id: ai-doc-01_max-age-months
        values: ["6"]   # tier-4 requires re-signature every 6 months
      - param-id: ai-doc-01_required-signer-role
        values: ["head-of-ai-governance"]
      - param-id: ai-doc-02_min-slsa-level
        values: ["3"]   # tier-4 requires SLSA L3
      - param-id: ai-log-01_min-retention-tier4
        values: ["10y"]
  # <!-- needs-research: verify the OSCAL profile field name for
  #      responsibility-assignment overlay; OSCAL binds responsibilities
  #      in the SSP layer rather than the profile layer, and the profile
  #      layer's mechanism for expressing party-scoped applicability
  #      should be pinned to the current spec -->
```

The profile is short — that is the point. It expresses only the delta from the underlying catalog: which controls are in scope, what parameter values apply, what responsibility overlays hold. When a new tier-4 EU system is onboarded the enterprise instantiates a component-definition and an SSP that reference this profile; every control in the profile is either implemented on the system, marked out-of-scope with justification, or flagged as gap.

Every profile is versioned, is authored by the level-50 architect, is ratified by the head of AI governance, and is retired only via a governance-log substrate event (chapter 02 family-3). Profile changes are second-line-reviewed the same way template changes are (chapter 05).

## Component-definitions and system-governance-plans

The catalog and the profiles carry *what the enterprise expects* of AI systems in a given scope. Component-definitions and SSPs carry *what a specific AI system is and does about it*.

A **component-definition** describes an AI system as a component (or group of components) with declared capabilities and, per applicable control, an implementation statement referencing evidence. The component-definition is authored by the model owner in collaboration with the governance analyst (mod-108 chapter 07 walks the coordination) and is submitted for pre-deployment gate as part of the evidence contract. Sketch:

```yaml
component-definition:
  uuid: 4e8a2d15-9c3b-4f6d-a2e0-7b1c5d3f8a91
  metadata:
    title: Underwriting-Assist AI System Component Definition
    version: "1.0.0"
    oscal-version: "1.1.2"
  components:
    - uuid: 6a3e9c14-5f8b-4d2a-b7c1-8e0d4a5f2b93
      type: ai-system
      title: underwriting-assist-v3
      description: >
        Retrieval-augmented generative AI system supporting underwriter
        adjudication of consumer credit applications in the EU market.
      props:
        - name: system-tier
          ns: https://oscal.enterprise.corp/ns/ai-evidence
          value: "4"
        - name: jurisdictions
          ns: https://oscal.enterprise.corp/ns/ai-evidence
          value: "EU"
        - name: eu-ai-act-annex-iii-category
          ns: https://oscal.enterprise.corp/ns/ai-evidence
          value: "credit-scoring"
      control-implementations:
        - uuid: 2b7d1e05-4a9c-4f3b-8e6a-1d5c2f8b9e04
          source: "https://profiles.enterprise.corp/tier-4-eu-profile-v2.0.0.yaml"
          implemented-requirements:
            - uuid: 3c8e2f16-5b0d-4a7c-9f1b-2e6d3a8c9f15
              control-id: ai-doc-01
              description: >
                Model card current-within-6-months requirement met.
                See back-matter resource for the current signed card.
              props:
                - name: implementation-status
                  value: implemented
              links:
                - href: "#current-model-card-underwriting-v3"
                  rel: evidence
```

A **system-governance-plan** — what OSCAL calls a *system-security-plan* — is the per-system authored instance that names the system boundary, the components in scope, and the control implementations. The SSP is the artefact the pre-deployment gate reviewer runs against, the ongoing-assurance programme monitors against, and the internal auditor samples against.

The SSP composes with mod-105's Statement of Applicability. The SoA declares which Annex A controls the AIMS applies enterprise-wide; the profile selects the applicable catalog controls for the tier-and-jurisdiction; the SSP is the per-system instance that binds implementations to the selected controls. The three-layer composition (SoA drives profile selection at scope; profile drives control selection per system; SSP drives implementation per control) is what makes the OSCAL surface tractable — no single document has to carry everything.

## Assessment plans and assessment results

The pre-deployment gate (mod-107 chapter 02) and the ongoing-assurance programme (mod-107 chapter 03) are OSCAL assessment activities.

An **assessment-plan** for the pre-deployment gate for a tier-4 EU high-risk system declares: which controls will be assessed (typically every control in the profile), by which methods (examine, interview, test — OSCAL's three canonical assessment methods), on which system, producing which evidence. The plan references the SSP, names the assessor parties (the governance analyst, the evaluation engineer, the head of AI governance as gate chair), and specifies the assessment window.

An **assessment-results** document is produced when the assessment runs. It carries the *findings* — for each control assessment-objective, a determination (satisfied, other-than-satisfied) with cited observations. Observations reference specific evidence via back-matter resources. Findings that are other-than-satisfied become POA&M entries.

The mapping from mod-107 review structure to OSCAL:

| Mod-107 review | OSCAL element |
|---|---|
| Pre-deployment gate charter | assessment-plan for the gate meeting |
| Evidence contract discharge (gate review 2) | assessment-plan's control-selection + method=examine |
| Independent-evaluation attestation (gate review 3) | assessment-plan's method=test |
| Residual-against-appetite decision (gate review 4) | assessment-results.finding with risk-link |
| Gate decision record | assessment-results with overall status |
| Ongoing-assurance cadence | assessment-plan with recurring schedule |
| Drift-triggered re-assessment | ad-hoc assessment-plan referencing the trigger |
| Corrective action, remediation tracking | POA&M item per open finding |

Every assessment-plan and assessment-results is versioned, signed, and lands as a substrate event in the family-3 governance-workflow log (chapter 02).

## The three killer queries the catalog enables

The catalog exists to be queried. Three queries carry most of the operational value; every enterprise architecture should be able to answer them directly against the OSCAL substrate without a human doing tag hunts.

### Query 1 — coverage

*List every system-governance-plan where the model-card-currency control (ai-doc-01) is not marked implemented, or where the referenced card resource is missing its typed hash.*

Sketch of the query in a YAML-DSL style (the actual query language depends on the OSCAL query layer the enterprise adopts — SQL over an OSCAL-shaped database, JQ over YAML documents, or a purpose-built OSCAL query tool):

```yaml
query:
  id: coverage-model-card
  over: system-governance-plans
  find:
    - ssp
    where:
      - ssp.control-implementations
        .implemented-requirements[control-id == "ai-doc-01"]
        .props[name == "implementation-status"].value != "implemented"
  or:
    - ssp.control-implementations
        .implemented-requirements[control-id == "ai-doc-01"]
        .links[rel == "evidence"].href
        resolves-to: back-matter-resource
        where: resource.rlinks[0].hashes is empty
  return:
    - ssp.metadata.title
    - ssp.components[0].props[name == "system-tier"].value
    - ssp.metadata.last-modified
    - failure-reason
```

The auditor no longer samples six systems by hand. The auditor runs the query and gets the population.

### Query 2 — freshness

*List every system-governance-plan where an evidence artefact's signature timestamp is older than the tier-implied threshold from the applicable profile.*

Sketch:

```yaml
query:
  id: freshness-all-artefacts
  over: system-governance-plans
  for-each: ssp.control-implementations.implemented-requirements
    where: ir.links[rel == "evidence"].exists
  join:
    - resource = back-matter.resources[uuid == ir.links[rel == "evidence"].href.fragment]
    - control = catalog.controls[id == ir.control-id]
    - profile = profiles.by-uuid[ssp.imports.source]
    - max-age = profile.set-parameters
        [param-id == control.params[label ~= "max age"].id]
        .values[0]
    - signature-ts = resource.props
        [name == "signature-timestamp"
         and ns == "https://oscal.enterprise.corp/ns/ai-evidence"]
        .value
  filter:
    - (now - signature-ts) > max-age
  return:
    - ssp.metadata.title
    - control.id
    - signature-ts
    - max-age
    - age-in-months
```

The pre-deployment gate reviewer now has a machine-check for the evidence contract's freshness dimension without opening a card.

### Query 3 — obligation crosswalk

*For a specific regulator ask — an Article 11 notified-body request for the underwriting-assist system — list every substrate artefact the Article 11 packet requires, its current status per the catalog, and any gaps.*

Sketch:

```yaml
query:
  id: article-11-coverage-per-system
  inputs:
    - system-id
    - regulator-ask: "eu-ai-act-article-11"
  find:
    - template = packet-registry.templates[id == regulator-ask]
    - required-sections = template.sections
    - substrate-refs = for section in required-sections:
        section.source-substrate-artefacts
  for-each: substrate-ref
    join:
      - ssp = system-governance-plans[system-id == input.system-id]
      - implementation = ssp.control-implementations
          .implemented-requirements
          .filter(ir -> ir.props[artefact-class == substrate-ref.artefact-class]
                        .exists)
      - resource = back-matter.resources
          [uuid == implementation.links[rel == "evidence"].href.fragment]
    return:
      - substrate-ref.artefact-class
      - substrate-ref.expected-freshness
      - resource.signature-timestamp (if present)
      - implementation-status
      - gap (if implementation-status != "implemented"
             or (now - resource.signature-timestamp) > substrate-ref.expected-freshness)
```

The compliance analyst no longer runs tag hunts to answer "what do we have and what do we owe" for a specific packet. The query returns the coverage picture in seconds. Where a gap exists the packet-assembly pipeline (chapter 05) fails-closed and the gap becomes a substrate finding fed to CAPA (POA&M).

These three queries are what the catalog exists for. An enterprise that has authored the catalog but cannot run them has authored a compliance museum.

## The assembly path — how the OSCAL catalog drives packet assembly

Chapter 05 designed the regulator-facing packaging templates as executable view definitions over the substrate. The OSCAL catalog closes the loop by expressing each template's *coverage requirement* as an assessment-plan whose objectives are the template sections.

Schematically:

```yaml
assembly_flow:
  step-1_load_template:
    input: template.id (e.g. eu-ai-act-article-11)
    output: template document with section-projection logic

  step-2_load_target_system:
    input: system-id
    output:
      - ssp
      - profile (via ssp.imports.source)
      - applicable-catalog-controls (via profile.include-controls)

  step-3_derive_assessment_plan:
    input: template + applicable-catalog-controls
    output: assessment-plan whose objectives are the union of
            (a) template.sections and
            (b) catalog controls whose artefact-class appears in
                any template.sections.source-substrate-artefacts
    guarantee: no template section references an artefact class that
               is not declared in the catalog (see invariant 2 and
               failure mode 2 below); if it does, the assembly refuses.

  step-4_run_assessment:
    input: assessment-plan + substrate snapshot at snapshot_ts
    output: assessment-results
      for each control: implementation-status + evidence-references
      for each template section: source-substrate-artefacts resolved
                                or gap declared

  step-5_gate_check:
    input: assessment-results
    if: any gap
    then:
      substrate-finding emitted; POA&M entry created;
      assembly pipeline halts pre-render; packet not filed
    else: proceed to render

  step-6_render_and_file:
    input: assessment-results + template + substrate content
    output: rendered packet in regulator format + signatures +
            packet-registry entry with (template_version,
            pipeline_version, snapshot_ts, assessment-results-uuid)
```

The consequence is architectural: no packet is filed whose coverage is not first proven by an OSCAL assessment-results document. The gap-driven fail-closed is what turns the packet-assembly pipeline from a hope-based mechanism into a claim the enterprise can defend. Every filed packet carries a link to the assessment-results that authorised it; re-emitting the packet at the same snapshot_ts also reproduces the assessment-results.

## Six invariants the catalog holds

**Invariant 1 — one representation is canonical.** For every controlled schema in the enterprise's AI-evidence programme there is one authoritative OSCAL (or equivalent) representation, and it is the source the operating rhythm reads from. Duplication in spreadsheets or wiki pages is drift. Failure mode fix: any secondary representation is stamped as a derived view and is regenerated from the OSCAL source; hand-maintained secondary representations are retired.

**Invariant 2 — every substrate artefact class is declared in the catalog.** Every artefact class the substrate holds (model card, dataset card, risk card, system card, ML-BOM, SPDX record, SLSA attestation, Sigstore signature, filed regulator packet, family-1/2/3 log entry classes) has a corresponding entry in the enterprise's OSCAL extension inventory and is referenced by at least one control's `props/artefact-class`. Failure mode fix: quarterly catalog-substrate reconciliation query enumerates substrate artefact classes not declared in the catalog; each is either declared in the next catalog revision or the substrate stops emitting the undeclared class.

**Invariant 3 — every regulator packet is a query against the catalog.** No filed regulator packet is authored outside the assembly pipeline; every filed packet's coverage is proved by an OSCAL assessment-results document produced against the catalog. Failure mode fix: packet-registry ingestion policy refuses filings that do not carry an assessment-results reference; policy is expressed in-code and versioned.

**Invariant 4 — the catalog is versioned.** Every catalog, profile, and component-definition carries a semver in its `metadata.version`; changes go through second-line review and produce a family-3 governance-workflow log event; SSPs and assessment-plans reference the specific catalog and profile versions they were authored against. Failure mode fix: no direct edits to production catalog documents; edits happen in the enterprise's Git-of-record for the OSCAL substrate and land only via reviewed merges; every merge produces a versioned release.

**Invariant 5 — the catalog is queried in the operating rhythm.** The three killer queries (coverage, freshness, obligation-crosswalk) run on a defined cadence — coverage weekly, freshness monthly, obligation-crosswalk on every regulator ask and on quarterly rehearsal. Query outputs feed the pre-deployment gate, the ongoing-assurance programme, and internal audit sampling. Failure mode fix: the operating rhythm calendar lists the queries by cadence with named owners; query outputs are archived as substrate events; unrun queries produce a finding in the next second-line review cycle.

**Invariant 6 — catalog-substrate agreement is monitored.** A monitoring process runs against the substrate and catalog periodically to detect drift — controls that reference substrate stores that do not exist; substrate stores whose content classes are not declared in the catalog; profile parameter values that no SSP has adopted; POA&M items past due. The monitoring output is a report to the head of AI governance in the operating rhythm. Failure mode fix: drift-monitor is a scheduled job whose outputs are substrate events; a monitor whose last successful run is more than a cadence-length ago is itself a finding.

## Two failure modes to design against

**Failure mode 1 — OSCAL as compliance museum.** The enterprise authors the catalog. The catalog is well-shaped, the profiles are correct, the SSPs exist for every system in the operational population. Nobody queries any of it. The compliance analyst still runs tag hunts because the tag-hunt workflow was in place before the catalog and no one retrained. The pre-deployment gate reviewer still eyeballs the evidence contract because the gate ceremony was designed against a link farm and no automated coverage check was added. The internal auditor still samples by hand because the audit programme was designed before OSCAL landed and no one updated the sampling methodology. At the next external audit the certification body opens the catalog, finds it well-formed, and asks the enterprise to show three months of query outputs — the enterprise cannot; the catalog is not part of the operating rhythm. The finding is "documentation exists but is not operationalised." The fix is architectural and cultural: the operating rhythm calendar names the queries; the pre-deployment gate charter (mod-107 chapter 02) is amended to require the machine-check outputs alongside the human review; the internal audit programme (mod-107 chapter 04) is amended so sampling starts from the catalog query rather than from a spreadsheet; the head of AI governance's operating-rhythm dashboard shows the query outputs with named owners for each finding.

**Failure mode 2 — catalog drift.** The substrate begins to carry an artefact class the catalog does not declare. The most common concrete instance: a new evaluation methodology lands (a benchmark-of-benchmarks emerges; a red-team methodology from an external AISI publication is adopted); the evaluation engineer starts producing a new class of evaluation-workpaper artefact and depositing it in a new substrate store; the model owners cite these in their evidence-contract submissions; the catalog is not updated because the new artefact class was introduced through the evaluation function rather than through the level-50 architect's design cycle. The pre-deployment gate reviewer sees the new artefacts and accepts them; the OSCAL coverage query does not know about them and continues to report the systems as compliant on the old evidence; the internal auditor samples the OSCAL query, misses the systems with drifted evidence, and reports coverage that is a partial fiction. Six months later a certification body sample surfaces the class of artefact the OSCAL surface does not describe. The finding is that the catalog and substrate are out of agreement. The fix is architectural: the catalog-substrate agreement monitor (invariant 6) runs on cadence and any new substrate artefact class produces a catalog-authoring finding; the responsibility for extending the catalog to declare a new artefact class is assigned (chapter 07); no new artefact class is admitted to the substrate without a paired catalog extension proposal in-flight.

## Summary

The machine-readable evidence catalog is the surface that turns the substrate designed in chapters 02–04 from a store of evidence into a queryable claim about what evidence exists for what system at what freshness against what obligations. OSCAL — with its catalog, profile, component-definition, SSP, assessment-plan, assessment-results, and POA&M models — is the default choice because its shape maps onto the AI-governance evidence problem more directly than any alternative and because the enterprise's security programme is likely already using it. The enterprise extends OSCAL along three surfaces: additional control catalogs (ISO/IEC 42001 Annex A, jurisdictional obligation catalogs, the enterprise's own AI-specific controls); artefact-class declarations expressed through the OSCAL `props` extension mechanism referencing substrate content by hash; and profile parameterisation for tier-and-jurisdiction-specific thresholds. Profiles overlay tier and jurisdiction; component-definitions and SSPs express the per-system instance the pre-deployment gate reviewer runs against; assessment-plans and assessment-results express the gate and the ongoing-assurance programme; POA&M expresses CAPA. Three killer queries — coverage, freshness, obligation-crosswalk — are what the catalog exists for. The regulator-facing packet-assembly pipeline (chapter 05) is closed against the catalog: no packet is filed whose coverage is not first proved by an OSCAL assessment-results document. Six invariants and two failure modes shape the design. The next chapter pins the coordination contracts with the level-15 governance analyst, the level-25 risk engineer, and the peer level-35 evaluation engineer that make the substrate, the catalog, and the packagings actually populated by the right roles at the right time.
