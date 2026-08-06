# The AI policy hierarchy — authority, cadence, and exception paths

## Why this chapter exists

The control library from mod-102 answers *what does the organisation do, and how do we test it?* It does not answer *why does the organisation do it, on whose authority, and what happens when someone wants an exception?* Those questions live one layer up, in the policy taxonomy. This chapter designs the *shape* of that taxonomy — the five layers, the authority body attached to each, the review cadence, and the exception path.

The five-layer taxonomy — Responsible AI principles → binding policy → standards → procedures → work instructions — is not novel to AI. It is the same layering mature enterprises use for information-security, quality-management, and safety-management. The level-50 architect's contribution is not to invent it; it is to *insist on it* for the AI programme, draw the boundaries between layers so each does exactly one job, and attach the right authority body, verb strength, and exception path to each. A programme that collapses two layers together — the most common failure — ends up with either a "policy" no exec will ratify because it is really a 40-page work-instruction, or a "standard" no engineer will follow because it is really an aspirational principle.

The architect at this level authors the *taxonomy* and the *standards* layer inside it. The architect does not draft binding-policy text (head-of-AI-governance at level 60 sponsors that, with legal); does not draft procedures (ai-risk-engineer at level 25 owns engineering-heavy ones); does not draft work instructions (ai-governance-analyst at level 15 maintains those for governance workflows). Getting this delegation right is what stops the architect becoming the bottleneck for every wording change.

**Policy versus control — the essential distinction.** A policy is a *stance* the organisation takes. A control (mod-102) is the *testable expression* of that stance. "The organisation shall ensure meaningful human oversight of high-risk AI decision systems" is a policy statement — an outcome at a level of abstraction that survives platform choices. "For each tier-1 decision system, a human reviewer approves output before it reaches the customer, and the review is logged with reviewer identity, timestamp, and decision" is a control statement — an auditor can test it. Policies live in this taxonomy; controls live in mod-102's catalog; chapter `02-principle-to-policy-to-standard-to-control-traceability.md` shows how one traces to the other.

## Layer 1 — Responsible AI principles

**What they are.** Five to eight top-level commitments the organisation makes about AI, ratified by the board or its AI committee. Principles are short, unconditional, evergreen, non-negotiable. They appear in the annual report, on the external "our AI values" page, and in the front matter of every downstream policy.

**Anchor sources.** Principles are almost always adapted from a small set of international-consensus documents. Three the level-50 architect should know cold, cited by publisher:

- **OECD AI Principles** — published by the OECD in 2019, updated in 2024. Five values-based principles (inclusive growth; human-centred values; transparency; robustness/security/safety; accountability) plus five recommendations to governments. Endorsed by 40+ jurisdictions and referenced by the G7 Hiroshima Process; a principle set that ignores this framing looks eccentric to any regulator that grew up on it.
- **UNESCO Recommendation on the Ethics of Artificial Intelligence** — adopted by the UNESCO General Conference in November 2021. Broader than OECD, covering values (human rights and dignity; environment and ecosystem flourishing; diversity and inclusiveness; peaceful, just and interconnected societies) and principles (proportionality and do no harm; safety and security; fairness and non-discrimination; sustainability; privacy and data protection; human oversight and determination; transparency and explainability; responsibility and accountability; awareness and literacy; multi-stakeholder and adaptive governance).
- **NIST AI RMF trustworthy characteristics** — the seven characteristics NIST names in the AI Risk Management Framework 1.0: valid and reliable; safe; secure and resilient; accountable and transparent; explainable and interpretable; privacy-enhanced; and fair — with harmful bias managed. A convenient checklist to ensure the enterprise principle set does not silently omit a category regulators and auditors expect to see.

The architect's job is not to copy one source; it is to compose 5–8 principles that (a) each map to at least one anchor characteristic in each of the three sources, and (b) each survive translation into binding policy at layer 2 without becoming meaningless. "We will be ethical" fails (b); "we ensure meaningful human oversight of AI systems whose decisions materially affect people" survives.

**Authority body.** The board, or the board's AI / risk / ethics committee. Ratifying principles lower down is a category mistake and will bite the first time a regulator asks *who approved this?*

**Verb strength.** Declarative present tense. "We commit to…", "The organisation ensures…". No `shall`, no `must`. Principles state a stance; obligation-language starts at layer 2.

**Review cadence.** Two to three years, plus change-triggered review on: (a) material change to an anchor source (an OECD revision, a new UNESCO recommendation, a NIST AI RMF major version); (b) material change to the enterprise's AI strategy or business model; (c) a regulatory event that renders a principle materially incomplete. Calendar-only review at this layer is a smell.

**Exception path.** *None.* A principle cannot be waived. If the organisation cannot meet a principle in a specific case, either the case must change or the principle must change; there is no per-case bypass.

**Worked example.**

```markdown
# Responsible AI Principle 3 — Meaningful human oversight

We ensure that AI systems whose outputs materially affect people are
subject to meaningful human oversight — sufficient for a competent
human to understand the system's role in the decision, to override it,
and to be accountable for the outcome.

Ratified by: Board AI Committee
Anchor sources:
  - OECD AI Principles 2019/2024 (human-centred values; accountability)
  - UNESCO Recommendation 2021 (human oversight and determination)
  - NIST AI RMF 1.0 (accountable and transparent)
Next review: 24 months from ratification
Exceptions: None.
```

## Layer 2 — Binding policy

**What it is.** The corporate-policy document, legally binding on all employees and contractors, published in the corporate policy portal alongside anti-bribery, information-security, and code-of-conduct. This is where the organisation makes *obligations* — of the organisation to its stakeholders, and of employees to the organisation. Policy is where legal binding force lives.

**Shape.** Typically one enterprise-wide "AI Policy" document (5–15 pages), sometimes with a small number of topic policies (e.g. a GenAI Acceptable Use Policy for staff use of third-party GenAI tools). The AI policy references the principles above and the standards below; it does not re-explain either.

**Verb strength.** `shall`. Every operative clause uses `shall`. Not `must` (reserved for standards), not `should` (forbidden in binding policy). "Employees should consult the AI Governance Council" is not a policy; it is a suggestion.

**Authority body.** Board or ExCo. The head-of-AI-governance (level 60) is the *sponsor* who owns drafting, socialising, and shepherding to the board. The level-50 architect is a *contributor* who owns ensuring the policy references the standards layer correctly and does not accidentally do a standard's job.

**Review cadence.** Annual review at minimum, plus change-triggered review on: (a) a change to a layer-1 principle, (b) a material regulatory change in a jurisdiction where the organisation operates, (c) an internal audit finding on policy adequacy, (d) a serious incident that policy could reasonably have prevented. The annual review may confirm no change, but the review event itself must happen and be minuted — regulators look for the minute.

**Change approval body.** Board or ExCo, on recommendation from the AI Governance Council. Heavier than any lower layer: legal review, works-council consultation where applicable, publication with a stated effective date and (for material changes) a transition window (chapter `05-policy-change-communications-and-deprecation-windows.md`).

**Exception path.** *Rare, board-level waiver only.* A policy exception is an exception from a `shall` clause — an exception from something the board committed to. It exists — enterprises sometimes need a time-boxed pilot that a strict reading of policy would prohibit — but it goes to the board or its delegated committee, is time-boxed (typically 90 days or less), carries compensating controls, and is registered in the enterprise risk register (mod-106). See chapter `04-exception-and-waiver-workflow.md`. Rule of thumb: if a policy generates more than one or two waivers per year, the policy is probably wrong, not the requesters.

**Worked example (excerpt).**

```markdown
# AI Policy — clause 4.3 (Human oversight of high-risk decision systems)

The organisation shall ensure that every AI system classified tier-1
or tier-2 under the AI Risk Tiering Standard, and whose outputs
materially affect people, is subject to human oversight sufficient to
meet Responsible AI Principle 3.

The AI Governance Council shall maintain the AI Human Oversight
Standard that specifies the oversight modalities acceptable at each
tier.

Employees shall not deploy or materially modify a tier-1 or tier-2
system without an approved oversight design conforming to that
Standard.

Exceptions require Board AI Committee approval, are time-boxed to
no more than 90 days, and require the compensating controls
specified in the associated waiver record.
```

The policy does *not* say what "meaningful oversight" looks like operationally. It commits the organisation to the outcome, delegates the modality catalogue to a standard, and points at the exception path. That is the correct division of labour.

## Layer 3 — Standards

**What they are.** Topic-specific normative documents that expand a policy clause into implementable requirements. Standards are where the level-50 architect does most of the writing personally. A well-run AI programme has 10–25 standards; three means the layer has collapsed upwards into policy; eighty means fragmentation and needs consolidation.

Typical AI-programme standards include: AI Risk Tiering, AI Human Oversight, AI Data Governance, AI Model Documentation, AI Evaluation, AI Post-Market Monitoring, GenAI Application Development, Third-Party AI, AI Incident Response, AI Transparency. The list is enterprise-specific; the shape is not.

**Verb strength.** `must`. The distinction from policy's `shall` is a convention: `shall` says *the organisation commits*; `must` says *to comply, you do this*. Some enterprises collapse the two; if yours does, pick one and be consistent.

**Authority body.** Authored by the level-50 architect together with head-of-AI-governance (level 60), approved by the AI Governance Council, published under the head-of-AI-governance's signature. The Architecture Review Board is often the reviewing body for standards that touch platform architecture (e.g. AI Model Documentation constrains the model registry).

**Review cadence.** 12 to 18 months, plus change-triggered review on: (a) a change to the parent policy clause, (b) a change to any anchor framework the standard composes (a new NIST AI RMF version, an ISO/IEC 42001 amendment, an EU AI Act delegated act, an updated CSF profile), (c) a control-catalog change that renders a standard's requirement stale, (d) a repeated waiver pattern indicating the standard is out of step with reality.

**Change approval body.** AI Governance Council for material changes; the standard's owning architect for editorial changes. The change-classification rules mirror the semver-ish rules for controls in mod-102 chapter 06 — a *material* change alters what a downstream reader must do to comply; an editorial change does not.

**Exception path.** *Documented waiver.* More common than a policy exception, decided at AI Governance Council level, time-boxed (usually 6–12 months), with explicit compensating controls, registered in the exception register. Chapter `04-exception-and-waiver-workflow.md` details the workflow.

**Worked example (excerpt from the AI Human Oversight Standard).**

```markdown
# AI Human Oversight Standard — clause 3.2 (Oversight modalities by tier)

Tier-1 systems must implement one of:

  M1. Pre-decision review — a competent human reviewer approves each
      output before it reaches the affected person. The reviewer must
      have sufficient information to understand the system's
      contribution, authority to override or reject the output, and
      time budgeted for review that is not itself a productivity target.

  M2. Post-decision sampled review with reversibility — outputs reach
      affected persons without prior review, provided a documented
      sampling procedure reviews at least the sample fraction
      specified in the associated control, affected persons have a
      documented reversal channel with a stated SLA, and reversal
      outcomes feed back into evaluation quarterly.

Tier-2 systems must implement M1, M2, or:

  M3. Human-in-the-loop confirmation — the affected person themselves
      confirms the system's suggestion before it is acted upon.

Selection of modality must be recorded in the AI Model Documentation
package and approved by the deploying business unit's designated
AI accountable executive.
```

The standard names the modalities and their preconditions but does not tell an engineer *which button to click*. That is the next layer down.

## Layer 4 — Procedures

**What they are.** Repeatable process descriptions at role grain. A procedure names a role, describes the steps that role executes to comply with a standard clause, names the inputs and outputs, and points at the tools. Procedures are shorter than standards and more concrete.

**Verb strength.** Imperative. "Open the AI Model Registry. Locate the model card. Confirm…" — written for the human performing the work.

**Authority body.** Owned by the role that executes the procedure. Engineering-heavy procedures (e.g. "Logging pre-decision reviewer approvals in the decision-log service") are owned by ai-risk-engineer (level 25) in draft, reviewed by the architect, signed off by the AI Governance Council secretariat. Governance-workflow procedures (e.g. "AI Governance Council intake meeting") are owned by ai-governance-analyst (level 15) in draft, reviewed by head-of-AI-governance.

**Review cadence.** 6 to 12 months, plus change-triggered review on: (a) a change to the standard the procedure implements, (b) a tooling change, (c) an operational issue raised by the executing role, (d) an incident retrospective action.

**Change approval body.** The procedure owner's manager, or the AI Governance Council secretariat where the procedure is cross-role. Procedures are the layer where change velocity is highest and approval should be lightest; if a procedure change needs a council vote, the layering is wrong.

**Exception path.** *Change request.* A procedure exception is best treated as a change request against the procedure itself — either the procedure is right and the requester is wrong, or the procedure is out of date and needs an edit. A formal waiver at procedure level is a smell; it usually means the standard above needs the waiver instead.

**Worked example (excerpt).**

```markdown
# Procedure P-HOV-M1 — Logging a pre-decision reviewer approval

Implements: AI Human Oversight Standard clause 3.2 modality M1
Executing role: designated human reviewer
Owner role: ai-risk-engineer (level 25)

  1. Open the review queue in the Decision Log Service.
  2. Read the candidate output and the contextual package.
  3. Confirm the package contains the seven mandatory context fields
     (see WI-HOV-M1-01).
  4. Select one of {approve, reject, escalate}. If escalate, follow
     WI-HOV-M1-02.
  5. Confirm the log entry appears in your Decision Log dashboard
     within 60 seconds; if not, follow WI-HOV-M1-03.

Output: Decision Log entry (append-only) with reviewer identity,
timestamp, output hash, decision, and optional rationale.
```

## Layer 5 — Work instructions

**What they are.** Step-by-step how-to at task grain, often screen-by-screen. Tool-specific detail lives here — screenshot references, exact menu paths, exact copy-paste snippets. The shortest-lived of the five layers and the one where the architect's involvement is lightest.

**Verb strength.** Imperative and concrete. "Click the `Approve` button. Enter the rationale in the free-text box (maximum 500 characters)."

**Authority body.** Owned by the team executing the work — ai-governance-analyst (level 15) for governance workflows; the delivery team for engineering workflows.

**Review cadence.** On-change. Updated when the tool changes, when a screen changes, when a step is added or removed. Calendar-based review here is wasteful.

**Change approval body.** The team lead. Work-instruction changes do not go to the AI Governance Council; making them do so is a common way to strangle governance velocity.

**Exception path.** *Defect report.* If a work instruction cannot be followed as written, that is a defect. File it, fix it, close it. There is no such thing as a "waiver" at work-instruction level.

**Worked example (fragment).**

```markdown
# WI-HOV-M1-01 — Confirming the context package fields

  1. In the Decision Log Service review queue, click the candidate
     output row to open the detail pane.
  2. Scroll to the "Context Package" section.
  3. Confirm these seven fields are present and non-empty:
     subject_id, decision_type, model_version, input_summary,
     top_features, confidence, policy_flags.
  4. If any field is missing or empty, do NOT approve. Select
     "escalate" and follow WI-HOV-M1-02.
```

## Comparison table — the layers side by side

| Layer | Authority body | Verb strength | Review cadence | Change-approval body | Exception path |
|---|---|---|---|---|---|
| Principles | Board / Board AI Committee | Declarative present tense | 2–3 years + change-triggered | Board | None. Amend the principle or change the case. |
| Binding policy | Board or ExCo (sponsored by head-of-AI-governance, level 60) | `shall` | Annual + change-triggered | Board / ExCo on AI Governance Council recommendation | Rare board waiver, time-boxed, compensating controls. |
| Standards | AI Governance Council (authored by level-50 architect + head-of-AI-governance) | `must` | 12–18 months + change-triggered | AI Governance Council (material) / owning architect (editorial) | Documented waiver at AIGC level, time-boxed, compensating controls, register entry. |
| Procedures | Owning role (ai-risk-engineer engineering-heavy; ai-governance-analyst governance workflows) | Imperative | 6–12 months + change-triggered | Procedure owner's manager, or AIGC secretariat where cross-role | Change request against the procedure. |
| Work instructions | Executing team | Imperative and concrete | On-change | Team lead | Defect report. |

Two things to notice. First, authority *decreases* down the layers and change velocity *increases* — deliberate, and what lets the taxonomy carry stability at the top and agility at the bottom. Second, the exception path *shortens* down the layers — no exception for principles, board waiver for policy, AIGC waiver for standards, change request for procedures, defect report for work instructions. The shape of the exception path is a first-class design decision at every layer, not an afterthought. Chapter `06-iso-38507-22989-and-ieee-7000-composition.md` shows how this composes with ISO/IEC 38507's governance-of-AI framing; mod-111 shows how a GRC-for-AI platform models each exception type as a distinct workflow.

## Worked scenario — human oversight, all the way down

One topic — meaningful human oversight of high-risk decision systems — traced through all five layers. Each layer is short; the point is that each does its own job and does not do the layer below's or above's.

- **Layer 1 — Principle 3.** Reproduced above. Six lines. Board-ratified. No modalities, no thresholds, no tools.
- **Layer 2 — AI Policy clause 4.3.** Reproduced above. `shall` clauses. Commits the organisation to the outcome, delegates the modality catalogue to a standard, points at the exception path.
- **Layer 3 — AI Human Oversight Standard clause 3.2.** Reproduced above. `must` clauses. Names three modalities (M1, M2, M3), preconditions, tier-to-modality mapping. No tool names, no button labels.
- **Layer 4 — Procedure P-HOV-M1.** Reproduced above. Imperative steps. Names the Decision Log Service and the review queue. Points at three work instructions for the fiddly parts.
- **Layer 5 — Work instruction WI-HOV-M1-01.** Reproduced above. Screen-by-screen. Names the seven context-package fields exactly.

**What connects them into the control library from mod-102?** A single control — call it AIC-HOV-014, "Pre-decision human review for tier-1 decision systems" — whose statement re-expresses clause 3.2 M1 as a testable outcome, whose applicability filter selects tier-1 decision systems, whose implementation guidance points at Procedure P-HOV-M1, whose testing procedure names the Decision Log evidence, and whose crosswalk points at Principle 3, AI Policy clause 4.3, and Standard clause 3.2 M1. The traceability across all five layers plus the control catalog is what chapter `02-principle-to-policy-to-standard-to-control-traceability.md` covers in full. Chapter `03-policy-as-code-enforcement-tiers.md` then shows which of the resulting controls are enforceable at runtime (OPA / Rego / Cedar), which at CI/CD gate time, and which by attestation only. Multi-jurisdictional composition is in mod-104; the AIMS home for the standards is in mod-105.

## Two common shape mistakes

**Mistake 1 — Collapsing standards into policy.** A "policy" that runs to 40 pages, names specific tools, includes screenshots, and contains three tables of thresholds is not a policy. It is a policy plus a standard plus a procedure, glued together and sent to the board for ratification. Two symptoms: (a) the board will not ratify it because it is too operational, so it stays "in draft" for years; (b) any change requires board approval, so it never updates. The fix is to split — the top three pages become the policy, ratified once and rarely; the rest becomes one or more standards, ratified by the AI Governance Council on a 12–18 month cadence.

**Mistake 2 — `shall` at every layer, and calendar-only review cadences.** Using `shall` in principles ("we shall be transparent"), in standards, in procedures, and in work instructions ("the reviewer shall click Approve") flattens the verb-strength gradient and destroys the reader's ability to tell what layer they are in. A principle stated with `shall` reads like a policy clause, invites the auditor to test it as one, but has no testable operationalisation — the audit fails. The related sub-mistake is reviewing every layer only on its calendar cadence: the annual review then discovers standards drifted out of alignment with a regulatory change nine months ago, three procedures reference a decommissioned tool, and the policy still cites Principle 2.4 which was retired at the last board meeting. Every review cadence in the table above is *cadence-plus-triggers*, not cadence-only. The fix is disciplined verb selection (declarative for principles, `shall` for binding policy only, `must` for standards, imperative for procedures and work instructions) and disciplined trigger detection (chapter `05-policy-change-communications-and-deprecation-windows.md`).

## Summary

The AI policy taxonomy has five layers — principles, binding policy, standards, procedures, work instructions — and each does exactly one job. Principles are the board-ratified stance, anchored to OECD, UNESCO, and NIST AI RMF framings, reviewed every 2–3 years, un-waivable. Binding policy is the `shall`-language corporate document, board- or ExCo-ratified, reviewed annually, waivable only via a rare board-level exception. Standards are the `must`-language normative documents the level-50 architect authors, ratified by the AI Governance Council on a 12–18 month cadence, waivable through the documented exception workflow. Procedures are imperative, role-owned, reviewed every 6–12 months, and use change requests rather than waivers. Work instructions are step-by-step, team-owned, revised on change, and use defect reports rather than waivers. Authority decreases and change velocity increases as you go down; the exception path shortens correspondingly. Get the layering right and the AI programme has both stability at the top and agility at the bottom. Get it wrong — collapse standards into policy, use `shall` everywhere, or review on calendar alone — and every downstream artefact, including the control library from mod-102, inherits the flaw.
