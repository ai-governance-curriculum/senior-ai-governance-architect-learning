# External incident corpora as calibration references — AIID, OECD.AI, MIT AI Risk Repository

## Why this chapter exists

An enterprise post-market surveillance (PMS) programme sees only what its own footprint produces. That footprint — however large — is a strict subset of the AI failure-mode surface the field has actually documented. If the enterprise draws its taxonomy, its severity thresholds, its near-miss detection candidates, and its horizon-scanning exclusively from its own event store, the taxonomy will lag the ecosystem by whatever period separates the enterprise's next incident from the ecosystem's already-published one. When a certification body, a sector regulator, or a plaintiff's expert witness asks *why the enterprise's category set does not name a failure mode that has been in the AI Incident Database for two years*, "we had not observed it internally" is not a defensible answer. It is the exact shape of finding a mature safety-critical audit is designed to produce.

The architectural fix is to bind the enterprise PMS to the public corpora as *calibration references* — inputs whose deltas the enterprise formally reviews on a scheduled cadence, whose classifications the enterprise formally compares against its own, and whose horizon-scanning content the enterprise formally routes into taxonomy amendments, control-library updates, and monitor changes. The corpora do not replace the enterprise PMS; they calibrate it, the way a metrology lab calibrates a factory instrument against a national reference standard. This chapter walks the three principal corpora at title level, the four calibration activities the architect designs into the PMS cadence, the role separation that keeps the calibration lightweight, the four invariants the calibration workflow holds, and the three failure modes it is designed against.

The chapter sits after chapter 05's SOC handoff (which fixes where new detection candidates land) and before the module's exercise set. It composes with mod-106 chapter 07's taxonomy-amendment routing and mod-107 chapter 04's third-line sampling — the calibration artefacts this chapter defines are exactly the artefacts internal audit reaches for when it asks *are you actually doing horizon-scanning, or have you assumed your own footprint is representative?*

## The three principal corpora at title level

The AI-safety-relevant public-corpus landscape has consolidated around three widely cited references. The architect should know each at title level, understand what it is and what it is not, and cite it without inventing counts or specific case studies.

### AI Incident Database (AIID)

The **AI Incident Database** is a curated public collection of AI incidents drawn from press coverage, disclosures, and researcher submissions, indexed against multiple classifiers (including CSET's incident taxonomy) and maintained by the Responsible AI Collaborative at [incidentdatabase.ai](https://incidentdatabase.ai). Each indexed incident carries a canonical short description, references to the reporting sources, a mapping to one or more incident *classes* under the applied taxonomies, and an evolving set of contributing-factor and harm-type tags. The corpus's structural value is that it *is a corpus* — it is enumerated, addressable by ID, versioned, and open enough that an enterprise analyst can cite an AIID incident number in an internal review artefact and expect the reader to be able to verify it.

<!-- needs-research: current published number of AIID indexed incidents at the time this chapter is used; the count is updated on a rolling basis and should be cited from the live site rather than pinned here. -->

<!-- needs-research: current AIID applied taxonomies list — CSET, GMF, and any additional classifiers active at the time of citation. -->

For enterprise calibration, AIID's value is *breadth* — it surfaces incidents across sectors, modalities, capability tiers, and jurisdictions that an internal event store will not see. Its limitation is that it is a *press-and-disclosure* corpus, not a *ground-truth* corpus: the reporting bias toward high-visibility incidents means the corpus over-represents consumer-facing systems and under-represents enterprise back-office AI where failures are managed privately. The calibration workflow uses AIID for taxonomy-coverage and horizon-scanning, and treats its severity classifications as one *reference view* rather than as authoritative ground truth.

### OECD.AI Incidents Monitor

The **OECD.AI Incidents Monitor** is the Organisation for Economic Co-operation and Development's monitoring of AI incidents, hosted under the OECD.AI Policy Observatory at [oecd.ai](https://oecd.ai). It draws on media reports and disclosures and applies the OECD's own working definitions of AI incidents and hazards, developed through the OECD AI Governance Working Party. The Monitor is complementary to AIID rather than duplicative — it applies a policy-community lens, groups incidents against OECD taxonomic conventions, and surfaces cross-jurisdictional patterns useful for governance-office reporting.

<!-- needs-research: current OECD.AI Incidents Monitor methodology, definitions of "AI incident" versus "AI hazard", and any published totals or dashboards to cite. -->

For enterprise calibration, OECD.AI's value is *policy-shape alignment* — its category conventions map cleanly onto the vocabulary regulators, standards bodies, and national AI safety institutes use, which reduces translation friction when the enterprise's PMS output has to feed regulator-facing filings under the EU AI Act Article 73 workflow (chapter 04) or under sector-regulator equivalents.

### MIT AI Risk Repository

The **MIT AI Risk Repository**, hosted at [airisk.mit.edu](https://airisk.mit.edu), is a meta-analysis corpus that catalogues risk taxonomies and enumerated risks from the AI-safety and AI-governance literature. Its role in this track was fixed in mod-106 chapter 01, where it was named as the meta-taxonomy reference the architect *composes from* rather than *inherits verbatim*. Where AIID and OECD.AI index *incidents that happened*, the MIT Repository indexes *risks the field has considered*. The two are complementary — a taxonomy-coverage gap surfaces when the MIT Repository names a risk category the enterprise does not carry *and* AIID / OECD.AI show incidents of that category the enterprise's monitors would not catch.

<!-- needs-research: current published totals for taxonomies and risks catalogued in the MIT AI Risk Repository (43 taxonomies / 777 risks was the reported figure around the v1 release in 2024; the Repository updates on a rolling basis). -->

For enterprise calibration, the MIT Repository's value is *coverage completeness*. It is the reference against which the enterprise asks *have we named all the categories the field has named?* — with the understanding, from mod-106 chapter 01's failure-mode-2 warning, that verbatim adoption of 700-plus categories is itself a failure mode.

### Adjacent corpora to acknowledge

Beyond the three principal corpora, the architect should acknowledge a small set of adjacent references:

- **Partnership on AI**'s incident-related work — the AI Incidents Database collaborations, and PAI's contributions to responsible-disclosure norms for AI incidents.
- **CSET (Center for Security and Emerging Technology)** at Georgetown — publishers of the CSET taxonomy applied within AIID and of standalone incident-classification research.
- The broader **OECD AI Policy Observatory** — the parent site of the Incidents Monitor, which also carries policy trackers, national AI strategies, and OECD AI Principles reporting the enterprise's regulatory-scanning function may care about.

Sector-specific reference corpora — FDA MAUDE-style adverse-event registries for medical devices, ORX loss-event data for finance, ICS-CERT and CISA advisory corpora for industrial control — remain in scope for their sectors (mod-113 sector blueprints), and the cross-sector AI corpora complement rather than replace them.

## Why the enterprise PMS is not enough on its own

Chapter 01 of this module fixed the shape of the enterprise PMS event store and the near-miss discipline. That store is necessary and, by itself, insufficient. Four calibration questions the store cannot answer from its own contents motivate the external-corpus binding:

**(a) Taxonomy-coverage completeness.** Is the enterprise's category set (per mod-106) missing a failure mode the field has already documented? The internal store, populated by internal observation, will surface a missing category only after the enterprise has suffered an incident inside it — precisely the audit finding the architecture is meant to prevent. External corpora surface the same gap on a leading indicator: a category the MIT Repository names, that AIID shows incidents of, that the enterprise taxonomy does not carry, is a gap the enterprise ought to close before an internal incident forces the issue.

**(b) Near-miss calibration.** Is the enterprise's realised-to-near-miss ratio plausible against public precedent? Mature safety disciplines expect near-miss volume to exceed realised-incident volume by an order of magnitude or more (chapter 01 named the industrial-process-safety and healthcare-patient-safety precedents). An enterprise whose ratio is inverted — more realised than near-misses — has almost certainly mis-classified realised incidents as near-misses, or under-reported near-misses, or both. Public corpora provide the external anchor against which the internal ratio's plausibility can be argued.

**(c) Tier-4 severity calibration.** Is the enterprise's serious-incident threshold consistent with what the public corpora treat as serious? An enterprise whose tier-4 threshold is systematically stricter than the corpora's "serious incident" classification will under-report externally, and produce regulator- and audit-facing embarrassment when a public dataset flags an enterprise-comparable event as serious that the enterprise's own PMS logged at tier-3. The reverse drift — a laxer enterprise threshold — is equally exposed, and worse under Article 73's serious-incident reporting obligation.

**(d) Horizon-scanning.** Are emerging failure modes surfacing in the corpora that the enterprise threat model does not yet name? Agentic tool-misuse patterns, GPAI hallucination in high-stakes tool chains, prompt-injection across enterprise trust boundaries, deepfake-augmented social engineering targeting AI systems, and model-supply-chain compromise via third-party fine-tune datasets are all failure modes the field has documented and whose incidence in the corpora precedes their appearance in most enterprise event stores by months to years. Horizon-scanning is how the enterprise closes that lag.

## The three calibration activities the architect designs into the PMS cadence

The architect designs three named, calendared, artefact-producing activities that bind the corpora to the enterprise PMS. Each activity has a scheduled cadence, an assigned role, an artefact template, and downstream routing.

### (i) Periodic taxonomy-coverage review

Quarterly for tier-3-and-above enterprises (as defined by the mod-105 AIMS scope statement); semi-annually for smaller footprints. The `ai-governance-analyst` (level 15) walks the corpora deltas since the previous review — new AIID incidents indexed, new OECD.AI entries, MIT Repository updates — and produces a coverage-finding record for each corpus category that does not map cleanly onto an existing enterprise taxonomy category. Coverage findings that constitute a taxonomy gap are proposed as amendments and routed per mod-106 chapter 07's amendment process; the `ai-risk-engineer` (level 25) is the seat that formally proposes the amendment. The `head-of-ai-governance` (level 60) ratifies material amendments.

The review's discipline is *comprehensiveness on the delta*, not exhaustive re-review of the whole corpus. The analyst walks the window's new entries; the review artefact records what was walked and what was not, so third-line internal audit (mod-107 chapter 04) can sample against the stated scope.

### (ii) Severity calibration review

Semi-annually. The analyst samples a stratified set of enterprise incidents from the PMS event store (chapter 01) and a stratified set of comparable public incidents from the corpora, and measures severity-classification agreement between the enterprise's classifier and the corpus's classification. Systematic drift — the enterprise consistently classifying an incident type as tier-3 that AIID or OECD.AI consistently treats as serious — triggers a revision to the classification guidance rather than a per-incident re-classification. This is the calibration analogue of the mod-106 chapter 06 rater-agreement discipline, applied against an external reference rather than a second internal rater.

Where the enterprise disagrees with a corpus classification and elects to hold its own line, the disagreement is *documented in the review artefact with the defence* — invariant iii below. Silent divergence is the failure mode.

### (iii) Near-miss-pattern review

Quarterly. The analyst builds the enterprise near-miss library from the chapter 01 near-miss discipline's outputs over the window, and cross-references the patterns against the public corpora's near-miss and precursor-incident content. Patterns present in the public corpora that the enterprise near-miss library also shows are candidates for new detection signals (routed to the chapter 05 SOC handoff) and for new controls (routed to mod-102 as control-library amendments). Patterns present in the public corpora that the enterprise near-miss library does *not* show are candidates for new detection candidates the enterprise should build before an internal incident forces the issue.

Where a near-miss pattern suggests a new evaluation is warranted, the `ai-evaluation-engineer` (peer, level 35) is consulted through the mod-107 chapter 06 coordination contract. The architect does not author the evaluation; the architect designs the routing.

## Role separation

The calibration workflow is designed to be lightweight and to keep the level-50 architect out of the mechanical work. The seat assignments are:

- **`senior-ai-governance-architect` (level 50, this role)** — designs the calibration cadence, the artefact templates, the amendment-routing pathway, and the review scope; does not perform the review.
- **`ai-governance-analyst` (level 15)** — performs the mechanical corpus-review work: the walk of deltas, the sampling, the coding of coverage findings, the tabulation of severity comparisons.
- **`ai-risk-engineer` (level 25)** — formally proposes taxonomy amendments arising from the review, per mod-106 chapter 07.
- **`ai-evaluation-engineer` (peer, level 35)** — consulted where a near-miss pattern suggests a new evaluation is warranted; coordinates through the mod-107 chapter 06 contract.
- **`head-of-ai-governance` (level 60)** — ratifies material amendments to the enterprise taxonomy, the control library, or the PMS shape that arise from the calibration workflow's findings.

The calibration workflow is not a solo activity of the architect and not an audit exercise. It is a governance-office production process the architect designs and hands to the analyst seat to run.

## The four invariants

**Invariant i — external calibration cadence is scheduled, named, and calendared.** The review dates are in the AIMS calendar (mod-105 chapter 07). The analyst assignment is named. The review artefact template is pre-authored. *Detection test:* pick a random quarter in the last two years and ask the governance office to produce the review artefact for that quarter; a missing artefact for a quarter that fell inside the scheduled cadence is an invariant breach.

**Invariant ii — findings feed back into taxonomy amendments, control-library updates, and PMS-monitor changes — not into a report that dies in a shared drive.** Every review artefact carries downstream references: which findings became amendment tickets, which became detection candidates, which became control-library proposals. A review that produces no downstream artefacts is a red flag. *Detection test:* the previous four reviews are traced forward; the count of downstream artefacts produced per review is non-zero for each.

**Invariant iii — the calibration uses corpora at their published title / classification — no enterprise re-interpretation.** If the enterprise disagrees with a corpus classification, the disagreement is documented in the review artefact rather than silently changing the label. This preserves the reviewability of the review — the third-line auditor or the certification body can compare the enterprise's classification against the corpus's directly, and evaluate the enterprise's documented rationale for any divergence. *Detection test:* the review artefact does not contain re-labelled corpus entries; disagreements are logged in a `disagreements` section with a defence.

**Invariant iv — the review artefact is versioned and citable.** The artefact carries a stable identifier, a version number, and a persistent location addressable by third-line audit. The enterprise's taxonomy and control-library evolution can be traced back to the specific corpus deltas that drove it. *Detection test:* pick a recent taxonomy amendment ticket; the ticket cites the review artefact that surfaced the gap; the review artefact resolves and is accessible to internal audit.

## The three failure modes

**Failure mode (a) — calibration-as-report.** The review produces a slide deck. The slide deck is presented to the governance office and archived. No amendment ticket is filed, no detection candidate is proposed, no control is updated. Two years later, the taxonomy is unchanged, the controls are unchanged, the monitors are unchanged, and the corpora have documented three new categories the enterprise does not recognise. The internal audit sample surfaces the pattern, and the third-line finding is systemic. *Design fix:* invariant ii is the specific guard. The review artefact template *requires* a downstream-references section; a review with an empty section is an operational defect that the governance office's own review-of-reviews process catches.

**Failure mode (b) — anecdotal adoption.** The analyst latches onto a single interesting public incident as a driver for a new taxonomy category or new control, without measuring corpus-scale coverage. Over cycles, the taxonomy accretes single-incident-triggered categories that do not compose — MECE breaks (mod-106 invariant 2), category count grows, calibration burden climbs, and the taxonomy degenerates toward the mod-106 chapter 01 failure-mode-2 shape (imported-verbatim proliferation, arrived at by drift rather than by initial import). *Design fix:* the amendment-routing process (mod-106 chapter 07) requires a corpus-scale coverage argument for any proposed category, not just a single-incident precedent. The architect's review of amendment proposals is the checkpoint.

**Failure mode (c) — ignoring the corpora entirely because "our footprint is different".** The classic enterprise defence for not doing horizon-scanning: "we are a healthcare enterprise; consumer-chatbot incidents in AIID are not our concern." Rebuttal: even sector-specific footprints benefit from cross-sector calibration because attack techniques (prompt-injection, model-extraction, training-data poisoning, supply-chain compromise) travel across sectors far more freely than harms do. A prompt-injection technique documented against a consumer chatbot generalises to a clinical-decision-support agent that shares a frontier-model backbone with the chatbot; a training-data poisoning technique documented against an open-source vision model generalises to any downstream fine-tune of that model. *Design fix:* the calibration cadence is *mandatory in the AIMS scope statement*, not optional; sector-specific corpora (mod-113) *supplement* the cross-sector corpora rather than replacing them.

## The intake schema for a corpus-review artefact

The review artefact is the workflow's authoritative output and the third-line's sampling target. The architect authors the template; the analyst populates it each review cycle.

```yaml
corpus_review:
  id: PMS-CAL-2027-Q3
  scope: [ aiid, oecd_ai, mit_risk_repository ]
  window:
    from: 2027-04-01
    to: 2027-06-30
  reviewer: analyst-<seat>
  approver: head-of-ai-governance
  taxonomy_coverage_findings:
    - id: TCF-2027-Q3-01
      corpus_category: <corpus label>
      enterprise_taxonomy_status: gap | partial | covered
      proposed_amendment: <ref to mod-106 ch 07 amendment ticket, or none>
  severity_calibration_findings:
    - id: SCF-2027-Q3-01
      enterprise_incident_sample_id: INC-2027-...
      comparable_corpus_case: <ref>
      severity_delta: <e.g. enterprise tier-3, corpus serious>
      resolution: <re-classify | adjust-guidance | no-action-with-defence>
  near_miss_findings:
    - id: NMF-2027-Q3-01
      pattern: <description>
      proposed_detection_candidate: <ref to ch 05 SOC handoff>
      proposed_control_candidate: <ref to mod-102 ticket>
  disagreements:
    - id: DIS-2027-Q3-01
      corpus_label: <as published>
      enterprise_position: <as held>
      defence: <rationale, with references>
  version: 1.0
```

The template is intentionally shallow. Depth lives in the amendment tickets, the detection-candidate handoffs, and the control-library proposals the review produces — not in the review artefact itself. The review artefact is a routing document with a stable identifier; the substance travels downstream.

## Connection outward

The calibration workflow composes with the rest of the enterprise governance architecture at three named points:

- **mod-111 (the GRC-for-AI platform)** hosts the calibration workflow's inputs (the corpus-review templates, the amendment-ticket queues) and outputs (the versioned review artefacts, the downstream references). The workflow's automation lives on the GRC platform's process engine; the platform is where the analyst does the work.
- **mod-113 (sector blueprints)** determines the sector-specific corpus weighting. Healthcare footprints weight FDA MAUDE-style adverse-event corpora alongside the cross-sector AI corpora. Finance footprints weight sector loss-event databases such as ORX. Industrial-control footprints weight ICS-CERT and CISA advisories. The cross-sector AI corpora (AIID, OECD.AI, MIT Repository) *complement* but do not *replace* the sector corpora; the sector blueprint fixes the composition.
- **mod-107 chapter 04 (third-line internal audit)** samples the calibration artefacts to confirm the enterprise is actually doing this. A sample cycle in which the auditor cannot produce recent review artefacts, cannot trace amendments back to the review that surfaced them, or finds systematic empty downstream-references sections is a systemic third-line finding.

## Summary

External incident corpora — the AI Incident Database, the OECD.AI Incidents Monitor, and the MIT AI Risk Repository, supplemented by adjacent references (Partnership on AI, CSET, the wider OECD AI Policy Observatory, and sector-specific corpora per mod-113) — are calibration inputs to the enterprise post-market surveillance architecture, not replacements for it. The architect designs three calibration activities into the PMS cadence: quarterly taxonomy-coverage review, semi-annual severity calibration, and quarterly near-miss-pattern review. The mechanical work belongs to the `ai-governance-analyst` (level 15); amendment proposals belong to the `ai-risk-engineer` (level 25); the `ai-evaluation-engineer` (peer, level 35) is consulted where near-miss patterns warrant new evaluations; the `head-of-ai-governance` (level 60) ratifies material amendments. Four invariants hold — calibration is calendared, findings feed downstream, corpora are used at published classification, artefacts are versioned and citable. Three failure modes are designed against — calibration-as-report, anecdotal adoption, and ignoring the corpora entirely. The review artefact is a shallow routing document with a stable identifier; substance travels downstream into taxonomy amendments (mod-106 chapter 07), control-library updates (mod-102), detection candidates (chapter 05 SOC handoff), and PMS-monitor changes (chapter 02). Third-line internal audit (mod-107 chapter 04) samples the artefacts to confirm the workflow is real. The GRC-for-AI platform (mod-111) hosts the workflow; the sector blueprints (mod-113) fix the corpus weighting. The output is an enterprise PMS that is externally calibrated on a cadence a certification body, a sector regulator, and the enterprise's own board can each rely on.
