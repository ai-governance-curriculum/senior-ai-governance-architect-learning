# Resources — mod-105 ISO/IEC 42001 AIMS Architecture and Integration

Primary sources only. This module reads the ISO/IEC 42001 family, the sibling ISO/IEC 27001 ISMS family, ISO 31000, and the conformity-assessment standards behind third-party certification as *design inputs* — the URLs below are the publisher-hosted canonical locations for the standards, guidance, and adjacent instruments the chapters and exercises reference. Where a source is paywalled (ISO/IEC and ISO deliverables), the link resolves to the catalogue entry with the title, publication year, and abstract sufficient for architectural framing; the full normative text is behind ISO's licence and must be procured for authoritative use. Where a source is fast-moving (implementing acts, memoranda revisions, standards under development in JTC 1/SC 42), verify against the primary source at reading time and update the `<!-- needs-research: ... -->` markers in the chapter and exercise texts accordingly.

## The ISO/IEC 42001 family — the core (chapters 01–11, all exercises)

### ISO/IEC 42001:2023 — the AIMS requirements standard

- [ISO/IEC 42001:2023 — Information technology — Artificial intelligence — Management system (catalogue entry)](https://www.iso.org/standard/81230.html) — the AIMS requirements standard. This is the certifiable standard. Published December 2023. Clauses 4–10 follow the Annex SL harmonised structure; Annex A is the normative reference control set the SoA walks; Annex B is implementation guidance; Annex C carries AI-specific risk sources; Annex D touches integration with other management systems. Paywalled.
- [ISO/IEC 42001:2023 preview (limited free preview)](https://www.iso.org/obp/ui/#iso:std:iso-iec:42001:ed-1:v1:en) — the standard's front matter and clause structure without the normative text. Useful for confirming clause numbering and annex titles without procuring the full standard.

### ISO/IEC 42005:2025 — AI system impact assessment

- [ISO/IEC 42005:2025 — Information technology — Artificial intelligence — AI system impact assessment (catalogue entry)](https://www.iso.org/standard/44545.html) <!-- needs-research: verify final published number, publication date, and title against the ISO catalogue at reading time; the 42005 project has moved through DIS and FDIS stages. --> — the AI-specific impact-assessment process standard. Provides the methodology the AIMS's Clause 6.1.4 uses. Chapter 04 and exercise 05 anchor here.

### ISO/IEC 42006 — audit / certification body requirements

- [ISO/IEC 42006 — Information technology — Artificial intelligence — Requirements for bodies providing audit and certification of AI management systems (catalogue entry)](https://www.iso.org/standard/44546.html) <!-- needs-research: verify final published number, publication date, and structural requirements against the ISO catalogue at reading time; 42006 has moved through the standardisation stages. Confirm the sampling-methodology and auditor-day tables at reading time. --> — the AI-specialisation of ISO/IEC 17021-1 for AIMS certification bodies. Chapter 11 and exercise 06 anchor here.

### ISO/IEC 23894:2023 — AI risk-management guidance

- [ISO/IEC 23894:2023 — Information technology — Artificial intelligence — Guidance on risk management (catalogue entry)](https://www.iso.org/standard/77304.html) — the AI-specific risk-management guidance layered on top of ISO 31000. Published February 2023. Carries the AI-specific risk-source taxonomy and factors chapter 04 walks. Paywalled.

### ISO/IEC 23053:2022 — reference architecture for ML AI systems

- [ISO/IEC 23053:2022 — Framework for artificial intelligence (AI) systems using machine learning (ML) (catalogue entry)](https://www.iso.org/standard/74438.html) — the reference-architecture standard the AIMS scope statement is written against per chapter 02. Published June 2022. Paywalled.

### ISO/IEC 22989:2022 — AI concepts and terminology

- [ISO/IEC 22989:2022 — Information technology — Artificial intelligence — Artificial intelligence concepts and terminology (catalogue entry)](https://www.iso.org/standard/74296.html) — the definitional anchor for "AI system" the scope statement uses. Published July 2022. Paywalled.

### ISO/IEC 38507:2022 — governance implications of AI

- [ISO/IEC 38507:2022 — Information technology — Governance of IT — Governance implications of the use of artificial intelligence by organizations (catalogue entry)](https://www.iso.org/standard/56641.html) — the governance-body-tier standard sitting above the AIMS. Chapter 01 positions the AIMS as reporting up to the governance body under 38507. Published April 2022. Paywalled.

### Related JTC 1/SC 42 work in progress

- [ISO/IEC JTC 1/SC 42 (Artificial Intelligence) programme of work](https://www.iso.org/committee/6794475.html) — the committee catalogue page; useful for tracking work items in adjacent AI-standards areas (data quality, AI trustworthiness, governance of AI in specific sectors). Track at reading time for new publications relevant to the AIMS scope.

## ISO 31000 — the parent risk-management framework (chapters 03, 04, 06; exercise 03)

- [ISO 31000:2018 — Risk management — Guidelines (catalogue entry)](https://www.iso.org/standard/65694.html) — the parent risk-management guidance the AIMS's Clause 6.1 risk process specialises. Not certifiable; guidance. Published February 2018. Paywalled.
- [ISO/IEC 31010:2019 — Risk management — Risk assessment techniques (catalogue entry)](https://www.iso.org/standard/72140.html) — the companion techniques catalogue. Useful for the AI-risk-identification workshop pattern chapter 04 sketches. Paywalled.
- [ISO Guide 73:2009 — Risk management — Vocabulary](https://www.iso.org/standard/44651.html) — the risk-management vocabulary the ISO 31000 family uses. Paywalled.

## The ISO/IEC 27001 family — the sibling ISMS (chapter 10; exercise 04)

- [ISO/IEC 27001:2022 — Information security, cybersecurity and privacy protection — Information security management systems — Requirements (catalogue entry)](https://www.iso.org/standard/27001) — the ISMS requirements standard the AIMS integrates with per chapter 10. Published October 2022. Paywalled.
- [ISO/IEC 27002:2022 — Information security controls (catalogue entry)](https://www.iso.org/standard/75652.html) — the implementation-guidance companion to ISO/IEC 27001 Annex A. Paywalled.
- [ISO/IEC 27006:2015 (and 27006-1:2024) — Requirements for bodies providing audit and certification of information security management systems (catalogue entry)](https://www.iso.org/standard/62313.html) <!-- needs-research: confirm which edition (27006:2015 or 27006-1:2024) is current for accredited-body audits at reading time; the 27006-1 restructuring split the standard by part. --> — the ISMS certification-body specialisation of ISO/IEC 17021-1; the pattern ISO/IEC 42006 follows for the AIMS. Paywalled.
- [ISO/IEC 27701:2019 — Extension to ISO/IEC 27001 and ISO/IEC 27002 for privacy information management](https://www.iso.org/standard/71670.html) — the privacy management system standard referenced in exercise-04's facet-map stretch goal. Paywalled.
- [ISO 22301:2019 — Security and resilience — Business continuity management systems — Requirements](https://www.iso.org/standard/75106.html) — the business-continuity management system standard chapter 10 and exercise 04 name as a third facet an integrated management system may add over time. Paywalled.

## Conformity assessment — the parent (chapters 11; exercise 06)

- [ISO/IEC 17021-1:2015 — Conformity assessment — Requirements for bodies providing audit and certification of management systems — Part 1: Requirements](https://www.iso.org/standard/61651.html) — the general requirements standard for management-system certification bodies. ISO/IEC 42006 specialises this for AIMS certification. Paywalled.
- [IAF (International Accreditation Forum) — Mandatory Documents](https://iaf.nu/en/iaf-documents/) — the mandatory documents governing accredited-body practice under the IAF Multilateral Recognition Arrangement (IAF MLA). Track the AI-specific documents as they are published.
- [IAF Mandatory Document IAF MD 5 — Determination of Audit Time of Quality, Environmental, and Occupational Health & Safety Management Systems](https://iaf.nu/en/iaf-documents/?cat=documents) <!-- needs-research: confirm the current IAF Mandatory Document set applicable to AIMS at reading time; MD 5 is the traditional auditor-day model that the ISO/IEC 42001 / 42006 duration model adapts. --> — the traditional auditor-day model referenced qualitatively in chapter 11's sampling discussion.
- [IAF Mandatory Document IAF MD 9 — Application of ISO/IEC 17021-1 in the field of medical device quality management systems](https://iaf.nu/en/iaf-documents/?cat=documents) — cited only as an example of a sector-specific 17021-1 application; useful comparator for the ISO/IEC 42006 pattern.

## Annex SL — the harmonised management-system structure (chapters 01, 10)

- [ISO/IEC Directives Part 1 and Consolidated ISO Supplement — Annex SL (the "harmonised structure")](https://www.iso.org/sites/directives/current/consolidated/index.xhtml) — the source of the Annex SL clause pattern (Clauses 4–10) that ISO/IEC 42001 and ISO/IEC 27001 share. Chapter 01's "same shape" argument relies on this.

## The EU AI Act (referenced throughout for the high-risk overlay)

The AI Act is treated as regulatory input by this module, not re-authored here. Full treatment is in mod-104. The URLs below are what mod-105 chapters cite:

- [Regulation (EU) 2024/1689 (the EU AI Act) — consolidated Official Journal text](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — Articles 9 (risk-management system for high-risk providers), 10 (data governance), 11 (technical documentation), 12 (record-keeping / logging), 13 (transparency to deployers), 14 (human oversight), 15 (accuracy / robustness / cybersecurity), 26 (deployer obligations), 27 (fundamental-rights impact assessment for certain deployers), 71 (EU database), 72 (post-market monitoring), 73 (serious-incident reporting), and Annex III (high-risk-use-case list including insurance-and-life-and-health) are directly referenced by module-105 chapters and exercises.
- [European Commission — AI Act policy page](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai) — implementation timeline; delegated / implementing act status; harmonised-standards references (the CEN-CENELEC JTC 21 output the AI Act will operationalise).
- [European AI Office](https://digital-strategy.ec.europa.eu/en/policies/ai-office) — the Commission body responsible for GPAI oversight and coordination with national competent authorities.

## GDPR — the DPIA / AIA relationship (chapter 04; exercise 05)

- [Regulation (EU) 2016/679 (GDPR) — consolidated Official Journal text](https://eur-lex.europa.eu/eli/reg/2016/679/oj) — Article 22 (automated individual decision-making) and Article 35 (DPIA) are the anchors chapter 04 and exercise 05 name for the DPIA-and-AIA composition.
- [Article 29 Working Party / EDPB — Guidelines on Data Protection Impact Assessment (WP 248 rev.01)](https://ec.europa.eu/newsroom/article29/items/611236/en) — the DPIA methodology guidance the AIA composes with rather than duplicates.

## Sector overlays referenced in the Halden scenario (exercises 01–06)

### Insurance regulation — Solvency II

- [Directive 2009/138/EC (Solvency II) — consolidated Official Journal text](https://eur-lex.europa.eu/eli/dir/2009/138/oj) — the primary framework directive for European insurance regulation, including the model-governance expectations that overlay the pricing and reserving models in the Halden scenario. <!-- needs-research: confirm the specific Solvency II Delegated Regulation (Commission Delegated Regulation (EU) 2015/35) article for internal-model governance if the Halden pricing model is treated as internal-model input; the standard-formula calculation uses a different article. -->
- [Commission Delegated Regulation (EU) 2015/35 (Solvency II Delegated Regulation)](https://eur-lex.europa.eu/eli/reg_del/2015/35/oj) — the implementing regulation carrying the detailed model-governance requirements.
- [EIOPA — Supervisory statement on the use of governance arrangements in third-country branches](https://www.eiopa.europa.eu/browse/regulation-and-policy/supervisory-convergence-tools_en) <!-- needs-research: EIOPA publishes AI-specific supervisory-convergence guidance at intervals; check the current AI-and-machine-learning supervisory statement or opinion at reading time. --> — track EIOPA AI-and-ML guidance as it evolves.

### US insurance regulation

- [NAIC Model Bulletin — Use of Artificial Intelligence Systems by Insurers (adopted December 2023)](https://content.naic.org/sites/default/files/inline-files/2023-12-4%20Model%20Bulletin_Adopted_0.pdf) <!-- needs-research: verify the direct PDF URL and the current adopted list of state insurance departments that have issued the bulletin at reading time; adoption has spread across many states since December 2023. --> — the model bulletin state insurance departments have widely adopted; overlays the US specialty book in the Halden scenario.
- [NAIC Big Data and Artificial Intelligence (H) Working Group page](https://content.naic.org/cmte_h_bdai.htm) — track ongoing NAIC AI-related work at reading time.

## Financial-services model risk (referenced qualitatively)

- [Federal Reserve — SR 11-7: Guidance on Model Risk Management](https://www.federalreserve.gov/supervisionreg/srletters/sr1107.htm) — the US banking model-risk-management anchor referenced across financial-services AI governance discussions. Not a Halden scenario overlay directly, but useful comparator.

## IEEE 7000 — the concept-of-operations ethics review upstream (exercise 05)

- [IEEE 7000-2021 — IEEE Standard Model Process for Addressing Ethical Concerns during System Design](https://standards.ieee.org/ieee/7000/6781/) — the concept-of-operations ethics-review methodology referenced by chapter 04 and exercise 05 as upstream input to the AIA. Paywalled but with abstract and clause structure visible in the IEEE preview.

## NIST AI RMF — comparator framework (referenced in the US and Halden Specialty overlays)

- [NIST AI Risk Management Framework 1.0 (AI 100-1, January 2023)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf) — the US voluntary framework the OMB memoranda anchor to; the rebuttable-presumption anchor for some US state statutes and a widely-cited comparator to ISO/IEC 42001. The Halden US specialty book overlay in exercise-02's SoA touches this qualitatively.
- [NIST AI RMF Generative AI Profile (AI 600-1)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf) — the GenAI-specific profile companion to AI 100-1; relevant to the claims-triage assistant and broker-facing assistant in the Halden portfolio.

## Reading and preparation guides published by ISO and accreditation bodies

- [ISO — Guidance on AI management (published corporate content)](https://www.iso.org/artificial-intelligence.html) — the ISO corporate portal for AI standards; useful for locating the current family membership and any explanatory white papers ISO has published.
- [BSI, DNV, TÜV, and comparable IAF-accredited certification bodies — public-facing ISO/IEC 42001 readiness materials](https://www.bsigroup.com/en-GB/products-and-services/standards/iso-iec-420012023/) <!-- needs-research: certification bodies publish readiness guides that supplement 42001 with implementation examples; consult one or more current bodies' materials at reading time. --> — useful non-authoritative supplements to the standard, especially for the two-stage audit shape and typical stage-1 preconditions. Verify each body's materials against the standard when they diverge.

## How to use this list

- **When authoring an SoA row** (exercise 02): the Annex A control text is in ISO/IEC 42001:2023 itself; the implementation guidance is in Annex B; the risk-source input is in Annex C. Consult all three sections of the standard together.
- **When designing the risk process** (exercise 03): the four-layer stack is 31000 → 23894 → 42005 → 42001 Clause 6.1. Read the four documents in that order; the composition depends on the ordering.
- **When designing the AIMS+ISMS integration** (exercise 04): read ISO/IEC 42001 Annex D (integration with other management systems) alongside ISO/IEC 27001 Annex A and the Annex SL clause pattern the two share. Integration is a *shape* question; the shape is Annex SL.
- **When wiring 42005 into the AIMS** (exercise 05): the AIA methodology is 42005; the AIMS requirement that it be integrated is 42001 Clause 6.1.4; the risk-guidance layer is 23894. The GDPR Article 35 DPIA sits alongside as a related but distinct instrument.
- **When preparing for third-party audit** (exercise 06): 42006 is the auditor's rulebook; ISO/IEC 17021-1 is its parent; the IAF Mandatory Documents govern the accredited body's practice. Read all three at overview depth; the enterprise's discipline is to make the auditor's required activities land on evidence that is present, accessible, and defensible.

## Notes on citation currency

Standards move. ISO family members are amended, revised, and occasionally superseded. The URLs above resolve to the ISO catalogue's *current* record for each standard; if a revision has replaced a cited edition, the catalogue entry redirects and the record shows the current edition. When authoring exercise deliverables, always confirm the edition citation is current at authoring time and update any `<!-- needs-research: ... -->` markers accordingly. The chapter and exercise texts of mod-105 anchor to the editions cited above at authoring time (2026); readers reaching this module in a later year should confirm currency before treating any specific edition citation as authoritative.
