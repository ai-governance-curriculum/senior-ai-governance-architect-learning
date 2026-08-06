# Evaluating the GRC-for-AI vendor landscape against the reference architecture

## Why this chapter exists

An enterprise procuring a GRC-for-AI platform almost always ends up in the same failure pattern, and the pattern is depressingly easy to fall into. The head of AI governance and the CIO's procurement lead take four or five vendor demos over a two-month window. Each vendor arrives with a curated demo tenant pre-loaded with sample controls, sample risk records, sample evidence, and a compelling narrative arc from intake through to a board-ready dashboard. The demos are impressive in isolation; each vendor's platform is, by the standard the demo is asking to be judged against, the answer. A commercial preference forms — often on the strength of the last demo seen, the polish of a single UI screen, or the enthusiasm of an internal executive who has met the vendor's CEO — and a multi-year contract is signed on the strength of that preference. The reference architecture designed in chapter 01 is, at best, referenced in the RFP and never scored against; at worst, it does not exist yet and is written *after* the vendor is chosen, to justify the choice.

Two years in, the enterprise discovers what the demo did not surface. The workflow layer covers three of the seven flows chapter 03 will name and requires professional-services engagement to author the other four. The RBAC model can express three of the personas chapter 04 will fix and collapses the auditor and second-line reviewer into a single role, breaking segregation of duties. The evidence schema is fixed and does not bend to the mod-108 evidence contract, so evidence artefacts are shaped to the platform's model rather than the enterprise's obligations. The integration surface has API coverage for the two stores the vendor's other customers happen to use and does not terminate on the enterprise's ML platform, its mod-110 observability lake, or its enterprise-GRC system of record — three integrations sold as "roadmap" that never ship on a timeline the enterprise can plan against. The certification body the enterprise selects for ISO/IEC 42001 has never seen the platform in an audit and treats the vendor's control-mapping outputs with the scepticism reserved for unfamiliar artefacts. The contract's exit clause carries no data-portability guarantee in an interoperable shape; the migration cost at renewal is, on inspection, comparable to the cost of the initial procurement.

The level-50 architect exists in part to prevent this. This chapter designs the evaluation matrix that replaces vendor-demo shopping with reference-architecture-scored evaluation. It names the vendor landscape at category level without rating any vendor; it defines the dimensions the matrix uses; it fixes the proof-of-concept versus demo boundary that separates testable claims from marketing claims; it names the two failure modes the matrix is designed against; and it gives the matrix a schematic shape the architect authors *before* any demo is scheduled. The matrix is not a spreadsheet; it is the instrument through which the build-vs-buy decision is made honestly.

## The vendor landscape at category level

Three categories of platform compete for the GRC-for-AI seat in the enterprise stack. Naming them here is orientation to the market's shape, not endorsement of any vendor; the reference architecture is what generalises.

### Purpose-built AI-GRC platforms

These are platforms whose founding thesis is AI governance specifically — designed from a blank sheet around the AI system as the unit of governance, the AI risk register as the primary store, and the AI-specific workflow set (impact assessment, model-card intake, fairness attestation, LLM-quality evidence, red-team finding productionisation) as first-class flows.

- Credo AI `<!-- needs-research: verify Credo AI current product scope, workflow coverage, and named integrations as of authoring date against vendor documentation -->`
- Holistic AI `<!-- needs-research: verify Holistic AI current product scope, workflow coverage, and named integrations as of authoring date against vendor documentation -->`
- ModelOp `<!-- needs-research: verify ModelOp current product scope, workflow coverage, and named integrations as of authoring date against vendor documentation -->`
- Monitaur `<!-- needs-research: verify Monitaur current product scope, workflow coverage, and named integrations as of authoring date against vendor documentation -->`
- Fairly AI `<!-- needs-research: verify Fairly AI current product scope, workflow coverage, and named integrations as of authoring date against vendor documentation -->`
- Enzai `<!-- needs-research: verify Enzai current product scope, workflow coverage, and named integrations as of authoring date against vendor documentation -->`
- Trustible `<!-- needs-research: verify Trustible current product scope, workflow coverage, and named integrations as of authoring date against vendor documentation -->`

The category's strengths tend to be AI-specific workflow depth and pre-canned control-library mappings against the AI standards suite (NIST AI RMF, ISO/IEC 42001, EU AI Act, sector regimes). The category's structural weaknesses tend to be integration surface into the wider enterprise-GRC stack (where enterprise-GRC platforms already hold IT-general-controls, financial-controls, and privacy-controls state) and the audit-body track record — younger platforms have simply been in fewer certification audits.

### Adjacent enterprise-GRC extensions

These are established enterprise-GRC platforms — the incumbents in the Archer / ServiceNow / MetricStream / IBM OpenPages / OneTrust seat — extending into AI governance from an installed base the enterprise already runs for financial, operational, and privacy risk.

- ServiceNow AI Control Tower `<!-- needs-research: verify ServiceNow AI Control Tower current product scope, workflow coverage, and named integrations as of authoring date against vendor documentation -->`
- IBM watsonx.governance `<!-- needs-research: verify IBM watsonx.governance current product scope, workflow coverage, and named integrations as of authoring date against vendor documentation -->`
- OneTrust AI Governance `<!-- needs-research: verify OneTrust AI Governance current product scope, workflow coverage, and named integrations as of authoring date against vendor documentation -->` (positioning depends on whether the enterprise already runs OneTrust for privacy)

The category's strengths tend to be integration into the existing enterprise-GRC stack, mature RBAC and audit-trail primitives, and audit-body familiarity through the incumbent's existing certifications. The category's structural weaknesses tend to be depth on AI-specific workflows and evidence semantics — the AI extension is often a schema overlay on an underlying object model that was built for a different risk domain.

### AI-nexus security / inventory adjacencies — positioning note only

These are platforms whose primary thesis is AI-runtime security, model-inventory discovery, or attack-surface management for AI systems, not GRC. They surface in vendor conversations because they occupy the same market vocabulary and are sometimes mistaken for GRC-for-AI candidates. They are not.

- Cranium `<!-- needs-research: verify Cranium current product scope and positioning as inventory / attack-surface tooling versus GRC platform as of authoring date -->`
- HydroX AI `<!-- needs-research: verify HydroX AI current product scope and positioning versus GRC platform as of authoring date -->`
- Verify AI `<!-- needs-research: verify Verify AI current product scope and positioning versus GRC platform as of authoring date -->`

The positioning note the architect fixes: these platforms are *producers* into the GRC-for-AI system of record (inventory discovery, attack-surface signal, runtime-security detections) and integrate through the same normalisation layer mod-110 chapter 03 designed for observability producers. They are not GRC-for-AI candidates and evaluating them against the same matrix is a category error. Chapter 05 walks the integration positioning; here the matrix is scoped to platforms that plausibly hold the register.

## The proof-of-concept versus demo boundary

Before naming the dimensions, the architect fixes the boundary the matrix scores on. A **demo** is vendor-controlled: the tenant is pre-loaded, the data is synthetic and scoped to what the demo intends to show, the failure modes are hidden by construction, and the workflow is walked in a linear order that avoids the messy transitions. A **proof-of-concept** is enterprise-controlled: the tenant runs on the enterprise's own data (or a representative synthetic scoped by the enterprise), the integrations terminate on the enterprise's real systems (at least a sandbox mirror), the workflows are exercised in the order the enterprise's own operating model runs them, and the failure modes the enterprise cares about are deliberately provoked.

The matrix scores dimensions against proof-of-concept observations, not demo observations. A dimension that has been demonstrated only in a vendor demo does not score above **unverified** in the matrix, no matter how compelling the demo was. This is the discipline the matrix's third invariant fixes below; naming it here is what makes the rest of the chapter's rubric definitions meaningful. A vendor that refuses a bounded, enterprise-controlled proof-of-concept scoped to the matrix's dimensions is a vendor whose entire evaluation stays at **unverified**, and that stance is itself scoring signal.

## The evaluation dimensions

Each dimension gets a subsection. The pattern is definition, why it matters at level 50, how to score it, and a common trap. Weights are illustrative in the schematic below; the architect calibrates weights against enterprise-specific pressure (scale, jurisdictional mix, existing GRC estate, expected AI footprint growth).

### Workflow coverage

**Definition.** The platform's native coverage of the seven flows chapter 03 will fix as the GRC-for-AI workflow spine: intake, impact-assessment, control-testing, evidence-collection, exception-handling, incident-routing, and audit-facing packaging.

**Why it matters at level 50.** Every flow the platform does not cover natively becomes either professional-services custom work at implementation, an external adjacent tool the architect has to integrate and keep in sync, or a manual process an analyst runs alongside the platform. The first two consume budget the enterprise did not price; the third erodes the platform's single-source-of-truth property because state lives outside it. Native coverage of all seven flows is not the same platform decision as extensibility across all seven — the matrix scores both.

**How to score it.** Per flow, one of: **native-first-class** (the flow ships out of the box, is exercised in the proof-of-concept without custom development, and the state model is coherent with the platform's other flows), **native-partial** (the flow ships but requires enterprise-authored templates, forms, or workflow steps to reach the level 50 shape), **extensible** (the flow does not ship but the platform provides a documented extension mechanism the enterprise's proof-of-concept demonstrates working), **absent** (the flow is not covered, and integration with an external tool is the answer the vendor offers). Aggregate the seven per-flow scores into a workflow-coverage band on the matrix.

**Common trap.** Scoring workflow coverage on the basis of a demo that walked the three flows the vendor is strongest at. The seven-flow spine is exhaustive precisely so no vendor's demo can obscure the four flows the demo did not cover.

### RBAC and segregation-of-duties flexibility

**Definition.** The platform's ability to express the persona set chapter 04 will fix — first-line model owner, first-line engineer, second-line assurance reviewer, third-line internal audit, external auditor, architect, executive reader, incident-response — with the segregation-of-duties constraints those personas enforce (a reviewer cannot approve their own review; an author cannot mark their own evidence attested; a validator is independent from the developer under SR 11-7).

**Why it matters at level 50.** A collapsed persona model is not a UX inconvenience; it is a governance defect. If the platform cannot express the reviewer-cannot-approve-own-work invariant, the enterprise either accepts an audit finding on every SR 11-7 examination or builds a compensating manual process outside the platform. Either outcome undermines the reason the platform was bought.

**How to score it.** Test in the proof-of-concept: create the eight personas with the intended permissions; attempt each of the SoD-violating actions (self-approval, self-attestation, validator-writing-development-artefact); confirm the platform blocks each one at the workflow layer, not merely at the UI layer. Score **native SoD** (blocks at the workflow layer with a documented rule surface), **UI-only SoD** (blocks in the UI but the underlying API permits the action), **absent** (the platform relies on procedural discipline rather than enforcement).

**Common trap.** Accepting a vendor's assertion that RBAC is configurable without exercising the SoD invariants against the API surface. Every mature enterprise-GRC platform ships some RBAC; the question the matrix scores is whether the SoD constraints hold at the layer that matters.

### Integration surface

**Definition.** The platform's ability to terminate cleanly on the stores and systems the reference architecture (chapter 01) named: the mod-102 control-library store, the mod-105 AIMS documented-information store, the mod-106 risk register, the mod-108 evidence artefact index, the mod-110 observability lake and normalisation layer, the enterprise ML platform (MLflow, SageMaker Model Registry, Vertex Model Registry, Databricks Unity, or the enterprise's own), the runtime-security tooling (integrated per chapter 05), the enterprise-GRC system of record for IT-general and financial controls, identity and single sign-on, communications, and ticketing. Also scored: the shape of the surface itself — API-first (every UI action has a documented API equivalent), UI-with-API (the UI is primary, the API covers a subset), or UI-only with export.

**Why it matters at level 50.** The reference architecture is only as strong as its integration seams. A platform whose integrations are UI-first cannot participate in the automation the reference architecture assumes — evidence artefacts flowing on schedule from mod-108, observability signals flowing through the normalisation layer from mod-110, incidents flowing through the SOC interface from mod-110 chapter 05. API-first is the invariant; UI-first is a limitation the enterprise must build around and cost against.

**How to score it.** For each named integration target, score **native connector, tested** (the vendor ships a connector, the proof-of-concept exercises it against the enterprise's own instance, latency and error behaviour are observed), **native connector, untested** (the vendor ships a connector, the proof-of-concept did not exercise it — do not score above **unverified**), **API-buildable** (no shipped connector, but the API surface is complete enough that the enterprise can build the connector; estimate the build cost), **absent** (no connector and API insufficient to build one; the integration is on the vendor's roadmap or does not exist). Weight the integrations by criticality — the mod-108 evidence index and the mod-110 normalisation layer are typically the two highest-criticality integrations because chapter 01 named them as the register's authoritative producers.

**Common trap.** Scoring integration surface from the vendor's integration catalogue page. Catalogue entries frequently over-count: an entry may reference a partner-built connector no longer maintained, a beta connector for a version of the target system the enterprise does not run, or a hypothetical integration the vendor plans to build once a customer needs it. Only proof-of-concept-exercised integrations count.

### Evidence-schema flexibility

**Definition.** Whether the platform's evidence model bends to the mod-108 evidence contract, or the enterprise must bend to the platform's evidence model. Specifically: can the platform express the evidence artefact's provenance chain, freshness window, control binding(s), authoring persona, review persona, and hash-based integrity that mod-108 fixed; and does the schema evolve when the enterprise's evidence contract evolves (new evidence types added, new provenance requirements imposed by a new regime)?

**Why it matters at level 50.** The mod-108 evidence contract is the enterprise's obligation, not the platform's. The evidence types the enterprise must produce are determined by the regimes it operates under and the controls it has authored in the mod-102 library; the platform is downstream of those decisions. If the platform's evidence schema is fixed and the enterprise must reshape its evidence to fit, the enterprise has effectively subordinated its evidence contract to a vendor artefact — and the audit body inspects the enterprise's obligations, not the platform's schema.

**How to score it.** In the proof-of-concept, take three evidence artefact shapes from the enterprise's mod-108 catalogue (a fairness attestation, a red-team finding-plus-productionisation record, an LLM-quality evaluation run) and attempt to store, retrieve, and version them in the platform without loss. Score **contract-bending** (the platform's schema accepts the enterprise's shape without adaptation), **adaptable-with-effort** (the platform's schema requires configuration but no core-shape compromise), **schema-imposing** (the enterprise must reshape evidence to fit the platform's fixed model), **partial** (some evidence types fit; others do not).

**Common trap.** Accepting that "the platform has an evidence module" is the same as evidence-schema flexibility. A rigid evidence module is worse than no evidence module, because it obscures the fact that the enterprise's evidence has been reshaped to fit.

### Jurisdiction coverage

**Definition.** The regimes and standards for which the platform ships pre-canned control-library mappings, and — critically — whether the enterprise can bring its own control catalogue (authored in the mod-102 library) without being forced into the vendor's mappings. Target regimes at minimum: EU AI Act, NIST AI Risk Management Framework, ISO/IEC 42001, US SR 11-7 (for regulated model risk), Colorado SB24-205, sector regimes (FDA guidance for AI/ML-enabled medical devices, FINRA and OCC guidance for financial services, HIPAA for health data). `<!-- needs-research: verify current status and specific citation of Colorado SB24-205 and any subsequent amendments -->`

**Why it matters at level 50.** Enterprises operate under jurisdictional overlaps mod-104 already covered — a single AI system may sit under EU AI Act, NIST AI RMF, and a sector regime simultaneously. A platform that ships strong mappings for one regime and nothing for the others forces the enterprise to author the missing mappings itself, which the enterprise is already doing in the mod-102 library. The value the platform offers is either that its mappings *accelerate* the enterprise's authoring, or that its schema *hosts* the enterprise's authoring without imposing the vendor's opinions. Both are valuable; a platform that offers neither is scoring poorly on this dimension.

**How to score it.** Per regime the enterprise operates under: **pre-canned mapping, current-version, verified** (the mapping ships, references the current version of the regime, and the proof-of-concept confirmed the mapping's shape matches the enterprise's mod-102 interpretation on a representative sample), **pre-canned mapping, out-of-date or divergent** (the mapping exists but references a stale version or diverges from the enterprise's interpretation in ways the enterprise has to reconcile), **bring-your-own supported** (no pre-canned mapping but the platform hosts the enterprise's mod-102 catalogue cleanly), **absent** (no coverage and the schema fights the enterprise's catalogue). Weight the regimes by the enterprise's exposure.

**Common trap.** Scoring on the count of regimes the vendor's marketing page lists. A long list of shallow, out-of-date, or divergent mappings is negative signal, not positive.

### Audit-body track record

**Definition.** Which certification bodies, independent assurance providers, and internal-audit functions have seen the platform in a real audit (ISO/IEC 42001 certification, SOC 2 attestations, sector-regulator examinations, third-line internal audit fieldwork) and can speak to its behaviour under audit. This is a proxy for interoperability with third-line and external assurance — a platform every mainstream certification body has audited across several enterprises is a platform whose outputs auditors know how to consume.

**Why it matters at level 50.** The enterprise's own ISO/IEC 42001 certification, its SR 11-7 examinations, and its external independent-assurance engagements are the moments at which the platform's outputs are tested against external scrutiny. A platform that has never been audited is a platform whose outputs the auditor will treat with the scepticism reserved for unfamiliar artefacts — sampling more heavily, requesting more corroborating evidence, extending fieldwork. That translates directly into audit hours the enterprise pays for.

**How to score it.** Ask the vendor for named reference customers who have been through ISO/IEC 42001 certification, SR 11-7 examination, or independent AI assurance engagement with the platform in use, and — with permission — speak to the audit-side counterpart. Score **broad track record** (multiple certification bodies, multiple regimes, multiple enterprises, references speak specifically to audit interoperability), **narrow track record** (one or two certifications observed, references speak to the platform generally but not to audit interoperability), **thin track record** (no independently confirmable audit exposure), **unverified** (vendor claims exist but references are not available for direct conversation).

**Common trap.** Confusing the *vendor's* SOC 2 with the platform's track record in *customer* audits. The vendor's own SOC 2 is table stakes; the question is whether the platform's outputs have survived scrutiny in the customer's audits.

### Deployment model

**Definition.** SaaS multi-tenant, SaaS single-tenant, private-cloud dedicated, or on-premises; data-residency options; customer-managed-key posture for data at rest and in transit; the tenancy shape of any AI-inference the platform performs (for classification, summarisation, or agent-assisted workflows the platform itself embeds).

**Why it matters at level 50.** The GRC-for-AI store holds the enterprise's most sensitive risk state — unmitigated findings, unresolved incidents, pending regulatory submissions, board-committee materials. Deployment-model constraints frequently intersect with the enterprise's data-classification policy, the customer-managed-key requirements the CISO imposes on tier-1 systems, and the data-residency obligations the enterprise's own regulated business units carry. A platform whose only deployment shape is SaaS multi-tenant in a single region is not deployable in enterprises with strong residency obligations, regardless of its other strengths.

**How to score it.** Enumerate the enterprise's non-negotiables (residency regions, CMK requirement, tenancy isolation for tier-1 data) and score the platform's deployment shapes against each. **Meets all non-negotiables, verified in POC** is the top band; **meets all non-negotiables on roadmap** is unverified; **fails a non-negotiable** disqualifies the platform on this dimension regardless of other scores.

**Common trap.** Underestimating the vendor's own embedded AI. Many GRC-for-AI platforms have added assistant features — classification suggestions, evidence summarisation, agent-authored draft narratives — whose inference runs in the vendor's own environment on the customer's data. The deployment-model dimension covers this; the CMK and residency posture must extend to the platform's own AI-inference calls, or the enterprise has silently exported its sensitive risk state.

### Roadmap velocity and vendor viability

**Definition.** The vendor's shipping cadence over the last twelve to twenty-four months (against the vendor's publicly-committed roadmap), the vendor's funding history and runway posture, its customer count and reference density in comparable enterprises, and its absorptive capacity when the enterprise's needs change (does the vendor engineer against enterprise-driven roadmap requests, or wait for market signal to justify the work).

**Why it matters at level 50.** The reference architecture is a multi-year commitment; the vendor selected is a partner for the length of that commitment. A vendor that ships slowly against roadmap, has short runway, or lacks reference density in comparable enterprises is a vendor whose viability at year three of the contract is uncertain — and the migration cost at year three is dominant in the total-cost calculation. Absorptive capacity matters because the enterprise's needs will change: a new regime will emerge, a new evidence type will be required, a new integration target will surface. A vendor that responds to enterprise-driven requests within a reasonable window is a partner; a vendor that does not is a supplier.

**How to score it.** Compare the vendor's last-twelve-months roadmap commitments to what actually shipped; ask reference customers whether their own enterprise-driven requests have landed and on what timeline; review publicly-available funding history for the vendor's runway posture. Score **strong** (velocity matches commitments, references confirm absorptive capacity, funding posture supports multi-year commitment), **moderate**, **weak** (velocity gaps, references cannot confirm enterprise-driven requests land, funding posture uncertain), **concerning** (any one of: significant roadmap slip, references report enterprise requests not landing, funding runway short).

**Common trap.** Weighting recent-funding-round headlines over shipping cadence. A vendor that raised a large round two quarters ago and has since shipped little is not more viable than one that raised less and ships consistently.

### Total cost of ownership

**Definition.** The full multi-year cost of running the platform: list price (per-seat, per-system, per-workflow, or hybrid), integration engineering (internal engineering effort to stand up the connectors), professional-services engagement (vendor-side implementation and ongoing customisation), ongoing platform maintenance (internal effort to keep the platform in step with enterprise change), and — the term most often omitted from the initial calculation — migration cost at contract end (the effort to extract the enterprise's data in a usable shape and populate a successor platform, if the vendor is not renewed).

**Why it matters at level 50.** List price is a red herring on this dimension. Two vendors with a 3× list-price difference frequently converge on similar total cost when integration, professional services, and migration are included; the cheaper-on-list vendor may be the more expensive on total when its API surface is thin, its integrations require custom engineering, or its data-portability posture makes migration a multi-quarter project. The architect owns the total-cost calculation; the CFO's procurement lead owns the list-price negotiation. Both are necessary; only the total-cost calculation is decision-quality.

**How to score it.** Build a five-year cost model per vendor covering the five terms above. Explicitly cost the migration-at-end term on the assumption the vendor is not renewed — the cost of extracting the register, the evidence index, the workflow state, the RBAC configuration, and the historical audit trail into a shape a successor platform can ingest. Score against the enterprise's budget envelope; vendors whose five-year total exceeds the envelope disqualify regardless of other strengths, and vendors whose migration-cost-at-end is materially uncertain carry a matrix flag.

**Common trap.** Treating the vendor's initial-year quote as the cost. The initial year is frequently discounted to win the deal; the standing cost lands in year two, and the migration cost lands only when the enterprise chooses not to renew — at which point the negotiating leverage is entirely with the incumbent.

## The evaluation matrix — YAML-shaped schematic

The matrix is a versioned artefact. The architect authors it before any demo is scheduled; it evolves as the reference architecture evolves and as the vendor landscape moves. The shape below is illustrative; enterprises calibrate weights against their own pressure and add dimensions their circumstances require.

```yaml
evaluation_matrix:
  id: GRC-AI-VEM-v2.1
  authored_by: senior-ai-governance-architect
  ratified_by: [ head-of-ai-governance, ciso, cio-procurement-lead ]
  authored_before_vendor_demos: true
  scoring_cadence:
    - trigger: annually as vendors ship
      cadence: Q1
    - trigger: before every contract renewal
      cadence: T-minus-6-months
    - trigger: any material change in the enterprise reference architecture
      cadence: within-one-quarter-of-change
  scoring_bands:
    - band: native-first-class-verified
      meaning: shipped, exercised in POC on enterprise data, meets rubric
      numeric: 5
    - band: native-verified
      meaning: shipped, exercised in POC, meets rubric with configuration
      numeric: 4
    - band: extensible-verified
      meaning: not shipped, but POC demonstrated extension mechanism works
      numeric: 3
    - band: partial-verified
      meaning: covers part of the rubric; the remainder requires workaround
      numeric: 2
    - band: unverified
      meaning: claim not tested in POC, or POC not offered
      numeric: 1
    - band: absent-or-disqualifying
      meaning: does not cover the rubric; on a non-negotiable dimension, disqualifies
      numeric: 0
  dimensions:
    - id: workflow-coverage
      weight: 0.18
      per_flow_rubric:
        - intake
        - impact-assessment
        - control-testing
        - evidence-collection
        - exception-handling
        - incident-routing
        - audit-facing-packaging
      aggregation: weighted-mean-of-per-flow-scores
    - id: rbac-sod-flexibility
      weight: 0.12
      rubric: express-all-eight-personas + block-sod-violations-at-workflow-layer
    - id: integration-surface
      weight: 0.16
      per_target_rubric:
        - mod-102-control-library-store
        - mod-105-aims-documented-information
        - mod-106-risk-register
        - mod-108-evidence-index         # highest criticality
        - mod-110-normalisation-layer    # highest criticality
        - ml-platform-model-registry
        - runtime-security-tooling
        - enterprise-grc-system-of-record
        - identity-sso
        - communications
        - ticketing
      api_shape_bonus: api-first > ui-with-api > ui-only-with-export
    - id: evidence-schema-flexibility
      weight: 0.12
      rubric: contract-bending vs adaptable-with-effort vs schema-imposing
    - id: jurisdiction-coverage
      weight: 0.10
      per_regime_rubric:
        - EU-AI-Act
        - NIST-AI-RMF
        - ISO-IEC-42001
        - SR-11-7
        - Colorado-SB24-205
        - sector-regimes-per-enterprise-exposure
      bring-your-own-catalogue: required
    - id: audit-body-track-record
      weight: 0.08
      rubric: references-confirmable-across-multiple-certification-bodies
    - id: deployment-model
      weight: 0.08
      non_negotiables: [ residency, cmk, tenancy-for-tier-1, cmk-for-embedded-ai-inference ]
    - id: roadmap-velocity-and-viability
      weight: 0.08
      rubric: 12mo-shipped-vs-committed + absorptive-capacity + runway
    - id: total-cost-of-ownership
      weight: 0.08
      five_year_model:
        - list-price
        - integration-engineering
        - professional-services
        - ongoing-maintenance
        - migration-at-contract-end
      migration_cost_uncertainty_flag: true
  disqualifying_conditions:
    - deployment-model fails a non-negotiable
    - five-year-tco exceeds envelope
    - any-dimension score = unverified across all POC attempts (vendor refused POC or POC did not proceed)
```

The matrix is scored per candidate vendor and per configuration option the vendor offers (deployment shape, integration bundle, seat model). The output is not a single ranked list; it is a decision instrument the architect, the head of AI governance, the CISO, and the CIO's procurement lead read together, with the disqualifying conditions applied first and the weighted aggregate applied to what remains.

## Invariants

**Invariant 1 — the matrix is authored before vendor demos.** The dimensions, the weights, the rubrics, and the disqualifying conditions are fixed before the first vendor demo is scheduled. Amendments after demos begin are visible in the version history and are ratified jointly; a demo that reveals a dimension the matrix did not name is a legitimate trigger for amendment, but the amendment happens explicitly, not by silent re-weighting to favour the demo that surfaced it.

**Invariant 2 — every dimension has a testable rubric.** No dimension scores on vibes. Each dimension's subsection defines what the rubric is and how the proof-of-concept exercises it. A dimension whose rubric cannot be operationalised is a dimension that does not belong in the matrix; the architect either sharpens the rubric or removes the dimension.

**Invariant 3 — vendor claims that cannot be tested in a proof-of-concept do not score above unverified.** This is the boundary the earlier section fixed. A vendor's marketing page, its analyst-report placement, its executive references, and its demo footage do not produce scoring bands above **unverified**. Only enterprise-controlled proof-of-concept observations against the enterprise's own systems and data (or a representative sandbox mirror) produce **verified** scoring bands.

## Failure modes

**Failure mode (a) — picking on a demo without a reference-architecture-scored evaluation.** The commercial preference is set by the polish of the last demo seen, an executive relationship, or an analyst-report ranking; the reference architecture is not authored yet or is treated as post-hoc justification. Two years in, the enterprise discovers what the demo did not surface — the missing workflows, the collapsed RBAC, the rigid evidence schema, the thin integration surface, the audit-body unfamiliarity. The migration cost dominates the renewal decision, and the enterprise is locked in to a platform the reference architecture would not have selected. This is the failure mode the matrix's first invariant (author-before-demo) is designed against; the matrix exists precisely so that the demo experience is scored against a shape the architect already committed to.

**Failure mode (b) — treating the matrix as a spreadsheet exercise rather than as a build-vs-buy decision instrument.** The matrix is filled in for three or four vendors, weighted scores are computed, the top-ranked vendor is selected, and the exercise ends. But the matrix's real question is not "which vendor scores highest" — it is "does any vendor score highly enough on enough dimensions that buy is the right decision, or does the pattern of scores tell us build (or partial-build with a specific vendor providing specific dimensions) is the right decision?" A pattern where the top vendor scores well on four dimensions and poorly on five is not a vendor recommendation; it is a signal that the enterprise's requirements outrun the market, and either the enterprise builds (accepting the engineering and maintenance cost), buys with a plan to build against the weak dimensions (accepting the integration and customisation cost), or waits for the market to catch up (accepting the delay cost). The matrix is the instrument through which that conversation happens honestly; used as a scorecard it obscures the real decision.

## Summary

The vendor landscape sorts into three categories — purpose-built AI-GRC platforms, adjacent enterprise-GRC extensions, and AI-nexus security or inventory adjacencies whose positioning is producer-into-the-register rather than register itself. Naming the vendors is orientation only; every vendor claim carries a `needs-research` marker because features, integrations, and roadmaps move faster than lecture chapters do. The matrix scores against nine dimensions — workflow coverage, RBAC and SoD flexibility, integration surface, evidence-schema flexibility, jurisdiction coverage, audit-body track record, deployment model, roadmap velocity and viability, and total cost of ownership — with per-dimension rubrics, illustrative weights, and disqualifying conditions on non-negotiables. The proof-of-concept versus demo boundary is what separates **verified** scoring bands from **unverified** ones; the third invariant holds that vendor claims which cannot be tested in an enterprise-controlled proof-of-concept do not score above **unverified**, no matter how compelling the demo. The two failure modes the matrix is designed against are picking on a demo without a reference-architecture-scored evaluation and treating the matrix as a spreadsheet rather than as a build-vs-buy decision instrument. Chapter 03 designs the workflow layer the matrix's first dimension scores against; chapter 04 designs the RBAC and SoD model the second dimension scores against; chapter 05 designs the integration positioning for the observability and runtime-security adjacencies the third dimension covers; chapters 06 and 07 close the deployment posture and the interoperability seam the eighth and ninth dimensions land on.
