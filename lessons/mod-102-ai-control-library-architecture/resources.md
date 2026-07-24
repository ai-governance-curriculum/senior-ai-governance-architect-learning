# Resources — mod-102 Enterprise AI Control Library Architecture

Primary sources only — the composition discipline this module builds requires you to work from the source texts, not from paraphrases. Every URL below is the publisher-hosted canonical location; if a link changes or a version supersedes, update in-repo rather than silently letting it drift.

## The governance-family sources composed by the library

- [NIST AI Risk Management Framework 1.0 (AI 100-1, Jan 2023)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf) — GOVERN / MAP / MEASURE / MANAGE functions and sub-categories that populate crosswalks.
- [NIST AI RMF Playbook](https://airc.nist.gov/AI_RMF_Knowledge_Base/Playbook) — suggested actions per sub-category (non-normative but useful when authoring implementation guidance).
- [NIST AI RMF Generative AI Profile (AI 600-1, Jul 2024)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf) — GenAI-specific overlay of MAP / MEASURE / MANAGE actions.
- [ISO/IEC 42001:2023](https://www.iso.org/standard/81230.html) *(paywalled)* — AIMS management-system standard, Annex A controls.
- [ISO/IEC 23894:2023](https://www.iso.org/standard/77304.html) *(paywalled)* — AI risk management guidance, source-of-harm shape.
- [Regulation (EU) 2024/1689 — the EU AI Act](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — Articles 9-15 provider obligations for high-risk systems; Article 11 technical documentation; Article 12 automatically generated logs; Article 72 post-market monitoring; Article 73 serious-incident reporting.
- [European Commission AI Act page](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai) — official summaries and delegated / implementing act status tracker.

## The threat-family sources composed by the library

- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) — pin to the specific list version in the catalog preface.
- [OWASP Machine Learning Security Top 10](https://owasp.org/www-project-machine-learning-security-top-10/) — classical-ML companion; useful for non-LLM engineering-side entries.
- [MITRE ATLAS](https://atlas.mitre.org/) — live matrix of adversarial tactics, techniques, mitigations, and case studies for ML.
- [MITRE ATLAS Navigator](https://mitre-atlas.github.io/atlas-navigator/) — heat-map view; useful for producing the technique-coverage map.
- [Google Secure AI Framework (SAIF)](https://safety.google/cybersecurity-advancements/saif/) — risk areas and mitigations organised across the AI development lifecycle.
- [Google SAIF risk map interactive](https://saif.google/) — companion tool to walk the risk areas against your system.
- [CISA / NCSC — Guidelines for Secure AI System Development](https://www.cisa.gov/resources-tools/resources/guidelines-secure-ai-system-development) — joint-agency provider-side secure-development recommendations across design / development / deployment / operation.
- [ENISA — Multilayer Framework for Good Cybersecurity Practices for AI](https://www.enisa.europa.eu/publications/multilayer-framework-for-good-cybersecurity-practices-for-ai) — organisational / technical / procedural layered practices.
- [ENISA — Securing Machine Learning Algorithms](https://www.enisa.europa.eu/publications/securing-machine-learning-algorithms) — companion ENISA publication on ML-specific threats and mitigations.

## OSCAL and the machine-readable representation layer

- [OSCAL — Open Security Controls Assessment Language](https://pages.nist.gov/OSCAL/) — model reference; catalog, profile, component-definition, SSP, assessment-plan, assessment-results, and POA&M shapes.
- [OSCAL reference (catalog model)](https://pages.nist.gov/OSCAL-Reference/models/latest/catalog/) — the model the AI control library serialises into.
- [OSCAL reference (profile model)](https://pages.nist.gov/OSCAL-Reference/models/latest/profile/) — the model the baseline profiles serialise into.
- [OSCAL content GitHub — NIST-published catalogs](https://github.com/usnistgov/oscal-content) — SP 800-53 rev5, FedRAMP baselines, and CSF 2.0 sample content.
- [OSCAL developer tools index](https://pages.nist.gov/OSCAL/tools/) — validators, converters, editors.

## Composition anchors — NIST catalog + CSF + Playbook

- [NIST SP 800-53 rev5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) — the control catalog the AI library composes with via `link rel="related"`.
- [NIST SP 800-37 rev2 — Risk Management Framework](https://csrc.nist.gov/publications/detail/sp/800-37/rev-2/final) — process shape the AI library implicitly follows.
- [NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework) — executive overlay; the AI library's `csf_v2` crosswalk edges land here.

## AI documentation shapes referenced in the evidence contract

- [Mitchell et al. — Model Cards for Model Reporting (arXiv:1810.03993)](https://arxiv.org/abs/1810.03993) — the model-card shape mod-108 designs the enterprise instance of.
- [Gebru et al. — Datasheets for Datasets (arXiv:1803.09010)](https://arxiv.org/abs/1803.09010) — the dataset-datasheet shape.
- [Hugging Face — Model Cards guide](https://huggingface.co/docs/hub/en/model-cards) — a widely adopted operational variant.
- [Partnership on AI — ABOUT ML](https://partnershiponai.org/paper/about-ml/) — reporting practices reference.
- [UK Algorithmic Transparency Recording Standard (ATRS)](https://www.gov.uk/government/publications/algorithmic-transparency-recording-standard-hub/algorithmic-transparency-recording-standard-guidance-for-public-sector-bodies) — public-sector-facing artefact shape.

## AI supply-chain and attestation references

- [CycloneDX ML-BOM](https://cyclonedx.org/capabilities/mlbom/) — machine-learning BOM specification for the AI supply chain.
- [SPDX 3.0 AI profile](https://spdx.dev/use/specifications/) — SPDX specification hub; AI profile lives in the SPDX 3.0 series.
- [SLSA — Supply-chain Levels for Software Artifacts](https://slsa.dev/) — supply-chain integrity attestation framework.
- [Sigstore project](https://www.sigstore.dev/) — signing and transparency-log platform used at enterprise scale.

## Certification and audit-body references

- [ISO/IEC 42006](https://www.iso.org/standard/44546.html) *(paywalled; check current publication status)* — requirements for bodies providing audit and certification of AIMS. The library's SoA must be auditable under this standard.
- [IAF — International Accreditation Forum](https://iaf.nu/) — cross-border accreditation-body index that certification bodies operate under.
- [ForHumanity — Independent AI Auditor / IAAIS](https://forhumanity.center/) — independent audit standard used as a shape reference for third-line audit programmes.

## Field references used in the worked examples

- [EU AI Act Article 10](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — data-and-data-governance obligations for high-risk systems, referenced by the training-data-provenance worked example throughout chapters 01–07.
- [EU AI Act Article 12](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — automatically generated logs; the minimum-retention text referenced in chapter 07. <!-- needs-research: verify the current Article 12 minimum-retention wording against the consolidated Regulation. -->
- [EU AI Act Article 15](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — accuracy, robustness, and cybersecurity of high-risk systems; referenced by the prompt-injection and adversarial-robustness worked examples.
