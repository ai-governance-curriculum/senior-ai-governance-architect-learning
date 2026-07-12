# exercise-02: NIST AI RMF as Architectural Input Annotation

**Estimated effort:** 2 hours

## Objective

Convert a fixed set of NIST AI RMF 1.0 sub-categories into **architectural inputs** — draft control-library entries, taxonomy tags, RACI cells, and evidence contracts — for a fictional but realistic enterprise. Produce a repeatable *annotation shape* you can apply to the rest of the Framework in later modules.

The exercise is not "read the sub-category and paraphrase it." It is: read the sub-category, decide what has to exist inside the enterprise to make the outcome real, and specify the artefacts.

## Prerequisites

- Chapter [`02-nist-ai-rmf-as-architectural-input.md`](../02-nist-ai-rmf-as-architectural-input.md) read once.
- Access to NIST AI RMF 1.0 (AI 100-1) itself: <https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf>.
- Access to the NIST AI RMF Playbook: <https://airc.nist.gov/AI_RMF_Knowledge_Base/Playbook>.
- Access to the NIST AI RMF Generative AI Profile (AI 600-1): <https://airc.nist.gov/AI_RMF_Knowledge_Base/AI_RMF/Uses/GenAI-Profile>.

## Scenario

You are the newly hired senior AI governance / risk architect at **"Northbrook Financial"** — a US regional bank with:

- Existing ISO/IEC 27001 ISMS, SR 11-7-aligned Model Risk Management framework.
- Roughly forty AI systems in the internal inventory: fraud-detection classifiers, document extraction, an internal RAG assistant, a customer-facing chat assistant (generative), and several third-party AI SaaS integrations.
- Colorado + New York City deployments in scope (so US state AI regulation applies), and expanding into the EU next year (so EU AI Act applies).
- A CISO, a CRO, a CDO, and a newly appointed head of AI governance (level 60). No dedicated risk engineer yet; two governance analysts.

## Deliverables

Author two artefacts in a working directory of your choice:

1. **`rmf-annotation-table.md`** — an annotation table covering the six sub-categories listed below.
2. **`rmf-annotation-method-notes.md`** — a short (≤ 300-word) methods note that other architects could pick up.

## Requirements

### Sub-categories to annotate (mandatory)

Annotate all six:

| Function | Sub-category | Focus |
|---|---|---|
| GOVERN | GOVERN-1.1 | Policies and procedures |
| GOVERN | GOVERN-3.2 | Roles / responsibilities across the AI lifecycle |
| MAP | MAP-1.1 | Context of use established / documented |
| MAP | MAP-2.3 | Scientific / technical / socio-technical / legal capabilities and limitations |
| MEASURE | MEASURE-2.11 | Fairness / bias assessed |
| MANAGE | MANAGE-2.3 | Incident response and prevention |

For **each** sub-category, the annotation row must include:

- **Sub-category ID and outcome statement** (verbatim from the Framework).
- **Enterprise operating-model location** — which function of the operating model owns it (following chapter 02's mapping — GOVERN cross-cutting, MAP joint intake, etc.). Name the specific roles.
- **Existing enterprise controls** that partially satisfy the outcome. For Northbrook Financial, cite parents from SR 11-7, ISO/IEC 27001 Annex A, or NIST SP 800-53 rev5 by ID where credible.
- **Gaps at architect scope** — a plain-language description of what is missing.
- **Proposed new / extended control entries** — control ID (invent one following an `AIC-<FAMILY>-###` convention), one-sentence statement, and a one-sentence applicability filter.
- **Evidence contract** — the minimum artefacts + owner + retention that would satisfy the outcome.
- **Crosswalk** — the sibling standards the entry composes with. Include at minimum NIST AI RMF (self), ISO/IEC 42001 Annex A control ID, EU AI Act article (where applicable), and one CSF 2.0 subcategory.
- **Playbook use** — one action from the Playbook you *adopted* and one you *deliberately rejected*, with a one-line justification per rejection.
- **Generative-AI Profile treatment** — for MEASURE-2.11 and MANAGE-2.3 at minimum, note the Profile-specific applicability filter or new-control addition per the "superset applicability filter, not a fork" rule from chapter 02.

### `rmf-annotation-method-notes.md`

- Explain the annotation shape you followed and why. (Someone picking up the annotation for the rest of the Framework should be able to continue without reading chapter 02.)
- Name at least one Framework-side ambiguity you had to resolve (e.g. sub-category outcome that read plausibly two ways).
- Name at least one architecture-side ambiguity that annotation surfaced (e.g. missing enterprise data you'd need before finalising).

## Starter guidance

- Annotate GOVERN-1.1 first. It is the simplest and the pattern will be reusable.
- Do not annotate more than six sub-categories in this exercise. The point is to build a shape you can extend later, not to complete the Framework.
- The evidence contract row is the most-often-skipped and most-often-argued-about column. Fill it every time; keep artefact names concrete.
- When you cite an ISO/IEC 42001 Annex A control ID or an EU AI Act article, verify the citation against the primary source before submitting. Do not invent IDs. If uncertain, mark with `<!-- needs-research: ... -->` rather than guessing.
- The Playbook column is where reading discipline shows. Adopt one, reject one, per sub-category. If you find yourself adopting everything, you are compliance-scoring, not architecting.

## Acceptance criteria

- [ ] All six sub-categories annotated with every required column filled.
- [ ] Crosswalks cite ISO/IEC 42001 Annex A control IDs and EU AI Act articles verified against primary sources, not paraphrased.
- [ ] Every proposed new / extended control entry has a unique ID, a one-sentence statement, and an applicability filter.
- [ ] Evidence contract lists at least one concrete artefact per sub-category, with a named owner and a retention period.
- [ ] Playbook adoption / rejection is filled in for every sub-category with justification.
- [ ] Generative AI Profile treatment appears on at least MEASURE-2.11 and MANAGE-2.3.
- [ ] Methods note is ≤ 300 words and names at least one Framework-side and one architecture-side ambiguity.

## Stretch goals

- Extend the annotation to two additional sub-categories of your choice from **MEASURE** — one from a category that mostly reuses classical-ML machinery (e.g. MEASURE-2.7 security) and one that is Profile-specific (e.g. a confabulation-related MEASURE sub-category).
- Sketch what an OSCAL representation of one annotated control entry would look like (see chapter 04 for the shape). Do not fully produce OSCAL — a JSON / YAML sketch is enough.
- Propose one **enterprise-specific applicability filter** that would attach at *every* sub-category (e.g. `system_tier: [tier-1, tier-2]`) and argue where it should live: in the OSCAL profile, in individual controls, or in the SoA. Justify against composition patterns from chapter 04.
