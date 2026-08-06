# exercise-01: EU AI Act Article 72 Enterprise Shape Drill

**Estimated effort:** 3 hours

## Objective

Produce the **enterprise post-market surveillance (PMS) shape** for a specified scenario — the artefact the level-50 architect takes to the AI-accountable executive and the ISO 42001 certification body when either asks *walk me through your post-market monitoring system*. The deliverable is a decision document, a machine-readable schematic, an incident-and-near-miss taxonomy, a regulatory-reporting cadence table, and a deviation-from-appetite alarm registry.

The correctness spine is the six invariants and two failure modes fixed in chapter 01. Every design choice in the artefacts you produce here must be pinnable to one of the invariants (as the choice that enforces it) or to one of the failure modes (as the choice that defends against it). The deliverable set composes downstream: exercise-02 (monitoring-register contract) writes into fields you define here; exercise-04 (Article 73 SOP) triggers on the incident classifications you fix here; exercise-05 (SOC interface) hands off signals into the event store whose schema you commit to here. Get the shape right and the module composes. Get it wrong — collapse near-miss into incident, leave the alarm registry ad-hoc, punt the clock-start convention — and every downstream exercise inherits the flaw.

Draft as a senior architect briefing an apprentice. The deliverables are contractual artefacts, not essays; the requirements below name what must be present, not how to phrase it.

## Prerequisites

- Chapter [`01-eu-ai-act-article-72-and-the-enterprise-post-market-surveillance-shape.md`](../01-eu-ai-act-article-72-and-the-enterprise-post-market-surveillance-shape.md) read once, with the four PMS things, the six invariants, and the two failure modes marked.
- Chapter [`02-monitoring-to-risk-register-contract-at-enterprise-scale.md`](../02-monitoring-to-risk-register-contract-at-enterprise-scale.md) skimmed — you need to know what fields the register-contract exercise will bind to.
- Chapter [`04-article-73-serious-incident-reporting-workflow-design.md`](../04-article-73-serious-incident-reporting-workflow-design.md) skimmed — the incident classifications you fix here trigger that SOP.
- Chapter [`06-external-incident-corpora-as-calibration-references.md`](../06-external-incident-corpora-as-calibration-references.md) skimmed — the taxonomy deliverable maps to AIID / OECD.AI categories.
- The mod-106 risk taxonomy and appetite work — chapters `01`, `02`, `04`, and `07` — because the deviation-from-appetite alarms bind to that taxonomy and those tolerance-table rows.
- The mod-105 chapter `06` (risk-treatment plan) and chapter `09` (CAPA) walk-throughs, because the feedback-loop invariant closes into their artefacts.
- The mod-107 chapter `01` three-lines architecture (the third-line audit-sampling role reads from the PMS store) and chapter `04` (the internal-audit interface the auditability invariant is tested against).
- Access to primary references — EU AI Act Articles 72 and 73, Article 26 for deployer obligations, FDA guidance on AI/ML-enabled device software functions and PCCPs, SR 11-7 and OCC 2011-12, Colorado SB24-205 and its AG-issued rules, NAIC Model Bulletin on the Use of AI Systems by Insurers, AIID and OECD.AI incident catalogues. See [`../resources.md`](../resources.md).

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose PMS shape you are **least familiar with**; that is where the exercise will teach you most. State your choice at the top of the decision document.

- **A US regional bank** (Northbrook Financial-style, per mod-101 exercise-02 and mod-102 exercise-01) with ~40 AI systems: fraud classifiers, document extraction, an internal RAG legal assistant, a customer-facing generative chat, third-party AI SaaS integrations. Colorado + NYC deployments; EU expansion planned. An SR 11-7-aligned MRM programme and an ISO/IEC 27001 ISMS already in place; internal audit function reports to the audit committee.
- **A global healthcare payer / provider** with clinical-decision-support pilots, patient-facing chat, coding automation, and utilisation-management AI. US federal HIPAA scope; EU AI Act relevance for European insurance subsidiaries; multiple US state deployments including Illinois and California. A classical clinical-safety oversight committee reports to the CMO; internal audit reports to the audit committee.
- **A B2B SaaS platform vendor** shipping GenAI-augmented HR-tech capabilities into enterprise customers across the US, UK, EU, and Singapore. Customers include public-sector deployments (subject to OMB M-25-21 shape) and financial-services enterprises (subject to SR 11-7 vendor-review shape). Internal audit is a small function co-sourced with a Big Four provider.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`pms-shape-proposal.md`** — the decision document. The trade-offs live here; the other four artefacts instantiate them.
2. **`pms-schematic-v1.0.0.yaml`** — the machine-readable schematic of the PMS system, mirroring the chapter-01 schematic shape.
3. **`incident-and-near-miss-taxonomy.md`** — the enterprise incident and near-miss classes, bound to the mod-106 risk taxonomy and mapped to public-corpus categories.
4. **`regulatory-reporting-cadence.md`** — the reporting calendar / table across at least three regimes the enterprise faces.
5. **`deviation-from-appetite-alarms.yaml`** — the pre-registered alarm registry binding monitors to mod-106 categories and tolerance-table rows.

## Requirements

### `pms-shape-proposal.md`

The decision document. Every subsection below is required.

- **Scenario declaration.** Name the chosen scenario and the two or three enterprise-specific facts (jurisdictions, provider-vs-deployer split, sectoral overlay) that most constrain the PMS shape.
- **Six-invariant enforcement table.** For each of the six invariants from chapter 01 (I1 single source of truth, I2 near-miss parity, I3 versioned regulatory cadence, I4 pre-registered alarms, I5 feedback closure, I6 end-to-end auditability): name the specific design choice in your architecture that enforces the invariant and the detection test that would surface a violation. The test must be executable by an internal auditor in a working session, not a philosophical assertion.
- **Failure-mode defence.** For each of the two chapter-01 failure modes (three-sources-of-truth divergence; near-miss blindness): name at least one concrete architectural move you have made in your scenario to prevent it. Each move must be tied to a design choice named elsewhere in the document.
- **Owners.** Name the enterprise seat that owns the PMS system's design, the seat that ratifies it, the seats that populate the event store, the seat that triages, the SOC-interface seat, and the third-line audit-sampling role. Use the level-numbered role names from the track's role tree (level-60 head of AI governance, level-50 senior architect, level-35 AI evaluation engineer, level-35 AI infra security, level-25 AI risk engineer, level-15 AI governance analyst). Where the scenario adds a specialist seat (MRM function for the bank; clinical-safety-committee liaison for the healthcare scenario; customer-facing incident-liaison for the B2B SaaS scenario), name it and place it.
- **Inputs.** Enumerate the input streams to the PMS system — observability platforms in the scenario's stack, SOC signals, third-party incident feeds (vendor status pages, frontier-model provider notices), external corpora (AIID, OECD.AI, MIT AI Risk Repository), user reports, internal reports.
- **Stores.** Commit to the PMS event store shape and the raw-retention tier. Name the retention default (the longest applicable regulatory regime), the schema families (`incident`, `near_miss`, `operational_signal`), and the referencing convention linking event records to the raw telemetry.
- **Outputs.** Enumerate the downstream artefact classes the PMS produces — regulatory reporting feeds, risk-register writes (into the mod-106 register via the mod-110 chapter 02 contract), control-library amendments (mod-102), taxonomy amendments (mod-106 chapter 07), CAPA records (mod-105 chapter 09), risk-treatment updates (mod-105 chapter 06), evidence-contract changes (mod-108), and periodic PMS reports to the AI-accountable executive.
- **Cadences.** Name the cadence for each of: regulatory reporting (per regime, in the calendar artefact); deviation-alarm evaluation (streaming for critical, periodic for trend-based); near-miss review; taxonomy-coverage review against external corpora; periodic PMS report to the executive; and PMS-system change control itself.
- **Escalation paths.** For each of at least three escalation types — a suspected serious incident (Article 73-eligible), a deviation-from-appetite breach, an evidence-contract failure surfaced by the PMS — draw the routing from first-alert receiver to closure and to which seat signs the closure.
- **Non-scope.** Name at least three things you deliberately excluded from the PMS system's scope and why. Candidates: the risk register itself (this is mod-106 and is a *reader* of the PMS store, not a component of it); the classical incident-response runbook for enterprise IT incidents (owned by the SOC / CISO and interfaced via the mod-110 chapter 05 handoff, not subsumed here); the third-party governance programme for frontier-model vendors (owned by mod-109 and hands events into the PMS store, but the vendor-management workflow is out of scope); duplicated MRM monitoring for models already under the classical MRM programme.
- **Composition contract for downstream exercises.** A short subsection naming, per downstream exercise (`exercise-02` monitoring-register contract; `exercise-03` observability wiring; `exercise-04` Article 73 SOP; `exercise-05` SOC-handoff design; `exercise-06` corpora-calibration cadence), which fields of your schematic or taxonomy it binds to. This is the seam that lets a peer author the next exercise against your shape.

### `pms-schematic-v1.0.0.yaml`

Machine-readable schematic of the PMS system, mirroring the chapter-01 schematic. Version the file (`version: 1.0.0`) and include, at minimum, blocks for `owner`, `ratifies`, `scope` (provider systems, deployer systems, internal systems), `inputs`, `stores`, `outputs`, `cadences`, `coordinating_roles`, `invariants` (all six, each with an identifier the other artefacts can reference), and a `change_control` block naming who ratifies schematic versions and how. Every identifier in the schematic (invariant IDs, store IDs, input-stream IDs) must be stable — the taxonomy, cadence table, and alarm registry will cite them.

### `incident-and-near-miss-taxonomy.md`

At least **ten classes** total, with a defensible **realised-vs-near-miss split** — i.e. the taxonomy must specify, per class, whether the class captures realised harms, near-miss precursors, or both, and how a given event is assigned. Every class must carry:

- A class identifier and short definition.
- A binding to a mod-106 risk-taxonomy category (cite by identifier from the mod-106 taxonomy you or a peer authored under mod-106 exercise-01).
- A binding to at least one external-corpus category (AIID category and/or OECD.AI classification). If the corpus has no adequate category, say so explicitly and cite the gap — do not invent a category and attribute it to the corpus.
- The default severity band and the escalation trigger (which class-plus-severity combinations trip the Article 73 workflow; which trip the CAPA process; which trip a taxonomy-amendment review).
- The near-miss precursor pattern where applicable — for classes that also capture near-misses, name the specific precursor signals the observability or SOC stack would surface.

Do **not** copy the MIT AI Risk Repository categories verbatim (the mod-106 chapter-01 failure mode applies here). The taxonomy is an enterprise-specific composition; cite the source repositories, do not paste them.

### `regulatory-reporting-cadence.md`

A calendar or table naming **at least three regimes** the enterprise faces:

- **EU AI Act Article 72 / Article 73** as the anchor regime (with an explicit provider-vs-deployer note; the Article 73 timeframes belong here, and the enterprise's clock-start convention must be stated).
- **At least one sector regime** relevant to the chosen scenario — FDA post-market and MDR-equivalent workflows for the healthcare scenario; SR 11-7 ongoing monitoring plus OCC 2011-12 supervisory expectations for the bank; the customer-contract SLA / breach-notification obligations plus SR 11-7 vendor-review shape for the B2B SaaS scenario.
- **At least one US state regime where applicable** — Colorado SB24-205 for the bank or the SaaS vendor; state-insurance-commissioner NAIC-Model-Bulletin obligations for the healthcare scenario; NYC Local Law 144 for scenarios with employment-screening AI; Illinois BIPA-adjacent obligations for the healthcare scenario; California CPRA / ADMT rules where applicable.

Per regime, the table must state: the **cycle** (fixed calendar vs event-triggered vs both); the **owner** seat that signs; the **populated fields** in the PMS event store that feed the report; the **clock-start convention** (moment-of-awareness rule — who in the enterprise is the recorder of the moment of awareness, and how the awareness is timestamped in the store); and any **late-report handling** convention (a late report is itself a PMS event per invariant 3).

Every clause number, article number, timeframe, and jurisdiction citation must be verifiable from a primary source or marked `<!-- needs-research: ... -->`. Do not invent Article numbers or hours-to-report figures.

### `deviation-from-appetite-alarms.yaml`

At least **eight pre-registered alarms**, each an entry of the form:

```yaml
alarm:
  id: ALM-<stable-id>
  monitor: <observability-platform monitor identifier or SOC signal identifier>
  binds_to:
    mod_106_category: <taxonomy category id from your mod-106 taxonomy>
    tolerance_table_row: <appetite-table row id>
  threshold:
    metric: <named metric>
    condition: <e.g. "p95 fairness disparity > 0.10 over a 7-day window">
    pre_registration_source: <where in the appetite table the threshold is set>
  routing:
    first_receiver: <seat>
    triage_seat: <seat>
    escalation_path: <ordered list of seats>
    closure_signer: <seat>
  event_class: <incident-and-near-miss-taxonomy.md class id>
  detection_test_for_invariant_4: <how an auditor would verify the pre-registration>
```

The alarm set must span at least three distinct mod-106 categories, must include at least one near-miss-classed alarm (a precursor signal that fires below the incident threshold), and must include at least one alarm that is deliberately *streaming* and at least one that is deliberately *periodic trend-based*. Ad-hoc alarms — the ones a product team set up in Grafana last quarter and never registered — must be **excluded by construction**, and the exclusion convention must be stated at the top of the file.

## Starter guidance

- Draft the decision document first. The schematic, taxonomy, cadence table, and alarm registry each instantiate a trade-off the decision document forced. Working the other order produces internally inconsistent artefacts.
- Do **not** copy the MIT AI Risk Repository verbatim into the incident-and-near-miss taxonomy. The mod-106 chapter-01 failure mode (importing a research taxonomy as an enterprise taxonomy) applies here too; the taxonomy is a composition, not a paste.
- Near-miss capture is where most first-cut PMS proposals fail. Author its schema with the same seriousness as the incident schema — separate class definitions, separate precursor-pattern fields, and an explicit position on how a system with zero near-misses logged is treated (chapter 01 pins it as *under-reported*, not *safe*; commit to that or defend an alternative).
- Regulatory-reporting cadence is where clock-start conventions matter. Specify the **moment-of-awareness recorder role** — the named seat whose action starts the reporting clock — and how the timestamp lands in the event store. A cadence table without a clock-start convention will collapse the first time the enterprise has to defend a late report.
- Pre-registration is the entire point of invariant 4. If your alarm registry can accommodate an alarm whose threshold was not derived from a specific appetite-table row, you have re-invented the ad-hoc alarm pattern; the registry format itself should make ad-hoc entries impossible or explicitly flagged.
- Do not confuse the PMS shape (this module) with the risk register schema (mod-106) or the risk-treatment plan (mod-105 chapter 06). The PMS *references* both and *writes into* both, but is neither. The non-scope section is where you make this explicit; the composition contract subsection is where you show it composes.
- For the healthcare scenario, do not fold the clinical-safety oversight committee into the PMS system. It is a specialised first-line-or-second-line safety body per the mod-107 exercise-01 pattern; the PMS interfaces with it, does not subsume it.

## Acceptance criteria

- [ ] Chosen scenario is stated at the top of `pms-shape-proposal.md` and every artefact is coherent against it.
- [ ] `pms-shape-proposal.md` covers scenario declaration, six-invariant enforcement table, failure-mode defence, owners, inputs, stores, outputs, cadences, escalation paths, non-scope (at least three exclusions with rationale), and composition contract for downstream exercises.
- [ ] Each of the six chapter-01 invariants has a named design choice and a named detection test executable by an internal auditor in a working session.
- [ ] Each of the two chapter-01 failure modes has at least one named architectural defence tied to a specific design choice.
- [ ] `pms-schematic-v1.0.0.yaml` includes stable identifiers for invariants, stores, and input streams that the taxonomy, cadence, and alarm registry artefacts cite.
- [ ] `incident-and-near-miss-taxonomy.md` names at least ten classes with a defensible realised-vs-near-miss split, each bound to a mod-106 category and to at least one external-corpus category (or with the corpus gap explicitly stated).
- [ ] Near-miss classes carry explicit precursor-pattern fields; the position on zero-near-miss systems is stated.
- [ ] `regulatory-reporting-cadence.md` names at least three regimes (EU AI Act Article 72/73 as anchor; at least one sector regime; at least one US state regime where applicable), each with cycle, owner, populated fields, clock-start convention, and late-report handling.
- [ ] The moment-of-awareness recorder role is named and its interaction with the event-store timestamp is specified.
- [ ] `deviation-from-appetite-alarms.yaml` includes at least eight pre-registered alarms spanning at least three mod-106 categories, at least one near-miss-classed alarm, and at least one each of streaming and periodic-trend-based alarms; the exclusion convention for ad-hoc alarms is stated.
- [ ] The composition contract subsection specifies, per downstream exercise (`exercise-02`, `exercise-03`, `exercise-04`, `exercise-05`, `exercise-06`), which fields the exercise binds to.
- [ ] Every unverified citation to an EU AI Act article, an FDA guidance, SR 11-7, OCC 2011-12, Colorado SB24-205, NAIC Model Bulletin, NYC LL144, California ADMT rules, HIPAA, or an AIID/OECD.AI category is marked `<!-- needs-research: ... -->` — no invented article numbers, timeframes, jurisdictions, or public-incident details.

## Stretch goals

- **Cross-jurisdictional cadence overlay.** Produce a second view of `regulatory-reporting-cadence.md` that overlays the EU AI Act Article 73 clock against a US state regime the enterprise faces (Colorado SB24-205, or a state insurance-commissioner cycle for the healthcare scenario) — where do the clocks compete, where do they compose, and which seat resolves the conflict when a single event triggers both.
- **AIID-corpus coverage assessment.** Take a representative sample of AIID incidents (say, twenty) from the last year, map each to your `incident-and-near-miss-taxonomy.md`, and produce a coverage report — what fraction of the sample is cleanly classifiable in your taxonomy, what fraction requires a taxonomy amendment, and what does the coverage gap tell you about the mod-106 taxonomy your alarms bind to.
- **Portfolio-level dashboard mock.** Sketch (as a markdown or a wireframe) the monthly PMS dashboard the AI-accountable executive sees — what four to six numbers, what trend lines, what escalation flags, and where the dashboard reads from (which stores, which cadences). The invariant-1 test is that every number on the dashboard resolves to a single event-store field.
- **Scenario integration with mod-113 sector blueprint.** If the mod-113 sector blueprint for your chosen scenario has been authored, produce a one-page overlay showing which of your PMS design choices are sector-generic and which are sector-specific — the seam where a peer authoring the sector blueprint reads from your shape.
