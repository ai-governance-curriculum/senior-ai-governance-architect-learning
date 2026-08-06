# Cross-cutting international obligations

## Why this chapter exists

Some obligations do not attach to a *territory*. They attach to the *organisation* — because the organisation is subject to a treaty its state signed, because it operates in a member state of a rights-based convention, because it is a member of a group that publicly committed to a code, or because a horizontal law (GDPR being the archetype) captures its processing regardless of where the AI system is deployed. These cross-cutting obligations sit *underneath* the jurisdictional regimes and *interact* with them. They rarely change what an individual control looks like; they routinely change *why the control exists at all* and *what the enterprise's own principles-layer document (mod-103 chapter 01) commits to*.

The level-50 architect's job with cross-cutting obligations is different from the jurisdictional-regime job. It is not to invent new controls; it is to (a) ensure the enterprise's Responsible AI principles layer traces to the international consensus documents so the programme reads coherently to any regulator anywhere, (b) attach the right cross-cutting obligation records to existing controls so their crosswalks tell the full story, and (c) know which cross-cutting obligations are *soft* (aspirational, non-binding, reputational) versus *hard* (legally binding on the enterprise or its counterparties). This chapter reads the five that matter most.

## GDPR Article 22 — the automated decision-making floor

The General Data Protection Regulation (Regulation (EU) 2016/679) is often not filed under "AI regulation" — it predates the current wave — but Article 22 is one of the most consequential AI-facing legal provisions in force in the EU, and it applies *regardless* of whether the AI system is high-risk under the EU AI Act.

**What Article 22 says.** The data subject has the right not to be subject to a decision based *solely* on automated processing, including profiling, which produces legal effects concerning them or similarly significantly affects them. Exceptions: necessary for entering into or performance of a contract with the controller; authorised by Union or member-state law with suitable safeguards; based on the data subject's explicit consent. Where an exception applies, the controller must implement suitable measures to safeguard the data subject's rights, freedoms, and legitimate interests — at least the right to obtain human intervention, to express their point of view, and to contest the decision.

**Architectural takeaway.** Article 22 lands as an applicability-filter dimension: for any AI system whose outputs are *decisions* about identifiable natural persons in the EU/EEA, the *solely-automated* status is a filter attribute on the control that must be present. If a decision *is* solely automated, three controls must be in force:

1. A control that verifies the legal basis (contract necessity, authorising law, or explicit consent) is documented per case.
2. A control that provides human-intervention, expression-of-view, and contestation channels with a stated SLA.
3. A control that produces "meaningful information about the logic involved" (Articles 13(2)(f), 14(2)(g), 15(1)(h)) — the transparency artefact for the affected person.

All three are commonly shape A on top of existing decision-system governance controls, with GDPR Article 22 added as a crosswalk edge. Where the shape-A extension bites is on control (3) — most enterprise transparency artefacts predate the *individual-level* rendering GDPR asks for and need explicit content extensions.

**Where Article 22 overlaps the EU AI Act.** The EU AI Act's Article 14 (human oversight) is closely related to Article 22's human-intervention requirement, and the enterprise control commonly discharges both — the applicability filter names both regimes, the evidence contract satisfies the strictest, and the crosswalk carries both edges. But the two are *not* the same obligation: Article 14 is a design-time obligation on the *provider*; Article 22 is a real-time obligation on the *controller* (which in AI-system terms is closest to the deployer) toward the *data subject*. A single control cannot discharge both without an addressee attribute that carries both.

## Council of Europe Framework Convention on Artificial Intelligence

The Council of Europe Framework Convention on Artificial Intelligence and Human Rights, Democracy and the Rule of Law was opened for signature on 5 September 2024 in Vilnius. It is the first international treaty on AI. Signatories include Council of Europe member states and non-members that participated in negotiation (the United States, the United Kingdom, Israel, and others). Not all signatories have ratified as of writing.

**What it does.** Establishes obligations of parties (signatory states, not organisations) to ensure activities within the lifecycle of AI systems are consistent with the protection of human rights, the rule of law, and democratic principles. Parties commit to a set of general obligations (accountability, equality and non-discrimination, protection of privacy and personal data, transparency and oversight, safe innovation) and a set of procedural safeguards (documentation and information, redress). The obligations attach to activities of *public authorities* and *private actors acting on their behalf*; obligations toward *private actors' own activities* are addressed by parties applying the convention "as appropriate" through domestic regulation.

**Architectural takeaway.** The Framework Convention is (a) *aspirational* for most enterprise activities in most signatory states, because it binds the state to legislate rather than binding the enterprise directly, and (b) *substantive* where the enterprise is the *supplier* to a public authority (the state's obligations flow through the procurement contract, and the enterprise as vendor inherits them). The reconciliation architecture treats the Convention as:

1. An anchor source in the enterprise's Responsible AI principles layer (mod-103 chapter 01), so the principle-set explicitly references rights-based framing.
2. A crosswalk edge — not a driver — on public-sector-deployment applicability filters. When the enterprise sells into a Council of Europe signatory state's public sector, the procurement contract typically flows Convention-derived requirements down; those requirements route to existing controls and add the Convention as a crosswalk edge.
3. A *watch-list source* for the deprecation path (chapter `07-designing-the-reconciliation-architecture.md`) — as domestic implementation legislation follows in signatory states, the enterprise obligation register must catch it.

Do *not* create standalone controls keyed to Convention articles. The obligations flow through domestic law implementation and through procurement contracts. Standalone controls will drift.

## OECD AI Principles

Adopted by the OECD Council in May 2019, updated in May 2024. Five values-based principles (inclusive growth, sustainable development, and well-being; human rights and democratic values including fairness and privacy; transparency and explainability; robustness, security, and safety; accountability) and five recommendations to governments (investing in AI research and development; fostering an inclusive AI-enabling ecosystem; shaping an interoperable governance and policy environment for trustworthy AI; building human capacity and preparing for labour-market transformation; international co-operation). Endorsed by 47 jurisdictions and the G20. Non-binding.

**Architectural takeaway.** OECD principles are the single most influential *shape source* for enterprise Responsible AI principles layers. Almost every published AI principle set in the industry — Google, Microsoft, IBM, Apple, most large financial-services and healthcare enterprises — traces to OECD framing. The level-50 architect uses OECD as:

1. The primary anchor source in the mod-103 chapter 01 principles layer. Each enterprise principle should map to at least one OECD value-based principle so the traceability is legible to a regulator familiar with the OECD frame.
2. A common vocabulary in the reconciliation architecture. When two regimes disagree on wording, the OECD framing is the compromise formulation that lets a single enterprise principle survive both.
3. A soft-law anchor with no direct control-level entries. OECD principles do not appear on individual controls' crosswalks; they appear on the principles-layer document and on the library preface.

## UNESCO Recommendation on the Ethics of Artificial Intelligence

Adopted by the UNESCO General Conference on 24 November 2021. All 193 UNESCO member states adopted it. Broader than OECD — includes values (respect, protection and promotion of human rights and fundamental freedoms and human dignity; environment and ecosystem flourishing; ensuring diversity and inclusiveness; living in peaceful, just and interconnected societies), principles (proportionality and do no harm; safety and security; fairness and non-discrimination; sustainability; right to privacy and data protection; human oversight and determination; transparency and explainability; responsibility and accountability; awareness and literacy; multi-stakeholder and adaptive governance and collaboration), and policy-action areas (impact assessment, governance and stewardship, data policy, development and international cooperation, environment and ecosystems, gender, culture, education and research, communication and information, economy and labour, health and social well-being).

UNESCO also publishes a Readiness Assessment Methodology (RAM) and an Ethical Impact Assessment (EIA) tool that some member states adopt as domestic implementation shape.

**Architectural takeaway.** UNESCO is the *widest* soft-law anchor available and is particularly consequential for enterprises with operations in the Global South, where UNESCO framing is often the primary AI-policy vocabulary the regulator is fluent in. Use UNESCO in the reconciliation architecture in two ways:

1. As a *secondary* anchor source in the principles layer, alongside OECD and NIST AI RMF trustworthy characteristics. The mod-103 chapter 01 worked example already lists UNESCO explicitly.
2. As an *ethical-impact-assessment* shape reference for jurisdictions that adopt EIA as domestic implementation (a handful of African, Latin American, and Southeast Asian jurisdictions are moving in this direction). Where domestic implementation adopts EIA, the enterprise's FRIA (mod-104 chapter 2 Article 27) and DPIA controls should carry an EIA rendering as one of their acceptable evidence shapes.

## G7 Hiroshima Process Code of Conduct

The Hiroshima Process International Code of Conduct for Organizations Developing Advanced AI Systems was published in October 2023 alongside the International Guiding Principles for Organizations Developing Advanced AI Systems, following the May 2023 G7 Leaders' Summit in Hiroshima. Eleven guiding principles, and a code of conduct that operationalises them. Voluntary; addressed to organisations, not to states. Signatories include most major foundation-model developers.

**What it commits organisations to.** Eleven commitments spanning risk identification and mitigation across the lifecycle, external red-teaming, transparency and reporting, information sharing, security investment, watermarking / provenance mechanisms, prioritising research on societal risks, developing to address global challenges, technical standards adoption, data-input controls including personal-data and IP, and international engagement.

**Architectural takeaway.** Hiroshima is the most operational of the soft-law commitments — it reads as if the drafters expected an enterprise to be able to point at an existing control against each commitment. For a GPAI-provider enterprise that is a signatory:

1. Each Hiroshima commitment should map to an existing GPAI-family control (`AIC-GPAI-*`) in the enterprise library. If any commitment cannot be mapped, the *gap* is the more interesting artefact than the mapping.
2. Hiroshima is a *crosswalk edge*, not a driver. It does not typically justify new controls; it justifies reviewing existing controls for coverage.
3. Public commitments create *reputational* consequence even without legal penalty. The obligation record for a Hiroshima commitment should carry consequence-type `reputational` (chapter `07-designing-the-reconciliation-architecture.md`) so risk appetite (mod-106) treats it visibly.

The Hiroshima Code is now referenced in EU AI Act Article 56 codes-of-practice discussions and by the OECD as a possible model — the reconciliation architecture should treat it as a *forward-looking* source whose obligations may migrate from soft to hard over the next several years.

## How cross-cutting obligations show up in the applicability filter and evidence contract

These five sources do not usually create *new applicability-filter dimensions*. They rarely create *new evidence-contract fields*. What they do is:

- **Enrich crosswalks.** Every control's crosswalk field grows to include the cross-cutting source that also bears on it. An adversarial-robustness control (mod-102 chapter 03) may end up crosswalked to NIST AI RMF MEASURE 2.7, EU AI Act Article 15, ISO/IEC 42001 Annex A, OECD principle on robustness/security/safety, UNESCO principle on safety and security, Hiroshima commitment on risk identification, and Council of Europe Convention safe-innovation obligation. Seven crosswalk edges, one control. That is *good* — it shows the enterprise's single practice discharges seven overlapping expectations.

- **Justify the principles-layer document.** The enterprise's principles document names OECD, UNESCO, and the NIST AI RMF trustworthy characteristics as anchor sources. A principles document that omits OECD reads as parochial to any European or OECD-familiar regulator. A principles document that omits UNESCO reads as parochial in most of the Global South.

- **Create *watch-list* entries for the deprecation path.** The Council of Europe Framework Convention will spawn domestic implementation legislation in signatory states over the next several years. The reconciliation architecture must catch each implementation as it lands (chapter `07-designing-the-reconciliation-architecture.md` names the watch-list mechanism).

- **Carry *reputational* consequence.** The Hiroshima commitments carry no legal penalty, but breach carries reputational exposure — a signatory that visibly fails to red-team its frontier model will be called out publicly. The consequence field on the obligation record must not default to `none` for soft-law commitments; use `reputational`, and let mod-106's risk-appetite bands handle it.

## Two common failure modes

**Failure mode 1 — treating soft law as noise.** The enterprise skips OECD, UNESCO, and Hiroshima because they carry no direct penalty. Two problems: (a) the principles-layer document reads as home-cooked and does not compose cleanly with regulator-familiar vocabulary; (b) the enterprise is caught out when a signatory state passes domestic legislation implementing a soft-law source, because it had no watch-list entry to trigger the review. The fix is to file soft law as *low-consequence-but-tracked* rather than untracked.

**Failure mode 2 — treating soft law as controls.** The enterprise creates one control per OECD principle, one control per UNESCO principle, one control per Hiroshima commitment. Catalogue bloat, duplicated evidence, drift within a quarter. The fix is the shape-A discipline: soft-law sources are crosswalk edges on existing controls; they do not spawn controls of their own except in the rare case where a soft-law commitment demands a practice the enterprise does not otherwise have.

## Summary

Cross-cutting international obligations are the layer beneath the jurisdictional regimes. GDPR Article 22 is the exception that proves the rule — it is *hard* law that applies to any solely-automated decision about an EU data subject and creates real applicability-filter and evidence-contract work. The other four (Council of Europe Framework Convention, OECD AI Principles, UNESCO Recommendation, G7 Hiroshima Code of Conduct) are largely soft law today; they anchor the enterprise's principles-layer document, enrich crosswalks on existing controls, populate the deprecation-path watch-list for future domestic implementation, and carry reputational consequence rather than legal penalty. Getting them into the reconciliation architecture explicitly (and with the right consequence type) is what lets the enterprise absorb them without creating parallel programmes — and what lets the principles layer read as internationally literate rather than home-cooked.
