# mod-106-risk-taxonomy-and-enterprise-appetite: Enterprise AI Risk Taxonomy and Appetite Architecture

> Scaffolded by `aicg org execute-plan`. Lecture chapters and exercise content are authored on subsequent autonomous cycles.

**Estimated effort:** 16 hours

## Learning objectives

- Design the enterprise AI risk taxonomy — canonical harm categories, capability tiers, dependency map — reusing NIST AI RMF Generative AI Profile categories, ISO/IEC 23894 risk sources, AIRO ontology categories, MIT AI Risk Repository meta-taxonomy, and AIID / OECD.AI incident classes as reference corpora
- Design the enterprise AI risk appetite — how the board's risk-appetite statement translates into per-tier tolerances, escalation triggers, and stop-shipping thresholds — layering ISO 31000, ISO/IEC 27005, COSO ERM as parent frameworks
- Design the aggregation model — how risk across systems / products / business units rolls into a portfolio-level view without hiding tail risks; how residual and control-defeated risk stack
- Draw shape lessons from frontier-lab tiered-risk frameworks (Anthropic RSP, OpenAI Preparedness, Google DeepMind Frontier Safety Framework, Frontier Model Forum publications) — pre-registered capability thresholds, rollback criteria, dangerous-capability tripwires — and adapt them to enterprise (not frontier-lab) scope
- Coordinate with `ai-risk-engineer` (level 25) on the quantification contract (impact × likelihood × exposure scoring, inherent vs residual vs control-defeated); with `model-evaluation-engineer` (peer, level 30) on statistical calibration methodology depth; the architect designs the taxonomy + aggregation, engineers do the scoring
- Author the risk-taxonomy versioning + migration plan — how a new risk category is added, deprecated categories retired, historical risk-register entries reclassified

## Structure

- `01-…md` … `0N-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs.
- `quizzes/`: knowledge checks.
- `resources.md`: external references.
