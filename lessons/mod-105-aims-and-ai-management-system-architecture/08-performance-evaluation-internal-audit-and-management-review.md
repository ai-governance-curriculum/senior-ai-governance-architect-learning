# Performance evaluation, internal audit, and management review — Clause 9 and the operations calendar

## Why this chapter exists

Clause 9 is where the AIMS *checks itself*. Clause 9.1 requires the organisation to monitor, measure, analyse, and evaluate the AIMS's performance and effectiveness. Clause 9.2 requires an *internal audit* programme that verifies the AIMS conforms to the standard and the organisation's own requirements. Clause 9.3 requires a *management review* at planned intervals to ensure the AIMS's continuing suitability, adequacy, and effectiveness.

These three sub-clauses are the AIMS's most visible cadence. The architect designs the calendar the operations run on — what happens monthly, what happens quarterly, what happens annually, what management sees, what internal audit tests, what evidence lands where. A well-designed calendar is legible to the head of AI governance (who runs it), to top management (who acts on it), and to the certification body (which samples it). A badly-designed calendar collapses into a series of decorative meetings that produce minutes but no decisions and no improvements.

## Clause 9.1 — monitoring, measurement, analysis, and evaluation

Clause 9.1 asks the organisation to determine *what* needs monitoring and measurement, *by what methods*, *when*, and *by whom*, with the results *evaluated* and *retained as documented information*.

The AIMS's monitoring surface is broad. It typically includes:

**AIMS-process metrics** — metrics about how the AIMS *itself* is running:

- Number and status of open RTP entries; entries closed and opened in the cycle; entries overdue.
- SoA coverage — Annex A controls with `in-place` status vs. `partially-in-place` vs. `planned`; trend since last review.
- Awareness completion — percent of workforce completed current awareness module by tier; overdue count.
- Competence records — percent of AIMS role holders with current competence assessment; overdue count.
- AIA throughput — AIAs authored, in review, accepted, and rejected in the cycle; median cycle time.
- Non-conformity trend — new NCs raised, NCs closed, mean-time-to-closure.

**AI-system metrics** — metrics about the *behaviour* of AI systems in scope, aggregated for the AIMS view:

- Incident count and severity trend (feed from mod-110 post-market surveillance).
- Model performance and drift indicators (aggregated from operational monitoring).
- Bias and fairness measurements (per system, per protected characteristic where measurable).
- Human-oversight interventions and human-override rates.
- Third-party model changes and their triggered re-evaluations.

**Compliance-posture metrics** — metrics about the enterprise's regulatory posture:

- Registration status per jurisdiction (per system, per regime).
- Filing completeness (post-market monitoring reports, serious-incident reports, transparency reports).
- Regulatory findings — count, severity, closure status.

The architect designs the *measurement plan* — which metrics, at what cadence, from what source, aggregated by whom, reviewed at what forum. Not every metric goes to every forum: the monthly operational review sees operational metrics; the quarterly review sees trend and coverage; the annual management review sees the full set with year-on-year comparison.

**Measurement plan shape.**

```yaml
metric_id: METRIC-AIMS-004
name: SoA in-place coverage
definition: >
  Percent of SoA rows marked in-place / total SoA rows marked
  included, at the SoA version approved at the most recent
  quarterly desk-review.
source: SoA register in GRC-for-AI platform.
method: automated computation from the register; verified by
  head of AI governance at each review.
cadence: quarterly.
review_forum: quarterly AIMS operational review; escalated to
  management review annually.
target: 100% in-place at annual management review; deviation
  above 5% requires RTP entry with named owner and timing.
data_retained: quarterly snapshot in the AIMS documented-
  information register; retained indefinitely.
```

Metrics without a review forum and without a target are decorative. Metrics with a review forum, a target, and a defined escalation are AIMS instruments.

## Clause 9.2 — internal audit

Clause 9.2 requires the organisation to conduct internal audits at planned intervals to provide information on whether the AIMS conforms to the organisation's own requirements and to the standard, and whether it is effectively implemented and maintained. The audits must be planned, established, implemented, and maintained; must define audit criteria and scope for each audit; must be conducted by *auditors independent* of the audited activity; must report results to relevant management; must retain documented information as evidence.

**The internal audit programme.** The architect designs a *multi-year audit programme* that samples the AIMS end-to-end over a defined cycle — typically two or three years — so that every clause and every material control is audited at some point in the cycle. The programme is calibrated:

- *Higher-risk areas audited more frequently.* Annex A controls tied to consequential-decision systems or to material regulatory exposure are audited annually. Lower-risk controls may be audited every two years.
- *All AIMS clauses touched over the cycle.* Clauses 4–10 each get direct audit coverage at some point.
- *Systems sampled representatively.* Not every system every year — a sampling plan that covers each system tier over the cycle.
- *Findings from previous audits followed up.* Every audit round begins by verifying the previous round's findings were closed.

**Auditor independence.** Clause 9.2 requires auditors independent of the audited activity. The internal auditor cannot audit their own work. Practically, this means:

- The enterprise's internal audit function (typically part of the third-line-of-defence — audit committee reporting) runs the AIMS internal audit programme.
- Where internal audit lacks AI-specific competence, the enterprise buys or borrows it — a co-source engagement with a firm that has AI auditors, or a competence-development plan for internal audit.
- The head of AI governance *is not* the AIMS internal auditor. The head owns the AIMS operations; auditing the operations must be independent. Cross-reference: mod-101 chapter 06 (engagement contracts) and mod-107 (assurance architecture) walk the three-lines-of-defence positioning.

**Audit report shape.** Each internal audit produces:

- A summary of scope audited (clauses, controls, systems).
- Sampling methodology.
- Findings, classified (nonconformity major / minor; observation; opportunity for improvement).
- Recommendations.
- Distribution list (head of AI governance, AI-accountable executive, audit committee).
- Response required (per finding: root cause analysis, corrective action, target closure date).

Findings feed the CAPA process (Clause 10, chapter `09-non-conformity-corrective-action-and-continual-improvement.md`).

## Clause 9.3 — management review

Clause 9.3 requires top management to review the AIMS at planned intervals to ensure its continuing suitability, adequacy, and effectiveness. The review must consider defined inputs and produce defined outputs.

**Required inputs (canonical shape).**

- Status of actions from previous management reviews.
- Changes in external and internal issues relevant to the AIMS (Clause 4.1) and changes in the needs and expectations of interested parties (Clause 4.2).
- Information on the AIMS's performance and effectiveness — including trends in nonconformities and corrective actions, monitoring and measurement results (Clause 9.1), audit results (Clause 9.2), fulfilment of AI objectives (Clause 6.2), and performance of AI systems in scope (aggregated from mod-110).
- Adequacy of resources (Clause 7.1).
- Opportunities for continual improvement.
- Feedback from interested parties (customer feedback, regulatory feedback, employee feedback, external audit feedback).

**Required outputs.**

- Decisions and actions related to continual improvement opportunities.
- Any need for changes to the AIMS.
- Resource needs.

The architect designs the *review package* — the standard input document that lands with top management ahead of each review — and the *decision record* — the minutes that record what was decided and what actions arose.

**Review package shape.**

```markdown
# AIMS Management Review — 2026-Q4

## 1. Actions from previous review
[Table: action, owner, status, closure evidence.]

## 2. Context changes
### 2.1 External
[Regulatory / standards / stakeholder changes since last review.]
### 2.2 Internal
[Portfolio / organisation / talent / technology changes since last review.]

## 3. Performance
### 3.1 AIMS-process metrics
[Metrics with trend and target comparison.]
### 3.2 AI-system metrics (aggregated)
[Aggregated view; detail in appendices.]
### 3.3 Compliance-posture metrics
[Registration / filing / findings.]

## 4. Objectives
[Clause 6.2 objectives register; progress; escalation.]

## 5. Audit results
### 5.1 Internal audits since last review
[Findings summary; open items; overdue items.]
### 5.2 External audit / assessor engagements
[Certification-body engagement (if any); external assessor engagements; findings.]

## 6. Nonconformities and corrective actions
[Trend; overdue; systemic issues surfaced.]

## 7. Resources
[Resource inventory review; gaps against RTP; funding requests.]

## 8. Continual-improvement opportunities
[Head of AI governance's recommendations; input from other stakeholders.]

## 9. Interested-party feedback
[Customer / regulator / employee / civil-society signals.]

## 10. Proposed decisions
[Specific decisions requested from top management.]

## Appendices
- SoA current version.
- RTP full register.
- AIA register (list of AIAs in progress and completed since last review).
- Full metric appendix.
```

**Cadence.** The management review typically runs annually. Larger or higher-risk enterprises run semi-annually. In either case, the review is a *scheduled forum* with top management demonstrably attending. "Circulated by email" does not satisfy Clause 9.3.

**Attendance.** The AI-accountable executive attends and chairs (or top management delegates a designated chair). Other top-management members attend as their portfolios require — CISO for ISMS integration touchpoints, CDO for data-quality touchpoints, general counsel for legal-and-regulatory touchpoints, chief risk officer for ERM integration touchpoints, chief HR officer for competence and awareness touchpoints. The head of AI governance presents; internal audit attends and speaks to the audit findings; the level-50 architect attends as subject-matter expert on any architectural questions.

**Minutes and decision record.** Decisions taken are recorded with clarity — who decided, what was decided, what action was required, who owns the action, by when. The record is filed as documented information (Clause 7.5) and referenced by every downstream artefact it authorises.

## The AIMS operations calendar

Assembling Clauses 7, 9, and 10 produces the AIMS *operations calendar* — the recurring set of activities that the head of AI governance runs, at defined cadences, producing defined outputs. The architect designs the calendar so it interlocks with the enterprise's other management cadences (board calendar, audit-committee calendar, ERM calendar, ISMS calendar).

A typical calendar:

**Monthly** — head of AI governance's operational leadership team meeting: RTP progress; open incidents; open audit findings; upcoming external filings; new AI systems in the pipeline; escalations.

**Quarterly** — AIMS operational review with the extended AIMS stakeholder group (AI-accountable executive; direct reports of the head; internal audit representative; legal representative; CISO representative for ISMS integration; CRO representative for ERM integration): metrics trend; RTP quarterly close; SoA desk-review; objectives progress; upcoming management-review preparation.

**Semi-annually** — internal audit round (per the audit programme): audit team executes the sampled clauses/controls/systems; produces report; findings distributed and CAPA opened.

**Annually** — management review (Clause 9.3): full package as described above; decisions recorded; actions distributed.

**Annually** — SoA and RTP annual walk: full review of every SoA row; RTP entries reviewed for closure or continuation; new entries opened for the coming cycle; SoA reapproved.

**Annually** — competence and awareness cycle: competence records refreshed for role holders due for reassessment; awareness modules refreshed and released; completion tracking reset for the new cycle.

**Annually** — resource inventory refresh: resources reviewed against RTP and objectives; funding requests raised for the following budget cycle.

**Annually** — communications plan refresh: internal and external communications reviewed and updated; external filings calendar refreshed.

**Event-driven** — new AI system in scope, material system change, incident, regulatory change, standard revision, third-party model change, new legal entity, exit of legal entity: triggers the specific out-of-cycle updates the calendar's shape defines.

The calendar is a documented artefact in the AIMS handbook. Auditors will look at it and cross-reference it with the actual evidence — did the quarterly review actually happen; did the semi-annual audit round actually run; did the annual management review actually convene with the required attendance.

## The architect's ownership of the calendar

The architect designs the calendar. The head of AI governance runs it. The distinction is the same one that applies across the module: the architect *designs the artefact and the flow*; the operator *runs the artefact and the flow*. The head of AI governance may propose calendar changes (based on operational experience); the architect confirms the changes preserve conformance with Clauses 7, 9, and 10; top management ratifies material changes at the management review.

## Two failure modes

**Failure mode 1 — the management review that reviews nothing.** The review runs to schedule. The head of AI governance presents a slide deck. Top management asks polite questions. No decisions are taken; no actions arise. Six months later the same slide deck runs again with slightly updated numbers. Auditors ask for the decision record; it says "AIMS reviewed; no exceptions raised." The finding is severe: Clause 9.3 has become theatre. The fix is designing the review as a *decision-forcing event* — the review package always names *specific decisions requested from top management*, and the minutes always record *what was decided*.

**Failure mode 2 — the internal audit programme without competent auditors.** The enterprise's internal audit function runs the AIMS audits with generalist auditors who lack AI-specific competence. Findings are shallow — process issues rather than substantive control issues. The certification body then finds material gaps that the internal audit missed. This is a systemic problem, not a one-off. The fix is *competence — bought in, co-sourced, or developed* — for the auditors before the audit programme runs. Chapter `11-designing-for-third-party-audit-iso-42006.md` walks the parallel auditor-competence expectation the certification body itself operates under.

## Summary

Clause 9 is where the AIMS checks itself. Monitoring and measurement (9.1) collect the process, system, and posture metrics; internal audit (9.2) samples clauses, controls, and systems independently on a multi-year programme; management review (9.3) brings top management to a scheduled forum with defined inputs and required outputs. Assembled with Clause 7's support cadences and Clause 10's CAPA flow, they produce the AIMS operations calendar — monthly, quarterly, semi-annual, annual, event-driven — that the head of AI governance runs and the certification body samples. The architect designs the calendar; the head of AI governance operates it. The two classic failure modes — management review as theatre, internal audit without competence — are prevented by decision-forcing package design and by explicit auditor competence investment. The next chapter walks Clause 10 — non-conformity, corrective action, and continual improvement — that closes the loop back to Clause 6 planning.
