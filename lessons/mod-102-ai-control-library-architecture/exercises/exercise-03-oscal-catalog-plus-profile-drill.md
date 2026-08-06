# exercise-03: OSCAL Catalog + Profile Drill

**Estimated effort:** 3 hours

## Objective

Serialise the four composed entries from exercise-02 as a real **OSCAL catalog** and produce one **OSCAL profile** that tailors that catalog for a specific applicability (EU tier-1 high-risk). The drill has two learning goals: (1) that the seven library fields land on OSCAL primitives cleanly, and (2) that the profile model does the applicability-slicing job the library is designed for — so the next time the head-of-AI-governance asks "what controls apply to our new Munich deployment?", the answer is *dereference the EU profile*, not *ask the architect*.

The drill is technical. It is also small — one catalog fragment and one profile — because chapter 04's point is that OSCAL is *shape*, not *scale*.

## Prerequisites

- Chapter [`04-oscal-representation.md`](../04-oscal-representation.md) read once.
- Exercise-02 completed, or the four composed entries drafted equivalently.
- OSCAL model references open:
  - Reference index: <https://pages.nist.gov/OSCAL/>
  - Catalog model: <https://pages.nist.gov/OSCAL-Reference/models/latest/catalog/>
  - Profile model: <https://pages.nist.gov/OSCAL-Reference/models/latest/profile/>
- Skim access to NIST-published OSCAL content, for shape sanity checks: <https://github.com/usnistgov/oscal-content>.
- Optional but useful: an OSCAL validator (the developer-tools index links to several).

## Scenario

Northbrook Financial has approved the first four composed entries from exercise-02 for the initial catalog release. The GRC-for-AI platform (a Northbrook-hosted planning target for mod-111) will consume OSCAL directly. You have been asked to publish:

- `aic-catalog-2026.0.1.{xml|json|yaml}` — the initial catalog, pinned to a specific OSCAL revision.
- `profile-eu-tier1-high-risk-2026.0.1.{xml|json|yaml}` — the first baseline profile, covering EU-deployed tier-1 high-risk systems.

The EU expansion team is standing up its first Munich deployment next quarter and will pin its SSP against this profile. The profile's tailorings must be defensible to an EU AI Act notified body at pre-market conformity assessment.

## Deliverables

Produce a directory containing:

1. **`aic-catalog-2026.0.1.<ext>`** — the OSCAL catalog serialising the four entries from exercise-02.
2. **`profile-eu-tier1-high-risk-2026.0.1.<ext>`** — the OSCAL profile that includes and tailors those entries for the EU tier-1 baseline.
3. **`oscal-design-notes.md`** — the design decisions and their rationale.
4. **`evidence-schema-registry-stubs.md`** — one paragraph per evidence-schema URN the catalog references but does not embed.

You may serialise in XML, JSON, or YAML — one format across both files. Well-formed content that a reader familiar with OSCAL can validate on inspection is sufficient; if you run a validator, note the tool and the OSCAL version pinned.

## Requirements

### `aic-catalog-2026.0.1.<ext>`

- `metadata` block with `title`, `version` (`2026.0.1`), `last-modified`, and `oscal-version` (pin the specific OSCAL revision your toolchain supports).
- Four `group`s at minimum: `AIC-DAT`, `AIC-OVS` (oversight), `AIC-LOG`, `AIC-MON` — one per exercise-02 outcome.
- Each of the four `control`s carries:
  - `id`, `title`, and a `prop name="version"` for the per-control version.
  - Applicability predicates as **namespaced** `prop`s (`ns="urn:northbrook:aic:applicability"`) — one prop per (dimension, value) pair.
  - `part name="statement"` — the enterprise-outcome statement from exercise-02, unchanged.
  - `part name="guidance"` — implementation guidance, referencing platform artefacts as URNs.
  - `part name="objective"` — the assessment objective; chapter 04 explains why this is where the evidence-contract summary lives.
  - `part name="assessment"` with one nested `part name="method"` per test method, each carrying `prop`s for `method` (EXAMINE / INTERVIEW / TEST), `cadence`, and `owner_role`.
  - `link rel="related"` to a `back-matter/resource` for every crosswalk edge (NIST AI RMF, ISO/IEC 42001 Annex A, EU AI Act, MITRE ATLAS where present). Every crosswalk resource must resolve to a pinned URL and a source-version citation.
  - `link rel="reference"` for evidence-schema URNs into the parallel registry (`urn:northbrook:schema:...#version`). The URN must appear in `evidence-schema-registry-stubs.md` — no dangling references.
- `back-matter` with a `resource` for every referenced source, each carrying `title`, `citation`, and `rlink`.
- Standardised `part` names across every control per chapter 04's advice — `statement`, `guidance`, `objective`, `assessment`, `evidence`, `notes` — no ad-hoc names.

### `profile-eu-tier1-high-risk-2026.0.1.<ext>`

- `metadata` block with a title that names the applicability (`Northbrook AI Baseline — EU Tier-1 High-Risk`), version aligned with the catalog, and pinned `oscal-version`.
- `import` block referencing the catalog by URN, using **`include-controls`** with explicit IDs — not `include-all` + `exclude-controls`. Chapter 04 explains why explicit inclusion is the defensible choice.
- `modify` block with at least **two** tailorings:
  - One `alter` that *adds* a jurisdictional assessment method (e.g., an EU pre-market examination step on the training-data provenance entry).
  - One `alter` that *tightens* a parameter — e.g., a tighter cadence, a shorter reconstruction window, or a higher retention floor — expressed as an additional prop on the assessment method, not a statement rewrite.
- No `modify` that rewrites a `statement` field. Chapter 04 says outcome changes are *new controls*, not profile tailorings.
- `back-matter` with the EU-side resources the tailorings reference (Article 43 conformity assessment, Article 11 technical documentation, Article 12 log-retention obligation, notified-body index).

### `oscal-design-notes.md`

Cover, in prose:

- Which OSCAL revision you pinned, and why (toolchain support, downstream consumer support). If you pin to `latest`, defend it — chapter 04 warns against `latest`.
- Your `prop` namespace strategy — the specific URN scheme you chose for applicability predicates, evidence-schema references, and any Northbrook-specific extension props. Preview the namespace-registry idea you would carry into mod-108.
- Your part-name convention and how you would document it in the catalog preface so downstream renderers can rely on it.
- The one place your composed entries did *not* land cleanly on OSCAL primitives, and how you resolved it. There will be at least one — surface it explicitly. (Chapter 04 flags the evidence-contract schema registry as an example.)
- One paragraph on the *cross-catalog composition* posture per chapter 04: which SP 800-53 rev5 controls would you `link rel="related"` from your four entries once the NIST-published SP 800-53 rev5 catalog is imported? Do not import it; name the links you would add and the pinned SP 800-53 revision.

### `evidence-schema-registry-stubs.md`

For every `link rel="reference"` URN in the catalog, one paragraph:

- The URN, at pinned version.
- What the schema describes at record level (e.g., a training-run provenance record).
- The **producer** (pipeline / role) and the **consumer** (analyst tool / GRC-for-AI platform / regulator submission).
- Whether the schema exists today (assume no — Northbrook is at day one) and, if not, that the schema-authoring task is on the same release as the control per chapter 07's "no dangling schema references" rule.

## Starter guidance

- Write the catalog first, then the profile. The profile only makes sense once the catalog exists to import.
- For the applicability predicates, prefer *multiple `prop`s with the same name* (one per allowed value) over comma-separated values inside a single prop. Downstream selection code is easier and OSCAL validation is happier.
- Do not embed the evidence-schema definitions in OSCAL. OSCAL points; the schema registry defines. Chapter 04 is explicit that this indirection is *the* right choice, not a workaround.
- Verify every `back-matter/resource` URL manually — a broken link at audit time is a fixable embarrassment, a broken link at regulator-submission time is an incident.
- Resist the temptation to use `modify` for anything other than *additive* profile tailoring. If the profile needs to change the statement, the profile is asking for a *different control*, not a tailoring.

## Acceptance criteria

- [ ] Catalog serialises all four exercise-02 entries in OSCAL with the required `metadata`, `groups`, `controls`, `parts`, `props`, `links`, and `back-matter`.
- [ ] Every applicability predicate rides on a namespaced `prop`; no applicability text buried inside `statement` or `guidance`.
- [ ] Every crosswalk edge is a `link` to a `back-matter/resource`, not free-text prose.
- [ ] `part` names are consistent across every control, drawn from the standardised set named in `oscal-design-notes.md`.
- [ ] Profile uses explicit `include-controls`, tailorings are additive (`alter`/`add` with an `objective` or `assessment` `part`, not a `statement` rewrite), and each tailoring names a specific EU-side justification.
- [ ] `oscal-version` is a pinned specific version in both catalog and profile metadata — not `latest`.
- [ ] Every `link rel="reference"` URN appears in `evidence-schema-registry-stubs.md`.
- [ ] Design notes name the OSCAL primitive that did not fit cleanly and how the design resolved it.

## Stretch goals

- Add a *second* profile — `profile-us-financial-tier1` — that includes the same four controls but tailors for SR 11-7 documentation cadences (annual model documentation attestation, tri-annual full model validation). Demonstrates that the same catalog serves two very different regulatory postures.
- Sketch the SSP the Munich deployment will produce against `profile-eu-tier1-high-risk-2026.0.1` — just the shape, no substance. One YAML block that a level-15 analyst could fill in.
- Add a `link rel="related"` on the training-data provenance entry to a *specific* SP 800-53 rev5 control (e.g., SI-12 information handling / retention) with a `back-matter/resource` pinned to the current SP 800-53 rev5 OSCAL revision. Verify the SP 800-53 control ID against the primary catalog, do not guess.
