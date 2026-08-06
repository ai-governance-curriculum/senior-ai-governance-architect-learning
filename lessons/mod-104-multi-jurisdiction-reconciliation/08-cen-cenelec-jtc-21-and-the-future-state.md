# CEN-CENELEC JTC 21 and the future state — presumption of conformity as the architecture target

## Why this chapter exists

The reconciliation architecture chapter 7 designed is *load-bearing*. It works. It scales. But it is expensive to operate — the obligation register requires ongoing curation, the applicability filter requires ongoing evaluation, the evidence contract requires per-obligation renderings, and internal audit tests the whole thing every cycle. The architecture would be substantially cheaper if the enterprise could point at a *single external artefact* — a harmonised standard — and say *conformance with this standard is presumption of conformity with the following bundle of obligations*.

That is exactly what the *presumption of conformity* mechanism in EU law provides, and it is exactly what CEN-CENELEC Joint Technical Committee 21 is producing for the EU AI Act. The JTC 21 harmonised-standards programme is the single most consequential *future-state architecture target* the level-50 architect can plan against. When the programme lands (progressive publication is under way; some standards are further along than others), an enterprise whose control library conforms to the JTC 21 standards can discharge a large fraction of its EU AI Act obligations by attestation to the standards rather than by per-obligation evidence. The programme also becomes a *shape source* for jurisdictions outside the EU that reference or align to the JTC 21 shape.

This chapter reads the JTC 21 programme as the future-state target, names the standards in the programme and their maturity as of writing, and connects the programme to the reconciliation architecture from chapter 7 so the enterprise can plan for the transition.

## What "presumption of conformity" means in EU law

An EU regulation like the EU AI Act imposes essential requirements — Articles 9-15 for high-risk AI systems are essential requirements. The Commission then mandates the European Standardisation Organisations (ESOs) — CEN, CENELEC, and ETSI — to develop *harmonised standards* that specify how to meet those essential requirements. When a harmonised standard is published with its reference in the *Official Journal of the European Union*, conformity with that standard creates a *rebuttable presumption* that the corresponding essential requirements are met.

**Why this matters architecturally.** Presumption of conformity does three things for the enterprise:

1. **It converts open-ended obligations into testable checklists.** *"Ensure appropriate accuracy, robustness, and cybersecurity"* (Article 15) is not directly testable. A harmonised standard on accuracy, robustness, and cybersecurity for AI systems is. Instead of arguing to a supervisory authority about what "appropriate" means, the enterprise attests to conformance with the standard.
2. **It changes the burden of proof.** Under presumption, the supervisory authority must rebut the presumption to challenge conformity — the enterprise does not have to prove conformity anew each time.
3. **It creates a stable audit trail.** Standards are versioned, dated, and pinned to essential-requirements references in the Official Journal. Evidence produced against a specific standard version has clear provenance.

The presumption is rebuttable, not absolute. If a supervisory authority finds that a harmonised standard does not fully cover an essential requirement, or that conforming to the standard does not in a specific case discharge the requirement, the presumption fails. Standards therefore do not remove risk; they concentrate it in a well-understood shape.

## The CEN-CENELEC JTC 21 programme

CEN and CENELEC established Joint Technical Committee 21 — *Artificial Intelligence* — in 2021. Its work programme, driven by the European Commission's Standardisation Request in support of the EU AI Act (originally issued in draft in 2022 and formally issued in 2023, with revisions to align with the final Regulation text), produces the harmonised standards that will supply presumption of conformity for the Act's essential requirements.

<!-- needs-research: verify the current Commission Standardisation Request in support of the EU AI Act (the M/ mandate identifier and the exact scope after the alignment revisions). The chapter's characterisation of the request as covering Articles 9-15 essential requirements plus AIMS is correct in substance but the specific mandate identifier and revision history should be pinned. -->

**The standards work items under JTC 21 include** (non-exhaustive; the programme is under active development):

- **AI risk management** — a standard aligned to Article 9 (risk-management system). Draws on ISO/IEC 23894 shape.
- **AI trustworthiness framework** — a standard aligned to the trustworthiness characteristics that cut across Articles 9-15.
- **Quality of AI systems and quality management for AI systems** — a standard aligned to Article 17 (quality management system) and drawing on ISO/IEC 42001 shape.
- **Bias mitigation** — a standard aligned to Article 10 (data and data governance) bias-provisions and to the general non-discrimination essential-requirement thread.
- **Trustworthiness characterisation of AI systems** — a standard on the characterisation and measurement of AI-system trustworthiness properties.
- **Cybersecurity of AI systems** — a standard aligned to Article 15 (cybersecurity of high-risk AI systems).
- **AI system logging** — a standard aligned to Article 12 (automatically-generated logs).
- **Conformity assessment of AI systems** — a standard on assessment methodology, addressing Annex VI internal-conformity-assessment shape.

<!-- needs-research: pin the current work-item list for JTC 21, the current publication or draft status of each item (pre-committee draft, committee draft, enquiry, formal vote, published, referenced in OJEU), and the specific reference numbers assigned. As of writing, some items are drafts and others have progressed to formal vote or publication. -->

**Adoption of ISO/IEC deliverables.** JTC 21 works closely with ISO/IEC JTC 1 SC 42 (Artificial Intelligence). Some JTC 21 standards are essentially adoption-with-modification of SC 42 deliverables (ISO/IEC 42001 for the AIMS shape; ISO/IEC 23894 for risk management; ISO/IEC 24029 for robustness; ISO/IEC TR 24028 for trustworthiness). Where an SC 42 standard aligns cleanly with the EU AI Act essential requirements, JTC 21 adopts it (often with an EN prefix — for example EN ISO/IEC 42001 as the European adoption of ISO/IEC 42001, with any EU-specific Annexes or modifications). Where the SC 42 shape does not fit the Act's requirements, JTC 21 produces a new EN standard.

**Why this matters for the reconciliation architecture.** An enterprise that has already invested in ISO/IEC 42001 (mod-105 AIMS) and in ISO/IEC 23894 (risk management) is *ahead* on JTC 21 conformance — the same investment discharges both the ISO/IEC standard and the harmonised standard when published. The choice of ISO/IEC 42001 as the enterprise AIMS is directly enabled by the JTC 21 adoption path.

## The transition path — obligations to standards

The reconciliation architecture from chapter 7 is designed to *absorb* the JTC 21 transition without rework. As each harmonised standard is published and its reference appears in the Official Journal:

1. **A new obligation record is created** representing conformance to the standard. `regime.identifier: eu_ai_act_hs`, `citation.standard: EN <number>:<year>`, `demand.category: process` (conformance is a process attested-to), `consequence.type: administrative_penalty` (through the presumption path), `effective_date: <OJEU-reference-publication-date>`.
2. **The new record's `supersedes` field points at the essential-requirement obligation records the standard covers.** For example, a published EN standard on risk management would carry `supersedes: [OBL-EUAI-Art9-risk-management]` — not because Article 9 disappears (it does not; the essential requirement remains in force), but because the presumption path *provides an alternative discharge route* for the same requirement. The reconciliation architecture models this as a *conditional supersession*: if the enterprise elects the standards route, the essential-requirement obligation's evidence contract is discharged by conformance evidence for the standard; if the enterprise elects the non-standards route, the essential-requirement obligation's evidence contract remains fully in force.
3. **The applicability filter on each affected control is updated.** For controls in scope of the harmonised standard, the filter grows an *evidence-mode* dimension: `evidence_mode in {standards_route, direct_evidence_route}`. Controls with `standards_route` produce standards-conformance evidence; controls with `direct_evidence_route` continue producing per-obligation evidence. The enterprise can carry both modes concurrently, per system.
4. **The evidence contract adds a standards-conformance rendering.** The rendering names the specific standard version, the conformance-attestation shape (self-declaration, third-party assessment, notified-body certification per the Act's conformity-assessment articles), and the retention of the attestation.

**Design note — do not delete pre-transition obligation records.** The reconciliation architecture retains the essential-requirement obligation records even after the harmonised standard is published. They remain the *authoritative source* for what the Act actually requires; the standard is a *conformance route*. This is why the deprecation-path from chapter 7 uses `supersedes` rather than `replaces` — supersession is a conditional relation, not an unconditional one.

## What the CEN-CENELEC path does not do

**It does not remove the addressee-role and applicability discipline.** The harmonised standards cover *provider* obligations under Articles 9-15 primarily; deployer obligations, Article 27 FRIA, Article 50 transparency, and Article 73 serious-incident reporting are less well-covered (or not covered) by the standards programme. The applicability filter's addressee-role dimension remains as important as before.

**It does not cover jurisdictions other than the EU.** JTC 21 is a European standardisation programme. Colorado does not recognise EN ISO/IEC 42001 as presumption of conformity with its algorithmic-discrimination duty. Korea's AI Basic Act does not defer to CEN-CENELEC. The reconciliation architecture continues to carry per-jurisdiction obligations for the rest of the international patchwork; the JTC 21 pathway simplifies the EU slice only.

**It does not eliminate the risk of rebuttal.** Presumption is rebuttable; a supervisory authority can find that conformance to the standard does not discharge the essential requirement in a specific case. The enterprise's evidence architecture must retain the ability to produce direct per-obligation evidence for those cases — the two evidence modes coexist rather than the standards mode fully replacing the direct-evidence mode.

**It does not eliminate the reason for the reconciliation architecture.** The obligation register, applicability filter, evidence contract, and deprecation path remain the enterprise's source of truth. What changes is that many essential-requirement records acquire a standards-conformance rendering, which is *cheaper to produce* than per-obligation evidence. The architecture is unchanged in shape.

## The parallel non-EU tracks

Several jurisdictions have their own standards or standards-like tracks the enterprise should also watch as future-state simplification opportunities:

- **ISO/IEC JTC 1 SC 42.** The mother lode. ISO/IEC 42001 (AIMS), ISO/IEC 23894 (risk management), ISO/IEC 24029 (robustness), ISO/IEC 42005 (AI system impact assessment), ISO/IEC 42006 (AIMS certification requirements). Not tied to any single regulator's presumption of conformity, but referenced by many. Chapter `08-delegation-to-ai-risk-engineer.md` of mod-102 already pointed at these.
- **NIST AI RMF and its overlays.** NIST is not a standards body in the CEN-CENELEC sense — it does not create presumption of conformity — but its outputs (AI RMF 1.0, AI 600-1 GenAI Profile, AI 800-1 sub-guides) are the *reference* framework OMB memoranda (chapter 4) and Colorado SB24-205 (chapter 5) reference as rebuttable-presumption anchors in their own frames.
- **UK AISI methodology.** For frontier-model evaluations, becoming a de facto reference in the UK AI-safety conversation.
- **Singapore AI Verify.** For testing methodology, becoming a de facto reference in Southeast Asian contexts.
- **China TC260.** For Chinese-market GenAI compliance, the reference the CAC security assessment is measured against.

**Architectural takeaway — invest in ISO/IEC 42001 and the SC 42 family first.** They are the anchor the JTC 21 EN standards adopt, they are the anchor NIST and Singapore reference, they are the anchor for enterprise AIMS in mod-105, and they are the enterprise's best forward-compatibility bet across the fewest independent programmes.

## What the level-50 architect should do this year

Concrete steps to position the enterprise for the JTC 21 transition without over-investing in a moving target:

1. **Publish the enterprise AIMS against ISO/IEC 42001** (mod-105 covers this). The EN-prefixed adoption will be a versioning update, not a rewrite.
2. **Pin the enterprise risk-management approach to ISO/IEC 23894**. Same forward-compatibility argument.
3. **Maintain a watch list on JTC 21 publications.** Quarterly review at minimum, faster during standards' formal-vote windows. For each near-published standard, pre-work the *evidence-mode extension* on the affected controls so the transition is filter update rather than control authoring.
4. **Do not switch to standards-route evidence prematurely.** A standard in draft or in enquiry is not the standard that will be published; the shape may change materially. Continue producing direct-evidence renderings for essential-requirement obligations until the standard is *referenced in the OJEU*.
5. **Reserve budget for the notified-body relationship** where applicable. Some high-risk AI systems (particularly those covered by Annex I harmonised legislation) require notified-body conformity assessment; the enterprise's relationships with notified bodies matter and are usually multi-quarter to establish.
6. **Read the Article 56 codes of practice** (chapter 2). For GPAI providers, the code-of-practice route is a parallel presumption-of-conformity path with its own dynamics; the reconciliation architecture must carry it alongside the JTC 21 standards path.

## Two failure modes

**Failure mode 1 — waiting for the standards before building the reconciliation architecture.** The enterprise defers the obligation register and the applicability filter until the standards land, on the reasoning that the standards will simplify the work. Two problems: (a) the Act applies before the standards are all published, so waiting means being out of compliance; (b) the standards work is not comprehensive — a portion of the essential requirements will never be covered by a JTC 21 standard — so the reconciliation architecture is needed regardless. Build the architecture now; extend it as standards land.

**Failure mode 2 — treating the standards as regulation.** The enterprise adopts EN ISO/IEC 42001 and reads it as *the* obligation, forgetting that the essential requirement in the Regulation remains the authoritative statement. When a supervisory authority rebuts the presumption in a specific case, the enterprise has no direct-evidence pathway. The fix is chapter 7's discipline — obligation records for essential requirements remain in force; standards-conformance is a *rendering* on the evidence contract, not a *replacement* for the underlying obligation record.

## Summary

CEN-CENELEC JTC 21 is producing the harmonised standards that will supply presumption of conformity for the EU AI Act's essential requirements. As standards are published and their references appear in the Official Journal, an enterprise whose control library conforms to them can discharge the corresponding essential requirements through standards-conformance evidence rather than through per-obligation evidence — cheaper to produce, cleaner to audit, more stable across regulatory-change cycles. The reconciliation architecture from chapter 7 absorbs the transition without rework: new obligation records represent the standards, they conditionally supersede the essential-requirement records they cover, and the evidence contract acquires a standards-conformance rendering alongside its direct-evidence rendering. The JTC 21 pathway does not eliminate the reconciliation architecture — it does not cover non-EU jurisdictions, does not cover deployer or Article 50 or Article 73 obligations well, does not remove the rebuttal risk. It does simplify the EU high-risk provider slice materially, and it aligns the enterprise's ISO/IEC 42001 and 23894 investments with the future European standards path. The architect's near-term job is to build the reconciliation architecture now (chapter 7), invest in the ISO/IEC anchors that JTC 21 adopts, maintain a formal watch on JTC 21 publication progress, and pre-work the evidence-mode extensions so each standard's publication is a filter update rather than a control-authoring sprint. Together with chapters 1–7, this closes the reconciliation-architecture module.
