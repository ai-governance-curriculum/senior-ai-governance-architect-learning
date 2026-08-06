# AI supply-chain evidence — CycloneDX ML-BOM, SPDX 3.0 AI, SLSA, Sigstore

## Why this chapter exists

Ask three questions of a typical AI programme after fifteen months of operation and observe how quickly the answers stall.

- Which of our production systems are downstream of the pretrained checkpoint we pulled from a public model hub last spring? Do we know whether that checkpoint has been superseded by a variant with revised licence terms?
- We were told a training dataset we consumed has a licence issue and needs to be quarantined; which of our currently-serving models depend on it, at what depth, and what does the enterprise need to do about the models?
- A regulator asks: is the checkpoint you filed with your Article 11 packet last quarter the checkpoint that is currently serving customers, and can you show the provenance of every intermediate build?

If the answers to those questions require anyone to spelunk git history and a data scientist's Slack, the enterprise does not have an AI supply-chain evidence slice. It has an inventory story it hopes never to have to defend at speed.

Software supply chain has been an enterprise-security concern for years; SPDX and CycloneDX SBOMs have been the deliverables that answer "what libraries did we build with." AI systems have the same problem in a shape that the classical SBOM was not designed for. Models are components. Datasets are components. Training runs and fine-tuning runs are "builds" whose provenance the enterprise wants to attest at the same level of rigour it attests the software build. The enterprise's model registry is the gating equivalent of a container registry — and it needs to enforce evidence checks at ingestion the same way a modern container registry enforces SLSA-attested signatures.

This chapter designs the AI supply-chain evidence slice: the four evidence formats (CycloneDX ML-BOM, SPDX 3.0 AI profile, SLSA attestations, Sigstore signing), the producer / verifier / gating actor topology across the enterprise, the model registry as the gating point, and the composition with the chapters 02 (substrate) and 03 (cards) already designed. Exercise-03 walks the drill against a concrete scenario.

## What the AI supply-chain evidence slice is, structurally

The slice is:

- **A per-artefact machine-readable inventory.** Every model, every dataset, every material intermediate has a bill of materials that names its components and their provenance.
- **A per-build attestation.** Every training / fine-tuning / evaluation build produces an attested provenance record — who ran the build, on what infrastructure, against what inputs, at what commit, producing what output hash.
- **A signature chain.** Every artefact and every attestation is signed; signatures are recorded in a transparency log; verification is straightforward for downstream consumers.
- **An enforcement point.** The model registry (and, for data, the dataset registry) refuses ingestion when evidence is missing, mis-signed, or below the tier's minimum.
- **A composition with the substrate.** Every ingestion event is a family-1 lifecycle log entry; every attestation is referenced from the model card (chapter 03); every packet-facing regulator view (chapter 05) is anchored in the supply-chain slice.

## What it is not

- **A classical SBOM.** A CycloneDX or SPDX SBOM without ML-specific extensions describes libraries and packages. Software supply chain hygiene of the AI system's *code* is necessary but not sufficient; the ML-BOM and SPDX AI-profile extensions carry the model / dataset content.
- **A model registry alone.** The registry is the gating point but the slice includes the producers, verifiers, and the substrate events; the registry is one component.
- **An engineering hygiene concern.** The slice is a *governance* concern with engineering delivery. The level-50 architect owns the shape; MLOps and platform own the production; the model-registry policy owns the gate.
- **Only for self-trained models.** For third-party checkpoints (open-weight base models pulled from a hub; frontier-model provider fine-tunes; vendor-supplied specialised models) the slice adapts — provenance attestations from the vendor are ingested and verified rather than produced. Mod-109 walks the third-party governance interface.

## The four evidence formats

### CycloneDX ML-BOM

CycloneDX is a lightweight software bill of materials standard maintained by OWASP. Since version 1.4 CycloneDX has supported machine-learning components — a `component` of type `machine-learning-model` with structured fields for architecture, hyperparameters, datasets, and considerations, and a companion `data` component type for datasets. CycloneDX 1.5 and 1.6 broaden and refine the ML fields; the CycloneDX documentation and the OWASP CycloneDX ML-BOM guide are the authoritative references.

What an ML-BOM captures for a model:

- **Component identity.** Name, version, hash, licence.
- **Model kind.** Base model, fine-tune, prompt-engineered specialisation.
- **Architecture / algorithm.** The architecture family and, where meaningful, hyperparameters.
- **Dataset references.** Each dataset the model was trained or evaluated on, by reference. The referenced datasets are separately captured (as `data` components in the same BOM or in a separate SPDX dataset record).
- **Considerations.** Ethical / fairness / robustness considerations attached to the model.
- **Dependencies.** The classical software components the model requires at inference (framework version, tokenizer, runtime).

For the enterprise slice the ML-BOM is emitted by the training pipeline at build time, signed alongside the model artefact, and stored in the model registry adjacent to the artefact. The registry ingestion policy verifies the ML-BOM exists, is well-formed, is signed by an identity authorised for the build, and passes the enterprise's tier-appropriate content policy (for tier-3 and tier-4 models the ML-BOM must reference datasets whose SPDX records exist).

### SPDX 3.0 with the AI profile

SPDX (Software Package Data Exchange, Linux Foundation) is the ISO/IEC 5962-standardised SBOM format. SPDX 3.0 introduces a profile system; the AI profile adds AI-specific fields to describe an AI-related component, and a Dataset profile adds fields specific to datasets. The AI profile fields include AI-specific licence and provenance metadata, including <!-- needs-research: verify exact SPDX 3.0 AI profile field names — the profile at time of writing captures energy consumption, safety risks, standards compliance, hyperparameters, model architecture, use restrictions, but the exact field names should be checked against the SPDX 3.0 specification --> considerations that map roughly to what a model card would disclose but in structured machine-readable form.

For the enterprise slice SPDX 3.0 with the AI profile is used for two purposes:

- **Datasets.** SPDX has richer, more standardised metadata around dataset provenance and licensing than CycloneDX's `data` type. Enterprises with a licence-management concern (procured datasets, mixed-licence corpora, public-domain / creative-commons blends) tend to prefer SPDX for the dataset layer.
- **Cross-organisation interoperability.** Where the enterprise ingests or emits AI components across organisational boundaries — vendor to enterprise; enterprise to customer; open-source to enterprise — SPDX's stability as an ISO/IEC 5962 standard and its adoption in supply-chain security tooling make it the interoperable choice.

CycloneDX and SPDX are not mutually exclusive; many enterprises emit both. The architectural question is not "which format wins" but "which artefact class is authoritative in which format" — a decision the architect makes and pins.

### SLSA (Supply-chain Levels for Software Artifacts)

SLSA is a supply-chain security framework from the Open Source Security Foundation (OpenSSF). SLSA 1.0 defines a Build track with levels L1–L3:

- **L1 — provenance exists.** The build system emits provenance describing what it built and what inputs it used.
- **L2 — provenance is authenticated.** The provenance is signed by the build platform.
- **L3 — provenance is hardened.** The build platform runs on isolated infrastructure resistant to tampering; the build's inputs cannot be modified by the build steps.

SLSA is agnostic about what "a build" is. Applied to AI, the build is the training / fine-tuning / evaluation pipeline; the provenance describes the inputs (base model checkpoint, datasets, code commit, hyperparameters), the build environment (compute infrastructure, container image, orchestrator), and the outputs (model artefact, evaluation metrics, ML-BOM).

The enterprise tier / SLSA mapping (a common architectural choice; adapt to the enterprise's actual constraints):

- **Tier-1 / tier-2 systems** — SLSA L1 for their training / fine-tuning runs.
- **Tier-3 systems** — SLSA L2 for their training / fine-tuning runs; the build platform is a known named platform whose signing key is controlled.
- **Tier-4 systems** — SLSA L3 for their training / fine-tuning runs; hardened build platform; provenance verified on ingest; no direct-to-registry pathway.

SLSA attestations are typically carried in in-toto attestation envelope format — a signed JSON payload with a predicate describing the build. The enterprise ingests the attestation into the model registry alongside the ML-BOM and the model artefact.

### Sigstore — cosign, Rekor, Fulcio

Sigstore is a project of the Linux Foundation providing a keyless signing infrastructure for software artefacts, adopted broadly across the cloud-native ecosystem. Three components:

- **cosign.** The signing CLI / library. Signs container images, blobs, attestations. Supports keyless flow (identity-bound signature via OIDC) and key-based flow.
- **Fulcio.** A certificate authority that issues short-lived certificates bound to OIDC identities. Enables the keyless flow where the signer's identity (a GitHub Actions workflow, a Google Workspace user, a corporate OIDC identity) is bound to the signature without long-lived key management.
- **Rekor.** A transparency log for signed artefacts. Every Sigstore signature is recorded in Rekor; inclusion proofs are issued; the log is verifiable and tamper-evident.

For the enterprise slice Sigstore is the primary signing surface across all four evidence types:

- The model artefact is signed by the build's identity.
- The ML-BOM is signed by the build's identity.
- The SPDX record is signed by the build's identity (for datasets ingested at build time) or the dataset owner (for standalone dataset publication).
- The SLSA attestation is signed by the build platform's identity.

Enterprises with existing PKI can use cosign in key-based mode with their own CA. Enterprises adopting keyless flow use Fulcio with their own OIDC issuer (the enterprise SSO). Rekor entries either land in the public Rekor instance (for artefacts the enterprise is comfortable publicly logging) or in a private Rekor deployment (for artefacts the enterprise treats as confidential).

## The producer / verifier / gating-actor topology

Every evidence artefact has a producer, one or more verifiers, and a gating actor whose refusal blocks progression. Naming these is the architect's design; the following is a canonical layout the level-50 architect authors and defends.

| Evidence artefact | Primary producer | Second-line verifier | Gating actor |
|---|---|---|---|
| Model artefact | Training pipeline (MLOps) | Model-registry ingestion policy | Model registry |
| ML-BOM | Training pipeline (auto-emitted) | Governance analyst spot-checks; evaluation engineer for tier-3/4 | Model registry ingestion policy |
| SPDX 3.0 dataset record | Data engineering (auto-emitted at dataset promotion) | Governance analyst; third-party governance for external datasets | Dataset registry ingestion policy |
| SLSA build attestation | Build platform (auto-emitted) | Model-registry ingestion policy | Model registry |
| Cosign signatures | Build platform / signer identity | Rekor / verifier tooling | Model registry / dataset registry / packet assembly |
| Model card (chapter 03) | Model owner; evaluation engineer for eval section | Governance analyst; publication gate | Publication gate |
| Risk card (chapter 03) | Risk engineer | Governance analyst; publication gate | Publication gate |

The pattern the architect defends: *producers auto-emit at build / promotion / publication time; verifiers spot-check on cadence and sample on incident; gating actors enforce the ingestion policy in-code and produce a substrate event on every accept or reject*.

## The model registry as the gating point

The model registry is the pivotal control in the AI supply-chain evidence slice — the equivalent of the container registry with admission-controlled policy in modern software supply chain. The registry's ingestion policy is expressed in-code (Rego / Cue / Kyverno / OPA) and is part of the enterprise's policy-as-code enforcement (mod-103).

An ingestion policy for a tier-3 model, sketched:

```rego
package model_registry.ingest

default allow = false

# Basic well-formedness
required_files := {
  "model_artifact",     # the actual weights or hosted-model handle
  "ml_bom.cdx.json",    # CycloneDX 1.5+ ML-BOM
  "provenance.intoto.jsonl",  # SLSA attestation
  "model_card.md",      # chapter 03 model card
}

allow {
  all_required_files_present
  ml_bom_wellformed
  slsa_level_at_least_2  # tier-3 minimum
  signatures_verified
  datasets_have_spdx_records
  model_card_signed_and_published
}

all_required_files_present {
  count(input.uploaded_files - required_files) >= 0
  count(required_files - input.uploaded_files_names) == 0
}

ml_bom_wellformed {
  input.ml_bom.bomFormat == "CycloneDX"
  input.ml_bom.specVersion in {"1.5", "1.6"}
  count(input.ml_bom.components) > 0
  some c in input.ml_bom.components
  c.type == "machine-learning-model"
}

slsa_level_at_least_2 {
  input.slsa.build_level >= 2
  input.slsa.build_platform in trusted_build_platforms
}

signatures_verified {
  # Sigstore verification: signature bound to identity from trusted OIDC issuer;
  # Rekor entry present and log inclusion proof valid.
  input.cosign.identity_issuer == "https://oidc.enterprise.corp"
  input.cosign.identity in trusted_build_identities
  input.rekor.inclusion_proof_valid == true
}

datasets_have_spdx_records {
  every dataset_ref in input.ml_bom.dataset_refs {
    dataset_ref in registered_dataset_ids
  }
}

model_card_signed_and_published {
  input.model_card.publication_gate_signed == true
  input.model_card.tier == input.system.tier
}
```

The policy is one artefact; the composition matters. Registry ingestion produces:

- Accept → substrate event in family 1 (lifecycle); model artefact and evidence bundle promoted; downstream consumers (deployment, pre-deployment gate) allowed to reference.
- Reject → substrate event in family 1 with the policy failures enumerated; incident-response notified if the failure is a signal (e.g., signature verification failure could indicate tampering).

The registry also enforces *deletion / retirement* policy: promoted artefacts move out of active status only through a governance-family log event, and never leave the substrate for the retention horizon (chapter 02).

## The end-to-end pipeline — a schematic

```yaml
ai_supply_chain_evidence_pipeline:
  version: 1.0.0
  stages:
    - id: base-model-pull
      description: Pull a base checkpoint from an external model hub or vendor
      producer: mlops-platform
      artefacts_emitted:
        - hub_checkpoint_hash
        - vendor_attestation (if provided)
      substrate_events: [family-1: base-model-pulled]
      policy_check: >
        base checkpoint hash matches enterprise-approved base-model registry (mod-109);
        vendor attestation, where required, verified

    - id: dataset-promotion
      description: Promote datasets required for training / eval to the dataset registry
      producer: data-engineering
      artefacts_emitted:
        - dataset_artefact
        - spdx_3_ai_record (with dataset profile)
        - cosign_signature
        - rekor_entry
      substrate_events: [family-2: dataset-promoted]
      policy_check: >
        SPDX record well-formed with AI profile; licence acceptable per enterprise
        licence policy; PII posture assessed; signatures verified

    - id: training-or-fine-tuning-run
      description: Execute a training / fine-tuning run
      producer: training-platform (build-platform)
      artefacts_emitted:
        - checkpoint_artefact
        - ml_bom_cdx_json (auto-emitted)
        - slsa_attestation_intoto_jsonl
        - cosign_signature over each artefact
        - rekor_entries
      substrate_events: [family-1: training-run-started, family-1: training-run-completed]
      policy_check: >
        build ran on trusted build-platform; inputs match declared inputs;
        outputs' hashes stable; ML-BOM includes dataset references matching the
        SPDX records emitted at dataset-promotion stage

    - id: evaluation-run
      description: Execute an evaluation run against a versioned eval set
      producer: evaluation-platform
      artefacts_emitted:
        - eval_run_report
        - eval_set_hash_reference
        - cosign_signature
        - rekor_entry
      substrate_events: [family-1: evaluation-run-completed]
      policy_check: >
        eval-set hash matches registered eval-set version; reproducibility invariants
        (fixed seed, versioned code) honoured

    - id: model-card-authoring
      description: Author or update the model card for the produced checkpoint
      producer: model-owner (draft) + evaluation-engineer (eval section)
      reviewer: governance-analyst
      publication_gate: publication-gate (chapter 03)
      substrate_events: [family-3: card-drafted, family-3: card-reviewed, family-3: card-published]
      policy_check: >
        publication gate signed off; substrate bindings intact

    - id: model-registry-ingestion
      description: The gate. No promotion without evidence.
      producer: model-registry
      inputs:
        - checkpoint_artefact
        - ml_bom_cdx_json
        - slsa_attestation
        - cosign_signatures + rekor_entries
        - model_card (published)
        - risk_card (published)
      policy: package model_registry.ingest (see above)
      outputs:
        accept: [family-1: model-registry-ingested] + downstream promotion pathway open
        reject: [family-1: model-registry-rejected] + reason enumerated + incident-response notification

    - id: deployment-promotion
      description: Promote through environments to production
      producer: platform-team
      substrate_events: [family-1: deployed-to-env-*]
      policy_check: >
        pre-deployment gate decision-record present and signed (mod-107 ch02);
        deployment target's tier requirements match model tier

  cross_references:
    audit_log_substrate: chapter 02
    card_family: chapter 03
    pre_deployment_gate: mod-107 chapter 02
    third_party_governance: mod-109
    post_market_monitoring: mod-110
    oscal_catalog: chapter 06 (schemas referenced above land as OSCAL catalog entries)
```

## Third-party ingestion — vendor-supplied models and datasets

Not every model is trained in-house. Frontier-model provider APIs, open-weight checkpoints from public hubs, vendor-supplied fine-tunes on the enterprise's data — each is a supply-chain ingest event with a different attestation profile.

- **Frontier-model provider APIs.** The enterprise does not receive weights; the "artefact" is a service endpoint plus the provider's system card, model card, and any attestations. The registry treats the endpoint as a versioned reference; the provider's attestations are recorded in the family-1 log at first-ingest; the vendor-relationship metadata (mod-109) accompanies.
- **Open-weight checkpoints.** The checkpoint has a public hash and typically a licence file; some hubs are beginning to publish provenance attestations. The enterprise records the hash and licence; where the hub does not provide a signature the enterprise's ingestion pipeline signs the artefact-as-received with a "third-party-received" identity and records the ingest in Rekor. Downstream fine-tuning inherits the base-model reference into its own ML-BOM.
- **Vendor-supplied fine-tunes.** The vendor may deliver a signed attestation with the model artefact. The enterprise's ingest policy verifies the vendor's signature against a registered vendor identity; the SLSA level of the vendor's attestation counts toward the tier check.

Where the ingest cannot produce SLSA L2 or above (a public open-weight checkpoint without any provenance attestation), the model registry can accept the ingest only under an explicit exception recorded in the family-3 governance log with a named executive sponsor — the exception carries the residual risk that the artefact's provenance is undefended, and the risk register entry (mod-106) reflects it.

## The six invariants the slice holds

**Invariant 1 — every deployed model has an ML-BOM in the registry.** No exceptions in the operational population. Failure mode: three tier-2 systems that shipped before the ML-BOM policy landed continue to serve without evidence; a licence issue on a dataset silently affects them because the enterprise cannot answer the "who depends on this" query for those systems.

**Invariant 2 — every promotion is signature-verified.** Signature check runs on every ingestion attempt; verification failure is a policy reject. Failure mode: a tampered checkpoint is promoted because the signature check was disabled during a platform migration; the incident is discovered months later.

**Invariant 3 — the SLSA level per tier is enforced by the ingestion policy.** Tier-4 requires L3; tier-3 requires at least L2; the policy expressed in code is the enforcement. Failure mode: the policy is a documented expectation but not enforced; tier-4 systems ship with SLSA L1 evidence and no one notices until the certification body samples.

**Invariant 4 — datasets referenced in an ML-BOM exist in the dataset registry with SPDX records.** No orphan dataset references. Failure mode: the ML-BOM references a dataset by name but no SPDX record exists; when the licence question arises the enterprise cannot bound the exposure.

**Invariant 5 — every third-party ingest has a recorded provenance disposition.** Where the vendor's evidence is complete, the disposition is "vendor-verified"; where evidence is incomplete, the disposition is a recorded exception with named executive sponsor. Failure mode: open-weight checkpoints are pulled from a hub without any disposition; the enterprise's exposure to the hub's tampering or licence-change events is undocumented.

**Invariant 6 — registry decisions are substrate events, and the substrate is queryable.** Accept and reject decisions both produce family-1 log entries with reason codes; the OSCAL catalog's queries can enumerate rejects per week. Failure mode: rejects are silent; the platform team quietly reworks and re-submits; the pattern of failing evidence never surfaces to the second line.

## Two failure modes to design against

**Failure mode 1 — SBOM but not ML-BOM.** The enterprise's supply-chain security programme has been rolled out for classical software; every container image has a CycloneDX SBOM at build. The AI systems inherit the CycloneDX SBOM for their runtime dependencies (framework, libraries) and stop there. The model itself, the datasets, the fine-tuning, the eval — none of it is captured in a machine-readable evidence artefact. When the licence-issue question fires the enterprise's SBOM tooling returns nothing about the models because the models were never in the SBOM. The fix is architectural: the ML-BOM is a required artefact class alongside the SBOM at every ingest; the enterprise's supply-chain security tooling ingests both.

**Failure mode 2 — the registry that accepts anything with a well-formed manifest.** The registry exists, the policy exists, the policy checks the manifest is well-formed and the file is signed. But the policy does not check the *content* — the ML-BOM's datasets could be fictional and the policy would not know; the SLSA attestation could name an untrusted build platform and the policy would not reject; the model card's tier could be one tier lower than the risk card's tier and the discrepancy would not surface. The registry becomes a shape check masquerading as a content check. The fix is architectural: the policy cross-verifies against other registries (referenced datasets exist in the dataset registry; build platform identity is in a trusted-build-platform allowlist; tier consistency across model card and risk card is enforced); the policy is versioned and its version is a substrate log entry; policy changes require the second-line's sign-off.

## Summary

The AI supply-chain evidence slice — CycloneDX ML-BOM, SPDX 3.0 AI profile, SLSA build attestations, Sigstore signing — composes with the audit-log substrate (chapter 02) and the card family (chapter 03) to make the enterprise's AI programme's supply chain queryable, provenance-attested, and gate-enforced. Every model, every dataset, every training run has a machine-readable inventory and a signed attestation. The model registry (and dataset registry) is the gating point where evidence is checked in-code before promotion; policy is expressed in a policy-as-code language and versioned. Producers, verifiers, and gating actors are named; third-party ingest is a distinct sub-flow with vendor attestations verified against registered vendor identities. Six invariants and two failure modes shape the design. The next chapter designs the regulator-facing packaging templates that consume this slice's evidence, along with the card family and the substrate.
