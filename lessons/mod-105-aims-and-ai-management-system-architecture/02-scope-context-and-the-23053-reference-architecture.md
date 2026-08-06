# Scope, context, and the ISO/IEC 23053 reference architecture — Clause 4

## Why this chapter exists

Clause 4 of the harmonised structure — *Context of the organisation* — sounds like a foreword. In an ISO/IEC 42001 AIMS it is the load-bearing wall. Clause 4.1 makes the enterprise state the *external and internal issues* the AIMS is expected to address. Clause 4.2 makes the enterprise enumerate the *interested parties* and their relevant requirements. Clause 4.3 makes the enterprise draw the *scope of the AIMS* — the artefact everything else in the system refers back to. Clause 4.4 makes the enterprise commit that a management system *exists* and is maintained. Every downstream artefact — the SoA, the risk process, the risk-treatment plan, the internal audit programme, the management review — is scoped by Clause 4.3. If the scope statement is fuzzy or over-broad, the AIMS is un-auditable. If it is under-broad, systems the enterprise ought to be governing float outside it and inherit no controls.

The level-50 architect authors the scope statement. Legal reviews the wording; the head of AI governance signs it; top management commits to it under Clause 5. But the *drafting* is architectural work because the scope statement is a technical artefact — it names AI systems, lifecycle stages, and organisational boundaries in terms that must survive an audit sample. The reference architecture the drafting reaches for is ISO/IEC 23053, so that the scope is expressed in vocabulary the certification body already knows.

## Clause 4.1 — external and internal issues

Clause 4.1 asks the enterprise to determine the external and internal issues *relevant to its purpose and that affect its ability to achieve the intended outcomes of the AIMS*. This is not a marketing exercise — the auditor will look for a documented determination that was actually used as input to Clause 6 planning.

**External issues** — items outside the enterprise's direct control that shape the AIMS. In an AI context:

- Regulatory environment — EU AI Act (with implementing acts and delegated acts arriving through the compliance windows), US federal executive orders and OMB memoranda, applicable state statutes, sector regulators (financial services, health, employment, education), international regimes the enterprise touches. Chapter cross-reference: mod-104 catalogues the regimes; the Clause 4.1 register carries the *ones-that-apply-to-this-enterprise* subset.
- Standards environment — the ISO/IEC 42001 family, NIST AI RMF and the Generative AI Profile, CEN-CENELEC JTC 21 harmonised-standards trajectory, sector-specific standards (e.g. IEC 62304 for medical software, ISO 26262 for automotive, PCI DSS for card processing).
- Stakeholder expectations — customer expectations of responsible AI, investor and rating-agency expectations, public and civil-society expectations in the jurisdictions the enterprise operates in.
- Technology environment — model-provider dependencies, cloud provider concentration, open-source ecosystem risks, adversarial-actor landscape.

**Internal issues** — items inside the enterprise that shape the AIMS.

- Governance structures — presence or absence of a board AI committee, existing ISMS, ERM programme, model-risk-management function, ethics function, legal-and-compliance operating model.
- Talent and competence — availability of AI-risk engineers, evaluation engineers, and governance analysts; competence gaps that the AIMS support plan (Clause 7) will have to close.
- Portfolio characteristics — number and criticality of AI systems in production, mix of first-party vs. third-party AI, presence of GPAI-provider vs. GPAI-deployer postures, exposure to consequential decisions (employment, credit, healthcare, biometrics).
- Cultural factors — history of governance maturity, tolerance for centralised control, existing incident-management culture.

The Clause 4.1 output is a *documented determination*. In practice this is a two-page register maintained by the head of AI governance and refreshed on the same cadence as the enterprise risk universe (typically annual, with event-driven updates when major issues change). The architect designs the register's *shape*, not its content:

```yaml
issue_id: ISS-EXT-004
scope: external
category: regulatory
title: EU AI Act — high-risk Annex III systems in EU-27 markets
description: >
  The enterprise ships two products the EU AI Act classifies as
  high-risk under Annex III (employment decision support; credit
  scoring assistance). Provider obligations under Chapter III
  Section 2 attach; deployer obligations attach in the EU-27
  markets where enterprise deploys directly.
relevance_to_aims: >
  Drives Annex A control inclusion for risk management (Article 9),
  data governance (Article 10), technical documentation (Article 11),
  logging (Article 12), transparency (Article 13), human oversight
  (Article 14), accuracy / robustness / cybersecurity (Article 15).
  Feeds registration and post-market monitoring obligations
  (Articles 71, 72, 73).
owner_role: head-of-ai-governance
last_reviewed: 2026-06-30
review_cadence: annual + event-driven
```

The auditor will trace *forward* from this register into Clause 6 planning; every external issue that is material should demonstrably feed at least one identified risk or one Annex A applicability decision. That trace is what distinguishes a live Clause 4.1 register from a decorative one.

## Clause 4.2 — interested parties and their requirements

Clause 4.2 asks the enterprise to identify the interested parties relevant to the AIMS and the requirements of those interested parties that are relevant. This is not "list every stakeholder in the universe" — it is "list the interested parties whose requirements the AIMS is *expected to meet*, and for each, name those requirements."

The interested-parties register for an AI enterprise typically includes:

- **Users and non-user affected persons** of the AI systems in scope. Requirements: transparency of AI use, ability to contest consequential decisions, protection from foreseeable harms.
- **Customers** (in a B2B enterprise: the deploying organisations; in a B2C enterprise: end consumers). Requirements: documented AI performance and limitations; contractual assurance of governance maturity; response to incidents.
- **Regulators** — sector supervisors (FCA, OCC, PRA, FDA, EMA, EEOC), horizontal AI regulators (EU market-surveillance authorities under the AI Act, US federal agencies operating under EO 14179 and OMB memoranda, state attorneys general and consumer-protection agencies), data-protection authorities. Requirements: registration and notification duties; documented compliance; incident reporting; audit cooperation.
- **Employees** who develop, deploy, or operate AI systems. Requirements: clarity on responsibilities; training and competence; protection from being required to act against professional judgement.
- **Business partners** — third-party model and platform providers, downstream distributors, embedded-AI integrators. Requirements: contractual clarity on provider / deployer / distributor roles; shared incident-response protocols; evidence-of-conformance exchange.
- **Investors and lenders**. Requirements: risk transparency; alignment with responsible-AI expectations (increasingly a fund-mandate concern).
- **Standards bodies and consortia** the enterprise participates in (ISO/IEC JTC 1/SC 42, IEEE, NIST public workshops, MLCommons). Requirements: participation commitments; evidence contribution.
- **Broader society and civil-society organisations** in the jurisdictions the enterprise operates in. Requirements: public transparency reports; incident disclosure; engagement with civil society on high-impact use cases.

The Clause 4.2 register shape:

```yaml
party_id: IP-REG-002
category: regulator
name: EU market surveillance authorities (per member state)
territorial_scope: EU-27
relevant_requirements:
  - EU AI Act Article 21 (cooperation with competent authorities)
  - EU AI Act Article 72 (post-market monitoring reporting)
  - EU AI Act Article 73 (serious-incident reporting)
  - EU AI Act Article 61 (registration in the EU database via Article 71)
aims_touchpoints:
  - Annex A control on incident notification
  - Annex A control on regulator cooperation
  - Documented-information set (Clause 7.5) — retained for regulator inspection
review_cadence: annual + on EU AI Act implementing/delegated act publication
```

The auditor will test that the interested-parties register is used by the risk process (Clause 6.1) — the risk criteria should recognise the interested parties whose concerns are material, and the risk-treatment decisions should demonstrably consider their requirements. A register that never gets referenced is a decorative artefact.

## Clause 4.3 — the scope statement

Clause 4.3 is the load-bearing wall. It asks the enterprise to determine the boundaries and applicability of the AIMS to establish its scope, and to document the scope as *documented information*. The auditor will read this document first, use it to bound their sampling, and hold every later artefact accountable to it.

**Five things the scope statement must name.**

1. **The AI systems in scope.** Not vaguely ("all AI at the enterprise") but precisely — by system class, by product family, by lifecycle stage. The 23053 reference architecture (below) supplies the nouns.
2. **The organisational units in scope.** By legal entity, by division, by function. Which business units are covered; which — if any — are explicitly excluded and why.
3. **The geographies in scope.** Which markets or jurisdictions the AIMS's operations cover. This is not the same as the applicability of specific obligations; it is the AIMS's own operational footprint.
4. **The lifecycle stages in scope.** Design / development / evaluation / deployment / operation / decommissioning. If the enterprise procures models and does not train them, the *training* stage may be out of scope in a specific-role sense; the auditor still expects the interfaces to third-party training to be documented.
5. **The explicit exclusions and their justification.** If R&D prototypes are out of scope, say so and justify why. If a specific legal entity acquired recently is out of scope pending integration, say so and give the integration date. Unstated exclusions become audit findings.

**A worked example — a European medtech enterprise with a US subsidiary.**

```markdown
## Scope of the AI Management System

The AI Management System of Northbrook Medical Group covers the
design, development, evaluation, deployment, operation, and
decommissioning of AI systems as defined in ISO/IEC 22989:2022,
where the systems are those referenced in the reference
architecture of ISO/IEC 23053:2022, subject to the specific
inclusions and exclusions below.

### In scope — AI systems

- Northbrook's regulated medical-device AI products, comprising:
  - The `CardioAssist` clinical-decision-support product family
    (device classification EU Class IIa; FDA 510(k)-cleared).
  - The `ImagingReview` diagnostic-imaging-support product family
    (device classification EU Class IIb; FDA 510(k)-cleared).
- Northbrook's non-regulated commercial AI products, comprising:
  - The customer-facing generative assistant used for patient-
    education content generation (non-device).
  - The internal retrieval-augmented generation (RAG) assistant
    used by clinical operations for literature review (non-device).
- Northbrook's internal enterprise AI systems, comprising:
  - The fraud-detection classifier fleet operated by finance.
  - The candidate-screening assistant used by talent acquisition.

### In scope — organisational units

- Northbrook Medical Group parent (Netherlands B.V.).
- Northbrook Medical Devices GmbH (Germany).
- Northbrook Medical US Inc. (Delaware C-corp) — device business
  only. Non-device US operations are covered by the US regional
  business unit AIMS extension (see Clause 4.3 exclusion below).

### In scope — geographies

EU-27, EEA (Norway, Iceland, Liechtenstein), Switzerland, UK,
United States. Systems deployed in Canada, Brazil, and Australia
are subject to the AIMS's design and evaluation controls but not
its jurisdictional-registration controls, which are handled by
each country's local regulatory affairs function.

### In scope — lifecycle stages

Design, development, evaluation, deployment, operation, and
decommissioning of the systems above. Where third parties
undertake any of these stages on Northbrook's behalf, the AIMS
covers Northbrook's role in specifying, supervising, and
accepting the third-party deliverable (see mod-109 for the
third-party governance shape).

### Explicit exclusions

- Non-device US commercial operations of Northbrook Medical US
  Inc. beyond the two systems named above. These operations
  are covered by the parallel US regional AIMS extension,
  planned for integration into this AIMS in the FY2027 audit
  cycle. Justification: separate acquisition (2025-11-01),
  integration in progress, sub-scope AIMS is being run in the
  interim to preserve control coverage.
- Research prototypes not yet advanced to a development-track
  pipeline. Justification: pre-development activity does not
  meet the AIMS's threshold of "AI system" per the 22989
  definition applied at the concept-of-operations stage. Any
  prototype advancing to a development-track pipeline is
  brought into scope by the tollgate defined in the
  development-lifecycle standard.
- Non-AI statistical models (e.g. traditional actuarial models
  in the underwriting function). Justification: outside the
  22989 definition of AI system.
```

Notice what this scope statement does:

- It anchors the definition of "AI system" in ISO/IEC 22989 and the *reference architecture* in ISO/IEC 23053, so the certification body reads it against a known frame.
- It enumerates systems by product family with device-classification annotations where relevant, so the auditor can sample specifically.
- It names organisational units by legal entity so the auditor can trace to the corporate structure.
- It names lifecycle stages so the auditor knows where the operational controls (Clause 8) attach.
- It marks exclusions explicitly with a stated justification so unstated exclusions do not become findings.

Notice what it does *not* do:

- It does not say "all AI at Northbrook" — a phrase that survives no audit.
- It does not exclude entire jurisdictions without a stated integration plan.
- It does not attempt to enumerate every individual model — an enumeration that would need updating monthly. It names *product families* whose contents are enumerated in the SSP-tier artefacts (mod-108) the AIMS references.

The scope statement should be one to three pages. Anything shorter is under-specified; anything longer is doing work that belongs in the SSP tier.

## The role of ISO/IEC 23053 — the reference architecture the scope is written against

The scope statement above uses phrases like "AI systems as defined in ISO/IEC 22989:2022, where the systems are those referenced in the reference architecture of ISO/IEC 23053:2022". That reference to 23053 is doing work. This section explains what work.

**What ISO/IEC 23053:2022 is.** *Framework for artificial intelligence (AI) systems using machine learning (ML)*. Published jointly with ISO/IEC 22989:2022, it supplies a reference architecture — a component-and-relationship model — for ML-based AI systems. Its components include (in the vocabulary the standard uses): data acquisition and preparation pipelines, model training pipelines, model registries, model deployment interfaces, inference services, monitoring and feedback loops. Its relationships describe how data flows through the pipeline, how the trained model is registered and deployed, and how post-deployment monitoring feeds back into training. (Paywalled ISO/IEC deliverable; consult the published standard for the authoritative component list.) <!-- needs-research: confirm the specific component names and relationship diagrams in the published ISO/IEC 23053:2022. -->

**Why it matters to the scope statement.** The scope statement has to name the systems and lifecycle stages the AIMS covers. Two enterprises using the same words can mean different things by "AI system" and "deployment" if they are not anchored in a shared reference architecture. 23053 supplies that shared reference architecture. When the auditor reads "the AIMS covers the design, development, evaluation, deployment, operation, and decommissioning of AI systems referenced in the reference architecture of ISO/IEC 23053:2022", the auditor knows precisely which components and relationships are on the table.

**How the AIMS uses 23053 downstream.** 23053 vocabulary flows into the SoA (Annex A controls that reference "training pipeline" or "inference service" use 23053 nouns), into the operational-control clauses (Clause 8 controls on change-management for AI systems reference 23053 components), and into the internal audit programme (auditors sample by 23053 component, not by ad-hoc labels). The architect's discipline is to import 23053 vocabulary *once* — in the scope statement and the AIMS glossary — and then use it consistently across every downstream artefact.

**Where 23053 does not fit — non-ML AI.** 23053 is explicitly framed for ML-based AI. If the enterprise's portfolio includes non-ML AI systems (symbolic reasoners, rule engines that fall inside 22989's definition of AI, expert systems), 23053's reference architecture does not fully apply. The scope statement should acknowledge this — either by declaring that only ML-based AI systems (per 23053) are in scope, or by extending the reference architecture with a documented enterprise addendum for the non-ML systems. In practice, for most enterprises reading this module, the portfolio is overwhelmingly ML-based and this is a minor caveat.

## Clause 4.4 — the management system

Clause 4.4 is short and often overlooked. It requires the organisation to *establish, implement, maintain, and continually improve* the AIMS, including the processes needed and their interactions. In the audit, this clause is where the auditor checks whether the AIMS is *actually running* — not whether the artefacts exist, but whether the artefacts are current, whether the processes are followed, whether the interactions between processes work. If the scope statement dates from 2024 and it is now 2026, Clause 4.4 has failed. If the internal audit programme exists on paper but has not run a round in 18 months, Clause 4.4 has failed. If the risk-treatment plan and the SoA are inconsistent, Clause 4.4 has failed.

The architect designs the *interactions* between processes explicitly. The classic pattern is a *process interaction diagram* that shows: risk process → risk-treatment plan → SoA → operational controls → performance evaluation → management review → change to risk criteria → back to risk process. Chapter `06-risk-treatment-plan-and-operational-clauses.md` and chapter `08-performance-evaluation-internal-audit-and-management-review.md` will detail the specific arrows; the point at Clause 4.4 is that the interactions are *designed*, not accidental.

## Two failure modes in Clause 4

**Failure mode 1 — the "all AI" scope statement.** The enterprise wants to look comprehensive so the scope statement says "the AIMS covers all AI activity at the enterprise". This fails the first stage-1 audit. What is an "AI activity"? Where does it stop? Is the enterprise's use of a spam filter in scope? A finance function's Excel regression? A vendor's embedded AI feature the enterprise switched on but does not know about? Without a definition anchor, the scope is un-sample-able. The 23053 anchor plus the 22989 definition is the way out.

**Failure mode 2 — the "compliance-window" scope statement.** The enterprise scopes the AIMS to *only* the systems that trigger EU AI Act Article 6 high-risk obligations, because those are the systems with the most-visible compliance pressure. Two years later a non-high-risk system causes a serious incident and the enterprise has no AIMS coverage for it. The scope statement should be driven by the enterprise's *governance strategy*, not by the immediate compliance calendar. A defensible scope covers all AI systems that make consequential decisions, are deployed to significant user populations, or handle regulated data — regardless of whether the specific regulatory regime attaches this quarter.

## Summary

Clause 4 is the load-bearing wall of the AIMS. Clause 4.1 documents the external and internal issues the AIMS addresses; Clause 4.2 documents the interested parties and their requirements; Clause 4.3 documents the *scope statement*, the anchor artefact for the entire management system; Clause 4.4 commits that the AIMS exists as a set of interacting processes and is maintained over time. The scope statement is written against the ISO/IEC 23053 reference architecture so the certification body reads it against known vocabulary; systems are named by product family with lifecycle stages, organisational units, geographies, and explicit exclusions with justification. The "all AI" scope statement fails first-stage audit; the "compliance-window" scope statement fails within two years. Anchor the scope in 23053 and 22989, keep it to one-to-three pages, and design the process interactions the Clause 4.4 audit will look for. The next chapter walks Clauses 5 (leadership) and 6.1 (planning) — the top-management commitment and the shape of the risk-and-opportunity process the scope statement bounds.
