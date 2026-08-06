# exercise-01: EU AI Act Articles → Controls Map

**Estimated effort:** 3 hours

## Objective

Produce the **EU-AI-Act-to-control-library mapping** for a working enterprise control library. For each of the article groups the level-50 architect owns (Articles 9-15 high-risk; Articles 16-29 provider/deployer/notified-body; Article 50 transparency; Articles 51-56 GPAI; registration + Articles 72-73 post-market and serious-incident), decide — atomic requirement by atomic requirement — whether the requirement lands as a shape-A crosswalk edge on an existing control or a shape-B new control, name the applicability filter attributes needed, and produce a shape-B control entry for the genuinely-novel cases.

The deliverable is what the level-50 architect would take into an EU-readiness review with the head-of-AI-governance and internal legal so the enterprise commits to the mapping *before* individual control edits begin.

## Prerequisites

- Chapter [`02-reading-the-eu-ai-act-as-architectural-input.md`](../02-reading-the-eu-ai-act-as-architectural-input.md) read.
- Chapter [`07-designing-the-reconciliation-architecture.md`](../07-designing-the-reconciliation-architecture.md) skimmed for the schemas.
- The mod-102 chapter 01 anatomy-of-a-control-library-entry and chapter 07 evidence-contract chapters as reference — your shape-B entries must conform to those shapes.
- Access to Regulation (EU) 2024/1689 (see [`../resources.md`](../resources.md) for the canonical link).

## Scenario

You are the level-50 architect at a hypothetical enterprise you will name in your deliverable — pick one from:

- **A US regional bank (Northbrook-style, per mod-102 exercise-01)** expanding into the EU next year with a customer-facing GenAI assistant, an internal RAG for legal research, three fraud classifiers, and two third-party AI SaaS integrations.
- **A European medtech company** providing an AI clinical-decision-support product regulated under the MDR (so Annex I harmonised legislation applies).
- **A global HR-tech vendor** whose product performs employment-decision assistance and is sold into the EU, the UK, several US states, and Singapore.

Choose the one whose obligation shape you are least familiar with; that is where the exercise will teach you most. Note your choice at the top of the deliverable.

The enterprise has a working control library. For the purposes of this exercise, assume the library carries the families named in mod-102: `AIC-GOV-*`, `AIC-DAT-*`, `AIC-DOC-*`, `AIC-LOG-*`, `AIC-HOV-*`, `AIC-ROB-*`, `AIC-SEC-*`, `AIC-TRP-*`, `AIC-RSK-*`, `AIC-INC-*`, `AIC-GPAI-*` (if the enterprise is a GPAI provider — decide whether it is).

## Deliverables

1. **`eu-ai-act-obligation-mapping.md`** — the mapping document.
2. **`shape-b-entries.yaml`** — YAML entries for each shape-B control you decided the mapping needs.
3. **`review-brief.md`** — a one-page brief to the head-of-AI-governance.

## Requirements

### `eu-ai-act-obligation-mapping.md`

For each of the following article groups, produce a section with the shape below:

- **Articles 6-7 (classification cascade + Annex III scope)** — treat as one upstream obligation; the mapping is to a single upstream control and to the applicability-filter attribute.
- **Article 9 (risk management system)**
- **Article 10 (data and data governance)** — decompose 10(1)-(5); Article 10(5) special-category-data exemption should get its own atomic entry.
- **Article 11 (technical documentation)**
- **Article 12 (automatically generated logs)**
- **Article 13 (transparency and provision of information to deployers)** — decompose the 13(3) enumeration.
- **Article 14 (human oversight)**
- **Article 15 (accuracy, robustness, and cybersecurity)** — decompose into the three sub-requirements.
- **Article 16 (provider umbrella)** — treat as scope-setter; the mapping is to how the enterprise's provider-role attribute is set on the applicability filter.
- **Article 17 (quality management system)**
- **Articles 18-19 (documentation and log retention)**
- **Article 20 (corrective actions and duty of information)**
- **Article 21 (cooperation with competent authorities)**
- **Article 22 (authorised representatives)** — only relevant if the enterprise is established outside the Union.
- **Articles 23-24 (importer and distributor obligations)** — only relevant if the enterprise plays those roles.
- **Articles 25-27 (deployer obligations)** — decompose; Article 27 FRIA should get its own atomic entry.
- **Articles 28-29 (notified bodies)** — treat as scope-setter for whether third-party assessment is triggered.
- **Article 50 (transparency)** — decompose into the four categories.
- **Articles 51-56 (GPAI)** — only if the enterprise is a GPAI provider; decompose per article. Article 53(1)(d) training-content summary should get its own atomic entry.
- **Registration duties + Article 71 EU database** — pin the article numbers you use.
- **Article 72 (post-market monitoring)**
- **Article 73 (serious-incident reporting)**

For **each atomic requirement** within each section, capture:

| Field | Content |
|---|---|
| Atomic requirement | A one-sentence restatement of what the Regulation requires. |
| Addressee | provider / deployer / importer / distributor / authorised-representative / user (or GPAI-provider / GPAI-provider-with-systemic-risk). |
| Trigger | The system-classification / use-case / scale attribute that turns the obligation on. |
| Demand category | design_state / process / document / test / disclosure / notification / filing. |
| Shape decision | shape_A (extend existing control) or shape_B (new control) — with a one-sentence rationale. |
| Existing control family | For shape_A: the family (`AIC-DAT-*`, `AIC-DOC-*`, etc.) the extension attaches to. |
| New control id (proposed) | For shape_B: the id you would give the new control. |
| Applicability-filter attributes needed | Any new attributes the filter must acquire (name and type). |

The mapping table is the core artefact; each section should also carry two or three sentences of explanation where the shape decision is non-obvious.

### `shape-b-entries.yaml`

For **each shape-B decision** you made in the mapping, produce a complete control entry in the mod-102 chapter 01 shape (statement, applicability filter, evidence contract, implementation guidance skeleton, testing procedure skeleton, ownership, crosswalk). At minimum, produce entries for:

- Article 27 fundamental rights impact assessment (if the enterprise is a deployer of a scope-27 system).
- Article 50 category 2 machine-readable marking of synthetic outputs (if the enterprise is a provider of a GenAI system with output modalities in scope).
- Article 53(1)(d) training-content summary (if the enterprise is a GPAI provider).
- Article 73 serious-incident reporting.
- Any other shape-B control you decided the enterprise needs.

Entries must include full crosswalk (EU AI Act article + at least one related NIST or ISO reference where applicable) and ownership consistent with mod-102's field-level ownership matrix.

### `review-brief.md`

One page. Must contain:

- The **top three shape-B decisions** you made and why each is unavoidable.
- The **top three shape-A crosswalk-edge extensions** and which existing control families they attach to.
- The **open questions for legal** — regulatory-interpretation questions where the shape decision depends on an interpretation you should not make unilaterally (e.g., whether Article 27 FRIA scope covers a specific enterprise use case; whether the enterprise's role for a specific product is provider or deployer).
- The **open questions for the head-of-AI-governance** — enterprise-level scope questions (e.g., whether the enterprise elects to become an Article 56 code-of-practice signatory; whether the enterprise appoints an authorised representative or forgoes EU sales for products where it would).

## Starter guidance

- Start from the article, not from the control. If you look at each existing control and ask "what EU AI Act obligations does this discharge?", you will miss atomic requirements that no existing control touches. Instead: enumerate atomic requirements first, then map each to a control decision.
- Do not conflate "different demand category" with shape B. An existing control that produces a document can extend its evidence contract to also produce a filing under Article 73 — that is shape A with a rendering extension, not shape B.
- Article 15 is the classic decomposition trap. Do not treat "accuracy, robustness, cybersecurity" as one obligation — the evidence contracts diverge and the shape decision may differ per sub-requirement.
- The GPAI decision (whether Articles 51-56 apply at all) turns on whether the enterprise is a GPAI provider. If it fine-tunes but does not train the base model, the analysis is nuanced — flag it as a legal question rather than deciding unilaterally.
- Where a shape decision depends on the enterprise's role (provider vs deployer), and the enterprise plays both roles across different products, produce the mapping *per role*. The applicability filter carries the role attribute per system; the mapping document must be equally explicit.
- Use `<!-- needs-research: ... -->` markers freely for regulatory citations you cannot verify from the Regulation text at reading time. Do not invent article numbers or paragraph references.

## Acceptance criteria

- [ ] The mapping covers every article group listed in the Requirements — no gaps, no `TBD`s.
- [ ] Each atomic requirement is decomposed to a single obligation with a single addressee, trigger, demand category, and shape decision. Multi-paragraph articles produce multiple rows.
- [ ] Every shape-B decision carries a written rationale for why shape A is not sufficient. Bare "shape B" without rationale is not accepted.
- [ ] Every shape-B decision has a corresponding entry in `shape-b-entries.yaml` with all fields from mod-102 chapter 01.
- [ ] The applicability-filter attributes section enumerates every new attribute needed and its type — no free-text placeholders.
- [ ] The one-page brief contains at least one open question for legal and at least one for the head-of-AI-governance. If it does not, either you have not looked hard enough or you have decided things above your pay grade.
- [ ] Regulatory citations that could not be verified against the Regulation text are marked `<!-- needs-research: ... -->` rather than left as invented references.
- [ ] The choice of scenario (US regional bank, European medtech, global HR-tech) is stated at the top and the shape decisions reflect that scenario consistently.

## Stretch goals

- Add a *sensitivity analysis* section to the mapping document: for the three most consequential shape decisions, describe how the decision changes if the enterprise's role, sector, or GPAI-status differs.
- Produce a *first-pass obligation register* in the schema from chapter 07 for the ten atomic requirements you consider most consequential. Do not fill the entire Act; the exercise is about shape, not scale.
- Sketch what changes in the mapping if the CEN-CENELEC JTC 21 harmonised standard on risk management is published and referenced in the OJEU next quarter. One paragraph.
