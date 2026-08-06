# Harm categories, capability tiers, and the dependency map

## Why this chapter exists

Chapter 01 argued that the taxonomy is a distinct architectural artefact and named six invariants it must hold. This chapter builds it. The taxonomy has a *shape*: three orthogonal axes that together let any risk in the register be located by three coordinates plus its edges to other risks. The three axes are **harm category** (what kind of harm to whom), **capability tier** (what the system can do that makes the harm reachable), and **deployment context** (who uses the system, in what setting, with what dependencies). Overlaid on the three-axis coordinate system is the **dependency map** — the directed graph of how one risk conditions, escalates, or subsumes another.

This chapter walks each axis in turn, shows how the reference corpora (NIST AI RMF Generative AI Profile categories, ISO/IEC 23894 risk sources, AIRO ontology categories, MIT AI Risk Repository meta-taxonomy, AIID / OECD.AI incident classes) compose into each axis without any single corpus being adopted verbatim, and produces the schema the exercises in this module will populate. The point of the chapter is not to hand you a finished enterprise taxonomy — that is exercise-01. The point is to walk the composition move so the exercise is defensible.

## Axis 1 — harm category

The harm-category axis asks: *what kind of harm to whom?* Every category has a defined **harmed party** (individual / group / organisation / ecosystem / enterprise-itself) and a defined **harm nature** (physical / psychological / financial / rights-based / reputational / operational / systemic). The two together define the category. "Confidential-information-exfiltration" is `harmed_party=enterprise-itself` and `harm_nature=operational-plus-legal`. "Discriminatory-decision-in-employment" is `harmed_party=individual-plus-group` and `harm_nature=rights-based-plus-financial`. "Misinformation-at-scale" is `harmed_party=ecosystem` and `harm_nature=systemic`.

### Composing the harm-category axis from the reference corpora

The enterprise does not invent the harm-category axis from scratch. It composes it from five reference corpora, each contributing a specific view. The architect's job is to lay them side by side, identify the categories that appear in more than one corpus (those are consensus categories the enterprise should carry), identify the categories that appear in only one corpus but that the enterprise's own footprint makes relevant (those are footprint-specific categories the enterprise should carry), and identify the categories that appear only in corpora that do not fit the enterprise's scope (those the enterprise can defensibly decline). The composition move is the same one mod-102 chapter 02 argued for at the control level: *one entry per outcome, multi-parented via crosswalk*.

**NIST AI RMF Generative AI Profile (AI 600-1)** enumerates twelve GenAI-specific risk categories in its section 2 — including CBRN information/capabilities, confabulation, dangerous or violent recommendations, data privacy, environmental impacts, harmful bias and homogenisation, human-AI configuration, information integrity, information security, intellectual property, obscene/degrading/abusive content, and value chain and component integration. Each is defined precisely enough that the enterprise can adopt-or-adapt it. The Profile also cross-references the parent AI RMF's MAP / MEASURE / MANAGE outcomes, so a category adopted from the Profile inherits a lineage to the parent framework. The Profile is *the* single most authoritative starting point for the harm-category axis for a GenAI-heavy enterprise. <!-- needs-research: confirm the exact section numbering and the exhaustive category list from NIST AI 600-1 v1 (July 2024); update if a v1.1 or v2 has been published. -->

**ISO/IEC 23894:2023** *(Guidance on risk management)* enumerates AI-specific risk sources in Annex A / Annex B (guidance on risk sources and objectives specific to AI). It provides definitions that align with the ISO/IEC 22989 vocabulary — a compositional convenience because the enterprise's AIMS (mod-105) already references 22989. The 23894 risk sources tend to be *engineering-shaped* (training-data quality issues, drift, model brittleness, opacity, dependence on external components) and complement the more outcome-shaped NIST GenAI Profile categories.

**AIRO (AI Risk Ontology)** — developed under the VAIR (Vocabulary for AI Risks) project affiliated with the ADAPT SFI Research Centre and released as an open-source RDF/OWL ontology — provides a formal, machine-readable ontology of AI risks. AIRO's contribution is the *structured relationships* between concepts (AI system, risk source, risk, consequence, stakeholder, control, obligation) rather than a novel list of categories. The enterprise adopting AIRO's structure gets a schema for its own taxonomy that is interoperable with other AIRO-conformant tooling. The enterprise adopting AIRO's *category set* verbatim gets 100-plus categories that overlap heavily with the NIST/ISO sources — do not do that; use AIRO for shape, use NIST and ISO for content. <!-- needs-research: verify current AIRO/VAIR publication URL and version; the ontology is periodically updated on the ADAPT Centre pages. -->

**MIT AI Risk Repository** — a meta-analysis project (Slattery et al., 2024, with rolling updates) that reviewed 43 pre-existing AI risk taxonomies and consolidated 777 individual risks into two facet-level taxonomies: a *causal* taxonomy (entity, intent, timing) and a *domain* taxonomy (seven domains and 23 subdomains). The Repository's domains include: (1) Discrimination and toxicity; (2) Privacy and security; (3) Misinformation; (4) Malicious actors and misuse; (5) Human-computer interaction; (6) Socioeconomic and environmental harms; (7) AI system safety, failures, and limitations. The Repository is the *best* published sanity check for coverage — a taxonomy the enterprise proposes can be walked against the Repository's 23 subdomains to verify no important consensus category is missing. It is *not* the taxonomy the enterprise ships, because 23 subdomains × the enterprise's own footprint filter yields the right number, not 23. <!-- needs-research: confirm the current published domain / subdomain counts and any newer release of the MIT AI Risk Repository beyond v1. -->

**AIID (AI Incident Database)** and **OECD.AI Incidents Monitor** provide *incident classifications* — the taxonomy of what has actually happened, drawn from real reported events. AIID's Common Set of AI Incident and Hazard Taxonomies (CSET / GMF taxonomies) and the OECD.AI classification system are the empirical grounding: they tell the enterprise which categories from the theoretical corpora have produced real-world harms and in what proportions. For the enterprise, they are the *incident lineage* — a category in the enterprise taxonomy that maps to an AIID category has a track record; a category that does not may be theoretically important but under-tested. The enterprise's post-market surveillance (mod-110) will bind to AIID / OECD.AI categories anyway, so the enterprise taxonomy must map cleanly to them.

### The composition move

The architect produces a **harm-category composition worksheet** with one row per proposed enterprise category and columns for its parent NIST GenAI Profile category, parent ISO/IEC 23894 risk source(s), AIRO ontology term (if used), MIT Repository subdomain(s), and AIID category (if there is a mapped one). Every enterprise category must be defensible against the worksheet: at minimum one NIST or ISO parent, at minimum one MIT Repository subdomain (to prove coverage against the meta-analysis), and ideally an AIID mapping (to prove incident lineage). Categories that do not map to at least two corpora are candidates for rejection or for filing as *speculative* pending further evidence.

A worked example (partial, for illustration; exercise-01 requires the full sheet):

| Enterprise category | NIST GenAI Profile parent | ISO 23894 risk source | MIT Repository subdomain | AIID mapping |
|---|---|---|---|---|
| `confidential-information-exfiltration-via-extraction-attack` | Information Security (2.9) | Model behaviour under adversarial input | 2.4 System security vulnerabilities and attacks | GMF taxonomy "Model theft / extraction" |
| `synthetic-media-attribution-failure` | Information Integrity (2.7) | Output authenticity | 3.2 Pollution of information ecosystem | Multiple AIID entries under content-authenticity |
| `discriminatory-decision-in-employment-context` | Harmful Bias and Homogenisation (2.6) | Bias in training data / bias in system behaviour | 1.2 Unequal performance across groups | Multiple AIID entries under hiring-discrimination |

<!-- needs-research: verify the exact section numbers referenced in NIST AI 600-1 for each named category; the section numbering above is illustrative and must be checked against the published PDF before shipping. -->

The point of the composition is defensibility. When the head of AI governance asks "why do we carry a `synthetic-media-attribution-failure` category?", the answer is "NIST AI RMF GenAI Profile Information Integrity (section 2.7); MIT Repository subdomain 3.2 Pollution of information ecosystem; a well-documented history of incidents in the AIID; and EU AI Act Article 50 obligations for GenAI outputs bind the enterprise to it if we operate in the EU." That answer takes fifteen seconds and closes the question. The alternative — "the architect thought it was important" — takes fifteen minutes and reopens the question at every subsequent quarterly review.

## Axis 2 — capability tier

The capability-tier axis asks: *what can the system do that makes the harm reachable?* The same harm category attaches very differently to different capability tiers, and the appetite architecture (chapter 03) will attach different tolerances to different tiers. A "confidential-information-exfiltration" risk on a tier-1 read-only classifier is a very different risk shape than the same risk on a tier-4 agent with tool-calling access to internal systems.

### The four attributes that define a capability tier

The architect does not assign a tier by intuition. A tier is a function of four attributes the enterprise scores on each system:

- **Modality access.** What kinds of input does the system consume (text / image / audio / video / structured data / multimodal / real-time sensor)? What kinds of output does it produce? Multimodal input on the input side and open-ended output on the output side expand the attack surface substantially.
- **Action authority.** What can the system do? *Read* (retrieve information for a human to act on) / *recommend* (surface a decision the human accepts or rejects) / *auto-decide* (take an action without human review under normal circumstances) / *auto-act via tools* (invoke external tools, APIs, agents, or physical actuators). Each step up the action-authority ladder unlocks harms the previous step could not reach.
- **Data-access blast radius.** What data can the system see? *Public data only* / *enterprise-non-sensitive* / *enterprise-confidential* / *personal-data* / *special-category-personal-data* / *safety-critical or life-critical data*. The blast radius bounds the exfiltration and misuse harms the system can produce.
- **Autonomy horizon.** How long does the system operate without human interlock? *Per-interaction (chat-turn-scale)* / *per-session (task-scale)* / *scheduled task (batch-scale)* / *continuous-autonomy (agentic loop with no fixed end)*. Longer autonomy horizons increase the exposure window for compounding failures and the difficulty of rollback.

### Tier definitions

Four to five tiers is the standard enterprise shape (frontier labs use similar counts for very different reasons — chapter 05 walks the difference). A defensible enterprise tier scheme:

- **Tier 1 — bounded assist.** Read-only, single-modality, non-sensitive data, per-interaction horizon. Example: an FAQ classifier over public documentation.
- **Tier 2 — recommend-with-review.** Recommends decisions a human reviews before action; may access enterprise-confidential data; per-session horizon. Example: a legal-research RAG assistant over the enterprise's contract corpus.
- **Tier 3 — auto-decide under policy.** Takes actions within a defined policy envelope without per-decision human review, but with post-hoc human oversight; may access personal data; scheduled or per-session horizon. Example: an automated fraud classifier that declines transactions above a threshold with a review queue.
- **Tier 4 — auto-act via tools.** Invokes external tools / APIs / other agents to complete tasks with limited per-action human oversight; may operate autonomously across a session or a batch. Example: a customer-service agent that can issue refunds, update records, and escalate.
- **Tier 5 — continuous agentic autonomy.** Operates in an agentic loop with tool-calling authority, long autonomy horizons, and potential access to safety-critical or life-critical data. Rare in most enterprises today; the reason chapter 05 studies the frontier-lab frameworks.

The tier assignment is a *policy decision* against a rubric, not a numerical index. The rubric is published; the assignment is made per system by the risk engineer under the architect-designed governance process; the applicability filter on control-library entries (mod-102) can key off tier.

### Orthogonality to harm category

Capability tier is orthogonal to harm category — this is invariant 4 from chapter 01. A tier-2 system can produce a "discriminatory-decision" harm; so can a tier-4 system. The category is the same; the appetite tolerance for that category differs by tier (chapter 03); the aggregation weighting differs by tier (chapter 04); the required control depth differs by tier (mod-102's applicability filter). Do not fold the tier into the category — the temptation is real (naming things like "tier-4 hallucination risk" and "tier-2 hallucination risk" as separate categories) and it double-counts the aggregate and inflates the catalog.

## Axis 3 — deployment context

The deployment-context axis carries a small set of coarse attributes that matter for risk shape but that are neither harm categories nor capability attributes. The three attributes that consistently pay their maintenance cost:

- **Sector.** Financial services / healthcare / public sector / education / employment / consumer / critical-infrastructure / defence / general-enterprise. Sector determines which sector regulators apply (SR 11-7 for banking, FDA GMLP for medical devices, EEOC for employment, and so on), which sector-specific harm categories are salient, and often which appetite tolerances are stricter by policy or by law. Mod-113 sector blueprints will bind heavily to this attribute.
- **User population.** Enterprise-internal / enterprise-external-professional / general-consumer / vulnerable-population (children, patients, defendants, welfare recipients). User population conditions the discrimination, safety, and rights-based harm shapes and often triggers stricter regulatory obligations (EU AI Act Annex III scope, COPPA, HIPAA).
- **Deployment jurisdiction(s).** The set of jurisdictions the system operates in. Mod-104's reconciliation architecture reads this attribute; the taxonomy uses it as a filter to decide which categories are *even applicable* to a given system. A category driven by EU AI Act Article 50 does not attach to a system that is not deployed in the EU.

The deployment-context axis is intentionally lightweight. It is the *lens*, not the shape. The heavy shape is in axis 1 (harm category) and axis 2 (capability tier).

## The dependency map

Chapter 01's invariant 5 said the taxonomy is not a flat list. Risks condition risks. The dependency map is a directed graph over the harm-category axis, with edges labelled by kind. Four edge kinds pay for themselves:

- **Causes.** `A causes B` means realising risk `A` mechanically produces risk `B`. Example: `training-data-poisoning` *causes* `discriminatory-decision-in-consequential-context` if the poisoning targeted a protected class's distribution. The aggregation model uses `causes` edges to avoid double-counting — a residual on `B` that is downstream of an unmitigated `A` is not additive to `A`; it is a re-expression.
- **Escalates.** `A escalates B` means realising `A` makes `B` more likely or more consequential without mechanically producing it. Example: `prompt-injection-succeeded` *escalates* `confidential-information-exfiltration` on an agent with tool-calling authority. The aggregation model uses `escalates` edges to compute conditional appetite consumption — a system with an unmitigated `A` residual carries a heavier `B` residual than the raw score suggests.
- **Subsumes.** `A subsumes B` means `A` is a broader category and `B` is a special case. The taxonomy prefers not to have subsumption edges (they violate MECE — invariant 2) but occasionally they are unavoidable across granularity levels. When they exist, the aggregation model rolls `B` up into `A` at the portfolio level to avoid double-counting.
- **Mitigation-conflicts.** `A mitigation-conflicts-with B` means the control mix that reduces `A` tends to raise `B`. Example: adding a heavy content-filter to reduce `harmful-content-generation` tends to raise `false-refusal-of-legitimate-request` (a category the enterprise may or may not carry, depending on whether false-refusal is a harm it recognises). The mitigation-conflict edges are the taxonomy's warning system to the risk engineer that reducing one residual will move another.

The dependency map is authored once (per taxonomy version) and reviewed on every version change. The edges are enumerated in the taxonomy release notes so the risk engineer's aggregation code can consume them (chapter 04 shows the shape). The graph should stay small — dozens of edges, not thousands. If the dependency map explodes, the taxonomy has probably violated invariant 2 and needs to be re-cut.

## A worked taxonomy schema fragment

The taxonomy is best represented as a versioned YAML or JSON file that mod-108's evidence architecture can index and mod-111's GRC platform can consume. A schema fragment for one category:

```yaml
taxonomy_version: 1.2.0
categories:
  - id: RSK-CAT-042
    name: confidential-information-exfiltration-via-extraction-attack
    definition: >
      An adversary reconstructs training data, model weights, or system-prompt content
      by systematically querying the deployed model or its inference infrastructure.
    axis_harm:
      harmed_party: enterprise-itself
      harm_nature: [operational, legal-regulatory]
    axis_capability:
      applicable_tiers: [2, 3, 4, 5]
      not_applicable_to_tier_1_because: >
        A read-only classifier over public documentation offers no confidential training corpus.
    axis_deployment:
      sector_amplifiers: [financial-services, healthcare, defence]
      user_population_amplifiers: [enterprise-external-professional, general-consumer]
      jurisdiction_amplifiers: [eu-ai-act-scope, us-federal-cui-handling]
    parents:
      nist_ai_600_1: [information-security]
      iso_iec_23894: [model-behaviour-under-adversarial-input]
      mit_ai_risk_repository: [2.4 system-security-vulnerabilities-and-attacks]
      airo: [risk-source/adversarial-model-query]
      aiid_category: [model-theft-extraction]
    dependency_edges:
      - kind: escalates
        target: RSK-CAT-091 # unauthorised-personal-data-disclosure
        because: >
          Successful extraction of a training corpus containing personal data
          triggers the disclosure category by construction.
      - kind: escalates
        target: RSK-CAT-118 # regulatory-notification-obligation
        note: escalates only in jurisdictions with breach-notification laws
    residual_scoring_hint:
      impact_axis: [confidentiality-loss, regulatory-notification-scope, ip-loss]
      likelihood_axis: [attacker-model-fitness, query-budget-controls]
      exposure_axis: [deployment-tier, data-classification-of-training-corpus]
    version_history:
      - version: 1.0.0
        change: introduced
        rationale: Initial catalog release
      - version: 1.2.0
        change: split-from
        source_id: RSK-CAT-018
        rationale: >
          Previously combined with weights-exfiltration; separated because
          the mitigation control set diverges (query-budget vs. weight-encryption).
```

This is the level of definition every category in the taxonomy carries. It is what makes the taxonomy queryable, aggregable, and versionable. Exercise-01 produces a starter set of a dozen or so categories at this level of definition; exercise-05 walks the versioning process that migrates them across releases.

## Summary

The enterprise AI risk taxonomy has a three-axis shape: **harm category** (composed from NIST AI RMF GenAI Profile, ISO/IEC 23894, AIRO, MIT AI Risk Repository, and AIID / OECD.AI incident classes), **capability tier** (a 4-5 level ladder scored on modality access × action authority × data blast radius × autonomy horizon), and **deployment context** (sector, user population, jurisdiction). Overlaid on the coordinate system is a dependency map — a small directed graph with `causes`, `escalates`, `subsumes`, and `mitigation-conflicts` edges. Every category is composed from the reference corpora rather than invented, and every category is defensible against a composition worksheet the head of AI governance and internal audit can walk. This is the schema chapter 03's appetite architecture attaches tolerances to, chapter 04's aggregation model queries against, and chapter 07's versioning process migrates.
