# The card family — model, system, dataset, and risk cards as authoritative disclosures

## Why this chapter exists

The most consequential documentation failure inside an AI programme is not the missing document; it is the *unowned* document. A GenAI feature ships; a model owner has a page describing what the model does; an analyst has an AI Impact Assessment mostly derived from the model owner's page; a compliance analyst has an entry in a regulator-facing spreadsheet mostly derived from the AIA. Three disclosures exist about the same model; none is *the* disclosure the enterprise stands behind. When the regulator asks "what did you tell the market about this model," the answer is a wiki search. When a plaintiff subpoenas the disclosure record in litigation, the artefact produced is not signed and its provenance cannot be established. When internal audit samples for completeness, the disclosures conflict on the details that matter — the training data cited, the evaluation harness used, the population the model was tested against.

The card family — model cards, system cards, dataset cards, risk cards — is the architectural answer. A *card* is a controlled, versioned, signed disclosure about a specific AI-programme artefact that the enterprise stands behind. It is authored inside a defined lifecycle, reviewed by a defined chain, gated by a defined publication forum, retained in the audit-log substrate (chapter 02), and represented machine-readably in the OSCAL catalog (chapter 06). Chapter 05's regulator-facing packagings are *views* over the card content plus other substrate evidence — cards are the single most important input to those packagings. Exercise-02 walks the drill.

The card family is grounded in a set of primary works that the level-50 architect should be able to cite by name and expect the audit committee to recognise. Mitchell et al. "Model Cards for Model Reporting" (FAT* 2019) fixes the model-card shape. Gebru et al. "Datasheets for Datasets" (Communications of the ACM) fixes the dataset-card shape. The Hugging Face Model Cards guide operationalises the model-card metadata schema for a repository the enterprise's engineers are already using. The Partnership on AI ABOUT ML framework specifies the recommended lifecycle for machine-learning documentation. The UK Algorithmic Transparency Recording Standard (ATRS) fixes a public-sector-facing tiered disclosure shape. System cards as published by frontier-model providers (OpenAI, Anthropic and Google DeepMind system cards for major model releases) fix a shape reference for the integrated-system disclosure this chapter treats as its own artefact class. The architect composes these primary sources into the enterprise's own controlled schema; the enterprise's cards are not literal copies of Mitchell et al.'s shape but disciplined derivatives of it.

## What a card is, structurally

A card is:

- **A controlled artefact.** Its schema is authored, its fields are declared mandatory or optional per tier, its version is tracked, its author and reviewers are named.
- **A disclosure about one artefact.** Cards are *per-artefact*: one model, one system, one dataset, one system's risk. A card that tries to cover multiple artefacts becomes a wiki page.
- **Provenance-attested.** The card is signed at publication (Sigstore cosign or the enterprise's PKI); the signature is recorded in the substrate (chapter 02); the signature is verifiable against a transparency log.
- **Bound to substrate content by reference.** The card cites evaluation runs by pipeline-run-ID (family 1 log), datasets by dataset-registry ID and manifest hash (family 2 log), controls by control-library ID (mod-102), risk register entries by RR-ID (mod-106).
- **Rendered both for humans and machines.** The human-facing rendering is Markdown; the machine-readable rendering is JSON / YAML the OSCAL catalog (chapter 06) ingests.
- **Subject to a lifecycle.** Draft → second-line review → sign-off → publication gate → publication → re-review on trigger.

## What a card is not

- **An AI Impact Assessment.** The AIA is a *decision* artefact (mod-105 chapter 04) that recommends whether and how to deploy. A card is a *disclosure* artefact that describes what was deployed. Some fields overlap; the artefacts have different audiences and different lifecycles.
- **A user manual.** The card serves regulator-facing, audit-facing, and disclosure-facing audiences primarily. It may compose into an end-user disclosure but is not itself an end-user document.
- **A wiki page.** The wiki page is where the model owner takes notes. The card is where the enterprise makes a claim.
- **A one-time artefact.** Cards have re-review triggers; a card that has not been re-reviewed in 18 months on a system that has drifted is a card that no longer describes the system.
- **Sufficient by itself.** The card describes; the substrate holds the evidence backing the description. A card that makes claims without substrate evidence backing them is either an over-claim or a documentation-only implementation.

## The four cards

The enterprise's card family is four cards. Some enterprises collapse the model and system cards into one; the architect's choice is defensible either way, but conflating them tends to produce cards that describe the model well and the surrounding system poorly.

### Model card

The model card describes a specific model artefact — a trained checkpoint, a fine-tuned variant, a prompt-engineered specialisation — as a mathematical object with a training origin, an evaluation profile, and a stated intended use.

Grounded in Mitchell et al. FAT* 2019. Required sections (mandatory across tiers) approximate the Mitchell et al. shape and extend it with substrate-binding fields the enterprise's audit-log substrate populates:

- **Model details.** Model name, version, artefact hash, training run pipeline-run-ID, base model (for fine-tunes), architecture, parameter count, training compute (order-of-magnitude), authoring team, licence, contact.
- **Intended use.** Primary intended uses; primary intended users (populations); explicitly out-of-scope uses. This is the section the regulator and the deployer read first.
- **Factors.** Relevant groups (protected characteristics, environments, deployment contexts) the evaluation covers or explicitly does not.
- **Metrics.** The metrics selected, why they were selected, uncertainties in the measurement.
- **Evaluation data.** Datasets used; motivation for choice; preprocessing; the eval-set version hashes and seeds. Cross-reference to the dataset card(s).
- **Training data.** Datasets used; motivation for choice; preprocessing. Cross-reference to the dataset card(s). Where training data cannot be fully disclosed (proprietary corpora, purchased data with licence restrictions), the section says so and cites the restriction.
- **Quantitative analyses.** Disaggregated evaluation results across factors; confidence intervals or equivalent uncertainty. Cross-reference to evaluation-run pipeline IDs the substrate holds.
- **Ethical considerations.** Foreseeable harms, mitigations, unresolved concerns. Cross-reference to the risk card and to the AIA (mod-105 chapter 04).
- **Caveats and recommendations.** The residual limitations the deployer must know.

Higher-tier overlays (tier-3 and tier-4) add: red-team engagement findings summary; robustness / adversarial-ML measurements; interpretability / reason-code evidence where the deployment requires it; independent-evaluation attestations from the level-35 evaluation engineer (chapter 07).

Hugging Face Model Card metadata (`README.md` YAML front-matter for models pushed to the Hub) is a compatible operational schema — `license`, `datasets`, `metrics`, `tags`, `pipeline_tag`, `model-index` and related fields — that the enterprise's schema can emit as a downstream projection when the model is published to a shared registry. Enterprises with an internal model registry (chapter 04) commonly emit the same shape for internal consumption.

### System card

The system card describes an *integrated system* — the model plus its wrapper, prompt, retrieval, tools, guardrails, human-oversight surface, and UX. This is the disclosure that answers "what will actually be deployed in front of a user?"

There is no equivalent to the Mitchell et al. paper for system cards; the shape is drawn from operator-of-frontier-model system cards (OpenAI system cards for GPT-4 / GPT-4o and successors; Anthropic system cards for Claude releases; Google DeepMind system cards for Gemini releases) and from the emerging practice. The architect makes the schema defensible in the enterprise's own terms.

Required sections (mandatory across tiers):

- **System identity.** System name, version, deployment scope (population, jurisdiction, product surface), owner team.
- **System composition.** The model(s) called (cross-reference model cards); the retrieval store(s) or knowledge sources; the tools (function-calling interfaces, code-execution sandboxes, web-browsing capabilities, external APIs); the guardrails (input filters, output classifiers, refusal patterns).
- **Prompt-engineering surface.** The system prompt shape and versioning; the range of user-shaped inputs the system is designed to accept; the prompt-injection posture.
- **Human-oversight design.** The oversight surface — who reviews what, at what latency, with what authority. Reference to the mod-105 chapter 09 human-oversight architecture.
- **Guardrails and safety features.** Content-safety classifiers used; refusal patterns; escalation paths; kill-switch design.
- **Known behaviours.** Behaviours the system is known to exhibit — including behaviours the system deliberately produces (refusals, redirects) and behaviours known to occur but not intended (hallucination classes; systematic biases; failure modes under adversarial input).
- **Evaluation summary.** The system-level evaluation — not just the model-level — including red-team results.
- **Change history.** Material changes since last publication.

Higher-tier overlays add: post-market monitoring plan cross-reference (mod-110); serious-incident response plan cross-reference; rollback / kill-switch demonstration record.

### Dataset card

The dataset card describes a specific dataset artefact — a training corpus, an evaluation dataset, a fine-tuning corpus, a benchmark — as a collection with a provenance, a composition, a collection process, and a maintenance discipline.

Grounded in Gebru et al. "Datasheets for Datasets." Required sections approximate the Gebru et al. shape:

- **Motivation.** Why the dataset was created; the tasks and populations it was created to serve; who funded / created it.
- **Composition.** What instances the dataset comprises; number of instances; sampling process; recommended data splits; PII posture; sensitivity posture.
- **Collection process.** How the data was acquired, sampled, and annotated; time frame; the individuals or automated processes involved; consent / notice arrangements where personal data is included.
- **Preprocessing / cleaning / labelling.** The transformations applied; the labelling protocol; annotator population; inter-annotator agreement where measured.
- **Uses.** Intended uses; discouraged uses; known off-label uses; considerations for downstream users. Cross-reference to model cards / system cards using the dataset.
- **Distribution.** How the dataset is made available (internal-only, published under a licence, sold, restricted); the licence terms; distribution channel.
- **Maintenance.** Who maintains; how updates are versioned; deprecation schedule; retention posture; DSAR-driven modification handling for personal-data datasets.

Higher-tier overlays add: dataset-registry ingestion evidence (SPDX 3.0 record, chapter 04); representativeness measurements against the deployment population; harmfulness / offensive-content analysis; the dataset-manifest hash the training run cites (cross-reference to family 2 audit-log substrate).

### Risk card

The risk card describes the *risk posture* of a specific AI system as scored against the enterprise's risk taxonomy (mod-106). It is the artefact that carries the risk-engineering slice into regulator-facing packagings.

Required sections (mandatory across tiers):

- **System identity.** System name, version, deployment scope; cross-reference to the model / system cards.
- **Applicable taxonomy categories.** Named risk categories from mod-106 that apply; motivation for the applicability filter (why others are excluded).
- **Inherent-risk scoring.** Per applicable category, the inherent-risk reading (before controls); the scoring methodology; the confidence band.
- **Controls in place.** Per applicable category, the mod-102 control-library rows that address the risk; the implementation state; the evidence reference into the substrate.
- **Residual-risk scoring.** Per applicable category, the residual-risk reading (after controls); comparison against the appetite / tolerance table (mod-106 chapter 03).
- **Control-defeated scoring.** Per applicable category, the residual reading under the *control-defeated* hypothesis (mod-106 chapter 03 vocabulary). This is the reading the pre-deployment gate (mod-107 chapter 02) uses to test whether the residual is within tolerance under adversarial pressure.
- **Monitoring plan.** How each residual is monitored; the metric, the threshold, the trigger action. Cross-reference to mod-110 post-market monitoring.
- **Accepted residuals.** Where the residual is above tolerance and has been accepted, the acceptance record — who accepted, under what authority, with what expiry. Cross-reference to mod-105 chapter 03 accountable-executive sign-off records.

Higher-tier overlays add: quantitative-scenario reads (Anthropic RSP / OpenAI Preparedness / Google DeepMind FSF shape references where the risk is capability-tier or catastrophic-scenario relevant); scenario-narrative fields (a plaintiff-facing account of what *could* go wrong under the residual, with the mitigating factors that make the enterprise's risk-acceptance defensible).

## The ATRS tiering and public-facing views

The UK Algorithmic Transparency Recording Standard (ATRS) fixes a tiered public-facing disclosure shape for public-sector algorithms. It is not a card in itself but a *view* over the enterprise's cards for a specific consumer (the public / researchers / journalists via the government's ATRS repository). Enterprises operating in public-sector contexts, or private-sector enterprises adopting an ATRS-shape public disclosure by choice, produce ATRS records by projecting the model card, system card, and risk card content through a defined view.

ATRS's approximate shape (verify against the current standard version): a Tier-1 summary (short public-friendly disclosure — what the system does, why it is used, who is affected, key risks) and a Tier-2 detailed record (fuller technical detail including evaluation, procurement, and human-oversight information). <!-- needs-research: verify ATRS current version tier structure against gov.uk publication -->

The Partnership on AI ABOUT ML framework contributes the lifecycle recommendations — that ML documentation is authored progressively across the ML development lifecycle rather than dumped at the end, that documentation is versioned, that responsibility for authorship and review is distinct. The enterprise's card lifecycle (below) draws from ABOUT ML.

## The card lifecycle

Every card moves through a lifecycle the architect designs. The lifecycle has six stages; each stage produces a governance-workflow log event (chapter 02 family 3) so the substrate holds the full history.

1. **Draft.** The first-line author (model owner for a model card; system owner for a system card; data engineer or dataset owner for a dataset card; risk engineer for a risk card) drafts against the current schema. The draft is a substrate event; the draft artefact is stored in the card registry as an unpublished revision.
2. **Second-line review.** The governance analyst reviews for schema completeness, internal consistency, and traceability into substrate evidence. Findings drive a return-to-draft or a proceed.
3. **Specialist sign-off.** For higher-tier cards the specialist role signs off on the section they own — the level-35 evaluation engineer signs off on the evaluation section of a tier-3 or tier-4 model / system card; the level-25 risk engineer signs off on the risk card; the level-15 analyst assembles.
4. **Publication gate.** The second-line-owned decision that the card is fit for publication in its intended audiences (internal register; customer-facing DPA appendix; regulator-facing packet; public disclosure). Signature block, minimum-fields check, redaction check.
5. **Publication.** The card is signed (Sigstore cosign or enterprise PKI), the signature written to the transparency log (Rekor), and the card is emitted to its intended audiences via the packet / registry / feed channels.
6. **Re-review.** On any re-review trigger — material change to the system, drift beyond a monitoring threshold, incident on the system, scheduled cadence per tier (typically 12 months for tier-2, 6 months for tier-3, 3 months for tier-4), regulatory change altering the disclosure shape — the card re-enters at stage 1 with the previous published version as the baseline.

The signing evidence in stages 3 and 5 is what makes the card enforceable. A card that is "published" by being emailed to a compliance drive without a cryptographic signature and a substrate event is a wiki page.

## The publication gate

Distinct from the pre-deployment gate (mod-107 chapter 02), the publication gate is the *card-publication* forum. It exists because publishing a card the enterprise does not stand behind is worse than not publishing at all — the enterprise then owns a claim it cannot substantiate.

The gate's checks:

- **Minimum-fields check.** Every mandatory field for the card's tier is populated; no `TBD` or `TK` remains.
- **Traceability check.** Every claim of substance is traceable to substrate evidence (a training-run pipeline ID, an evaluation-run pipeline ID, a dataset manifest hash, a control-implementation-evidence reference, a risk register entry).
- **Redaction check.** No PII appears in the card body; trade-secret and security-sensitive detail is properly redacted where the card is intended for external publication; the redaction pipeline is versioned.
- **Consistency check.** The model card and system card do not contradict each other on shared claims (model version, evaluation results); the risk card scoring is consistent with the risk register.
- **Audience-appropriateness check.** For an ATRS or public-facing card the reading level and disclosure depth are appropriate; for a customer-facing card the DPA-appendix appropriate fields are present; for a regulator packet the Article 11 / SR 11-7 mapping is intact.
- **Signature block completion.** Second-line reviewer, specialist signer (where applicable), publication-gate chair. Where the card is public-facing or regulator-facing, an additional legal-seat signature.

The gate's failure to complete produces a return-to-draft; the return event is itself a substrate log entry.

## Machine-readable representation

Every card is authored primarily in Markdown for human readability and emitted in a JSON / YAML representation the OSCAL catalog (chapter 06) ingests. The emission is *derived*, not authored — a card that is authored only in YAML tends to become a data-entry chore that no one reads; a card authored in Markdown against a schema that constrains the Markdown structure (front-matter + defined sections + defined field encodings) produces both a readable document and a machine-readable projection.

Sketch of the shared card schema shape (aligned with Hugging Face Model Card metadata for compatibility, extended for the enterprise's substrate binding):

```yaml
---
card_type: model | system | dataset | risk
card_schema_version: 1.3.0
card_version: 4
system_id: SYS-2027-0087
tier: 3
jurisdictions_in_scope: [US-CO, US-NY, EU-*]
authored_by:
  role: model-owner  # or system-owner, dataset-owner, risk-engineer
  seat: <named seat>
reviewed_by:
  role: governance-analyst
  seat: <named seat>
signed_by:
  - role: evaluation-engineer   # for the eval section
    seat: <named seat>
  - role: publication-gate-chair
    seat: <named seat>
  - role: legal-seat  # for external-publication cards
    seat: <named seat>
substrate_bindings:
  training_run_pipeline_id: pipe-run-2027-000442
  evaluation_run_pipeline_ids: [pipe-run-2027-000501, pipe-run-2027-000502]
  dataset_ids: [DS-2027-004, DS-2027-005]
  ml_bom_ref: mlbom://model-registry/sys-2027-0087/v4.2.1
  slsa_attestation_ref: attestation://.../v4.2.1
  control_implementation_evidence_refs: [ctrl-ev/CTL-101/impl-2027-1180, ...]
  risk_register_refs: [RR-2027-1122, RR-2027-1123]
published_at: 2027-04-12T14:22Z
publication_signature: <cosign signature reference / rekor entry>
publication_scope: [internal-register, customer-dpa-appendix, article-11-packet, atrs-tier-2]
next_review_at: 2027-07-12  # tier-3 quarterly cadence for evaluation section
re_review_triggers:
  - material_change: any change to substrate_bindings.evaluation_run_pipeline_ids
  - drift: any monitoring plan trigger fires
  - incident: any tier-3 incident on this system
  - regulatory: change in EU AI Act obligation scope for Colorado / NY / EU jurisdictions
---

# Model card — <system name> — v4.2.1

## Model details
...

## Intended use
...

## Factors
...

<!-- and so on, one section per required schema section -->
```

The front-matter is the machine-readable projection. The body is the Markdown document. The OSCAL catalog (chapter 06) ingests the front-matter; the substrate stores the whole file with its signature; the human reader reads the body; automation reads the front-matter.

## The six invariants the card family holds

**Invariant 1 — every declared card in scope has a current version at the tier's currency window.** For every AI system in the enterprise's AI inventory (mod-105 chapter 02) whose tier requires a card, a current, in-review-window card exists. Failure mode: the inventory has 40 tier-3 systems and only 22 have current cards; the certification body samples and finds the gap.

**Invariant 2 — every card is traceable to substrate evidence.** Every claim of substance in a card references a substrate artefact by ID and hash. Failure mode: the card claims 87% accuracy on a proprietary eval; internal audit cannot find the evaluation run and cannot reproduce the number.

**Invariant 3 — every card is signed at publication and the signature is verifiable against a transparency log.** Failure mode: the card is downloaded as a PDF from a compliance drive; a plaintiff's expert alleges it was edited after the incident; the enterprise cannot show the file the regulator was given is the file that was published.

**Invariant 4 — the publication-gate discipline is enforced by workflow, not by convention.** No card is published without the gate having emitted a substrate event with the required signatures. Failure mode: a model owner publishes a card directly to the customer-facing DPA appendix without the gate; the gate's authority is theatrical.

**Invariant 5 — the re-review triggers actually fire.** Cards on drifted systems are re-reviewed; cards on incident-affected systems are re-reviewed; the scheduled cadence is met. Failure mode: cards are published and forgotten; three years later they describe a system that no longer exists.

**Invariant 6 — the machine-readable projection is authoritative for automation.** The OSCAL catalog's completeness queries (chapter 06) run against the front-matter; the front-matter is the source of truth for automation. Failure mode: the wiki page differs from the front-matter; automation reads the front-matter, humans read the wiki, and they diverge.

## Two failure modes to design against

**Failure mode 1 — the card that lives in a wiki.** The model owner keeps a page describing the model. The page is called the "model card." The page has no schema, no signature, no substrate binding, no publication gate, no re-review trigger. When the regulator asks, the compliance team screenshots the page and calls it the disclosure. The wiki page drifts as the model changes; the screenshot is stale before the regulator has finished reading it. The fix is architectural: the wiki page is a *draft input* to a controlled card in the card registry; the controlled card is what the enterprise stands behind; the wiki is deprecated as a disclosure vehicle.

**Failure mode 2 — the card that is never re-reviewed after publication.** The first publication is a triumph — the schema is filled, the signatures are captured, the substrate binding is intact. Twelve months later the system has been fine-tuned, the eval harness has evolved, a new content-safety classifier has been added to the guardrail stack, and the card still describes the system at publication time. When the regulator asks about the current state, the card is embarrassing. The fix is architectural: re-review triggers are declared in the card front-matter and monitored by the OSCAL catalog's freshness query; the operating rhythm has a monthly re-review sweep against the trigger list; a stale card is a substrate finding that opens a CAPA.

## Summary

The card family — model, system, dataset, and risk cards — is the enterprise's authoritative disclosure surface. Grounded in Mitchell et al., Gebru et al., HF Model Cards, PAI ABOUT ML, and ATRS as primary references, the enterprise's schema is a disciplined derivative sized for the tier the card applies to. Each card is a controlled artefact — schema-declared, versioned, signed at publication, traceable to substrate evidence, re-reviewed on trigger. The card lifecycle (draft → review → sign-off → publication gate → publication → re-review) produces substrate events at every stage. The publication gate enforces minimum-fields, traceability, redaction, consistency, audience-appropriateness, and signature-completion checks. The machine-readable projection lives in the front-matter and is authoritative for automation. Six invariants and two failure modes shape the design. The next chapter designs the supply-chain evidence slice — CycloneDX ML-BOM, SPDX 3.0 AI, SLSA, Sigstore — that the ML-BOM references from a model card ultimately bind to.
