# Resources for mod-111-grc-for-ai-platform-and-toolchain-architecture (GRC-for-AI Platform and Toolchain Reference Architecture)

Curated primary sources for the module. Prefer the primary source over any secondary summary. Where a URL is not confirmed at authoring time it is left as a `<!-- needs-research: ... -->` marker rather than guessed.

## Standards and frameworks

- **ISO/IEC 42001:2023 (AI management systems)** — the AIMS anchor. ISO catalog: <https://www.iso.org/standard/81230.html>.
- **ISO/IEC 42005 (AI system impact assessment)** — the impact-assessment reference the workflow layer's impact-assessment flow shapes against. <!-- needs-research: verify current ISO catalog URL and standard status (DIS / FDIS / IS) -->.
- **ISO/IEC 42006 (Requirements for bodies providing audit and certification of AI management systems)** — the certification-audit reference the audit-facing packaging flow's counterparty operates under. <!-- needs-research: verify current ISO catalog URL and publication status -->.
- **ISO/IEC 23894:2023 (AI risk management)** — the risk-source reference. <https://www.iso.org/standard/77304.html>.
- **ISO/IEC 27001:2022 (Information security management systems)** — the enterprise ISMS the AIMS composes with. <https://www.iso.org/standard/27001>.
- **NIST AI Risk Management Framework (AI RMF 1.0)** — <https://www.nist.gov/itl/ai-risk-management-framework>.
- **NIST AI 600-1 — Generative AI Profile** — same landing page: <https://www.nist.gov/itl/ai-risk-management-framework>.
- **NIST SP 800-53 Rev. 5** — the security controls catalogue the enterprise GRC's IT-general controls reference. <https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final>.
- **NIST SP 800-37 Rev. 2 — Risk Management Framework** — the process-shape reference. <https://csrc.nist.gov/publications/detail/sp/800-37/rev-2/final>.

## Primary regulation

- **EU AI Act — Regulation (EU) 2024/1689** — Articles 26 (deployer obligations), 72 (post-market monitoring), 73 (serious-incident reporting). <https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R1689>.
- **Federal Reserve SR 11-7 — Supervisory Guidance on Model Risk Management** — the validation-independence anchor. <!-- needs-research: canonical federalreserve.gov URL -->.
- **OCC Bulletin 2011-12 — Supervisory Guidance on Model Risk Management** — the OCC parallel to SR 11-7. <!-- needs-research: canonical occ.treas.gov URL -->.
- **Colorado AI Act (SB24-205)** — Colorado consumer protections for AI, risk-management-programme obligations. <!-- needs-research: canonical Colorado General Assembly URL -->.
- **FDA — AI/ML-enabled medical device guidance** — including the predetermined change control plan (PCCP) line of work. <https://www.fda.gov/medical-devices/software-medical-device-samd/artificial-intelligence-and-machine-learning-aiml-enabled-medical-devices>.
- **HIPAA (45 CFR Parts 160, 162, 164)** — the healthcare-scenario data-protection anchor. <!-- needs-research: canonical HHS URL for the HIPAA rules index -->.

## Enterprise GRC / IRM platforms (orientation, not endorsement)

- **RSA Archer** — enterprise GRC platform. <!-- needs-research: canonical vendor URL and current product-line naming -->.
- **ServiceNow GRC / IRM** — enterprise GRC on the ServiceNow platform. <https://www.servicenow.com>.
- **MetricStream** — enterprise GRC / integrated risk management. <https://www.metricstream.com>.
- **IBM OpenPages** — enterprise GRC on IBM. <!-- needs-research: canonical URL -->.
- **OneTrust** — privacy-anchored GRC with AI-governance extension. <https://www.onetrust.com>.
- **LogicGate** — GRC and integrated risk management. <!-- needs-research: canonical URL -->.

## Purpose-built AI-GRC platforms (orientation, not endorsement)

- **Credo AI** — <https://www.credo.ai>.
- **Holistic AI** — <https://www.holisticai.com>.
- **ModelOp** — <https://www.modelop.com>.
- **Monitaur** — <https://www.monitaur.ai>.
- **Fairly AI** — <!-- needs-research: canonical URL -->.
- **Enzai** — <!-- needs-research: canonical URL -->.
- **Trustible** — <!-- needs-research: canonical URL -->.
- **ServiceNow AI Control Tower** — <!-- needs-research: canonical product URL under servicenow.com -->.
- **IBM watsonx.governance** — <!-- needs-research: canonical product URL under ibm.com -->.

## AI runtime security (orientation, not endorsement)

- **Robust Intelligence** — <https://www.robustintelligence.com>.
- **Lakera / Lakera Guard** — <https://www.lakera.ai>.
- **CalypsoAI** — <https://calypsoai.com>.
- **HiddenLayer** — <https://hiddenlayer.com>.
- **Protect AI** — <https://protectai.com>.

## AI observability (orientation, not endorsement)

- **Fiddler AI** — <https://www.fiddler.ai>.
- **Arthur AI** — <https://arthur.ai>.
- **WhyLabs / whylogs** — <https://whylabs.ai> (whylogs OSS: <!-- needs-research: verify current github.com/whylabs/whylogs URL -->).
- **Evidently AI** — <https://www.evidentlyai.com>.

## Assurance testing frameworks

- **AI Verify Foundation and toolkit** — Singapore's IMDA-associated open-source assurance testing framework. <!-- needs-research: verify canonical URL for AI Verify Foundation and current release notes -->.
- **Singapore Model AI Governance Framework** — the framework the AI Verify toolkit sits under. <!-- needs-research: canonical URL under imda.gov.sg or the AI Verify Foundation -->.

## Identity, ticketing, communications (platform primitives referenced by chapter 01)

- **Okta** — <https://www.okta.com>.
- **Microsoft Entra ID** — <https://www.microsoft.com/security/business/identity-access/microsoft-entra-id>.
- **Ping Identity** — <https://www.pingidentity.com>.
- **Jira** — <https://www.atlassian.com/software/jira>.
- **ServiceNow ITSM** — <https://www.servicenow.com>.
- **Azure DevOps Boards** — <!-- needs-research: canonical URL under dev.azure.com or azure.microsoft.com -->.
- **Slack** — <https://slack.com>.
- **Microsoft Teams** — <https://www.microsoft.com/microsoft-teams>.

## Assurance and audit references

- **IIA Three Lines Model (2020)** — <https://www.theiia.org>.
- **ForHumanity — Independent Audit of AI Systems (IAAIS)** — <https://forhumanity.center>.
- **Big Four AI / ISO 42001 practice pages** — <!-- needs-research: cite specific practice pages when confidently URL-verified -->.
- **AICPA SOC 2** — <!-- needs-research: canonical AICPA URL for SOC 2 -->.

## Sibling modules in this track

- [mod-102 — AI Control Library Architecture](../mod-102-ai-control-library-architecture/) — the `AIC-*` control identifiers the platform hosts and the evidence contract mod-108 defines against.
- [mod-104 — Multi-Jurisdiction Reconciliation](../mod-104-multi-jurisdiction-reconciliation/) — the reconciled control set the platform's jurisdiction-coverage dimension scores against.
- [mod-105 — AIMS Architecture](../mod-105-aims-and-ai-management-system-architecture/) — the ISO/IEC 42001 AIMS documented information the platform hosts; Clause 7.2 competence backing the RBAC model.
- [mod-106 — Risk Taxonomy and Enterprise Appetite](../mod-106-risk-taxonomy-and-enterprise-appetite/) — the risk-register schema the platform holds; the appetite tolerance table the workflow-layer incident-routing flow reads.
- [mod-107 — Assurance Architecture and Audit Readiness](../mod-107-assurance-architecture-and-audit-readiness/) — the three-lines shape the RBAC model implements; the pre-deployment gate the control-testing flow drives.
- [mod-108 — Evidence Architecture and Documentation Schemas](../mod-108-evidence-architecture-and-documentation-schemas/) — the evidence-artefact index the platform hosts; the freshness contract observability + runtime-security signals refresh.
- [mod-109 — Third-Party and Supply-Chain Governance Architecture](../mod-109-third-party-and-supply-chain-governance-architecture/) — the third-party AI inventory the platform hosts.
- [mod-110 — Monitoring and Post-Market Surveillance Architecture](../mod-110-monitoring-and-post-market-surveillance-architecture/) — the two-store convergence (chapter 03), the monitor-to-register contract (chapter 02), the Article 73 SOP (chapter 04), the SOC interface (chapter 05).
- [mod-112 — Program Design and Organisational Shape](../mod-112-program-design-and-organisational-shape/) — the level-60 head-of-AI-governance program shape that bounds the level-50 architect's authority.
- [mod-113 — Sector and Jurisdiction Blueprints](../mod-113-sector-and-jurisdiction-blueprints/) — the sector overlays that add scenario-specific controls, workflows, and evidence types.
