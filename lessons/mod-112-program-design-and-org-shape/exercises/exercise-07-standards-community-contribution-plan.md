# exercise-07: Standards-Community Contribution Plan

**Estimated effort:** 1.5 hours

## Objective

Author the **standards-community contribution posture** for the enterprise chosen in exercise-02, with a per-body posture level, representative seat, commitment cost, internal ratification path, and a cross-body consistency plan that keeps the enterprise's positions coherent across ISO/IEC JTC 1/SC 42, CEN-CENELEC JTC 21, IEEE, NIST, PAI, OECD, and adjacent bodies. Chapter 07 designs the framework; this exercise applies it to the enterprise's actual exposure.

The deliverable set is the machine-readable posture across the community-body map, a per-body engagement plan for the bodies where the posture is level 2 or higher, an internal-ratification-path runbook, a cross-body consistency plan, and a first-year calendar for the representative seat's engagement.

The correctness spine is chapter 07's five invariants (every body has a named posture level; every representative-carried position is council-ratified; cross-body positions compose consistently; head-of-AI-governance owns the external face; posture is versioned and re-ratified periodically) and its three failure modes (reactive we-don't-participate; committee tourism; solo-representative-goes-native drift).

## Prerequisites

- Chapter [`07-standards-community-contribution-posture.md`](../07-standards-community-contribution-posture.md) read once, with the four commitment levels, the community-body map, the invariants, and the failure modes marked.
- Chapter [`01-ai-governance-council-charter-and-decision-forum.md`](../01-ai-governance-council-charter-and-decision-forum.md) skimmed — the posture is a council reserved matter; every material representative-carried position is council-ratified before external engagement.
- Chapter [`04-cross-audience-communications-architecture.md`](../04-cross-audience-communications-architecture.md) skimmed — the position paper's six-section shape is the drafting reference for cross-body-position papers.
- Chapter [`08-boundary-to-head-of-ai-governance-and-chief-ai-officer.md`](../08-boundary-to-head-of-ai-governance-and-chief-ai-officer.md) skimmed — the head-of-AI-governance owns the external face; the CAO owns strategic-tier external positioning; the architect represents at working technical tier under the head's authority.
- The mod-104 jurisdiction-reconciled control set — the enterprise's jurisdictional exposure informs which bodies are load-bearing (JTC 21 for material EU AI Act exposure; NIST for US federal / state; SC 42 for global).
- The national-member-body path for the enterprise's operating jurisdictions (ANSI for US, BSI for UK, DIN for Germany, and adjacent — chapter 07 references these).

## Scenario

Carry the enterprise chosen in exercise-02 forward. The scenario's implications for the posture:

- **Bank scenario.** The enterprise's US federal / state exposure weights NIST engagement higher (AI RMF, AISI programme, US federal contracting where relevant). SC 42 is baseline; JTC 21 is level 1 or 2 depending on European exposure. Sector-adjacent industry-body engagement (via the American Bankers Association or similar) may compose with the posture at the sector level.
- **SaaS vendor scenario.** The enterprise's EU AI Act exposure is central; JTC 21 is at level 3 (active contributor) with the architect's material commitment. SC 42 is level 2. NIST is level 2. FMF is level 1 (observer — consumer enterprise). PAI is level 2 given the public-facing product surface. UK AISI is level 2 for material UK exposure.
- **Healthcare scenario.** The enterprise's FDA composition weights NIST AISI engagement and sector-adjacent bodies (e.g., FDA public workshops on AI/ML guidance) higher. SC 42 is baseline; JTC 21 is level 1 or 2 depending on European exposure. IEEE 7000-series has increased weight given the clinical-safety / ethics-review composition.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`contribution-posture-v1.0.yaml`** — the machine-readable posture across the community-body map.
2. **`per-body-engagement-plans/`** — one plan per body at level 2 or higher, naming attendance targets, position-drafting cadence, and specific working-groups engaged.
3. **`internal-ratification-runbook.md`** — the runbook the representative uses to ratify positions internally before carrying externally; discharges chapter 07 invariant 2 (every representative-carried position is council-ratified).
4. **`cross-body-consistency-plan.md`** — the plan the architect uses to keep positions coherent across bodies; discharges chapter 07 invariant 3 (cross-body positions compose consistently).
5. **`first-year-representative-calendar.md`** — the calendar the representative seat's time is committed against; ties commitment percentages in the posture YAML to actual dated engagements.

## Requirements

### `contribution-posture-v1.0.yaml`

The machine-readable posture. Structure per chapter 07's schematic:

- **Metadata.** `id`, `ratified_by` (AI governance council reserved matter), `head_of_posture` (head-of-AI-governance), `drafter` (senior-AI-governance-architect), `scenario`, `next_review` (annual).
- **Bodies.** One block per body. Each block carries:
  - `id` (body identifier).
  - `body_name`.
  - `posture_level` (1 / 2 / 3 / 4).
  - `access` (via national member body / direct / open workshops).
  - `representative_seat` (typically architect or head-of-AI-governance; for level 4, typically head or CAO).
  - `commitment_percent` (of the representative seat's time, per chapter 07's shape — 5–15% for level 2; 20–40% for level 3; 40%+ for level 4).
  - `internal_ratification_path` (draft position → ARB technical review → council reserved-matter ratification for material positions).
  - `current_working_group_focus` (the working groups the representative attends; may be `<!-- needs-research -->` where the current groups are unverified).
- **Cross-body composition.** A block naming the topics on which the enterprise's positions are consistent across bodies (e.g., "the enterprise's position on Article 9 risk-management-system shape is consistent between SC 42 42005 discussions, JTC 21 harmonised-standard drafting, and NIST AI RMF composition") with the architect named as owner of the consistency.
- **Invariants block.** All five chapter-07 invariants with a `test:` per invariant.

At minimum, include entries for: ISO/IEC JTC 1/SC 42, CEN-CENELEC JTC 21, IEEE 7000-series, NIST AI RMF public engagement + AISI, Frontier Model Forum (level 1 observer for consumer enterprises), Partnership on AI, OECD.AI. Add scenario-specific bodies (UK AISI for material UK exposure; Singapore AI Verify for Singapore exposure; sector-specific industry associations).

### `per-body-engagement-plans/`

One file per body at level 2 or higher. Each plan carries:

- **Body context.** One paragraph — what the body does, why the enterprise engages at this level.
- **Access path.** The national member body or direct path; the specific committee / working group the representative attends.
- **Attendance target.** Meeting-attendance commitment (regular meetings, mailing list participation, editorial group participation if level 3).
- **Position-drafting cadence.** How often the representative expects to author or co-author position input; typical topics.
- **Working groups engaged.** Named working groups the representative attends and the specific document drafts being tracked. Use `<!-- needs-research -->` for working-group titles the reader cannot verify.
- **Chapter 07 solo-representative-drift defence.** The specific practices the representative uses to avoid failure mode 3 (going-native) — regular back-briefing to the ARB, position-ratification discipline before every material intervention, rotation-or-review of the representative seat.
- **Cost estimate.** Time percentage of the representative seat plus travel / dues cost estimate.

### `internal-ratification-runbook.md`

The runbook. Sections:

- **Trigger for ratification.** Every material position the representative carries externally requires ratification. Material is defined — a position that would change the enterprise's stated posture on a mod-104 obligation, a position that would signal the enterprise's alignment with or divergence from an emerging standard, a position that would bind the enterprise's future implementation. Non-material positions (attendance, procedural votes, technical clarifications not signalling substantive posture) do not require ratification but are logged.
- **Ratification path.** Draft position paper (chapter 04's six-section shape) → ARB technical review → council reserved-matter ratification for material positions.
- **Timing.** State the lead time the ratification path requires (typically 2–4 weeks for material positions); state the compressed path for time-critical positions (council ad-hoc convening per chapter 01).
- **Documentation.** Every ratified position is minuted at the council with a decision id; every representative-carried position externally cites the decision id in the representative's internal log; every log entry is auditable at the annual internal-audit engagement.
- **Failure-mode-3 defence.** The runbook includes a quarterly cross-check the head-of-AI-governance and the architect run to identify representative-carried positions that lack a council-minute reference. Positions found are either retroactively ratified (with a "carried without ratification" flag) or the representative is briefed to reverse the position at the next opportunity.

### `cross-body-consistency-plan.md`

The plan. Sections:

- **Cross-body composition table.** One row per topic on which the enterprise carries positions across multiple bodies. Columns: topic, bodies engaged, enterprise's position, latest revision date, drafter, cross-body composer (typically the architect).
- **Consistency-review cadence.** How often the architect reviews cross-body positions for coherence — typically at each council quarterly meeting on the standing standards-community agenda item.
- **Divergence-detection protocol.** How the architect detects when a body's evolving position drifts from the enterprise's ratified position, and the escalation path when the drift is material.
- **Cross-body position-paper library.** The set of position papers the enterprise maintains as its authoritative reference across bodies. Every representative carries the library into meetings; the library is versioned; amendments are council-ratified.

### `first-year-representative-calendar.md`

The calendar. Structure:

- **Header.** The representative seat's total time commitment across bodies (sum of `commitment_percent` from the posture YAML).
- **Month-by-month.** Meeting-attendance dates for each body's regular meeting cadence; position-drafting deadlines; council ratification cycle dates (before each material external engagement).
- **Milestones.** Named milestones — e.g., "SC 42 42005 revision comment window closes M+4"; "JTC 21 harmonised-standard drafting cycle M+7"; "NIST RFI response deadline M+9".
- **Contingency.** Named contingency for schedule slippage, working-group agenda changes, and unplanned regulator-driven engagement that competes for the representative's time.

## Starter guidance

Draft the posture YAML against the enterprise's actual exposure, not against an aspirational contribution ambition. Chapter 07's failure-mode-2 (committee tourism) is the pattern where the enterprise participates in many bodies at level 2 without a strategic focus and produces shallow engagement everywhere. The posture concentrates commitment where the enterprise's material interests are; that concentration is the discipline. For most enterprises, that means level 3 on one or two bodies (typically SC 42 or JTC 21 depending on jurisdictional focus), level 2 on three to five, and level 1 on the rest.

The internal ratification runbook is where chapter 07's failure-mode-3 (solo-representative drift) is defended against. The pattern is subtle — the representative attends a body over years, forms professional relationships, and begins carrying positions that reflect the working-group consensus rather than the enterprise's position. The failure is invisible until a cross-body inconsistency surfaces. The runbook's quarterly cross-check is the mechanism that catches the drift.

The cross-body consistency plan is the architect's ongoing homework. Bodies overlap on many topics — the risk-management-system shape appears in SC 42 (23894), JTC 21 (harmonised standards for Article 9), NIST (AI RMF Manage function), IEEE (7000-series composition), and OECD (Principles composition). Positions on this topic must compose; a divergence between the architect's JTC 21 position and the head's NIST RFI response signals the enterprise cannot be reasoned with. The consistency plan's table is the artefact that makes coherence testable.

The representative-calendar's total commitment must sum. Chapter 07's commitment percentages are for individual bodies; if the posture puts JTC 21 at level 3 (20–25%) and SC 42 at level 2 (10–15%), the total is 30–40% of the architect's seat time. That is a material commitment; the CFO memo (exercise-03) and the operating-model workload (exercise-02) must accommodate it. If the sum exceeds the seat's capacity, the posture is over-ambitious; if it fits comfortably in half the seat's time, the posture may be under-ambitious for the enterprise's exposure. Calibrate.

For the bank scenario, the sector-adjacent industry-body engagement (e.g., American Bankers Association's AI work) is a natural composition. State it in the posture; the industry body's positions on regulatory questions inform (and are informed by) the enterprise's positions at SC 42 / JTC 21 / NIST. The cross-body consistency plan tracks the sector-adjacent positions alongside the standards-body positions.

For the SaaS vendor scenario, the level-3 JTC 21 engagement is the load-bearing commitment. The harmonised-standard drafting for Article 9 (risk-management-system) is what will determine what "conformity" looks like for the enterprise's high-risk-classified capabilities; the architect's active-contributor engagement is what shapes the drafting. The commitment percentage should be honest — 20–25% of the architect's time is what level-3 requires, and the CFO memo (exercise-03) must plan around it.

For the healthcare scenario, the FDA-adjacent posture is worth explicit statement. FDA public engagement is not a standards-community body in the chapter-07 sense but is analogous — the FDA's public workshops and RFIs on AI/ML guidance are venues where the enterprise's positions are heard and shaped. State the FDA engagement in the posture and cross-reference to the sector regulator posture the head-of-AI-governance carries.

## Acceptance criteria

- [ ] Chosen scenario is stated at the top of `contribution-posture-v1.0.yaml`; every artefact is coherent against it.
- [ ] `contribution-posture-v1.0.yaml` includes metadata, one block per chapter-07 default body plus scenario-specific bodies; every block has posture level, access, representative seat, commitment percent, internal ratification path, and current working group focus; cross-body composition block is present; invariants block with test fields for all five chapter-07 invariants.
- [ ] `per-body-engagement-plans/` includes one plan per body at level 2 or higher; every plan has body context, access path, attendance target, position-drafting cadence, working-groups engaged, drift-defence practices, and cost estimate.
- [ ] `internal-ratification-runbook.md` includes trigger for ratification (material vs non-material), ratification path, timing (with compressed-path option), documentation discipline, and quarterly failure-mode-3 cross-check.
- [ ] `cross-body-consistency-plan.md` includes cross-body composition table, consistency-review cadence, divergence-detection protocol, and position-paper library reference.
- [ ] `first-year-representative-calendar.md` includes header (total representative-seat commitment), month-by-month attendance and drafting deadlines, milestones, and contingency.
- [ ] Total commitment percentage across bodies in the posture YAML is sensible against the representative seat's capacity; the sum is explicit in the calendar's header.
- [ ] Every design choice is pinnable to a chapter-07 invariant (I1–I5) as enforcer or a chapter-07 failure mode (reactive we-don't-participate; committee tourism; solo-representative drift) as defence; a `pinning:` block or footnote in the posture YAML makes this explicit.
- [ ] Every unverified specific — working-group titles, meeting cadences, harmonised-standard drafting-cycle dates, national-member-body access paths, dues costs — carries `<!-- needs-research -->` rather than a guessed value. Do not invent working-group names or drafting-cycle timelines.

## Stretch goals

- **Rotation plan for the representative seat.** Author a plan for rotating the representative seat between the architect and a delegated senior second-line seat (e.g., the ai-evaluation-engineer for SC 42 evaluation-methodology working groups). The rotation defends against chapter 07's failure-mode-3 (solo-representative drift) more strongly than the quarterly cross-check alone.
- **Position-paper library index.** Author the index of the enterprise's position-paper library referenced from `cross-body-consistency-plan.md`. One entry per paper: paper id, topic, bodies applicable, version, council minute ratifying, drafter, cross-body composer, next review date.
- **Public-facing summary of the posture.** Draft a one-page public-facing summary of the enterprise's standards-community engagement — the kind of statement the enterprise's website carries under its Responsible AI page. The summary composes chapter 04's customer-audience shape with chapter 07's engagement content, and the head-of-AI-governance signs it.
