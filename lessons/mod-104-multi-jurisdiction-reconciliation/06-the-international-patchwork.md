# The international patchwork — UK, Canada, China, Singapore, Australia, India, Korea, Brazil

## Why this chapter exists

The EU AI Act (chapter `02-reading-the-eu-ai-act-as-architectural-input.md`) and the US federal-and-state frame (chapters 4 and 5) do not exhaust the reconciliation problem. Every enterprise with international reach must read a second patchwork — the international one — whose regimes are further apart in *shape* than the US regimes are apart in *content*. The UK layers regulator-led guidance on top of existing sectoral law; Canada has both a stalled comprehensive bill and a live federal-government directive; China has already-effective interim measures for generative AI plus mandatory security assessments; Singapore has a mature voluntary framework and an evaluation-tooling programme; Australia has moved from voluntary standard to proposed mandatory guardrails; India lands AI under its new data-protection statute plus an NITI Aayog framework; Korea has just passed a horizontal AI act; Brazil has a comprehensive bill working through the Congress.

The level-50 architect's job with the international patchwork is not to become an expert in each — that is legal's and government-affairs' domain — but to *know the shape of each regime well enough to attach it to the reconciliation architecture correctly*. This chapter walks the regimes named in the objective, pins each to its addressee / trigger / demand / consequence, and lands them in the reconciliation architecture. Where information is fast-moving (particularly Canada AIDA status, Brazil PL 2338/2023 status, and Korea AI Basic Act implementation), the chapter marks the fast-moving parts with `needs-research` and encourages the reader to verify against the primary source at reading time.

## United Kingdom — Pro-Innovation Approach, AISI methodology, ATRS

**Pro-Innovation Approach to AI Regulation.** The March 2023 white paper (updated with a February 2024 government response) sets the UK's default posture: no horizontal AI statute; instead, five cross-sector principles (safety, security and robustness; appropriate transparency and explainability; fairness; accountability and governance; contestability and redress) that existing sector regulators (ICO, FCA, PRA, MHRA, Ofcom, HSE, CMA, EHRC, ...) are asked to apply through their existing authorities. Followed by continuing publications; a horizontal Act has been floated but not enacted as of writing.

<!-- needs-research: check whether the UK has moved from the pro-innovation white-paper posture toward a horizontal AI act (variously reported as the "AI Bill" under either Labour or Conservative sponsorship). If a statute has been introduced or passed, update the paragraph. -->

**UK AI Safety Institute (UK AISI).** Announced November 2023 at the Bletchley AI Safety Summit. Publishes evaluation methodology for frontier models and partners with the US AISI and other national AISIs through the International Network. Voluntary agreements with major foundation-model providers allow pre-deployment evaluation of designated models.

**Algorithmic Transparency Recording Standard (ATRS).** The UK's public-sector-facing algorithmic transparency artefact shape. Managed by the Central Digital and Data Office. Public bodies using algorithmic tools in decisions that meaningfully affect the public are required (as of adoption in central government) to publish an ATRS record; the record follows a specified content structure covering how the tool works, its purpose, its data, and human oversight.

**Architectural takeaway.** UK obligations attach *through the sector regulator that would already regulate the enterprise* — for a bank, the FCA / PRA; for a health-service supplier, the MHRA; for a consumer-data processor, the ICO. The reconciliation architecture does not carry a "UK AI Act" scope; it carries a *UK-jurisdiction-plus-sector* filter attribute, and each sector regulator's guidance shows up as a crosswalk edge on the relevant controls. UK AISI methodology attaches on the GPAI-evaluation control (chapter `04-the-us-federal-frame.md` addressed the US AISI as reference; the UK AISI serves the same role in the UK-facing crosswalk). ATRS attaches as an *evidence-artefact rendering* on the enterprise's model-card / transparency-artefact control when the customer is a UK public body — one control, one artefact, multiple renderings.

Shape B is rare in the UK; shape A dominates. The single exception is where the enterprise sells to UK public bodies and the ATRS becomes the primary evidence rendering — the ATRS-specific content requirements may force an extension to the model-card evidence contract.

## Canada — AIDA and TBS Directive on Automated Decision-Making

**Artificial Intelligence and Data Act (AIDA).** Part of Bill C-27 (Digital Charter Implementation Act), introduced June 2022. Would establish federal obligations on providers and deployers of *high-impact* AI systems, with a definition to be filled in by regulation. Status: Bill C-27 was pending at prorogation and has not passed into law as of writing. <!-- needs-research: verify current status of Bill C-27 / AIDA in the new Parliament — whether it has been reintroduced, whether it passed, and if so, what the final text and effective dates are. If it has passed, this section's shape-of-obligations treatment must be replaced with the actual statutory obligations. -->

**Treasury Board Secretariat Directive on Automated Decision-Making.** In force for federal-government use of automated decision systems since April 2019 (revised subsequently). Requires an *Algorithmic Impact Assessment* (AIA — a specific questionnaire producing a score that maps to one of four *impact levels*), with progressively stricter obligations at higher impact levels (peer review, notice, explanation, human intervention, quality assurance, testing).

**Architectural takeaway.** For a vendor to the Canadian federal government, the TBS Directive is the primary reconciliation input — the Directive's AIA is a specific artefact shape (published questionnaire, published scoring rubric) that must be produced. Shape B for the AIA control if the enterprise has no analogue; more commonly shape A extending the enterprise's FRIA / DPIA control with the AIA as one of the acceptable renderings. The impact-level thresholds map onto the enterprise's system-tiering — the reconciliation architecture should carry a crosswalk between the enterprise tier and the TBS impact level so the enterprise can attest to the level at intake rather than at each contract award.

For AIDA (if and when enacted), the *provider* obligations will likely mirror EU AI Act Article 9-15 in shape, and the reconciliation should be primarily shape-A crosswalk-edge work. The watch-list mechanism (chapter `07-designing-the-reconciliation-architecture.md`) should have AIDA queued.

## China — Interim Measures for Generative AI Services and TC260 Basic Safety Requirements

**Interim Measures for the Management of Generative Artificial Intelligence Services.** Issued jointly by the Cyberspace Administration of China (CAC) and six other authorities; effective 15 August 2023. Applies to *the provision of generative AI services to the public within the People's Republic of China*. Requires providers to conform to socialist core values; take measures to prevent discrimination; respect intellectual property, business ethics, and personal privacy; ensure the authenticity, accuracy, objectivity, and diversity of training data; and — critically — undergo a *security assessment* filed with the CAC before providing services with *public-opinion attributes* or *social-mobilisation capacity*.

**TC260 Basic Safety Requirements for Generative AI Services.** Technical standard issued by the National Information Security Standardization Technical Committee (TC260). Provides the technical safety-requirements shape that the Interim Measures' security assessment is measured against. Covers training-data safety, model safety, service safety, and monitoring shape requirements.

<!-- needs-research: verify the current version of the TC260 Basic Safety Requirements (initial version issued 2024, subject to update cycles), and any additional joint measures issued by CAC / MIIT / MPS / MoST after the initial Interim Measures. -->

**Architectural takeaway.** China's regime is *hard, filed, and inspected* — the security assessment is a documented submission to CAC and the TC260 shape is prescriptive. For an enterprise providing GenAI services in China:

1. Shape B for the *CAC security-assessment submission* control — the specific submission format, the specific filing recipient, the specific pre-launch and post-material-change triggers have no clean pre-existing analogue.
2. Shape A for the *training-data safety* controls — the TC260 requirements overlap with EU AI Act Article 10 and NIST AI RMF data-governance controls, with China-specific extensions (source lawfulness, personal-information handling under PIPL, IP handling).
3. Shape A for the *content-safety* controls — the "socialist core values" and content-restriction requirements attach to the enterprise's content-moderation and safety-filter controls, with China-specific rulesets.
4. A distinct addressee scope on the applicability filter — many enterprises operate in China through a joint venture or licensed local entity whose relationship to the extraterritorial parent must be encoded, so the *actual filing entity* per obligation is unambiguous.

The China scope is one of the two regimes (with Korea) where the reconciliation architecture may need to acknowledge that certain obligations are *incompatible* with other regimes — the socialist-core-values content requirement is not something an EU-facing product can adopt. Reconciliation in that case means *deploying a different variant of the system per market*, and the applicability filter must carry the variant identifier.

## Singapore — Model AI Governance Framework and AI Verify

**Model AI Governance Framework.** Issued by the Infocomm Media Development Authority (IMDA) and the Personal Data Protection Commission. First edition 2019, second edition 2020, *Model AI Governance Framework for Generative AI* published 2024. Non-binding but authoritative shape for AI governance across four dimensions: internal governance structures and measures; determining the level of human involvement in AI-augmented decision-making; operations management; stakeholder interaction and communication. The GenAI framework extends across accountability, data, trusted development and deployment, incident reporting, testing and assurance, security, content provenance, safety and alignment research, and AI for public good.

**AI Verify.** The IMDA/AI Verify Foundation's testing framework and open-source toolkit. Combines process checks against internationally recognised AI ethics principles with quantitative technical tests (fairness, explainability, robustness) to produce a testing report the organisation can share with stakeholders. Voluntary; not a certification.

**Architectural takeaway.** Singapore's framework is *reference architecture* rather than regulation. The reconciliation architecture treats it as:

1. An anchor source in the mod-102 chapter 03 threat-family composition (particularly for GenAI evaluation), attached as a crosswalk edge on relevant controls.
2. An *evidence-artefact-shape reference* — where the enterprise operates in Singapore or sells into Southeast Asian markets that reference Singapore's framing, the AI Verify report is an acceptable evidence rendering on the evaluation control.
3. A *methodology reference* for the enterprise's testing / evaluation control's evidence contract, alongside the US AISI and UK AISI methodologies.

No shape B; no independent applicability-filter dimension beyond `jurisdiction: singapore` for enterprises with a Singapore presence.

## Australia — Voluntary AI Safety Standard and proposed mandatory guardrails

**Voluntary AI Safety Standard.** Published by the Department of Industry, Science and Resources in September 2024. Ten *guardrails* covering accountability, risk management, data governance, testing, human oversight, information for users, contestability, supply-chain transparency, records, and engagement. Voluntary.

**Proposed Mandatory Guardrails for AI in High-Risk Settings.** Consultation paper issued September 2024 by the same department. Proposes the ten guardrails (or a substantially similar set) become mandatory for AI in high-risk settings, with a to-be-determined definition of high-risk. Status: consultation-and-development. <!-- needs-research: verify whether the mandatory guardrails have progressed from consultation to legislation, and if so, the final scope, guardrail list, and effective dates. -->

**Architectural takeaway.** Australia's voluntary standard is a *shape-A crosswalk edge* on existing controls today — it composes cleanly with NIST AI RMF and ISO/IEC 42001. The mandatory guardrails are on the watch list. Pre-work: map each of the ten guardrails to the enterprise controls that would discharge them, and identify any guardrail without a natural control home (contestability is a common one) so shape-B work is queued if and when the mandatory instrument passes.

## India — DPDPA and NITI Aayog Responsible AI

**Digital Personal Data Protection Act (DPDPA).** Enacted August 2023. Comprehensive data-protection statute applicable to digital personal data processed within India (and to processing outside India in connection with offering goods or services to data principals within India). Establishes data-fiduciary duties, data-principal rights, and Significant Data Fiduciary designations with elevated obligations. AI systems processing personal data fall under DPDPA's data-fiduciary obligations.

<!-- needs-research: verify current status of DPDPA implementation rules issued by the government, and any AI-specific provisions in those rules. -->

**NITI Aayog Responsible AI for All.** Two-part document (Approach Document Part 1 2021; Operationalizing Principles Part 2 2021). Establishes principles for Responsible AI in India (safety and reliability; equality; inclusivity and non-discrimination; privacy and security; transparency; accountability; protection and reinforcement of positive human values). Non-binding but frames government AI strategy and public-sector procurement expectations.

**Architectural takeaway.** India is primarily a *DPDPA plus soft-framework* regime today, with an anticipated AI-specific instrument. For an enterprise processing personal data of Indian data principals:

1. Shape A on data-protection controls: DPDPA obligations largely map onto the enterprise's GDPR-anchored data-protection controls, with India-specific extensions (Significant Data Fiduciary thresholds, cross-border transfer restrictions per notified rules).
2. Shape A on transparency and accountability controls: NITI Aayog framing as a crosswalk edge; where the enterprise sells into the Indian public sector, NITI Aayog framing shows up in procurement.
3. Watch list for the anticipated horizontal AI instrument.

## Korea — AI Basic Act

**Act on the Development of Artificial Intelligence and the Establishment of Trust (AI Basic Act).** Passed by the National Assembly 26 December 2024, promulgated January 2025, enters into force 22 January 2026. Establishes obligations on providers of *high-impact AI* and *generative AI*, transparency obligations, and a domestic-representative requirement for foreign providers. Enforcement by the Ministry of Science and ICT.

<!-- needs-research: verify the AI Basic Act's final scope (particularly the high-impact-AI definition and thresholds), the transparency-obligation trigger set, and the enforcement regime and penalty schedule against the promulgated text and any subsequent enforcement decrees. -->

**Architectural takeaway.** Korea's AI Basic Act is the first horizontal AI statute in Asia and its shape is broadly EU-AI-Act-adjacent, with different definitional lines and a lighter penalty regime. For an enterprise operating in Korea:

1. Shape A on risk-management, transparency, human-oversight, and evaluation controls — most obligations map onto the same underlying controls the EU AI Act references.
2. Shape B for the *domestic-representative appointment* control if the enterprise is foreign; the specific appointment shape, notice, and public disclosure obligation typically has no pre-existing analogue.
3. Shape B for the transparency-to-user obligations to the extent they diverge in content from EU AI Act Article 50 renderings.
4. Distinct addressee-scope `jurisdiction: korea` applicability-filter attribute; the effective-date attribute captures the 22 January 2026 entry-into-force.

Korea joins the EU as the second jurisdiction with a comprehensive horizontal AI act carrying penalty exposure; the reconciliation architecture must be able to handle a *second* set of horizontal obligations with different definitions but overlapping shape, without duplicating controls.

## Brazil — PL 2338/2023

Bill 2338/2023 was approved by the Federal Senate in December 2024, with the text now under consideration by the Chamber of Deputies (Câmara dos Deputados). If enacted, it will establish a comprehensive AI regime with risk classification (excessive-risk prohibitions; high-risk obligations including impact assessments, transparency, human oversight, and post-market monitoring; general obligations for all AI systems), a supervisory authority function (likely designated to the ANPD or a coordinated body), and penalty exposure.

<!-- needs-research: verify current status of PL 2338/2023 in the Chamber of Deputies, any amendments in the House, the final signed shape (if enacted), and the effective-date staging. -->

**Architectural takeaway.** Brazil is on the watch list, with the same shape as Korea and the EU AI Act — pre-work is (a) identifying the enterprise's role (provider vs deployer vs operator per the Brazilian bill's addressee taxonomy) per system, (b) mapping the bill's likely risk-classification categories to the enterprise's tiering, (c) queuing shape-A crosswalk-edge work on existing risk-management, impact-assessment, transparency, and post-market-monitoring controls. Where the bill's supervisory authority regime is settled, the enterprise's incident-reporting control will need a Brazil-specific recipient rendering (probable shape B).

## Designing the multi-country overlay

Six design decisions the architect makes once for the international patchwork:

**Decision 1 — Jurisdiction taxonomy at the country level.** The applicability-filter jurisdiction attribute needs the full enumerated set of countries (or country-plus-sub-national where relevant). Country codes (ISO 3166-1 alpha-2 is the workable choice) plus supra-national buckets for the EU and, where relevant, ASEAN or Council of Europe.

**Decision 2 — Territorial-scope rules.** Every jurisdiction defines its own reach differently. GDPR reaches processing outside the EU where connected to EU offers. Colorado reaches out-of-state deployers deploying against Colorado consumers. China's Interim Measures reach services provided to the Chinese public. Korea's AI Basic Act reaches into foreign providers via the domestic-representative mechanism. India's DPDPA has extraterritorial reach conditions. The reconciliation architecture must carry a *territorial-scope evaluation function* on each obligation — not just "this obligation exists in jurisdiction X" but "this obligation reaches into this system because of these facts." Legal owns the interpretation; the architecture carries the encoding.

**Decision 3 — Variant management.** Where two regimes' obligations are *incompatible* (China's content-safety rules vs an EU-facing product's expected content behaviour; Korea's specific transparency wording vs the EU AI Act's), the system must be *deployed as a variant* per market. The applicability filter carries the variant identifier as an attribute, and controls attach to specific variants rather than to the system in the abstract. This is where mod-105 AIMS and mod-108 evidence architecture meet — the variant identifier appears on the model card, on the AIMS system record, and on the compliance evidence.

**Decision 4 — Filing-entity attribution.** Many international deployments are through licensed local entities (China joint venture; Korea local entity; India Significant Data Fiduciary). The reconciliation architecture must record the *filing entity* for each obligation — the entity whose signature appears on a CAC filing, the entity that appoints the Korean domestic representative, the entity that files the Brazilian impact assessment. When the entity is not the parent enterprise, controls carry an *entity attribute* on the applicability filter.

**Decision 5 — Language-of-record.** Some regimes require artefacts in the local language. Article 27 FRIA under EU AI Act does not specify, but member-state supervisory-authority practice often expects the local language for submissions. Korea, Brazil, China explicitly expect local-language artefacts. The evidence contract must carry a *language rendering* attribute for each obligation, and mod-108 evidence architecture must budget translation infrastructure.

**Decision 6 — Watch-list mechanics.** Half the international patchwork is in flight — Brazil PL 2338, Canadian AIDA, Australian mandatory guardrails, further UK activity. The reconciliation architecture's watch-list workflow (chapter `07-designing-the-reconciliation-architecture.md`) must be a first-class artefact with a review cadence (quarterly is typical for the international watch list), a per-bill status update, and a *trigger-to-active* checklist for each watched bill.

## Two failure modes

**Failure mode 1 — mapping to shape rather than substance.** The enterprise notices that Korea's AI Basic Act, Brazil's PL 2338, and the EU AI Act all use similar high-risk framing and treats them as one obligation set. Six months later a Korean regulator asks about the specific domestic-representative appointment and there is no evidence. The fix is per-jurisdiction crosswalk edges plus per-jurisdiction evidence renderings — the *underlying practice* may be common, but the *evidence artefact per regulator* is not.

**Failure mode 2 — over-relying on a single anchor source.** The enterprise pins all its evaluation methodology to the US AISI reference and finds a Chinese CAC assessor unwilling to accept the US methodology as sufficient. The reconciliation architecture must carry *multiple acceptable methodology references* per control (US AISI, UK AISI, Singapore AI Verify, TC260 Basic Safety Requirements as applicable) and produce the appropriate reference per audit context.

## Summary

The international patchwork is UK (regulator-led, plus AISI evaluation methodology and ATRS artefact shape), Canada (TBS Directive live for federal use; AIDA pending), China (already-effective Interim Measures for GenAI plus TC260 mandatory security requirements), Singapore (mature voluntary framework and AI Verify testing toolkit), Australia (voluntary standard, mandatory guardrails in consultation), India (DPDPA plus NITI Aayog framing), Korea (comprehensive AI Basic Act entering force January 2026), and Brazil (PL 2338/2023 progressing through the legislature). The reconciliation architecture handles this patchwork by carrying a full country-level jurisdiction taxonomy, an explicit territorial-scope evaluation per obligation, variant management for incompatible-regime cases, filing-entity attribution for locally-established operations, language-of-record renderings on the evidence contract, and a formal watch-list workflow for the several bills still in flight. Shape A dominates because the underlying practices are common; shape B lands on the genuinely local obligations (CAC security-assessment filing; Korean domestic-representative appointment; Canadian TBS AIA; Colorado-analogues by state; Brazilian impact-assessment filing). Chapter `07-designing-the-reconciliation-architecture.md` folds all of this into the single schema — obligation record, applicability filter, evidence contract, deprecation path — that lets the enterprise scale to fifteen jurisdictions on one control library.
