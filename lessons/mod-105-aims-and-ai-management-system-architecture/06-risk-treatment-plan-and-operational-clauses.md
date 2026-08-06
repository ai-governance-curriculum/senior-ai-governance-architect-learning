# The risk-treatment plan and the operational clauses — Clauses 6.1.3 and 8

## Why this chapter exists

The risk-treatment plan (RTP) is the AIMS's *action-tracking* artefact — the register that records, per identified risk, what treatment was chosen, which controls implement it, who owns delivery, when it is due, what evidence closes it out, and what residual risk remains after treatment. Clause 6.1.3 requires the AIMS to produce one; Clause 8 requires the AIMS to *operate* under it. The two clauses together are how the AIMS moves from "we have identified risks and applicable controls" to "we are actually doing the things and can prove it".

The architect designs the RTP shape and the operational-control shape. The head of AI governance operates them. The auditor tests, on any given audit visit, that identified risks have treatments, that treatments have owners, that owners are moving, that residual-risk acceptance decisions have been made explicitly, and that Clause 8 operational controls exist for the AI-system lifecycle activities the enterprise conducts.

## Clause 6.1.3 — the risk-treatment structure

Clause 6.1.3 requires the AIMS to define and apply a risk-treatment process. The process must select appropriate treatment options, determine controls necessary to implement the chosen options, compare the determined controls with Annex A to verify no necessary controls have been omitted, produce the SoA (Clause 6.1.3.d — walked in chapter `05-statement-of-applicability-and-annex-a.md`), and formulate a *risk-treatment plan*.

The four canonical treatment options — inherited from ISO 31000 and specialised for AI in ISO/IEC 23894 — are:

1. **Avoid** — do not undertake the activity that gives rise to the risk. Applicable when the risk exceeds appetite and no combination of controls can reduce it below appetite. For an AI system: decline to deploy; retire an in-flight system; refuse a use case a customer requests. Rare, but visible and correct when used.
2. **Reduce** (or mitigate) — apply controls that reduce likelihood, impact, or both. Almost every RTP entry is a reduce entry. The controls are the specific enterprise AI control library entries; the reduction is measured against a defined pre-and-post analysis.
3. **Transfer** (or share) — shift the risk to another party through contract, insurance, or shared governance. Common in third-party AI (mod-109 details): risk of third-party model malfeasance is partially transferred through provider indemnification and service-level warranties. Never *fully* transferred — the enterprise retains accountability for the outcome, even where liability is contractually shifted.
4. **Retain** (or accept) — decide, at appropriate authority, that the residual risk after other treatments is within appetite and the enterprise will bear it. Explicit acceptance by named authority is required; passive acceptance ("we did nothing") is not the same as retention and produces audit findings.

The AIMS's RTP records the treatment option per risk. Most entries will be `reduce`; the enterprise designs the RTP shape to make the `avoid` and `retain` decisions equally visible so that top management sees them clearly at the management review.

## The RTP entry shape

An RTP entry has a stable identifier, links backward to the risk register (and — where the risk was identified through an AIA — to the AIA), and links forward to the SoA rows whose implementation-status closure it supports and to the enterprise AI control library entries whose implementation delivers the treatment. It carries owner, timing, evidence, and residual-risk fields.

```yaml
rtp_entry_id: RTP-2026-Q2-021
title: Close RAG-corpus drift monitoring gap for internal RAG assistant
risk_register_links:
  - AI-RISK-2026-102 — RAG-corpus drift causing degraded internal answer quality.
aia_links:
  - AIA-2026-049 — internal RAG assistant impact assessment.
soa_row_links:
  - A.6.2.6 — AI system operation and monitoring (partially-in-place).
enterprise_controls_implemented:
  - AIC-MON-055 v3.0 — model output drift monitoring (in-place; needs RAG-corpus extension).
  - AIC-MON-062 v0.9 — retrieval corpus content-drift monitoring (draft; owner: data-platform team).
treatment_option: reduce
description: >
  Deploy corpus-drift monitor on the RAG assistant's document
  index, feeding weekly drift reports to the AIC-MON-055 alerting
  pipeline. Retrieval-quality regressions above threshold trigger
  a human review by the RAG assistant's operational owner and,
  above a higher threshold, an incident under mod-110.
owner_role: ai-risk-engineer (level 25)
owner_person: R. Patel
support_roles:
  - data-platform team (implements corpus-drift monitor).
  - ai-evaluation-engineer (level 35, defines drift metric).
timing:
  planned_start: 2026-05-01
  planned_completion: 2026-09-30
  progress_review_cadence: monthly at AIMS operational review.
evidence_to_close:
  - AIC-MON-062 v1.0 registered and marked in-place.
  - First four weekly drift reports produced and reviewed.
  - SoA row A.6.2.6 updated to in-place.
  - Residual-risk assessment refreshed for AI-RISK-2026-102.
residual_risk:
  pre_treatment: material (level 4 on the enterprise 1-5 scale).
  post_treatment_estimated: low (level 2).
  post_treatment_actual: <to be assessed once treatment complete>
  acceptance_authority_if_above_appetite: head-of-ai-governance.
change_log:
  - 2026-05-15 created by head-of-ai-governance following AIA acceptance.
  - 2026-06-20 timing revised (Q3 completion) after data-platform team dependency review.
```

Notice the RTP entry does *not* re-describe the risk itself in detail — it links to the risk register and the AIA. This keeps the RTP's role clear: it is an action tracker, not a risk description.

## The four common RTP failure patterns

**Pattern 1 — the RTP with no closure criteria.** Entries have owners and dates but no evidence-to-close list. When the date arrives, the owner declares completion; the auditor asks how completion was verified and finds no record. The fix is the `evidence_to_close` list on every entry, agreed at entry-creation time and verified by the head of AI governance before the entry is marked closed.

**Pattern 2 — the RTP that never accepts residual risk.** Entries close with implementation complete but no post-treatment residual-risk assessment. The AIMS has no record of what risk remains after treatment; the auditor cannot verify whether residual is within appetite. The fix is the `residual_risk` block on every entry, with pre-treatment estimated at creation, post-treatment estimated at creation, post-treatment actual assessed at closure, and acceptance recorded (with named authority) where post-treatment residual exceeds appetite.

**Pattern 3 — the RTP that grows without pruning.** Every audit finding, every regulatory change, every risk workshop adds entries. Entries never close cleanly; they linger in `in-progress` status; the total grows quarter over quarter until the RTP is unmanageable. The fix is the *pruning discipline* at the quarterly review: entries whose original driver no longer applies are closed with an explicit note; entries whose scope has changed are re-authored as new entries with links back.

**Pattern 4 — the RTP that lives in the head of AI governance's spreadsheet.** The RTP is a personal artefact of the head of AI governance rather than a shared, discoverable, machine-readable register. On leave, promotion, or attrition, the artefact disappears. The fix is the RTP register living in the GRC-for-AI platform (mod-111) with defined access, versioning, and integration to the risk register and the SoA.

## Clause 6.1.3 — the "compare against Annex A" check

Clause 6.1.3.c specifies a specific check: after selecting treatment options and determining controls, compare the determined controls with Annex A to verify no necessary controls have been omitted. This is a *sanity check* — not a mandate to include every Annex A control, but a discipline that the enterprise cannot design controls that ignore the reference set.

Concretely, the check runs like this. After the risk-treatment plan has proposed treatments for the current cycle, the head of AI governance walks the Annex A list and, for each control, verifies either that it is included in the SoA (with an implementation, planned or in-place, in the RTP), or that it is excluded (with a justified exclusion in the SoA). Any Annex A control that appears in neither the SoA-included list nor the SoA-excluded list is a gap; either the SoA needs an entry for it, or the risk analysis missed the risk it addresses.

The check is an audit exhibit. The head of AI governance produces a signed record — for example, the SoA version-2.4 approval record — confirming the comparison was done for that version. Auditors will ask to see the record.

## Clause 8.1 — operational planning and control

Clause 8.1 requires the organisation to plan, implement, and control the processes needed to meet requirements, and to implement the actions determined in Clause 6. In practice this means:

- Every AI system in scope of the AIMS follows a defined *AI system lifecycle process* that operationalises the enterprise's AI standards and the enterprise AI control library. The lifecycle covers concept, design, development, evaluation, deployment, operation, and decommissioning. Cross-reference: ISO/IEC 5338 provides the AI system life-cycle process framework; 42001 Clause 8 obliges the enterprise to have one, not to use 5338 specifically, but 5338 is the natural reference. <!-- needs-research: confirm ISO/IEC 5338 current publication status. -->
- *Documented information* proving the process was followed is retained per Clause 7.5 — the SSP artefacts (mod-108), the evaluation reports (mod-107), the deployment approvals, the incident records (mod-110).
- *Changes* are controlled — Clause 8 change-management applies to material changes in AI systems, in the underlying model, in the training data, in the deployment context. The architect defines what a "material change" is; the change-control process gates it against re-evaluation, AIA re-fire, and management-review notification where above threshold.

## Clause 8.2 — the AI system impact assessment (the operational tie-in)

Where Clause 6.1.4 requires the *process* for AI impact assessments to be defined and documented, Clause 8.2 requires that the *process is run* on the AI systems in scope. Chapter `04-risk-and-impact-assessment-composition.md` walked the AIA shape and wire-up; Clause 8.2 is the operational commitment that the AIA process is exercised for the systems it applies to. <!-- needs-research: confirm the specific Clause 8 sub-clause numbering for AI impact assessment in the published ISO/IEC 42001:2023. -->

The architect designs the *trigger* — the enterprise process step at which an AIA fires. The three canonical triggers were named in chapter 04: new AI system in scope, material change to an existing system, event that changes impact context. Clause 8 makes the trigger *operational* — a step in the AI system lifecycle at which the AIA gate must pass.

## Clause 8.3 — operational controls specific to AI systems

Under Clause 8 the enterprise operates a family of controls specific to the AI system lifecycle. The specific control set is enterprise-designed — walked in mod-102 for the control-library shape and mod-108 for the evidence architecture. From the AIMS's perspective, the operational controls that Clause 8 typically expects to be operating include:

- **Data governance and data quality controls** — dataset registration, provenance records, quality assessments per ISO/IEC 5259, purpose limitations, retention. Mod-108 covers the evidence contracts.
- **Model development controls** — training-run reproducibility, hyperparameter and configuration logging, model card production, model registration.
- **Model evaluation controls** — pre-deployment evaluation on defined tests (accuracy, robustness, bias, safety, adversarial), acceptance criteria, evaluation report retention. Mod-107 covers assurance architecture.
- **Deployment controls** — deployment approval per system tier, deployment-window controls, canarying, rollback capability.
- **Operational monitoring controls** — model performance monitoring, drift monitoring, safety monitoring, feedback-loop analysis. Mod-110 covers post-market surveillance.
- **Incident and event controls** — incident classification, escalation, root-cause analysis, corrective action. Feeds Clause 10.
- **Change-management controls** — material-change definition, re-evaluation gates, communication.
- **Third-party controls** — provider governance, contract clauses, evidence exchange. Mod-109 covers third-party governance architecture.
- **Decommissioning controls** — decommission planning, data disposition, communication to affected users.

The AIMS Clause 8 is not the place these controls are *authored* — that is mod-102's control library. It is the place they are *operationally required* to run. The auditor looks at the SoA to see which controls are in scope, then samples systems to verify the controls are running on those systems, then samples evidence to verify the controls are producing what their evidence contracts require.

## The RTP-to-SSP relationship

The AIMS Clause 8 controls operate at the *system* level. Each system in scope maintains a *system security-and-safety plan* (SSP) — sometimes called a system implementation plan, sometimes an AI system plan of action and milestones — that records, per system, which enterprise controls are in place, which evidence has been produced, and what open items remain. Mod-108 walks the SSP shape.

The RTP is the *enterprise-level* action tracker; the SSP is the *system-level* implementation tracker. An RTP entry that owns rolling out a new evidence contract will typically produce dozens of SSP updates (one per system to which the contract now applies). The link is bi-directional — the RTP entry lists the systems affected; each SSP references the RTP entries that touched it.

## Two failure modes at the RTP-and-operations boundary

**Failure mode 1 — treatments that never operationalise.** The RTP marks a treatment "complete" when a policy document is signed. Clause 8 operational controls are not actually running; SSPs do not carry the new evidence; systems in production have not adopted the new practice. The auditor discovers the seam by sampling systems and finding no evidence. The fix is the closure criterion — evidence-to-close on every RTP entry must include *system-level evidence*, not just enterprise-level artefacts.

**Failure mode 2 — Clause 8 controls that operate but have no lineage to the RTP.** Systems operate a mature set of controls; the SSPs are clean; the evidence is produced. But the RTP has no record of how those controls came into existence — no treatment entries, no ownership, no historical decisions. The AIMS looks static and un-improving. The fix is retroactive RTP entries for controls that pre-date the AIMS's formal stand-up, so the register captures the historical baseline; and prospective RTP entries for every control introduction going forward.

## Summary

The risk-treatment plan is the AIMS's action-tracking register — per identified risk, the treatment chosen (avoid / reduce / transfer / retain), the controls implementing it, the owner, the timing, the evidence-to-close, and the residual-risk assessment. Clause 6.1.3 requires the RTP to exist and to be joined bi-directionally to the SoA (which the RTP moves toward its `in-place` end-state) and to the risk register (which the RTP takes as input). Clause 8 requires the operational controls to actually run — an operational-planning-and-control commitment that binds the RTP's completed treatments to system-level SSP evidence. The four canonical failure patterns — no closure criteria, no residual-risk acceptance, unpruned growth, personal-spreadsheet ownership — are prevented by shape discipline and cadence. The Annex A "no necessary controls omitted" check (Clause 6.1.3.c) is a sanity check that the head of AI governance runs at every SoA revision. The next chapter walks Clause 7 — the support layer that makes the AIMS's people, competence, communications, and documented information run.
