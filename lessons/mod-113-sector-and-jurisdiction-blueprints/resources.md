# Resources for mod-113-sector-and-jurisdiction-blueprints (Sector and Jurisdiction Reference Blueprints)

Curated primary sources for the module. Prefer the primary source over any secondary summary. Where a URL is not confirmed at authoring time it is left as a `<!-- needs-research: ... -->` marker rather than guessed.

## Sector-adaptation methodology (chapter 01)

- **NIST OSCAL — Open Security Controls Assessment Language** — the machine-readable catalog / profile / component-definition schemas the sector applicability-profile discipline (chapter 01) builds on; cross-reference mod-102 chapter 04 for the catalog and profile mechanic. <https://pages.nist.gov/OSCAL/>.
- **NIST SP 800-53 Rev. 5 — OSCAL representations** — the reference OSCAL catalog and baseline profiles the sector-profile deltas key into. <https://github.com/usnistgov/oscal-content>.
- **ISO/IEC 42001:2023, Clause 4.3 (Determining the scope of the AI management system)** — the AIMS-scope-determination discipline every sector I5 (scope addendum) relies on. ISO catalog: <https://www.iso.org/standard/81230.html>.
- **ISO/IEC 42001:2023, Clause 4.1 (Understanding the organisation and its context) and Clause 4.2 (interested parties)** — the context clauses the sector-profile framing composes with. Same catalog entry as above.

## US G-SIB banking (chapter 02)

- **Federal Reserve SR 11-7 — Guidance on Model Risk Management** (issued 4 April 2011; joint with OCC Bulletin 2011-12) — the model-risk-management anchor for the banking blueprint; the effective-challenge, validation-independence, and MRM-inventory obligations that the banking SAR composes with. <!-- needs-research: canonical federalreserve.gov URL for SR 11-7 -->.
- **OCC Bulletin 2011-12 — Supervisory Guidance on Model Risk Management** — the OCC parallel to SR 11-7. <!-- needs-research: canonical occ.treas.gov URL for Bulletin 2011-12 -->.
- **Federal Reserve SR 23-4 / OCC Bulletin 2023-17 / FDIC FIL-29-2023 — Interagency Guidance on Third-Party Relationships: Risk Management** (issued 6 June 2023 <!-- needs-research: verify exact SR number and date -->) — the interagency third-party-risk anchor the banking blueprint layers on top of mod-109. <!-- needs-research: canonical federalreserve.gov / occ.treas.gov / fdic.gov URLs -->.
- **OSFI Guideline E-23 — Enterprise-Wide Model Risk Management** (revised 2024 <!-- needs-research: verify current version and effective date -->) — the Canadian model-risk anchor for cross-border G-SIB scoping. <!-- needs-research: canonical osfi-bsif.gc.ca URL for E-23 -->.
- **EU AI Act — Regulation (EU) 2024/1689** — Annex III paragraph 5(b) (creditworthiness / credit scoring), Articles 8-15 (high-risk requirements), Article 26 (deployer obligations), Article 72 (post-market monitoring), Article 73 (serious-incident reporting). <https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R1689>.
- **Colorado AI Act (SB 24-205)** — signed 17 May 2024, effective 1 February 2026 <!-- needs-research: verify effective date has not been amended --> — consequential-decision obligations covering financial-services use cases. <!-- needs-research: canonical Colorado General Assembly URL -->.
- **Regulation B (12 CFR Part 1002) — Equal Credit Opportunity Act (ECOA) implementing regulation** — the adverse-action notice regime the banking evidence-contract extensions terminate on. <!-- needs-research: canonical consumerfinance.gov / eCFR URL for 12 CFR Part 1002 -->.
- **Home Mortgage Disclosure Act (HMDA) — Regulation C (12 CFR Part 1003)** — the HMDA reporting regime relevant to mortgage-model deployments. <!-- needs-research: canonical consumerfinance.gov / eCFR URL for 12 CFR Part 1003 -->.

## US health system (chapter 03)

- **FDA / Health Canada / MHRA — Good Machine Learning Practice for Medical Device Development: Guiding Principles** (10 guiding principles, published October 2021) — the GMLP anchor for the health blueprint. <https://www.fda.gov/medical-devices/software-medical-device-samd/good-machine-learning-practice-medical-device-development-guiding-principles>.
- **FDA — Marketing Submission Recommendations for a Predetermined Change Control Plan for AI/ML-Enabled Device Software Functions** (PCCP guidance) — the change-control-plan mechanic the health blueprint composes with the mod-110 PMS shape. <!-- needs-research: canonical fda.gov URL for the final PCCP guidance -->.
- **FDA — AI/ML-enabled medical device landing page** — the running list of authorised devices and the SaMD framing. <https://www.fda.gov/medical-devices/software-medical-device-samd/artificial-intelligence-and-machine-learning-aiml-enabled-medical-devices>.
- **HIPAA Privacy Rule, Security Rule, and Breach Notification Rule (45 CFR Parts 160, 162, 164)** — the privacy-and-security substrate the health-sector evidence contracts extend. <!-- needs-research: canonical hhs.gov / eCFR URL for the HIPAA rules landing -->.
- **Section 1557 of the Affordable Care Act — nondiscrimination provisions** — the nondiscrimination obligation applicable to health-programme decision-support systems. <!-- needs-research: canonical hhs.gov OCR URL for the Section 1557 final rule -->.
- **EU AI Act Annex III entries applicable to medical AI** — Regulation (EU) 2024/1689 Annex III (medical-device intersections with Regulation (EU) 2017/745 on medical devices). <https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R1689>.
- **State medical-AI regulations (CA, CO, TX and others)** — the state-level overlay tracking against the federal SaMD posture. <!-- needs-research: current status of state medical-AI statutes (CA AB 3030 / SB 1120, CO SB 24-205 health carve-outs, TX HB / SB medical-AI bills) at authoring date -->.

## Insurance carrier (chapter 04)

- **NAIC Model Bulletin — Use of Artificial Intelligence Systems by Insurers** (adopted December 2023 <!-- needs-research: verify exact adoption date -->) — the NAIC anchor the insurance blueprint composes with. <!-- needs-research: canonical naic.org URL for the AI Model Bulletin -->.
- **Colorado Insurance Regulation 3 CCR 702-10 (Regulation 10-1-1) — Governance and Risk Management Framework Requirements for Life Insurers' Use of External Consumer Data and Information Sources, Algorithms, and Predictive Models** <!-- needs-research: verify current title, citation, and companion regulations (e.g. testing requirements) --> — the Colorado insurance-sector precedent. <!-- needs-research: canonical Colorado Division of Insurance URL -->.
- **EU AI Act Annex III entry 5(a)** — Regulation (EU) 2024/1689 Annex III paragraph 5(a) covering risk assessment and pricing in life and health insurance. <https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R1689>.
- **NAIC Market Regulation Handbook** — the state DOI market-conduct examination reference the insurance blueprint's evidence-contract extensions target. <!-- needs-research: canonical naic.org URL for the current Market Regulation Handbook edition -->.

## Pharmaceutical (chapter 05)

- **GxP predicate rules** — the compliance substrate every pharma AI use case sits inside:
  - **21 CFR Part 58 — Good Laboratory Practice for Nonclinical Laboratory Studies (GLP)**. <!-- needs-research: canonical eCFR URL for 21 CFR Part 58 -->.
  - **Good Clinical Practice (GCP)** — implemented via ICH E6 and FDA regulations at 21 CFR Parts 50, 54, 56, 312, 812.
  - **21 CFR Parts 210 and 211 — Current Good Manufacturing Practice (cGMP) for Finished Pharmaceuticals**. <!-- needs-research: canonical eCFR URLs for 21 CFR Parts 210 / 211 -->.
  - **Good Distribution Practice (GDP)** — EU GDP Guidelines 2013/C 343/01 and equivalents.
  - **Good Pharmacovigilance Practices (GVP)** — EMA GVP modules.
- **21 CFR Part 11 — Electronic Records; Electronic Signatures** — the electronic-records anchor for pharma AI evidence contracts. <!-- needs-research: canonical eCFR URL for 21 CFR Part 11 -->.
- **FDA — Using Artificial Intelligence and Machine Learning in the Development of Drug and Biological Products** (2023 discussion paper and request for information). <!-- needs-research: canonical fda.gov URL for the 2023 CDER / CBER AI/ML discussion paper -->.
- **FDA — Considerations for the Use of Artificial Intelligence to Support Regulatory Decision-Making for Drug and Biological Products** (AI/ML credibility-assessment framework draft guidance, January 2025 <!-- needs-research: verify exact issuance date and finalised title -->). <!-- needs-research: canonical fda.gov URL -->.
- **EMA — Reflection Paper on the Use of Artificial Intelligence in the Lifecycle of Medicines** (September 2024 <!-- needs-research: verify exact date and finalised title -->). <!-- needs-research: canonical ema.europa.eu URL -->.
- **ICH E6(R3) — Good Clinical Practice** — the current GCP revision. <!-- needs-research: canonical ich.org URL for E6(R3) -->.
- **ICH Q9(R1) — Quality Risk Management** — the risk-management reference pharma AI validation composes with. <!-- needs-research: canonical ich.org URL for Q9(R1) -->.
- **GAMP 5 (ISPE) — A Risk-Based Approach to Compliant GxP Computerized Systems** (2nd edition 2022 <!-- needs-research: verify current edition and issuance date -->). <https://ispe.org>.
- **21 CFR Part 3 and 21 CFR Part 4 — combination products** — the regime for drug-device combinations relevant when a pharma product ships with an AI-enabled companion device. <!-- needs-research: canonical eCFR URLs for 21 CFR Parts 3 and 4 -->.

## US federal agency contractor (chapter 06)

- **OMB M-25-21 — Accelerating Federal Use of AI through Innovation, Governance, and Public Trust** (issued 3 April 2025 <!-- needs-research: verify -->; supersedes OMB M-24-10). <!-- needs-research: canonical whitehouse.gov / OMB URL for M-25-21 -->.
- **OMB M-25-22 — Driving Efficient Acquisition of Artificial Intelligence in Government** (issued 3 April 2025 <!-- needs-research: verify -->; supersedes OMB M-24-18). <!-- needs-research: canonical whitehouse.gov / OMB URL for M-25-22 -->.
- **FedRAMP programme** — the Low / Moderate / High baselines the federal-contractor blueprint targets; the ATO artefact set (System Security Plan, Security Assessment Report, Plan of Action and Milestones). <https://www.fedramp.gov>.
- **NIST SP 800-53 Rev. 5 — Security and Privacy Controls for Information Systems and Organizations**. <https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final>.
- **NIST SP 800-53A Rev. 5 — Assessing Security and Privacy Controls in Information Systems and Organizations**. <!-- needs-research: canonical csrc.nist.gov URL for SP 800-53A Rev. 5 -->.
- **NIST AI Risk Management Framework 1.0 and NIST AI 600-1 — Generative AI Profile**. <https://www.nist.gov/itl/ai-risk-management-framework>.
- **US AI Safety Institute (NIST AISI)** — publications, evaluation methodologies, and voluntary agreements with frontier developers. <https://www.nist.gov/aisi>.
- **Section 508 of the Rehabilitation Act (29 U.S.C. Section 794d)** — accessibility obligations for public-facing federal services. <https://www.section508.gov>.
- **Federal Information Security Modernization Act (FISMA) of 2014** — the federal information-security statute the ATO regime implements. <!-- needs-research: canonical cisa.gov / nist.gov URL for the FISMA overview -->.
- **E-Government Act of 2002, Section 208 — Privacy Impact Assessments (PIAs)**. <!-- needs-research: canonical whitehouse.gov / archives.gov URL for the E-Government Act Section 208 text -->.

## Critical infrastructure (chapter 07)

- **CISA / UK NCSC — Guidelines for Secure AI System Development** (published 26 November 2023, joint with international partners <!-- needs-research: verify partner count and full agency list -->). <https://www.ncsc.gov.uk/collection/guidelines-secure-ai-system-development>.
- **ENISA — Multilayer Framework for Good Cybersecurity Practices for AI** (published <!-- needs-research: verify date -->). <https://www.enisa.europa.eu>.
- **NIST Cybersecurity Framework 2.0** (published 26 February 2024) — the CSF the critical-infrastructure blueprint composes with. <https://www.nist.gov/cyberframework>.
- **NIST SP 800-82 Rev. 3 — Guide to Operational Technology (OT) Security** <!-- needs-research: verify Rev 3 issuance date -->. <!-- needs-research: canonical csrc.nist.gov URL for SP 800-82 Rev. 3 -->.
- **NERC CIP standards** — CIP-002 through CIP-014 plus emerging CIP-015 <!-- needs-research: verify the current active set and status of CIP-015 (INSM) at authoring date -->. <https://www.nerc.com>.
- **TSA pipeline and rail Security Directives** <!-- needs-research: current SD numbers and revisions in force at authoring date (SD Pipeline-2021-01/-02 series and rail SD-1580/-1582 series) -->. <https://www.tsa.gov>.
- **EU NIS2 Directive — Directive (EU) 2022/2555** — Article 21 (cybersecurity risk-management measures) and Article 23 (incident notification). <https://eur-lex.europa.eu/eli/dir/2022/2555/oj>.
- **Information-sharing partners** — sector ISACs the critical-infrastructure blueprint's incident-reporting pipelines integrate with:
  - **E-ISAC (Electricity ISAC)** — <https://www.eisac.com>.
  - **WaterISAC** — <https://www.waterisac.org>.
  - **Downstream Natural Gas ISAC (DNG-ISAC)** — <https://www.dngisac.com>.

## Public-sector adjacencies (chapter 08)

- **Government of Canada — Directive on Automated Decision-Making** (Treasury Board of Canada Secretariat; in force 1 April 2019; revised <!-- needs-research: verify latest revision date and version -->). <https://www.tbs-sct.canada.ca/pol/doc-eng.aspx?id=32592>.
- **Government of Canada — Algorithmic Impact Assessment (AIA) tool** — the questionnaire-driven impact-assessment implementation of the Directive. <https://www.canada.ca/en/government/system/digital-government/digital-government-innovations/responsible-use-ai/algorithmic-impact-assessment.html>.
- **UK — Algorithmic Transparency Recording Standard (ATRS)** — CDDO / CDEI publication <!-- needs-research: verify current version, publishing owner (DSIT / CDDO), and status at authoring date -->. <!-- needs-research: canonical gov.uk URL for the current ATRS version -->.
- **UK AI Standards Hub** — <https://aistandardshub.org>.
- **UK AI Safety Institute** — <https://www.aisi.gov.uk>.
- **UK ICO — Guidance on AI and Data Protection** — the ICO's AI-and-data-protection guidance package. <https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/artificial-intelligence/>.
- **Singapore — Model AI Governance Framework** (2nd edition, January 2020) and **Model AI Governance Framework for Generative AI** (2024 <!-- needs-research: verify current status and finalised title -->). <!-- needs-research: canonical pdpc.gov.sg / aiverifyfoundation.sg URLs for both editions -->.
- **Singapore — AI Verify testing framework and AI Verify Foundation** — <https://aiverifyfoundation.sg>.
- **Singapore — PDPC (Personal Data Protection Commission) and IMDA (Infocomm Media Development Authority)** — the regulator-and-authority pair the Singapore blueprint terminates on. <https://www.pdpc.gov.sg> and <https://www.imda.gov.sg>.
- **ASEAN Guide on AI Governance and Ethics** (2024 <!-- needs-research: verify exact publication date and endorsing body -->). <!-- needs-research: canonical asean.org URL for the ASEAN AI governance guide -->.

## Sibling modules in this track

- [mod-101 — Role Scope and Standards Landscape](../mod-101-role-scope-and-standards-landscape/) — the standards-landscape reader posture that positioning the sector-specific regulators in each blueprint relies on.
- [mod-102 — AI Control Library Architecture](../mod-102-ai-control-library-architecture/) — the control catalog and OSCAL profile mechanic (chapter 04) every sector chapter's profile-delta section builds on.
- [mod-103 — Policy Taxonomy and Policy as Code](../mod-103-policy-taxonomy-and-policy-as-code/) — the policy-as-code layer sector guards parameterise against.
- [mod-104 — Multi-Jurisdiction Reconciliation](../mod-104-multi-jurisdiction-reconciliation/) — the obligation register every SAR's evidence-contract extensions key into.
- [mod-105 — AIMS and AI Management System Architecture](../mod-105-aims-and-ai-management-system-architecture/) — the AIMS every sector adds a scope addendum to.
- [mod-106 — Risk Taxonomy and Enterprise Appetite](../mod-106-risk-taxonomy-and-enterprise-appetite/) — the taxonomy each sector augments with specialisations.
- [mod-107 — Assurance Architecture and Audit Readiness](../mod-107-assurance-architecture-and-audit-readiness/) — the three-lines and pre-deployment gate the banking blueprint composes with SR 11-7 effective challenge and the health blueprint composes with clinical-safety review.
- [mod-108 — Evidence Architecture and Documentation Schemas](../mod-108-evidence-architecture-and-documentation-schemas/) — the schema registry each sector's evidence-contract extensions register into.
- [mod-109 — Third-Party and Supply-Chain Governance Architecture](../mod-109-third-party-and-supply-chain-governance-architecture/) — the provider programme the banking (SR 23-4) and pharma (GxP vendor) blueprints layer sector expectations onto.
- [mod-110 — Monitoring and Post-Market Surveillance Architecture](../mod-110-monitoring-and-post-market-surveillance-architecture/) — the PMS shape sector-specific incident-reporting pipelines feed into.
- [mod-111 — GRC-for-AI Platform and Toolchain Architecture](../mod-111-grc-for-ai-platform-and-toolchain-architecture/) — the platform each SAR is written to as a first-class artefact.
- [mod-112 — Program Design and Org Shape](../mod-112-program-design-and-org-shape/) — the AI governance council that ratifies each SAR as a reserved matter at version increments.
