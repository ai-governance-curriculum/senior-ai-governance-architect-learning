# exercise-05: Evidence Schema OSCAL Representation

**Estimated effort:** 2 hours

## Objective

Author the **OSCAL representation** of the enterprise's evidence-schema catalog for the chosen scenario — a catalog of AI-specific evidence-obligation controls, one or two profiles that select and parameterise the controls for a specific tier, one or two system-security-plans (OSCAL SSPs, the OSCAL name for what this track calls the per-system governance plan) that instantiate the profile for representative AI systems, and at least three executable queries against the catalog that satisfy chapter 06's "three killer queries" — coverage, freshness, and obligation-crosswalk.

The deliverable is not a paper OSCAL exercise. The catalog, profile, and SSPs together must make the substrate content designed in exercises 01–04 (audit logs, cards, ML-BOMs plus SLSA plus Sigstore, regulator-facing templates) *machine-queryable* against a declared control set — a pre-deployment gate reviewer, an internal auditor, or a regulator counterpart should be able to run the three queries and get a defensible answer without opening a wiki. Chapter 06's failure mode 1 (the "museum" — OSCAL JSON that nothing queries) is the specific outcome this exercise is designed to prevent.

## Prerequisites

- Chapter [`06-oscal-and-machine-readable-evidence-catalog.md`](../06-oscal-and-machine-readable-evidence-catalog.md) read once, with the three killer queries and the two failure modes marked. Do not attempt this exercise before reading chapter 06 — the exercise is anchored to that chapter's model.
- Exercises 01 (audit-log architecture), 02 (card schemas), 03 (ML-BOM plus SLSA plus Sigstore), 04 (regulator-facing artefact templates) available. The catalog controls in this exercise *reference* the substrate designed in those exercises; a catalog without those exercises done is a catalog of controls against a substrate that does not exist yet.
- Mod-102 (enterprise control library) available — many catalog controls trace to mod-102 control IDs. The AI-specific evidence-obligation controls authored here *extend* the mod-102 library rather than duplicate it.
- Mod-105 (AIMS and Statement of Applicability) available — the SoA drives which controls the profile marks in-scope, and the roles and responsibilities assigned in the profile trace to the AIMS org design.
- Primary references:
  - NIST OSCAL (Open Security Controls Assessment Language) landing at https://pages.nist.gov/OSCAL/
  - OSCAL 1.x model documentation — catalog, profile, component-definition, system-security-plan, assessment-plan, assessment-results, plan-of-action-and-milestones
  - OSCAL GitHub repository at https://github.com/usnistgov/OSCAL
  - NIST SP 800-53 Rev. 5 published as a canonical OSCAL catalog example (import target for the enterprise's security-control baseline)
  - ISO/IEC 42001 Annex A as the control-family reference the enterprise-authored AI-evidence catalog extends the NIST 800-53 baseline with
  - JSON Schema and YAML specifications for the syntactic layer
  - Chapter [`06-…md`](../06-oscal-and-machine-readable-evidence-catalog.md), [`../resources.md`](../resources.md)

## Scenario

Use the same scenario you carried through exercises 01–04:

- **Scenario A — US regional bank.** Retail deposit and lending; SR 11-7 MRM discipline in place; internal-audit function seasoned; one credit-decision AI in production with adverse-action reason-code obligations under ECOA/Regulation B; one fraud-detection AI; the enterprise is preparing for ISO 42001 certification and has one HR-tech AEDT deployment in NYC scope of Local Law 144.
- **Scenario B — Global healthcare payer/provider.** US and EU operations; one clinical-decision-support AI in FDA SaMD scope; one utilization-review AI; a European insurance subsidiary with a claims-triage AI in AI Act Annex III scope; HIPAA, GDPR, and AI Act obligations concurrent.
- **Scenario C — B2B SaaS HR-tech vendor.** ATS ranking model plus an interview-scheduling assistant; EU customers deploy the ATS in AI Act high-risk employment contexts; US customers include NYC-based enterprises under LL144; ISO 42001 certification is customer-required.

State your scenario choice at the top of the deliverable. The scenario determines which tiers exist in the profile set (the bank scenario adds SR 11-7 tiering; the healthcare scenario adds an FDA-SaMD tier; the SaaS scenario adds a customer-contractual tier) and which representative AI systems the SSPs instantiate.

## Deliverables

Author five artefact groups in a working directory of your choice.

1. **`catalog/enterprise-ai-evidence.oscal.yaml`** — the OSCAL catalog carrying 8–12 illustrative controls drawn from the evidence obligations across the three regimes (EU AI Act, SR 11-7 or FDA or LL144 as sector-appropriate, ISO 42001) and the sector overlays relevant to the chosen scenario. Controls are organised into named groups.
2. **`profiles/`** — a profile per active tier for the chosen scenario. At minimum:
   - `profiles/tier-3-eu-high-risk.oscal.yaml` — the profile that imports the catalog, selects the controls in scope for a Tier-3 (EU high-risk) system, sets parameter values (retention duration, review cadence, SLSA level target), and assigns responsibilities.
   - Optionally a second profile where the scenario demands one — e.g., `profiles/tier-2-sr-11-7.oscal.yaml` for the bank scenario, `profiles/tier-2-samd-class-ii.oscal.yaml` for the healthcare scenario, `profiles/customer-contractual-tier-a.oscal.yaml` for the SaaS scenario.
3. **`component-definitions/`** — one OSCAL component-definition per representative AI-system class in the scenario (at least two component-definitions). A component-definition may model a *class* of components (for example "vendor foundation model", "internal fine-tune", "third-party MLOps platform") that many system-inventory IDs instantiate — this is the pattern preferred over authoring one component-definition per system-inventory row.
4. **`ssps/system-<id>.oscal.yaml`** — one or two OSCAL SSPs (system-security-plans; called *system-governance-plans* in the track's prose) instantiating the profile for a specific representative system, populating `implemented-requirements` with the enterprise's control-implementation statements and referencing the component-definitions via `by-component`.
5. **`queries/`** — three executable queries expressed in a repeatable form (JQ over the OSCAL JSON serialisation, a SQL sketch over a flattened OSCAL projection, or an OPA/Rego rule set over the OSCAL JSON). The three queries satisfy chapter 06's three killer queries:
   - `queries/01-coverage.<jq|sql|rego>` — every in-scope system whose model-card-currency control is currently unmet.
   - `queries/02-freshness.<jq|sql|rego>` — every in-scope system with an evidence artefact older than the tier-implied threshold.
   - `queries/03-obligation-crosswalk.<jq|sql|rego>` — for a chosen filed Article 11 packet for system X, every substrate artefact the packet requires, with current status.

## Requirements

### `catalog/enterprise-ai-evidence.oscal.yaml`

The catalog carries **at least 8 controls** (target 8–12). Each control has:

- a stable `uuid` (any RFC 4122 UUID; do not reuse across controls)
- a `title` and a `class` (a `class` naming convention aligned to the AICG evidence-obligation lens; e.g., `class: aicg-evidence-obligation`)
- a `parts` block including at minimum a `statement` part and one or more `assessment-objective` parts
- one or more `props` recording (i) the primary-source obligation the control derives from (regulation article, standard clause, sector rule — cited by name), (ii) the tier at which the control applies, (iii) a link to the mod-102 control-library row the control extends where applicable
- one or more `links` — including `back-matter` `resources` links to the underlying primary-source obligation citation

Controls must span **at least these eight obligation shapes**:

1. **Model-card currency.** Tier X requires the system's model card to be signed within N months. Parameterise both the tier and N.
2. **Dataset-card presence.** Every training dataset in the system's training composition has a dataset card meeting the mod-108 dataset-card schema.
3. **ML-BOM presence and SLSA attestation.** Every registered model artefact carries a CycloneDX ML-BOM and a SLSA-L≥N provenance attestation. Parameterise the SLSA level target.
4. **Substrate log retention meeting the tier's obligation.** The audit-log family retention meets or exceeds the horizon required for the system's highest-severity applicable regime. Parameterise the horizon.
5. **Risk-card presence bound to a risk-register entry.** The system carries a risk card and every risk in the card resolves to a live entry in the mod-106 risk register (RR-ID resolvable).
6. **Regulator-packet reproducibility posture.** Every filed regulator packet (Article 11, Article 72, Article 73, SR 11-7 documentation, FDA PCCP) is reproducible from the substrate at the recorded `snapshot_ts`; the reproducibility check is executed on a defined cadence and its result is recorded.
7. **Substrate `snapshot_ts` recording.** Every filed regulator packet records the substrate snapshot identifier at filing; the identifier resolves against the substrate content-addressable store.
8. **Audit-log immutability posture.** The relevant audit-log families are stored in WORM or hash-chain-verified backends and the immutability posture is verified quarterly.

Controls are organised into `groups`. The group naming aligns to the AICG evidence-obligation lens; suggested group set:

- `evidence-completeness` — controls 1, 2, 5
- `evidence-provenance` — control 3
- `evidence-reproducibility` — controls 6, 7
- `evidence-retention` — controls 4, 8

Additional controls beyond the required eight are welcome where the scenario surfaces them (e.g., ECOA adverse-action reason-code record retention for the bank; PCCP change-log control for the healthcare SaMD system; LL144 published-summary retention for the SaaS scenario).

Where you cite a specific OSCAL 1.x field name and are not certain the field exists at that path in the model version you are targeting, mark the citation `<!-- needs-research: verify OSCAL 1.1.x field name … -->`. Do not invent fields.

### `profiles/tier-3-eu-high-risk.oscal.yaml` (and a second profile as scenario-appropriate)

The profile:

- `imports` the enterprise-authored catalog (and, as a stretch, imports the NIST SP 800-53 Rev. 5 catalog for the security-baseline controls the enterprise inherits — see starter guidance).
- `include-controls` selects the controls in scope for the tier. If the tier includes all catalog controls, use the "include all" pattern the OSCAL profile model provides; if the tier is a subset, list the controls by ID.
- `set-parameters` operations assign tier-specific values:
  - retention horizons (e.g., Article 12/19 default 10-year evidence-retention window for high-risk systems <!-- needs-research: verify Article 12 vs Article 19 retention-clause location in the consolidated Regulation (EU) 2024/1689 text -->)
  - review cadence (e.g., model-card re-signature every 6 months for Tier 3)
  - SLSA level target (e.g., SLSA-L3 minimum for Tier-3 model artefacts)
- `metadata.parties` and `metadata.roles` assign responsibilities. At minimum, name the roles referenced by control-implementation statements downstream: head-of-ai-governance, ai-accountable-executive, model-owner, second-line-reviewer, evaluation-engineer, risk-engineer, internal-audit-lead, general-counsel-seat. Where the scenario has additional named seats (e.g., head-of-MRM for the bank scenario, head-of-regulatory-affairs for the FDA SaMD scenario), include them.
- `modify` may adjust control statements where the tier-specific parameterisation requires. Prefer `set-parameters` over `modify` where possible.

If the scenario has a second active tier, author the second profile with the same discipline. The second profile's `set-parameters` values must differ from the Tier-3 profile's — a copy-paste second profile is a tell that the tier model does not actually bite.

### `component-definitions/`

**At least two component-definitions**, each modelling a class of components rather than a specific system-inventory row.

Each component-definition:

- declares `components` with at least one `component` per class (e.g., "vendor-foundation-model", "internal-fine-tune", "third-party-mlops-platform", "in-house-classical-ML-model")
- for each component, declares `capabilities` enumerating what the component brings — training-data provenance, inference-time logging, model-card generation, SLSA-attesting build pipeline, etc.
- for each component, declares `control-implementations` importing controls from the enterprise-authored catalog (and where applicable the NIST 800-53 catalog); each `implemented-requirement` states the enterprise's implementation stance for the control *at the component-class level* (not the per-system statement — that lives in the SSP)

The two component-definitions taken together must cover the substrate patterns exercised in exercises 01–04. Where a control depends on a substrate the component does not natively produce, the component-definition must state so explicitly (e.g., a vendor foundation model that does not ship a dataset card requires a compensating dataset-card synthesis at the enterprise boundary).

### `ssps/system-<id>.oscal.yaml`

**At least one SSP** (target one or two) instantiating the Tier-3 profile for a representative AI system in the scenario.

Each SSP:

- `imports` the profile (which in turn imports the catalog)
- declares the `system-characteristics` block naming the system (system name, system-inventory ID, system boundary, information types processed, users, environments)
- populates `implemented-requirements` for every profile-selected control; each implementation statement references the component-definitions via `by-component` so the reader can trace the implementation to the responsible component class
- every citation to substrate content (audit-log query, card excerpt, ML-BOM location, SLSA attestation URI, filed regulator packet) is expressed as a `back-matter` `resources` entry so it is *machine-resolvable* — a substrate URI, a content-addressable hash, or a substrate-query identifier; not a prose reference the reader has to interpret
- names the responsible parties for each implementation statement, drawing from the roles declared in the profile

Where OSCAL's SSP model does not carry a native field for an AI-specific concept (e.g., "current model-card signature timestamp"), express it via `back-matter` `resources` with documented `props`, and mark the workaround `<!-- needs-research: verify OSCAL 1.1.x extension pattern for … -->` where you are not certain the pattern is the sanctioned OSCAL extension approach.

The SSP is the artefact the pre-deployment gate reviewer would consult; its readability at that use-site is a first-order requirement.

### `queries/`

Three executable queries. Each query is authored in **one** of: JQ over the OSCAL JSON serialisation, SQL over a flattened OSCAL projection (you may sketch the projection view), or OPA/Rego over the OSCAL JSON. Use the form most natural to your tooling; the form does not matter as long as the query is *runnable*.

For each query, deliver:

- the query text (JQ, SQL, or Rego)
- a sample projection of the expected output shape (a small YAML or JSON block showing what the query returns against a plausible substrate state)
- an explicit list of which catalog controls the query touches (control IDs)

The three queries:

- **`queries/01-coverage.<ext>` — coverage.** For every in-scope system (i.e., every SSP), does the model-card-currency control (control 1) currently hold? Output is a per-system row: system ID, control ID, `held | unmet`, and where `unmet`, the age of the current model card.
- **`queries/02-freshness.<ext>` — freshness.** For every in-scope system, is any evidence artefact older than the tier-implied threshold (via the profile's `set-parameters`)? Output is a per-system row: system ID, artefact class, artefact reference, age, threshold, `within | over`.
- **`queries/03-obligation-crosswalk.<ext>` — obligation-crosswalk.** For a chosen filed Article 11 packet for a specific system, enumerate every substrate artefact the packet requires and its current status. Output rows: packet section, required artefact class, artefact reference, `present | missing | stale`.

Every query must produce a projection *from the OSCAL artefacts themselves* (not from a parallel wiki). If a query cannot be answered from the OSCAL layer, either extend the OSCAL layer or explicitly name the substrate-side extension the query relies on.

### Cross-cutting

- Every catalog control must trace to a **primary-source obligation cited by name** — regulation article, standard clause, sector rule (e.g., "Regulation (EU) 2024/1689, Article 12", "ISO/IEC 42001:2023 Annex A control A.6.2.3", "SR 11-7 Section III.C model documentation expectations"). Do not cite by phrase alone; cite by identifier. Where the identifier is not verified, mark `<!-- needs-research: … -->`.
- Where the OSCAL model does not have a native field for an AI-specific concept, use `back-matter` `resources` extensions or documented `props`. Do not invent OSCAL field names.
- Every uncertain OSCAL 1.x field name, path, or model shape must be marked `<!-- needs-research: verify OSCAL 1.1.x field name … -->`.
- All YAML/JSON artefacts must parse (valid YAML, valid JSON). The three queries must actually run against the artefacts.

## Starter guidance

- OSCAL is opinionated. Do not fight the model — extend where required at `back-matter` `resources` and `links` rather than authoring parallel fields the OSCAL toolchain will not honour. If you find yourself wanting to add a top-level field that OSCAL does not have, that is the moment to reach for `resources` and `props`.
- Do not represent every audit-log event class as an OSCAL control. **Controls are the obligations** ("the audit-log family for governance-workflow events must be retained for N years"); **events are the substrate that carries evidence of control implementation**. Conflating the two produces a catalog of thousands of pseudo-controls and no useful queries.
- The SSP is a **per-system** artefact. The profile is the **per-tier** artefact. If you find yourself authoring per-tier content in the SSP or per-system content in the profile, the layering is wrong — refactor before proceeding.
- The queries are **the reason the catalog exists**. If the exercise ends with an OSCAL JSON that nothing queries, the OSCAL layer is a museum (chapter 06 failure mode 1). Write the queries first if you are stuck on where to spend authoring effort — the queries will tell you which controls, parameters, and back-matter resources need to exist.
- Do not conflate the OSCAL `component` with the enterprise's **system-inventory ID**. The component-definition models a *class* of components (e.g., "vendor foundation model") that many system-inventory IDs instantiate. The SSP references the component-definition and identifies the specific system-inventory row. This layering is what makes the OSCAL slice reusable across the enterprise system portfolio.
- **Import the NIST SP 800-53 catalog** for the security-control baseline the enterprise inherits, and let the enterprise-authored catalog be the *AI-specific extension* rather than a duplicate of 800-53. The AI-evidence catalog is small and focused; the security baseline is large and already OSCAL-native.
- Two hours is not enough time to author a production-grade OSCAL slice. It is enough time to author a defensible skeleton that demonstrates the shape and answers the three killer queries against a plausible substrate. Optimise for the skeleton, not the finish.

## Acceptance criteria

- [ ] Scenario choice stated at the top of the deliverable; the scenario's active tiers named.
- [ ] `catalog/enterprise-ai-evidence.oscal.yaml` authored with at least 8 controls organised into named groups.
- [ ] Every catalog control cites a primary-source obligation by identifier (article, clause, section); every unverified identifier is marked `<!-- needs-research: … -->`.
- [ ] At least one profile (Tier-3 EU high-risk) authored with `import`, `include-controls`, `set-parameters` for retention / cadence / SLSA level, and `metadata.parties` and `metadata.roles` assignments. A second profile is authored where the scenario has a second active tier, and its parameter values differ substantively from the Tier-3 profile's.
- [ ] At least two component-definitions authored, each modelling a component *class* with capabilities and control-implementations.
- [ ] At least one SSP authored with `implemented-requirements` populated and `by-component` references resolvable to the component-definitions; every substrate citation is a `back-matter` `resources` entry.
- [ ] Three executable queries authored (coverage, freshness, obligation-crosswalk); each query has query text, a sample projection, and a list of the controls it touches; each query actually runs against the authored OSCAL artefacts.
- [ ] Every non-native concept is represented via `back-matter` `resources` extensions or documented `props`; no invented OSCAL fields.
- [ ] Every unverified OSCAL 1.x field name or model path is marked `<!-- needs-research: … -->`.
- [ ] All YAML/JSON artefacts parse (validate with a YAML/JSON parser, or run through the OSCAL toolchain where available).

## Stretch goals

- Represent the Article 11 packet-assembly template (from exercise 04) as an OSCAL **assessment-plan** whose objectives are the Annex IV headings, and simulate an **assessment-results** file capturing a rehearsed pre-deployment gate run for one representative system.
- Add a **plan-of-action-and-milestones (POAM)** entry for a gap surfaced by one of the three queries, and demonstrate the POAM's composition with the mod-105 chapter 09 CAPA process and the mod-107 chapter 04 internal-audit-finding workflow — one CAPA lineage, three artefact projections.
- Publish the catalog, profiles, and component-definitions as an internal **OCI-image bundle** signed with Sigstore cosign (composition with exercise 03's Sigstore discipline). Downstream consumers verify the signature before consuming the catalog.
- Build a machine-readable **dashboard projection** over the OSCAL catalog plus the substrate that renders a per-system evidence heat-map (rows: systems; columns: catalog controls; cells: `held | unmet | stale | unknown`) for the head of AI governance's quarterly review. The projection is not additional substrate — it is a view over the OSCAL layer and the substrate the OSCAL layer references.
