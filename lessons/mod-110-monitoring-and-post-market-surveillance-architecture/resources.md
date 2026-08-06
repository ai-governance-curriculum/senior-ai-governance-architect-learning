# Resources for mod-110-monitoring-and-post-market-surveillance-architecture (Enterprise Post-Market Surveillance and Monitoring Architecture)

Curated primary sources for the module. Prefer the primary source over any secondary summary. Where a URL is not confirmed at authoring time it is left as a `<!-- needs-research: ... -->` marker rather than guessed.

## Primary regulation

- **EU AI Act** — Regulation (EU) 2024/1689. Article 72 (post-market monitoring by providers) and Article 73 (reporting of serious incidents) are the anchors for this module. Article 12 (record-keeping / logs) is the retention lever chapter 03 leans on. Full text via EUR-Lex: <https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R1689>.
- **GDPR** — Regulation (EU) 2016/679. Article 33 (personal-data-breach notification, 72-hour window) is the parallel-obligation baseline chapter 04 fans out from. EUR-Lex: <https://eur-lex.europa.eu/eli/reg/2016/679/oj>.
- **US SEC cybersecurity disclosure** — 2023 final rule on cybersecurity risk management, strategy, governance, and incident disclosure (Item 1.05 8-K and Regulation S-K Item 106). <!-- needs-research: verify canonical SEC.gov URL for the final rule and its current amendments as applied to AI-specific incidents -->.
- **Colorado AI Act (SB24-205)** — Colorado consumer protections for AI, risk-management-programme obligations. Statute page: <!-- needs-research: canonical Colorado General Assembly / Attorney General URL for SB24-205 and its rulemaking -->.
- **FDA — AI/ML-enabled medical device guidance** — including the predetermined change control plan (PCCP) guidance line of work, which sits parallel to Article 72 for medical-device AI. Landing page: <https://www.fda.gov/medical-devices/software-medical-device-samd/artificial-intelligence-and-machine-learning-aiml-enabled-medical-devices>.
- **Federal Reserve SR 11-7** — Supervisory guidance on model risk management (2011); the classical ongoing-model-validation reference the AI-scope programme extends. <!-- needs-research: canonical federalreserve.gov URL for SR 11-7 -->.

## Standards and frameworks

- **NIST AI Risk Management Framework (AI RMF 1.0)** — <https://www.nist.gov/itl/ai-risk-management-framework>.
- **NIST AI 600-1 — Generative AI Profile** — <https://www.nist.gov/itl/ai-risk-management-framework>. Cited across mod-106 and mod-110 for harm-category coverage.
- **NIST SP 800-37 Rev. 2 — Risk Management Framework** — <https://csrc.nist.gov/publications/detail/sp/800-37/rev-2/final>. The process-shape reference the assurance-plus-PMS composition binds to.
- **NIST SP 800-61 Rev. 2 — Computer Security Incident Handling Guide** — <https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final>. The SOC-side reference chapter 05 aligns against.
- **NIST SP 800-53 Rev. 5** — Security and privacy controls; the SOC-side control-framework touchpoint. <https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final>.
- **ISO/IEC 42001:2023 — AI management systems** — the AIMS reference (mod-105 anchor). ISO catalog: <https://www.iso.org/standard/81230.html>.
- **ISO/IEC 23894:2023 — AI risk management** — the risk-source reference (mod-106 anchor). ISO catalog: <https://www.iso.org/standard/77304.html>.
- **ISO/IEC 42005 — AI system impact assessment** — the impact-assessment reference (mod-105 chapter 04). <!-- needs-research: verify current ISO catalog URL and standard status (DIS / FDIS / IS) -->.
- **ISO/IEC 27035 (series) — Information security incident management** — the SOC-side incident-management reference chapter 05 aligns against. ISO catalog: <https://www.iso.org/standard/78973.html>.
- **ISO/IEC 42006 — Requirements for bodies providing audit and certification of AI management systems** — the certification-audit competence reference. <!-- needs-research: verify current ISO catalog URL and 2025 publication status -->.

## Regulatory reporting and post-market surveillance context

- **European AI Office (EU Commission)** — the EU-level coordinator for AI Act implementation and market-surveillance guidance. Landing page: <!-- needs-research: canonical digital-strategy.ec.europa.eu / AI Office URL -->.
- **OECD AI Policy Observatory** — <https://oecd.ai>. Policy-instrument tracker, national-strategy tracker, and the AI Incidents Monitor.
- **Colorado Attorney General AI rulemaking** — <!-- needs-research: canonical Colorado AG / DORA rulemaking page for SB24-205 implementation -->.
- **State Attorneys General AI enforcement statements** — <!-- needs-research: NAAG or state-level pages relevant to the chosen scenario -->.

## Model-observability platforms (orientation only, not endorsement)

Chapter 03 references these platforms by name at category level. Cite the vendor's own documentation when making any specific product-feature claim; do not rely on secondary summaries.

- **Fiddler AI** — <https://www.fiddler.ai>.
- **Arthur AI** — <https://arthur.ai>.
- **WhyLabs** — <https://whylabs.ai>.
- **Evidently AI** — <https://www.evidentlyai.com>.

The module does not endorse or recommend any specific platform; the citations are orientation for the wiring drill in exercise-03.

## Security-side references

- **MITRE ATLAS** — Adversarial Threat Landscape for Artificial Intelligence Systems; the AI-specific TTP matrix chapter 05 aligns SOC and governance classifications against. <https://atlas.mitre.org>.
- **MITRE ATT&CK** — the parent adversary-TTP framework MITRE ATLAS composes with. <https://attack.mitre.org>.
- **OWASP Top 10 for LLM Applications** — <https://owasp.org/www-project-top-10-for-large-language-model-applications/>. The application-security lens the SOC-interface signal registry commonly cites.
- **OWASP Machine Learning Security Top 10** — <https://owasp.org/www-project-machine-learning-security-top-10/>. The classical-ML companion.

## External incident and risk corpora

- **AI Incident Database (AIID)** — curated public collection of AI incidents; hosted by the Responsible AI Collaborative. <https://incidentdatabase.ai>.
- **OECD.AI Incidents Monitor** — the OECD's ongoing monitoring of AI incidents drawing on media and disclosures. <https://oecd.ai/en/incidents>.
- **MIT AI Risk Repository** — meta-analysis of AI risk taxonomies and enumerated risks. <https://airisk.mit.edu>. <!-- needs-research: latest published totals for taxonomies analysed and risks catalogued (43 taxonomies / 777 risks was the reported v1 figure around 2024). -->
- **Partnership on AI — incident work** — <https://partnershiponai.org>.
- **CSET (Georgetown) — AI incident taxonomies** — the CSET classification schemes referenced within AIID entries. <https://cset.georgetown.edu>.

## Adjacent industry / assurance references

- **IIA Three Lines Model (2020)** — the assurance-line reference (mod-107 anchor). <https://www.theiia.org>.
- **ForHumanity — Independent Audit of AI Systems (IAAIS)** — the independent-auditor programme referenced across mod-107 and this module. <https://forhumanity.center>.
- **BIS / FSB reports on AI in finance** — the sector-regulator horizon-scanning references most relevant to the banking scenario. <!-- needs-research: canonical BIS and FSB URLs for the most recent AI-in-finance publications -->.

## Sibling modules in this track

- [mod-102 — AI Control Library Architecture](../mod-102-ai-control-library-architecture/) — control taxonomy, applicability filter, and evidence contract chapter 02 references.
- [mod-105 — AIMS Architecture](../mod-105-aims-and-ai-management-system-architecture/) — Clause 9 performance-evaluation and Clause 10 non-conformity chapters (specifically chapter 09 CAPA) that the PMS feedback loop closes into.
- [mod-106 — Risk Taxonomy and Enterprise Appetite](../mod-106-risk-taxonomy-and-enterprise-appetite/) — the closed-world taxonomy every monitor-to-register edge references; the appetite tolerance table the deviation-from-appetite alarms bind to.
- [mod-107 — Assurance Architecture and Audit Readiness](../mod-107-assurance-architecture-and-audit-readiness/) — the three-lines assurance shape the PMS sits inside; chapter 03 ongoing-assurance triggers consume the assurance-trigger threshold from this module's chapter 02.
- [mod-108 — Evidence Architecture and Documentation Schemas](../mod-108-evidence-architecture-and-documentation-schemas/) — the evidence-artefact freshness updates that consume this module's register writes.
- [mod-109 — Third-Party and Supply-Chain Governance Architecture](../mod-109-third-party-and-supply-chain-governance-architecture/) — the third-party attack-surface and vendor-side PMS considerations chapter 05 references.
- [mod-111 — GRC-for-AI Platform and Toolchain Architecture](../mod-111-grc-for-ai-platform-and-toolchain-architecture/) — the platform layer that hosts the PMS store, the monitoring-register contract, and the calibration workflow's inputs and outputs.
- [mod-113 — Sector and Jurisdiction Blueprints](../mod-113-sector-and-jurisdiction-blueprints/) — sector-specific reviewers and the loss-event databases that complement the AI-specific external corpora referenced in chapter 06.
