# mod-106-risk-taxonomy-and-enterprise-appetite: Enterprise AI Risk Taxonomy and Appetite Architecture

**Estimated effort:** 16 hours

## Why this module exists

An enterprise AI risk programme lives or dies on two artefacts the level-50 architect owns: the **risk taxonomy** — the closed-world vocabulary that names, categorises, and relates every AI-specific harm the enterprise is willing to reason about — and the **risk-appetite architecture** — the machinery that translates the board's one-sentence appetite statement into per-tier tolerances, escalation triggers, and stop-shipping thresholds that engineers on Monday morning can actually apply. Without a taxonomy, every risk register drifts to free-text and the portfolio view is a hallucination. Without an appetite architecture, "acceptable" is decided ad hoc at every launch review and the same risk is accepted for Product A on Tuesday and rejected for Product B on Thursday with no defensible reason. This module builds both.

The architect *does not* score individual risks (`ai-risk-engineer` at level 25 does), *does not* set the appetite (the board and audit committee do, informed by `head-of-ai-governance` at level 60), and *does not* run the calibration studies that keep scoring consistent (`model-evaluation-engineer` at peer level 30 does). The architect designs the *taxonomy shape*, the *appetite translation logic*, the *aggregation model*, the *quantification contract* the engineering roles fill in, and the *versioning + migration plan* that keeps the taxonomy usable as the enterprise, the regulations, and the underlying AI systems evolve. This module walks that shape end-to-end and lands in a taxonomy + appetite pair that mod-107 (assurance), mod-108 (evidence), mod-110 (monitoring), and mod-111 (GRC platform) can all bind to.

## Learning objectives

- Design the enterprise AI risk taxonomy — canonical harm categories, capability tiers, dependency map — reusing NIST AI RMF Generative AI Profile categories, ISO/IEC 23894 risk sources, AIRO ontology categories, MIT AI Risk Repository meta-taxonomy, and AIID / OECD.AI incident classes as reference corpora.
- Design the enterprise AI risk appetite — how the board's risk-appetite statement translates into per-tier tolerances, escalation triggers, and stop-shipping thresholds — layering ISO 31000, ISO/IEC 27005, COSO ERM as parent frameworks.
- Design the aggregation model — how risk across systems / products / business units rolls into a portfolio-level view without hiding tail risks; how residual and control-defeated risk stack.
- Draw shape lessons from frontier-lab tiered-risk frameworks (Anthropic RSP, OpenAI Preparedness, Google DeepMind Frontier Safety Framework, Frontier Model Forum publications) — pre-registered capability thresholds, rollback criteria, dangerous-capability tripwires — and adapt them to enterprise (not frontier-lab) scope.
- Coordinate with `ai-risk-engineer` (level 25) on the quantification contract (impact × likelihood × exposure scoring, inherent vs residual vs control-defeated); with `model-evaluation-engineer` (peer, level 30) on statistical calibration methodology depth; the architect designs the taxonomy + aggregation, engineers do the scoring.
- Author the risk-taxonomy versioning + migration plan — how a new risk category is added, deprecated categories retired, historical risk-register entries reclassified.

## Chapters

1. [`01-the-risk-taxonomy-problem-and-the-architects-lens.md`](01-the-risk-taxonomy-problem-and-the-architects-lens.md) — Why the enterprise AI risk taxonomy is a distinct architectural artefact from the control library and from the incident classification scheme, the six invariants a workable taxonomy holds, and the two failure modes to design against.
2. [`02-harm-categories-capability-tiers-and-the-dependency-map.md`](02-harm-categories-capability-tiers-and-the-dependency-map.md) — The three-axis shape of the taxonomy (harm category × capability tier × dependency map), composed on top of NIST AI RMF Generative AI Profile categories, ISO/IEC 23894 risk sources, AIRO categories, MIT AI Risk Repository meta-taxonomy, and AIID / OECD.AI incident classes as reference corpora.
3. [`03-appetite-translation-from-board-statement-to-bench-triggers.md`](03-appetite-translation-from-board-statement-to-bench-triggers.md) — Composing ISO 31000 (parent risk-management framework), ISO/IEC 27005 (information-security-risk method), and COSO ERM (enterprise-risk-management) as the parent layers; the four artefacts of the appetite architecture (statement → tolerance table → escalation triggers → stop-shipping thresholds); who authors, who ratifies, who operates.
4. [`04-aggregation-and-the-portfolio-view.md`](04-aggregation-and-the-portfolio-view.md) — Rolling risk across systems, products, and business units to a portfolio-level view without hiding tail risks; the three legitimate aggregation shapes (sum-of-worst-case, exposure-weighted, dependency-graph); how inherent, residual, and control-defeated risks stack; why the FAIR-shaped monetisation is a stretch, not a default.
5. [`05-frontier-lab-tiered-risk-frameworks-as-shape-templates.md`](05-frontier-lab-tiered-risk-frameworks-as-shape-templates.md) — Anthropic Responsible Scaling Policy, OpenAI Preparedness Framework, Google DeepMind Frontier Safety Framework, and Frontier Model Forum publications read as *shape templates*: pre-registered capability thresholds, rollback criteria, dangerous-capability tripwires. What the enterprise adopts, what it rejects, and why the frontier-lab framework does not survive naïve transplantation to enterprise scope.
6. [`06-the-quantification-contract-with-the-ai-risk-engineer.md`](06-the-quantification-contract-with-the-ai-risk-engineer.md) — The interface between the architect (who designs the taxonomy + aggregation) and the `ai-risk-engineer` (level 25, who scores). Impact × likelihood × exposure scoring; inherent vs residual vs control-defeated risk; the boundary with `model-evaluation-engineer` (peer, level 30) on calibration depth.
7. [`07-taxonomy-versioning-and-migration-plan.md`](07-taxonomy-versioning-and-migration-plan.md) — Adding a new risk category (proposal → review → publish → migrate), retiring deprecated categories, reclassifying historical risk-register entries. The semver-shaped taxonomy release process, the change-communications contract, and the two-way-traceability guarantee across versions.

## Exercises

- [`exercises/exercise-01-risk-taxonomy-shape-proposal.md`](exercises/exercise-01-risk-taxonomy-shape-proposal.md) — draft the enterprise AI risk taxonomy shape proposal, composing the reference corpora into a defensible closed-world vocabulary.
- [`exercises/exercise-02-risk-appetite-translation-drill.md`](exercises/exercise-02-risk-appetite-translation-drill.md) — translate a board risk-appetite statement into per-tier tolerances, escalation triggers, and stop-shipping thresholds.
- [`exercises/exercise-03-portfolio-aggregation-model-drill.md`](exercises/exercise-03-portfolio-aggregation-model-drill.md) — design the aggregation model that rolls per-system risk into a portfolio view without hiding tail risks.
- [`exercises/exercise-04-frontier-lab-tiered-risk-shape-adaptation.md`](exercises/exercise-04-frontier-lab-tiered-risk-shape-adaptation.md) — adapt the tiered-risk shape from an Anthropic-, OpenAI-, or DeepMind-style framework to enterprise (non-frontier-lab) scope.
- [`exercises/exercise-05-taxonomy-versioning-and-migration-plan.md`](exercises/exercise-05-taxonomy-versioning-and-migration-plan.md) — author the taxonomy versioning process and the migration playbook for adding, deprecating, and reclassifying risk categories.

## Structure

- `01-…md` … `07-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs (planned).
- `quizzes/`: knowledge checks (planned).
- `resources.md`: external references.
