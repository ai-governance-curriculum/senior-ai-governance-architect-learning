# The AI supply-chain evidence contract — CycloneDX ML-BOM, SPDX 3.0 AI profile, SLSA, Sigstore, and the ingestion gate

## Why this chapter exists

Every model checkpoint, dataset artefact, fine-tune, adapter, evaluation set, or vendor-shipped component that enters the enterprise's substrate is a *third-party artefact* until it is ingested through a discipline that proves what it is, where it came from, who signed for it, and what constraints its licence imposes. Without that discipline the enterprise's model registry becomes a shared drive of files a data scientist pulled from Hugging Face last quarter — the checkpoint hash is unrecorded, the licence terms are the vendor's marketing page, the provenance chain is a link in a Slack thread, and the artefact is now behind a customer-facing production endpoint.

The evidence architecture in mod-108 chapter 04 already fixes the *shape* of the AI supply-chain evidence layer for the enterprise's own artefacts — CycloneDX ML-BOM, SPDX 3.0 AI profile, SLSA levels, Sigstore signatures, in-toto attestations. This chapter's contribution is the *contract with the third party* — what the vendor must produce, what the enterprise ingests, what the ingestion gate refuses, and how the runtime enforcement is split between the level-50 architect (who owns the evidence contract) and the level-35 AI infrastructure security peer (who owns the runtime supply-chain / signing platform / registry admission controller / KMS).

The chapter is short because the substrate work lives in mod-108. What this chapter adds is the *third-party evidence contract*: which artefacts the enterprise ingests, what evidence each artefact must arrive with, how the tier-scaled minimum evidence bundle attaches, how the registry gate operates against the bundle, how the coordination with the level-35 peer is declared, and how the ingest event lands in the substrate. Exercise-05 (stretch) walks the drill.

## What the supply-chain evidence contract is, structurally

The contract is:

- **A named artefact class taxonomy.** The classes of third-party artefacts the enterprise ingests: base-model checkpoints (closed-weight API access, open-weight download), vendor-supplied fine-tunes, vendor-supplied adapters (LoRA, PEFT), vendor-supplied evaluation sets, vendor-supplied datasets, vendor-supplied guardrail models / classifiers, vendor-supplied prompt libraries or agent templates.
- **A minimum-evidence bundle per class × tier.** For each artefact class, at each vendor tier, the evidence the artefact must arrive with — the specific SLSA level, the specific BOM format, the specific signature discipline, the specific licence disclosure, the specific data-provenance disclosure.
- **A registry ingestion gate.** The model registry, dataset registry, and evaluation-set registry each enforce the bundle at ingestion — refuse promotion of artefacts whose bundle is incomplete; permit ingestion into a quarantine tier for artefacts under active diligence.
- **A coordination contract with the level-35 peer.** The level-35 AI infrastructure security peer owns the runtime signing platform, the registry admission controller, the KMS, the transparency-log integration, the artefact scanner. The level-50 architect owns the *evidence contract shape* the level-35 platform enforces. The interface between the two is declared.
- **A substrate binding.** Every ingest emits an event in the mod-108 substrate; every gate decision is a substrate event; every promotion is a substrate event; every rejection is a substrate event.
- **A versioned catalog.** The evidence contract is a versioned artefact of chapter 04's control catalog; contract changes are second-line-reviewed; contracts executed against a specific version are traceable.

## What it is not

- **The runtime supply-chain platform.** The signing platform (Sigstore or in-house cosign infrastructure), the registry admission controller (Kyverno, OPA, custom), the KMS, the artefact scanner, the transparency-log integration — the platform layer is the level-35 peer's craft. The architect specifies the evidence contract the platform enforces; the platform's build is not this chapter.
- **A substitute for chapter 04's contract-template controls.** The enterprise's contract with the vendor (chapter 04) creates the *obligation* on the vendor to deliver the bundle. This chapter's contract is the *enterprise-internal ingestion discipline* the enterprise runs against the delivered bundle. Both are required.
- **A substitute for the DDQ.** The DDQ (chapter 03) captures the vendor's *self-representation* about supply-chain practice at contract time. This chapter's contract captures the *artefact-by-artefact evidence* at ingest time. The DDQ tells the enterprise the vendor claims to sign artefacts; the ingest gate verifies that this specific artefact has a valid signature.
- **Applicable to closed-weight API-only engagements at the artefact level.** For a foundation-model provider whose model is accessed only through an API — no checkpoint download, no fine-tune weights delivered — the artefact-level evidence contract does not directly apply. The enterprise instead relies on the vendor's DDQ representations, the vendor's own supply-chain evidence (SOC 2 sub-service posture, the vendor's own SLSA / Sigstore posture as evidence), and the chapter 04 contract clauses. The evidence contract activates when the enterprise ingests something.

## The artefact-class taxonomy

Eight artefact classes cover most of the third-party artefact population the enterprise ingests. Each class has a characteristic evidence expectation.

### Class 1 — Open-weight base-model checkpoints

*Artefacts:* base-model weight files (safetensors, GGUF, PyTorch state dict, JAX weights, Hugging Face Transformers repository) distributed under an open-weights licence (Apache-2.0, MIT, Llama Community License, Mistral licence, Gemma licence, Qwen licence, various OpenRAIL variants). Sources: Hugging Face hub, model publisher's official distribution channel, model publisher's mirror.

Characteristic evidence expectation:

- Checkpoint SHA-256 (or comparable) hash of the exact file(s) ingested.
- Licence text, licence identifier (SPDX identifier where applicable), and any use-restriction annexes.
- Publisher-signed attestation of the checkpoint where offered (Hugging Face's model provenance features, publisher-run Sigstore attestations, publisher's own signature).
- Publisher-supplied model card content — training-data disclosure to the extent published, evaluation results, safety-tuning approach, intended-use disclosure.
- CycloneDX ML-BOM referencing the model component or SPDX 3.0 AI-profile document if the publisher provides one; the enterprise generates its own if the publisher does not, capturing what is knowable.
- SLSA provenance of the *distribution chain* to the extent the publisher supports (currently limited across open-weight distribution; the discipline is aspirational for many publishers). <!-- needs-research: verify the current state of SLSA / Sigstore adoption by major open-weight model publishers at reading time; adoption is moving quickly across the Hugging Face + independent-publisher ecosystem. -->

### Class 2 — Vendor-supplied fine-tunes (delivered as weights)

*Artefacts:* vendor-produced fine-tune weights, adapter files (LoRA / PEFT), quantised variants, distillation targets. Delivered from a vendor engagement where the vendor fine-tuned on the enterprise's corpus, or from a vendor's own fine-tune of a base model for a specialised use case.

Characteristic evidence expectation:

- Base-model identity and hash the fine-tune derives from (linking to class 1's evidence).
- Fine-tune training-data manifest (referencing the enterprise-supplied corpus by hash and version if enterprise-derived; the vendor's corpus if vendor-derived).
- Training-run provenance (SLSA provenance of the training pipeline — training platform, code version, hyperparameters, compute environment, entry point).
- Evaluation-run provenance (SLSA provenance of the pre-delivery evaluation, referencing the evaluation set(s) used).
- Publisher signature (vendor's cosign signature, ideally keyless bound to a verifiable identity).
- Licence and IP terms specific to the fine-tune (enterprise's rights in the fine-tune weights, vendor's rights, third-party rights arising from the base-model licence).

### Class 3 — Vendor-supplied datasets

*Artefacts:* vendor-supplied training datasets, evaluation datasets, red-team prompt corpora, licensed-content datasets (news, images, code), synthetic-data corpora. Delivered from dataset providers (Scale AI, Surge AI, Toloka, Appen, licensed-content aggregators, academic distributors).

Characteristic evidence expectation:

- Dataset SPDX 3.0 document with the AI / dataset profile, or CycloneDX BOM with dataset components; capturing composition, licence, source attribution, PII handling posture, sensitive-attribute handling posture.
- Licence terms with SPDX identifier where applicable; any redistribution constraints; permitted downstream uses (commercial training, evaluation, redistribution of derived models).
- Provenance of the labels (human-labelled, model-labelled, hybrid) and, for labelled datasets, the vendor's labour-practices posture.
- Contamination-check methodology for evaluation datasets (whether the evaluation set overlaps with common training corpora).
- Sensitive-attribute assessment.
- Publisher / distributor signature and integrity hash for the dataset artefact.

### Class 4 — Vendor-supplied guardrail models / classifiers

*Artefacts:* content-classification models, PII detectors, jailbreak detectors, prompt-injection detectors, content-safety classifiers. Delivered as weights (for self-hosted deployment) or as API access (for hosted guardrails).

Characteristic evidence expectation:

- For weight-delivered: class 2 evidence expectation applies (base-model identity, training-data manifest, training-run provenance, evaluation provenance, signature, licence).
- Additionally: classifier evaluation methodology and disclosed performance on the classes the enterprise depends on; false-positive / false-negative disclosures; taxonomy the classifier operates against.
- For API-only: chapter 03 DDQ evidence expectation covers most of the surface; the artefact-level contract does not directly apply.

### Class 5 — Vendor-supplied evaluation sets

*Artefacts:* benchmark evaluation sets, red-team prompt libraries, safety-evaluation corpora, factuality evaluation sets.

Characteristic evidence expectation:

- Class 3 evidence expectation applies.
- Additionally: methodology disclosure (how the set was constructed, whether it was subject to contamination-avoidance discipline, how the ground-truth was established, known limitations).
- Update / version history (an evaluation set is a versioned artefact; the enterprise's evaluation baselines depend on a specific version).

### Class 6 — Vendor-supplied prompt libraries, agent templates, tool definitions

*Artefacts:* prompt libraries, agent-scaffold templates, tool-use definitions, retrieval templates, function-calling schemas. Increasingly a distinct artefact class as the agent-development market matures.

Characteristic evidence expectation:

- Version and integrity hash.
- Licence.
- Composition disclosure — what the template composes with (specific base models, specific tools, specific retrieval infrastructure).
- Evaluation posture — has the template been evaluated? on what tasks? with what results?
- Security disclosure — has the template been evaluated for prompt-injection resistance? tool-use safety?

### Class 7 — Vendor-supplied training / fine-tuning platform runtimes

*Artefacts:* container images, orchestration templates, Kubeflow pipelines, Ray applications, or comparable runtime artefacts a vendor ships for the enterprise to run on its own infrastructure.

Characteristic evidence expectation:

- Container image SLSA provenance at the level chapter's tier-scaled minimum (typically SLSA Build L2 or L3 for tier-3+).
- CycloneDX BOM for the container's OS packages, language packages, and any bundled ML libraries.
- CVE-scan results at build time; ongoing scan cadence commitment.
- Signature (cosign) with keyless-signing or a well-provisioned key.
- Composition with the mod-108 chapter 04 supply-chain evidence for the *enterprise's own* container artefacts (the vendor's image is one input; the enterprise's derived images are another).

### Class 8 — Vendor-supplied GRC-for-AI or observability platform components

*Artefacts:* on-prem or hybrid deployments of GRC-for-AI or AI-observability platforms — vendor-supplied Helm charts, agent binaries, sidecar images. Increasingly common as vendors ship agent-based observability that runs alongside the enterprise's inference.

Characteristic evidence expectation:

- Class 7 evidence expectation applies for any deployed image.
- Additionally: data-flow disclosure (what the agent collects and where it sends it), retention / deletion posture, sub-service organisation status for SOC 2 purposes.

## The tier-scaled minimum-evidence bundle

Each artefact class × vendor-tier combination has a *minimum evidence bundle* the ingestion gate enforces. The bundle scales with tier: a tier-1 vendor's dataset artefact ingests with a lighter bundle than a tier-4 vendor's checkpoint. The catalog:

```yaml
minimum_evidence_bundle:
  version: 1.0.0

  # SLSA levels used below reference the SLSA v1.0 Build track:
  # L0 = no provenance; L1 = provenance exists; L2 = signed provenance from
  # hosted build platform; L3 = hardened, non-forgeable provenance from
  # hardened build platform. See resources.md for the SLSA spec.

  class_1_open_weight_checkpoint:
    tier_2:
      hash: sha256 required
      licence: SPDX-id or full licence text required
      publisher_signature: strongly preferred; if absent, second-line reviewer justification
      cyclonedx_ml_bom_or_spdx_ai: enterprise-generated at ingest if not publisher-provided
      slsa_provenance: opportunistic (accept if publisher provides)
    tier_3:
      hash: sha256 required
      licence: SPDX-id or full licence text required
      publisher_signature: required unless documented exception
      cyclonedx_ml_bom_or_spdx_ai: required
      slsa_provenance: required at level L1 minimum (attestation exists); L2 preferred
      publisher_model_card: required
      training_data_disclosure_level: required at category level
    tier_4:
      hash: sha256 required
      licence: SPDX-id or full licence text required
      publisher_signature: required; keyless-signing preferred; substrate-recorded verification against transparency log
      cyclonedx_ml_bom_or_spdx_ai: required
      slsa_provenance: required at level L2 minimum; L3 preferred
      publisher_model_card: required
      publisher_system_card_if_frontier: required
      training_data_disclosure_level: required at category level; specific-source level where publisher discloses
      independent_third_party_safety_evaluation: required (report or letter)

  class_2_vendor_fine_tune:
    tier_2:
      base_model_identity_and_hash: required
      fine_tune_training_data_manifest: required (references + hashes)
      slsa_provenance_of_training_run: L1 minimum
      vendor_signature: required
      licence_and_ip_terms: required
    tier_3:
      as tier_2 plus:
      slsa_provenance_of_training_run: L2 minimum
      slsa_provenance_of_evaluation_run: L1 minimum
      vendor_signature: keyless-signing preferred; substrate-verified against transparency log
      training_data_manifest_with_pii_scan_results: required
    tier_4:
      as tier_3 plus:
      slsa_provenance_of_training_run: L3 preferred; L2 minimum
      slsa_provenance_of_evaluation_run: L2 minimum
      training_compute_environment_disclosure: required
      independent_third_party_pre_delivery_evaluation: required (report or letter)

  class_3_vendor_dataset:
    tier_2:
      spdx_ai_or_cyclonedx_ml_bom: required
      licence_terms: SPDX identifier or full text; downstream-use permissions explicit
      integrity_hash: required
      pii_and_sensitive_attribute_assessment_summary: required
    tier_3:
      as tier_2 plus:
      label_provenance_disclosure: required
      contamination_check_for_eval_sets: required
      labour_practices_attestation_for_labelled_datasets: required
      publisher_signature: required
    tier_4:
      as tier_3 plus:
      independent_third_party_dataset_audit_where_available: required
      dataset_datasheet_per_gebru_shape: required
      re_signing_cadence_for_evolving_datasets: contract-defined

  class_4_vendor_guardrail (weight-delivered):
    (per class_2 + additional classifier evaluation methodology fields)

  class_5_vendor_evaluation_set:
    (per class_3 + methodology and version-history disclosure)

  class_6_prompt_library_or_agent_template:
    tier_2:
      integrity_hash: required
      licence: required
      composition_disclosure: required
    tier_3:
      as tier_2 plus:
      evaluation_posture_disclosure: required
      prompt_injection_evaluation_where_applicable: required
    tier_4:
      as tier_3 plus:
      publisher_signature: required
      substrate_recorded_verification: required

  class_7_vendor_runtime_container:
    tier_2:
      slsa_provenance: L1 minimum
      cyclonedx_bom_or_spdx: required
      cve_scan_at_build: required
      cosign_signature: required
    tier_3:
      slsa_provenance: L2 minimum
      cyclonedx_bom_or_spdx: required
      cve_scan_at_build_and_ongoing_scan_commitment: required
      cosign_signature: keyless-signing preferred
    tier_4:
      slsa_provenance: L3 preferred; L2 minimum
      cyclonedx_bom_or_spdx: required
      cve_scan_ongoing_commitment_with_defined_cadence: required
      cosign_signature: keyless-signing required with substrate-verified transparency-log entry

  class_8_vendor_grc_or_observability_agent:
    (per class_7 + data-flow disclosure + retention posture + sub-service status)

  exceptions_policy:
    - a missing element in the bundle at ingest requires a documented exception with:
      - named authoriser (head-of-ai-governance for tier-3; ai-accountable-executive for tier-4)
      - defined remediation path (vendor commits to deliver by date X)
      - substrate log entry
      - risk register entry
    - artefact ingests into a quarantine tier pending remediation; is not promotable until remediated
```

The bundle is the *shape*, not the specific choice of formats. An enterprise may pin CycloneDX or SPDX as the sole SBOM format; both are valid. An enterprise may pin Sigstore keyless or a private cosign key; both are defensible. What matters is that the choice is *pinned*, *versioned*, *enforced*, and *auditable*.

## The registry ingestion gate

The gate is the point at which the bundle is checked. Every third-party artefact that enters the enterprise's model registry, dataset registry, or evaluation-set registry passes through the gate. The gate:

```yaml
ingestion_gate:
  version: 1.0.0

  location: model registry / dataset registry / evaluation-set registry ingestion pipelines
  enforcement_layer: admission controller / registry policy engine (Kyverno, OPA-Rego, custom)
  ownership:
    evidence_contract: senior-ai-governance-architect (level 50)
    platform_implementation: ai-infrastructure-security (level 35 peer)

  gate_actions:
    - identify: which artefact class does this artefact belong to?
    - identify: which vendor (register lookup) and what tier?
    - resolve: which minimum-evidence bundle applies?
    - verify:
        - hash matches declared hash
        - licence text or SPDX identifier present and readable
        - publisher signature verifies (against transparency log where keyless)
        - SLSA provenance verifies against the declared build platform's public key or transparency log
        - BOM parses and passes schema validation
        - CVE scan produces results below defined severity thresholds (tier-scaled)
    - decide:
        - all required elements present and verified: ADMIT to registry, tag as promotable
        - required elements missing but declared exception: ADMIT to quarantine tier, tag not promotable
        - required elements missing and no exception: REJECT
    - emit substrate events for every gate decision

  substrate_events_emitted:
    - artefact-ingest-attempted (subject: artefact URI + hash; actor: producing service identity)
    - artefact-evidence-verified (fields per verification step)
    - artefact-admitted OR artefact-rejected (with disposition rationale)
    - artefact-promoted (subsequent event when a quarantine artefact transitions to promotable)

  operating_rhythm_queries:
    - all quarantined artefacts older than N days
    - all rejected ingest attempts by vendor in the last 90 days
    - all artefacts by tier + class with missing recommended-not-required elements

  break_glass:
    - a same-day promotion of a tier-4 artefact with an unmet minimum evidence element requires:
        - written authorisation from ai-accountable-executive
        - risk register entry with compensating-control commitment
        - substrate log entry of the break-glass event
        - post-hoc remediation obligation with deadline
    - break-glass without the substrate log entry is a bypass, not a break-glass
```

The gate is what turns "the vendor is contractually obligated to deliver the bundle" (chapter 04) into "the substrate can prove the specific artefact serving customer traffic today had a verified bundle at the time it was promoted."

## Coordination with the level-35 AI infrastructure security peer

The evidence-contract-and-gate ownership splits between the level-50 architect and the level-35 AI infrastructure security peer. The split is declared:

```yaml
level_50_architect_owns:
  - the evidence-contract shape (this chapter's catalog)
  - the minimum-evidence bundle per class × tier
  - the gate's decision rules (what admits, what quarantines, what rejects)
  - the substrate event contract for gate decisions
  - the exception / break-glass policy
  - the composition with chapter 04 (contract obligations) and chapter 03 (DDQ representations)
  - the composition with mod-108 chapter 04 (substrate storage of ingested evidence)
  - the versioning discipline for the catalog

level_35_peer_owns:
  - the signing platform (Sigstore infrastructure, cosign toolchain, or in-house equivalent)
  - the transparency-log integration (Rekor operation or equivalent)
  - the KMS supporting signature verification (HSM / cloud-KMS / TPM integration)
  - the registry admission controller (Kyverno / OPA / custom) implementation
  - the artefact scanner (CVE scan, secret scan, malware scan, model-artefact scan for embedded backdoors)
  - the container-runtime, model-serving-runtime, and network policies enforcing runtime supply-chain
  - the SLSA provenance verification implementation
  - the key-management posture for the enterprise's own signing keys
  - the observability of the gate (metrics, dashboards, alerts for degradation)

shared_ownership_zone:
  - the interface between the evidence contract (what to check) and the gate implementation (how to check it)
  - the substrate event contract (what events the gate emits, how they land in mod-108's substrate)
  - the break-glass procedure (authorisation is the architect's; execution is the platform peer's)
  - the joint runbook for artefact rejection escalation
  - the joint runbook for vendor evidence-format changes

coordination_cadence:
  - quarterly joint review of gate behaviour (rejections, quarantines, exceptions)
  - annual joint review of the catalog against vendor market practice
  - ad-hoc coordination on any material change to signing infrastructure, transparency log, or admission-controller platform
```

This split is invariant 8 from chapter 01 (composition with the level-35 peer) at the specific-artefact layer.

## The DDQ-to-contract-to-gate closure loop

The three chapters compose:

- **Chapter 03 DDQ front-matter and category-5 (version / change) questions** capture the vendor's *representations* about supply-chain practice — the vendor claims to sign artefacts, publish SLSA provenance, deliver BOMs, disclose licences.
- **Chapter 04 EV-05 (supply-chain evidence)** binds those representations as a *contract obligation* — the vendor must deliver the bundle per the tier's requirements at each artefact delivery.
- **Chapter 07 ingestion gate (this chapter)** verifies the *specific artefact* at ingest — the bundle arrived, verifies, and satisfies the tier's minimum.

Failures at each layer have different remedies:

- DDQ misrepresentation at contract time → contract remedy (representation-and-warranty breach).
- Contract obligation breach (vendor stops delivering bundles) → chapter 04 material-breach and cure path.
- Ingest gate rejection → operational escalation, vendor engagement, quarantine.

## The mod-108 substrate composition

Every ingest event lands in the mod-108 substrate as a lifecycle-family event (per mod-108 chapter 02's event schema). The substrate retention obligations attach — a tier-4 artefact ingest event under EU AI Act scope retains for the horizon EU AI Act imposes on the underlying evidence class. Chapter 04's EX-05 (extended evidence retention post-termination) composes: even after a vendor engagement ends, the substrate carries the ingest evidence for the retention horizon of the systems that used the artefact.

The composition is what makes the evidence *durably enforceable* — an EU AI Act market-surveillance authority in year 4 can query the substrate for the specific ingest event of a specific artefact version, retrieve the ML-BOM, retrieve the SLSA attestation, verify the signature against the transparency log's historical record, and reconstruct provenance without a live vendor engagement.

## The six invariants the evidence contract holds

**Invariant 1 — every third-party artefact ingests through the gate.** No back-door path admits an artefact without evidence verification. Failure mode: a data scientist pulls a checkpoint into a training environment directly; the checkpoint is later moved to the registry as a "finished" artefact; the registry admission controller's ingest event was for the wrong artefact class; the actual checkpoint's evidence was never checked.

**Invariant 2 — the tier-scaled bundle is enforced, not aspirational.** The minimum bundle at the artefact's applicable tier is what the gate demands; artefacts below the minimum quarantine or reject. Failure mode: the catalog exists; the gate is misconfigured to log-warn rather than deny; artefacts admit with missing evidence; the reviewer sweeps periodically and finds gaps; too late to remediate at scale.

**Invariant 3 — exceptions are authorised, expiring, and substrate-recorded.** Missing evidence at ingest time is admissible only with a documented exception; every exception has an expiry and a remediation obligation. Failure mode: exceptions accumulate without expiry; the quarantine tier fills with artefacts nominally awaiting remediation that in fact serve production traffic; the register cannot query for the accumulation.

**Invariant 4 — signatures verify against a transparency log, not just against a key.** Publisher signatures verify against public transparency-log entries (Sigstore Rekor or the vendor's private transparency log) so that a signature's provenance and issuance time are independently reconstructable. Failure mode: the enterprise verifies against a key file the platform team maintains; the key rotates; the substrate cannot prove *when* an artefact was signed; a subsequent claim of key compromise breaks the enterprise's ability to reason about which artefacts pre-date the compromise.

**Invariant 5 — the substrate carries the evidence, not just a reference to the vendor's page.** The BOM, the attestation, the signature, the licence text, and the verification result are stored in the enterprise substrate at ingest time; the vendor's page is not the substrate. Failure mode: the substrate carries "verified against https://vendor.example.com/attestations/..."; the vendor rotates the URL; the substrate's evidence is a dead link.

**Invariant 6 — the coordination with the level-35 peer is declared and rehearsed.** The interface is not a folk agreement between individuals; it is an artefact both roles reference. Failure mode: the architect updates the minimum-evidence bundle; the level-35 peer's admission controller is not updated to match; artefacts admit against the old rule for six months.

## Two failure modes to design against

**Failure mode 1 — the ingest gate that logs warnings instead of denying.** The enterprise stands up the gate. The gate initially runs in a report-only mode so the platform team can measure blast radius. Report-only continues past the intended date because the sponsoring team's promotional pipeline surfaces false-positives from checkpoints the sponsoring team wants promoted immediately. The gate stays report-only; the substrate accumulates warnings; nothing gates. Six quarters later a certification body samples the registry and finds a large population of admitted-but-unverified artefacts. The fix is architectural: the gate ships in enforcing mode from day one, with an explicit break-glass procedure for legitimate urgent cases; report-only mode exists only for evidence-contract updates and only for a defined migration window with a defined date to re-enforce.

**Failure mode 2 — the split-ownership seam that silently drifts.** The architect updates the minimum-evidence bundle to require SLSA L2 for tier-3 class-1 artefacts as of Q3. The level-35 peer's admission-controller policy still enforces the previous L1 requirement. Artefacts admit at L1 through Q3, Q4, and into the next year; the substrate carries evidence that the bundle was tightened but the gate never enforced the tightening. The failure surfaces at internal audit or at a regulator engagement where the discrepancy between policy and enforcement becomes the finding. The fix is architectural: catalog updates are executable — the catalog itself is a machine-readable artefact the admission controller consumes; both the level-50 architect's ratification and the level-35 peer's platform update land in the same versioned release; the operating rhythm queries for policy-enforcement divergence.

## Summary

The AI supply-chain evidence contract is what turns chapter 04's "vendor commits to deliver the bundle" into a specific-artefact discipline the enterprise's model, dataset, and evaluation-set registries enforce at ingest. Eight artefact classes cover the third-party artefact population; each class × vendor-tier combination has a minimum evidence bundle (hash, licence, publisher signature, CycloneDX ML-BOM or SPDX 3.0 AI profile, SLSA provenance at a tier-scaled level, class-specific fields). The ingestion gate verifies the bundle and emits substrate events for admission, quarantine, or rejection. Ownership splits: the level-50 architect owns the evidence-contract shape, the minimum-evidence bundle catalog, the gate's decision rules, the exception policy, and the substrate binding; the level-35 AI infrastructure security peer owns the signing platform, transparency-log integration, KMS, admission-controller implementation, artefact scanner, and runtime supply-chain enforcement. The composition with chapter 03 (DDQ representations), chapter 04 (contract obligations), and mod-108 chapter 04 (substrate storage of evidence) closes the loop. Six invariants (all ingest via gate, tier-scaled bundle enforced, exceptions authorised-and-expiring, signatures verify via transparency log, substrate carries the evidence, coordination declared) and two failure modes (report-only forever, split-ownership drift) shape the discipline. The stretch stream in exercise-05 walks the drill. The module closes here; the level-50 architect has designed the third-party AI governance programme end to end.
