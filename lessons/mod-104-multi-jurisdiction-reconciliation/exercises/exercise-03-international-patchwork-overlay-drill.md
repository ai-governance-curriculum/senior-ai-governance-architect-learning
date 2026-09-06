# exercise-03: International patchwork overlay drill

**Estimated effort:** 3 hours

## Objective

Produce the **international-patchwork overlay** for an enterprise control library — the country-level jurisdiction taxonomy, the territorial-scope evaluation per obligation, the variant-management map for regimes that are pairwise incompatible, the filing-entity attribution per country, the language-of-record renderings, and the watch-list workflow — for a single AI product deployed across the UK, Canada, China, Singapore, Australia, India, Korea, and Brazil.

The drill exists because the international patchwork is where the *reconciliation architecture stops being a US or EU story*. The six design decisions chapter 6 names (jurisdiction taxonomy, territorial scope, variant management, filing entity, language of record, watch-list mechanics) either get made once, up front, or get made ad hoc every quarter as new markets go live. This exercise walks you through each decision for a concrete product so the pattern is durable when the enterprise adds its ninth or tenth country.

The deliverable is what the level-50 architect would present to the head-of-AI-governance and the head of international regulatory affairs at the start of an international expansion, before individual country-launch project managers begin their compliance workstreams.

## Prerequisites

- Chapter [`03-cross-cutting-international-obligations.md`](../03-cross-cutting-international-obligations.md) read (GDPR Article 22 is the archetype; the four soft-law layers).
- Chapter [`06-the-international-patchwork.md`](../06-the-international-patchwork.md) read (the eight named jurisdictions plus the six design decisions).
- Chapter [`07-designing-the-reconciliation-architecture.md`](../07-designing-the-reconciliation-architecture.md) read (obligation-record schema, applicability-filter dimensions, variant identifier, filing-entity attribute, language rendering).
- Exercises [`exercise-01-eu-ai-act-articles-to-controls-map.md`](exercise-01-eu-ai-act-articles-to-controls-map.md) and [`exercise-02-us-federal-plus-state-crosswalk-drill.md`](exercise-02-us-federal-plus-state-crosswalk-drill.md) completed or reviewed — the atomic-requirement decomposition and shape-A vs shape-B disciplines are prerequisite.
- Primary sources for the eight jurisdictions: UK Pro-Innovation white paper + AISI + ATRS; TBS Directive on Automated Decision-Making + Bill C-27 status; China Interim Measures for GenAI + TC260 Basic Safety Requirements; Singapore Model AI Governance Framework for GenAI + AI Verify; Australia Voluntary AI Safety Standard + proposed Mandatory Guardrails; India DPDPA + NITI Aayog RAI; Korea AI Basic Act; Brazil PL 2338/2023. Links live in [`../resources.md`](../resources.md). Because several of these are fast-moving, you are *expected* to use `<!-- needs-research: ... -->` markers where you cannot verify current status against the primary source.

## Scenario

You are the level-50 architect at **Meridian AI Assistants**, a GenAI-vendor enterprise that ships **Meridian Advocate**, an AI writing-and-research assistant for enterprise legal, procurement, and internal-communications teams. Meridian Advocate:

- Uses a *self-trained foundation model* — Meridian is a GPAI provider under the EU AI Act framing, and has signed the Hiroshima Code and a US AISI voluntary agreement.
- Produces generative outputs across text (drafting, summarisation), and offers an *optional* image-generation module and an *optional* voice-summary module.
- Includes an "advocate mode" that generates *ranked arguments* for and against a user-supplied position and a "cite mode" that produces citation-checked briefs.
- Is offered as a *cloud-hosted SaaS* to enterprise customers globally, with data-residency options for the EU, the UK, Canada, Australia, Singapore, and India, and a *China-market variant* provided through a licensed local joint-venture entity.
- Has a *UK public-sector edition* sold to central-government departments (subject to ATRS publication expectations and the UK's cross-sector principles as applied by the relevant regulator per customer).

Meridian's control library carries the families named in mod-102 (`AIC-GOV-*`, `AIC-DAT-*`, `AIC-DOC-*`, `AIC-LOG-*`, `AIC-HOV-*`, `AIC-ROB-*`, `AIC-SEC-*`, `AIC-TRP-*`, `AIC-RSK-*`, `AIC-INC-*`) plus a `AIC-GPAI-*` family for its GPAI-provider obligations (populated in exercise-01 from the EU AI Act Articles 51-56 mapping) plus a `AIC-CONTENT-*` family for content-safety and provenance controls.

The international expansion under review adds live production customers in **the UK, Canada, China (via the JV), Singapore, Australia, India, Korea, and Brazil** on top of Meridian's existing EU-and-US footprint.

## Deliverables

1. **`jurisdiction-taxonomy.yaml`** — the country-level applicability-filter jurisdiction attribute and any sub-national or supra-national bucket needed.
2. **`per-country-obligation-decomposition.md`** — the atomic-requirement decomposition for the eight jurisdictions, focused on the Meridian-Advocate-triggering provisions.
3. **`overlay-design-choices.md`** — the six design decisions from chapter 6 answered for Meridian.
4. **`variant-management-map.md`** — the deployment variants Meridian must ship to handle pairwise-incompatible regimes and the applicability-filter attribute that carries the variant.
5. **`watch-list.yaml`** — the current international watch list (Brazil PL 2338, Canada AIDA, Australia mandatory guardrails, and any other bill under legislative consideration you identify).
6. **`review-brief.md`** — a one-page brief.

## Requirements

### `jurisdiction-taxonomy.yaml`

Produce the enumerated jurisdiction vocabulary Meridian's applicability filter uses. Follow the chapter-07 shape:

- ISO 3166-1 alpha-2 country codes for the eight target countries (`gb`, `ca`, `cn`, `sg`, `au`, `in`, `kr`, `br`) plus the pre-existing US (`us`) and EU (`eu` supra-national) buckets.
- Sub-national attributes where the enterprise cares (`us_state.*`, `us_municipality.*` from exercise-02; `ca_province.quebec` for Quebec-specific Law 25 data privacy; state-level attributes for India if you decide to carry them; nothing at sub-national level for the others unless justified).
- Supra-national buckets — the EU is one; Council of Europe is another (needed for the Framework Convention crosswalk); ASEAN or OECD are optional and should be included only if a specific obligation attaches at that level.
- A metadata block per jurisdiction: primary regime identifiers active there, effective dates, and whether the jurisdiction requires a *filing entity* distinct from the parent (China JV → yes; Korea AI Basic Act domestic representative → yes; the rest → no unless you find otherwise).

### `per-country-obligation-decomposition.md`

For each of the eight jurisdictions, walk the Meridian-Advocate-triggering provisions and decompose to atomic requirements. Use the exercise-01/02 table shape (identifier, atomic requirement, addressee, trigger, demand, shape decision, existing family, new id, effective date, consequence).

Cover at minimum:

- **UK** — the sector-regulator pattern (does ICO reach Meridian because Meridian processes UK personal data? does the FCA reach any Meridian customer's financial-services use case? does MHRA reach health-care customer use cases?); the UK AISI evaluation methodology as an *acceptable-methodology* reference on `AIC-GPAI-EVAL-*`; the ATRS artefact rendering as an evidence-contract rendering for the UK-public-sector edition. Note whether any horizontal UK AI Bill has moved from proposal to statute since the chapter was authored — mark with `<!-- needs-research: ... -->` if you cannot verify.
- **Canada** — the TBS Directive AIA obligation for Meridian's federal-government customer (if any) with a specific rendering; AIDA status (mark with `<!-- needs-research: ... -->` and describe the pre-work if it has moved to statute); Quebec Law 25 data-privacy interaction with Meridian's Canadian-tenant data residency.
- **China** — the CAC security-assessment filing (shape-B control), the socialist-core-values content-safety extension (shape-A on `AIC-CONTENT-*`, but with a China-variant-specific rule set), the TC260 Basic Safety Requirements as the technical shape the CAC assessment is measured against, the PIPL interaction for personal-information handling, the JV entity as the filing entity.
- **Singapore** — Model AI Governance Framework for GenAI as an anchor / crosswalk-edge source; AI Verify report as an acceptable evidence rendering on the evaluation control; no shape-B typically.
- **Australia** — Voluntary AI Safety Standard's ten guardrails mapped as shape-A crosswalks; the *mandatory-guardrails* consultation as a watch-list entry with pre-work; the two guardrails where Meridian may not have a natural control home (contestability is a common one).
- **India** — DPDPA data-fiduciary and Significant Data Fiduciary obligations mapped as shape-A on data-protection controls; NITI Aayog RAI as a soft-law crosswalk-edge source; any language-of-record requirement (Hindi + English?) for user-facing artefacts.
- **Korea** — the AI Basic Act's high-impact-AI obligations mapped mostly as shape-A on existing risk-management / transparency / human-oversight / evaluation controls; the domestic-representative appointment as a shape-B control; transparency-to-user obligations where they diverge in content from EU AI Act Article 50 renderings.
- **Brazil** — PL 2338 status (mark `<!-- needs-research: ... -->`); the pre-work fields the watch-list schema expects; if the bill has been enacted, the risk-classification mapping and the ANPD-or-equivalent supervisory-authority filing shape as a shape-B.

For each atomic requirement, state whether it is *soft* (aspirational, non-binding) or *hard* (legally binding on Meridian or on the JV) — the consequence field on the obligation record.

### `overlay-design-choices.md`

Answer the six design decisions from chapter 6, one section each, for Meridian:

1. **Jurisdiction taxonomy at the country level** — reference your `jurisdiction-taxonomy.yaml` and justify any addition beyond the eight named plus the pre-existing US/EU.
2. **Territorial-scope rules per obligation** — the *territorial-scope evaluation function* for at least three obligations Meridian must reason about: GDPR Article 22 (extraterritorial reach), China Interim Measures (services-to-Chinese-public trigger, not offer-based), India DPDPA (extraterritorial reach for goods-or-services offered to Indian data principals). State how Meridian encodes the trigger in the applicability filter and where legal must sign off on the interpretation.
3. **Variant management** — cross-reference your `variant-management-map.md` (below) and state the *decision rule* for when the enterprise ships a variant vs adds a filter branch on the same variant.
4. **Filing-entity attribution** — the JV in China; the domestic representative in Korea; any Significant Data Fiduciary designation in India; the choice of Canadian entity for federal-government sales. Enumerate the filing-entity attribute values and which obligations reference each.
5. **Language of record** — the language-rendering matrix for the evidence contract. Which artefacts must be produced in which languages for which regulators / user populations? Include Chinese (Simplified), Korean, Portuguese (Brazil), Hindi + English, French (Quebec / Canadian federal), and English at minimum. Cover model card, user-facing disclosure, and incident-report artefacts.
6. **Watch-list mechanics** — the cadence, the per-bill status update format, the trigger-to-active checklist. Reference the actual watch list in `watch-list.yaml`.

### `variant-management-map.md`

Enumerate the *deployment variants* of Meridian Advocate:

- **Global core variant** — the default; deployed everywhere except China. Covers UK, Canada, Singapore, Australia, India, Korea, Brazil (and the pre-existing EU and US).
- **China-market variant** — deployed through the JV entity, subject to CAC security assessment, content rules aligned to socialist core values, TC260 Basic Safety Requirements, PIPL data handling, likely no image-generation module or a restricted image-generation module.
- **UK-public-sector variant** — an overlay on the global core variant that adds ATRS publication as a first-class artefact and (as chapter 6 flags) may require content-of-model-card extensions specific to ATRS's enumerated fields. Consider whether this is a genuine variant or a customer-tier rendering on the global core.

For each variant, state:

- The applicability-filter attribute value (`variant: global_core`, `variant: china_jv`, `variant: uk_public_sector`).
- The feature-set delta relative to the global core (modules included / excluded; content-rule differences; data-residency differences).
- The filing-entity value per variant.
- The regime set the variant serves.
- The evidence artefacts unique to the variant.

Then state the *variant decision rule*: when Meridian encounters a new jurisdictional requirement, when does it become a new variant vs a filter branch on an existing variant? A one-paragraph rule of thumb, referencing the pairwise-incompatibility test chapter 6 alludes to.

### `watch-list.yaml`

Produce the international watch list Meridian's obligation register carries. Follow the `status: watched` schema from chapter 7. For each entry:

```yaml
- id: WATCH-BR-PL2338
  jurisdiction: br
  regime_identifier: brazil_pl_2338_2023
  status: watched
  legislative_status: <as of authoring time; mark with needs-research if unverified>
  obligation_shape: comprehensive_ai_regime  # or novel / eu_ai_act_analog / colorado_analog
  trigger_to_active:
    - <the event that would move this from watched to proposed to active>
  pre_work_owner: <legal / architect / ai-governance-analyst>
  pre_work_checklist:
    - <the specific pre-work items the trigger-to-active review will consume>
  review_cadence: quarterly
```

Cover at minimum: Brazil PL 2338; Canada AIDA; Australia Mandatory Guardrails; any UK horizontal AI Bill under active consideration; any Indian AI-specific instrument beyond DPDPA; a Korean enforcement decree under the AI Basic Act; any TC260 update or new Chinese joint measure.

### `review-brief.md`

One page. Must contain:

- **The variant-count decision** — how many variants does Meridian actually ship, and why. If the answer is more than two, the reader should be able to follow the logic without asking.
- **The two-to-three shape-B controls** the international overlay adds beyond what exercises 01 and 02 already produced (CAC security-assessment filing; Korean domestic-representative appointment; ATRS-artefact rendering as its own control if you decided so; anything else).
- **The single most consequential *legal open question*** — usually one of: filing-entity attribution in a jurisdiction where the enterprise structure is not settled; territorial-scope interpretation on an obligation the primary source is ambiguous on; a variant-vs-filter decision that turns on a legal interpretation.
- **The single most consequential *risk-appetite question*** for the head-of-AI-governance — usually one of: whether Meridian accepts the China market at the cost of a persistent variant with distinct content-safety rules; whether Meridian accepts the compliance burden of a UK-public-sector variant if the market opportunity is small; whether Meridian pre-invests in Brazil PL 2338 pre-work before the bill is signed.
- **The methodology-reference multiplicity paragraph** — how Meridian holds US AISI + UK AISI + Singapore AI Verify + China TC260 methodologies on the evidence contract of `AIC-GPAI-EVAL-*` so no single audit context is under-served.

## Starter guidance

- **Start from the six design decisions, not from the countries.** The countries are inputs; the decisions are the architecture. If you decompose one country at a time first, you will re-decide the same taxonomy question eight times.
- **The variant question is where the drill pays off.** Most enterprises resist variants because they cost engineering; most reconciliation architectures should ship variants anyway because the alternative (one product that violates one regime to satisfy another) is worse. Be honest about whether China is a variant.
- **Territorial scope is not the same as jurisdiction attribute.** A control can apply in the UK because Meridian offers services to UK data subjects even if Meridian has no UK entity. The applicability filter carries the *effect* of the territorial-scope evaluation; the *evaluation function itself* is a per-obligation piece of legal analysis that lives on the obligation record.
- **Filing entity is often not obvious.** The JV, the domestic representative, the Significant Data Fiduciary, and the parent enterprise are four different legal persons that may each be the correct filing entity for a different obligation. Get this wrong and the wrong person signs a filing.
- **Language of record is not a translation project.** It is an evidence-contract-rendering project. The evidence architecture produces the artefact once and renders it in the required languages; the translation memory belongs to the evidence toolchain, not to the control library.
- **Do not invent bill statuses.** Brazil, Canada, Australia, and (potentially) the UK have fast-moving legislative situations. If you cannot verify the current status against the primary source at authoring time, mark the entry with `<!-- needs-research: ... -->` and describe the *pre-work* — that is the point of the watch-list.
- **The soft-law layer earns its keep here.** The Hiroshima Code and the Council of Europe Framework Convention thread through several jurisdictions; do not double-count them (a Hiroshima commitment discharged in a UK evidence artefact does not need re-discharge in a Singapore one). Chapter 3's discipline applies.

## Acceptance criteria

- [ ] The jurisdiction taxonomy YAML covers all eight target countries plus the pre-existing EU and US buckets, uses ISO 3166 codes correctly, and includes any sub-national attribute you decided to carry with a stated reason.
- [ ] The per-country obligation decomposition is *focused on Meridian-Advocate-triggering provisions*, not the whole regime.
- [ ] Every shape-A decision names the existing `AIC-*` family; every shape-B decision carries a rationale for why shape A is insufficient.
- [ ] The six design-choice sections each land a decision, not a survey of options.
- [ ] The variant-management map answers a *concrete variant count* (not "we may ship one or more variants"), states the applicability-filter attribute values, and gives a decision rule for the next variant question.
- [ ] The watch list covers Brazil, Canada, Australia at minimum, with `<!-- needs-research: ... -->` markers on any status you could not verify against the primary source.
- [ ] Filing-entity attribution is stated per jurisdiction, and the JV / domestic-representative / SDF cases are handled explicitly.
- [ ] The language-of-record matrix names the artefact class and the required languages per jurisdiction — no free-text "translate as needed".
- [ ] The review brief carries the variant-count decision, the shape-B additions, the legal open question, and the risk-appetite question — nothing skipped.
- [ ] Every soft-law source (OECD, UNESCO, Hiroshima, Council of Europe) is carried as a crosswalk edge somewhere, not as a control.
- [ ] No invented bill statuses; every fast-moving datum either verified or marked `<!-- needs-research: ... -->`.

## Stretch goals

- Draft the *quarterly watch-list review agenda* — the actual meeting shape that keeps the watch-list current. Include attendees (legal, architect, government-affairs, analyst), inputs (bill trackers, sector-regulator publications, national-AISI announcements), outputs (updated statuses, moved-to-proposed records, pre-work-triggered items).
- Add a *ninth-country expansion analysis* — pick Japan (which has active AI-governance discussions) or Saudi Arabia (which has published an SDAIA AI-Ethics framework) or South Africa, and sketch the two-page addition to the reconciliation architecture required.
- Produce the *Article 27 FRIA vs Canadian AIA vs Brazil-if-enacted impact-assessment* comparison as a rendering map on Meridian's `AIC-IMPACT-*` control (if you carry one; otherwise on `AIC-RSK-*`). One artefact, three renderings, three filings, three languages.
- Sketch what Meridian does if the *US AISI methodology diverges materially from the UK AISI methodology* over the next two years — how the evidence contract handles the divergence without forcing a per-methodology re-evaluation. One paragraph.
