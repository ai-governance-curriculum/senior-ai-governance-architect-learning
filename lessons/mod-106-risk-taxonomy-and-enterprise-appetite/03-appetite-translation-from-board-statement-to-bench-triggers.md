# Appetite translation — from board statement to bench-level triggers

## Why this chapter exists

The board of directors or the audit committee ratifies a *risk-appetite statement* — a paragraph, sometimes a page, that says in prose what levels of risk the enterprise is willing to accept in pursuit of its strategic objectives. A typical enterprise statement reads something like: *"We accept low levels of operational, regulatory, and reputational risk in the deployment of AI systems; we accept no willingness to expose customers to physical harm; we accept moderate innovation risk in exploratory pilots of GenAI capabilities but require material risk mitigation before customer-facing production deployment."* That is a statement of *intent*. It is not, in itself, actionable. A model-risk-committee reviewer facing a launch decision on a specific chatbot at 09:17 on Tuesday cannot compute against a paragraph. The engineer building the guardrail thresholds on the same chatbot cannot either.

The **appetite translation architecture** is what turns the board's paragraph into machinery: a per-tier tolerance table, a set of escalation triggers, and a set of stop-shipping thresholds that engineers and reviewers can apply consistently. The level-50 architect designs the translation logic. The board sets the appetite. The head of AI governance (level 60) sponsors the translation to the board and defends it when audit committee members push. The risk engineer (level 25) computes the residuals that get compared against the tolerances. The evaluation engineer (level 30, peer) calibrates the scoring so the residuals are comparable across systems and across raters.

This chapter walks the parent frameworks the appetite architecture composes on (ISO 31000, ISO/IEC 27005, COSO ERM), enumerates the four artefacts the appetite architecture produces, and pins the ownership matrix. Chapter 06 will detail the quantification contract that produces the residuals the tolerances test against; this chapter designs the tolerance shape.

## The parent frameworks

The appetite architecture does not float free. It composes on three parent frameworks the enterprise almost certainly already runs — or the enterprise ERM function does — and layering on top of them is what makes the AI-specific appetite architecture defensible to the audit committee and legible to the internal audit function.

**ISO 31000:2018** *(Risk management — Guidelines)* is the generic parent. It defines risk criteria (the terms of reference against which the significance of risk is evaluated), the risk-management process (context establishment, risk identification, risk analysis, risk evaluation, risk treatment, monitoring and review, communication and consultation), and — critically for this chapter — the concept of *risk appetite* as "the amount and type of risk that an organization is willing to pursue or retain." Every ISO management-system standard defers to 31000 for the shape of its risk process; ISO/IEC 42001 clause 6.1 (mod-105 chapter 04) is a 31000-shaped process specialised to AI. The AI appetite architecture inherits 31000's vocabulary and the requirement that risk criteria be documented, ratified, and reviewed.

**ISO/IEC 27005:2022** *(Information security risk management)* is the information-security-specific companion to 31000. It brings a mature per-domain example of how appetite translates into concrete criteria: the ISO 27001 information-security risk process runs against 27005-shaped risk-acceptance criteria that the enterprise almost certainly has documented for the ISMS. The AI appetite architecture is the AI-specific sibling; it must be *compatible* with the ISMS's 27005-shaped criteria (some AI risks are information-security risks with an AI-specific twist, and the two frameworks must reach the same conclusion on shared risks) but not *identical* (the AI-specific harm categories — chapter 02 — do not all fit inside the ISMS's confidentiality / integrity / availability triad).

**COSO ERM (2017)** — *Enterprise Risk Management: Integrating with Strategy and Performance* — is the enterprise-scale parent used by most publicly-traded US and internationally-listed enterprises for board-facing risk reporting. COSO ERM's cube (governance and culture; strategy and objective-setting; performance; review and revision; information, communication, and reporting) is the frame the audit committee actually reasons in. The AI appetite architecture must produce artefacts the COSO ERM aggregation can consume — categorical alignment with the enterprise's existing risk categories (financial, operational, compliance, strategic, reputational) so AI risk rolls into the same enterprise view the board already reads, plus AI-specific tail-risk detail the aggregation would otherwise flatten.

The three frameworks are compatible; they operate at different altitudes. ISO 31000 is the vocabulary and the process. ISO/IEC 27005 is the sibling domain the AI framework must interoperate with. COSO ERM is the reporting envelope the AI appetite plugs into at board level. The architect composes rather than choosing.

<!-- needs-research: verify the current published editions of ISO 31000 (2018 confirmed), ISO/IEC 27005 (2022 confirmed), and COSO ERM (2017 confirmed) as of the writing date; note any subsequent revisions if published. -->

## The four artefacts of the appetite architecture

The appetite architecture produces four artefacts, each with a different owner, cadence, and audience. All four must exist; miss one and the translation from board statement to bench decision breaks somewhere.

### Artefact 1 — the risk-appetite statement (board-ratified)

A single artefact, one to three pages, that names for each of the enterprise's canonical risk categories (financial, operational, compliance, strategic, reputational — and, for AI-specific reasons, sometimes physical-safety and human-rights as separate categories) the *appetite level* the board is willing to accept. Appetite levels are usually a short ordinal scale — *averse / minimal / cautious / open / hungry* is the common COSO-inspired scale, though other names appear. Some appetite statements express appetite as bands (green / amber / red / hard-red-line); others as descriptive prose; the choice is the enterprise's.

The AI-specific appetite statement layers on top of the enterprise-wide statement. It says, for the AI category (or for the enterprise risk categories as applied to AI): what appetite level applies, what conditions modify it (e.g., "cautious for customer-facing deployment; open for internal-only pilots"), and what *hard limits* exist that no downstream translation can override (e.g., "no willingness to deploy AI that produces autonomous decisions affecting a customer's access to healthcare or credit without human review").

The board owns this artefact. The head of AI governance drafts it, sometimes with legal and with the chief risk officer, and sponsors it to the board. The architect is *not* the author — the architect is a consumer. The architect's stake is that the artefact must be *specific enough* to translate. A statement that reads "we accept moderate AI risk" is not translatable; the architect must go back to the head of AI governance and ask for a category-by-category breakdown.

### Artefact 2 — the per-tier tolerance table (architect-authored, head-of-AI-governance-ratified)

The tolerance table is the workhorse artefact of the appetite architecture. It is a matrix whose rows are the harm categories from the taxonomy (chapter 02, axis 1) and whose columns are the capability tiers (chapter 02, axis 2). Each cell contains a *tolerance* — the maximum acceptable residual risk score in that (category, tier) combination — plus optional cell-specific overrides (jurisdiction amplifiers, sector amplifiers).

A schematic fragment:

```yaml
tolerance_table:
  version: 2.1.0
  effective_from: 2026-Q3
  ratified_by:
    head_of_ai_governance: yes
    board_awareness_date: 2026-08-14
  cells:
    - category: confidential-information-exfiltration-via-extraction-attack
      tier: 1
      tolerance_score: 4  # low, on the 1-9 impact-x-likelihood-x-exposure scale defined in chapter 06
      rationale: Read-only classifier over public corpus; category not physically reachable
    - category: confidential-information-exfiltration-via-extraction-attack
      tier: 2
      tolerance_score: 6
      rationale: Enterprise-confidential access; residual after query-budget controls
    - category: confidential-information-exfiltration-via-extraction-attack
      tier: 3
      tolerance_score: 5
      jurisdiction_override:
        - jurisdiction: eu-ai-act-scope-high-risk
          tolerance_score: 3
          rationale: Article 15 cybersecurity minimum + Article 73 notification exposure
    - category: confidential-information-exfiltration-via-extraction-attack
      tier: 4
      tolerance_score: 3
      rationale: Tool-calling authority; residual after controls must be low
    - category: confidential-information-exfiltration-via-extraction-attack
      tier: 5
      tolerance_score: 2
      rationale: Continuous autonomy; residual after controls must be very low
    - category: discriminatory-decision-in-employment-context
      tier: 3
      tolerance_score: 3
      user_population_override:
        - population: vulnerable-population
          tolerance_score: 2
      sector_override:
        - sector: financial-services
          tolerance_score: 2
          rationale: SR 11-7 fair-lending expectations
```

The tolerance table has three design invariants:

- **Monotonicity in tier.** Higher capability tier has a *stricter* tolerance (lower score) for the same category, all else equal. If a lower tier has a stricter tolerance than a higher tier for the same category, either the tier definitions are wrong (chapter 02) or the tolerance is wrong. This is testable.
- **Explicit overrides, no implicit inheritance.** Sector, jurisdiction, and user-population overrides are enumerated per cell. There is no "if EU then tighten by two" global rule the reviewer has to know; the applicable override is on the cell.
- **Hard-red-line separation.** Cells whose tolerance is the reserved value *hard-red-line* (or `null` with a `hard_red_line: true` flag) are separately enumerated in an appendix that also lists them out of the table for board visibility. A hard-red-line cell is a *stop-shipping* trigger regardless of scoring; the artefact-3 stop-shipping threshold is where those live.

The architect authors the tolerance table. The head of AI governance ratifies it. The board is *made aware of* the table (it is a downstream translation of the artefact-1 statement they already ratified) but does not typically ratify each cell.

### Artefact 3 — the escalation triggers (architect-authored, head-of-AI-governance-ratified)

The tolerance table says what residual score is acceptable in each (category, tier) cell. The escalation triggers say what happens when the score approaches or exceeds the tolerance. Escalation triggers are the *procedural machinery* that keeps residual risk from silently drifting above appetite. Three trigger kinds, each with a defined addressee and a defined response cadence:

- **Approach-of-tolerance trigger.** Fires when the residual crosses a threshold below the tolerance (e.g., 80% of tolerance). Addressee: the system owner and the AI governance analyst monitoring that system's risk register. Response: increased monitoring cadence, additional evidence collection, review at the next scheduled AI governance council.
- **Breach-of-tolerance trigger.** Fires when the residual exceeds the tolerance. Addressee: the system owner, the AI risk engineer, the head of AI governance. Response: mandatory review at the next monthly (or shorter) risk committee; the system is *not* stopped by this trigger alone; the review decides whether to accept the residual as an exception, apply additional controls, restrict use, or stop-ship.
- **Aggregate-portfolio trigger.** Fires when the *aggregate* residual across multiple systems on the same category crosses a portfolio threshold (chapter 04). Addressee: the chief risk officer, the head of AI governance, potentially the audit committee. Response: portfolio-scale intervention — control-library changes, appetite reconsideration, cross-system remediation programme.

Each trigger is enumerated with (a) the threshold definition, (b) the addressee list, (c) the response cadence, and (d) the artefact the response produces (a review-log entry, an exception filing, a portfolio remediation ticket). Triggers without artefacts do not survive: an escalation that produces no record is an escalation that did not happen.

### Artefact 4 — the stop-shipping thresholds (board-ratified, no manager override)

The stop-shipping thresholds are the appetite architecture's *hard limits* — conditions under which the enterprise will not deploy a system, regardless of business pressure, regardless of the model-risk committee's judgement, regardless of the CEO's opinion. They implement the artefact-1 statement's hard limits and add whatever additional hard limits the head of AI governance and the board find defensible.

Stop-shipping thresholds are qualitatively different from tolerance breaches:

- A **tolerance breach** is a *scored residual above the appetite* and produces an escalation for judgement. Judgement can conclude "accept with compensating controls" or "restrict use" or "add more controls before deploying." Tolerance breaches are a machinery of *reasoning*.
- A **stop-shipping threshold** is a *categorical property of the system* that forbids deployment full stop. Examples: any tier-5 system that lacks a pre-deployment red-team report at the taxonomy-defined depth for its category; any customer-facing GenAI system that lacks synthetic-media attribution controls where EU AI Act Article 50 applies; any system that would take autonomous action affecting a person's access to safety-critical services without a human-in-the-loop interlock. Stop-shipping thresholds are a machinery of *refusal*.

Stop-shipping thresholds are ratified by the board (typically via the audit committee) because their exercise stops shipping. They cannot be overridden by any single manager, including the CEO — override, if it exists at all, requires a board-level decision with a documented rationale. Chapter 05 shows how frontier-lab tiered-risk frameworks structure their equivalent artefacts (Anthropic RSP's Deployment / Security / Capability Thresholds; OpenAI Preparedness Framework's tracked-risk-category thresholds; DeepMind FSF's Critical Capability Levels) — the enterprise adaptation is at very different scale but the *shape* is the same.

## The translation logic — from artefact 1 to artefacts 2, 3, 4

The architect's design task is the mechanical, defensible translation from the board's paragraph to the three downstream artefacts. The translation logic is enumerated so the head of AI governance can defend it to the audit committee and the internal audit function can test it.

A schematic translation record for one category:

```yaml
translation_record:
  category: discriminatory-decision-in-employment-context
  board_statement_reference: >
    "We accept no willingness to expose customers or employees to unlawful
    discrimination arising from the deployment of AI systems."
  translation:
    tolerance_table_row_ids: [TT-2.1.0-cell-042, TT-2.1.0-cell-043, TT-2.1.0-cell-044]
    tolerance_derivation: >
      "No willingness to expose" is translated as tolerance_score=2 (very low)
      at tier >= 3 and hard-red-line at tier 5 in jurisdictions where a private right
      of action attaches to the harm (Illinois BIPA / NYC LL 144 / Colorado SB24-205).
    escalation_triggers: [ET-2.1.0-004, ET-2.1.0-005, ET-2.1.0-042]
    stop_shipping_thresholds: [SST-2.1.0-011]
    stop_shipping_derivation: >
      Systems providing consequential decisions in the employment context that lack
      the disparity-testing evidence artefact defined in AIC-EVL-018 are stop-shipping.
    open_questions_for_legal:
      - >
        Interpretation question — is "consequential" in the board statement equivalent to
        the Colorado SB24-205 "consequential decision" definition, or narrower / broader?
```

Every category in the taxonomy has a translation record. The translation record is the audit-committee-facing evidence that the appetite is being translated systematically. Ambiguity in the board statement is *flagged*, not silently resolved. Where the board statement does not decisively answer a translation question, the record carries an open question for legal or for the head of AI governance to close — the architect does not decide interpretation unilaterally.

## Who authors, who ratifies, who operates

A compact ownership matrix for this module's appetite architecture, pinned to the level ladder:

| Artefact | Author | Ratifier | Operator |
|---|---|---|---|
| Risk-appetite statement | Head of AI governance (level 60), with legal and CRO | Board / audit committee | Applied via the three downstream artefacts |
| Tolerance table | Senior AI governance architect (level 50) | Head of AI governance (level 60) | AI risk engineer (level 25) scores against it; AI governance analyst (level 15) monitors cell utilisation |
| Escalation triggers | Senior AI governance architect (level 50) | Head of AI governance (level 60) | AI governance analyst (level 15) monitors; AI risk engineer (level 25) responds; head of AI governance (level 60) resolves |
| Stop-shipping thresholds | Senior AI governance architect (level 50), advised by legal | Board / audit committee (via head of AI governance) | Head of AI governance (level 60) invokes; no lower authority can override |
| Calibration of scoring | AI risk engineer (level 25) proposes; model-evaluation-engineer (peer level 30) validates methodology | Head of AI governance (level 60) | Ongoing, per chapter 06 |

## Two failure modes to design against

**Failure mode 1 — the untranslated appetite.** The board ratifies a statement. The enterprise publishes it in the annual report. No tolerance table, escalation triggers, or stop-shipping thresholds are authored. When a specific system launch review asks "is this within appetite?", the answer is a discussion among managers reading the paragraph and reaching a judgement. Two launch reviews reach opposite conclusions on the same category. Internal audit tests appetite conformance and cannot; there is no artefact to test against. The board's appetite is worse than aspirational — it is a compliance risk, because the enterprise is claiming to operate against an appetite it has not made operational. This module's four-artefact design is what prevents this failure mode.

**Failure mode 2 — the ossified tolerance table.** The tolerance table is authored, ratified, and never revisited. Two years pass. The taxonomy has added new categories (chapter 07's versioning process was not run); the enterprise has added new capability tiers (agentic autonomy tier 5 was not in the original table); the regulatory landscape has moved (EU AI Act's high-risk overlays now change the effective tolerances for a dozen cells). The tolerance table reads plausibly but no longer aligns to the actual footprint. Reviews start applying "the spirit of the table" instead of the table. The appetite architecture becomes the compliance-checkbox failure mode chapter 01 warned about. The fix is the versioning cadence and the migration plan chapter 07 walks — the appetite architecture and the taxonomy version together, on the same cadence, with the same discipline.

## Summary

The appetite architecture translates the board's ratified risk-appetite statement into four artefacts an enterprise can operate against: the statement itself, the per-tier tolerance table, the escalation triggers, and the stop-shipping thresholds. It composes on ISO 31000 (parent risk-management framework), ISO/IEC 27005 (sibling info-sec risk method the AIMS must interoperate with), and COSO ERM (the reporting envelope the audit committee reads in). The architect authors the tolerance table, the escalation triggers, and the stop-shipping thresholds; the head of AI governance ratifies; the board ratifies the statement and (via the audit committee) the stop-shipping thresholds; the risk engineer scores; the evaluation engineer calibrates. Every translation from artefact 1 to artefacts 2–4 is enumerated in a translation record with open questions for legal called out explicitly. Miss any of the four artefacts and the appetite is aspirational rather than operational; ossify any of them and the appetite drifts out of alignment with the enterprise. Chapter 04 shows how the aggregation model rolls per-system residuals against the tolerance table without hiding tail risks; chapter 05 studies how frontier labs have shaped equivalent stop-shipping structures at very different scale; chapter 06 details the scoring contract that produces the residuals the tolerances test against.
