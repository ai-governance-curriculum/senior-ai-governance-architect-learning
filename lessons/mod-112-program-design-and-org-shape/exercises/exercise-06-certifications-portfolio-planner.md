# exercise-06: Certifications Portfolio Planner

**Estimated effort:** 1.5 hours

## Objective

Author **your own certifications portfolio positioning** as a level-50 architect targeting a defined role-and-enterprise shape, then produce a **24-month attainment-and-CPE schedule** that discharges the plan against the maintenance-cycle realities of the credentials involved. Chapter 06 designs the positioning framework; this exercise applies it to a specific architect (you) targeting a specific target-role shape.

The deliverable set is the portfolio-composition YAML with hold-now / actively-pursuing / consider-later / not-priority classifications, a target-role-and-enterprise brief that pins the classifications to a defensible target, the 24-month attainment-and-CPE schedule with study time and cost, a lapse-defence plan (chapter 06's failure-mode-3 defence), and an audit-committee-style CV-narrative note explaining the portfolio's shape to a hypothetical hiring committee.

The correctness spine is chapter 06's three invariants (hold-now set defensible against the target-role hiring shape; not-priority set deliberately excluded; recertification maintenance planned) and its three failure modes (over-collection; under-collection; lapsed-maintenance).

## Prerequisites

- Chapter [`06-certifications-portfolio-positioning.md`](../06-certifications-portfolio-positioning.md) read once, with the four positioning categories, the load-bearing set, the strongly-optional set, the sector-specific and adjacent sets, the portfolio-composition framework, and the failure modes marked.
- Chapter [`03-role-descriptions-and-hiring-plan.md`](../03-role-descriptions-and-hiring-plan.md) skimmed — the enterprise-side credential bar cross-references your own portfolio positioning; the head-of-AI-governance's portfolio informs the enterprise's credential-bar setting.
- The IAPP AIGP body of knowledge, ISO/IEC 42001 Lead Implementer / Lead Auditor training-provider landscape, ForHumanity FHCA subject-matter areas, BABL AI credential portfolio, and ISACA CISA / CISM / CRISC exam pages — the study-time and cost estimates draw on the issuing bodies' own published information. Cite with `<!-- needs-research -->` where the specifics have moved since the chapter was authored.

## Scenario — your own target

State your target-role-and-enterprise shape at the top of the portfolio YAML. The scenario is you, the architect completing this exercise; the target is where you plan to be in 24 months. Choose from:

- **General-sector level-50 continuation.** You are staying at level 50 at a mid-enterprise not pursuing near-term 42001 certification. The load-bearing set is narrower; optional signal breadth is your focus.
- **Level-60 head-of-AI-governance progression.** You are targeting a level-60 head seat within 3 years. Audit-adjacent and MRM-adjacent credentials matter more; executive-search readiness is your focus.
- **Sector-specialised architect move.** You are targeting a level-50 seat at a banking / healthcare / insurance enterprise. Sector-specific competence via credentials-or-work-history matters; general breadth may drop in weight.
- **Certification-body or Big Four practice move.** You are targeting a move into an external-assurance career track. Lead Auditor, ForHumanity FHCA, and Big-Four-adjacent audit competence matter most.
- **AIMS-implementer specialisation at a 42001-pursuing enterprise.** You are staying at level 50 but the enterprise's near-term (12–18 month) AIMS certification pursuit weights 42001 Lead Implementer as load-bearing.

State the choice, then compose the portfolio against it.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`target-role-brief.md`** — the target-role-and-enterprise brief: what you are targeting, why, on what horizon.
2. **`portfolio-v1.0.yaml`** — the machine-readable portfolio classification (hold-now / actively-pursuing / consider-later / not-priority) with per-credential rationale referencing the target-role brief.
3. **`attainment-schedule-v1.0.md`** — the 24-month schedule for hold-now attainment (where not already held), actively-pursuing attainment, and CPE maintenance across the held set. Study time (in hours per week) and cost (dollar totals) are stated.
4. **`lapse-defence-plan.md`** — the plan against chapter 06's failure-mode-3 (lapsed-maintenance). Cycle tracker, renewal alerts, CPE-source diary, budget for CPE-eligible activity.
5. **`cv-narrative-note.md`** — a one-page narrative explaining the portfolio's shape to a hypothetical hiring committee. Answers: why this mix, why not others, what the mix signals about the architect's career trajectory.

## Requirements

### `target-role-brief.md`

One or two pages. Sections:

- **Target-role shape.** The role packet (level 50 continuation, level 60 progression, sector-specialised move, external-assurance move, AIMS-implementer specialisation) — reference chapter 06's target-role-shape input.
- **Target enterprise shape.** The enterprise's sector, jurisdictional focus, AIMS-certification ambition (high / medium / low / none), and shape of AI-governance programme maturity. If your current enterprise is the target, state that.
- **Career trajectory.** The 3–5-year horizon: where you plan to be, what seats you plan to occupy, what enterprise shapes you plan to be inside.
- **Composition inputs.** The three chapter-06 inputs — target-role shape, enterprise's AIMS ambition, career trajectory — with a paragraph per input stating how you resolve it in your context.

### `portfolio-v1.0.yaml`

The machine-readable portfolio. Structure per chapter 06's schematic, tailored to your target:

- **Metadata.** `id`, `architect_target_role` (from the brief), `enterprise_context` (AIMS ambition, sector, jurisdictional focus).
- **Hold-now.** Credentials to hold immediately. For each: name, issuing body, why load-bearing given the target, held-vs-pursuing status, target attainment date if not held.
- **Actively-pursuing.** Credentials being pursued on a defined study schedule. For each: name, issuing body, why on the pursuing list (chapter 06's optional-signal or audit-adjacent path), target attainment date, weekly study-hour commitment.
- **Consider-later.** Credentials on a 12–24 month horizon. For each: name, issuing body, trigger for moving to actively-pursuing (e.g., "if target-role shape shifts to certification-body move" or "if enterprise AIMS ambition moves to high").
- **Not-priority.** Credentials deliberately excluded. For each: name, issuing body, reason for exclusion — this is chapter 06's invariant 2 (the reader can name why each excluded credential is not-priority). Do not leave the block empty; the discipline of naming exclusions is what defends against over-collection.
- **Sector-specific.** Where the target is sector-specialised, name the sector-specific competence (SR 11-7 MRM for banking; FDA AI/ML for healthcare; state-insurance-commissioner model-audit for insurance) and state whether it is being demonstrated via work history, adjacent credential, or specific study.
- **Recertification and maintenance.** Per held credential, state the maintenance cycle (annual CPE, triannual renewal, etc.) and the mechanism.
- **Invariants block.** All three chapter-06 invariants with a `test:` per invariant.

### `attainment-schedule-v1.0.md`

The 24-month schedule. Structure:

- **Header.** Total study-hour-per-week commitment across the schedule; total 24-month cost estimate.
- **Month-by-month.** A row per month with the credentials in flight (studying, exam-scheduled, exam-taken), study-hour allocation across credentials, and cost incurred (exam fees, training-provider costs, materials).
- **Dependencies.** State credential-attainment dependencies (e.g., ISO 42001 Lead Implementer training is a prerequisite for the exam; some credentials have exam-eligibility requirements requiring prior work-experience documentation).
- **CPE maintenance overlay.** For credentials already held, the CPE cycle overlay (typically annual for IAPP and ISACA; state each held credential's cycle). CPE hours per year and source-diary reference.
- **Contingency.** Named contingency for exam failures, schedule slippage, or a change in target-role shape.

### `lapse-defence-plan.md`

Chapter 06's failure-mode-3 defence. Sections:

- **Cycle tracker.** A calendar of every held credential's renewal / recertification date over the next 36 months.
- **Renewal alerts.** The alert mechanism (calendar, GRC-platform reminder, external service) with lead-time triggers (typically 90 and 30 days before renewal for CPE cycles).
- **CPE-source diary.** How CPE hours will be accumulated — attended events, published articles, work-history claims, professional-committee time. State the diary's home (a specific document or system) and the maintenance cadence.
- **Budget for CPE-eligible activity.** Annual budget for conferences, subscriptions, and adjacent professional development. If the enterprise offers a tuition-reimbursement policy (chapter 06 references this), state the composition.
- **Failure-mode-3 self-check.** A quarterly self-check the architect performs to verify that no held credential is drifting toward lapse. Chapter 06's discussion of the lapsed-credential-as-negative-signal applies.

### `cv-narrative-note.md`

A one-page narrative for the hypothetical hiring committee. Answers:

- **Why this mix.** The strategic rationale — how the hold-now + actively-pursuing set fits the target role.
- **Why not other credentials.** The credentials that the committee might expect but that the architect has deliberately excluded (or de-prioritised) and why. Chapter 06's invariant 2 test lives here.
- **What the mix signals about trajectory.** The credential mix signals both a current shape and a direction. State the direction plainly — level-60 progression, sector specialisation, external-assurance move.
- **Anti-drift note.** A short paragraph stating what the credential mix is not compensating for. The narrative's honesty is what makes it credible.

## Starter guidance

The target-role brief is the input to the portfolio, not the other way around. Draft the brief first; the portfolio's classifications follow from the brief's target. An architect who drafts the portfolio first and then reverse-engineers a target from the credentials the architect happens to have is producing a defensive rationalisation, not a plan. Chapter 06's positioning framework is target-first for this reason.

The not-priority block is the discipline. The temptation is to leave it empty ("I might want these later, so I will not exclude anything"). Chapter 06's invariant 2 tests against this — the reader must be able to name why each excluded credential is not-priority. The not-priority block is what protects your study time from dilution; without it, every professional-development conversation becomes a fresh evaluation and the hold-now set never fills.

The 24-month schedule is where reality bites. Chapter 06's load-bearing set at the level 50 includes multiple credentials each requiring 100–200 hours of study; the actively-pursuing set adds more. A schedule that commits 15 hours per week to study for two years is plausible only if the architect's other commitments accommodate it. Draft the schedule against actual weekly capacity, not aspirational capacity; slippage is worse than a lower-ambition schedule that gets completed.

The CPE overlay is easy to miss. Most credentials require annual CPE; the CPE hours must come from named sources (attended events, published articles, professional-committee time) and the diary must be maintained. Chapter 06's failure-mode-3 discussion is that a lapsed credential is a stronger negative signal than an absent one. The lapse-defence plan is not optional discipline; it is the invariant-3 test in operational form.

The CV narrative is where the hiring committee's read of the portfolio is anticipated. A committee that sees ForHumanity FHCA on your CV wonders whether you are targeting an independent-audit move; a committee that sees ISACA CISA on your CV wonders whether you are targeting an audit-committee career; a committee that sees no CISA but sees GARP FRM wonders about the banking-sector focus. The narrative pre-empts the reads that would misinterpret the mix. Write it as though the committee's read is your own future defence.

Certification-body-specific pricing and cycle information is often out of date within months. Cite with `<!-- needs-research -->` for anything you are not confident about. The exercise's value is in the framework, not in the numerical precision.

## Acceptance criteria

- [ ] Chosen target is stated at the top of `portfolio-v1.0.yaml`; every artefact is coherent against it.
- [ ] `target-role-brief.md` names target-role shape, target enterprise shape, career trajectory, and how each of chapter 06's three composition inputs is resolved.
- [ ] `portfolio-v1.0.yaml` includes metadata, hold-now (with rationale per credential), actively-pursuing (with weekly study commitment), consider-later (with trigger for elevation), not-priority (with reason for exclusion — non-empty), sector-specific competence where applicable, recertification and maintenance per held credential, and invariants block with test fields.
- [ ] `attainment-schedule-v1.0.md` includes header (total study-hour commitment and 24-month cost), month-by-month schedule with credentials in flight and study-hour allocation, dependencies, CPE maintenance overlay, and contingency.
- [ ] `lapse-defence-plan.md` includes cycle tracker across 36 months, renewal-alert mechanism with lead-time triggers, CPE-source diary, budget for CPE-eligible activity, and quarterly self-check.
- [ ] `cv-narrative-note.md` is one page and answers why-this-mix, why-not-other-credentials, trajectory signalled, and anti-drift.
- [ ] Every design choice is pinnable to a chapter-06 invariant (I1–I3) as enforcer or a chapter-06 failure mode (over-collection; under-collection; lapsed-maintenance) as defence; a `pinning:` block or footnote in the portfolio YAML makes this explicit.
- [ ] Every unverified specific — exam fees, CPE cycle lengths, body-of-knowledge references, credential names and subject-matter areas — carries `<!-- needs-research -->` rather than a guessed value. Do not invent CPE cycle lengths or exam fees.

## Stretch goals

- **Enterprise-credential-bar cross-reference.** Take the exercise-03 hiring plan's credential bars and cross-check them against your own portfolio. Where does the bar you set for a hire you are recruiting exceed the bar you hold for yourself? Where is that defensible (junior seats have narrower requirements) and where is it a gap? The cross-reference makes the head-of-AI-governance's role in setting enterprise credential bars concrete.
- **Career-shift replan.** Take the portfolio you authored and replan it against a different target-role shape from the scenario list. What changes in the hold-now set, what moves from not-priority to actively-pursuing, what CPE-source diary changes. The replanning exercise makes chapter 06's target-first-framework operational.
- **Tuition-reimbursement business case.** Author the short business case the architect takes to the head-of-AI-governance or the CHRO to fund the actively-pursuing set from the enterprise's tuition-reimbursement policy. State the cost, the enterprise-side benefit (chapter 06's discussion of the enterprise credential bar), and the discipline the architect commits to (completion cadence, evidence of attainment).
