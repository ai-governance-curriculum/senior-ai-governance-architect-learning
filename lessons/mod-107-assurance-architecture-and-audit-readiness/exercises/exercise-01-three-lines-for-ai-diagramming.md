# exercise-01: Three-Lines-for-AI Diagramming

**Estimated effort:** 3 hours

## Objective

Produce the **three-lines-of-defence architecture for AI** at a specified enterprise scenario — the artefact you would take to the AI-accountable executive to ratify *before* the pre-deployment gate, the ongoing programme, and the third-line audit programme can each be executed against it.

The deliverable is a decision document, a per-system three-line ownership schematic, and a defensible independence matrix. Downstream artefacts in this module (exercise-02 pre-deployment gate, exercise-03 ongoing cadence, exercise-04 third-line audit programme, exercise-05 external interface, exercise-06 coordination contracts) all bind to the shape you draw here. Get it right and every subsequent exercise composes; get it wrong and every subsequent artefact inherits the flaw.

## Prerequisites

- Chapter [`01-the-three-lines-of-defence-for-ai.md`](../01-the-three-lines-of-defence-for-ai.md) read once, with the six invariants and the two failure modes marked.
- The mod-105 chapter 08 walk-through of the AIMS internal audit as a Clause 9.2 artefact (the third-line audit programme in this module extends it).
- The mod-101 chapter 06 walk-through of engagement contracts between the level-50 architect, the level-60 head, and adjacent roles.
- Access to primary references — IIA Three Lines Model (2020), SR 11-7 (2011) and OCC 2011-12, ISO/IEC 42001:2023 Clause 5 and Clause 9.2, ISO/IEC 42006:2025 (title level enough if paywalled). See [`../resources.md`](../resources.md).

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose three-line shape you are least familiar with; that is where the exercise will teach you most. State your choice at the top of the deliverable.

- **A US regional bank** (Northbrook Financial-style, per mod-101 exercise-02 and mod-102 exercise-01) with ~40 AI systems: fraud classifiers, document extraction, an internal RAG legal assistant, a customer-facing generative chat, third-party AI SaaS integrations. Colorado + NYC deployments; EU expansion planned. An SR 11-7-aligned MRM programme and an ISO/IEC 27001 ISMS already in place; internal audit function reports to the audit committee.
- **A global healthcare payer / provider** with clinical-decision-support pilots, patient-facing chat, coding automation, and utilisation-management AI. US federal HIPAA scope; EU AI Act relevance for European insurance subsidiaries; multiple US state deployments including Illinois and California. A classical clinical-safety oversight committee reports to the CMO; internal audit reports to the audit committee.
- **A B2B SaaS platform vendor** shipping GenAI-augmented HR-tech capabilities into enterprise customers across the US, UK, EU, and Singapore. Customers include public-sector deployments (subject to OMB M-25-21 shape) and financial-services enterprises (subject to SR 11-7 vendor-review shape). Internal audit is a small function co-sourced with a Big Four provider.

## Deliverables

Author three artefacts in a working directory of your choice.

1. **`three-lines-architecture.md`** — the decision document.
2. **`per-system-ownership-schematic.yaml`** — the machine-readable per-system, per-line ownership registry for at least eight representative systems from the scenario.
3. **`independence-matrix.md`** — the matrix that shows every role in the assurance ecosystem and, for each role and each line, whether the role is on-line, off-line, or requires-fence-out-per-activity.

## Requirements

### `three-lines-architecture.md`

Decide and justify **each** of the following:

- **First-line specification.** The seats that carry first-line AI-assurance responsibilities at your scenario. For each: named role, primary outputs, quality bar, escalation upward. Distinguish shared-service seats (platform, MLOps) from per-system seats (model owner, product owner).
- **Second-line specification.** The seats that carry second-line responsibilities. Include the governance office, the AI risk-engineering function (the `ai-risk-engineer` role at level 25 in this track's role tree), the AI evaluation function (the peer-level `ai-evaluation-engineer` at level 35), and — where applicable — the MRM function under SR 11-7. State how you resolve the MRM / AI-evaluation overlap for the bank scenario; for the healthcare scenario, resolve the clinical-safety-committee interface; for the B2B SaaS scenario, resolve the customer-facing assurance-report obligation.
- **Third-line specification.** The internal audit function's AI-scope shape at your scenario. State the reporting line (functional and administrative), the competence pattern chosen from A / B / C (development, co-sourcing, external contract) per chapter 04's preview, and the audit-committee reporting cadence.
- **External-providers register.** Enumerate the external providers your scenario faces (see chapter 05 for the five types) and name the enterprise's audit-liaison seat that owns each.
- **Six-invariant enforcement.** For each of the six invariants from chapter 01, name the specific design choice in your architecture that enforces it and the test that would detect its violation.
- **Failure-mode defence.** For each of the two chapter-01 failure modes (collapsed lines, theatrical lines), name at least one architectural move you have made to prevent it in your scenario.
- **Change-control.** Describe the change-control procedure for the architecture itself — how a new role is added, an existing role's line assignment is changed, an external-provider relationship is added or wound down.
- **Non-scope.** At least three things you *chose not to* include in the architecture and why. Candidates: an "AI ethics board" as a separate line (belongs inside second-line or as an advisory to the AI-accountable executive, not on its own line); a "fourth line" of external assurance (belongs in the external-providers register per chapter 01); a duplicate MRM programme for models already covered by classical MRM.

### `per-system-ownership-schematic.yaml`

Enumerate at least eight representative systems from your scenario. For each system, produce an entry:

```yaml
system:
  id: SYS-2027-0042
  name: <system name>
  tier: <tier from your enterprise scheme>
  first_line:
    model_owner: <seat>
    product_owner: <seat>
    platform_dependencies: [<shared-service seats>]
  second_line:
    governance_reviewer: <seat>
    ai_risk_engineer_scorer: <seat>
    ai_evaluation_engineer: <seat>
    mrm_reviewer: <seat or n/a>
  third_line:
    internal_audit_engagement_lead: <seat>
    co_source_partner: <partner or n/a>
  scope_boundaries:
    - <boundary description; e.g. platform team is first-line for the ingest but
      third-party frontier-model provider is out-of-scope for internal audit and
      covered via mod-109 third-party governance>
```

At least one system in your set must be one where invariant 3 (per-line ownership) is currently *unmet* — a system in the AI inventory with no named second- or third-line owner. Name the gap explicitly and specify the remediation timeline you propose.

### `independence-matrix.md`

For every role in your assurance ecosystem (first-line seats, second-line seats, third-line seats, external-provider liaisons, adjacent governance seats — legal, compliance, CRO, CISO, CDO, general counsel), fill a matrix:

|  | 1st line | 2nd line | 3rd line | Notes |
|--|--|--|--|--|
| Role name | on / off / fenced | on / off / fenced | on / off / fenced | |

- **on** — the role is inside this line for the specified activity.
- **off** — the role is not on this line.
- **fenced** — the role occasionally participates on this line, but under an activity-specific fence-out discipline the matrix describes (e.g. the head of AI governance may attend a third-line audit-committee reporting meeting for context but is not on the third line and does not review findings before they land with the audit committee).

Every "fenced" cell requires a note describing the fence-out. Every role that appears in more than one "on" cell requires a defensible explanation.

## Starter guidance

- Draft the decision document *before* the schematic and matrix. The document forces the trade-offs; the artefacts instantiate them.
- Do not conflate reporting line with assurance line. The head of AI governance is level-60 and reports to the AI-accountable executive but is *not* second-line for auditing the AIMS operations they run — the third-line internal audit function is.
- Do not push more than one AI-specific competence into a single role at the same line. If your architecture requires the internal audit lead to be *both* AI-evaluation-competent and ML-security-competent and sector-regulation-competent, you are hiding a competence gap. Name the gap and specify the co-source / development plan.
- Resist the urge to add a "fourth line." IIA 2020 specifies external providers, not a fourth line; adding a fourth line is a signal you are trying to duplicate what chapter 05 pins as the external interface.
- For the bank scenario, the MRM function's AI-scope overlap with the AI-evaluation function is the most consequential decision; do not paper it over.
- For the healthcare scenario, the clinical-safety oversight committee is not third-line even though it produces safety findings; it is a specialised first-line-or-second-line committee. Name where you place it and defend.
- For the B2B SaaS scenario, the customer-facing assurance-report obligation to enterprise customers (SOC 2 shape; AI-specific attestation; enterprise DPA appendices) is a first-line output but with second-line quality gating; specify the interface.

## Acceptance criteria

- [ ] Scenario is stated at the top; architecture is coherent against it.
- [ ] `three-lines-architecture.md` decides all eight requirements bullets, each with stated rationale.
- [ ] At least eight systems in `per-system-ownership-schematic.yaml`, each with full first-line / second-line / third-line seats named. At least one system has a currently-unmet invariant-3 gap named with a remediation plan.
- [ ] `independence-matrix.md` enumerates every ecosystem role. Every "fenced" cell carries a fence-out note. Every role in multiple "on" cells has a defence.
- [ ] Each of the six chapter-01 invariants has a named design-choice-plus-detection-test.
- [ ] Each of the two chapter-01 failure modes has at least one architectural defence.
- [ ] Non-scope section names at least three things deliberately excluded and why.
- [ ] Every unverified citation to IIA, SR 11-7, ISO 42001, ISO 42006, or a scenario-specific regulation is marked `<!-- needs-research: ... -->` — no invented dates, clause numbers, or agency names.

## Stretch goals

- Add a *cross-line escalation map* — for each of the four escalation types from chapter 02 (residual within tolerance-band boundary, stop-shipping breach, evidence-contract failure, regulatory-posture ambiguity) draw the routing across the three lines and to external providers where relevant.
- Sketch how the architecture changes if the enterprise acquires a subsidiary with an *existing* AI programme under a different assurance shape (e.g. the bank acquires a fintech with a lightweight AI oversight committee but no third-line audit programme for AI); describe the integration-vs-replacement decision.
- Extend the independence matrix with a *maturity column* naming, per role, the enterprise's current competence maturity on a 1–5 scale and the target maturity at 18 months. Preview of the mod-112 operating-model chapter.
- Add a *comparable-enterprise reference* — pick one of the frontier-lab tiered-risk frameworks (Anthropic RSP, OpenAI Preparedness Framework, Google DeepMind Frontier Safety Framework) or a published enterprise assurance architecture and note two shape moves you adopted and two you rejected and why.
