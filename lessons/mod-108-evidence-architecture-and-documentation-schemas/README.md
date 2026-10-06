# mod-108-evidence-architecture-and-documentation-schemas: AI Evidence Architecture and Documentation Schemas

**Estimated effort:** 16 hours

## Why this module exists

An enterprise AI programme without an *evidence architecture* fails in a distinctive way. Every layer produces evidence — model owners keep evaluation notebooks, analysts collect model cards, the platform team logs training runs, the compliance team files SR 11-7 model documentation and an ISO/IEC 42001 documented-information register, and (where in scope) the EU AI Act Article 11 technical documentation dossier. Each artefact is defensible in isolation. In aggregate they tell the enterprise's story three different ways, and at the seam where any two regulators compare notes the divergence is what the enterprise gets asked about.

The level-50 architect owns the *surface* the evidence sits on. This module designs that surface — the substrate (audit logs, immutable stores, model and dataset registries), the schema catalog (cards, ML-BOMs, SLSA attestations, regulator-facing packagings), the provenance and immutability layer (Sigstore, Rekor, RFC 3161 timestamps, hash-chained ledgers), the packaging layer (view definitions over the substrate), the role model (who authors, who reviews, who signs), and the machine-readable catalog (OSCAL) that lets automation reason over the whole thing. The three convergent regimes — EU AI Act, SR 11-7, ISO/IEC 42001 — demand the same underlying evidence in three different packagings; the architecture accepts that by making the substrate canonical and the packagings views.

The architect does not author individual cards or evidence records — the `ai-governance-analyst` (level 15) does. The architect does not produce the risk-engineering slice — the `ai-risk-engineer` (level 25) does. The architect does not package the assurance slice — the peer `ai-evaluation-engineer` (level 35) does. The architect designs the *shape* each of them operates against — the schemas, the catalog, the publication gates, the view definitions, the coordination contracts — and holds the invariants that keep the surface defensible.

## Learning objectives

- Design the enterprise AI evidence architecture — audit-log architecture, evidence retention + immutability, chain-of-custody, evidence-storage tiering — that satisfies EU AI Act evidence obligations, SR 11-7 documentation obligations, and ISO/IEC 42001 records requirements simultaneously.
- Author the model card / system card / dataset card / risk-card schemas — required fields, review workflow, publication gate, machine-readable representation — grounded in Mitchell et al. Model Cards, Gebru et al. Datasheets for Datasets, Hugging Face Model Cards guide, Partnership on AI ABOUT ML, and the UK Algorithmic Transparency Recording Standard.
- Design the AI supply-chain evidence slice at enterprise scale — CycloneDX ML-BOM, SPDX 3.0 AI profile, SLSA levels, Sigstore signing — where the evidence lives, who produces it, who verifies it, how the model registry gates ingestion.
- Design the regulator-facing artefact templates — EU AI Act Article 11 technical documentation shape, Article 72 post-market surveillance shape, Article 73 serious-incident report shape, SR 11-7 model documentation shape, FDA PCCP submission shape — so the enterprise is not authoring one-offs.
- Represent the evidence-schema catalog in OSCAL or an equivalent machine-readable format so downstream automation can reason about it.
- Coordinate with `ai-governance-analyst` (level 15) — that role authors evidence records against these schemas — and with `ai-risk-engineer` (level 25) — that role produces the risk-engineering slice — and with `ai-evaluation-engineer` (level 35) — that role packages the assurance slice.

## Chapters

1. [`01-the-evidence-architecture-lens.md`](01-the-evidence-architecture-lens.md) — The architecture-of-the-surface framing: substrate, schema catalog, provenance / immutability layer, packaging layer, role model, machine-readable catalog. The three convergent regimes (EU AI Act, SR 11-7, ISO/IEC 42001) and the sector overlays. Five architectural stances; six invariants; two failure modes (three-stories, hand-assembled packet).
2. [`02-audit-log-architecture-retention-and-immutability.md`](02-audit-log-architecture-retention-and-immutability.md) — The audit-log substrate. Three log families (model/system lifecycle, data lifecycle, governance workflow). The shared event contract, the retention matrix, the immutability posture per family (WORM, hash-chain, external notarisation), and the chain-of-custody discipline across production, ingest, query, and handoff.
3. [`03-the-card-family-model-system-dataset-and-risk-cards.md`](03-the-card-family-model-system-dataset-and-risk-cards.md) — The card family as authoritative disclosures. Model cards (Mitchell), dataset cards (Gebru datasheets), system cards (ATRS, frontier-lab exemplars), and risk cards (the architect's local extension). Required fields, review workflow, publication gate, versioning, and machine-readable front-matter that the OSCAL catalog ingests.
4. [`04-ai-supply-chain-evidence-cyclonedx-spdx-slsa-sigstore.md`](04-ai-supply-chain-evidence-cyclonedx-spdx-slsa-sigstore.md) — The AI supply-chain slice at enterprise scale. CycloneDX ML-BOM and SPDX 3.0 AI profile as the two SBOM shapes; SLSA levels as the build-provenance ladder; Sigstore / in-toto as the signing and transparency layer. The model-registry-as-gate pattern, producer-verifier-gating-actor topology, and the SSDF (SP 800-218) composition.
5. [`05-regulator-facing-artifact-templates.md`](05-regulator-facing-artifact-templates.md) — The one-source-many-packagings discipline. EU AI Act Article 11 Annex IV; Article 72 post-market monitoring plan; Article 73 serious-incident report; SR 11-7 model documentation; FDA PCCP submission. Templates as executable view definitions over the substrate, with reproducibility and signature invariants.
6. [`06-oscal-and-machine-readable-evidence-catalog.md`](06-oscal-and-machine-readable-evidence-catalog.md) — The machine-readable catalog layer. OSCAL layered model (Catalog, Profile, Component Definition, SSP, Assessment Plan, Assessment Results, POA&M) applied to AI-specific evidence obligations. The three killer queries (coverage, freshness, obligation-crosswalk) and the two failure modes (OSCAL museum, format-fetish).
7. [`07-coordination-contracts-with-analyst-risk-engineer-and-evaluation-engineer.md`](07-coordination-contracts-with-analyst-risk-engineer-and-evaluation-engineer.md) — The coordination surface. Interface contracts with the level-15 analyst (who authors records against the schemas), the level-25 risk engineer (who produces the risk-engineering slice), and the peer level-35 evaluation engineer (who packages the assurance slice). Reciprocal boundaries, escalation discipline, and the "contract is the deliverable" stance.

## Exercises

- [`exercises/exercise-01-audit-log-architecture-drill.md`](exercises/exercise-01-audit-log-architecture-drill.md) — Design the audit-log substrate for a chosen scenario (US regional bank; healthcare payer/provider; B2B SaaS HR-tech). Five deliverables: decision document, event schema, retention matrix traced to citation, immutability and chain-of-custody design, storage-tiering and access-model design.
- [`exercises/exercise-02-model-and-system-and-dataset-card-schema-authoring.md`](exercises/exercise-02-model-and-system-and-dataset-card-schema-authoring.md) — Author the card-family JSON Schemas (model, system, dataset, risk) plus one worked instance per card class. Grounded in Mitchell, Gebru, Hugging Face, ABOUT ML, and ATRS references.
- [`exercises/exercise-03-ml-bom-plus-spdx-plus-slsa-plus-sigstore-enterprise-slice.md`](exercises/exercise-03-ml-bom-plus-spdx-plus-slsa-plus-sigstore-enterprise-slice.md) — Draw the AI supply-chain evidence slice: the producer-verifier-gating-actor topology across CycloneDX ML-BOM, SPDX 3.0 AI, SLSA, and Sigstore; the ingestion policy the model registry enforces.
- [`exercises/exercise-04-regulator-facing-artifact-template-authoring.md`](exercises/exercise-04-regulator-facing-artifact-template-authoring.md) — Author executable view definitions for the five regulator-facing packagings (Article 11, Article 72, Article 73, SR 11-7, FDA PCCP) plus one worked reproducible packet per in-scope template.
- [`exercises/exercise-05-evidence-schema-oscal-representation.md`](exercises/exercise-05-evidence-schema-oscal-representation.md) — Author the OSCAL representation of the enterprise evidence catalog: a catalog of AI-specific controls, one or two profiles, one or two SSPs, and at least three executable queries satisfying the "three killer queries" from chapter 06.

## Structure

- `01-…md` … `07-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs (planned).
- `quizzes/`: knowledge checks (planned).
- `resources.md`: external references.
