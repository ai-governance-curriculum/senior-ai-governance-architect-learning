# Reading the ISO/IEC 42001 family and its vocabulary layer

## Why this chapter exists

Where NIST AI RMF gives you *outcome statements* to design against, the ISO/IEC 42001 family gives you *management-system machinery* to write *inside*. That is a different genre of standard, with a different set of expectations for the architect. A management-system standard tells you what the *system* must look like — its scope, its documented information, its internal audit cadence, its statement of applicability — not what individual controls must say. If you read ISO/IEC 42001 the way you read NIST AI RMF, you will treat Annex A controls as the deliverable and miss that the *management system itself* is the deliverable.

This chapter walks the family — the AIMS standard (ISO/IEC 42001), the impact-assessment standard (42005), the certification-body standard (42006), the risk-management guidance (23894), the AI-quality model (25059), the AI-governance-implications standard (38507) — and the vocabulary layer (22989 / 23053 / TR 24028) — with the emphasis on how each shows up in your artefacts, not on paraphrasing their tables of contents.

## Family map, in one picture

The AI-specific management-system standards divide into three tiers:

**Tier 1 — the operating standards you *write against***

- **ISO/IEC 42001:2023** — Artificial intelligence management system (AIMS). Requirements standard. Annex A lists AI-specific control objectives and controls; Annex B is implementation guidance; Annexes C and D relate risk sources and application domains.
- **ISO/IEC 42005** — AI system impact assessment. Process standard for how impact assessments are structured and produced (aligned to the EU AI Act's fundamental-rights impact assessment obligation).
- **ISO/IEC 42006** — Requirements for bodies providing audit and certification of AI management systems. This is the standard your third-party auditor is audited *against*. If you know what 42006 requires of the auditor, you know what a defensible audit trail looks like from the auditee side.

**Tier 2 — the reasoning + quality standards you *reference***

- **ISO/IEC 23894:2023** — AI risk management guidance. Applies ISO 31000 to the AI context. Not certifiable; a guidance standard the AIMS's risk-management process can cite.
- **ISO/IEC 25059** — Quality model for AI systems, extending the ISO/IEC 25010 SQuaRE product-quality model with AI-specific characteristics.
- **ISO/IEC 38507:2022** — Governance implications of the use of AI by organizations. Board- and executive-scope standard; its language is what CAO / audit committee will use.

**Tier 3 — the vocabulary layer that lets the others talk to each other**

- **ISO/IEC 22989:2022** — AI concepts and terminology. Definitions your control library, taxonomy, and policy statements all cite.
- **ISO/IEC 23053:2022** — Framework for AI systems using ML. A reference architecture for ML systems: components, roles, data flows. Your AIMS scope statement writes against this frame.
- **ISO/IEC TR 24028:2020** — Overview of trustworthiness in AI. Older, more surveyable; useful for early-program vocabulary alignment.

If your enterprise already has an ISO/IEC 27001 ISMS, you already have organisation-wide muscle memory for Tier 1 standards. Reuse it.

## Read 42001 as shape, not as content

The single biggest mistake reading 42001 for the first time is starting at Annex A and treating each Annex-A control as the deliverable. Do the opposite: read Clauses 4–10 first — those are the *shape* of the management system.

- Clause 4 — **Context of the organization**: scope statement, needs and expectations of interested parties. Your architectural deliverable here is the AIMS scope document (mod-105).
- Clause 5 — **Leadership**: AI policy, roles / responsibilities / authorities. Your deliverable is the AI policy hierarchy and the RACI (mod-103, mod-112).
- Clause 6 — **Planning**: risks and opportunities, AI objectives, planning of changes. Your deliverable is the risk taxonomy and appetite (mod-106) plus the risk-treatment plan.
- Clause 7 — **Support**: resources, competence, awareness, communication, documented information. Your deliverable is the competence + communications architecture and the documented-information (evidence) schema (mod-108, mod-112).
- Clause 8 — **Operation**: operational planning and control, AI impact assessment, AI system life cycle, third-party. Your deliverable is the AIMS operations layer, the impact-assessment schema (per 42005), and the third-party programme (mod-109).
- Clause 9 — **Performance evaluation**: monitoring, internal audit, management review. Your deliverable is the assurance architecture (mod-107) plus the monitoring architecture (mod-110).
- Clause 10 — **Improvement**: non-conformity, corrective action, continual improvement.

Now Annex A becomes readable in its actual role: a *control objective catalog* the AIMS's statement of applicability picks from. You justify inclusions and exclusions on the SoA. That is architecture. The Annex A controls themselves are inputs into your enterprise AI control library (mod-102), not the library.

## The Statement of Applicability (SoA) is the architect's deliverable

If you take away one artefact from ISO/IEC 42001, take away the SoA. It is the document where you list every Annex A control and either:

- **Include** it — cite the enterprise control(s) that implement it; cite the evidence contract; note applicability filters (e.g. only for high-risk systems).
- **Exclude** it — justify the exclusion.

The SoA is the single artefact an ISO/IEC 42006-conformant auditor will spend the most time on. If your SoA is defensible, your certification is largely defensible. If it is not — if inclusions are unbacked, exclusions are unjustified, or applicability filters are hand-waved — you fail on a governance-shape gap, not on a technical gap.

Sketch of a SoA row:

```yaml
control_id: A.6.2.4   # ISO/IEC 42001 Annex A control ID
title: AI system impact assessment
included: true
justification: >
  All tier-1 and tier-2 AI systems require an impact assessment per
  Clause 8; the process is defined in ENT-PROC-IA-01 aligned to
  ISO/IEC 42005.
implementing_controls:
  - AIC-IA-011  # enterprise impact-assessment schema control
  - AIC-IA-012  # impact-assessment intake gate
applicability_filter:
  system_tier: [tier-1, tier-2]
evidence_contract:
  - artifact: impact_assessment_v3.md
    owner: ai-governance-analyst
    retention_years: 10
crosswalk:
  eu_ai_act: [Article 27]  # FRIA
  nist_ai_rmf: [MAP-1.1, MAP-1.5, MAP-3.1]
```

You will build many of these in mod-105 (AIMS architecture). Understanding here in mod-101 that the SoA *is* the architectural deliverable saves you from arguing later about what Annex A is for.

## ISO/IEC 42005 — impact assessment as a process, not a template

ISO/IEC 42005 defines *how* an AI impact assessment is scoped, planned, executed, documented, reviewed, and updated. It does not give you the fill-in-the-blank template. Your architectural deliverable is the enterprise's impact-assessment schema — the fields, the applicability triggers, the evidence links, the review cadence — that conforms to the 42005 process. The analyst at level 15 fills the schema in for a given system. The 42005 process describes what "filling it in" ought to look like.

Two implications:

- **Do not conflate impact assessment with risk assessment.** Impact-assessment is scoped to affected parties, harms, and mitigations for a particular *system* deployed in a particular *context*. Risk assessment (per ISO/IEC 23894) is the AIMS's ongoing risk-management activity. They inform each other but are separate artefacts.
- **The EU AI Act's Article 27 fundamental-rights impact assessment (FRIA)** should map cleanly to your 42005-aligned schema *as an applicability filter*, not as a fork. If it forks, mod-104 will spend energy stitching them back together.

## ISO/IEC 42006 — read the auditor's rulebook to design a defensible audit trail

You are not going to be audited *against* 42006 — the auditor is. But 42006 tells you what an auditor must do to remain accredited, and reading it turns "prepare for audit" into a specific, artefact-driven exercise.

The two most useful things 42006 tells you:

- The evidence they will look for is *documented information* — the AIMS's own Clause 7.5 documented-information set. If a control claims to exist but has no documented-information trail, an auditor cannot certify it.
- Their audit is competence-gated. A 42006-conformant certification body must field auditors with AI competence *and* management-systems competence. In practice, that means your walkthroughs will be with people who read Annex A the same way you do. Communicate at that level, not at compliance-checklist level.

Design the AIMS to be *auditable by the rules the auditor plays by* and you avoid the two most common audit failures: undocumented controls and management-review meetings that produced no traceable output.

## ISO/IEC 23894 — how AI risk management plugs into ISO 31000

ISO/IEC 23894 is guidance, not requirements — it does not add a new certifiable programme. What it does is give you a *bridge* between ISO 31000's generic risk-management process (context, risk identification, analysis, evaluation, treatment, monitoring, communication) and AI-specific risk sources.

Architecturally, 23894 is where you anchor mod-106's risk taxonomy. The taxonomy itself will draw from NIST AI RMF's Generative AI Profile categories, MIT AI Risk Repository, AIRO, and AIID / OECD.AI incident classes; 23894 gives you the process shell those categories live inside.

If your enterprise already runs ISO 31000 for enterprise risk, 23894 lets you make the AI-risk process a *specialisation* of that process rather than a parallel programme. That is the deliverable in mod-105 (AIMS integration with existing management systems).

## 25059, 38507, and the vocabulary layer — what they buy you

- **ISO/IEC 25059** is a quality model — reliability, robustness, transparency, controllability, and other characteristics for AI systems as extensions of SQuaRE. Useful in mod-102 (control library) and mod-108 (evidence architecture) because model / system cards can align to a shared quality taxonomy.
- **ISO/IEC 38507** is written for boards. Its value to the architect is that *it fixes the vocabulary your executive audience uses*. When you brief the CAO / audit committee, using 38507 language makes the brief legible to them.
- **ISO/IEC 22989 / 23053 / TR 24028** are the vocabulary and reference-architecture layer. Every control statement, policy statement, and taxonomy node in your library should use 22989 definitions where 22989 has defined them. Where the term is not in 22989, define it locally *and* cite that the definition is local. This discipline is what makes cross-jurisdiction reconciliation (mod-104) survive.

## Reading order for a first pass

If you are new to this family, read in this order:

1. **ISO/IEC 22989** — vocabulary first; everything else uses these terms.
2. **ISO/IEC 42001** clauses 4–10 — shape of the management system.
3. **ISO/IEC 42001** Annex A — the control-objective catalog.
4. **ISO/IEC 23894** — how the AI risk process plugs in.
5. **ISO/IEC 42005** — impact-assessment process.
6. **ISO/IEC 42006** — what the auditor must do.
7. **ISO/IEC 25059**, then **ISO/IEC 38507**, then **TR 24028** — quality, governance-implications, and older-vocabulary layers, in that order.

Exercise-03 has you build a shape-read of 42001, 42005, and 42006 you can rely on for the rest of the track.

## Summary

The ISO/IEC 42001 family is management-system machinery. Read 42001 for its Clause 4–10 shape first; treat Annex A as a control-objective catalog whose inclusions and exclusions the SoA justifies. Design the impact-assessment schema against 42005; design the audit trail against what a 42006-conformant auditor will look for. Anchor the AIMS's risk process in 23894 as a specialisation of ISO 31000. Adopt 22989 vocabulary throughout so mod-104's cross-jurisdiction reconciliation survives. Take 38507 into board briefings and 25059 into model / system card schemas.
