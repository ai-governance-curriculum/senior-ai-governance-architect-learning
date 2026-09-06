# exercise-05: CEN-CENELEC JTC 21 alignment planning

**Estimated effort:** 2 hours

## Objective

Produce **the CEN-CENELEC JTC 21 alignment plan** for the enterprise — the mapping from the harmonised-standards programme to the enterprise's existing control library and reconciliation architecture, the *evidence-mode* extension that lets a control produce either direct-evidence or standards-conformance evidence, the timeline of anticipated standards-publication events with the pre-work each triggers, and the near-term ISO/IEC investment posture that positions the enterprise for the transition without over-committing to a moving target.

The point of the drill is that the JTC 21 pathway is *simplification through standards*, and the reconciliation architecture must be able to *absorb* the transition without rework. If the enterprise's schema (exercise-04) is right, adding EN ISO/IEC 42001 as an alternative discharge route for the Article 17 QMS obligation is an evidence-mode extension and a new obligation record; it is not a control-authoring sprint. This exercise makes that concrete by walking the transition end-to-end for one worked standard.

The deliverable is what the level-50 architect would present to the head-of-AI-governance and the CFO at the annual planning cycle to justify (a) sustaining investment in ISO/IEC 42001 and 23894, (b) funding a JTC 21 watch, (c) *not* pre-emptively switching to standards-route evidence, and (d) reserving budget for the notified-body relationship where the enterprise's product mix requires it.

## Prerequisites

- Chapter [`08-cen-cenelec-jtc-21-and-the-future-state.md`](../08-cen-cenelec-jtc-21-and-the-future-state.md) read (presumption of conformity, the JTC 21 work programme, the transition path, the failure modes).
- Chapter [`07-designing-the-reconciliation-architecture.md`](../07-designing-the-reconciliation-architecture.md) read — the evidence-mode extension and the conditional-supersession pattern live in the architecture.
- Chapter [`02-reading-the-eu-ai-act-as-architectural-input.md`](../02-reading-the-eu-ai-act-as-architectural-input.md) read — the essential requirements the harmonised standards discharge live in Articles 8-15 and 17.
- Exercise [`exercise-04-reconciliation-architecture-schema.md`](exercise-04-reconciliation-architecture-schema.md) completed — this exercise adds to the register you already populated.
- Primary sources: CEN-CENELEC JTC 21 work-programme page; European Commission Standardisation Request in support of the EU AI Act (M/593 draft and any later revisions); ISO/IEC 42001, ISO/IEC 23894, ISO/IEC 24029, ISO/IEC 42005, ISO/IEC 42006 (paywalled — verifiable against the ISO catalogue for titles and publication years). Links live in [`../resources.md`](../resources.md). Because JTC 21 publication status changes quickly, `<!-- needs-research: ... -->` markers are expected.

## Scenario

Continue with the scenario you carried through exercise-04 (Northbrook Financial Services or Meridian AI Assistants). The enterprise's EU footprint includes at least one *high-risk provider* obligation set (Meridian's EU-marketed GenAI product is not automatically high-risk but has an Article-50 rendering; Northbrook Career Match is a Category-4 Annex III system if the enterprise deploys it to EU offices — pick one product, state your reasoning about its EU AI Act classification, and proceed).

Assume:

- The enterprise carries a documented AIMS aligned to ISO/IEC 42001 (mod-105's remit — assume it exists as a working investment).
- The risk-management approach is aligned to ISO/IEC 23894.
- The enterprise's reconciliation architecture from exercise-04 is in place — obligation register, applicability filter with an `evidence_mode` dimension, evidence contract with per-obligation renderings.
- The enterprise has *no* current notified-body relationship (Meridian would need one if a product tier moves under Annex I harmonised legislation; Northbrook does not — pick honestly for your scenario).
- The finance function is asking for a three-year JTC 21 posture for budgeting.

## Deliverables

1. **`jtc-21-work-programme-map.md`** — the mapping from JTC 21 work items to the enterprise's control library and to the essential-requirement obligation records they discharge.
2. **`evidence-mode-extension.yaml`** — the schema and applicability-filter extension that carries `evidence_mode` on affected controls.
3. **`transition-timeline.md`** — the three-year timeline of anticipated JTC 21 publication events and the pre-work each triggers.
4. **`iso-iec-investment-posture.md`** — the annual investment plan for the ISO/IEC anchors JTC 21 adopts.
5. **`review-brief.md`** — a one-page brief.

## Requirements

### `jtc-21-work-programme-map.md`

For each JTC 21 work item named in chapter 8 (AI risk management, AI trustworthiness framework, quality-of-AI-systems and QMS, bias mitigation, trustworthiness characterisation, cybersecurity of AI systems, AI system logging, conformity assessment of AI systems), produce a section with:

- **Work-item identifier and status** — as best you can verify from the JTC 21 work-programme page. Mark unverified specifics with `<!-- needs-research: ... -->`. Distinguish drafts (WI, prEN, prEN ISO/IEC) from adopted-and-referenced-in-OJEU standards.
- **Underlying essential requirement(s)** — which EU AI Act article(s) the standard is aimed at (Articles 9-15 primarily, Article 17 for QMS, potentially Articles 10 / 15 for data governance and cybersecurity). Reference the obligation-record ids you produced in exercise-04 where they exist.
- **ISO/IEC adoption relationship** — is the standard adoption-with-modification of an ISO/IEC standard (e.g. EN ISO/IEC 42001), a JTC-21-native standard, or a hybrid? State the ISO/IEC parent where applicable and note whether the enterprise's existing ISO/IEC investment carries forward.
- **Enterprise control-library home** — which `AIC-*` families the standard covers (usually multiple; the standard's shape may span two or three families).
- **Presumption-of-conformity value** — for the enterprise's specific product, what fraction of the essential-requirement obligation does the standard actually cover? This is a judgment call; state your reasoning. A standard that covers 80% of an essential requirement still leaves 20% of direct-evidence work.
- **Deployer coverage** — does the standard extend to deployer obligations, or only provider obligations? (Most JTC 21 standards target provider obligations; state where the deployer gap sits and how the reconciliation architecture handles it.)

Below the per-work-item sections, produce a *coverage matrix* — rows are enterprise controls in scope of the EU AI Act, columns are JTC 21 work items, cells are `full` / `partial` / `none` presumption-of-conformity coverage. This matrix is what the CFO looks at to decide the standards-route budget.

### `evidence-mode-extension.yaml`

Extend the applicability-filter vocabulary from exercise-04 with the `evidence_mode` dimension. Include:

- **The dimension definition** — `evidence_mode` values are `direct_evidence_route`, `standards_route`, and (optional) `hybrid`.
- **The per-control default** — most controls default to `direct_evidence_route` while the standards programme is not yet published; specific controls (particularly those covered by EN ISO/IEC 42001, which is already published, and EN ISO/IEC 23894) may default to `standards_route` where the standard is referenced in OJEU.
- **The per-system override** — the applicability filter must be able to pick a mode per system (a system undergoing notified-body third-party assessment for Annex I harmonised legislation may require standards-route; a system under Annex III internal-conformity assessment may reasonably use hybrid).
- **The evidence-contract rendering per mode** — extend the `per_obligation_renderings` block from exercise-04 with `evidence_mode`-conditional shape (a `standards_route` rendering names the standard version, the conformance-attestation shape, the notified-body identifier where applicable; a `direct_evidence_route` rendering names the per-obligation artefacts as before).
- **The conditional-supersession expression** — the chapter-8 pattern where the standards-route obligation record `supersedes` the essential-requirement obligation record *conditionally*. Encode the conditionality (roughly: `applies_when: evidence_mode == standards_route AND standard_referenced_in_ojeu == true`).

### `transition-timeline.md`

Produce a three-year Gantt-style timeline (a table is fine — a chart is unnecessary) for each named JTC 21 work item, showing:

- **Current status** — WI / prEN / formal-vote / published / OJEU-referenced (mark `<!-- needs-research: ... -->` on any specifics you cannot verify).
- **Anticipated progression** — the milestones you expect over the three-year window and their *rough* dates. Use quarters, not exact dates.
- **Pre-work triggered at each milestone** — for each anticipated milestone, what the enterprise's ai-governance-analyst and architect team should do. Draft the evidence-mode rendering during prEN; author the conditional-supersession obligation record during formal vote; migrate the applicability filter's default when OJEU-referenced; retire the direct-evidence rendering (if elected) after a stated hold period.
- **Enterprise resource cost per milestone** — a rough sizing (person-weeks). This is what the CFO uses.

Below the table, capture the *contingency plan* — what happens if a standard's publication slips six months (very likely for several items), what happens if a standard is published *narrower* than expected (also likely; the essential-requirement obligation record remains authoritative for the uncovered portion), and what happens if the enterprise's product moves into a scope requiring notified-body assessment mid-cycle.

### `iso-iec-investment-posture.md`

Produce the three-year investment plan for the ISO/IEC anchor family JTC 21 adopts. For each of ISO/IEC 42001 (AIMS), ISO/IEC 23894 (risk management), ISO/IEC 24029 (robustness), ISO/IEC 42005 (impact assessment), and ISO/IEC 42006 (AIMS certification requirements):

- **Current enterprise posture** — adopted / partially adopted / on-roadmap / not on-roadmap. State the reason for the posture.
- **Recommended posture over the three-year window** — sustain / expand / initiate. Justify.
- **Forward-compatibility argument** — how the ISO/IEC investment carries into the JTC 21 EN adoption. Reference the specific expected EN standard.
- **Certification decision** — for ISO/IEC 42001, whether the enterprise pursues *certification* (with an accredited certification body) vs *conformance without certification*. Certification is expensive but is the strongest presumption-of-conformity anchor when EN ISO/IEC 42001 is published.
- **Adjacent-standards posture** — SC 42 has a broader family (ISO/IEC TR 24028, ISO/IEC 5259 data-quality series, ISO/IEC 12792 transparency, ISO/IEC 8183 data-lifecycle framework, and others — verify against the SC 42 catalogue and mark `<!-- needs-research: ... -->` where unverified). State whether the enterprise engages any of these.

Then produce a *three-year budget line* — rough person-weeks and external-spend estimates for the ISO/IEC investments and the JTC 21 watch. The CFO does not need dollar precision at this stage; they need a coherent posture.

### `review-brief.md`

One page. Must contain:

- **The two most consequential recommendations** — usually a combination of (i) sustain / expand a specific ISO/IEC investment, (ii) commit / defer the ISO/IEC 42001 certification decision, (iii) reserve / defer notified-body relationship budget.
- **The single "do not do this yet" recommendation** — usually *do not switch to standards-route evidence prematurely*. Anchor the recommendation in chapter 8's failure mode 2 (treating standards as regulation) and the reality that pre-OJEU drafts change materially.
- **The *timing* dependency** — the milestone in the transition timeline that most changes the plan (usually the OJEU reference of EN ISO/IEC 42001 or of the JTC 21 risk-management standard). State what triggers a plan revision.
- **The variant-and-jurisdiction reminder** — the paragraph reminding the head-of-AI-governance that JTC 21 covers the EU slice only; the reconciliation architecture continues to carry per-jurisdiction obligations for Colorado, NYC, Korea, China, Brazil, and the rest of the international patchwork (where the enterprise operates). Standards route is EU simplification, not global simplification.
- **The Article 56 GPAI code-of-practice track** — one paragraph on how the Article 56 code-of-practice route (chapter 2) sits alongside the JTC 21 standards route for GPAI providers, and whether the enterprise elects to be an Article 56 signatory. For non-GPAI-provider enterprises this paragraph can note that the track is not applicable and move on.

## Starter guidance

- **Do not commit to standards-route evidence for a standard that is not yet OJEU-referenced.** The failure mode is real: prEN drafts change materially between prEN and published. The pre-work is *drafting* the rendering, not deploying it.
- **Do not ignore ISO/IEC 42001 certification just because it is expensive.** Where the enterprise's EU footprint includes high-risk provider obligations at scale, certification is the single strongest presumption-of-conformity investment. Justify the deferral where you defer; do not default to no.
- **Notified-body relationships are multi-quarter to establish.** If any of the enterprise's products may move into Annex I harmonised legislation scope in the three-year window (medical devices, machinery, radio equipment, in-vitro diagnostics, and others — see EU AI Act Annex I), the notified-body relationship needs to be started *now*, not on demand. Chapter 8 flags this explicitly.
- **The Article 56 GPAI code-of-practice route is not the same as the JTC 21 standards route.** For GPAI providers, both apply. Do not conflate them.
- **The deployer gap is real.** JTC 21 covers provider obligations primarily. Where the enterprise plays a deployer role (Northbrook does; Meridian does for its own use of third-party models), the standards route does not simplify the deployer obligations. State the gap.
- **Mark unverified specifics with `<!-- needs-research: ... -->`.** The JTC 21 work-programme page changes quarterly; the Commission Standardisation Request has been revised; the ISO/IEC catalogue is authoritative for titles and years but paywalled for text. Do not invent identifiers or publication years.

## Acceptance criteria

- [ ] Every JTC 21 work item named in chapter 8 has a section in the work-programme-map with all six fields populated.
- [ ] The coverage matrix names enterprise controls (not just work items) and cell values are one of `full`, `partial`, `none` with no `TBD`s.
- [ ] The `evidence_mode` dimension is added to the applicability-filter vocabulary with default values, per-system override semantics, and the conditional-supersession expression.
- [ ] The three-year transition timeline covers each work item with milestone-and-pre-work-and-cost per stage.
- [ ] The ISO/IEC investment posture treats each of 42001, 23894, 24029, 42005, 42006 explicitly and lands on a sustain / expand / initiate recommendation.
- [ ] The ISO/IEC 42001 *certification decision* is stated explicitly (pursue / defer / abstain) with reasons.
- [ ] The notified-body posture is stated explicitly (initiate now / initiate on trigger / not applicable) with reasons.
- [ ] The review brief includes the *do not do this yet* paragraph on premature standards-route switching.
- [ ] The variant-and-jurisdiction paragraph reminds the reader that JTC 21 covers the EU slice only.
- [ ] Every regulatory or standards-programme fact is either verifiable against the primary source or marked `<!-- needs-research: ... -->`.
- [ ] The three-year budget line names person-weeks (or an equivalent effort unit) — no bare "sustain investment" statements.

## Stretch goals

- **Author the *migration playbook* for an OJEU-reference event** — the step-by-step procedure the ai-governance-analyst executes when a JTC 21 standard is referenced in OJEU: create the new obligation record, update the affected controls' `evidence_mode` defaults, produce the standards-conformance-attestation rendering, retire the direct-evidence rendering after the hold period. Half a page.
- **Extend the coverage matrix with a *non-EU column set*** — showing that the ISO/IEC investment carries into NIST AI RMF crosswalk (US), Singapore AI Verify (SG), and any other framework that recognises the ISO/IEC anchor. Half a page. Reference chapter 8's parallel non-EU tracks.
- **Sketch the *rebuttal-of-presumption playbook*** — what the enterprise does if a supervisory authority rebuts presumption for a specific system despite the enterprise's standards-route attestation. Which direct-evidence pathway does the enterprise fall back to? Chapter 8 flags this as a failure-mode risk; give the enterprise a plan.
- **Draft the *sponsorship note*** for the enterprise's AI governance board on whether to actively participate in JTC 21 (as an observer, as a nominated national-body expert, or through an industry-association route). This is a strategy question the level-50 architect informs but does not decide unilaterally.
- **Compare Article 56 code-of-practice vs JTC 21 standards route** for a GPAI-provider scenario — the presumption-of-conformity mechanism is the same; the drafting process, the maintenance cadence, and the scope differ. Half a page.
