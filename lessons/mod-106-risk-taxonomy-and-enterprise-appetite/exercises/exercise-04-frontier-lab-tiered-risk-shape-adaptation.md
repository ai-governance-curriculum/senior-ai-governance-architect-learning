# exercise-04: Frontier-Lab Tiered-Risk Shape Adaptation

**Estimated effort:** 3 hours

## Objective

Produce the **audit-committee-facing adaptation memo** that answers, in explicit, defensible detail, the question the audit committee will ask on first briefing of your enterprise's tiered-risk architecture: *"why not just adopt what Anthropic / OpenAI / DeepMind do?"* The deliverable exercises chapter 05's six-adopt / four-adapt / one-decline structure against a *specific* frontier-lab framework applied to *your specific* enterprise scenario, and lands in a concrete pair of enterprise-scope artefacts (a capability-tier definition and a threshold-crossing decision packet) that show the adaptation is not talk.

The memo is not a literature review of the frontier-lab framework you chose. It is the enterprise architect's design decision on which architectural moves import cleanly, which import only with substantive re-scoping, and which do not import at all — with the paired enterprise artefacts that prove the import is real.

## Prerequisites

- Chapter [`05-frontier-lab-tiered-risk-frameworks-as-shape-templates.md`](../05-frontier-lab-tiered-risk-frameworks-as-shape-templates.md) — the six adopt-moves, the four adapt-moves, the one decline-move, and the failure mode of naïve transplantation.
- Chapter [`02-harm-categories-capability-tiers-and-the-dependency-map.md`](../02-harm-categories-capability-tiers-and-the-dependency-map.md) — the four-attribute capability-tier axis (modality access × action authority × data blast radius × autonomy horizon) that your enterprise-scope adaptation of the frontier-lab capability-threshold move will bind to.
- Chapter [`03-appetite-translation-from-board-statement-to-bench-triggers.md`](../03-appetite-translation-from-board-statement-to-bench-triggers.md) — the tolerance table, escalation triggers, and stop-shipping thresholds that the frontier-lab pre-registration and rollback moves import into.
- Exercise-01's taxonomy and exercise-02's tolerance table as the substrate you will bind the adaptation to. If you did not do the earlier exercises, use the chapter fragments as your working versions.
- One frontier-lab framework read at architectural depth — enough to name its pre-registered thresholds, its evaluation cadence, its deployment / security level pairing, and its rollback commitments. See [`../resources.md`](../resources.md) for the current published versions of Anthropic RSP, OpenAI Preparedness, and Google DeepMind FSF.

## Scenario

You continue as the level-50 architect at your chosen enterprise from exercise-01. The audit committee has read the recent public reporting on frontier-lab safety frameworks and has asked the head of AI governance to explain, at the next committee meeting, why the enterprise is not "just adopting" one of them. The head of AI governance has asked you to produce the adaptation memo that will drive her ninety-second answer to the committee and the supporting artefacts the committee's technology or risk sub-committee can read at depth.

Choose **one** frontier-lab framework as the source for your adaptation:

- **Anthropic Responsible Scaling Policy (RSP)** — AI Safety Level (ASL) tiers, Deployment Standard / Security Standard pairing, pre-registered evaluations and pause commitments.
- **OpenAI Preparedness Framework** — tracked risk categories with per-category risk levels (the framework's own labels have evolved across versions), Safety Advisory Group review, per-level deployment gates.
- **Google DeepMind Frontier Safety Framework (FSF)** — Critical Capability Levels (CCLs), early-warning evaluations, response actions per CCL, governance review.

State your choice at the top of the memo and pin the framework version you are reading (frameworks revise; the adaptation must be against a specific published version).

## Deliverables

Author three artefacts in a working directory of your choice:

1. **`adaptation-memo.md`** — the audit-committee-facing memo answering the "why not just adopt?" question, with the ninety-second briefing answer at the top.
2. **`enterprise-capability-tier-spec-v1.0.0.yaml`** — the enterprise-scope capability-tier specification that adapts (not copies) the frontier-lab framework's capability-threshold move onto your enterprise's actual system-deployment attribute surface.
3. **`threshold-crossing-decision-packet.md`** — a worked scenario in which a specific system in your enterprise scenario approaches an enterprise-tier threshold, the evaluations named for that threshold are run, and the pre-registered decision (release, restrict, pause, rollback) is invoked. This is the artefact that proves the adaptation has teeth.

## Requirements

### `adaptation-memo.md`

Structure:

- **Ninety-second briefing answer** at the very top: the three-paragraph version of chapter 05's "what to bring to the audit committee" section, tailored to your chosen framework and your scenario. This is the paragraph the head of AI governance will read at the meeting; every other section of the memo backs it up.
- **Framework read at architectural altitude.** One page on your chosen framework's architectural moves: pre-registered thresholds, evaluation gates, deployment / security level pairing, rollback commitments, external accountability, versioning. Cite the specific published version.
- **The six moves the enterprise adopts.** For each of chapter 05's six adopt-moves (pre-registration, named-in-advance evaluations, deployment-and-security pairing, rollback commitments, explicit versioning, internal review body with defined authority), walk how your enterprise architecture (exercise-01 taxonomy, exercise-02 tolerance table, exercise-03 aggregation model) imports it. Where an adopt-move requires an artefact your enterprise does not yet have, name the artefact and the delivery date.
- **The four moves the enterprise adapts.** For each of chapter 05's four adapt-moves (capability-threshold definition, eval set, external accountability, safety-adjacent research programme), walk the enterprise-scope re-scoping. The capability-threshold adaptation is the most consequential and gets a full page — the enterprise-scope capability-tier spec (deliverable 2) is the concrete artefact of that adaptation and is referenced from here.
- **The move the enterprise does not adopt.** State whether the training-pause move applies to your enterprise scenario. Most enterprises decline it because they do not train the underlying models. Where the enterprise *does* fine-tune (bank on domain-specific fine-tunes; healthcare payer on utilisation-management specialisations; SaaS on customer-specific personalisations), name the scoped equivalent and its trigger conditions.
- **The failure mode of naïve transplantation.** State explicitly what you *did not* do: you did not adopt ASL-1/2/3/4 (or low/medium/high/critical) labels for your enterprise systems, because ASL-N is a property of a foundation model, not of an enterprise deployment. One paragraph on this; the audit committee reads it as a hedge against a specific mistake other enterprises have made.
- **What the enterprise inherits from its model providers.** Where your enterprise deploys foundation models produced by labs that publish tiered frameworks, the model provider's mitigation commitments become an *input* into the enterprise's evidence architecture (mod-108 preview). Enumerate two or three specific commitments (e.g., a model-provider ASL-3 Deployment Standard commitment on tool-use restriction) that flow into your enterprise's control library or evidence packet.

### `enterprise-capability-tier-spec-v1.0.0.yaml`

Adapt (not copy) the frontier-lab capability-threshold move. Populate a specification with:

- **Tier definitions.** 4 or 5 enterprise capability tiers. Each tier defined against the four attributes from chapter 02: `modality_access`, `action_authority`, `data_blast_radius`, `autonomy_horizon`. Do *not* re-use ASL / OpenAI-risk-level / CCL labels; use tier numbers or enterprise-scope names.
- **Tier-crossing evaluations.** For each tier boundary (tier-1→2, tier-2→3, etc.), the *system-level* evaluations that determine whether a system crosses into the higher tier. Name the evaluation, the pass criterion, the responsible role (risk engineer at level 25 for scoring, evaluation engineer at peer level 30 for calibration). Evaluations are named in advance per chapter 05's adopt-move-2.
- **Tier-crossing deployment and security controls.** For each tier, the deployment-side controls (from mod-102's `AIC-HOV-*` / `AIC-TRP-*` families) and the security-side controls (from `AIC-SEC-*` / `AIC-ROB-*`) required at the tier. Deployment and security are enumerated separately per chapter 05's adopt-move-3.
- **Foundation-model inheritance stanza.** For each tier, whether the enterprise inherits any mitigation from the underlying foundation model's provider framework, and which specific commitment (e.g., "at tier-4, if the underlying model is an ASL-3 model, the provider's Deployment Standard on tool-use restriction is a required input to the tier-4 control set"). Where no inheritance applies, state so explicitly.
- **Pre-registered rollback triggers.** For each tier, the conditions under which a system already deployed at that tier is rolled back to a lower tier or removed from service. This is the enterprise-scope import of chapter 05's adopt-move-4 (rollback commitments).
- **Governance-body assignment.** For each tier-crossing decision, the internal body that ratifies (AI risk committee for tier-1↔2 in most enterprises; model risk committee for tier-2↔3; head of AI governance plus audit committee for tier-3↔4). This is chapter 05's adopt-move-6 (internal review body with defined authority).
- **Version stamp.** Per chapter 07 discipline: taxonomy version the spec binds to, effective-from date, ratification record.

### `threshold-crossing-decision-packet.md`

A worked scenario, roughly 2–3 pages, in which one of the systems from your exercise-01 scenario (or an invented system consistent with the scenario) approaches an enterprise-tier boundary and the pre-registered machinery runs. The packet demonstrates that the adaptation is operational, not aspirational.

Must contain:

- **System profile.** The system: what it does, its current tier assignment, why the tier is being reconsidered (new capability planned, new deployment context, new data access).
- **The proposed tier crossing.** From tier-N to tier-N+1, with the specific attribute change that drives it.
- **The evaluations run.** The named evaluations from the tier-crossing evaluations section of your spec, with the (invented, marked-as-illustrative) results. At least one evaluation should surface a concern that requires deliberation rather than a clean pass.
- **The deployment and security controls proposed at the new tier.** Referenced from the spec; any deviations flagged.
- **The foundation-model inheritance check.** What the model provider's framework commits to at the model's tier, and how that flows into the enterprise decision.
- **The decision.** Release at the new tier; release at the new tier with additional conditions; refuse the crossing; queue for redesign. The decision cites the pre-registered thresholds, not a case-by-case judgement.
- **The rollback triggers now in effect.** If released at the new tier, the specific conditions under which the system is rolled back.
- **The record.** Who ratified; on what date; what artefacts are attached; what the mod-108 evidence architecture will hold for future audit reference.

## Starter guidance

- **Choose your framework by fit, not familiarity.** If your enterprise scenario is B2B SaaS with tool-calling agent capabilities, DeepMind's CCLs on autonomy and ML R&D adapt more naturally than OpenAI's persuasion tier. If your scenario is a bank with mostly narrow classifiers plus a customer-facing GenAI chat, the OpenAI Preparedness Framework's cyber and CBRN categories map poorly and the RSP's deployment-standard shape imports more cleanly. Do not choose the framework you find easiest to summarise; choose the one whose shape most instructs your enterprise's actual problems.
- **The ninety-second answer is the artefact that survives the meeting.** Draft it first; if the answer is not clear in three paragraphs, the memo is not clear.
- **Do not import the tier labels.** Chapter 05 is emphatic on this. Naming your enterprise tiers ASL-1/2/3/4 or low/medium/high/critical imports the frontier-lab framework's implicit semantics that your enterprise cannot deliver on. Use tier numbers or enterprise-scope names; the four-attribute definitions from chapter 02 do the work.
- **The threshold-crossing packet is where teeth show.** A packet in which every evaluation passes cleanly and the decision is "release at the new tier" is a packet that has not exercised the machinery. Design the scenario so one evaluation surfaces a real question, one control is not yet met, or one rollback trigger is invoked because a similar system's deployment produced a concerning signal. The audit committee reads the packet for evidence that the framework runs under pressure, not that it runs in the sunshine.
- **The training-pause section is short for most enterprises.** One paragraph, correctly declining the move, is defensible. Where the enterprise *does* fine-tune, one page on the scoped equivalent is defensible. Do not fabricate a fine-tuning programme the enterprise does not run just to keep the section symmetrical.
- **Foundation-model inheritance is a real architectural feature; treat it that way.** If the enterprise deploys models from providers who publish frameworks, the provider's commitments *are* part of the enterprise's mitigation stack. Enumerate them; do not paraphrase; cite the specific commitment version.

## Acceptance criteria

- [ ] The chosen framework is stated with its specific published version.
- [ ] The ninety-second briefing answer is on the first page and fits in three paragraphs.
- [ ] All six adopt-moves are walked with a specific artefact reference from your exercise-01, exercise-02, or exercise-03 output.
- [ ] All four adapt-moves are walked with the enterprise-scope re-scoping named concretely (not abstractly).
- [ ] The training-pause section states explicitly whether the enterprise adopts, adapts, or declines the move, with a defence.
- [ ] The naïve-transplantation failure mode is called out and the specific mistake (importing tier labels) is disclaimed.
- [ ] The capability-tier spec has 4 or 5 tiers with all four chapter-02 attributes populated per tier.
- [ ] Tier-crossing evaluations are named in advance per tier boundary, with pass criteria and responsible role.
- [ ] Deployment-side controls and security-side controls are enumerated separately per tier, referencing specific mod-102 control families.
- [ ] Foundation-model inheritance is stated per tier, including tiers where no inheritance applies.
- [ ] The threshold-crossing packet exercises the machinery — at least one evaluation surfaces a concern; the decision cites pre-registered thresholds, not case-by-case judgement.
- [ ] The packet names the ratifying body and the mod-108 evidence artefacts that will hold the record.
- [ ] No enterprise tier is labelled ASL-N, "low/medium/high/critical" borrowed from a frontier-lab framework, or CCL-N. Tier labels are enterprise-scope.

## Stretch goals

- **Comparator table.** Add a comparator table across all three frontier-lab frameworks (RSP, Preparedness, FSF) with a row per architectural move and a column per framework, showing what each names, how it names it, and how your adaptation composes across them. Useful when the enterprise deploys models from multiple providers with different framework shapes.
- **Frontier Model Forum interoperability read.** One paragraph on any Frontier Model Forum publications that inform the enterprise's adaptation and whether the enterprise's tier definitions are interoperable with any FMF-published shared vocabulary. Preview of the industry-coordination shape mod-109 (third-party governance) will develop.
- **UK / US AI Safety Institute overlay.** One page on how the UK AI Safety Institute (AISI) and US AI Safety Institute methodologies inform the enterprise's tier-crossing evaluation set, especially for tier-3 and tier-4 systems whose capabilities the AISIs are actively evaluating. Where the enterprise consumes AISI-published evaluation methods, that consumption is itself a mod-108 evidence artefact.
- **Fine-tuning scoped-pause commitment.** If your enterprise is one that fine-tunes, author the scoped equivalent of the training-pause move: the categories on which fine-tuning would pause, the trigger conditions, the ratifying body, and the release conditions for resumption. This is the adaptation of chapter 05's declined move for enterprises to which it partially applies.
- **Preview mod-107 assurance.** Identify at least two places in your capability-tier spec where the tier-crossing decision would benefit from second-line assurance (independent verification of the evaluation results, independent challenge of the tier assignment). Name what the assurance function would test.
