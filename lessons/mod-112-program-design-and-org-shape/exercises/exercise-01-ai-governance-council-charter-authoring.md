# exercise-01: AI Governance Council Charter Authoring

**Estimated effort:** 2 hours

## Objective

Author the **AI governance council charter** for a specified enterprise — the artefact chapter 01 designs against, ratified by the board (typically via the audit committee), signed by the AI-accountable executive, and referenced by every downstream mod-102–111 forum whose escalations terminate at the council. The deliverable set is a machine-readable charter, the reserved-matters register keyed against modules 104 / 105 / 106 / 107 / 109 / 110, a first-year meeting calendar, an escalation-packet template the ladder's step-1 originating forum uses, and a minute template with the six required fields chapter 01 names.

The correctness spine is chapter 01's five invariants and its two failure modes — the council-as-status-meeting and the council-as-rubber-stamp. Every design choice must be pinnable to an invariant (as the choice that enforces it) or to a failure mode (as the choice that defends against it). Do not reproduce the chapter's YAML schematic verbatim; the charter is an enterprise artefact and its shape reflects an enterprise-specific decision.

## Prerequisites

- Chapter [`01-ai-governance-council-charter-and-decision-forum.md`](../01-ai-governance-council-charter-and-decision-forum.md) read once, with the reserved-matters list, escalation ladder, quorum rules, and minute-keeping discipline marked.
- Chapter [`02-three-lines-of-defence-for-ai-operating-model.md`](../02-three-lines-of-defence-for-ai-operating-model.md) skimmed — the operating model's second-line seats supply the council's standing non-voting attendees and rotating advisors.
- Chapter [`08-boundary-to-head-of-ai-governance-and-chief-ai-officer.md`](../08-boundary-to-head-of-ai-governance-and-chief-ai-officer.md) skimmed — the chair, head, and CAO boundaries the charter names.
- The mod-105 chapter 03 AI-accountable-executive discussion and Clause 9.3 management-review shape — the council's quarterly cadence hosts the management review; the chair is typically the AI-accountable executive named in the AIMS.
- The mod-106 residual-above-appetite escalation, the mod-107 pre-deployment gate escalation, the mod-109 material-third-party escalation, and the mod-110 material-incident escalation designs — the four operational escalations that most commonly terminate at the council.

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose council shape you are **least familiar with**; that is where the exercise will teach you most. State your choice at the top of the charter and carry it consistently across all five artefacts.

- **A publicly listed US regional bank** with an established audit committee, an established risk committee, and an SR 11-7-aligned Model Risk Management function reporting to the CRO. There is no CAO; the AI-accountable executive under the AIMS is the CRO. The council must interlock with the existing MRM oversight forum without duplicating it — where does an MRM validation escalation land, and where does the AI governance council land items MRM does not cover (e.g., agentic-system control, jurisdictional reconciliation, third-party frontier-model onboarding).
- **A European B2B SaaS platform vendor** whose enterprise customers include public-sector deployers and whose EU AI Act exposure is central. There is a CAO who chairs the council. The customer-facing surface means that material posture changes to a shipped AI capability may require customer notification (mod-104 / mod-109 / mod-110 composition) — how does the council terminate those, and how does the ratification interlock with the CAO's strategic-external-positioning authority (chapter 08)?
- **A global healthcare payer / provider** with a clinical-safety oversight committee reporting to the CMO and an FDA-regulated SaMD product surface. The AI-accountable executive is the COO; there is no CAO. Clinical-safety review of AI-enabled clinical-decision-support is a first-line-or-second-line safety function per the mod-107 exercise-01 pattern; the council must terminate escalations from clinical-safety review that exceed CMO-side authority without collapsing clinical safety into the council.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`ai-governance-council-charter-v1.0.yaml`** — the machine-readable charter. Ratifying body, chair, membership (voting / standing / rotating), reserved-matters list, escalation ladder, cadence, quorum, decision procedure, veto rights, minute-keeping discipline, invariants.
2. **`reserved-matters-register-v1.0.md`** — the reserved-matters list expanded into a register with per-matter cross-references to modules 104 / 105 / 106 / 107 / 109 / 110 / 111 / 112 and the operational-forum whose escalation it terminates.
3. **`first-year-calendar.md`** — the first-year meeting calendar with quarterly standing (four dates), monthly working (twelve dates), and a stated ad-hoc-meeting convening path. Each standing meeting has a proposed agenda class (the reserved matters it will likely land) so that pack preparation can begin.
4. **`escalation-packet-template.md`** — the template the step-1 originating forum uses; enforces the packet-shape the ladder's step 2 (head triage) expects.
5. **`minute-template.md`** — the minute-of-record template with the six chapter-01 required fields (decision id, decision statement, rationale, vote/consensus record, executional accountability, cross-references), plus the drafter / reviewer / signer discipline and the write-to-GRC-for-AI-platform hook.

## Requirements

### `ai-governance-council-charter-v1.0.yaml`

- **Ratification.** `ratified_by:` naming the board (typically via audit committee); `ratification_date:` (may be a `TBD` placeholder); `next_review:` (annual, per invariant 5's spirit).
- **Chair.** The AI-accountable executive named in the AIMS; state which enterprise executive discharges the role in the chosen scenario and cross-reference to the mod-105 chapter 03 identification.
- **Membership — voting.** At least the chapter-01 default set (chair, CRO, CISO, GC, CIO, CPO/DPO, head-of-AI-governance, CFO, BU-executive-representatives). Adapt the vocabulary to the chosen enterprise's committee structure; every deviation from the default is annotated with rationale.
- **Membership — standing non-voting.** At minimum the level-50 architect (pack owner) and the CIA. Add CHRO and chief compliance officer where the enterprise has those seats.
- **Membership — rotating specialist advisors.** The chapter-01 set (external ethics advisor, ai-evaluation-engineer, agentic-safety-engineer, ai-risk-engineer, external counsel, frontier-model-provider representative) plus at least one scenario-specific advisor (e.g., MRM head for the bank; sector regulator liaison for the SaaS vendor; chief medical officer for the healthcare enterprise).
- **Deputies.** A `deputies:` block naming a nominated deputy per voting seat, per invariant 4 (mandatory chair-or-deputy and head-of-AI-governance for quorum).
- **Reserved matters.** Reference `reserved-matters-register-v1.0.md` by version; do not restate the register here.
- **Escalation ladder.** The four-step ladder (originating forum → head triage → pre-read → decision) with the pre-read interval stated in business days (chapter 01's default is 5). State the board-escape path (escalation to the audit or risk committee via the head) as a first-class element.
- **Cadence.** Quarterly standing + monthly working + ad-hoc convening. State the ad-hoc-convening notice period (chapter 01's default is 48 hours) and the seat authorised to convene an ad-hoc meeting (typically the chair).
- **Quorum.** State the voting-member count required, and the mandatory-seats set (chair-or-deputy AND head-of-AI-governance per chapter 01's invariant 4). Where the enterprise's scale supports it, state whether a lower quorum is defined for consent-agenda operational items on the monthly working meeting.
- **Decision procedure.** Default consensus-with-recorded-dissent; formal-vote fallback (simple majority; no chair casting vote); veto rights for head / GC / CISO with the tied-vote escalation to the board.
- **Minute-keeping.** Reference `minute-template.md` by version; state the drafter (level-15 analyst), the technical reviewer (level-50 architect), the programme reviewer (head-of-AI-governance), the signer (chair), and the turnaround (chapter 01's default is 10 business days).
- **Invariants block.** All five chapter-01 invariants (single decision forum, minuted-decision-of-record, escalation via defined ladder, mandatory quorum seats, minute turnaround) with a `test:` field per invariant naming how the enterprise verifies it.
- **Charter amendment discipline.** State that amendments to the charter are themselves a reserved matter (chapter 01's reserved-matters list includes charter amendments) and that each amendment carries a version-history entry cross-referenced to a council minute id.

### `reserved-matters-register-v1.0.md`

A table with one row per reserved matter. Every row carries:

- **Reserved matter id.** Stable identifier the charter and the operational forums cross-reference.
- **Reserved matter title.**
- **Cross-references.** The upstream module(s) whose escalation the matter terminates (e.g., `mod-106` for residual-above-appetite; `mod-104` for jurisdiction-reconciled control-set version increment; `mod-109` for material-third-party changes; `mod-110` for material-incident findings).
- **Originating operational forum.** The forum that authors the escalation packet (e.g., the mod-107 pre-deployment gate for gate escalations; the mod-106 risk-engineer function for residual escalations; the mod-105 chapter-07 AIMS management review for Clause 9.3 outputs).
- **Escalation trigger.** The specific condition that turns the operational item into a council reserved matter (e.g., "residual persists above appetite for two consecutive quarterly assessments"; "third-party provider material posture change per mod-109 chapter X threshold").
- **Decision authority.** The council authority the matter invokes (ratification, acceptance, minuting, budget approval, charter amendment).
- **Frequency expectation.** How often the reserved matter is expected to appear (annually, at increment cadence, event-driven).
- **Scenario-tailoring.** The scenario-specific adjustment for the chosen enterprise (e.g., the bank's MRM escalation route through the CRO before council; the SaaS vendor's customer-notification composition before council; the healthcare enterprise's clinical-safety-committee handoff).

At minimum, the register includes the chapter-01 default set (enterprise AI risk appetite; residual-above-appetite acceptance; jurisdiction-reconciled control-set version increments; AIMS scope changes and Clause 9.3 management review outcomes; material third-party provider changes; post-market surveillance material-incident findings; regulatory-posture ratification; standards-community contribution posture; programme budget; council charter amendments) plus at least two scenario-specific reserved matters.

### `first-year-calendar.md`

- Four quarterly standing meeting dates with the standing agenda class per meeting. At minimum, one of the four is the AIMS Clause 9.3 management review (mod-105 chapter 07).
- Twelve monthly working meeting dates with the standing agenda class per meeting (typically: residual acceptances, third-party changes, incident minutes).
- The ad-hoc-convening path: who requests, who convenes, notice period, pack expectation.
- Chapter-01's cadence-failure-mode reminder: too rarely and the council starves for throughput; too frequently and it degrades into a status meeting. State the calibration expectation — that after year one the enterprise reviews the calendar's fit against actual throughput.

### `escalation-packet-template.md`

The template with a fixed set of required sections the step-1 originating forum populates:

- **Header.** Originating forum, escalation date, target council meeting, packet author, packet reviewer.
- **Decision to be taken.** One sentence stating exactly what the council is being asked to decide.
- **Why this exceeds the forum's authority.** One paragraph naming the operational forum's authority scope and the specific reason the item exceeds it.
- **Options considered.** Each option a paragraph — shape, rationale, residual if adopted.
- **Second-line recommendation.** The recommended option with the recommender named.
- **Substantiating artefacts.** Cross-references to the pack elements the council will see (risk register entry ids, control-family attestations, evidence-artefact ids, prior related minutes).
- **Time-bound.** The date by which the escalation must be resolved (a council deferral longer than this triggers the board-escape path).

State that a packet not meeting the template is returned by the head at step 2 without council placement; the head is the packet-quality gatekeeper.

### `minute-template.md`

The minute-of-record template with the six chapter-01 required fields:

- **Decision identifier.** Stable id linking the decision to the reserved-matters register and the originating escalation packet.
- **Decision statement.** A concise sentence.
- **Rationale.** The reasoning referencing the pack evidence.
- **Vote or consensus record.** Consensus reached, or the vote count with named dissenting members and their stated reasons.
- **Executional accountability.** The named seat that owns discharging the decision and the timeline.
- **Cross-references.** To pack contents, reserved-matters register, downstream artefacts affected (risk register entries, control-library versions, AIMS scope statements).

Plus the process discipline:

- **Drafter.** Level-15 governance analyst.
- **Technical reviewer.** Level-50 architect.
- **Programme reviewer.** Head-of-AI-governance.
- **Signer.** Chair.
- **Turnaround.** Signed within 10 business days.
- **Write-to.** GRC-for-AI platform (mod-111) as first-class artefact.
- **Audit-committee access.** Default; privileged content retained separately under GC control.

## Starter guidance

Draft the reserved-matters register first. The register is what makes the charter testable — a charter without a specific register can look complete on paper and produce nothing at council-time because no one knows what "reserved matter" means in operation. The chapter-01 default list is the floor, not the ceiling; the scenario-specific additions matter (the bank's MRM composition, the SaaS vendor's customer-notification composition, the healthcare enterprise's clinical-safety composition).

The chair-vs-head boundary is subtle and worth explicitly stating. The chair sets the agenda and signs the minute; the head prepares the pack and carries the item externally. If the enterprise has no CAO, the chair is typically the CRO / COO / CIO / CTO under whom the head reports (per mod-105 chapter 03). If the enterprise has a CAO, the chair is typically the CAO. State the choice explicitly in the charter with the mod-105 chapter 03 cross-reference; downstream artefacts (chapters 04, 05, 08) inherit the choice.

Deputies are boring and load-bearing. A meeting that cannot proceed because a named voting member is travelling and no deputy was pre-named is a meeting the reserved matter deferred by chance. Chapter 01's invariant 4 is uncompromising on chair-or-deputy and head-of-AI-governance mandatory presence; the deputy discipline is what makes the invariant survive contact with executive calendars.

The escalation-packet template is the head's leverage. If step-1 originators author their own escalations to their own shape, the head triage step 2 becomes rewrite-the-packet-for-the-council instead of triage. The template must be strict and the head must enforce the strictness; the failure mode is where a permissive template lets weak packets reach the pre-read and voting members burn their preparation time on questions the packet did not answer.

Cadence balance is easy to get wrong. The chapter's default (quarterly + monthly + ad-hoc) is a starting point; enterprises with high throughput may find monthly is not enough and enterprises with lower throughput may find monthly is over-cadenced. State the calibration expectation in `first-year-calendar.md`; year one is a learning year.

## Acceptance criteria

- [ ] Chosen scenario is stated at the top of `ai-governance-council-charter-v1.0.yaml` and every artefact is coherent against it.
- [ ] Charter YAML carries ratification block, chair with mod-105 chapter 03 cross-reference, three-tier membership with deputies, reserved-matters reference, four-step escalation ladder with board-escape path, quarterly + monthly + ad-hoc cadence with 48-hour ad-hoc notice, quorum with mandatory chair-or-deputy and head-of-AI-governance, decision procedure with head / GC / CISO vetoes, minute-keeping reference, all five chapter-01 invariants with test fields, and the charter-amendment-as-reserved-matter discipline.
- [ ] `reserved-matters-register-v1.0.md` includes the chapter-01 default set plus at least two scenario-specific matters; every row carries reserved-matter id, title, cross-references, originating operational forum, escalation trigger, decision authority, frequency expectation, and scenario-tailoring.
- [ ] `first-year-calendar.md` includes four quarterly (one being the AIMS Clause 9.3 management review), twelve monthly, and the ad-hoc convening path; each meeting has a standing agenda class.
- [ ] `escalation-packet-template.md` enforces the seven required sections and states the head's step-2 gatekeeping discipline.
- [ ] `minute-template.md` carries the six required fields and the drafter / technical-reviewer / programme-reviewer / signer / turnaround / write-to / audit-committee-access discipline.
- [ ] Every design choice is pinnable to a chapter-01 invariant (I1–I5) as enforcer or to a chapter-01 failure mode (status-meeting or rubber-stamp) as defence; a `pinning:` block or footnote in the charter YAML makes this explicit.
- [ ] Every unverified specific — ISO clause wording, EU AI Act article numbers, regulator programme names — carries `<!-- needs-research -->` rather than a guessed value. Do not invent standards-body language.

## Stretch goals

- **Draft one worked escalation.** Take a plausible mod-106 residual-above-appetite scenario in the chosen enterprise, populate the escalation-packet template with realistic content, and then author the minute that would result from the council's ratification. The worked example demonstrates the flow end-to-end.
- **Chair-succession playbook.** Author a short playbook for chair succession — what happens when the AI-accountable executive changes (retirement, reassignment, external hire). The playbook names the interim-chair path, the charter re-ratification trigger, and the pack-continuity discipline.
- **First-year retrospective template.** Author the template the council uses at the end of year one to evaluate whether the cadence, quorum, and reserved-matters register are calibrated to the enterprise's actual throughput. Chapter 01's cadence-failure-mode discussion applies.
