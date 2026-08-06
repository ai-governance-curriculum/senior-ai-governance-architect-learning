# exercise-01: Control Catalog Shape Proposal

**Estimated effort:** 3 hours

## Objective

Produce the **catalog shape proposal** for the enterprise AI control library at Northbrook Financial (the scenario from mod-101 exercise-02) — the artefact you would take to the head-of-AI-governance to get the library's shape ratified *before* any control is authored.

The deliverable is a decision document plus three worked entries that instantiate the decisions. Downstream modules in this track (policy taxonomy in mod-103, AIMS in mod-105, evidence architecture in mod-108, GRC-for-AI in mod-111) will pin to the shape you propose here — get it right and everything composes; get it wrong and every downstream artefact inherits the flaw.

## Prerequisites

- Chapter [`01-anatomy-of-a-control-library-entry.md`](../01-anatomy-of-a-control-library-entry.md) read once.
- Skim [`CURRICULUM.md`](../../../CURRICULUM.md) for the level-50 architect ownership rule.
- Access to the primary sources you will crosswalk into your entries: NIST AI RMF 1.0, ISO/IEC 42001 Annex A (title level is enough if you do not have the paywalled standard), and Regulation (EU) 2024/1689 Articles 9-15. See [`../resources.md`](../resources.md).

## Scenario

Same Northbrook Financial as mod-101 exercise-02:

- US regional bank, existing SR 11-7-aligned MRM programme, existing ISO/IEC 27001 ISMS.
- ~40 AI systems: fraud classifiers, document extraction, an internal RAG assistant, a customer-facing generative chat, several third-party AI SaaS integrations.
- Colorado + New York City deployments; EU expansion in-flight for next year.
- No enterprise AI control library exists yet. You are the level-50 architect and the head-of-AI-governance has asked you for the shape proposal by end of quarter.

## Deliverables

Author three artefacts in a working directory of your choice:

1. **`catalog-shape-proposal.md`** — the decision document.
2. **`worked-entries.yaml`** — three worked control entries that instantiate the shape.
3. **`shape-review-brief.md`** — a one-page brief you would send with the proposal to the level-60 head-of-AI-governance.

## Requirements

### `catalog-shape-proposal.md`

Must decide and justify **each** of the following:

- **Library scope.** One library or multiple? If multiple, on what axis are they split (chapter 05 previews the shapes). Justify against Northbrook's org shape.
- **ID naming convention.** The three-part `<library>-<family>-<sequence>` (or an alternative you defend). Enumerate the initial family prefixes you propose (governance, data, model, engineering, security, monitoring, evidence, third-party, transparency, oversight, robustness, ...) and state which are *reserved for later* vs. active on release 1.
- **Versioning scheme.** The semver-ish major/minor/patch mapping. Define exactly what a *breaking* change is at Northbrook, distinct from *additive* and *editorial*.
- **The seven fields, per chapter 01.** For each field: whether Northbrook adopts it verbatim, extends it, or overloads it, and why. If you extend the shape, name the additional fields and defend them.
- **Applicability-filter dimensions.** The named dimensions the library uses (`system_tier`, `system_kind`, `use_case`, `jurisdiction`, `risk_appetite_band`, `data_class`, ...). For each: enumerated values, the closed-world convention, and where the enumeration is maintained.
- **Field-level ownership matrix.** The table from chapter 01, adapted for Northbrook's actual seats (level-50 architect, level-25 risk engineer *when hired*, two level-15 analysts *today*, evaluation seat at level 35 *joint with model validation team under SR 11-7*).
- **Catalog preface.** The set of one-time-decisions the library preface publishes so downstream consumers do not have to re-derive them (closed-world applicability rule, enumerated cadence set, enumerated immutability modes, source-version pins, deprecation-window defaults per family).
- **Explicit non-shape.** At least three things you *chose not to* include in the shape and why (e.g., a `severity` field, a per-control cost estimate, a per-control maturity score).

### `worked-entries.yaml`

Instantiate the shape with three entries chosen to exercise it:

1. **A governance-family entry** — e.g. an `AIC-GOV-*` for AI RACI register or governance council charter.
2. **A data-lifecycle entry** — e.g. an `AIC-DAT-*` for training-data provenance (you may adapt the chapter 01 worked example, but tune the applicability filter to Northbrook's actual footprint).
3. **An engineering-side entry the architect can *only* draft as an outcome** — e.g. an `AIC-ENG-*` for prompt-injection isolation on the customer-facing chat. Chapter 08 says the mechanism is co-authored with the risk engineer, who does not exist yet; write the entry so it is honest about the empty co-author seat.

Every entry must have all seven fields, plus the `ownership` block, plus a `status` (start at `proposed` per chapter 06). Every crosswalk field must cite at least one primary source verified against the source, not paraphrased. Where you cannot verify, use `<!-- needs-research: ... -->` inline — do not guess.

### `shape-review-brief.md`

One page. Must contain:

- The *three most consequential decisions* in the proposal, each with one sentence stating what breaks if the head-of-AI-governance overrides that decision.
- The *one open question* you need level-60 to resolve before publication (there is always at least one — surface it explicitly rather than hoping it will not matter).
- A pointer to the worked entries and a one-line note on which decision each worked entry exercises.

## Starter guidance

- Draft the proposal *before* the worked entries. If you cannot decide the shape in prose, the YAML will be prettier but inconsistent.
- Do not defer decisions to "the library preface will decide." The library preface only records decisions you have already made — this exercise is where you make them.
- The applicability-filter dimensions section is the one that most first-cut proposals under-specify. Enumerate values; do not write "and other tiers as needed."
- Resist the urge to add a `severity` or `criticality` field. Severity belongs to the risk taxonomy (mod-106); a duplicated field on the control drifts within a quarter.
- The Northbrook risk engineer seat is empty on day one. Your ownership matrix must state how you route engineering-side field authorship *until the seat is filled*, and how the routing changes once it is filled. Do not hide the gap.

## Acceptance criteria

- [ ] `catalog-shape-proposal.md` decides all eight bullets under Requirements, each with a stated rationale.
- [ ] Applicability-filter dimensions are named, enumerated, and the closed-world convention is stated once.
- [ ] Family prefixes are enumerated with a status (active on release 1 / reserved for later) per prefix.
- [ ] Versioning scheme defines *breaking* concretely (not "significant change").
- [ ] Field-level ownership matrix covers every field for every role — no `?` cells; empty seats are named as `unassigned — routed via <fallback>`.
- [ ] `worked-entries.yaml` contains three complete entries covering governance, data, and engineering families.
- [ ] Every crosswalk citation resolves to a primary source, or carries a `<!-- needs-research: ... -->` marker.
- [ ] Every entry has a `status: proposed` and an `ownership` block consistent with the matrix in the proposal.
- [ ] `shape-review-brief.md` fits on one page, names three consequential decisions, and surfaces at least one open question.

## Stretch goals

- Sketch what changes in the shape if Northbrook adopts a *second* library (a business-unit overlay per chapter 05) later in the year. Do not build it; note the two decisions in the proposal that would need to be revisited.
- Propose a *diff format* the release-notes contract (chapter 06) will use to render entry-level changes to non-technical reviewers. One paragraph.
- Extend the ownership matrix with a *historic mistake column* naming, per role, one canonical way the seat can drift out of scope on the library and the artefact you have designed to prevent it.
