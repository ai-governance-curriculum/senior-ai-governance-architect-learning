# Non-conformity, corrective action, and continual improvement — Clause 10

## Why this chapter exists

Clause 10 closes the AIMS's improvement loop. Clause 10.1 addresses *continual improvement* — the standing obligation to make the AIMS continually more suitable, adequate, and effective. Clause 10.2 addresses *nonconformity and corrective action* — the specific process by which the enterprise handles the moments when reality departs from the AIMS's requirements. Between them, Clause 10 is what turns a static AIMS into a learning system.

The auditor scrutinises Clause 10 for two things: is there a defined process, and is it *being used*. An AIMS with zero nonconformities in a year is not compliant, it is either aspirational or not running: real management systems generate nonconformities constantly, and their maturity is visible in how they process them, not in how few they surface.

## Clause 10.1 — continual improvement

Clause 10.1 requires the organisation to *continually improve* the suitability, adequacy, and effectiveness of the AIMS. The three adjectives correspond to distinct dimensions the improvement loop must address:

- **Suitability** — the AIMS remains fit for the enterprise's changing context. As the enterprise's AI portfolio evolves, as regulations change, as interested-party expectations shift, the AIMS must be updated to remain suitable.
- **Adequacy** — the AIMS covers what it needs to cover. Gaps identified through audit, incident, external assessment, or self-review are closed.
- **Effectiveness** — the AIMS produces the outcomes it is designed to produce. If controls exist but do not reduce risk, if processes run but do not surface issues, effectiveness is failing.

Continual improvement is not the same as continuous change. The auditor is not looking for a certain velocity of change; they are looking for evidence that the enterprise *reads its own signals* — audit findings, incidents, management-review inputs, external feedback — and *acts on them*.

The improvement loop is enacted through several channels the AIMS already operates:

- The RTP (chapter `06-risk-treatment-plan-and-operational-clauses.md`) is the primary channel — every audit finding, every material incident, every accepted continual-improvement opportunity produces an RTP entry with an owner and a timeline.
- The management review (chapter `08-performance-evaluation-internal-audit-and-management-review.md`) is the forum — improvement opportunities are surfaced there, ratified by top management, and injected into the RTP.
- The CAPA process (below) is the specific mechanism for nonconformities.
- The AIMS-change process (Clause 6.3, chapter `03-leadership-policy-and-planning-clauses-5-and-6-1.md`) is the mechanism for material AIMS-level changes.

The architect designs the interlock so improvement signals do not fall between channels. An audit finding classified as a nonconformity goes through CAPA (Clause 10.2) *and* produces an RTP entry (Clause 6.1.3). A continual-improvement opportunity that is not a nonconformity goes through the management review and lands as an RTP entry. Nothing gets logged in an inbox and forgotten.

## Clause 10.2 — nonconformity and corrective action

Clause 10.2 requires that when a nonconformity occurs the organisation reacts to it (containing it and correcting the immediate consequences), evaluates the need for action to eliminate the causes, implements the action needed, reviews the effectiveness, updates risks and opportunities determined during planning if necessary, and makes changes to the AIMS if necessary. The organisation must retain documented information as evidence of the nature of the nonconformity and the actions taken, and the results of any corrective action.

**What counts as a nonconformity in an AIMS?**

- An identified failure of the AIMS itself to satisfy a requirement of ISO/IEC 42001 or of the AIMS's own defined processes (a *system-level nonconformity*). Examples: the SoA was not refreshed in the last cycle; the management review was not held; competence records for a role holder are missing.
- An identified failure of an AI system in scope to meet the enterprise's own control or performance requirements, discovered through internal audit, monitoring, incident, or external feedback (a *system-behaviour nonconformity*). Examples: an operational bias measurement exceeded the acceptance threshold; a model drift trigger fired but was not acted on within SLA; an AIA was not conducted before a material change went live.
- A regulatory or contractual nonconformity — the enterprise fell short of a specific obligation. Examples: a serious incident was reported outside the regulatory window; a required transparency notice was not present in a production interface.

Nonconformities may be classified — *major* (systemic or severe; blocks or would block certification), *minor* (localised; does not block certification but requires closure), *observation* (concern short of nonconformity; no formal closure required but recorded).

## The CAPA record

The AIMS's CAPA (corrective-and-preventive-action) register is the artefact that records each nonconformity from discovery to closure. The architect designs the record shape; the head of AI governance and the assigned owner operate it.

```yaml
capa_id: CAPA-2026-047
date_opened: 2026-05-14
source:
  type: internal-audit-finding
  reference: INT-AUDIT-2026-Q1-014
description: >
  The AIMS's SoA row A.6.2.7 (AI system change management) is
  marked in-place, but the audit found that three of six sampled
  material-change events in the last quarter did not trigger
  the required AIA re-fire step. The change-management procedure
  exists but the material-change definition is ambiguous on
  system-prompt updates, and prompt updates were treated as
  non-material by the operational team.
classification: minor-nonconformity
affected_clauses:
  - 8.1 operational planning and control.
  - 8.2 AI system impact assessment (operational tie-in).
affected_soa_rows:
  - A.6.2.7 change management (marked in-place; status now
    contested pending CAPA closure).
affected_systems:
  - SYS-CHAT-001 (customer-facing GenAI); SYS-RAG-002
    (internal RAG assistant).

immediate_action:
  action: >
    Freeze non-emergency system-prompt changes to the two affected
    systems pending clarification. Re-run AIA impact-check
    retroactively on the three missed events; document any
    material impact identified.
  owner: ai-risk-engineer (level 25) R. Patel.
  target_date: 2026-05-21.
  actual_date: 2026-05-20.
  effectiveness_verified: yes; three retroactive AIA impact-checks
    completed; no material impact identified; frozen scope
    reopened with clarified rule.

root_cause_analysis:
  method: 5-whys plus process walkthrough.
  finding: >
    (1) Prompt update was not treated as material because the
    material-change definition in the change-management procedure
    enumerated model-weight changes, training-data changes, and
    architecture changes, but did not explicitly name prompt
    updates. (2) The operational team followed the enumeration
    literally. (3) The definition was authored before the
    enterprise had material system-prompt engineering as a
    routine operational activity, and was not refreshed when
    the practice matured. (4) The AIMS-change process (Clause
    6.3) did not surface the drift because no incident had
    previously depended on it.

corrective_action:
  action: >
    Amend the change-management procedure to enumerate system-
    prompt updates as material-change events (or, where prompts
    have version-controlled minor / major distinction, prompt
    major updates as material). Republish the procedure, notify
    all operational owners, refresh the awareness module.
  owner: head-of-ai-governance.
  target_date: 2026-06-30.
  actual_date: 2026-06-24.

preventive_action:
  action: >
    Add a quarterly "material-change definition currency" review
    to the AIMS operations calendar to catch drift of this shape
    proactively. Add a rule to the AIMS-change process that
    procedural definitions touching Clause 8 operations are
    reviewed on every management-review cycle.
  owner: architect (level 50) + head-of-ai-governance.
  target_date: 2026-09-30.

rtp_link: RTP-2026-Q2-047 (owns the corrective and preventive
  action delivery).

effectiveness_review:
  planned_date: 2026-12-31 (six months after closure).
  method: sample five material-change events in the two quarters
    following procedure amendment; verify AIA impact-check ran
    for each; verify no repeat nonconformity of same shape.
  actual_outcome: <to be assessed>.

closure:
  closure_date: <pending effectiveness review>.
  closed_by: head-of-ai-governance (proposes); ai-accountable
    executive (ratifies at next management review).
```

The record has three properties worth naming explicitly.

**Property 1 — immediate action is separated from corrective action.** The immediate action *contains the harm*; the corrective action *addresses the cause*. Confusing them produces CAPAs where the enterprise fixes the symptom (patch the process for the specific systems that hit it) and never fixes the cause (procedure definition drift). The audit will find the same shape of nonconformity again at the next round.

**Property 2 — preventive action is a first-class field.** Clause 10.2 focuses on corrective action; the enterprise's own maturity is measured by whether preventive action is routinely considered. If a nonconformity was discoverable through a systemic pattern (procedure drift, competence-refresh gap, monitoring blind spot), the preventive action addresses the systemic pattern, not just the specific incident.

**Property 3 — effectiveness review is a distinct event.** The CAPA is not closed when the corrective action is delivered; it is closed when the effectiveness of the corrective action has been verified. The effectiveness review typically happens weeks or months after delivery, samples the process the corrective action changed, and produces a written outcome. Without this step, corrective actions are hopes, not fixes.

## The relationship of CAPA to the RTP

The CAPA process (Clause 10.2) and the risk-treatment plan (Clause 6.1.3) are related but distinct.

- **The CAPA process** handles specific *nonconformities* — moments when a defined requirement was not met — and drives them to root cause and closure.
- **The RTP** handles *treatments of identified risks* — planned interventions to reduce, transfer, avoid, or accept risk.

Every CAPA typically produces (or references) an RTP entry that owns the delivery of the corrective and preventive action. Not every RTP entry has an underlying CAPA — many RTP entries are proactive (implement a new control to reduce a proactively-identified risk), not reactive.

The wire-up is bi-directional and named. The CAPA record's `rtp_link` field points at the RTP entry that owns delivery; the RTP entry's `capa_source` field (where present) points back at the CAPA. Audit will trace both directions.

## Common sources of nonconformity

Nonconformities enter the CAPA process from multiple sources; the AIMS accepts them all:

- **Internal audit findings** — the systematic source, per Clause 9.2.
- **External audit / assessor findings** — from the certification body (stage-1 or stage-2 audit), from co-source assessors, from customer or regulatory audits.
- **Incident outcomes** — mod-110 post-market surveillance turns up incidents; each is triaged, and where a nonconformity is identified, CAPA opens.
- **Self-identified issues** — the head of AI governance's team, the AI-risk engineers, the AI-evaluation engineers, or any AIMS role holder identifies a gap in the course of their work and raises it.
- **Interested-party signals** — customer complaint, employee report, civil-society letter, regulatory guidance — that surfaces a control gap.
- **Regulatory-change gaps** — a new regulation lands; the AIMS's current state does not meet the new requirement; a CAPA is opened for the gap-closure work.

The AIMS documented-information register includes the CAPA register; the register is retained per the retention policy and is available on request to internal audit, external assessors, and top management.

## Common CAPA failure patterns

**Pattern 1 — no root-cause analysis.** The CAPA record says "corrective action: fixed the issue". No 5-whys, no process walkthrough, no analysis of why the issue occurred in the first place. Predictable outcome: the same shape of nonconformity recurs, sometimes under a different label. The fix is *discipline* — every CAPA above observation-tier requires a documented root-cause analysis, and closure without one is refused by the head of AI governance.

**Pattern 2 — corrective action without preventive action.** The CAPA fixes the symptom on the specific systems where it appeared. Six months later the same shape appears on different systems because the underlying systemic cause was never addressed. The fix is the *preventive-action field* being non-optional — the head of AI governance requires it or a documented justification of why none is warranted.

**Pattern 3 — closure without effectiveness verification.** The corrective action is delivered; the CAPA is closed the same day. Later audits find the corrective action did not work — the procedure was amended but never followed; the training was delivered but never landed. The fix is the *effectiveness review* — a scheduled event weeks or months after delivery that samples the process the corrective action changed.

**Pattern 4 — CAPA register invisible to management review.** The CAPA register operates quietly. The management review sees a headline number ("14 CAPAs open, 12 closed this quarter") but no detail; systemic patterns across CAPAs are not surfaced; top management cannot see whether the AIMS is systematically improving. The fix is the *CAPA-trend section* in the management-review package (per chapter 08) — trend, pattern analysis, systemic themes, and the specific decisions requested from top management on systemic themes that require resourcing or scope changes.

## The AIMS-level nonconformity

A specific case worth calling out — a nonconformity in the AIMS *itself*. Examples: the SoA has not been refreshed in the last review cycle; the management review has been missed; competence records for AIMS roles are systematically overdue; the risk-treatment plan has not been pruned in a year. These nonconformities are found either by internal audit or by the certification body, and they are more consequential than system-level nonconformities because they indicate the AIMS is not operating as a management system.

AIMS-level nonconformities go through CAPA the same as any other, but the corrective action typically touches the AIMS-change process (Clause 6.3) — the AIMS itself needs to change to prevent recurrence. Effectiveness review must confirm the change was not just made but is being *followed*.

## Continual improvement as an operating principle

The composition of Clause 10.1 and Clause 10.2 gives the AIMS a specific operating principle: *every signal produces a decision, and every decision produces an action, and every action produces evidence, and every action's effectiveness is verified*. This is the ordinary-language statement of the improvement loop. Enterprises that internalise it produce AIMSes that mature over time; enterprises that treat improvement as an ideal produce AIMSes that decay.

## Two failure modes at the Clause 10 level

**Failure mode 1 — the AIMS with no nonconformities.** The head of AI governance believes visible nonconformities are embarrassing and quietly closes them out through side-channels. The auditor asks for the CAPA register and finds it near-empty for a year. This is a fatal signal — either the AIMS is not running or issues are being hidden. Either way, certification is unlikely. The fix is *cultural* — the head of AI governance and top management treat nonconformities as ordinary signals to be processed, not as failures to be hidden.

**Failure mode 2 — continual improvement as slogan.** The AI policy commits to continual improvement. The management review states continual improvement is a priority. No specific improvements are being made; the RTP does not carry improvement entries beyond those driven by immediate compliance pressure. The auditor asks for evidence of continual improvement and receives a slogan. The fix is *routine surfacing* — the management-review package has a Section 8 (continual-improvement opportunities) that is expected to carry real content; if it is empty two cycles in a row, that itself is a Clause 10.1 nonconformity.

## Summary

Clause 10 closes the AIMS improvement loop. Continual improvement (10.1) is a standing obligation to make the AIMS more suitable, adequate, and effective, operationalised through the RTP, the management review, and the CAPA process. Nonconformity and corrective action (10.2) is the specific process by which each departure from AIMS requirements is contained, root-caused, corrected, prevented from recurring, and verified for effectiveness. The CAPA register is the artefact; immediate-action, root-cause-analysis, corrective-action, preventive-action, and effectiveness-review fields are non-optional; the register is joined to the RTP and visible at the management review. AIMS-level nonconformities are handled through the same process but touch the AIMS-change flow (Clause 6.3). An AIMS with zero nonconformities is either not running or hiding signals; an AIMS with a mature CAPA and continual-improvement practice is what a certification body under ISO/IEC 42006 recognises as effective. The next chapter walks the integration with the ISO/IEC 27001 ISMS — the neighbour management system every enterprise reading this module already has.
