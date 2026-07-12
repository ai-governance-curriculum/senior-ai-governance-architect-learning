# Where architectural design authority begins and ends

## Why this chapter exists

Every AI governance decision that reaches this role's desk has already touched a chain of people below and will hand off to a chain of people above. If you do not know exactly where your seat sits on that chain, you will do one of two things wrong: you will *re-do* work that a lower level already owns (drafting an impact assessment when the analyst already ships them) or you will *reach into* work that a higher level owns (making the board-level trade-off between shipping a high-risk model and holding the line). Both waste the org and neither is architecture.

This chapter draws the ladder in your family and pins your seat to it. It is deliberately narrow: only the AI Governance family, only the seats immediately adjacent, only the deliverables that flow across the interfaces. Everything else in this module — reading standards, composing catalogs, authoring engagement contracts — hangs off this positioning.

## The level ladder in one picture

The AI Governance family runs from operational analysts up to executive scope. The public curriculum names each seat, its level number, and its scope:

| Level | Track | Scope this role owns |
|---|---|---|
| 15 | `ai-governance-analyst` | Operational analyst legwork — intake, inventory, framework crosswalk drafts, first-draft impact assessments, model / system / dataset cards at analyst tier, control-tracking, jurisdictional tracking, reading eval / audit evidence into governance records. |
| 25 | `ai-risk-engineer` | Hands-on engineering craft of AI risk — harm-model authoring, red-team / adversarial-ML / fairness / privacy / guardrail engineering, quantification, MRM engineering, monitoring-to-risk-register wiring, incident RCA. |
| 35 | `ai-evaluation-engineer` | Release-assurance and audit-trail methodology — pre-deployment gates, audit-facing evidence packs, regulator-facing artifact production inside the assurance system. |
| 40 | `agentic-safety-engineer` | Frontier-agent red-team methodology and dangerous-capability evaluation. |
| **50** | **`senior-ai-governance-architect`** *(this track)* | **Architecture** of the enterprise AI governance and risk program — control library, policy taxonomy + policy-as-code, cross-jurisdiction reconciliation, AIMS, risk taxonomy + appetite, assurance architecture, evidence architecture, third-party and supply-chain programme, post-market surveillance, GRC-for-AI reference architecture, operating model, sector blueprints. |
| 60 | `head-of-ai-governance` | Program leadership — board reporting, regulator engagement, budget, escalation, external positioning at working level. |
| 70 | `chief-ai-officer` | AI strategy, P&L alignment, executive positioning. |

Read this table as a *scope contract*, not a promotion path. Level 50 is not "senior level 25" — the shape of the work is different. Below 50, work is *executed*. At 50, work is *designed*. Above 50, work is *led*.

## The one-line role definition

A useful pocket definition:

> The senior AI governance / risk architect designs the operating system of the enterprise AI governance program — the schemas, catalogs, taxonomies, reference architectures, and engagement contracts that lower levels execute inside and higher levels lead through.

Two words in that sentence do heavy lifting.

*Designs*: the deliverable is a design artifact — a control library, a taxonomy, an OSCAL representation, an operating model — not a filled-in impact assessment for a single system, not a fixed bug, not a testified board minute.

*Operating system*: everything the architect ships is meant to be reused. A single-shot output that only helps one team once is a smell; if the architect keeps producing those, the org is missing an analyst.

## What is not architecture — the two most common scope mistakes

**Mistake 1: doing analyst work at architect scope.** Analysts at level 15 draft the impact assessment for the new fraud-detection model. The architect owns the *impact-assessment schema* that the analyst fills in — the fields, the applicability filters, the evidence links, the sign-off routing. If you find yourself opening a specific system's assessment and writing sentences into it, you have slid down two levels. Go back up: is the schema wrong? Missing a field? Missing a filter? Fix the schema; hand it back.

**Mistake 2: doing head-of-governance work at architect scope.** The head-of-AI-governance (level 60) walks into a board meeting and defends the enterprise's decision to deploy a high-risk model. That defence draws on the risk taxonomy, the appetite statement, the assurance evidence — all of which the architect designed. But the *seat at the board table* is not yours. Nor is the regulator meeting. Nor is the budget request for the GRC-for-AI platform. The architect briefs the head; the head executes.

Both mistakes are unwinnable from your seat. In Mistake 1, you get slower results than the level 15 who does this every day; in Mistake 2, you lack the org standing to make the call stick.

## What architectural design authority *does* include

Six deliverable classes are unambiguously yours. Each of them is developed in a later module of this track:

1. **The AI control library** (mod-102) — control statements + applicability filters + implementation guidance + testing procedures + evidence contracts, composing NIST AI RMF sub-categories with ISO/IEC 42001 Annex A, EU AI Act obligations, OWASP LLM Top 10, MITRE ATLAS, Google SAIF, CISA/NCSC Secure AI System Development, all in OSCAL.
2. **The policy taxonomy and policy-as-code slice** (mod-103) — Responsible AI principles → binding policy → standards → procedures → work instructions, plus OPA/Rego and Cedar policy implementations at runtime, CI/CD gate, and attestation.
3. **The cross-jurisdiction reconciliation architecture** (mod-104) — EU AI Act + US federal + US state + UK + Canada + China + Singapore + Australia + India + Korea + Brazil folded into one implementable target-state.
4. **The AIMS + risk taxonomy + assurance architecture** (mods 105–107) — a certifiable ISO/IEC 42001 AIMS, an ISO 31000-compatible risk taxonomy with appetite, and a three-lines-of-defense-for-AI assurance architecture.
5. **The evidence architecture + third-party programme + post-market surveillance** (mods 108–110) — model / system / dataset card schemas, ML-BOM + SPDX AI + SLSA + Sigstore representation, third-party AI governance, EU AI Act Article 72/73 monitoring.
6. **The GRC-for-AI reference architecture + operating model + sector blueprints** (mods 111–113) — the system-of-record architecture, the governance council + RACI, the sector instantiations.

If a request does not fit into one of these six buckets and does not naturally sit above (leadership) or below (execution), question whether it is really yours.

## Where the boundary is soft — and how to signal ownership on those

Some deliverables have both an architectural face and an operational face. Two examples:

- **The risk register** is *populated* by analysts and *acted on* by risk engineers, but the *schema* — fields, states, aggregation rules, appetite triggers, escalation routing — is architected. If you touch it, touch the schema.
- **The evaluation harness** is *built and run* by evaluation engineers (level 35), but the *evidence contract* — which fields the harness must emit, which retention obligations apply, how the evidence links back to the control library entry — is architected. If you touch it, touch the contract.

On soft boundaries, name the artifact you own explicitly in tickets and RFCs: "architect owns evidence contract v1.3; evaluation-engineer owns harness implementation." Two owners with clear artifacts beats one owner spread thin.

## How to use this ladder in day-to-day triage

When work lands on your desk, run three questions:

1. **Is the deliverable a design artifact?** If it is a filled-in-for-one-system output, route it down to level 15 (analyst) or level 25 (engineer).
2. **Is the decision a working-level call, or does it hit the board / regulator / budget?** If the latter, route it up to level 60 (head) or 70 (CAO).
3. **Which of the six deliverable classes does this fit?** If none, ask again whether it is really architecture.

Getting these three questions habitual is the biggest single skill this module is trying to build.

## Summary

The senior AI governance architect sits at level 50, between hands-on execution below (levels 15, 25, 35, 40) and program leadership above (60, 70). The role designs the operating system of the AI governance program — schemas, catalogs, taxonomies, reference architectures, engagement contracts — packaged so lower levels can execute inside them and higher levels can lead through them. The chapter's ladder table, the one-line role definition, and the two most common scope mistakes give you the pocket tools to sort work correctly the first time.
