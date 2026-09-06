# exercise-04: Regulator-Facing Artefact Template Authoring

**Estimated effort:** 3 hours

## Objective

Author a **set of executable view definitions** over the evidence substrate for the five regulator-facing packagings chapter 05 defines (EU AI Act Article 11 technical documentation, EU AI Act Article 72 post-market monitoring, EU AI Act Article 73 serious-incident report, SR 11-7 model documentation, FDA PCCP), scoped to the templates your scenario actually requires. Alongside the templates, produce one *worked reproducible packet* per in-scope template — a rendered instance of the template against a fixed substrate `snapshot_ts`, with signature and immutability manifest attached.

The templates are the "one source, many packagings" discipline made concrete. Every section in every template must trace to a substrate reference (a card by ID, a supply-chain artefact by digest, a control-implementation record by control ID, a log-family query by filter) rather than to free-text authored on the compliance team's laptop. The templates must satisfy the reproducibility invariant chapter 05 names: re-emission of a filed packet against the same substrate `snapshot_ts` produces the same content, byte-for-byte or content-hash-identical modulo declared formatting variance.

## Prerequisites

- Chapters [`01-evidence-architecture-scoping.md`](../01-evidence-architecture-scoping.md), [`02-audit-log-architecture-decisions.md`](../02-audit-log-architecture-decisions.md), [`03-model-and-system-and-dataset-cards.md`](../03-model-and-system-and-dataset-cards.md), [`04-ml-bom-plus-spdx-plus-slsa-plus-sigstore.md`](../04-ml-bom-plus-spdx-plus-slsa-plus-sigstore.md), and [`05-regulator-facing-artifact-templates.md`](../05-regulator-facing-artifact-templates.md) read once, with the six invariants and two failure modes marked.
- Exercises 01 (substrate scoping), 02 (cards schema authoring), and 03 (supply-chain slice) complete or available in a form you can cite — the templates *are views over the substrate those exercises design*, and every template section must resolve to a substrate reference you have already declared.
- Mod-107 chapter [`05-external-audit-and-certification-interface.md`](../../mod-107-assurance-architecture-and-audit-readiness/05-external-audit-and-certification-interface.md) walked once — the packet-delivery ceremony, packaging discipline, and audit-liaison seat authored there compose with the assembly pipeline you specify here.
- Mod-110 chapter on post-market monitoring architecture available at least at preview depth — the Article 72 template is a *view over the monitoring plan and monitoring signals* the mod-110 architecture holds; the Article 73 template consumes the incident-triage records the same architecture emits.
- Access to primary sources:
  - Regulation (EU) 2024/1689 (the AI Act) — Article 11 (technical documentation, referencing Annex IV structure <!-- needs-research: verify Annex IV numbering against the current consolidated Regulation (EU) 2024/1689 text -->), Article 43 (conformity assessment involving notified bodies), Article 72 (post-market monitoring), Article 73 (serious-incident reporting), Article 74 (information-request powers of market surveillance authorities), and Articles 9–15 (risk management, technical documentation, record-keeping, transparency, human oversight, accuracy / robustness / cybersecurity).
  - Federal Reserve SR 11-7 supervisory letter on model risk management (with OCC 2011-12 as parallel guidance) — model-documentation shape.
  - FDA "Marketing Submission Recommendations for a Predetermined Change Control Plan for Artificial Intelligence-Enabled Device Software Functions" guidance (December 2024) — the PCCP template shape <!-- needs-research: verify current FDA PCCP guidance publication metadata (title, docket, finalisation date) -->.
- See [`../resources.md`](../resources.md) for pointer set.

## Scenario

Reuse the scenario you chose in exercise-01 (US regional bank, global healthcare payer/provider, or B2B SaaS HR-tech vendor). Restate the scenario at the top of the deliverable. The scenario determines *which* of the five templates are in scope; enumerate the in-scope templates at the top of the deliverable, with a one-sentence justification per template.

- **Bank (A).** In scope typically: Article 11 (if the enterprise deploys any high-risk AI system into an EU-based operation or serves EU customers under Article 6 / Annex III scope); Article 72 (same trigger); SR 11-7 model documentation (for material models used in the US-regulated banking business); Article 73 conditional on incident occurrence (the template is authored *now* so the enterprise can meet the notification window without heroics). SR 11-7 is typically the most frequent filing shape.
- **Healthcare (B).** In scope typically: Article 11 and Article 72 for AI systems used by the enterprise's European insurance subsidiary or otherwise placed on the EU market; FDA PCCP for AI/ML-enabled Software-as-a-Medical-Device (SaMD) subject to FDA pre-market submission; Article 73 conditional on incident occurrence. SR 11-7 is not typically in scope for a healthcare enterprise (no US federal banking supervisor).
- **B2B SaaS (C).** In scope typically: Article 11 and Article 72 for EU customer-facing deployments of high-risk-classified systems (e.g., AEDT deployments in EU markets); a customer-facing SOC 2-shape variant of the same discipline for enterprise-customer trust reporting; Article 73 conditional on incident occurrence. The SOC 2-shape variant is not itself a regulator-facing template but instantiates the same one-source-many-packagings discipline against enterprise-customer auditors.

Enumerate the in-scope templates at the top of the deliverable. The rest of the exercise instantiates the discipline per enumerated template.

## Deliverables

Author the following six artefacts in a working directory of your choice.

1. **`packet-assembly-charter.md`** — the decision document that specifies the assembly pipeline shape, the signer discipline, the reproducibility discipline, the template versioning discipline, and the deviation discipline. This is the counterpart to the external-audit interface charter authored in mod-107 exercise-05 — that charter governs *how* the enterprise interacts with external providers; this charter governs *how* the enterprise assembles the packets those interactions produce.
2. **`templates/`** — a directory with one YAML view-definition file per in-scope template. Filenames like `eu-ai-act-article-11.yaml`, `eu-ai-act-article-72-postmarket.yaml`, `eu-ai-act-article-73-incident.yaml`, `sr-11-7-model-documentation.yaml`, `fda-pccp-ai-ml-samd.yaml`, or the SOC 2-shape variant `customer-facing-soc-2-ai-attestation.yaml` for scenario C.
3. **`assembly-pipeline.md`** — the pipeline shape: substrate-snapshot → template-execution → completeness-check → redaction-pass → format-render → signing → filing. All seven stages authored; the failure semantics of each stage stated (what happens when a substrate reference is missing; what happens when the redaction pipeline changes between filings; what happens when a signer is unavailable at filing time).
4. **`worked-packets/`** — one worked filed packet per in-scope template. Markdown-shape acceptable — no need to produce a real PDF/A or eSTAR render. Each worked packet must record `template_id`, `template_version`, `substrate_snapshot_ts`, and every section must show its substrate-reference resolution alongside the rendered content.
5. **`reproducibility-test.md`** — a documented re-emission procedure that assembles the same packet at the same substrate `snapshot_ts` and produces the same content. The document must state the test protocol, the comparison method (byte-identical or normalised-content-hash), the compensating discipline where byte-identity is not achievable, and the seat that runs the test.
6. **`signature-and-immutability-manifest.yaml`** — one manifest per worked packet: template ID, template version, substrate `snapshot_ts`, per-signer identity with signature-time, checksum manifest (per-section content hash + per-file hash + envelope hash), WORM storage location (immutable bucket or hash-chained ledger), and family-3 log event references for filing plus any subsequent regulator interactions.

## Requirements

### `packet-assembly-charter.md`

Decide and justify **each** of the following:

- **Assembly pipeline ownership.** The named seat that owns the pipeline (typically the head of AI governance or a regulatory-affairs seat reporting to the head of AI governance); the seat that runs the pipeline for each filing; the seat that authorises deviations. Distinguish the *author of the template* (typically the senior AI governance architect) from the *executor of the pipeline* (typically the compliance analyst or governance engineer) from the *signer of the filed packet* (see signer discipline below).
- **Signer discipline.** Signers sign the *assembled packet*, not the template. The charter must state this as a formal clause (chapter 05 invariant 5). Per template, the signer roles are declared; the signer's role assertion at signing time (Sigstore short-lived certificate with role attestation, or enterprise-PKI certificate with the signer's role encoded) is required; the charter refuses filings where a signer's role assertion is missing.
- **Template versioning discipline.** Templates carry semver (major.minor.patch). Every content-material change bumps minor or major depending on downstream regulator visibility. Every template change goes through second-line review before landing on `main`. Every template change produces a family-3 log event with references to the diff and the second-line reviewer. Filed packets reference the template version at filing time.
- **Reproducibility discipline.** For any filed packet, the enterprise must be able to re-emit the packet from the substrate at the recorded `snapshot_ts` and produce content-identical output modulo declared formatting variance. The charter states the reproducibility test cadence (at minimum: one filed packet per template per quarter is spot-checked), the compensating discipline where byte-identity fails (a normalisation function is authored and versioned; the normalised-content-hash is what is compared), and the failure escalation path (a failed reproducibility test produces a CAPA under mod-105 chapter 09 shape).
- **Deviation discipline.** Where a specific filing must deviate from the standard template — a regulator asks for a variant section shape, or a specific system requires a section the standard template does not carry — the deviation is *marked in the packet* with a `deviations` block that states the deviating section, the reason, the authorising seat, and the versioning implication (does this deviation feed a template version bump, or is it a one-off).
- **Redaction pipeline versioning.** The redaction pipeline that runs at the redaction stage of assembly is *itself* versioned. Every filed packet records the redaction pipeline version active at filing time. Reproducibility runs re-invoke the version-pinned redaction pipeline, not the current pipeline. The charter states this as a formal clause (chapter 05 failure mode 2 fix).
- **Anti-hand-assembly discipline.** Chapter 05 failure mode 1 (the template that is a Word file on the compliance team's laptop) is common. The charter must specify that no filed packet is authored in Word, Google Docs, or any freeform editor as the source of truth; the executable template is the source of truth; the format render is the *output* stage of the pipeline. Deviations from this discipline require named authorisation from the head of AI governance and produce a CAPA.
- **Non-scope.** At least three things the assembly discipline deliberately excludes. Candidates: authoring template content that the substrate does not carry (the template is a projection, not a container); signing the template itself as a way to satisfy signer discipline for future packets (invariant 5); allowing the compliance team to negotiate section shape with the regulator inside the packet (regulator-scope questions escalate to the head of AI governance and legal, they do not become in-packet edits).

### `templates/` — per-template requirements

Author one YAML view-definition per in-scope template. Every template file, at minimum, declares: template ID; template version (semver); authored-by / reviewed-by / ratified-by seats; the substrate view (log-family filters, card types, supply-chain artefact types the template consumes); the section list with per-section `source` reference and `content_type`; the signer role list with `required_when` predicates; the filing metadata (format, immutability class, transparency-log target, retention horizon).

Per-template additional requirements:

**Article 11 template.** Section structure follows Annex IV item ordering <!-- needs-research: verify Annex IV numbering against the current consolidated Regulation (EU) 2024/1689 text -->. Every section names its substrate source explicitly (which card ID and section, which supply-chain artefact by digest reference, which control-implementation evidence by control ID, which log-family query with filter predicates). Section 7 (EU Declaration of Conformity) references a separately-signed DoC artefact. Section 8 (post-market monitoring plan) references the Article 72 template's `plan` part assembled at the same `snapshot_ts` — the two templates compose. Signer block declares: head of AI governance (always); general counsel or the designated legal seat (always); AI-accountable executive (required when system is in Annex III highest-attention categories <!-- needs-research: verify Annex III scoping and the sub-categories with elevated attention -->).

**Article 72 template.** Two parts: the *plan* (authored pre-launch, filed alongside Article 11) and the *ongoing record* (updated with monitoring data on a declared cadence). The plan sections cover monitoring objectives, monitored signals, measurement methods, performance thresholds and triggers, responsibilities, reporting cadence and audiences, and review-and-update cycle. The ongoing record sections cover the period summary, measurement results, trigger events (with references to family-3 log entries), corrective actions taken, and open issues. The template composes with the mod-110 post-market monitoring architecture — the *plan* is a view over the monitoring plan the mod-110 architecture holds; the *ongoing record* is a view over the monitoring signals and family-3 log entries the same architecture emits. Cadence: quarterly for tier-3 systems, monthly summary plus quarterly consolidated for tier-4 systems.

**Article 73 template.** Serious-incident report. Timeline discipline declared with regulator-defined categories <!-- needs-research: verify Article 73 timeline categories (72-hour, 15-day, 2-day, life-critical variants and the categorisation criteria) -->. Sections cover system identity, incident description, harm assessed, immediate corrective actions, root-cause analysis (typically filed as a subsequent amendment when the initial notification is filed with root cause under investigation), affected population and scale, and provider declaration with signatures. Signer block: AI-accountable executive, head of AI governance, general counsel or the designated legal seat. The template must specify the interaction with legal review: the packet is assembled by the pipeline, legal reviews the assembled packet before filing, the filing then lands and produces a family-3 log event. Where legal review requires changes, the changes flow back into the substrate (an updated incident-triage record, an updated risk-card entry) and the packet is re-assembled — legal does *not* edit the assembled packet directly.

**SR 11-7 model documentation template.** Sections cover purpose, theory, design, data, implementation, testing, ongoing performance monitoring, limitations, and independent validation. The independent-validation section (section 9 in the reference shape) is authored by an MRM validator with declared independence from the model-developer team; the assembly pipeline enforces the independence discipline by refusing to render the section unless the referenced validation record is authored by a role in the MRM function with no crossover to the model's development team. Section 10 (usage controls) traces to control-implementation evidence for the applicable SoA rows. Signer block: MRM head, head of AI governance, model owner (attestation-of-accuracy — distinct from sign-off authority; the model owner attests to the accuracy of what the packet says about the model, not to independent validation of the model). Retention per bank records-retention schedule <!-- needs-research: verify SR 11-7 record retention expectation for AI models under supervisory examination -->.

**FDA PCCP template.** Predetermined change control plan structure per FDA guidance <!-- needs-research: verify current FDA PCCP guidance publication metadata (title, docket, finalisation date) -->. Sections cover device description, planned modifications (the authorised modification list), modification protocol (the methods used to evaluate each modification class, including performance evaluation, labelling updates, and real-world monitoring), impact assessment, cybersecurity considerations, and labelling considerations. The template composes with the model card (chapter 03) for device description and with the evaluation package (chapter 03 evaluations section) for the modification protocol's performance-evaluation methodology. **The template must include a plan-vs-actual reconciliation section** — the PCCP is a plan the enterprise commits to; the reconciliation section demonstrates adherence over time, listing each modification implemented under the PCCP with the modification date, the pre-declared modification class, the evaluation results, and the labelling updates. Signer block: regulatory-affairs head, head of AI governance, AI-accountable executive. Format render target: FDA eSTAR or eCopy — the enterprise's copy of record lives in WORM storage; the FDA electronic submission is the authoritative filing.

**Cross-cutting requirements applicable to every template.** Every template must specify:

- Named signer roles per template and per section where a section requires a specific attestation.
- Substrate source per section, using a query-language reference or path notation (e.g., `family-1.filter(system_id=<sid>, event_type=training-run)`; `model-card::<mid>.section=quantitative-analyses`; `ml-bom::<digest>.artifact=<name>`).
- `snapshot_ts` recording discipline: every filed packet records the substrate snapshot timestamp used for assembly.
- Template versioning: semver bump discipline as stated in the charter; second-line review requirement; family-3 log event on template change.
- Deviation discipline: `deviations` block schema for marking in-packet deviations from the standard template.

### `assembly-pipeline.md`

Author the seven-stage pipeline from chapter 05:

1. **Intent recorded.** The packet target (which template, which system, which regulator) and `snapshot_ts` are recorded as a family-3 log event before assembly starts.
2. **Substrate snapshot pin.** The substrate is pinned to the specified `snapshot_ts`; subsequent stages resolve against the pinned view. For "now" assemblies the pin is captured at the moment of pipeline start; for reproducibility runs the pin is the recorded `snapshot_ts` from the filed packet.
3. **Template execution.** Per-section content is assembled from substrate references; missing content produces a substrate finding (a CAPA input) and the pipeline fails-closed by default.
4. **Redaction pass.** For externally-facing packets, the version-pinned redaction pipeline runs against the assembled content. The redaction pipeline version is recorded on the packet.
5. **Format render.** The pipeline renders the packet in the regulator's preferred format — PDF/A for EU AI Act filings, FDA eSTAR or eCopy for FDA submissions, XML for specific regulator systems where required, Markdown-plus-attachments for the internal copy of record.
6. **Signing.** Signers apply cosign keyless (Sigstore short-lived certificates with role assertion) or enterprise-PKI signatures (with role assertion encoded in the certificate). Rekor entries land for cosign-signed packets. The signer's identity attestation is recorded alongside the signature.
7. **Filing.** The packet is filed in the packet registry (chapter 01 substrate.artefact_registries.packet-registry) with WORM policy applied, a family-3 log entry recorded, and any external submission channel triggered (EU regulator portal upload, FDA submission channel, filing with the notifying authority for Article 73). For externally-submitted packets, the submission acknowledgment (regulator receipt reference) is recorded as a subsequent family-3 log event and appended to the packet's manifest.

For each stage, state:

- The seat that runs or is accountable for the stage.
- The failure semantics (what happens when the stage fails; whether the pipeline aborts or emits a warning; how the failure is escalated).
- The audit-log family the stage writes to.

### `worked-packets/`

One worked packet per in-scope template. Each worked packet is a Markdown-rendered artefact that:

- Names the template ID and template version at the top.
- Records the substrate `snapshot_ts` used for assembly.
- Names the target regulator or audience.
- For each template section: shows the substrate reference used, then shows the rendered section content. Rendered content may be abbreviated with `[...abbreviated for exercise...]` where the substrate reference is unambiguous, but every section must show the reference resolution.
- Records the signer list with declared roles.
- Records the redaction pipeline version applied.
- Cross-references the corresponding entry in `signature-and-immutability-manifest.yaml`.

The worked packets do *not* need to be real PDF/A or eSTAR renders — Markdown-shape is acceptable. The point is that a reader can verify that every section resolves to a substrate reference and that the packet as a whole obeys the template.

### `reproducibility-test.md`

Document the re-emission procedure:

- **Test protocol.** Pick a filed packet (a worked packet from the deliverable set is fine for this exercise). At `T+30` days (or a simulated `T+30` for the exercise), re-run the assembly pipeline against the same substrate `snapshot_ts` recorded in the packet. Compare the output to the filed packet.
- **Comparison method.** Byte-by-byte comparison is the default. Where byte-identity is not achievable because of format-render nondeterminism (PDF/A rendering includes generation timestamps; the format renderer emits nondeterministic UUIDs in structural elements), state the normalisation function that strips or replaces nondeterministic elements before hashing, and compare content-hashes.
- **Compensating discipline.** Where a section's substrate reference is not deterministically resolvable (a query returns a set of records with nondeterministic ordering; a summary generator uses a nondeterministic LLM), state the compensating discipline: pin the resolver's ordering; pin the LLM's seed and model version; cache the generated content in the substrate itself and reference the cached artefact.
- **Escalation.** Where a reproducibility test fails, the failure produces a CAPA under mod-105 chapter 09 shape. The failing packet is not re-filed automatically; the enterprise assesses whether the divergence is material (a substantive claim in the packet differs) or immaterial (a formatting glitch), and the notified authority is informed of any material divergence.
- **Cadence.** Reproducibility tests run on at least one filed packet per template per quarter under the charter; state the seat that runs the tests and the seat that reviews the results.

### `signature-and-immutability-manifest.yaml`

One manifest entry per worked packet. Per entry:

- `template_id`, `template_version`, `substrate_snapshot_ts`.
- `signers`: list of {role, seat, signature_time, signature_method (cosign-keyless / enterprise-pki), certificate_reference, role_assertion}.
- `checksum_manifest`: per-section content hash; per-file hash for attached artefacts; envelope hash for the packet as a whole. State the hash algorithm (typically SHA-256).
- `redaction_pipeline_version`: the redaction pipeline version applied at filing time.
- `worm_storage`: the immutable-storage location (S3 Object Lock in compliance mode; hash-chained ledger; enterprise WORM appliance) with the specific bucket / ledger reference.
- `family-3_log_events`: references to the family-3 log entries recorded for filing plus any subsequent regulator interactions (acknowledgment, follow-up questions, responses).

## Starter guidance

- Do not author templates in Word, Google Docs, or any freeform editor as the source of truth (chapter 05 failure mode 1). The template lives in the same version-controlled repository the substrate configuration lives in; changes go through pull-request review; the rendered format is the *output* of the pipeline, not the source.
- Do not sign a template. Signers sign the *assembled packet*, not the template (chapter 05 invariant 5). A signature on a template carries no assertion about any specific packet and becomes a compliance-theatre artefact the first time a regulator asks what the signer attested to.
- Do not conflate the Article 72 report with the ongoing-assurance programme designed in mod-107 chapter 03. The report is a *view* over the programme's substrate; the programme itself lives in the operations of the enterprise. If the report starts to accumulate content the programme does not carry, the report has silently spawned a fourth evidence stack (chapter 01 invariant 1 violation).
- Do not treat the FDA PCCP as a one-shot submission. The PCCP is a *plan* the enterprise commits to and later shows adherence against. The template must include the plan-vs-actual reconciliation section from the outset; otherwise the enterprise will find, at first post-market review, that no substrate content connects filed modifications back to pre-declared modification classes.
- The SR 11-7 documentation and the Article 11 packet cite overlapping evidence — the same model card, the same evaluation results, the same control implementations. Every overlap is a *substrate reference*, not a duplicated section. The two templates project from the same substrate; where the shape a regulator expects differs, the projection differs, but the underlying substrate content is authored once.
- The redaction pipeline is versioned (chapter 05 failure mode 2 fix). The redaction rules that ran at filing time must be re-runnable at reproducibility-test time. If the enterprise's current redaction discipline is "the compliance analyst redacts manually," that is not a versioned pipeline and reproducibility will fail. Name the interim compensating discipline (an archived redaction-worksheet with the specific redactions applied per filing) and state the maturity path to a versioned automated pipeline.
- Do not sign a packet without recording the signer's identity attestation. Sigstore keyless issuance produces a short-lived certificate bound to the signer's OIDC identity with claims that can be encoded in the certificate; enterprise PKI produces a certificate with the signer's role encoded per the enterprise's directory. Either way, the signature manifest must carry a resolvable identity assertion — a bare cryptographic signature with no identity binding is repudiable.
- The Article 73 template exists so that at incident-time the enterprise assembles the report from the substrate rather than authoring it from scratch under time pressure. Author the template *now*; rehearse the assembly against a simulated incident; the goal is that at real incident-time the assembly is a routine operation, not a bespoke authoring project.

## Acceptance criteria

- [ ] Scenario is stated at the top; in-scope templates are enumerated with one-sentence justification each.
- [ ] All in-scope templates are authored as YAML view-definitions in `templates/`; no template is a Word file or freeform Markdown that lacks substrate references.
- [ ] Every template section names its substrate reference explicitly (card ID and section, supply-chain artefact by digest, control-implementation by control ID, or log-family query with filter predicates) — no section is free text.
- [ ] Every worked packet records `template_id`, `template_version`, and `substrate_snapshot_ts` at the top and cites the substrate reference resolved for each section.
- [ ] `assembly-pipeline.md` authors all seven stages (intent-recorded, substrate-snapshot-pin, template-execution, redaction-pass, format-render, signing, filing) with per-stage accountable seat, failure semantics, and audit-log family.
- [ ] Format-render target is declared per template (PDF/A for EU filings; eSTAR or eCopy for FDA; XML where required; Markdown-plus-attachments for internal copy).
- [ ] Signers named per template with declared role assertion at signing time (cosign keyless with OIDC identity, or enterprise-PKI certificate with role encoded); the charter refuses filings where a signer's role assertion is missing.
- [ ] `reproducibility-test.md` documents the test protocol, comparison method (byte-identical or normalised-content-hash), compensating discipline where byte-identity fails, escalation on failure, and test cadence.
- [ ] The reproducibility procedure is demonstrable — a reader could follow the document, pick a worked packet, and re-run the assembly against the recorded `snapshot_ts`.
- [ ] `signature-and-immutability-manifest.yaml` is present for every worked packet with template ID, template version, substrate `snapshot_ts`, signer entries with identity attestation, checksum manifest at section-plus-file-plus-envelope granularity, redaction pipeline version, WORM location, and family-3 log event references.
- [ ] SR 11-7 template records the validation-independence discipline (section 9 authored by MRM validator independent of model-developer team; assembly pipeline enforces the independence by refusing to render otherwise).
- [ ] FDA PCCP template includes the plan-vs-actual reconciliation section.
- [ ] Redaction pipeline versioning discipline is stated in the charter and recorded per worked packet in the manifest.
- [ ] Every unverified reference to Annex IV numbering, Article 73 timeline categories, Annex III sub-category attention scoping, FDA PCCP guidance metadata, or record-retention duration is marked `<!-- needs-research: ... -->` — no invented section numbers, article numbers, timeline hours, or FDA guidance titles.

## Stretch goals

- Extend the templates to a state-regulator variant: a NAIC AI Model Bulletin insurance packet (the shape state insurance departments are converging toward under NAIC guidance <!-- needs-research: verify current NAIC AI Model Bulletin adoption pace and per-state variants -->); a Colorado SB 24-205 developer-of-high-risk-AI documentation record. Show that the state-regulator variant is a template *composition* over the same substrate rather than a from-scratch authoring project.
- For the B2B SaaS scenario, add a customer-facing packet shape — a SOC 2 + AI-attestation variant that composes ISAE 3000 / SOC 2 control shapes with the AI-specific cards and control-implementation evidence. Show that the customer-facing template is a view over the same substrate the regulator-facing templates project from, with a redaction pipeline configured for customer-facing audience.
- Represent one template as an OSCAL `assessment-plan` (preview of chapter 06 and exercise-05). The OSCAL representation gives the substrate a machine-readable declaration of the template's scope, sections, and completeness expectations that the pre-deployment gate query and the internal audit sampling query can both consume.
- Simulate a regulator follow-up on a filed packet at `T+90` days. The notified body sends a follow-up question about a specific section of the Article 11 packet you filed. Run the reproducibility test at `T+90` — assemble against the recorded `snapshot_ts` — and demonstrate that the enterprise's answer to the notified body is derived from the reproduced packet rather than from human memory of what the packet said.
