# Policy-change communications and deprecation windows

## Why this chapter exists

A policy change silently published on a Friday is the single most common cause of surprise findings at audit. The mechanism is well understood: the policy repo receives a merge; the effective date lands the following Monday; downstream artefacts — the mapping matrix from `02-cross-framework-mapping-matrix.md`, the traceability index, the control-library entries pinned by SoAs, the policy-as-code bundles enforcing gate rules, the evidence pipelines collecting artefacts under the old cadence — continue to reference the previous text; the next external assessment hits the delta and finds that the org has been "operating out of policy" for months without knowing.

The architect's job in mod-103 is not to write every policy — that responsibility mostly sits with policy owners, legal, and the head-of-AI-governance (level 60). The architect's job is to design the *change-communications flow* so that a policy update propagates predictably: who gets a heads-up before publication, which downstream roles are on the routing list, how deprecation windows overlap old-and-new so in-flight audits and evidence-collection cycles can complete cleanly, and what constitutes a valid pre-publication notice packet.

This chapter designs that flow. It is the policy-side analogue of the control-library release discipline in `../mod-102-ai-control-library-architecture/06-control-authoring-lifecycle.md`; the two tracks are deliberately coordinated because policy publishes on one cadence and the control library follows on the next, and the bridging period is governed by the deprecation window designed here.

## The five policy change classes

Every policy change is classified at proposal time into one of five classes. The class determines the notice window, the routing depth, the deprecation posture, and the ratification requirement. This is the policy-side analogue of the semver-ish classification in `../mod-102-ai-control-library-architecture/06-control-authoring-lifecycle.md`, adapted to the fact that policies commit the organisation to obligations rather than to catalog entries.

| Class | What it covers | Notice | Deprecation window | Ratifier |
|---|---|---|---|---|
| Editorial | Typos, formatting, non-normative clarifications, hyperlink repair, style-guide alignment. | Changelog only; no stakeholder notice. | None. | Policy owner. |
| Minor | Additive requirements; new clauses that constrain no existing in-scope system; new definitions that do not change interpretation of existing clauses. | Two-week notice to the routing list. | Optional; use only if any downstream artefact needs re-authoring. | Policy owner + architect review. |
| Major | New normative requirement; changed applicability filter; tightened evidence contract; new obligation traceable to an existing framework mapping. | Minimum four-week notice. | Required on the superseded requirement. | Head-of-AI-governance (level 60). |
| Breaking | Removal of a policy; applicability change that invalidates existing Statements of Applicability; change forcing control-library retirement; principle-level revision. | Minimum eight-to-twelve-week notice. | Required with an explicit dual-authority schedule. | Head-of-AI-governance + AI governance council. |
| Emergency | Regulator-driven (new rule with a short compliance clock) or incident-driven (a live incident forces immediate policy tightening). | May skip standard notice with a documented emergency-change record; retroactive routing within a defined window. | Sometimes waived; more often *compressed* rather than skipped. | Head-of-AI-governance, escalated. |

Editorial and minor together should be the bulk of change volume in a mature program. Major happens on a regular cadence — every policy touches it a few times a year as frameworks evolve. Breaking is rare by design; if breaking is happening more than a handful of times a year the policy set is under-cooked and the architect's shape work from `01-policy-hierarchy-and-instrument-shape.md` needs revisiting. Emergency should be genuinely rare — a program that uses emergency to bypass routing under normal load has lost the discipline.

The class is set by the proposer and confirmed by the architect at review. A mis-classified change (a major shipped as minor) is the fastest way to blow routing-list trust, exactly as with control-library mis-classification; the architect is picky here on purpose.

## The routing list

Every policy change routes to a deterministic set of stakeholders. The list is not "whoever the policy owner remembers" — it is a fixed function of the change class and the impacted policy area. The routing list lives in the policy repo alongside the policy itself, versioned, reviewed at the same cadence as the policy taxonomy from `01-policy-hierarchy-and-instrument-shape.md`.

| Role | What they update on notice | Present on which classes |
|---|---|---|
| ai-governance-analyst (level 15) | Mapping matrix (chapter 02), traceability index, evidence-collection cadences, per-obligation retention schedule, POA&M entries where the policy delta forces re-collection. | Minor, major, breaking, emergency (retro). |
| ai-risk-engineer (level 25) | Engineering-side controls in the library, policy-as-code bundles, gate rules, sidecar guardrail configuration, CI/CD policy checks, mitigation-plan templates where relevant. | Minor (if PaC-affecting), major, breaking, emergency. |
| ai-evaluation-engineer (level 35) | Evaluation gates, model-card sections that reference the policy, evaluation-report templates, threshold definitions where a policy sets a floor. | Major and breaking that touch evaluation obligations; minor if the additive clause is evaluation-facing. |
| head-of-AI-governance (level 60) | Accountable ratifier for major and breaking. Reviews the notice packet and signs the effective-date decision. | Major, breaking, emergency. |
| Policy owner (varies) | The named accountable for the specific policy — often a director in the affected function. Reviews the delta for operational feasibility in their function. | All classes. |
| Product / business-unit contacts | Deployed-system owners whose systems will be re-scoped or have new obligations. They are the source of "will this break something on Tuesday" signal. | Major and breaking; minor if any deployed system is directly affected. |
| Legal | Any change with regulatory-obligation implications; any change that references or interprets external law; any change that could shift the org's public position. | Major, breaking, emergency; any minor with regulatory language. |
| Communications | Org-wide announcements on principle-level or brand-facing changes; external messaging where a policy change becomes visible outside the org. | Breaking (usually); major (sometimes); minor (rarely). |
| Internal audit / GRC (mod-111 seat) | The internal-audit function tracks the delta so their audit programme stays aligned with the current policy set. See `../mod-111-grc-tool-integration-and-vendor-strategy/`. | Major, breaking. |

The list is deterministic, not judgment-based. If a policy change is in-class it routes to every role marked for that class, whether or not the policy owner *thinks* the role needs to know. Silence past SLA (below) is an escalation trigger, not implicit approval — the routing exists precisely so that "we didn't hear back" cannot mean "so we assumed OK."

## The pre-publication notice packet

The notice is not a one-line email. A notice packet is the artefact that goes to the routing list; it has a fixed shape so that every recipient sees the same information in the same slots and can respond against a stable format. The packet's seven required sections:

1. **Old-text / new-text diff.** The literal text delta, clause-by-clause. Reviewers must be able to see exactly what changed without hunting; render as a side-by-side or a unified diff, not a summary. Summaries hide load-bearing wording changes.
2. **Change class and rationale.** The class from the table above, plus the *why*. Rationale is short but concrete: the trigger (regulator update, incident, internal review finding, framework revision), the outcome the change achieves, the alternative options considered and why not chosen.
3. **Impacted control-library entries.** The list of control IDs whose statement, applicability, evidence contract, or crosswalk will need updating downstream. This list is *maintained by the ai-governance-analyst* — the analyst runs the mapping matrix from `02-cross-framework-mapping-matrix.md` against the delta and returns the list before the packet ships. If the analyst has not returned the list, the packet is not ready.
4. **Impacted policy-as-code bundles.** The list of PaC bundle IDs whose rules will need updating. This list is *confirmed by the ai-risk-engineer* — the engineer walks the bundle registry against the delta and returns the list, plus a first-cut view of which bundles are simple substitutions vs. which require rule rewrites. Bundle authoring itself is covered in `04-policy-as-code-boundaries-and-authorship.md`; the notice packet consumes the bundle IDs the engineer names.
5. **Effective date, deprecation-window end date, and no-go-live-past-this-date milestones.** *Publication date* (when the new text appears in the repo), *effective date* (when the new text becomes authoritative), and *sunset date* (when the old text is no longer consumable). These three are distinct and always named — collapsing them is one of the shape mistakes below. The milestones section names any decision points inside the window: for example, "no new SoA may be authored against the old text after week N."
6. **Migration guidance.** Where downstream artefacts should look for guidance on migrating, whom to ask, and a checklist for common migration steps (update the mapping matrix rows, re-collect the affected evidence artefacts, re-run the affected PaC bundles under the new rule, refresh the affected model-card sections). Not exhaustive — pointers, not procedures.
7. **Rollback plan.** What happens if the change causes production impact within the first N days of the effective date. Concretely: who has authority to revert, what state the org returns to (the old text remains authoritative during the window precisely so rollback is available), what the rollback communication looks like, and what post-mortem process runs after. For breaking changes rollback is often not a full revert (the driver was usually external and cannot be un-done) — the plan then names the compensating stance instead.

A packet missing any of the seven is not ready to route. Enforce this as a checklist at the notice-drafting stage; the checklist is the architect's leverage against the pull to "just send something and iterate."

## Deprecation-window discipline

The deprecation window is the overlap period where both old and new policy text coexist. During the window:

- **Old policy remains authoritative until the window closes.** In-flight audits, evidence-collection cycles, and SoAs authored under the old text continue to reference the old text and are not retroactively judged against the new. This is the single most important discipline point — it is what makes the change flow *safe*.
- **New policy is published immediately but enforced from the effective date.** Publication and enforcement are distinct. Publishing early lets downstream teams read, plan, and start migration; enforcing from a named future date is what gives them the runway.
- **Window overlap allows in-flight audits and evidence-collection cycles to complete against the version that was authoritative at their start.** An audit begun in week 1 of a 12-week window completes against the old text even if the effective date lands in week 8. This is not laxness; it is the only way to keep audits reproducible.
- **Policy-as-code bundles run BOTH old and new during the window.** This is the dual-write / dual-check pattern: the CI pipeline evaluates the request against the old rule and against the new rule in parallel. The old result is authoritative for gating; the new result is *logged* so the org can validate that the new rule fires as intended before the effective date flips authority. Dual-check catches "the new rule is unexpectedly stricter" and "the new rule has a bug that would have blocked half of production" *before* those outcomes matter. Bundle mechanics live in `04-policy-as-code-boundaries-and-authorship.md`; the window discipline here is the reason bundles must support dual-check as a first-class mode.
- **Evidence collection continues on the old cadence until the effective date.** If the new policy tightens cadence (quarterly to monthly, say), the tighter cadence begins at the effective date, not at publication. The old cadence artefacts remain valid for their retention period; the new cadence artefacts join alongside.
- **The sunset event is a governed transition.** At window close, a sunset notice ships (not a silent end); the old text is marked non-authoritative but retained in the repo history (see `06-versioning-effective-dates-and-immutability.md` for the immutability rules). No new SoA may pin to the sunset text; existing SoAs that still reference it are flagged in the analyst's tracker for migration.

Window lengths track the change class — four weeks minimum for major, eight-to-twelve for breaking — but they can be extended when downstream complexity is high. A breaking change that forces re-collection of an evidence artefact with a long production cadence may need a window measured against that cadence, not against a default.

## Routing SLAs

Silence past SLA is an escalation trigger, not implicit approval. For major and breaking changes the routing SLAs are bounded and named:

- **Acknowledge — 5 business days from notice.** Every routed role returns a "received, in review" signal within a week. Not an approval; just proof of receipt.
- **Commit an implementation plan — 15 business days from notice.** The role returns a first-cut plan naming the downstream artefacts to be updated, the effort estimate, and any dependencies on other roles. If the plan surfaces a blocker (an evidence pipeline that cannot meet the tightened cadence, a PaC bundle that cannot be re-authored in time) that blocker is escalated *now*, not at the effective date.
- **Complete the update — 30 business days from notice, or before effective date, whichever is later.** For four-week major windows this is at the effective date; for eight-to-twelve-week breaking windows there is slack. Completion is proven by an update artefact — a matrix diff, a bundle version bump, a POA&M closure — not by a statement of completion.

Silence at any SLA milestone triggers an architect-initiated escalation to the role's line management and, for breaking changes, to the head-of-AI-governance. The escalation is procedural; it is not a punitive move. It is the only way to distinguish "the role is on plan" from "the role has not seen the notice."

For emergency changes the SLAs compress but the *shape* survives: acknowledge within one business day, plan within three, complete on a schedule set by the emergency's own driver. The emergency-change record documents the compressed timeline so the audit trail is intact.

## The publication artefact — the policy release notice

Every policy release ships with a release notice versioned in the same repo as the policy. This is the direct analogue of software release notes and of the control-library release notes shape in `../mod-102-ai-control-library-architecture/06-control-authoring-lifecycle.md`. A worked header:

```markdown
# Enterprise AI Acceptable-Use Policy 2026.2.0 (2026-Q2 release)

## Class
Major.

## Effective date
2026-07-15. Deprecation window on POL-AUP-2026.1.x closes 2026-08-12 (4 weeks).

## Summary
Adds a new normative requirement (clause 4.3) that generative-AI use for
customer-facing communications must go through a documented human review
step prior to publication. Tightens the applicability filter on clause 6
(evaluation obligations) to include agent systems, which were previously
scoped out.

## Changed clauses
- Added: 4.3 (human-review requirement).
- Modified: 6.1 (applicability filter now includes agent systems).
- Editorial: 2.4 (definition of "customer-facing" clarified; no
  interpretation change).

## Impacted downstream artefacts
- Control library: AIC-GOV-018 (v1.2 → v2.0 planned in AIC 2026.3),
  AIC-ENG-047 (applicability broadening planned in AIC 2026.3).
- PaC bundles: pac-genai-customer-facing (rule rewrite),
  pac-agent-eval-gate (applicability expansion).
- Mapping matrix: rows for NIST AI RMF GOVERN-3.2, ISO 42001 A.6.2,
  EU AI Act Article 14 revised.

## Routing acknowledgements
- ai-governance-analyst: acknowledged 2026-06-18; plan committed 2026-06-25.
- ai-risk-engineer: acknowledged 2026-06-18; plan committed 2026-06-24.
- ai-evaluation-engineer: acknowledged 2026-06-19; plan committed 2026-06-27.
- Legal: acknowledged 2026-06-18; no objections.
- head-of-AI-governance: ratified 2026-06-30.

## Rollback
If clause 4.3 causes measurable throughput impact on customer-facing
comms within 14 days of effective date, the architect + head-of-AI-
governance may revert to POL-AUP-2026.1.x for a 30-day extension while
the mitigation is redesigned. Rollback communication follows the standard
routing list.

## Migration guidance
See MIG-AUP-2026.1-to-2026.2.md.
```

The release notice is the single dereferenceable artefact for "what did this policy commit us to on this date." Every routing acknowledgement is on the record. The header is short by design — the diff and the migration guide are the long documents; the notice header is the front page.

## Coordination with the control-library release cadence

Policy publishes on its own cadence; the control library from `../mod-102-ai-control-library-architecture/06-control-authoring-lifecycle.md` publishes on a separate, coordinated cadence — typically quarterly. The two are deliberately decoupled *and* deliberately synchronised at the deprecation-window boundary.

The two-track discipline:

- Policy changes publish when they are ready (subject to notice windows). The policy release is not blocked on the control-library update.
- Control-library updates that follow a policy change land in the *next scheduled library release* after the policy's effective date. The architect coordinates so that the library update lands within the policy's deprecation window; if the policy window is four weeks and the library cadence is quarterly, the policy window is extended to bridge or the library ships an unscheduled minor.
- The bridging period — between the policy's effective date and the library update landing — is governed by the deprecation window. During bridging, downstream SoAs continue to be assessed against the old library entry (which still reflects the old policy text); the new policy is authoritative but the library-side dereference has not yet caught up. This is a known, bounded state, tracked in the analyst's traceability index.
- If the two tracks fall out of sync — a policy change lands but the library update is deferred past the deprecation window — the analyst raises a POA&M entry and the architect escalates. Sync failure is a governance event, not a rounding error.

This coordination is why the routing list includes both `ai-governance-analyst` and `ai-risk-engineer`: the analyst tracks the library-side update as a downstream consequence; the engineer tracks the PaC-bundle-side update in parallel. The two lanes cover the two artefact classes that most commonly drift when a policy changes silently.

## Two common shape mistakes

**Mistake 1 — Silent Friday publication.** A policy owner merges the change on Friday afternoon, the effective date is the following Monday, no notice packet ran, no routing acknowledgements exist. The downstream analyst finds out at the next mapping-matrix refresh two weeks later; the PaC bundles are still enforcing the old rule; the next audit hits the delta. Fix: enforce that publication requires a completed notice packet with routing acknowledgements as a pre-merge check. The architect's leverage is the notice-packet checklist; make it a gate, not a hope.

**Mistake 2 — Publication date treated as effective date.** The policy ships on Monday and is immediately enforced. Every in-flight audit and evidence cycle is retroactively judged against the new text; SoAs authored on Sunday are already out of date; PaC bundles have not been updated and now under-enforce or over-enforce. Fix: name three dates always — publication, effective, sunset — and never collapse them. The deprecation window is the tool that makes this discipline visible; use it even for minor changes when there is any downstream artefact at risk.

A third worth naming briefly: assuming email is the primary channel. Email is a delivery mechanism; the *artefact* is the notice packet in the policy repo, dereferenceable at any future date. If the only record of a change is an email thread, the audit trail has a hole. Email may be the *notification* that the packet has been published; the packet is the record.

## Summary

The policy change-communications flow is a five-class taxonomy (editorial, minor, major, breaking, emergency) governing a deterministic routing list (ai-governance-analyst at level 15, ai-risk-engineer at level 25, ai-evaluation-engineer at level 35, head-of-AI-governance at level 60, policy owner, product, legal, communications, internal audit) with class-dependent notice windows (two weeks minor, four weeks major, eight-to-twelve weeks breaking) and bounded routing SLAs (5-day acknowledge, 15-day plan, 30-day complete). Every change ships a seven-part notice packet with old/new diff, class and rationale, impacted controls (analyst-maintained), impacted PaC bundles (engineer-confirmed), three named dates, migration guidance, and rollback plan. The deprecation window overlaps old-and-new authority so in-flight audits complete cleanly and PaC bundles dual-check the new rule before it becomes authoritative. A versioned release notice in the policy repo is the single dereferenceable artefact of what changed and when. Coordination with the control-library cadence from `../mod-102-ai-control-library-architecture/06-control-authoring-lifecycle.md` runs on two tracks bridged by the deprecation window. Silent Friday publication and collapsing publication-into-effective are the two shape mistakes to guard against; the notice-packet checklist and the three-date discipline are how the architect prevents them. See `06-versioning-effective-dates-and-immutability.md` for the immutability rules that back the release notice, `../mod-108-audit-evidence-and-artefact-strategy/` for the evidence-collection cadences that consume policy effective dates, and `../mod-111-grc-tool-integration-and-vendor-strategy/` for the GRC-tooling integration points that surface the notice packet to internal audit.
