# The standards-community contribution posture — where the enterprise participates and why

## Why this chapter exists

The mod-101 chapter on standards positioned the level-50 architect against the standards landscape as a *reader* — the standards are architectural inputs the architect composes into the control library, the AIMS, the assurance architecture. This chapter positions the architect as a *contributor* — a participant in the community bodies whose outputs are those standards, whose deliberations shape the next revision, and whose positioning the enterprise's own interests depend on.

The failure mode this chapter designs against is the one where the enterprise reads standards but does not participate in their authoring. The next revision of ISO/IEC 42005 tightens the impact-assessment shape in a way that misfits the enterprise's implementation and adds material re-implementation cost; the next CEN-CENELEC JTC 21 harmonised standard for the EU AI Act writes the risk-management shape in a way that the enterprise did not anticipate; the next IEEE 7000-series document names a compliance discipline the enterprise had not folded in. In each case, the enterprise had no representative in the room and no visibility into the drafting timeline. By the time the standard is published, the enterprise's options are re-implement, non-conform, or delegate to external counsel to argue non-applicability. Each option is expensive.

The architectural correction is a **contribution posture** — a first-class artefact that names which bodies the enterprise participates in, at what commitment level, through which seats, on which topics, and with what internal ratification path for the enterprise positions its representatives carry. The posture is council-ratified (chapter 01) so that the enterprise speaks with one voice, and it is head-of-AI-governance-owned at the working level (chapter 08) so that regulator-adjacent representations are coordinated with the regulator-engagement stream.

This chapter authors the posture framework and walks the community bodies most relevant to the senior AI governance architect.

## What "contribution" means at community-body scale

Standards-community contribution runs at four commitment levels, and the enterprise's posture selects a level per body. The framework separates the observer-passive shape from the active-shape-and-influence-drafting shape so that the enterprise's investment matches its interest.

**Level 1 — observer.** The enterprise consumes the body's published outputs and monitors its work programme without participating in the drafting. Cost: low (subscription fees where required, occasional attendance at open sessions). Signal: passive; the enterprise does not appear in the record.

**Level 2 — member with attendance.** The enterprise's designated representative attends the body's working-group meetings, joins the mailing list, receives working-drafts under the body's confidentiality regime, and votes where votes are held. Cost: medium (representative's time — often 5–15% of a working seat, plus travel for in-person meetings). Signal: present; the enterprise's positions are recorded but not necessarily reflected in drafts.

**Level 3 — active contributor.** The enterprise's representative contributes text to drafts, participates in editorial groups, authors comments on working drafts, and takes an active role in the shaping of the standard. Cost: high (representative's time — 20–40% of a working seat, plus editorial-cycle commitments and travel). Signal: shape-influencing; the enterprise's positions reach the drafts.

**Level 4 — leadership.** The enterprise's representative chairs or co-chairs a working group, edits a document, or leads a project. Cost: very high (representative's time — 40%+ of a working seat, sustained commitment across several years). Signal: strong shape-influencing; the enterprise's positions strongly reach the drafts and the body's work-programme.

The posture selects the level per body. Most enterprises operate at level 2 for the bodies whose outputs materially affect them, level 3 for the bodies whose outputs are central, and consider level 4 only where the enterprise has both a strategic interest and a representative whose seniority and continuity fit the multi-year commitment. Level 1 is the default posture for bodies whose outputs the enterprise consumes but does not want to shape.

## The community bodies — the map

The following bodies are the ones most relevant to the senior AI governance architect. The mapping is not exhaustive; the posture framework applies to any body the enterprise adds to the map.

### ISO/IEC JTC 1/SC 42 — the AI committee

**What it is.** The ISO/IEC joint technical committee 1's sub-committee 42, chartered to produce international standards on AI. Its outputs include ISO/IEC 22989 (AI vocabulary), ISO/IEC 23053 (framework for AI systems using ML), ISO/IEC 42001 (AI management system), ISO/IEC 42005 (AI system impact assessment), ISO/IEC 42006 (AI-management-system audit-body requirements), ISO/IEC 23894 (AI risk management guidance), ISO/IEC TR 24028 (trustworthiness overview), and adjacent documents. The committee's structure has working groups on foundational topics, use cases, trustworthiness, computational approaches, and management-system standards.

**Why the level-50 architect participates.** SC 42's outputs are the substrate the level-50 architect's design work sits on. Every mod-105 AIMS decision inherits from 42001; every mod-102 control-family authorship composes with Annex A; every mod-108 evidence architecture composes with 42005 impact-assessment schemas. The architect who does not have visibility into SC 42's forward work programme is inherently reactive to the standards' evolution.

**How enterprises participate.** Participation is through the enterprise's national member body (ANSI in the US, BSI in the UK, DIN in Germany, JISC in Japan, and adjacent bodies elsewhere). The national body's technical advisory group forms the enterprise's participation channel; the enterprise nominates a representative to the TAG, and the TAG's positions form the national body's positions taken to SC 42.

**Posture positioning at the level-50 architect.** Level 2 (member with attendance via national TAG) is the baseline for enterprises with material AI-governance ambition. Level 3 (active contributor) is defensible where the enterprise's programme composes with several SC 42 documents and the architect has capacity for the commitment. Level 4 (leadership) is a small-set posture the enterprise adopts where it has strategic interest and a senior representative with multi-year continuity — typically the head-of-AI-governance or a delegated senior architect.

`<!-- needs-research: verify the current active working groups in ISO/IEC JTC 1/SC 42 at authoring date; the committee's structure is evolving and specific working-group titles change across cycles -->`

### CEN-CENELEC JTC 21 — European AI standardization

**What it is.** The joint technical committee 21 of CEN and CENELEC, chartered to develop European standards in the AI space. JTC 21 is the body producing the *harmonised standards* under the EU AI Act — the standards that, when published in the EU Official Journal, confer a *presumption of conformity* on products and systems that meet them. This makes JTC 21 uniquely important for enterprises whose EU AI Act obligations are material.

**Why the level-50 architect participates.** The harmonised standards will determine what "conformity" concretely looks like for the EU AI Act's high-risk system requirements (Articles 8–15) and adjacent obligations. Every mod-104 jurisdiction-reconciled control-set entry for the EU AI Act will need to compose with the harmonised standard once published; enterprises without visibility into the drafting risk building against an implementation shape that the harmonised standard subsequently contradicts.

**How enterprises participate.** Participation is through the enterprise's national member body (national standards bodies are the CEN/CENELEC members; enterprises join through the national mirror committee). Participation composition with SC 42 is significant because JTC 21's work programme includes both original European standards and adoption / adaptation of ISO standards from SC 42.

**Posture positioning at the level-50 architect.** Level 2 baseline for enterprises with material EU AI Act exposure. Level 3 (active contributor) where the enterprise's EU AI Act exposure is central and the architect has capacity. Level 4 rarely — the commitment is heavy and the seniority requirement is strict.

`<!-- needs-research: verify current JTC 21 work programme, its harmonised-standard drafting status, and the national mirror committees' access at authoring date; the programme is under active development and its structure and outputs are evolving -->`

### IEEE 7000-series and IEEE Standards Association AI work

**What it is.** The IEEE Standards Association's ethics-and-AI standards, including IEEE 7000-2021 (model process for addressing ethical concerns during system design), IEEE 7001 (transparency of autonomous systems), IEEE 7002 (data privacy process), IEEE 7003 (algorithmic bias considerations), and adjacent standards in the series. Beyond the 7000-series, IEEE SA carries broader AI-related standards work including IEEE 2857 (privacy engineering), IEEE 2830 (technical requirements for the analysis of trustworthy AI-based systems), and adjacent projects.

**Why the level-50 architect participates.** The IEEE 7000-series has become a reference frame for the ethics-and-transparency aspects of AI governance that the ISO/IEC 42001 family does not cover with the same depth. Enterprises composing an ethics-review discipline into the AI governance council's inputs (chapter 01) benefit from IEEE 7000-series composition. IEEE's process is more open than ISO's — participation is often possible directly rather than through national member bodies.

**Posture positioning at the level-50 architect.** Level 1 (observer) is the baseline for enterprises whose ethics-review discipline is composed from the ISO family and the OECD AI Principles. Level 2 (member) where the enterprise's ethics-review discipline meaningfully composes IEEE 7000-series. Level 3+ where the enterprise's product surface is directly composed against a specific IEEE standard (e.g., IEEE 7001 for autonomous-system transparency where autonomous systems are in the product portfolio).

`<!-- needs-research: verify current IEEE 7000-series status and adjacent IEEE SA AI work programme at authoring date; the series is being extended and specific standards are moving through revision cycles -->`

### NIST public workshops and the AI RMF public engagement

**What it is.** The US National Institute of Standards and Technology runs public engagement around the NIST AI RMF, the AI RMF Playbook, the Generative AI Profile (AI 600-1), and adjacent work. Engagement takes the shape of public workshops, request-for-information periods, and the AI Safety Institute's (NIST AISI) programme. The engagement is more open than ISO or CEN-CENELEC — enterprises attend workshops, respond to RFIs, and provide comment on drafts without membership requirements.

**Why the level-50 architect participates.** The NIST AI RMF is the level-50 architect's reference for the US risk-management shape (mod-105, mod-106). Enterprises operating in the US federal contracting space are exposed to NIST-authored guidance across procurement, and the AISI's evaluation-methodology work is directly relevant to the mod-107 assurance architecture and mod-108 evidence architecture.

**Posture positioning at the level-50 architect.** Level 1 (observer) is the baseline; the enterprise reads NIST outputs and monitors workshops. Level 2 (attend workshops and respond to RFIs) is the mid-level posture for enterprises with material US federal or state exposure. Level 3+ (active engagement on the AI RMF Playbook or AISI programme) where the enterprise has US-federal-contracting exposure or where the AISI evaluation methodology composes with the enterprise's own evaluation programme.

### Frontier Model Forum (FMF)

**What it is.** An industry organisation of frontier-model developers (Anthropic, Google, OpenAI, Microsoft, and members added over time) coordinating on safety practices, sharing information about frontier-model risks, and engaging policymakers. Membership is limited to frontier-model developers meeting defined criteria.

**Why the level-50 architect participates.** Most enterprises are *consumers* of frontier models, not developers, and FMF membership is not available to them. Consumer enterprises engage with FMF outputs (public reports, safety practices) as external inputs rather than as members. The engagement matters because FMF-published practices shape the third-party posture the enterprise's mod-109 third-party programme takes to frontier-model providers.

**Posture positioning at the level-50 architect (consumer enterprise).** Level 1 (observer) — the enterprise reads FMF outputs and monitors its work. There is no membership pathway; the observer posture is not a choice but the definition of the enterprise's relationship. Where the enterprise is itself a frontier-model developer, the posture question is different and belongs to the CAO's strategic remit (chapter 08), not the architect's alone.

### Partnership on AI (PAI)

**What it is.** A multi-stakeholder non-profit convening AI companies, civil society, academic, and adjacent stakeholders on AI policy and practice. Its work programme includes practices around AI safety, fairness, transparency, media integrity, and adjacent topics. Membership is open to organisations meeting defined criteria and is not limited to frontier-model developers.

**Why the level-50 architect participates.** PAI is one of the multi-stakeholder venues where enterprise AI-governance practice is shaped alongside civil society and academic input. Participation exposes the enterprise's practices to external challenge and lets the architect represent enterprise views in the multi-stakeholder frame.

**Posture positioning at the level-50 architect.** Level 2 (member) is a strong signal for enterprises with material public-facing AI activity. Level 3+ where the enterprise's programme composes with PAI-specific practices (e.g., media-integrity commitments, deployment-guideline authorship).

### OECD.AI and the OECD AI Principles

**What it is.** The OECD's AI Policy Observatory (OECD.AI), which hosts the OECD AI Principles adopted in 2019 (updated 2024), the OECD.AI Incidents Monitor, and adjacent policy work. OECD.AI's engagement runs through governmental delegations and stakeholder observation; direct enterprise membership is not the model, but observation-and-input pathways are open through national governments' AI advisory bodies.

**Why the level-50 architect participates.** The OECD AI Principles are one of the most-cited multi-jurisdiction reference frames (mod-104) and OECD.AI's incident monitor is a reference source (mod-106, mod-110). Engagement composes with the enterprise's own participation in national AI advisory bodies.

**Posture positioning at the level-50 architect.** Level 1 (observer) is the baseline; the enterprise reads OECD.AI outputs and cites the Principles in its own AIMS. Level 2+ where the enterprise participates in a national government's AI advisory body that feeds OECD.AI engagement — the posture is composed of the national participation plus the OECD-facing engagement.

### Adjacent bodies — jurisdictional and sector-specific

Several other bodies matter at the sector or jurisdiction level and the posture framework applies to them equally:

- **UK AI Safety Institute (UK AISI)** and the UK's AI Standards Hub — for enterprises with material UK exposure.
- **Singapore IMDA and the AI Verify Foundation** (mod-111 chapter 07) — for enterprises engaging with the Singapore Model AI Governance Framework or AI Verify.
- **National standards bodies for the enterprise's other jurisdictions** — for enterprises with material exposure to Canada, Australia, India, Korea, Brazil, China, and adjacent.
- **Sector-specific bodies** — banking industry associations, healthcare industry associations, insurance-industry associations that publish AI-governance guidance.

## The contribution-posture schematic — YAML shape

```yaml
standards_community_contribution_posture:
  id: STANDARDS-POSTURE-v1.0
  ratified_by: ai-governance-council (reserved matter per chapter 01)
  head_of_posture: head-of-ai-governance (level 60)
  drafter: senior-ai-governance-architect (level 50)

  bodies:
    - id: iso-iec-jtc1-sc42
      body_name: ISO/IEC JTC 1/SC 42 — AI
      posture_level: 2
      access: via national member body (ANSI/BSI/DIN/etc.)
      representative_seat: senior-ai-governance-architect (level 50)
      commitment_percent: 10-15% of seat time
      internal_ratification_path: enterprise-position-paper → ARB (technical) → council (reserved matter for material positions)
      current_working_group_focus: [ <working groups the representative attends> ]

    - id: cen-cenelec-jtc21
      body_name: CEN-CENELEC JTC 21 — European AI
      posture_level: 2  # elevate to 3 if EU AI Act exposure is central
      access: via national mirror committee at CEN/CENELEC member body
      representative_seat: senior-ai-governance-architect (level 50) or delegated senior second-line seat
      commitment_percent: 5-10% of seat time (level 2); 15-25% (level 3)
      internal_ratification_path: as for SC 42

    - id: ieee-7000-series
      body_name: IEEE Standards Association — 7000-series and adjacent AI work
      posture_level: 1  # elevate to 2 where ethics-review discipline meaningfully composes
      access: direct IEEE SA participation
      representative_seat: senior-ai-governance-architect (level 50)
      commitment_percent: 5% of seat time (level 1); 10-15% (level 2)

    - id: nist-ai-rmf
      body_name: NIST AI RMF public engagement + NIST AI Safety Institute
      posture_level: 2  # elevate to 3 for US-federal-contracting exposure
      access: public workshops + RFI responses + AISI programme
      representative_seat: senior-ai-governance-architect (level 50) + ai-evaluation-engineer (level 35, on AISI evaluation-methodology work)
      commitment_percent: 5-10% of seat time
      internal_ratification_path: enterprise-position-paper → council

    - id: frontier-model-forum
      body_name: Frontier Model Forum
      posture_level: 1  # observer (consumer enterprise); not a member-eligible relationship
      access: public outputs
      representative_seat: n/a
      commitment_percent: 0% (monitoring only)

    - id: partnership-on-ai
      body_name: Partnership on AI
      posture_level: 2  # elevate to 3 for public-facing AI activity
      access: membership open to organisations meeting defined criteria
      representative_seat: head-of-ai-governance (level 60) or delegated senior second-line seat
      commitment_percent: 5-10% of seat time

    - id: oecd-ai
      body_name: OECD.AI + OECD AI Principles
      posture_level: 1  # elevate via national-government AI advisory participation
      access: national-government engagement + OECD.AI observation
      representative_seat: head-of-ai-governance (level 60)
      commitment_percent: 2-5% of seat time

  cross_body_composition:
    - description: the enterprise's positions across bodies compose consistently
      example: the enterprise's SC 42 position on the 42005 revision aligns with its JTC 21 position on the harmonised-standard drafting
      owner: senior-ai-governance-architect (level 50) — cross-body position paper drafting

  invariants:
    - id: I1
      description: every body the enterprise participates in has a named posture level
      test: sample the bodies; find the posture level ratified in council minutes
    - id: I2
      description: every representative-carried position is council-ratified
      test: sample position papers; find the council minute references
    - id: I3
      description: cross-body positions compose consistently
      test: cross-check SC 42, JTC 21, and NIST positions on shared topics
    - id: I4
      description: the head-of-ai-governance owns the external face
      test: representative-facing external material carries the head's review
    - id: I5
      description: the posture is versioned and re-ratified periodically
      test: council minutes carry an annual posture-review agenda item
```

## The three failure modes to design against

**Failure mode 1 — the "we don't participate" reactive posture.** The enterprise consumes standards but does not participate, on the reasoning that participation is expensive and the enterprise is not a standards-setting organisation. The reasoning ignores that the enterprise's absence from the drafting means the standards will not reflect the enterprise's implementation shape; when the standards are published, re-implementation is expensive. The defence is the posture framework — enterprises with material exposure participate at level 2 minimum, and level 2's commitment is modest at 5–15% of a working seat.

**Failure mode 2 — the "committee tourism" over-participation.** The enterprise participates in many bodies at level 2 or level 3 without a strategic focus. The representative's time is dissipated; the enterprise's positions are shallow at every body. The defence is the strategic focus — the posture ratifies which bodies are level 3 and which are level 2, and the representative's time is allocated accordingly.

**Failure mode 3 — the "solo-representative-goes-native" drift.** The enterprise's representative to a body attends over years, forms strong professional relationships with the working group's other members, and gradually begins carrying positions that reflect the working group's consensus rather than the enterprise's ratified position. The failure is subtle and slow; it appears in cross-body inconsistency (the representative's SC 42 position drifts from the enterprise's JTC 21 position). The defence is invariants 2 and 4 — every position is council-ratified in advance and the head-of-AI-governance owns the external face, so the representative is carrying an authorised position, not authoring one.

## Coordination — the roles the posture interfaces with

- **AI governance council (chapter 01)** — ratifies the posture as a reserved matter and ratifies each material position the representatives carry.
- **Head-of-AI-governance (level 60)** — owns the external face; carries the posture to peer heads at other enterprises; coordinates with the CAO on strategic implications.
- **Chief-AI-officer (level 70)** — engages at level 4 (leadership) where the enterprise has strategic interest; carries positioning at senior policy fora.
- **General counsel** — reviews positions for regulatory-adjacent implications; consulted on privileged-content handling.
- **Chief communications officer** — carries external-communications composition (chapter 04) for the enterprise's participation.
- **The mod-101 standards-landscape architect module** — supplies the *reader* posture the *contributor* posture composes with.
- **The mod-104 multi-jurisdiction reconciliation** — supplies the jurisdictional exposure that positions the enterprise's material interests.

## Summary

The standards-community contribution posture is a first-class artefact that positions the enterprise as a contributor, not just a reader, of the standards its programme composes against. The four-level commitment framework — observer / member-with-attendance / active-contributor / leadership — matches the enterprise's investment to its interest and prevents both under-participation (reactive posture) and over-participation (committee tourism). The community-body map covers ISO/IEC JTC 1/SC 42 (the AI committee whose outputs 42001, 42005, 42006, 22989, 23053, 23894 substrate the level-50 architect's work), CEN-CENELEC JTC 21 (the harmonised-standards drafter for the EU AI Act), IEEE 7000-series (ethics-and-transparency composition), NIST public workshops and AISI programme, Frontier Model Forum (observer only for consumer enterprises), Partnership on AI, OECD.AI, and adjacent jurisdictional and sector-specific bodies. The posture selects a level per body, names the representative seat, states the commitment cost, and specifies the internal ratification path so that representatives carry council-ratified positions rather than authoring them at the meeting. Five invariants hold — every body has a posture level, every carried position is council-ratified, cross-body positions compose consistently, the head-of-AI-governance owns the external face, the posture is versioned and re-ratified annually — and three failure modes recur — reactive we-don't-participate, committee tourism, solo-representative drift. Exercise-07 in this module walks the architect's own contribution-plan drill using this framework. Chapter 08 walks the boundary between the level-50 architect who drafts the postures and the head-of-AI-governance and chief-AI-officer who carry them externally.
