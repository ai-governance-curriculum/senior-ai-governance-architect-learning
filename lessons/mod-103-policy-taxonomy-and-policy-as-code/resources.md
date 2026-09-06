# Resources — mod-103 AI Policy Taxonomy and Policy-as-Code Architecture

Primary sources only. This module authors policy against external anchor frames — international-consensus principles, ISO/IEC governance and vocabulary standards, IEEE ethics methodologies, jurisdictional statutes, and open-source policy-as-code engines. Every URL below is the publisher-hosted canonical location; if a link changes or a version supersedes, update in-repo rather than silently letting it drift.

## Responsible AI principle anchor sources (Layer 1)

- [OECD AI Principles (2019, updated May 2024)](https://oecd.ai/en/ai-principles) — the five values-based principles and five recommendations to governments. Referenced by chapter 01 as an anchor for the enterprise principle register.
- [OECD Recommendation of the Council on Artificial Intelligence (OECD/LEGAL/0449)](https://legalinstruments.oecd.org/en/instruments/OECD-LEGAL-0449) — the formal legal instrument text of the OECD AI Principles.
- [UNESCO Recommendation on the Ethics of Artificial Intelligence (2021)](https://www.unesco.org/en/artificial-intelligence/recommendation-ethics) — adopted November 2021. Broader value / principle set anchoring chapter 01's second reference frame.
- [NIST AI Risk Management Framework 1.0 (AI 100-1, Jan 2023)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf) — the seven trustworthy characteristics used as the third anchor frame in chapter 01.
- [G7 Hiroshima Process — International Guiding Principles and Code of Conduct for Advanced AI Systems](https://digital-strategy.ec.europa.eu/en/library/hiroshima-process-international-guiding-principles-advanced-ai-system) — cross-jurisdictional principle alignment referenced when composing enterprise principles across G7-member regulator posture.

## Policy taxonomy and instrument-shape references

- [ISO/IEC Directives Part 2 — Principles and rules for the structure and drafting of ISO and IEC documents](https://www.iso.org/sites/directives/current/part2/index.xhtml) — the ISO drafting convention that anchors the `shall` / `should` / `may` verb discipline chapter 01 uses to separate layer 2 (binding policy) from layers 3–5. Publicly available.
- [ISO/IEC 27002:2022](https://www.iso.org/standard/75652.html) *(paywalled)* — the information-security controls standard, cited as a shape reference for how a mature standards catalogue composes with a management-system standard (27001) — a pattern the AI programme mirrors with 42001.

## Governance-body and management-system reference frames (chapter 06)

- [ISO/IEC 38507:2022 — Governance implications of the use of AI by organizations](https://www.iso.org/standard/56641.html) *(paywalled)* — the governing-body-tier standard chapter 06 positions as the frame the board AI committee charter is shaped against, distinct from the management-system-tier 42001.
- [ISO/IEC 22989:2022 — Artificial intelligence concepts and terminology](https://www.iso.org/standard/74296.html) *(paywalled)* — the vocabulary standard chapter 06 anchors policy and standard definitions to.
- [ISO/IEC 23053:2022 — Framework for AI systems using machine learning (ML)](https://www.iso.org/standard/74438.html) *(paywalled)* — companion reference architecture cited when procedures need to name ML-system components consistently.
- [ISO/IEC 42001:2023 — Information technology — Artificial intelligence — Management system](https://www.iso.org/standard/81230.html) *(paywalled)* — the AIMS standard the policy taxonomy feeds into via Clause 7.5 documented information; worked in depth in mod-105.
- [ISO/IEC 23894:2023 — Guidance on AI risk management](https://www.iso.org/standard/77304.html) *(paywalled)* — the risk-management guidance bridging ISO 31000 to AI; anchors mod-106 and is referenced when standards need risk-shape language.
- [ISO/IEC 42005:2025 — AI system impact assessment](https://www.iso.org/standard/44545.html) *(paywalled)* <!-- needs-research: confirm publication year against the ISO catalogue at time of use --> — the impact-assessment process standard the enterprise impact-assessment schema conforms to.
- [ISO/IEC 5259 series — Data quality for analytics and machine learning](https://www.iso.org/committee/6794475/x/catalogue/) *(paywalled)* <!-- needs-research: confirm currently published parts of the series --> — multi-part standard feeding data-quality standards under the taxonomy's data domain.
- [ISO/IEC TR 24028:2020 — Overview of trustworthiness in artificial intelligence](https://www.iso.org/standard/77608.html) *(paywalled)* — older survey reference useful as an on-ramp before the newer standards.
- [IEEE 7000-2021 — IEEE Standard Model Process for Addressing Ethical Concerns during System Design](https://standards.ieee.org/ieee/7000/6781/) — the ethics-methodology anchor chapter 06 positions inside the procedure layer of the taxonomy.
- [IEEE Standards Association — 7000-series family index](https://standards.ieee.org/initiatives/artificial-intelligence-systems/standards/) — the family page; individual numbered standards (7001 transparency, 7002 data privacy process, 7003 algorithmic bias considerations, 7005 employer data governance, 7007 ontological, 7010 well-being, 7014 emulated empathy) sit under this hub. <!-- needs-research: confirm current publication status of each numbered standard cited by chapter 06 before pinning in policy. -->

## Regulatory anchor sources referenced in worked examples

- [Regulation (EU) 2024/1689 — the EU AI Act](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — the definition of "AI system" (Article 3), the high-risk obligations (Articles 9–15), and the transparency and post-market articles that show up as jurisdictional anchors in the taxonomy.
- [European Commission — AI Act policy page](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai) — official summaries and the delegated / implementing act status tracker.
- [US OMB M-24-10 — Advancing Governance, Innovation, and Risk Management for Agency Use of AI](https://www.whitehouse.gov/wp-content/uploads/2024/03/M-24-10-Advancing-Governance-Innovation-and-Risk-Management-for-Agency-Use-of-Artificial-Intelligence.pdf) — federal-adjacent policy-shape reference cited when Northbrook composes with US-federal customer requirements.
- [NYC Local Law 144 — Automated Employment Decision Tools](https://www.nyc.gov/site/dca/about/automated-employment-decision-tools.page) — jurisdictional example referenced for the fairness / disparate-impact worked example in chapter 02.
- [Colorado SB 24-205 — Consumer Protections for Artificial Intelligence](https://leg.colorado.gov/bills/sb24-205) — state-level anchor for the Northbrook Colorado deployments; referenced when standards need a jurisdictional applicability filter.

## Policy-as-code engines and infrastructure (chapter 03)

- [Open Policy Agent (OPA)](https://www.openpolicyagent.org/) — general-purpose policy engine referenced throughout chapter 03 for runtime and gate enforcement.
- [Rego language documentation](https://www.openpolicyagent.org/docs/latest/policy-language/) — the language reference for the Rego fragments in chapter 03 and exercise-03.
- [OPA Gatekeeper](https://open-policy-agent.github.io/gatekeeper/website/docs/) — OPA wired into the Kubernetes admission-controller webhook; the reference implementation for admission-time policy in K8s environments.
- [AWS Cedar policy language](https://www.cedarpolicy.com/) — the typed authorisation language referenced as the runtime-tier alternative to Rego for authorisation-shaped policies.
- [Cedar language reference](https://docs.cedarpolicy.com/policies/syntax-policy.html) — the syntax reference for the Cedar fragment in chapter 03.
- [Cedar schema documentation](https://docs.cedarpolicy.com/schema/schema.html) — the typed-schema reference chapter 03 contrasts with Rego's dynamic input model.
- [Sigstore](https://www.sigstore.dev/) — the keyless-signing platform referenced for signed policy bundles and signed attestation records.
- [in-toto attestation framework](https://in-toto.io/) — the attestation-envelope shape (signed statement about a subject) used in chapter 03's attestation-only tier.
- [SLSA — Supply-chain Levels for Software Artifacts](https://slsa.dev/) — the maturity model for supply-chain attestations that the same signing infrastructure supports; referenced when reasoning about bundle-signing posture.

## Traceability and OSCAL representation (chapter 02)

- [OSCAL — Open Security Controls Assessment Language](https://pages.nist.gov/OSCAL/) — the model chapter 02 references as the "Option B" projection of the mapping matrix; worked in depth in mod-102 chapter 04.
- [OSCAL catalog model reference](https://pages.nist.gov/OSCAL-Reference/models/latest/catalog/) — the catalog shape the AI control library serialises into, extended in chapter 02 with `principle_ids` on the crosswalk.
- [OSCAL profile model reference](https://pages.nist.gov/OSCAL-Reference/models/latest/profile/) — the profile shape the traceability projection composes with.

## Waiver / exception references (chapter 04)

- [NIST SP 800-53 Rev. 5 — Security and Privacy Controls (CA-5 Plan of Action and Milestones)](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) — shape reference for how the risk register consumes waiver overruns; the POA&M-shape that mod-106 mirrors.
- [FedRAMP POA&M template and guidance](https://www.fedramp.gov/documents-templates/) — the operational shape reference for POA&M practice, referenced when designing the waiver-overrun finding shape.
- [SR 11-7 — Federal Reserve Supervisory Guidance on Model Risk Management](https://www.federalreserve.gov/supervisionreg/srletters/sr1107.htm) — the anchor governing Northbrook's existing MRM programme; referenced when reasoning about how AI-programme waivers compose with the bank's existing model-risk exception process.

## Policy-change communications and release-notice references (chapter 05)

- [Semantic Versioning 2.0.0](https://semver.org/) — the versioning-scheme anchor mirrored (in a "semver-ish" form) by both the mod-102 control-library release discipline and the policy release cadence in chapter 05.
- [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/) — the changelog-shape reference for the policy release notice front page in chapter 05.
