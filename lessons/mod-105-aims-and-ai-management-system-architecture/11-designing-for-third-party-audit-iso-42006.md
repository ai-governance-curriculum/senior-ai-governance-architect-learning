# Designing for third-party audit — reading ISO/IEC 42006 as the auditor's rulebook

## Why this chapter exists

ISO/IEC 42006 is the standard that sets *requirements for the certification bodies* that audit AI management systems. The enterprise being audited does *not* implement 42006 — the certification body does. But the architect designing the enterprise's AIMS reads 42006 *in reverse*: what will the auditor be told to look for, at what sampling depth, with what competence, at which stage of the audit, over how many days of on-site work? The AIMS that survives a 42006-shaped audit is the AIMS the architect is trying to design.

This chapter reads 42006 as the auditor's rulebook, describes the two-stage certification audit the enterprise walks through, names the specific artefacts and behaviours the certification body will sample, and gives the architect the pre-certification readiness checklist that a first-time 42001 certification enterprise should run before the certification body arrives. The chapter is deliberately concrete — reading 42006 well is what separates enterprises that pass their first stage-2 audit from enterprises that discover, on the third day of the audit, that they should have been building differently for the last two years. <!-- needs-research: confirm the final publication date and specific requirements structure of ISO/IEC 42006:2025 against the published standard. -->

## What ISO/IEC 42006 is

ISO/IEC 42006:2025 — *Information technology — Artificial intelligence — Requirements for bodies providing audit and certification of AI management systems* — is a *conformity-assessment* standard. It is the AI-specific specialisation of ISO/IEC 17021-1 (the general requirements standard for bodies providing audit and certification of management systems) — the same pattern ISO/IEC 27006 follows for ISMS certification bodies and ISO 22003 follows for food-safety-management-system certification bodies.

Its subject is not the enterprise. Its subject is the certification body — the accredited third-party organisation (in most jurisdictions accredited by a national accreditation body under IAF Multilateral Recognition) that provides audits leading to certification against ISO/IEC 42001. It specifies:

- **Auditor competence** — what the auditors that a certification body sends to an AIMS audit must know and be able to do. AI-specific competence overlays on top of general management-system auditor competence.
- **Audit-team composition** — how a certification body assembles an audit team, when specialist expertise is required, how technical experts (non-auditor subject-matter specialists) participate.
- **Audit duration** — how many auditor-days a stage-1 and stage-2 audit require, based on the enterprise's size, scope complexity, and AI-specific factors. This is worked from the ISO/IEC 27006 duration model adapted for AI.
- **Sampling methodology** — how the certification body samples the AIMS's controls, systems, and evidence to reach an audit opinion.
- **Certification decision-making** — how the certification body's certification-decision function (separate from the audit team) reviews audit outputs and issues, maintains, suspends, withdraws, or refuses certification.

The enterprise reads 42006 to understand what the auditor is *required* to do. The AIMS is designed so the required audit activities land on evidence that is present, accessible, and defensible.

## The two-stage audit — stage 1 and stage 2

The 42001 certification audit follows the two-stage model that ISO/IEC 17021-1 mandates and 42006 specialises. The architect designs the AIMS with both stages in mind.

**Stage 1 — readiness audit.** Purpose: to confirm the AIMS is *sufficiently mature* to proceed to stage 2. The certification body reads the documented AIMS (scope statement, SoA, risk-treatment plan, competence and awareness plans, internal audit programme, management-review records, CAPA register, and any related documented information). The auditor conducts a short on-site visit (typically one or two days depending on enterprise size) to confirm the AIMS is *implemented* and not just documented. Stage 1 outputs are:

- A confirmation that stage 2 can proceed, or a list of preconditions (issues to close before stage 2 can be conducted).
- A stage-2 audit plan — which clauses will be sampled at which depth, which systems and organisational units will be visited, over what number of on-site days.
- A stage-1 report the enterprise reviews.

Enterprises that fail stage 1 typically fail on: (a) missing artefacts (no SoA, no internal audit programme, no management-review record), (b) artefacts that exist but have never been operated (SoA drafted but never approved by named authority; internal audit programme designed but no round completed), or (c) scope-statement issues (scope too vague, too broad, or inconsistent with the SoA).

**Stage 2 — certification audit.** Purpose: to *verify AIMS conformance*. The certification body sends an audit team to the enterprise for a defined number of on-site days (four to ten typical for a mid-size enterprise; more for large or complex ones), samples the AIMS across scope, and reaches an audit opinion. Stage 2 outputs are:

- Findings classified as major nonconformities, minor nonconformities, or observations.
- A stage-2 report the enterprise responds to (typically with a corrective-action plan for nonconformities within 90 days).
- A recommendation to the certification body's certification-decision function — issue certificate, defer pending closure of nonconformities, or refuse.

The audit team writes the report and the recommendation; the *certification-decision function* (a separate body within the certification body, not the audit team) makes the actual decision to certify. This separation is a 17021-1 principle and 42006 preserves it.

**Surveillance and re-certification.** Certification is granted for a three-year cycle. Between years, the certification body conducts *surveillance audits* (at least annually, typically annually or semi-annually depending on risk) that sample a subset of the AIMS. At the end of the three-year cycle, a *re-certification audit* — similar in scope to stage 2 — decides whether the certificate is renewed.

## What auditors are told to look for

Under 42006 the auditors' competence and sampling requirements shape what they will actually do on-site. The AIMS is designed so the following audit behaviours find evidence.

**Behaviour 1 — reading the scope statement first, sampling by it thereafter.** The scope statement (Clause 4.3) is the first artefact the auditor reads and the boundary of their sampling. If the scope statement names product families, the auditor samples across them. If it names lifecycle stages, the auditor traces evidence across them. If it excludes something, the auditor tests whether the exclusion is real. Chapter `02-scope-context-and-the-23053-reference-architecture.md` argued the scope statement's authoring discipline; this is the audit behaviour it enables.

**Behaviour 2 — walking the SoA row by row.** For each Annex A control marked included, the auditor selects a sample of systems and traces the control's implementation from SoA row → enterprise control library entry → evidence contract → produced evidence → validation record. For each Annex A control marked excluded, the auditor reads the justification and either accepts or challenges it. Chapter `05-statement-of-applicability-and-annex-a.md` argued the SoA's authoring discipline; this is the audit behaviour it enables.

**Behaviour 3 — tracing risks to treatments to controls to evidence.** The auditor samples the risk register, selects a risk, and traces it through the RTP (treatment selected, controls implementing it, owner, timing) to the SoA (which rows the treatment moves toward `in-place`) to the enterprise control library (specific control entries) to the produced evidence on a sampled system. Anywhere the trace breaks — a risk without a treatment, a treatment without a control, a control without evidence, evidence without validation — is a finding. Chapters 04–06 designed the interlocks that keep the trace unbroken.

**Behaviour 4 — reading the AIA process and sampling AIAs.** Under Clause 6.1.4 / 8.2 the AIA process must be defined and exercised. The auditor reads the process, samples a set of systems in scope, and asks for their AIAs. Absent AIAs (for systems that should have them per the process's triggers), stale AIAs, or AIAs whose outputs do not feed the risk register or the RTP are all findings. Chapter 04 designed the AIA process.

**Behaviour 5 — sampling the internal audit programme and management review.** The auditor asks for the internal audit programme, the last cycle's audit reports, the finding register, and the CAPA record. They ask for the management-review record — actual minutes, actual attendance, actual decisions, actual actions. A management review that took place on paper but not in substance is discovered quickly. Chapter 08 designed the review shape.

**Behaviour 6 — interviewing role holders across the workforce.** The auditor interviews the head of AI governance and AIMS role register holders. They also interview at random across the workforce — Tier-1 awareness targets. If a random employee cannot answer "what would you do if you encountered an AI concern?" in one sentence, Clause 7.3 has failed. Chapter 07 designed the awareness programme.

**Behaviour 7 — sampling third-party governance and incident-response.** The auditor asks for the third-party AI provider register (mod-109), samples a provider, and traces the assessment and ongoing monitoring evidence. They ask for the incident register (mod-110), sample an incident, and trace it through classification, containment, root-cause, corrective-action, and closure. Both are areas where AIMS integration with adjacent programmes must be visible; both are common finding areas.

**Behaviour 8 — reading the CAPA register for trend and maturity.** The auditor reads the CAPA register to understand how the AIMS handles nonconformities. An empty register is a red flag; a register full of unclosed items is a red flag; a register with a healthy flow of open, in-progress, and closed items with real root-cause analyses and effectiveness reviews is a good signal. Chapter 09 designed the CAPA process.

## Sampling methodology — what "sufficient sampling" means

42006 specialises 17021-1's sampling guidance for the AI context. Two AI-specific factors expand what the auditor samples:

**Factor 1 — the AI-system population.** The auditor samples systems in scope. Sampling size depends on the total population of AI systems in scope, the tiering (high-risk systems sampled more heavily), and the diversity (different system classes, different lifecycle stages, different organisational units, different geographies). A large, diverse AI portfolio produces a larger sample than a small, homogeneous one.

**Factor 2 — the evidence population per system.** For each sampled system, the auditor samples evidence produced against the control library. Sampling depth per system depends on the control's regulator-facing tier, the system's risk classification, and the auditor's confidence built during earlier audit activity.

Sampling is *risk-based*, not random. Higher-risk systems and higher-risk controls get deeper sampling. The auditor documents the sampling plan and the rationale; the enterprise sees this in the stage-1 audit plan and the stage-2 finding report.

## Auditor competence — what the audit team knows

Under 42006 the audit team collectively has competence in the AI-specific domains 42001 covers. Individual auditors are not required to be experts in every AI subdomain; the *team* is. Typical competence areas include:

- ISO/IEC 42001 management-system audit — the shape of the AIMS clauses.
- AI system life-cycle — how AI systems are designed, developed, evaluated, deployed, operated.
- AI risk management — the 23894 taxonomy and impact-analysis approach.
- AI impact assessment — the 42005 methodology.
- Data governance for AI — the 5259 data-quality frame and adjacent obligations.
- Sector-specific AI domain competence where the enterprise's portfolio requires it (medical AI, financial AI, employment AI).

Where the audit team lacks specific competence, 42006 allows *technical experts* — non-auditor specialists who provide subject-matter input during the audit. The enterprise may see a technical expert on the audit team who does not conduct auditing but consults with the audit team on, e.g., interpretability of a medical-AI evaluation report.

The enterprise's own internal audit function should be developed to a *comparable* competence baseline — chapter `08-performance-evaluation-internal-audit-and-management-review.md` walked why. Comparable does not mean identical, but internal audit findings that materially miss what the certification body catches produce a Clause 9.2 nonconformity.

## The pre-certification readiness review

Enterprises approaching first-time certification should conduct a *pre-certification readiness review* — an internal or co-sourced simulation of a stage-1 audit run six to twelve months before the real one. The purpose is to catch structural gaps early. The architect designs the readiness-review scope and produces a readiness-review checklist that walks the auditor's behaviours above.

**A readiness-review checklist (excerpt).**

```markdown
# AIMS Pre-Certification Readiness Review

## 1. Documented information — completeness
- [ ] AI policy exists, approved by AI-accountable executive, current-dated.
- [ ] Scope statement exists, current-dated, references 22989 and 23053.
- [ ] SoA exists, current-approved, walks every Annex A control, no dangling references.
- [ ] Risk-treatment plan exists, joined bi-directionally to SoA and risk register.
- [ ] Risk register exists, refreshed within the last quarter.
- [ ] AIA process documented; at least one AIA per system-tier sampled.
- [ ] Competence matrix exists per AIMS role.
- [ ] Awareness programme exists with completion tracking.
- [ ] Communications plan exists.
- [ ] Documented-information register exists and is current.
- [ ] Internal audit programme documented; at least one round completed.
- [ ] Management-review record exists for the most recent cycle with decisions and actions.
- [ ] CAPA register exists with active entries.

## 2. Documented information — quality
- [ ] Scope statement passes the "auditable" test — an auditor could sample by it.
- [ ] SoA justifications are real — no photocopy of Annex A, no bare "included".
- [ ] Every included SoA row maps to at least one enterprise control library entry that exists at the cited version.
- [ ] Every exclusion cites a role-based, scope-based, or risk-based pattern with named trigger for re-inclusion.
- [ ] RTP entries have closure criteria and residual-risk fields.
- [ ] Every AIA links to a risk register entry and to RTP entries.
- [ ] Competence records exist for each AIMS role holder at the current cycle.
- [ ] Awareness completion by tier is above target (or a CAPA is open on the gap).
- [ ] CAPA entries have root-cause analysis and effectiveness-review plans.

## 3. Interlocks — trace tests
- [ ] Pick a risk. Trace: risk register → RTP → SoA → enterprise control library → evidence → validation. Trace is complete.
- [ ] Pick a system. Trace: SSP → controls in scope → evidence produced against each → linked back to enterprise control library and SoA rows. Trace is complete.
- [ ] Pick an AIA. Trace: AIA → risk register entries created → RTP entries created → closure evidence. Trace is complete.
- [ ] Pick a CAPA. Trace: source (audit, incident, other) → nonconformity described → immediate action → root cause → corrective action → preventive action → effectiveness review. Trace is complete.

## 4. People and behaviour
- [ ] Interview head of AI governance. Can they walk the AIMS end-to-end? Do they know all six inseparable artefacts?
- [ ] Interview five AIMS role register holders. Do they know their responsibilities and authorities?
- [ ] Interview ten random employees across functions. Do they know the AI policy exists and who to escalate to?
- [ ] Attend an AIMS operational review. Does it produce decisions and actions?
- [ ] Review last management-review minutes. Are decisions specific, owned, dated?

## 5. Integration with the ISMS
- [ ] Integrated management-system documented-information register is a real integrated register, not two side-by-side.
- [ ] Integrated audit programme covers both facets in a single plan.
- [ ] Integrated management review has real cross-facet inputs and outputs.
- [ ] AIMS facet has its own scope, SoA, policy, and risk process (i.e. is not collapsed into the ISMS).
```

Every item on this list should be `[x]` before the certification body arrives for stage 1. Items left `[ ]` are the audit's likely findings.

## Two failure modes at the certification threshold

**Failure mode 1 — the enterprise that reads 42001 but not 42006.** The enterprise designs its AIMS from a careful read of ISO/IEC 42001. The AIMS is well-shaped. But it was not designed for *audit sampling*. Trace tests fail: risks do not connect to treatments; treatments do not connect to controls; controls do not have evidence in the places the auditor samples. The certification body finds material nonconformities on day two. The fix is early reading of 42006 — reverse-engineering the audit behaviours into the AIMS's design from the start.

**Failure mode 2 — the enterprise that treats stage 1 as a dry run of stage 2.** Stage 1 is not stage 2 with a lower stakes. It is a specific reading of the AIMS documented information plus a short implementation check. Enterprises that treat it as informational and hold back preparation for stage 2 discover that the stage-1 findings are structural — they require weeks or months to address — and stage 2 has to be deferred. The fix is *stage 1 preparation as if it were stage 2* — full readiness before the stage-1 auditor arrives.

## Summary

ISO/IEC 42006:2025 sets the requirements the AIMS certification body operates under — auditor competence, audit-team composition, audit duration, sampling methodology, certification-decision separation. The architect reads it *in reverse* to design an AIMS that survives the audit behaviours 42006 mandates: scope-statement-first sampling; SoA walk row-by-row; risk-treatment-plan-to-control-to-evidence trace; AIA process and sampling; internal-audit-and-management-review inspection; awareness-programme workforce interviews; third-party and incident-management sampling; CAPA-register trend reading. The two-stage audit (stage-1 readiness, stage-2 certification) and the three-year surveillance-and-recertification cycle set the cadence the enterprise plans for. The pre-certification readiness review is the architect-designed simulation the enterprise runs six-to-twelve months before the real audit to catch structural gaps early. Enterprises that read 42006 well design AIMSes that certify on first attempt; enterprises that read only 42001 design AIMSes that pass the paper test and fail the sampling test. With this chapter the module closes: an AIMS that is architecturally sound (chapters 01–02), planned and risk-driven (chapters 03–04), soundly scoped in the SoA (chapter 05), operationally managed through the RTP (chapter 06), supported through people and documented information (chapter 07), performance-evaluated through internal audit and management review (chapter 08), continually improved through CAPA (chapter 09), integrated with the ISMS (chapter 10), and designed for the third-party audit under 42006 (chapter 11) is the AIMS the certified enterprise operates.
