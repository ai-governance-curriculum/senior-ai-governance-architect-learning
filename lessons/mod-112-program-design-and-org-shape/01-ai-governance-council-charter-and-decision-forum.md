# The AI governance council — the single decision forum for enterprise AI risk

## Why this chapter exists

The eleven modules that precede this one have handed the level-50 architect a full inventory of design artefacts: a control library, a policy taxonomy, a jurisdiction-reconciled control set, an AIMS, a risk taxonomy and appetite statement, an assurance architecture, an evidence architecture, a third-party programme, a post-market surveillance shape, and a GRC-for-AI reference architecture. Each of those artefacts commits the enterprise to a decision path — when a residual crosses appetite, someone decides. When a jurisdiction-reconciled control set adds a new obligation, someone ratifies. When a third-party frontier-model provider makes an announcement that reshapes exposure across the estate, someone chooses a response. When the pre-deployment gate blocks a launch and the business escalates, someone lands the call.

The failure mode this chapter designs against is the one where every one of those decisions goes to a different forum. The residual-above-appetite decision goes to the risk committee; the jurisdiction-reconciliation ratification goes to the legal committee; the third-party-response decision goes to the sourcing committee; the pre-deployment-gate escalation goes to the CTO's staff meeting. Each of those forums has other work of its own; each brings its own decision culture, quorum rules, and minutes discipline; each sees only a slice of the AI-risk picture. Within a quarter the enterprise cannot answer the question "who decided that?" for any specific AI-risk decision, and within a year the audit committee begins receiving conflicting readouts from the same underlying artefact.

The architectural answer is a **single decision forum** — the AI governance council — that terminates every AI-risk decision the enterprise makes above the operational threshold. The council does not do the work of the assurance architecture (mod-107), the risk register (mod-106), or the pre-deployment gate (mod-107 chapter 02); those forums keep their operational shape. The council is where the *residuals* of those forums that require enterprise-level ratification land, and where the enterprise's authoritative decision-of-record is minuted. This chapter designs the council's charter, membership, decision authority, escalation ladder, cadence, quorum, and minute-keeping.

## What the council is — and what it is not

The AI governance council is the enterprise's **decision-of-record forum for AI risk above the operational threshold**. Four properties define it, and the definition is what prevents the council from degrading into a status meeting or a rubber-stamp committee.

- **Decision-of-record.** The council's output is a *minuted decision* — a versioned record with an identifier, a decision statement, a rationale referencing the evidence the council saw, the vote or consensus reached, the dissenting positions if any, and the seat that carries executional accountability for the outcome. The minute is what an external auditor is shown when the auditor asks "who authorised this?". Status readouts, discussion items, and information-only briefings appear in the pack but do not produce minutes.
- **Single forum.** Every AI-risk decision above the threshold routes here — residual-above-appetite escalations (mod-106), jurisdiction-reconciled control-set ratification (mod-104), material third-party changes (mod-109), post-market surveillance findings that cross the material-incident threshold (mod-110), and the enterprise AIMS's Clause 9.3 management review (mod-105) among others. There is no parallel forum with overlapping authority. The rule the charter states plainly is: *if a decision above the threshold is not landed at the council, it is not landed*.
- **Above the operational threshold.** The council does not adjudicate every pre-deployment gate decision, every incident triage, every control test failure. Those are operational, and the second-line functions they belong to (the pre-deployment gate for mod-107, the risk-engineer function for mod-106, the incident-response function for mod-110) discharge them without council intervention. The council sees the *residuals* — the escalations the operational forums cannot land within their own authority.
- **Enterprise-level.** The council decides on behalf of the enterprise. Its decisions bind first-line delivery, second-line programme operations, and — through appropriate escalation — third-line reporting to the board. A council decision cannot be overridden by a business-unit forum; a business-unit forum's disagreement is an escalation *into* the council, not a parallel decision *away from* it.

The council is *not* a substitute for the board's audit committee or risk committee. The board committees govern the enterprise; the council operates the enterprise's AI-risk decision-making inside the board's mandate. Chapter 08 walks the boundary to the head-of-AI-governance (level 60), who carries the interface to the board committees; the council's authority terminates below that interface.

The council is *not* an "AI ethics board" in the sense that IEEE 7000-adjacent literature sometimes uses — a standalone body advising the enterprise's leadership on ethical AI use. Ethics advisory *may* be a first-class input into the council (chapter 04 walks how ethics-review artefacts land in the pack) but it is not the council itself. A separate ethics board that competes with the council for decision authority reproduces the multi-forum failure mode; a specialist ethics advisory that feeds the council does not.

The council is *not* the pre-deployment gate. The gate is a second-line operational forum (mod-107 chapter 02); the council is the enterprise decision forum. A gate escalation lands *at* the council, not *inside* it.

## Membership — voting, standing, and rotating seats

The charter names three tiers of council membership, and the distinction is what makes the council's quorum and decision authority defensible.

### Voting members

Voting members carry decision authority and their presence counts toward quorum. The default composition assumes an enterprise with the AI-accountable executive shape mod-105 chapter 03 fixed, and adapts to the enterprise's committee vocabulary:

- **Chair.** The AI-accountable executive named in the AIMS (mod-105 chapter 03) — typically the chief AI officer (level 70) where the role exists, otherwise the executive to whom the head of AI governance reports (COO, CIO, CRO depending on enterprise shape). The chair sets the agenda, calls the vote, and signs the minute. The chair does not have a casting vote — deadlocks escalate rather than resolve on the chair's authority.
- **Chief risk officer (CRO)** or nominated deputy. Carries enterprise risk framework authority; owns the reconciliation between AI-nexus risk (mod-106) and the enterprise risk taxonomy.
- **Chief information security officer (CISO)** or nominated deputy. Carries information-security programme authority; owns the AI-runtime-security adjacency (mod-111 chapter 05).
- **General counsel (GC)** or nominated deputy. Carries legal authority for jurisdiction-reconciled obligations (mod-104), contractual third-party obligations (mod-109), and regulator engagement posture.
- **Chief information officer (CIO)** or nominated deputy. Carries enterprise-architecture and platform authority; owns the reference-architecture composition contract (mod-111 chapter 01).
- **Chief privacy officer (CPO) / data protection officer (DPO)** or nominated deputy. Carries privacy-programme authority; owns the DPIA / AIA composition (mod-105 chapter 04 with mod-108 chapter 04-adjacent).
- **Head of AI governance (level 60).** Owns the second-line governance programme; carries the enterprise's AIMS accountability at the working level.
- **Chief financial officer (CFO)** or nominated deputy. Carries budget authority for AI-risk expenditure and the P&L view on AI-programme trade-offs.
- **Business-unit executive representatives.** One or two seats — depending on enterprise scale — for the business units carrying material AI exposure. Rotation policy is specified in the charter (chapter 08's boundary discussion has more).

Voting membership is deliberately senior. A common failure mode when charters are drafted at working level is that seats are named at director / senior-director grade for expediency; the council then finds it cannot land enterprise-level decisions because its members lack the authority they nominally carry. The correction is that the seats are executive-level and their nominated deputies are named in advance (not appointed at the meeting), so absence does not become a decision-blocker.

### Standing non-voting attendees

Standing non-voting attendees are present at every meeting; they inform decisions but do not vote and do not count toward quorum.

- **The level-50 architect (this role).** Owns the pack — the material the council receives — and is the working-level author of most artefacts the council rules on. The architect does not vote; the architect informs.
- **Chief internal auditor (CIA)** or head of internal audit. Attends for context, does not vote or make findings inside the council (invariant 4 from mod-107 chapter 01 — third-line does not pre-negotiate findings). The CIA's presence lets the audit committee later observe that its independent view was formed with visibility into the council's operations without being contaminated by them.
- **Chief people officer (CPO-people) / CHRO** or nominated deputy where AI-employment decisions are on the agenda. Carries workforce and change-management authority (chapter 05 in this module).
- **Chief compliance officer** where separate from the CRO / GC seats.

### Rotating specialist advisors

The charter reserves 2–4 rotating seats for specialist advisors invited by the chair for specific agenda items. Common shapes:

- **External ethics advisor** or academic advisor for topics with novel-harm content.
- **The `ai-evaluation-engineer` (level 35)** or the `agentic-safety-engineer` (level 40) for topics turning on evaluation-methodology or dangerous-capability-evaluation results.
- **The `ai-risk-engineer` (level 25)** for topics turning on the quantitative risk register.
- **External counsel** for regulatory-posture items where privileged advice is being received.
- **A frontier-model provider representative** for third-party escalations where the provider's own posture materially informs the decision (mod-109-adjacent).

Rotating advisors receive the pack under NDA if appropriate and appear only for the agenda items they are invited to. They do not vote and do not attend the executive session where the council reaches its decision.

## Decision authority — the reserved-matters list

The charter names the **reserved matters** — the decisions that only the council may take — as a positive list. Anything outside the list is an operational decision the mod-105/106/107/109/110 forums discharge without council involvement. The reserved-matters list is what makes the council's authority testable; a decision the charter did not reserve is one the council does not need to see.

Representative reserved matters (each enterprise tailors, but this is the shape):

- **Enterprise AI risk appetite** — the appetite statement (mod-106) is ratified by the council annually and re-ratified whenever a proposed change to a top-level appetite trigger is on the agenda.
- **Residual-above-appetite acceptance** — a residual that the mod-106 aggregation shows above appetite, and that the second-line has not been able to bring within appetite through control uplift within the defined SLA, is accepted (or the underlying system withdrawn, or the release deferred) by the council.
- **Jurisdiction-reconciled control-set version increments** — the mod-104 control-set (the enterprise obligation set from EU AI Act, US federal, US state, UK, Singapore, and other jurisdictions rolled into a single implementable target) is ratified at each version increment.
- **AIMS scope changes and management review outcomes** — the ISO/IEC 42001 Clause 4.3 scope statement, and the Clause 9.3 management review conclusions, are ratified by the council.
- **Material third-party provider changes** — onboarding a new frontier-model provider into the estate at a material-exposure level, exiting an existing material provider, or accepting a material change in the provider's own posture (a shift in a Responsible Scaling Policy, a change in evaluations disclosure) is ratified by the council. `<!-- needs-research: verify whether the enterprise convention for material-third-party is captured in the mod-109 chapter authored elsewhere in this track; align threshold definition on ratification -->`
- **Post-market surveillance material-incident findings** — an incident classified as material under the mod-110 incident-severity schema is minuted at the council with the incident-response completion and lessons-learned attached.
- **Regulatory-posture ratification** — the enterprise's posture on a novel regulatory question (a new state law's applicability; a novel EU AI Act Article 6 classification argument; a sector regulator's information request) is landed at the council, not decided by legal alone or by the head-of-AI-governance alone. The GC and the head coordinate the drafting; the council rules on the enterprise's position.
- **Standards-community contribution posture** — the enterprise's position it takes into ISO/IEC JTC 1/SC 42, CEN-CENELEC JTC 21, IEEE 7000, NIST public workshops, FMF, PAI, and OECD.AI (chapter 07) is ratified by the council before the head-of-AI-governance carries it externally.
- **Programme budget** — the CFO's proposed AI-risk programme budget for the coming year is ratified by the council before submission to the board's finance committee.
- **Council charter amendments** — the charter itself is the council's, and amendments (a new voting seat, a change to the quorum, a new reserved matter) are ratified by the council with an audit trail (invariant 6 from mod-107 chapter 01 — the architecture is versioned).

The reserved-matters list is *not* an operational escalation policy. Operational escalations follow the mod-106/107/110 escalation ladders and land at the council only when the escalation ladder's terminal step names the council as the resolution forum. The charter states the mapping — which operational escalations terminate at the council — as a table cross-referenced with the mod-106/107/110 designs.

## The escalation ladder — how items reach the council

The council does not accept walk-in escalations. Every item that reaches the agenda has traversed a defined escalation ladder, the ladder is documented in the originating forum's charter (mod-107 chapter 02 for the gate, mod-110 chapter 04 for the incident-response), and the item arrives with an escalation packet the ladder has produced.

The four-step ladder every escalation traverses:

1. **Operational forum.** The originating forum (gate, risk-engineer review, incident-response) attempts to land the decision within its own authority. If it cannot, it authors an *escalation packet* — a document with the decision to be taken, the reason it exceeds the forum's authority, the options considered, the second-line's recommendation, the artefacts substantiating each option, and the time-bound the escalation must be resolved by. The escalation packet is what the ladder carries upward.
2. **Head of AI governance (level 60).** The head reviews the escalation packet with the level-50 architect and either lands the decision within the head's own authority (where the enterprise's governance charter permits) or accepts the escalation for council placement. If accepted, the head owns the pack's inclusion of the escalation, the recommended council disposition, and the pre-council briefings to voting members.
3. **Council pre-read.** Voting members receive the pack a defined interval before the meeting (default 5 business days; charter to specify). Pre-read includes the escalation packet, the head's recommended disposition, the architect's technical background note, and any adjacent-forum minutes the item builds on. Members submit questions or challenges asynchronously; the head and the architect prepare responses so that the meeting time is spent on decision, not briefing.
4. **Council decision.** The council debates the item, votes or reaches consensus, and lands a minuted decision. Non-decision outcomes — deferrals for further evidence, requests for a specialist advisor, remissions to a sub-committee — are minuted with the same discipline as decisions.

An escalation that cannot be resolved at the council escalates to the board's risk or audit committee through the head. That escalation is rare — the council is the enterprise-level forum by design — but the charter names the path so that a genuinely board-level item does not stall at the council for want of an escape route.

## Cadence and quorum

**Cadence.** The council meets on a **fixed quarterly cadence** for the enterprise decision cycle, with a **standing monthly working meeting** for operational reserved-matters items (residual acceptances, third-party changes, incident minutes) that would accumulate unacceptably between quarterly meetings. In addition, the charter provides for **ad-hoc meetings** convened by the chair on 48 hours' notice for time-critical items that cannot wait for the next standing meeting — a serious-incident-driven decision, a regulator-driven decision, an urgent third-party posture change.

The quarterly meeting is the AIMS management review's principal forum where the enterprise runs an ISO/IEC 42001-aligned AIMS (mod-105 chapter 07's Clause 9.3 outputs land here). The monthly meeting handles the operational-throughput residual escalations. The ad-hoc meeting handles the exceptions.

The cadence balances two failure modes. Meeting too rarely — annual or semi-annual — starves the council of throughput and every reserved matter becomes a "we will decide this at the next meeting" delay. Meeting too frequently — weekly — turns the council into a status meeting whose decisions are not enterprise-level in character. Quarterly with monthly working plus ad-hoc is the shape that composes.

**Quorum.** The council's quorum is **five voting members, of whom the chair (or nominated deputy chair) must be one, and the head of AI governance must be one**. The quorum requirement is enough to make consensus meaningful and few enough that the meeting can proceed when one or two voting members are travelling or on leave.

The chair's mandatory presence is what preserves the enterprise-executive shape of the council; the head's mandatory presence is what preserves continuity across meetings (the head is present at every escalation ladder step by design, and the meeting cannot land a decision without the head's contextual knowledge). The absence of either is a deferral, not a proceed-anyway.

Where a nominated deputy attends for a voting member, the deputy carries the vote. The charter names the deputy in advance; a deputy nominated at the meeting is not accepted.

## Decision procedure — vote, consensus, and dissent

The council's default decision procedure is **consensus with recorded dissent**. Where consensus is reached, the minute records the decision without a vote count. Where a voting member dissents, the minute records the dissent with the member's stated reason and the member's proposed alternative if any. Where consensus cannot be reached, the chair calls a **formal vote**. The formal vote is simple majority of voting members present; the chair does not have a casting vote. In the event of a tie, the item is deferred to the next meeting or escalated to the board.

The charter reserves a **veto authority** for the head of AI governance on items where the head's programme accountability would be compromised by a decision the head believes is unsupportable — for example, a residual acceptance the head believes exceeds the enterprise's regulatory posture on the applicable jurisdiction. The veto is not a decision; it stops the decision and forces the item to the next escalation step (the board). The veto is used sparingly; the charter states that the head must minute the veto's rationale and that the head's chain of command (the AI-accountable executive) is notified within one business day.

The **general counsel** carries an analogous veto on items where the GC believes the proposed decision would create material legal exposure. The CISO carries an analogous veto on items where information-security integrity would be compromised. The three veto rights are what make the council's decisions defensible under adversarial review; they are not what usually happens.

## Minute-keeping — the audit-facing shape of the record

The council's minutes are the enterprise's decision-of-record artefact for AI risk. The minute-keeping discipline is what makes the record survive external audit, regulatory examination, and adverse-inference litigation. Six elements every minute carries:

- **Decision identifier** — a stable id linking the decision to the reserved-matters register and to the originating escalation packet.
- **Decision statement** — a concise sentence stating what the council decided.
- **Rationale** — the reasoning that led to the decision, referencing the evidence in the pack. The rationale is not a transcript; it is the enterprise's stated reason, drafted by the head and approved by the chair.
- **Vote or consensus record** — whether consensus was reached; if a formal vote, the count; if dissent, the dissenting member(s) and their stated reasons.
- **Executional accountability** — the named seat that owns discharging the decision, and the timeline within which discharge is required.
- **Cross-references** — to the pack contents (agenda item, escalation packet, technical background note, prior related minutes), to the reserved-matters register, and to the downstream artefacts the decision affects (a risk register entry updated; a control-library version pinned; an AIMS scope statement re-ratified).

Minutes are drafted by the level-15 governance analyst (per the analyst's role packet) under the head's supervision, reviewed by the level-50 architect for technical accuracy, and signed by the chair within 10 business days of the meeting. The signed minute is written to the GRC-for-AI platform (mod-111) as a first-class artefact and referenced from the AIMS documented information (mod-105 chapter 05).

The council's minutes are **audit-committee-accessible by default**. The audit committee's chair (or the CIA on the committee's behalf) can review any minute; the minute-keeping discipline is designed so that the third-line's independence is preserved because the record is complete and unamended. Where the council reaches a decision under legal privilege, the minute records the fact of the decision and the seat that carries executional accountability, and the privileged content is retained separately under GC control. The audit committee is informed that the privileged content exists and that its scope was reviewed by the GC and the head.

## The council schematic — YAML shape

```yaml
ai_governance_council:
  id: AIGC-CHARTER-v1.0
  ratified_by: board (via audit committee) on <ratification-date>
  purpose: single enterprise decision-of-record forum for AI risk above the operational threshold

  chair: ai-accountable-executive (per mod-105 ch 03; typically chief-ai-officer or COO/CIO/CRO)

  membership:
    voting:
      - chair
      - chief-risk-officer
      - chief-information-security-officer
      - general-counsel
      - chief-information-officer
      - chief-privacy-officer-or-dpo
      - head-of-ai-governance (level 60)
      - chief-financial-officer
      - business-unit-executive-representatives: 1-2 (per charter rotation)
    standing_non_voting:
      - senior-ai-governance-architect (level 50) — pack owner
      - chief-internal-auditor — context only, no findings
      - chief-people-officer / chro — for workforce items
      - chief-compliance-officer (where separate)
    rotating_specialist_advisors:
      - external-ethics-advisor
      - ai-evaluation-engineer (level 35)
      - agentic-safety-engineer (level 40)
      - ai-risk-engineer (level 25)
      - external-counsel
      - frontier-model-provider-representative

  reserved_matters:
    - enterprise-ai-risk-appetite ratification (mod-106)
    - residual-above-appetite acceptance (mod-106)
    - jurisdiction-reconciled control-set version increments (mod-104)
    - AIMS scope changes and Clause 9.3 management review outcomes (mod-105)
    - material third-party provider changes (mod-109)
    - post-market surveillance material-incident findings (mod-110)
    - regulatory-posture ratification (novel-question territory)
    - standards-community contribution posture (mod-112 ch 07)
    - programme budget approval (annual + material intra-year adjustments)
    - council charter amendments

  escalation_ladder:
    step_1: originating operational forum authors escalation packet
    step_2: head-of-ai-governance triages and either lands or accepts for council
    step_3: council pre-read (5 business days before meeting; async challenge)
    step_4: council decision (minuted per procedure)
    board_escape: escalation to board risk / audit committee via head-of-ai-governance

  cadence:
    quarterly_standing: enterprise decision cycle; AIMS management review lands here
    monthly_working: operational reserved-matters throughput
    ad_hoc: 48-hour notice; chair-convened

  quorum:
    voting_members_required: 5
    mandatory_seats: [ chair-or-deputy, head-of-ai-governance ]
    deputies: named in advance in charter; no meeting-time appointment

  decision_procedure:
    default: consensus with recorded dissent
    fallback: simple majority of voting members present; no chair casting vote
    veto_rights:
      - head-of-ai-governance: on programme-accountability items
      - general-counsel: on material-legal-exposure items
      - ciso: on information-security-integrity items
    tied_vote: defer or escalate to board

  minutes:
    drafter: level-15 governance analyst (per role packet)
    reviewer_technical: senior-ai-governance-architect (level 50)
    reviewer_programme: head-of-ai-governance (level 60)
    signer: chair
    turnaround: signed within 10 business days
    written_to: mod-111 GRC-for-AI platform as first-class artefact
    audit_committee_access: default; privileged content retained separately under GC control

  invariants:
    - id: I1
      description: single decision forum for AI risk above the operational threshold
      test: no parallel forum with overlapping reserved-matters authority exists
    - id: I2
      description: every decision produces a minuted decision-of-record
      test: sample any reserved-matter over the past year; find its minute
    - id: I3
      description: escalations arrive via the defined ladder, not as walk-ins
      test: every agenda item traces to an escalation packet + head triage
    - id: I4
      description: quorum requires chair-or-deputy and head-of-ai-governance
      test: no meeting in the record proceeded without both
    - id: I5
      description: minutes are turned around within 10 business days
      test: sample the minute-vs-meeting turnaround distribution
```

## The two failure modes to design against

**Failure mode 1 — the council-as-status-meeting.** The council meets, the pack arrives, the head reads the pack aloud, the voting members ask clarifying questions, no decisions are landed. The pack is comprehensive; the discussion is informed; the record is empty of decisions because every substantive item was "noted" or "referred for further work". Within two quarters the enterprise has no minuted authorisation for the decisions the operational forums are nominally taking on the council's behalf, and the audit committee begins to notice.

The architectural defence is *reserved-matters discipline plus a minuted-decision test*. The reserved-matters list names the decisions the council must land; the minute-keeping discipline requires that every reserved matter on the agenda produces either a minuted decision, a minuted deferral with a return date, or a minuted escalation to the board. "Noted" is not an outcome. The chair enforces the discipline; the head prepares the pack so that decision-ready items arrive at the council with the recommended disposition explicit.

**Failure mode 2 — the council-as-rubber-stamp.** The council meets, the pack arrives, the head briefs the recommended disposition, the voting members endorse without challenge, every item is minuted as "consensus, no dissent". The record looks pristine; the substance is that the council did not exercise its authority — the head's recommendation is what actually decided, and the council added no independent view. Under adversarial review the record is defensible in form but the enterprise cannot demonstrate that the council was a functioning decision forum.

The architectural defence is *asynchronous challenge before the meeting plus a challenge-recorded discipline*. The pre-read cadence exists specifically so that voting members submit challenges asynchronously; the head prepares responses; the meeting time is used on the material challenges. The minute records the challenges raised and the disposition; a meeting whose minute shows no challenges on a material item is one where the chair should ask why. The head is not permitted to bring items that the pre-read did not surface challenges on into "consent-agenda" bundling for tier-3 or higher items.

Neither failure mode is prevented by charter language alone. The chair's discipline, the head's pack quality, and the architect's technical accuracy of the pack contents are what make the charter operate. The charter defines the surface; the surface's use is what makes the council defensible.

## Coordination — the roles the council interfaces with

The council is one enterprise forum among several; the charter names the interfaces so the surrounding forums know how the council terminates on them.

- **The board's audit committee** — receives the CIA's independent view (invariant 4 from mod-107 chapter 01); receives the council's minute record on the annual audit-committee cycle; receives serious escalations that the council could not land. Chapter 08 walks the interface in detail.
- **The board's risk committee** — receives the enterprise risk appetite as ratified by the council (mod-106) as an input to the enterprise portfolio appetite; receives residuals-above-appetite as escalations where the council could not land acceptance.
- **The pre-deployment assurance gate (mod-107 chapter 02)** — is an operational forum whose escalations terminate at the council when the gate cannot land within its own authority.
- **The AIMS management review (mod-105 chapter 07)** — is a formal ISO/IEC 42001 process whose Clause 9.3 outputs land at the council's quarterly meeting as reserved matters (AIMS scope, management review outcome).
- **The risk-engineer function (mod-106)** — supplies the risk register, the appetite trigger table, and the portfolio residual view as pack inputs. The council does not adjudicate individual risk-register entries below the escalation threshold; the risk-engineer function discharges those.
- **The third-party programme (mod-109)** — supplies material provider changes as pack inputs; the council ratifies onboarding, exit, and material posture changes.
- **The post-market surveillance function (mod-110)** — supplies material incidents and PMS-lessons-learned as pack inputs; the council minutes material incidents and their disposition.
- **The head-of-ai-governance (level 60)** and **the chief-ai-officer (level 70)** — chapter 08 walks the boundary and the seats' relationship to the council in detail.

The council is where the enterprise's AI-risk decision-making is *made visible*. The surrounding forums stay operational; the council is the point where their outputs become enterprise decisions of record.

## Summary

The AI governance council is the single decision forum for enterprise AI risk above the operational threshold. It exists to prevent the multi-forum failure mode where residual-acceptance, jurisdiction-reconciliation, third-party-change, and post-market-surveillance decisions land in different committees under different quorum rules with different minute-keeping disciplines. The council is a *decision-of-record* forum whose output is a minuted decision with a stable identifier, a rationale, a vote or consensus record, executional accountability, and cross-references to the artefacts the decision affects. Membership is three-tier: voting members (chair, CRO, CISO, GC, CIO, CPO/DPO, head-of-AI-governance, CFO, BU executives), standing non-voting attendees (level-50 architect as pack owner, CIA for context, CHRO where relevant), and rotating specialist advisors (external ethics, evaluation, risk, safety-engineering, external counsel, provider representatives). Decision authority is defined by a *reserved-matters* list — enterprise appetite, residual-above-appetite, jurisdiction-reconciled control-set version, AIMS scope and management review, material third-party changes, material PMS incidents, regulatory-posture ratification, standards-community posture, programme budget, and charter amendments. Escalations arrive via a four-step ladder (operational forum → head triage → pre-read → decision); walk-ins are not accepted. The cadence is quarterly standing plus monthly working plus ad-hoc; the quorum is five voting members with chair-or-deputy and head-of-AI-governance mandatory. Decision procedure is consensus with recorded dissent, fallback simple majority, veto rights for head / GC / CISO on their programme-integrity domains. Minutes are drafted by the level-15 analyst, reviewed by the level-50 architect and head, signed by the chair within 10 business days, and written to the GRC-for-AI platform. Two failure modes recur — council-as-status-meeting and council-as-rubber-stamp — and the defences are reserved-matters discipline plus minuted-decision testing and asynchronous pre-read challenge plus challenge-recorded discipline. The chapter's schematic is the artefact exercise-01 fills in and defends.
