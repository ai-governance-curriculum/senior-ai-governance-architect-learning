# Sibling frameworks the AI architecture plugs into

## Why this chapter exists

If your enterprise is not brand-new, it already has management systems for information security (ISO/IEC 27001 ISMS or NIST-based equivalent), enterprise risk management (ISO 31000 or COSO ERM), IT governance (COBIT 2019), and — if it is a systems-engineering shop — a values-in-design practice inspired by the IEEE 7000-series. The AI governance architecture is *not* delivered on a blank slate; it has to plug into these siblings without either re-inventing them or being swallowed by them.

Get the plug-in wrong and you produce either (a) an AI programme that runs in parallel to the ISMS and duplicates work forever, or (b) an AI programme that gets absorbed into infosec and stops being visibly *AI* governance. Both are architectural failures — solvable now, expensive later.

This chapter positions each of the sibling frameworks: what it owns, what it does not, and where the AI governance architecture plugs in. Read it as a *seams* discussion, not as a survey of the standards themselves.

## ISO/IEC 27001 (ISMS) — the sibling you likely already have

If your enterprise is ISO-mature at all, it almost certainly runs an ISO/IEC 27001 Information Security Management System. That is the sibling you plug into most heavily.

**What 27001 already covers:** confidentiality / integrity / availability of information; risk-treatment for information security risks; documented information; internal audit; management review; access, cryptography, logging, incident, business continuity, supplier relationships.

**What 27001 does *not* cover:** AI-specific risks (bias, opacity, autonomy, misuse, dual-use), AI-specific quality dimensions (fairness, explainability, robustness under distribution shift), AI-specific supply chain (foundation models, training data, red-team artefacts), AI-specific impact (fundamental-rights, environmental, societal).

**Plug-in shape:** the ISO/IEC 42001 AIMS is designed to *reuse* the ISO/IEC 27001 management-system machinery (Clauses 4–10 have the same shape). In an org with a mature ISMS you should:

- **Share** the scope-and-context, documented-information, competence, communication, internal-audit, management-review, and continual-improvement machinery. Do not fork.
- **Extend** the risk-treatment plan and the statement of applicability to include AI-specific controls (ISO/IEC 42001 Annex A on top of ISO/IEC 27001 Annex A).
- **Add** AI-specific processes 27001 does not carry: impact assessment (via ISO/IEC 42005), AI-specific supplier / third-party programme (mod-109), AI-specific incident and post-market surveillance (mod-110).

The AIMS scope statement will say — in plain language — "the AIMS integrates with the ISMS as follows." Mod-105 is where you write it.

## ISO 31000 (enterprise risk management)

**What 31000 owns:** the generic risk-management process — establishing context, risk identification, analysis, evaluation, treatment, monitoring, communication and consultation. It is a *guidance* standard (not certifiable), providing the vocabulary and process shell most enterprise risk functions run against.

**What 31000 does not own:** the specifics of AI risk (harm categories, capability tiers, dependency mapping, appetite calibration, aggregation). Those are what ISO/IEC 23894 fills in — as an *AI specialisation* of the 31000 process, not a fork of it.

**Plug-in shape:** the enterprise AI risk-management process is a *specialisation* of the enterprise's 31000 process. The AI risk register is *part of* the enterprise risk universe, not a separate universe. When the AI programme raises a risk to a level that would trigger enterprise-scope escalation, that escalation runs on 31000 lines — because that is how the enterprise is wired for board escalation already.

Mod-106 makes this concrete: the AI risk taxonomy plugs into the enterprise risk taxonomy; the AI appetite plugs into the enterprise appetite framework; the AI risk register is a *view* into the enterprise register with an AI-specific schema.

## ISO/IEC 27005 (information security risk management)

**What 27005 owns:** the 31000 process specialised for information security, in support of 27001. Guidance on risk identification, analysis, evaluation, treatment, and monitoring for information-security risks specifically.

**Plug-in shape:** where AI risks overlap with information-security risks (data confidentiality of training data, model IP protection, guardrail bypass, supply-chain integrity of model artefacts), 27005 is the sibling. Your AI risk-management process does not fork *information-security* risk-management; it extends it. The AIMS and ISMS share the same risk register for those overlapping risks — with different tagging, different treatment owners, different appetite thresholds if warranted.

The practical warning: some AI risks *look like* infosec risks but are not — dialect discrimination is not confidentiality, fairness is not integrity, human-AI role confusion is not availability. Treating them as infosec risks under 27005 alone misclassifies them and prevents the AI risk taxonomy from doing its job. Mod-106 draws the line explicitly.

## COSO ERM (Enterprise Risk Management — Integrating with Strategy and Performance)

**What COSO ERM owns:** the risk-management framework most US-headquartered enterprises use for enterprise-level risk. Five components (governance and culture; strategy and objective-setting; performance; review and revision; information, communication, and reporting) and twenty principles. It is the framework the board / audit committee is most likely reading against.

**Plug-in shape:** where 31000 is European-flavoured, COSO ERM is US-flavoured. Many enterprises run one *effectively* even if the other is nominally cited. You need to know which one the enterprise treats as authoritative and align the AI risk architecture to it — because when the board asks "how does the AI programme fit into ERM?" you need to give an answer in *their* framework's vocabulary.

Two hooks are architecturally important:

- **Strategy and objective-setting**: the AI risk architecture should be able to explain how AI programmes fit the enterprise's strategy — this is where CAO / CFO / board interface.
- **Information, communication, and reporting**: the AI programme's board-facing reporting cadence must plug into ERM reporting, not run separately.

Neither hook is a design deliverable for this role. But knowing what the head of governance and the CAO must feed into is essential for your architecture to be *reportable*.

## COBIT 2019 (IT governance framework)

**What COBIT 2019 owns:** governance and management objectives for enterprise IT — the framework CIO / CTO-office governance most often runs on. Distinguishes governance objectives (Evaluate, Direct, Monitor — EDM) from management objectives (Align, Plan, Organize — APO; Build, Acquire, Implement — BAI; Deliver, Service, Support — DSS; Monitor, Evaluate, Assess — MEA).

**Plug-in shape:** where the AI programme runs on top of IT that is COBIT-governed, the AI governance council is *not a replacement* for COBIT governance objectives. It is a *specialisation* — AI-specific decisions escalate to COBIT-governed IT governance when infrastructure, cost, or major architectural change is implicated.

Mod-112 (operating model) formalises the seams: which decisions stay in the AI governance council, which escalate to the IT governance body, which escalate to enterprise risk. In mod-101 the awareness is enough: do not build an AI council that violates COBIT-shaped IT governance the CIO's org already runs.

## IEEE 7000-series (values-in-design)

**What the 7000-series owns:** the IEEE 7000 model process for addressing ethical concerns during system design, plus a family of related standards (7001 transparency, 7002 data privacy process, 7003 algorithmic bias considerations, 7005 employer data governance, 7007 ontological standard for ethically driven robotics, 7010 well-being metrics, and others). It is a *systems-engineering-side* view of AI ethics, complementary to the management-systems view of 42001 and the risk-management view of 23894.

**Plug-in shape:** IEEE 7000 shows up two places:

- **In the policy taxonomy** (mod-103): principles / policies about values-in-design cite IEEE 7000 as the reference process.
- **In the impact-assessment schema** (mod-108 evidence architecture): the assessment can lean on IEEE 7000 for the values-elicitation step.

Where the enterprise has systems-engineering muscle memory (aerospace, medical devices, defence), 7000-series alignment reads as native and buys programme legitimacy with engineering leadership. Where the enterprise does not, 7000 is often mentioned in policy citations but rarely operationalised — do not force it.

## Sibling frameworks as *audiences*, not just *inputs*

There is a subtle but important second use of these frameworks. They are also *audiences*: your control library is read by, and must be legible to:

- The CISO's team (via 27001 / 27005 language)
- The CRO / audit-committee (via 31000 / COSO ERM language)
- The CIO / CTO office (via COBIT 2019 language)
- Engineering leadership (via IEEE 7000-series where present)
- The audit committee and board (via the highest-abstraction language of the org — often COSO ERM + CSF 2.0 GOVERN)

If your control library uses only NIST AI RMF and ISO/IEC 42001 vocabulary, it will read as "AI-team-only" to those audiences. Annotating each control with the sibling framework(s) it composes against is what makes the library organisationally legible.

## The three-way check for plug-in

Before you commit an AI-specific process, control, or artefact, run the three-way check:

1. **Does an existing sibling framework already carry this?** If yes, extend that framework's artefact; do not fork.
2. **Is the AI specialisation truly needed, or is the risk already covered generically?** If already covered, save the AI-specific entry for the AI-specific residual.
3. **Which sibling framework is the audience for this artefact?** Annotate accordingly.

Do this reliably and the AI architecture disappears into the enterprise's operating fabric — legible from every existing seat — instead of becoming an island the CISO's org and the CFO's org each treat as someone else's problem.

## Summary

The AI governance architecture plugs into ISO/IEC 27001 as an AIMS-alongside-ISMS extension, into ISO 31000 (or COSO ERM) as a specialisation of enterprise risk management, into ISO/IEC 27005 for overlapping information-security risk, into COBIT 2019 as an AI-specific specialisation of IT governance, and into IEEE 7000-series as a values-in-design reference where systems engineering is central. In each seam, extend before you fork; annotate control entries with sibling-framework audience language so the library is legible to CISO / CRO / CIO / audit-committee audiences without a translator. Get the seams right in mod-101 and every later module — AIMS integration in mod-105, risk taxonomy in mod-106, GRC platform in mod-111, operating model in mod-112 — will land without an escalation.
