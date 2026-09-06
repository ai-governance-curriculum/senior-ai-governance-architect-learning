# exercise-03: ML-BOM + SPDX + SLSA + Sigstore Enterprise Slice

**Estimated effort:** 3 hours

## Objective

Draw the **AI supply-chain evidence slice** for one of the three canonical scenarios — the topology that names where each of the four evidence formats (CycloneDX ML-BOM, SPDX 3.0 with the AI profile, SLSA attestations, Sigstore signatures) lives, who produces it at each pipeline stage, who verifies it, and how the model registry gates ingestion. The deliverable is the enterprise slice as a design, not an implementation — the learner is drawing the shape of producers, verifiers, gating actors, and the ingestion policy that composes them; the learner is *not* installing cosign or writing production Rego.

Downstream artefacts in this module (exercise-04 regulator-facing packet templates, the chapter-06 OSCAL slice, the chapter-07 coordination contracts) all cite the supply-chain slice you draw here. Get the topology right and the packet templates in exercise-04 have real evidence to cite; get it wrong and the packet becomes a narrative pointing at nothing.

## Prerequisites

- Chapter [`04-ai-supply-chain-evidence-cyclonedx-spdx-slsa-sigstore.md`](../04-ai-supply-chain-evidence-cyclonedx-spdx-slsa-sigstore.md) read, with the four evidence formats, the producer / verifier / gating-actor topology, the model-registry-as-gate framing, and the six invariants and two failure modes noted.
- Exercises [`exercise-01`](exercise-01-audit-log-architecture-drill.md) (audit-log substrate) and [`exercise-02`](exercise-02-model-and-system-and-dataset-card-schema-authoring.md) (card family) complete or their draft outputs available — the substrate carries the ingestion events this exercise gates on, and the cards cite the BOM by digest.
- Mod-102 control-library composition — the ingestion-policy gate is one control in the enterprise's control library; the SLSA-level-per-tier mapping is a tier-derived control obligation.
- Mod-103 policy-as-code — the model-registry ingestion policy composes with the enterprise's policy-as-code enforcement fabric (OPA / Rego, Cedar, Kyverno). This exercise's policy is expressed as a sketch in the same policy language the enterprise uses in mod-103.
- Mod-109 preview (third-party supply-chain governance) — the slice you design here is the technical evidence layer that mod-109's third-party-vendor governance rides on top of. Vendor attestations land through the ingestion pipeline defined here.
- Primary sources —
  - CycloneDX 1.6 specification, with the ML-BOM extension at [cyclonedx.org/capabilities/mlbom/](https://cyclonedx.org/capabilities/mlbom/).
  - SPDX 3.0 specification with the AI profile and Dataset profile.
  - OpenSSF SLSA framework at [slsa.dev](https://slsa.dev/) — Build track levels L1–L4 (verify against the current published levels; the framework has been evolving).
  - Sigstore project at [sigstore.dev](https://www.sigstore.dev/) — cosign, Fulcio, Rekor.
  - in-toto attestation specification at [in-toto.io](https://in-toto.io/).
  - NIST SP 800-218 Secure Software Development Framework (SSDF).
  - NIST SP 800-190 Application Container Security Guide.
  - EU Cyber Resilience Act SBOM obligations (relevant to enterprises with product-facing EU exposure). See [`../resources.md`](../resources.md).

## Scenario

Choose the scenario whose supply-chain topology you are least familiar with; that is where the exercise will teach you most. State your choice at the top of the deliverable.

- **Scenario A — US regional bank.** The Northbrook-Financial-style bank from prior modules. In-scope for this exercise: (i) a fraud-classification production line where the bank pulls a Llama-family checkpoint from a public model hub, fine-tunes it on internal transaction data on the bank's own training platform, and packages the fine-tune as a private serving image deployed into the fraud line; (ii) a customer-facing generative chat that is a vendor foundation model consumed via API — the bank does not receive weights, only endpoint access, system-card and vendor attestations; (iii) an internal RAG legal assistant that combines a smaller open-weight model with a retrieval index built from the bank's own policy corpus.
- **Scenario B — global healthcare payer / provider.** Vendor foundation model consumed via API for coding-automation and utilisation-management assistance; internal RAG over de-identified clinical notes for a clinical-decision-support pilot; a device-facing AI in the FDA SaMD pathway with an on-device inference component built by an internal medical-devices team on a hardened build platform.
- **Scenario C — B2B SaaS vendor shipping GenAI HR-tech.** Two vendor foundation models (one primary, one fallback) integrated via API into the customer-facing product plus one internal fine-tune (adapter over an open-weight base) that generates HR-tech-specific outputs. Customers span US, EU, UK, and Singapore; some customer contracts require the vendor to disclose the supply-chain provenance of the AI capability at customer request.

## Deliverables

Author five files in a working directory of your choice.

1. **`supply-chain-topology.md`** — the decision document. Prose narrative naming the four evidence classes, the producer for each, the registry as gating point, the SLSA-level-per-tier mapping, and the trust boundary per vendor foundation model.
2. **`producer-verifier-gating-map.yaml`** — machine-readable per-artefact-class × per-pipeline-stage table naming the producer, the verifier, the gating actor, and the substrate-log-event class the row emits into.
3. **`model-registry-ingestion-policy.yaml`** — the ingestion-gate policy that enforces the presence of each evidence class before an artefact can be promoted. Expressible as an OPA/Rego sketch or a Cedar sketch (the same policy language the enterprise uses in mod-103); do not write production Rego — sketch the gates so a policy engineer can implement.
4. **`example-ml-bom.cdx.json`** — one worked CycloneDX ML-BOM for a specific system in your scenario (the fraud fine-tune in Scenario A; the internal RAG in Scenario B; the internal HR-tech fine-tune in Scenario C). Headline fields only, not full enumeration.
5. **`example-slsa-attestation.intoto.jsonl`** — one worked in-toto envelope carrying a SLSA provenance predicate for a training run producing the same artefact your ML-BOM describes.

## Requirements

### `supply-chain-topology.md`

Decide and justify **each** of the following:

- **The four evidence classes covered.** For each of CycloneDX ML-BOM, SPDX 3.0 AI record, SLSA attestation, and Sigstore signature, state (i) what the class captures for this enterprise, (ii) which artefact classes carry it (model, dataset, evaluation run, container image), (iii) the pipeline stage that produces it and the producer seat, (iv) the composition with the substrate (which family-of-events the class touches per chapter 02) and with the card family (which card cites it per chapter 03).
- **The model registry as gating point.** Name the registry (e.g. `models.enterprise.corp`) and the registry-provider choice — an internal OCI-based registry with Cosign attestation storage; a vendor model-registry product; or a hybrid where internal artefacts live in one registry and vendor references land in a lightweight metadata registry. Defend the choice against the enterprise's constraints — the bank's on-prem-plus-cloud posture; the healthcare provider's HIPAA-scoped environments; the SaaS vendor's multi-tenant plane. Explicitly name whether the dataset registry is a separate registry or the same registry with a different artefact-class namespace.
- **SLSA-level target per tier.** Map the enterprise's tier scheme to a minimum SLSA Build-track level per tier. A defensible default: L1 for internal experiments and prototypes; L2 for pre-production tier-2 and tier-3 systems; L3 for regulated deployments (tier-4, tier-3 EU-AI-Act high-risk, tier-3 SR-11-7 material-model). State the target and defend it against cost, tooling maturity, and obligation. Do not push L4-style hardening onto internal-experiment systems.
- **Trust boundary per vendor foundation model.** For each vendor-consumed foundation model in your scenario, state: what the *enterprise* attests (that the endpoint reference is a known registered endpoint; that the ingest event landed with a specified operator identity; that the vendor's attestation, where provided, was verified against a registered vendor identity) and what the *vendor* claims (system-card, model-card, any published SLSA / cosign / SPDX artefacts the vendor emits). Name the residual attestation gap explicitly — a vendor with only a system-card and no ML-BOM leaves a specific gap the enterprise must record as a family-3 exception (chapter 02) or absorb via the mod-109 vendor-governance interface.
- **Public vs. private Rekor decision.** Declare whether the enterprise uses (i) the public Sigstore Rekor for all signatures, (ii) a private Rekor deployment for internal signatures, or (iii) a hybrid where certain artefact classes land in public Rekor and others in private. Defend the choice — regulated-scope artefacts frequently cannot land in a public transparency log by the enterprise's own confidentiality policy; a self-hosted Rekor imposes an operations burden but preserves the enterprise's control of its transparency substrate.
- **Base-model allow-list registry.** Name the enterprise's allow-listed base-model registry — the list of upstream checkpoints the enterprise sanctions for internal fine-tuning. State how a base model enters the allow-list (governance-family log event with named executive sponsor; licence review; a specific approver seat) and how one is removed.
- **Non-scope.** At least three things you *chose not to* include in the slice and why. Candidates: full SLSA L4 hardening on internal-experiment systems (cost/obligation trade); a public-Rekor policy for confidential internal fine-tunes (confidentiality conflict); ingesting a vendor "trust me — we have SBOMs internally" letter as if it were a signed attestation (it is not evidence, per chapter 04 failure-mode 2).

### `producer-verifier-gating-map.yaml`

Produce a machine-readable table with one row per (artefact class × pipeline stage). Cover every artefact class named in `supply-chain-topology.md` and every stage in the chapter 04 pipeline schematic (base-model pull, dataset promotion, training/fine-tuning run, evaluation run, model-card authoring, model-registry ingestion, deployment promotion). Row schema:

```yaml
row:
  artefact_class: <ml-bom | spdx-ai-record | slsa-attestation | cosign-signature | model-artefact | eval-report | ...>
  pipeline_stage: <base-model-pull | dataset-promotion | training-run | evaluation-run | model-card-authoring | model-registry-ingestion | deployment-promotion>
  producer:
    seat: <named seat — data-engineer, mle, mlops-platform, platform-sre, security-engineering, ...>
    line: <first-line | second-line | third-line | external-provider>
    automation: <auto-emitted | manual | hybrid>
  verifier:
    seat: <named verifier — ci-check, security-engineering, registry-ingestion-policy, first-line-pr-reviewer, second-line-spot-check, ...>
    line: <first-line | second-line | third-line>
    cadence: <every-build | sample-N-per-week | on-incident>
  gating_actor:
    seat: <named gate — registry-ingestion-policy, ci-pipeline-gate, admission-controller, publication-gate, deployment-controller>
    on_failure: <reject | quarantine | admit-with-warning>
  substrate_event_class:
    family: <family-1 lifecycle | family-2 data-plane | family-3 governance | family-4 external>
    event_type: <named event type from exercise-01 substrate>
  notes: |
    <edge cases, exceptions, vendor-boundary specifics>
```

Include rows for both internally-produced and third-party-ingested artefacts. Every row must trace to a substrate log-event class from exercise-01 (chapter 02 family reference). At least one row must name a vendor-supplied attestation whose verifier is the enterprise's ingest policy checking the vendor identity — not the vendor itself.

### `model-registry-ingestion-policy.yaml`

Sketch the ingestion-gate policy the model registry enforces at promotion time. Express as OPA/Rego pseudocode or a Cedar sketch — the same policy engine the enterprise uses in mod-103. Do not write production-quality code; sketch the gates so a policy engineer can implement. The policy must express **at minimum** the following gates:

- **(i) BOM present and syntactically valid.** The ML-BOM file exists at the expected path; parses as a CycloneDX document; declares `bomFormat` and `specVersion`; has at least one `machine-learning-model` component.
- **(ii) Licence reconciliation.** Every licence declared in the ML-BOM (per component) is present in the enterprise's licence-policy catalog (composed with mod-103) with a disposition of `permitted` or `permitted-with-notice`. `restricted`, `prohibited`, or `unknown` licences cause a policy failure.
- **(iii) SLSA-level minimum by target tier.** The SLSA build level of the provenance attestation meets or exceeds the minimum for the artefact's target tier (per `supply-chain-topology.md`). The attestation's declared `buildType` and `builder.id` are in the enterprise's trusted-builder allowlist.
- **(iv) Sigstore signature verifiable.** The Cosign signature on each artefact (model, ML-BOM, SLSA attestation) is verifiable against the enterprise's chosen transparency log (public Rekor or private Rekor — the policy declares which and enforces accordingly). The signer identity is in the trusted-build-identity allowlist and issued by the trusted OIDC issuer.
- **(v) Provenance points to in-scope build environment.** The provenance predicate's builder identity, builder image, and source repository are all in enterprise allow-lists. An arbitrary GitHub Actions workflow from an unauthorised repository is a policy failure even if signed correctly.
- **(vi) Transitive-dependency discipline.** A fine-tune's ML-BOM inherits its base model's BOM by reference — the base model must exist as a component in the fine-tune's BOM and must resolve to an artefact previously ingested by this registry (or an approved external base-model registry entry per `supply-chain-topology.md`). A RAG system's BOM composes the model BOM and the retrieval-index BOM; neither can be missing.
- **(vii) Failure disposition.** For each gate, name the disposition on failure — `reject` (the ingest attempt is refused; a family-1 substrate event captures the reason; incident-response is notified where the failure signals tampering), `quarantine` (the artefact is admitted to a quarantine namespace pending second-line review; a family-3 governance event captures the pending exception), or `admit-with-warning` (the artefact is admitted with a warning flag; a family-3 event records the residual gap and its named sponsor). Do not default every failure to `reject` — some gates (e.g. licence `permitted-with-notice`) warrant `admit-with-warning`; some (e.g. signature verification failure) require `reject`.

The policy sketch must be readable by a policy engineer without the design document — every gate has a comment naming the tier bound and the substrate event on failure.

### `example-ml-bom.cdx.json`

Sketch one CycloneDX 1.6 ML-BOM for a specific system in your scenario. Headline fields only, not full enumeration. Include:

- Top-level: `bomFormat` = `"CycloneDX"`; `specVersion` (mark `<!-- needs-research: confirm 1.6 or later per current CycloneDX release -->`); `serialNumber` (URN format); `version` (integer, monotonically increasing per BOM revision); `metadata` with at minimum `timestamp` and `tools`.
- `components` array with **at least one `machine-learning-model` component**. For that component include: `type`, `bom-ref`, `name`, `version`, `hashes`, `licenses`, and the ML-model extension fields — model card reference by URL, training-data manifests referenced by dataset ID, model architecture / algorithm indication. Mark `<!-- needs-research: verify current CycloneDX 1.6 field names for mlModel / modelCard / considerations / datasets -->` at the points where the exact field name should be checked against the published spec rather than invented.
- `dependencies` — express at least one dependency edge (the fine-tune depends on its base model; the RAG depends on the retrieval-index BOM).
- At least one `licenses` declaration on a component. If the licence is not on the enterprise's `permitted` list, name the disposition (`permitted-with-notice` with the notice text; or a marked exception).
- A `properties` block on the model component pointing back to the model card by URL, the SLSA attestation by digest, and the substrate ingestion-event ID.

Do not enumerate every possible field. Sketch enough that a downstream reader can trace the composition — the BOM cites the model card; the model card (per exercise-02) cites the BOM; the SLSA attestation names the same subject digest.

### `example-slsa-attestation.intoto.jsonl`

Sketch one in-toto v1 envelope carrying a SLSA provenance predicate for the same artefact your ML-BOM describes. Include:

- `_type` = `"https://in-toto.io/Statement/v1"` (mark `<!-- needs-research: verify current in-toto Statement URI -->`).
- `subject` — an array with at least one subject describing the model artefact: `name` and `digest` (SHA-256 of the model artefact); the digest must match the digest declared in the ML-BOM's model component's `hashes` field.
- `predicateType` — the SLSA provenance predicate URI at the level the artefact targets (mark `<!-- needs-research: verify current SLSA provenance predicate URI (v1.0 vs. v0.2) and confirm the exact predicate string for the enterprise's chosen level -->`).
- `predicate`:
  - `buildDefinition.buildType` — a URI identifying the enterprise's training-build type (e.g. `https://enterprise.corp/build-types/pytorch-fine-tune/v1`).
  - `buildDefinition.externalParameters` — the declared inputs to the build: base checkpoint reference (URL or digest), training dataset references (dataset IDs matching the SPDX records), hyperparameter file reference, code commit SHA.
  - `buildDefinition.internalParameters` — build-platform-selected parameters (e.g. compute allocation, container image digest of the training runtime).
  - `buildDefinition.resolvedDependencies` — the resolved digests of every external input.
  - `runDetails.builder.id` — the URI identifying the build platform (e.g. `https://enterprise.corp/build-platforms/training-plane`); this must be in the trusted-builder allowlist named in `model-registry-ingestion-policy.yaml`.
  - `runDetails.builder.version` — the build-platform version.
  - `runDetails.metadata` — the run's start / end timestamps and the invocation ID.
  - Optional but recommended: `runDetails.byproducts` — evaluation reports, ML-BOM emitted, log bundle location.

Do not sign the envelope (this is a design artefact, not a live attestation). The envelope demonstrates the shape; the fields that gate ingestion (subject digest matching the BOM; builder id matching the trusted allow-list; buildType matching an enterprise-approved type) must be present and consistent.

### Cross-cutting requirements (apply across all five artefacts)

- **Composition with chapter 03 cards.** The model card (exercise-02) cites the ML-BOM by digest and cites the SLSA attestation by digest. The dataset card cites the SPDX 3.0 AI record. The system card cites the aggregate manifest of all constituent BOMs. Show at least one composition edge in each direction (BOM → card and card → BOM).
- **Composition with the substrate (chapter 02 / exercise-01).** Every ingestion attempt (accept or reject) is a family-1 lifecycle log event. Every signature-verification event is a family-3 governance log event when the verification is being audited (spot-check, incident response), or a family-1 event when it is an automatic gate outcome. Every base-model allow-list change is a family-3 event. Name the event types explicitly in `producer-verifier-gating-map.yaml` and cross-reference them to the exercise-01 substrate.
- **Composition with mod-103 policy-as-code.** The ingestion policy in `model-registry-ingestion-policy.yaml` is the same policy language the enterprise standardised on in mod-103. State whether the licence catalog referenced in gate (ii) is the same licence-policy catalog that mod-103 hosts.
- **Quarantine playbook.** Include, in `supply-chain-topology.md`, a pre-authored playbook for the case "a training-dataset licence issue lands after production deployment." The playbook must specify: (a) how the enterprise queries the supply-chain graph — which registry query enumerates every downstream ML-BOM whose `resolvedDependencies` or transitive dataset reference names the flagged dataset; (b) how the enterprise enumerates every downstream serving system — walk from BOM references to the model registry to the deployment substrate; (c) the remediation options and their triggers — redeploy from a clean rebuild, rebuild from a clean-data variant, quarantine and continue serving with a filed notice, or shut down the affected systems. The playbook must be usable at incident time without further authoring — a playbook invented at incident time is the chapter 04 failure mode.

## Starter guidance

- Do not conflate the ML-BOM with a legal-review artefact. The `licenses` field on a component is a legal *input* — it declares what the component is licensed under; the legal *review* (whether the enterprise is comfortable using it in this context) is a separate governance step whose disposition lands in the enterprise's licence-policy catalog and in the family-3 log.
- Do not push SLSA L4-style hardening targets onto internal-experiment systems. The cost of achieving L3 on every training run is significant; L1 is a rational floor for tier-1 experiments; L2 becomes appropriate as an artefact approaches promotion; L3 is the target for regulated deployments. The obligation drives the level, not the aspiration.
- A Sigstore signature that resolves against a *private* Rekor is a fundamentally different assurance shape than one that resolves against the *public* Rekor instance. The public Rekor is a distributed transparency log with community-visible tamper-evidence; a private Rekor gives the enterprise control over the log substrate but is only as tamper-evident as the enterprise's operational discipline. Pick and defend the choice per artefact class; do not treat them as equivalent.
- Do not treat a vendor "trust me — we have SBOMs internally" assertion as an SBOM. Either the vendor signs an SPDX or CycloneDX artefact the enterprise can ingest and verify, or the vendor produces nothing (and the enterprise records the gap via mod-109 vendor governance). An unsigned narrative is not evidence — chapter 04 failure mode 2 is exactly this.
- Every quarantine playbook must be pre-authored. A playbook invented at incident time is the chapter 04 failure mode where the substrate is not queryable at speed.
- The model registry is a **policy enforcement point**, not merely a storage point. The distinguishing property is that it refuses ingestion; a registry that stores whatever is uploaded and calls itself governed is not a gate.
- Do not conflate the CI pipeline's provenance with the model's provenance. The CI pipeline is *one* build step (the container image the training runtime uses); the fine-tuning run is another build step; the evaluation run is another. Each requires its own SLSA attestation; the model registry ingests the model's attestation, but the model's attestation cites the training run — not the CI build of the runtime image.
- Do not skip the base-model allow-list. An open-weight checkpoint pulled ad hoc from a public hub with no allow-list disposition is a policy failure at ingestion; the allow-list is where the enterprise pre-decides which base models are acceptable and under what disposition.

## Acceptance criteria

- [ ] Scenario stated at the top; the topology is coherent against it (the bank's transaction-fine-tune has an ML-BOM path; the healthcare provider's vendor-foundation-model has an attestation-gap disposition; the SaaS vendor's customer-facing model has a customer-disclosable provenance path).
- [ ] All four evidence classes (CycloneDX ML-BOM, SPDX 3.0 AI record, SLSA attestation, Sigstore signature) have a named producer, a named verifier, and a named gating actor in `producer-verifier-gating-map.yaml`.
- [ ] SLSA levels are declared per tier in `supply-chain-topology.md` and enforced in gate (iii) of `model-registry-ingestion-policy.yaml`.
- [ ] The registry ingestion policy is expressible as OPA/Rego or Cedar sketch and cites the mod-103 policy-as-code fabric explicitly. All seven required gates are present.
- [ ] The ML-BOM shape traces to the CycloneDX ML-BOM spec (with `<!-- needs-research: ... -->` at every point the field name was not verified against the published spec).
- [ ] The SLSA attestation shape traces to the in-toto SLSA provenance predicate (with `<!-- needs-research: ... -->` at every point the URI / field name was not verified).
- [ ] Substrate log-event linkages named per artefact class — every row in the producer-verifier-gating map cites a family-of-events from the exercise-01 substrate.
- [ ] The quarantine playbook is demonstrable against a specific dataset ID in the scenario — walk through the query, the enumeration, the remediation choice, and the family-3 log entry that records the disposition.
- [ ] Transitive-dependency discipline is described — fine-tune's ML-BOM inherits base-model BOM by reference; RAG's BOM composes model BOM + retrieval-index BOM; both directions expressed in `example-ml-bom.cdx.json`'s `dependencies` block.
- [ ] Vendor foundation-model boundary is declared — for each vendor-consumed model in the scenario, `supply-chain-topology.md` states what the enterprise attests vs. what the vendor claims, and names the residual attestation gap and its disposition.
- [ ] Public vs. private Rekor decision is made and defended.
- [ ] Every unverified spec detail (CycloneDX field name, SPDX profile field name, SLSA predicate URI, in-toto Statement URI, exact SLSA level thresholds) is marked `<!-- needs-research: ... -->` — no invented URIs or field names.

## Stretch goals

- **CRA SBOM obligations.** For enterprises with product-facing EU exposure (all three scenarios have variants of this: the bank's EU expansion; the healthcare provider's European subsidiaries; the SaaS vendor's EU customers), extend `supply-chain-topology.md` to sketch how the slice satisfies the Cyber Resilience Act SBOM obligations for products the enterprise places on the EU market. Note where the AI-specific evidence classes (ML-BOM, SPDX AI profile) extend the classical CRA SBOM and where they are additive to it.
- **Private-Rekor deployment argument.** Sketch a private-Rekor deployment argument for enterprises that cannot use public Sigstore (regulated confidentiality; sovereign-cloud isolation; contractual restriction). Cover the operational burden (running the log; witness diversity; backup and durability); the assurance trade (self-hosted transparency vs. distributed transparency); and the disposition for artefact classes that could go public without a confidentiality concern (open-source model releases, published research reproducibility artefacts).
- **Build-provenance-diff view.** Add a diff view that compares the currently-serving model's provenance to the provenance filed with the regulator packet (mod-107). A drift — the checkpoint in production is not the checkpoint filed at Article 11 — is a detectable governance failure; sketch how the diff runs, on what cadence, and where the finding lands (family-3 event; assurance-programme cadence; escalation path).
- **Attestation-gated control-evidence.** Extend the ingestion policy so that a specific control-evidence artefact — e.g. the red-team-pass attestation for a tier-4 system — must be present as an in-toto attestation signed by the red-team lead before the model can be promoted. This composes with mod-102 control-library evidence and mod-107 pre-deployment gate; sketch the attestation subject, the predicate type (custom enterprise predicate), and the policy gate that verifies it.
