# Engagement contracts with adjacent roles

## Why this chapter exists

A control library that has no clear owner-per-entry is a wish list. A policy taxonomy that no one can *execute against* is a stack of vocabulary. An assurance architecture whose upstream signal and downstream escalation are not wired to named humans is a diagram. The way each of those artefacts becomes real is a *contract* between this role and the seat immediately below or above it on the ladder — a written agreement about what this role delivers, what it consumes, and what it defers.

This chapter authors those contracts explicitly for the three most-touched interfaces: down to the AI risk engineer (level 25), sideways-and-below to the AI evaluation engineer (level 35), and up to the head of AI governance (level 60). Exercise-05 has you write the same contract for a fourth: down to the AI governance analyst (level 15).

## What an "engagement contract" looks like

An engagement contract between two roles is a short, structured document. It fits on one page in its authored form. It has four sections:

1. **What role A delivers to role B.** Named artefacts + when they land + which fields role B relies on.
2. **What role A consumes from role B.** Named artefacts + which fields role A relies on + escalation if they are late / missing.
3. **What role A defers to role B.** Explicit non-scope. If role B is unhappy, they own the call.
4. **Shared artefacts and joint decisions.** Where the boundary is fuzzy on purpose, both parties named, with a written tiebreaker.

You will see the shape repeat below. The value is that once written, every request from B to A ("can you look at this?") has a written home: it is either a delivery, a consumption, a deferral, or a joint call.

## Contract 1 — Architect (this role, level 50) ↔ AI risk engineer (level 25)

The AI risk engineer at level 25 owns the *hands-on engineering craft of AI risk* — harm-model authoring, red-team and adversarial-ML engineering, fairness / privacy / guardrail engineering, quantification, MRM engineering, monitoring-to-risk-register wiring, incident RCA. The architect *names controls that role builds* and *reviews the deliverables*, but does not build them.

### What the architect delivers to the risk engineer

- The **AI control library** entries relevant to their scope — statement, applicability filter, implementation guidance, testing procedure, evidence contract. Delivered as OSCAL; consumable in CI/CD tooling.
- The **risk taxonomy + appetite** — canonical harm categories, capability tiers, appetite thresholds. The risk engineer classifies against this taxonomy; they do not invent it.
- The **evidence contract** — for every control the engineer is implementing, an explicit list of artefacts owed, format, retention, chain-of-custody.
- The **third-party programme's engineering interface** — tiering criteria, due-diligence questionnaire fields, contract-template clauses that the engineer's third-party integration must satisfy.
- The **monitoring-to-risk-register schema** — the fields the engineer's monitoring plumbing must emit into the register, and the appetite triggers those fields drive.

Cadence: quarterly for library updates; on-demand for new controls; incident-driven for schema evolution.

### What the architect consumes from the risk engineer

- **Control implementation feasibility feedback** — "the way this control is worded requires evidence we cannot produce inside the CI/CD budget"; "the applicability filter as written excludes this system that clearly should be in scope."
- **Draft evidence and testing procedures** — for controls where the engineering craft is closest to the ground truth, the engineer drafts the testing procedure and the architect edits it into the library entry.
- **Incident findings** — RCA outputs that inform whether an existing control needs to be added, extended, or a new control added.
- **Monitoring-signal design proposals** — the engineer knows what the monitoring plumbing can actually emit; the architect turns proposals into schema fields.

Escalation if late: to head of AI governance (level 60), who owns the resourcing.

### What the architect defers to the risk engineer

- The choice of *how* to red-team a specific system, adversarial-ML technique selection, guardrail library selection, quantification methodology within a bounded appetite. If the engineer is unhappy with the *methodology*, they own the call — the architect's job was to specify the *contract*, not the method.
- The wiring of a specific monitoring stack to a specific model deployment; the specific CI/CD hooks; the specific incident RCA process at engineering scope.
- All hands-on remediation of an individual system.

### Shared artefacts / joint decisions

- The **risk-register schema** — architect owns the schema; risk engineer owns the population and wiring. Change requests go through joint sign-off. Tiebreaker: head of AI governance.
- The **guardrail-effectiveness testing procedure** — architect owns the testing procedure as a control entry; risk engineer owns the specific test implementations. Change requests joint. Tiebreaker: architect if it affects other systems; risk engineer if it is scoped to one system.

## Contract 2 — Architect (this role, level 50) ↔ AI evaluation engineer (level 35)

The AI evaluation engineer at level 35 owns release-assurance methodology and audit-trail production inside the assurance system the architect designs. The seam is that the architect designs *the assurance system*; the evaluation engineer *runs assurance inside it*.

### What the architect delivers to the evaluation engineer

- The **assurance architecture** — three lines of defense for AI, pre-deployment assurance shape, ongoing assurance cadence, third-line audit expectations.
- The **evidence architecture and schemas** — model / system / dataset / risk-card schemas, audit-log architecture, chain-of-custody expectations, retention obligations.
- The **applicable control library entries** and their **evidence contracts**.
- The **regulator-facing artefact templates** — EU AI Act Article 11 technical documentation shape, SR 11-7 model documentation, FDA PCCP submission shape (where applicable).

Cadence: the assurance architecture is a mod-107 deliverable that stabilises once and updates by exception. Evidence architecture is versioned; changes cascade to evaluation-engineer tooling.

### What the architect consumes from the evaluation engineer

- **Evidence-schema field feasibility feedback** — "this field cannot be produced without instrumenting the harness in a way that breaks eval reproducibility"; "the retention period is infeasible for this artefact class."
- **Draft regulator-facing artefact contents** — the evaluation engineer drafts; the architect reviews for structural / crosswalk correctness.
- **Assurance-gate outcomes at aggregate level** — how the assurance architecture is holding up in practice; where gates are consistently late; where evidence gaps recur.

### What the architect defers to the evaluation engineer

- The specific evaluation methodology inside a gate — which benchmarks, which harness, statistical calibration, judge-vs-human setup, red-team playbook execution.
- The audit-trail *implementation* — the audit-log architecture is architected; the specific logging middleware, sink choice, and retention infrastructure are not.
- The regulator-facing *fill-in* — the template is architected; the actual filling-in for a system is delivered by the evaluation engineer with the analyst (level 15).

### Shared artefacts / joint decisions

- The **evidence-card schemas** — architect owns the schema; evaluation engineer owns the mechanics of producing evidence into them. Fields added or removed require joint sign-off. Tiebreaker: architect if the change affects regulator-facing artefacts; evaluation engineer if the change is internal-only.

## Contract 3 — Architect (this role, level 50) ↔ Head of AI governance (level 60)

The head of AI governance owns program leadership: board reporting, regulator engagement, budget, escalation, external positioning. The architect *hands the head the operating design*; the head *executes leadership through it*.

This contract runs slightly differently — the architect is delivering *up*, and consumption is what makes the architecture stick with executives and regulators.

### What the architect delivers to the head

- The **operating model** — governance council charter (draft), RACI, three-lines-of-defense for AI, escalation ladders, decision authorities.
- The **AIMS** — a certifiable ISO/IEC 42001 AIMS with SoA, risk-treatment plan, internal audit programme, management-review cadence.
- The **cross-jurisdiction reconciliation architecture** — a single implementable target-state across jurisdictions the org operates in.
- The **GRC-for-AI reference architecture** and the vendor-evaluation matrix.
- The **12-month rollout plan** (project-103 deliverable).
- **Position papers** on architectural decisions the head takes into board / regulator / audit-committee interactions.

Cadence: strategic artefacts land at project cadence; position papers on-demand.

### What the architect consumes from the head

- **Executive priorities and budget envelope** — which architectural investments are supported, at what pace.
- **Regulatory-engagement intelligence** — what regulators are asking about in current interactions; what the enterprise's regulatory posture will be. Architecture must adjust to this.
- **Board-level risk-appetite statements** — the architect calibrates thresholds against these; the appetite itself is a board / executive artefact.
- **Escalations from below** — cases where a downstream role (analyst, risk-engineer, evaluation-engineer) has raised something needing architectural response.

### What the architect defers to the head

- All **board reporting** — the architect does not present to the board; the head does. The architect writes the material.
- All **regulator engagement** at working level — the architect drafts the position; the head takes the meeting.
- **Programme budget** — the architect proposes; the head owns the ask.
- **External positioning** — public statements, community roles, media commentary.
- **Personnel decisions** — the architect proposes role shapes; the head hires.

### Shared artefacts / joint decisions

- The **governance council charter** — architect drafts; head chairs / owns membership. Changes go through joint sign-off. Tiebreaker: head, because they own the accountability with the executive team.
- **Regulatory position papers** — architect drafts the substance; head owns the framing for the specific audience. Changes joint; tiebreaker: head.

## Reading the three contracts as a system

Notice a pattern: the architect's role runs the same across the three interfaces — *design the operating system, deliver the schemas / catalogs / templates, consume feasibility and reality from below and priorities and posture from above, defer method to below and leadership to above*.

The architect does not have execution authority downward or leadership authority upward. What the architect has is *design authority* on the operating system. When either an execution question or a leadership question lands on the desk, the contract routes it.

## Writing your own contracts (exercise-05)

Exercise-05 asks you to write a fourth contract of your own — the architect ↔ AI governance analyst (level 15) interface. The analyst owns operational analyst legwork; the architect designs the schemas + templates the analyst executes against.

Use the same four-section shape. The four sections + a page limit are the discipline; they make the contract legible in real orgs, where nobody reads a five-page contract but everyone reads a one-pager.

## Summary

Engagement contracts turn architectural design authority into *executable seams*. Each contract has four sections — delivery, consumption, deferral, shared / joint — and fits on a page. Three interfaces matter most: the risk engineer (level 25) below, the evaluation engineer (level 35) sideways-and-below, and the head of AI governance (level 60) above. Write them explicitly, keep them in the same repo as the control library, and every "can you look at this?" request from an adjacent role has a written home.
