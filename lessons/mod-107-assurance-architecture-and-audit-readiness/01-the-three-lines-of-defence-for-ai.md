# The three lines of defence for AI — designing the assurance architecture

## Why this chapter exists

An enterprise AI programme without an *assurance architecture* collapses into two failure modes with predictable speed. In the first, everyone thinks they own assurance: the model owner tests, the governance analyst checks the tests, the head of AI governance signs off, internal audit reviews the sign-off, the certification body reviews the internal audit. Every layer inspects the layer above and no independent view is ever taken; every finding is deflected downward. In the second, no one owns assurance: the model owner ships, the governance analyst files the paperwork, internal audit shows up once a year with a checklist, the certification body sees polished documentation and finds nothing because it looked at the documentation and not at the systems. Both failures are architectural, not operational. Both are prevented by the same architectural move — a clean *three lines of defence* structure adapted for AI.

The level-50 architect owns the design of that structure. This chapter names the three lines, the AI-specific specialisation each takes, and the invariants that must hold for the architecture to be defensible under an ISO/IEC 42001 certification audit, a US federal SR 11-7 examination, a ForHumanity independent-auditor engagement, or a sector regulator's examiner arriving with subpoena power. The chapter is the anchor for the rest of the module — the pre-deployment gate (chapter 02), the ongoing assurance programme (chapter 03), the third-line audit (chapter 04), the external-audit interface (chapter 05), the coordination contracts (chapter 06), and the RMF composition (chapter 07) are all instances of the shape this chapter fixes.

## What "three lines of defence" is, and what it is not

The IIA Three Lines Model (2020 revision of the older Three Lines of Defence) is a governance-shape reference that names three distinct roles inside an organisation whose independence relationships determine whether the organisation can defensibly assert that it is *governed*:

- **First line — operational management.** The roles that own the risk-taking activity itself. In an AI context: model owners, product managers, MLE / MLOps, data scientists, the platform team. The first line *does the thing*. Its assurance responsibility is *self-assessment* — the model owner running their own evaluations before ship, the platform team running its own reliability tests. First-line self-assessment is necessary and not sufficient; on its own it is what produces the second failure mode.
- **Second line — risk and compliance functions.** The roles that *design and monitor the risk framework* the first line operates inside. In an AI context: the governance office (the head of AI governance, the level-50 architect, the analysts), the AI risk function (the `ai-risk-engineer` at level 25 scoring against the taxonomy), the model-risk-management function (SR 11-7-style validation team where applicable), the AI evaluation function (the peer-level `ai-evaluation-engineer` at level 35 running independent evaluations). The second line *inspects the first line* and *runs an independent view*, but it is still inside the operating structure — it does not report to the board independent of management.
- **Third line — internal audit.** The role that *provides independent assurance to the board* that the first and second lines are working. Third-line internal audit reports functionally to the board's audit committee, not to management. It samples the AIMS's execution, the risk-treatment plan's discharge, the evaluation programme's rigor, the incident-management programme's completeness — and reports findings up to the audit committee where management cannot suppress them.

Outside the three lines sit the *governing body* (the board, its audit committee, its risk committee) — which does not do assurance itself but *consumes* assurance from the third line — and *external assurance providers* (certification bodies, statutory auditors, ForHumanity independent auditors, sector regulator examiners) which provide independent third-party views the board and the market can trust. Some authors call these external providers "the fourth line" — the IIA 2020 model calls them *external assurance providers* to preserve the distinctness of internal audit as the board's own third-line provider. Chapter 05 walks the interface with the external providers.

The model is *not* a hierarchy of quality control (where each layer double-checks the one below). It is a *specialisation of independence*: each line's independence relationship to the others is what makes the whole defensible. Collapse any pair of lines and the architecture loses its explanatory power under audit — an internal auditor reporting to the head of AI governance is not third-line; a second-line evaluation team that shares OKRs and reports with the first-line model owner is not second-line; a first-line self-assessment counted as a second-line assurance finding is a category error the certification body will spot immediately.

## Why AI needs a specialisation of the three lines

Three lines of defence is a generic governance shape. AI adds specific structural pressures that force the architecture beyond the generic:

**Pressure 1 — the specialist competence problem.** The classical model risk management shape (SR 11-7, OCC 2011-12) assumes the second-line validators can competently re-derive the first-line model owner's work. For a linear regression that assumption holds. For a frontier-model-fine-tuned agentic system with tool use, RAG, and multi-turn planning, the second-line validators without deep ML competence cannot competently re-derive; the third-line audit without deep ML competence cannot competently sample. The architecture must specify *where the competence lives*, *how it is renewed*, and *what happens when the required competence is not resident in the enterprise* (co-sourcing, contracting, buying it in). Chapter 06 walks the coordination contract with the `ai-evaluation-engineer` role that carries much of this competence in the second line; the third-line audit programme (chapter 04) walks the parallel competence question for internal audit.

**Pressure 2 — the model-as-product boundary problem.** In classical MRM the boundary between "the model" (what the second line validates) and "the product it sits inside" (what the first-line owns end-to-end) is usually clean. In a modern GenAI system it is not. The model is the frontier-model provider's model, hosted at the frontier-model provider's endpoint, fine-tuned by the platform team, called by the product with retrieval and tools, shaped by the prompt at inference time. Every one of those boundaries is a first-line / second-line / third-line coordination point. The architecture must name the boundaries and the ownership on each side of each boundary explicitly, or every ambiguity resolves in favour of the first line (who owns nothing they can be held accountable for) and against the second and third (who can find nothing they can pin to an owner).

**Pressure 3 — the post-deployment surveillance obligation.** Regulatory obligations under the EU AI Act (Article 72 post-market monitoring), under FDA guidance for AI/ML-enabled medical devices, under the Colorado Consumer Protections for AI Act (SB24-205) risk-management-programme requirements, and under sector regulator expectations increasingly require *ongoing* assurance rather than one-time pre-deployment attestation. The architecture must fold the classical pre-deployment gate into an *assurance system* that includes periodic re-assessment, drift-driven re-assessment, and incident-driven re-assessment — with clear ownership across the three lines for each of those triggers. Chapter 03 walks this in detail.

**Pressure 4 — the external-audit-body proliferation problem.** An enterprise deploying AI at scale may within a year be audited by an ISO/IEC 42001 certification body (under 42006), by a ForHumanity-accredited independent auditor (under IAAIS), by a sector regulator's examiner (under the Fed's SR 11-7 examination programme; under a state insurance commissioner's model-audit programme; under a European member-state notified body's conformity assessment), and by the enterprise's own statutory financial auditors (where AI touches material financial-reporting processes). Each has a different scope, competence baseline, and reporting audience. The architecture must position the third line's internal audit programme to *feed* each of these external engagements without turning the enterprise into a full-time audit-response function. Chapter 05 walks the packaging and remediation-plan shape.

Pressures 1 through 4 make the AI three-lines architecture a *composition* — of IIA's Three Lines Model as the base shape, SR 11-7's model risk validation-vs-development independence as the AI-specific independence requirement for the second line, and ISO/IEC 42006's audit-body competence requirements as the specialisation the third line must be competent to survive an external certification audit against.

## The AI-specific specialisation, line by line

### First line — model owners, platforms, product

The first line for AI carries a set of assurance responsibilities the architecture defines and pins:

- **Self-assessment against the enterprise control library.** The first-line team implements the controls the architect designed in mod-102 and produces the evidence the mod-108 evidence contract demands. They *test* the controls — the model owner runs their pre-ship evaluation suite; the platform team runs its reliability tests; MLOps runs the drift monitors. First-line self-assessment produces the raw material the second line inspects.
- **Model card / system card authorship.** The first line authors the model-card / system-card style documentation that mod-108 will fix the shape of. The card is a first-line artefact even though the second line reviews it — the model owner owns the claims and the evidence backing them.
- **Impact assessment authorship.** Where an AI Impact Assessment fires (per mod-105 chapter 04), the first-line team authors the initial draft. The second line reviews.
- **Incident triage.** First-line detects and triages AI incidents; second-line classifies severity against the taxonomy; third-line samples the incident-response process for effectiveness. Chapter 03 walks the incident-driven re-assessment trigger; mod-110 walks the post-market surveillance shape.

The first line's assurance authority is *bounded* — the first line cannot self-certify readiness for launch. That is the pre-deployment gate's job (chapter 02), which is a second-line-owned artefact even though first-line evidence is what fills it.

### Second line — governance office, MRM, evaluation, risk

The second line for AI carries the independent-view responsibility. Its architecture must satisfy three independence requirements:

- **Independence from development for validation (SR 11-7 requirement).** The team that validates a model is not the team that developed it. In banking under SR 11-7 this is a hard boundary; in the AI context outside banking the same principle applies — the pre-deployment assurance decision cannot be signed by the person who trained the model.
- **Reporting-line independence from first-line management.** The governance office reports to the AI-accountable executive (mod-105 chapter 03), not to the head of engineering or the head of product whose delivery pressure would compromise assurance rigour. This is why the head of AI governance sits at level 60 in this track's role tree, not embedded in engineering.
- **Analytical independence — the ability to reach a different conclusion.** The second line must be resourced to *actually* re-run evaluations, sample training data, re-derive fairness measurements — not just accept the first line's evidence and stamp it. Chapter 02 walks the specific evidence artefacts the pre-deployment gate re-derives independently.

Roles typically sitting in the second line for AI:

- **The governance office.** The head of AI governance (level 60), the level-50 architect (this role), the level-15 governance analysts. Owns the AIMS operations; runs the pre-deployment gate as a second-line assurance forum; owns the risk-treatment plan discharge.
- **The AI risk-engineering function.** The `ai-risk-engineer` (level 25) scoring against the taxonomy (mod-106); the `ai-risk-quantification` and portfolio-view functions.
- **The AI evaluation function.** The `ai-evaluation-engineer` (peer, level 35) running independent evaluations, calibrating scoring methodologies, executing release-assurance methodology inside the pre-deployment gate. Chapter 06 walks this coordination in detail.
- **The MRM function (where SR 11-7 applies).** The classical model-validation team, which for AI models now overlaps with the AI evaluation function. Enterprises resolve the overlap either by (a) folding MRM's AI scope into the AI evaluation function under a shared reporting line, or (b) keeping a distinct MRM function under the CRO with a coordination contract with the AI evaluation function. The architect designs which.

### Third line — internal audit

The third line for AI is internal audit specialised for AI. Its architecture must satisfy the independence and competence requirements the IIA 2020 model, SR 11-7 (which effectively mandates independent internal audit for MRM), and ISO/IEC 42006 (which sets the competence bar the internal audit function should match) impose:

- **Reporting to the audit committee, not to management.** Internal audit's functional reporting is to the board's audit committee. This is what makes it third-line. An internal audit function that reports to the CFO or the head of AI governance is not third-line.
- **Independence from first- and second-line activities audited.** The internal auditor auditing the AI evaluation function was not on the evaluation team. This is the auditor-independence discipline mod-105 chapter 08 fixed for the AIMS internal audit; the same discipline applies here.
- **AI-specific competence.** Chapter 04 walks the competence question in detail. Where the enterprise's internal audit function lacks AI competence, the architecture must specify how it is bought, co-sourced, or developed — leaving the gap unfilled produces the "internal audit without competence" failure mode mod-105 chapter 08 named.
- **Sampling authority across first- and second-line evidence.** Internal audit's sampling reaches into first-line evidence (does the control actually operate?) and second-line records (did the pre-deployment gate actually challenge the evidence, or rubber-stamp it?). The architecture must guarantee sampling access — including model-inspection, evaluation-run reproduction, training-data sampling where needed.

The third line does *not* own the pre-deployment gate (the gate is a second-line forum). It does not own ongoing assurance (that is second-line, with third-line sampling). It owns the *periodic independent programme* that samples the whole assurance architecture and reports to the board. Chapter 04 designs it.

## The invariants the assurance architecture holds

Six invariants must hold across the design. Each is testable; each has an associated failure mode.

**Invariant 1 — independence-per-line is preserved.** No role sits in more than one line for the same activity. The head of AI governance does not audit the AIMS operations they own; the model owner does not sign the second-line pre-deployment gate; internal audit does not develop the risk taxonomy. Failure mode: the architecture looks like three lines on paper but role-holders shuttle across lines; every finding is deflected by the perpetrator to a role they themselves occupy on another line.

**Invariant 2 — competence is resident somewhere.** For every AI-specific competence the assurance architecture requires — deep ML, evaluation methodology, red-team, ML security, adversarial-ML, human-factors of AI-augmented decisions, sector-specific AI regulation — the architecture names *where in the three lines it is resident* and *how it is renewed*. Empty competence is filled by contract, co-source, or development plan, not by hope. Failure mode: shallow findings because no one on the audit team could reproduce the evaluation. The pre-certification readiness review (mod-105 chapter 11) discovers this too late.

**Invariant 3 — every AI system in scope has a named owner on each line.** For a specific system, the first-line owner (the model / product owner), the second-line owner (the AIA / risk / evaluation reviewer), and the third-line owner (the internal audit engagement lead responsible for sampling the system) are all named. Systems without one of the three are *outside the assurance architecture* and must be either brought in or explicitly out-of-scope in the scope statement. Failure mode: the system sits in the AI inventory but no one on second- or third-line has sampled it in two years; when it breaks, the incident-response cannot find an authoritative owner on any line.

**Invariant 4 — evidence flows down-the-lines, not up.** First-line evidence flows to second-line for review; second-line assurance records flow to third-line for sampling; third-line findings flow to the board. The architecture does not require the first line to see third-line findings before they are reported to the board (this would neuter third-line independence). Failure mode: management-friendly findings only; the audit committee sees a pre-negotiated version of every third-line finding.

**Invariant 5 — external-provider access is a first-class pathway.** The certification body, the independent auditor, the sector regulator's examiner all have named access into the third-line's records and into the second-line's evidence, coordinated by a named enterprise role (typically the head of AI governance or a dedicated audit-liaison seat). Ad-hoc access — the auditor emails whoever they met last time — is not architecture. Chapter 05 fixes the packaging. Failure mode: external auditor engagements degrade to a series of scavenger hunts; findings become "we could not obtain the evidence" rather than substantive.

**Invariant 6 — the architecture is versioned and change-controlled.** Adding a new line role (e.g., a co-sourced audit partner joins the third line), reassigning a competence (the AI-evaluation function moves from Governance to Engineering), or changing an external-provider relationship is a versioned change to the assurance architecture with a defined review-and-ratify path. The board's audit committee sees the change. Failure mode: silent drift; the architecture on paper diverges from the architecture in practice, and the certification body catches the divergence at the next audit.

## A schematic of the AI three-lines architecture

```yaml
assurance_architecture:
  version: 1.4.0
  owner: senior-ai-governance-architect (level 50)
  ratifies: head-of-ai-governance (level 60); audit-committee (annually)
  first_line:
    principle: the roles that take the AI risk own the first cut of assurance
    seats:
      - model-owner (per system)
      - platform-team-lead (shared)
      - mlops-lead (shared)
      - product-owner (per product)
    outputs:
      - control-implementation-evidence (per mod-102/108)
      - model-card / system-card
      - ai-impact-assessment (initial draft, per mod-105 chapter 04)
      - incident-triage-record
  second_line:
    principle: independent view, independent reporting line to the AI-accountable executive
    seats:
      - governance-office (head, architect, analysts)
      - ai-risk-engineering (risk-engineer, quantification)
      - ai-evaluation-function (evaluation-engineer)
      - mrm-function-ai-scope (where SR 11-7 applies)
    independence_requirements:
      - development-vs-validation separation (SR 11-7 principle)
      - reporting to AI-accountable executive, not to first-line management
      - resourced to re-derive evidence, not just review
    outputs:
      - pre-deployment gate decision-record (chapter 02)
      - ongoing-assurance re-assessment record (chapter 03)
      - risk register with quantification (per mod-106)
      - AIA formal review record (per mod-105 chapter 04)
      - risk-treatment-plan discharge status (per mod-105 chapter 06)
  third_line:
    principle: independent internal audit reporting to audit committee
    seats:
      - internal-audit-lead (AI scope)
      - internal-audit-engagement-team (per audit)
      - co-source-partner (where competence gap requires; contracted)
    reporting_line: audit-committee (functionally); CEO (administratively)
    outputs:
      - annual audit-plan (chapter 04)
      - engagement-report per engagement (chapter 04)
      - CAPA feed to second-line (per mod-105 chapter 09)
      - audit-committee reporting deck (chapter 04)
  external_providers:
    principle: external assurance is a first-class pathway with named liaison and packaged evidence
    relationships:
      - iso-42001-certification-body (per ISO/IEC 42006)
      - forhumanity-independent-auditor (per IAAIS)
      - sector-regulator-examiner (per applicable regime)
      - statutory-financial-auditor (where AI touches financial reporting)
    liaison: head-of-ai-governance or dedicated audit-liaison seat
    packaging: see chapter 05
  invariants:
    - id: I1
      description: independence-per-line preserved
      test: no role appears on more than one line for the same activity
    - id: I2
      description: competence resident somewhere
      test: every AI-specific competence has a named seat or a named external partner
    - id: I3
      description: every in-scope system has a named owner per line
      test: cross-reference AI inventory with per-system three-line owner registry
    - id: I4
      description: evidence flows down-the-lines
      test: no third-line finding was pre-negotiated with first-line management before reporting
    - id: I5
      description: external-provider access is first-class
      test: named liaison, evidence-packaging convention, remediation-plan template all exist
    - id: I6
      description: architecture is versioned and change-controlled
      test: version history exists; last three amendments each cite ratification
```

Exercise-01 asks you to author the diagram equivalent of this schematic and defend it. The schematic is what a certification body wants to see when it asks "walk me through your assurance architecture" — a document, not a slide.

## The two failure modes to design against

**Failure mode 1 — the collapsed lines.** The enterprise nominally has three lines, but on any given AI system the same senior engineer appears as first-line model owner, second-line evaluation reviewer, and third-line audit sampler for the "high-priority reviews" that the audit committee has flagged. The engineer is competent and well-intentioned; the architecture is broken. When the certification body samples for independence, it finds none — every assurance layer for that system is the same person. The finding is systemic. The fix is *hard role separation*, enforced by an assignment registry that prevents cross-line seating for the same activity, with a documented exception process for the rare cases (a very small enterprise on a specific system) where exceptions must be permitted — and those exceptions are the audit committee's decision, not the operations team's.

**Failure mode 2 — the theatrical lines.** The three lines exist on paper with the right independence relationships and reporting lines. The pre-deployment gate meets weekly. The internal audit programme runs on schedule. The audit committee receives reports. But the second line rubber-stamps first-line evidence without re-derivation; the third line reads second-line records without sampling first-line evidence; the audit committee accepts internal audit reports without probing whether internal audit had the competence to reach the findings it did. The architecture is fully specified and fully hollow. The fix is *sampling depth* built into the design — the pre-deployment gate charter specifies re-derivation quotas (chapter 02); the internal audit programme's engagement scope specifies sampling depth against a defensible baseline (chapter 04); the audit committee reporting deck names *what was sampled and what was not* rather than reporting only what was found.

Both failure modes are common. Both are architectural, not operational. Both are the architect's to design against — this module walks the specific design moves each subsequent chapter contributes.

## Summary

The three lines of defence — first-line operational management, second-line risk and compliance, third-line internal audit — is the shape the enterprise AI assurance architecture takes. The AI specialisation composes IIA's Three Lines Model with SR 11-7's development-vs-validation independence and ISO/IEC 42006's audit-body competence expectations. Four AI-specific pressures (specialist competence, model-as-product boundary, post-deployment surveillance obligation, external-audit-body proliferation) force the architecture beyond the generic. Six invariants (independence per line, competence resident, per-system per-line ownership, evidence flow direction, external-provider access, versioned change-control) are testable and each has a failure mode. Two systemic failure modes (collapsed lines, theatrical lines) are common enough that the design must specifically protect against them. The chapter's schematic is the architect's authored artefact and the anchor for the rest of the module — chapter 02 designs the pre-deployment gate that discharges second-line assurance, chapter 03 designs the ongoing assurance programme, chapter 04 designs the third-line audit programme, chapter 05 fixes the external-provider interface, chapter 06 walks the coordination with the evaluation and analyst roles, and chapter 07 shows how NIST SP 800-37 RMF as a process shape composes with the whole.
