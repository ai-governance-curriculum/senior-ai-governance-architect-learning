# mod-108-evidence-architecture-and-documentation-schemas: AI Evidence Architecture and Documentation Schemas

> Scaffolded by `aicg org execute-plan`. Lecture chapters and exercise content are authored on subsequent autonomous cycles.

**Estimated effort:** 16 hours

## Learning objectives

- Design the enterprise AI evidence architecture — audit-log architecture, evidence retention + immutability, chain-of-custody, evidence-storage tiering — that satisfies EU AI Act evidence obligations, SR 11-7 documentation obligations, and ISO/IEC 42001 records requirements simultaneously
- Author the model card / system card / dataset card / risk-card schemas — required fields, review workflow, publication gate, machine-readable representation — grounded in Mitchell et al. Model Cards, Gebru et al. Datasheets for Datasets, Hugging Face Model Cards guide, Partnership on AI ABOUT ML, and the UK Algorithmic Transparency Recording Standard
- Design the AI supply-chain evidence slice at enterprise scale — CycloneDX ML-BOM, SPDX 3.0 AI profile, SLSA levels, Sigstore signing — where the evidence lives, who produces it, who verifies it, how the model registry gates ingestion
- Design the regulator-facing artifact templates — EU AI Act Article 11 technical documentation shape, Article 72 post-market surveillance shape, Article 73 serious-incident report shape, SR 11-7 model documentation shape, FDA PCCP submission shape — so the enterprise is not authoring one-offs
- Represent the evidence-schema catalog in OSCAL or an equivalent machine-readable format so downstream automation can reason about it
- Coordinate with `ai-governance-analyst` (level 15) — that role authors evidence records against these schemas — and with `ai-risk-engineer` (level 25) — that role produces the risk-engineering slice — and with `ai-evaluation-engineer` (level 35) — that role packages the assurance slice

## Structure

- `01-…md` … `0N-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs.
- `quizzes/`: knowledge checks.
- `resources.md`: external references.
