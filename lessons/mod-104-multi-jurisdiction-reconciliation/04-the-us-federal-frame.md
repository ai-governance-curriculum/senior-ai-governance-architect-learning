# The US federal frame — executive orders, OMB memos, and the AISI methodology

## Why this chapter exists

The United States has no horizontal AI statute. It has (a) *executive orders* that bind the executive branch and — through federal procurement — the vendors that sell to it, (b) *OMB memoranda* that operationalise those orders for federal agencies, (c) the *US AI Safety Institute* at NIST, which publishes evaluation methodology adopted by both the federal government and by private-sector signatories to voluntary agreements, and (d) sector regulators (FTC, EEOC, HHS/OCR, financial-services agencies) whose existing enforcement authorities capture AI use.

For a level-50 architect, the federal frame is a *procurement filter* first and a *supervisory-authority filter* second. If the enterprise sells any product into the federal government, or into a federal contractor of scale, the OMB-derived requirements flow down through the procurement contract; the enterprise's reconciliation architecture must be able to answer *which controls attach when the customer is a US federal agency, and which change with executive-order transitions*.

This chapter reads the current-state frame — Executive Order 14179 (Trump, January 2025) with EO 14110 as historical context — and the OMB memoranda that operationalise it (M-24-10, M-24-18 for context; M-25-21, M-25-22 as the current instruments), plus the US AISI methodology. The point is to design the federal-facing crosswalk, not to memorise the memos.

## Executive orders — 14110 as historical context, 14179 as current

**Executive Order 14110 — "Safe, Secure, and Trustworthy Development and Use of Artificial Intelligence"** — issued by President Biden on 30 October 2023. Wide scope: instructed federal agencies to designate Chief AI Officers, established reporting and evaluation requirements for federal AI use, directed NIST to develop AI Safety Institute methodologies, imposed dual-use foundation-model reporting thresholds on private developers via the Defense Production Act, and instructed OMB to issue guidance to agencies. The immediate operational implementations were OMB M-24-10 (agency AI risk management) and OMB M-24-18 (AI acquisition).

**Executive Order 14179 — "Removing Barriers to American Leadership in Artificial Intelligence"** — issued by President Trump on 23 January 2025. Revoked EO 14110. Directed the assistant to the president for science and technology policy, the special advisor for AI and crypto, and the assistant to the president for national security affairs to develop and submit an AI Action Plan within 180 days. Directed the OMB Director to review and revise, as appropriate, existing OMB memoranda issued pursuant to EO 14110 to remove inconsistencies with the new administration's policy. That review produced the current OMB M-25-21 and M-25-22, which superseded M-24-10 and M-24-18 respectively.

<!-- needs-research: verify the exact issuance dates and titles of M-25-21 and M-25-22 against the OMB website; the objectives lists these as the current versions but the specific policy pivots (reduced barriers to federal AI adoption; retention of NIST AI RMF as a reference framework; removal of some private-sector reporting language) should be reconciled with the published memoranda before authoring worked examples that cite specific paragraphs. -->

**Architectural takeaway — build for executive-order transitions.** The single most important architectural fact about US federal AI regulation is that *executive orders change under administrations*. A reconciliation architecture that hard-wires "EO 14110" into controls will break in 15 months. Instead:

1. The obligation record (chapter `07-designing-the-reconciliation-architecture.md`) references the *current* executive order and OMB memoranda, with the *superseded-by* deprecation-path field carrying the transition. When EO 14179 revoked EO 14110, obligation records referencing EO 14110 acquired a `superseded_by: EO 14179` edge and, per obligation, a `migration_deadline` and a `superseded_evidence_valid_until` window.
2. The applicability filter on controls does not name the executive order directly. It names the *underlying OMB memorandum* (which is the operational instrument the enterprise vendor's contract flows down) and, for contract-inherited obligations, the *procurement contract clause*. When the memoranda change, the filter update is at the memorandum level; controls remain stable.
3. Public statements by the enterprise — the responsible-AI web page, the annual report, marketing materials — should reference *durable* frameworks (NIST AI RMF, NIST AI 600-1, US AISI methodology) rather than executive orders. Executive orders reference the frameworks; the frameworks outlast the orders.

## OMB M-24-10 — the previous federal-agency AI risk-management memorandum (context)

**"Advancing Governance, Innovation, and Risk Management for Agency Use of Artificial Intelligence,"** issued 28 March 2024 by then-OMB Director Shalanda Young, in implementation of EO 14110. Required each covered agency to designate a Chief AI Officer, establish an AI Governance Board, maintain a public inventory of AI use cases, implement minimum risk-management practices for *rights-impacting* and *safety-impacting* AI (waivable in narrow circumstances), and comply with reporting requirements. Superseded in 2025 by M-25-21.

**Why the architect still reads it.** M-24-10 established vocabulary — *rights-impacting AI*, *safety-impacting AI*, *presumed AI use case*, *categorical waiver*, *use case inventory* — that the successor memorandum largely retained. Controls that were designed against M-24-10 vocabulary continue to work under M-25-21 with crosswalk-edge updates. And where the enterprise carries pre-2025 evidence, that evidence must remain valid against the memorandum in force at the time of its production — the deprecation-path window matters.

## OMB M-24-18 — the previous federal-AI-acquisition memorandum (context)

**"Advancing the Responsible Acquisition of Artificial Intelligence in Government,"** issued 3 October 2024 by OMB. Required agencies to align AI procurement with M-24-10's risk-management practices, flow certain requirements to vendors, address performance monitoring and vendor accountability in contract structure, and support supply-chain risk-management for AI. Superseded in 2025 by M-25-22.

**Why the architect still reads it.** For any AI vendor selling to the federal government pre-2025, the contract-flowed-down requirements in M-24-18 became directly binding through the contract. Those contracts remain in force for their remaining terms even where the successor memorandum has changed the flow-down set. The reconciliation architecture must be able to answer *which controls apply because a live contract flowed them down under a superseded memorandum* — a per-contract applicability-filter attribute is the mechanism.

## OMB M-25-21 — the current federal-agency AI use memorandum

Issued in 2025, superseding M-24-10 for federal agency use of AI. Retains the Chief AI Officer, AI Governance Board, and AI use-case inventory apparatus. Reframes the minimum-risk-management-practice thresholds to focus on *high-impact* AI (the successor label to *rights-impacting* and *safety-impacting* in the prior memorandum) and updates the waiver process. Reaffirms the NIST AI RMF as a primary risk-management reference for federal agencies.

<!-- needs-research: pin the exact title, issuance date, and paragraph-level reframing of M-24-10 into M-25-21 against the published memorandum before authoring worked examples that cite specific sections. The chapter's characterisation of the high-impact reframing and the NIST AI RMF reference is directionally correct but the specific wording should be verified. -->

**Architectural takeaway.** For a vendor enterprise, M-25-21 shapes what the federal customer expects the vendor to *look like* — the customer's AI governance board reads the vendor's model documentation, applies its own risk-tiering process, and expects the vendor to be able to speak the memorandum's vocabulary. The enterprise's mod-108 evidence architecture must produce artefacts that map onto that vocabulary: a model card whose sections align to the memorandum's high-impact-AI evaluation categories, a risk-management artefact aligned to NIST AI RMF, a monitoring artefact aligned to the memorandum's post-deployment expectations. All are shape-A extensions of existing controls; the extension is in the artefact renderings, not in the controls themselves.

## OMB M-25-22 — the current federal-AI-acquisition memorandum

Issued in 2025, superseding M-24-18. Governs how federal agencies acquire AI systems and services. Adjusts the flow-down set from the prior memorandum, retains vendor accountability and performance-monitoring themes, and aligns acquisition requirements with the M-25-21 high-impact-AI framing.

<!-- needs-research: confirm the M-25-22 title, issuance date, and specific flow-down clause language against the published memorandum. The chapter's characterisation of the alignment with M-25-21 is directionally correct. -->

**Architectural takeaway — this is the memorandum that flows down.** The enterprise as vendor sees M-25-22 more than M-25-21. The FAR clauses (or FAR-equivalent agency-specific clauses) that implement M-25-22 attach directly to the contract. The reconciliation architecture treats these as:

1. An *addressee scope* — "US federal customer" — on the applicability filter of relevant controls.
2. A *contract-flowed obligation set* on each contract, referencing the specific clauses of that contract. The obligation register (chapter `07-designing-the-reconciliation-architecture.md`) can carry contract obligations alongside statute- and regulation-derived obligations; the deprecation path on contract obligations is the contract expiry, not a regulatory sunset.
3. A *pre-award artefact set* — the enterprise's response to solicitation requirements — that mod-108 evidence architecture must be able to produce on demand. Every artefact the memorandum names becomes an evidence-contract line item on some existing control, plus (rarely) a shape-B control for artefacts with no existing home.

## The US AI Safety Institute (US AISI) methodology

Established at NIST in November 2023 under EO 14110 (retained under EO 14179 with adjusted scope). Publishes evaluation methodology for foundation-model capabilities, safety, and misuse risks. Partners with the UK AISI and other national AI safety institutes through the International Network of AI Safety Institutes (formed at the AI Safety Summit follow-on in 2024).

**Key methodology outputs.** Foundation-model evaluation protocols including capability evaluations (across dangerous capability categories like cyber, biological, chemical, and radiological), safety evaluations, and adversarial testing (red-teaming). Model documentation and reporting shapes. Voluntary agreement templates that private-sector foundation-model providers can sign to commit to pre-deployment evaluation of designated models.

<!-- needs-research: confirm the current name, organisational placement within NIST, and the specific methodology publications (evaluation-protocol titles, voluntary-agreement templates) of the US AISI as of the current administration; the objective lists US AISI methodology, but the specific published artefacts should be pinned against the AISI publication list. -->

**Architectural takeaway.** For a GPAI-provider enterprise:

1. The US AISI methodology is *reference*, not *regulation* — it does not directly bind an enterprise. But it is the *most authoritative published methodology* the enterprise can point at when discharging obligations under EU AI Act Article 55 (systemic-risk GPAI evaluation) and UK-jurisdiction AISI methodology (chapter `06-the-international-patchwork.md`).
2. The enterprise's `AIC-GPAI-EVAL-*` control family should carry the US AISI methodology as an *acceptable methodology* in the evidence contract. When an EU competent authority asks *how did you evaluate this model?*, "using the US AISI methodology, augmented by ..." is a defensible answer; "using our internal evaluation" is not, unless the internal evaluation is documented against a comparable reference.
3. Voluntary-agreement signatures create *contractual* obligations to the AISI. Those obligations belong on the obligation register with `consequence: contractual` and a deprecation path tied to the agreement's term.

## Sector regulators — FTC, EEOC, HHS/OCR, financial-services agencies

The federal frame is not exhausted by executive orders and OMB. Existing sector-regulator enforcement authorities capture AI use directly.

- **Federal Trade Commission.** Section 5 of the FTC Act (unfair or deceptive acts or practices) applies to AI-driven marketing, advertising, and consumer-facing product claims. FTC guidance on AI ("Keep Your AI Claims in Check", "Aiming for Truth, Fairness, and Equity in Your Company's Use of AI") is non-binding but enforcement-adjacent. Data-security and privacy authorities under FTC Act Section 5 and the Health Breach Notification Rule interact with AI-adjacent breach scenarios.
- **Equal Employment Opportunity Commission.** Title VII, ADEA, and ADA apply to AI used in employment decisions. EEOC's Assessing Adverse Impact in Software, Algorithms, and Artificial Intelligence Used in Employment Selection Procedures (May 2023) provides the shape for adverse-impact analysis. The Biden-era AI and Algorithmic Fairness Initiative saw certain resource reallocation under the new administration; specific enforcement priorities are worth watching. <!-- needs-research: current EEOC enforcement posture on AI hiring tools under the current administration. -->
- **HHS Office for Civil Rights.** HIPAA and Section 1557 of the ACA apply to AI in healthcare. The Section 1557 final rule (May 2024) explicitly addresses discrimination arising from patient-care decision-support tools.
- **Financial-services agencies.** Federal Reserve, OCC, FDIC, CFPB, NCUA. SR 11-7 (Federal Reserve) and OCC Bulletin 2011-12 govern model risk management; SR 11-7 has been the primary reference for AI risk management in banking for a decade and is entirely applicable to modern AI systems. CFPB's guidance on adverse-action notices under ECOA and the enforcement against denial notices generated by AI credit-underwriting models makes ECOA a live AI regulator. Interagency statements on the use of AI (variously updated) provide additional shape.

**Architectural takeaway.** Sector-regulator obligations are the ones the reconciliation architecture *cannot ignore even if the enterprise reads only the horizontal frame*. Every sector-vertical the enterprise operates in adds its own applicability-filter dimension and, typically, a small number of shape-B controls where the sector's practice has no horizontal analogue (adverse-action-notice generation under ECOA; Section 1557 non-discrimination in clinical decision support). Mod-113 sector blueprints go deep on these; the point in the reconciliation chapter is to know that the *federal frame is horizontal-plus-sectoral* — omitting the sectoral half is the fastest way to be surprised by an enforcement action.

## Designing the federal-facing crosswalk — a checklist

For any control in the enterprise library, when the addressee-scope or use-case triggers the US-federal jurisdiction, the crosswalk field should carry — as applicable — edges to:

- The applicable OMB memorandum (M-25-21 for federal-agency-use obligations flowed down; M-25-22 for acquisition-flow-down obligations).
- The NIST AI RMF sub-category most on-point (both memoranda anchor to the RMF).
- The applicable sector regulator's guidance or rule (EEOC / ECOA / SR 11-7 / Section 1557 / FTC Act Section 5 as appropriate).
- The US AISI methodology, if the control is on the evaluation family for a system with foundation-model characteristics.
- The *superseded-by* edge to any prior executive order or OMB memorandum whose evidence remains valid for pre-transition production.
- The specific procurement-contract clause, if the obligation is contract-flowed rather than regulation-flowed.

## Two failure modes

**Failure mode 1 — writing controls to the executive order rather than the memorandum.** Controls that name "EO 14110" or "EO 14179" go stale every administration. Controls that name the OMB memorandum, the NIST framework the memorandum anchors to, and the procurement clause survive transitions with only crosswalk updates.

**Failure mode 2 — omitting the sector half.** An enterprise selling AI hiring tools that reads only the horizontal frame (EO / OMB) misses EEOC, which is where the enforcement risk lives. An enterprise selling AI clinical decision support that reads only the horizontal frame misses HHS/OCR, which is where the enforcement risk lives. The reconciliation architecture must carry the sector-regulator crosswalk edge for every applicable sector, and the applicability filter must carry the sector dimension.

## Summary

The US federal frame is executive orders (subject to administration transitions), OMB memoranda (the operational implementations that flow into contracts), the US AISI methodology (the reference evaluation shape for foundation models), and the sector regulators (whose existing statutes capture AI directly). EO 14179 revoked EO 14110 in January 2025 and produced M-25-21 and M-25-22 as the current implementations, with the prior memoranda M-24-10 and M-24-18 remaining relevant as the reference points for pre-transition evidence and for the deprecation-path window. The level-50 architect designs the federal-facing crosswalk against the *memoranda* rather than the *orders*, carries executive-order transitions in the deprecation path on the obligation record, layers sector-regulator crosswalks (EEOC, HHS/OCR, financial-services agencies, FTC) alongside the horizontal frame, and treats the US AISI methodology as reference-not-regulation that anchors the evaluation-family evidence contract. Get the memorandum-anchoring right and the enterprise survives the next administration transition with a documentation refresh rather than a rewrite.
