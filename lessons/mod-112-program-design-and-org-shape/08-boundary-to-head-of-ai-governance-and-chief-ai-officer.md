# The boundary above — head-of-AI-governance (level 60) and chief-AI-officer (level 70)

## Why this chapter exists

Every chapter so far in this module has terminated on a phrase of the form "chapter 08 walks the boundary in detail". The AI governance council's chair authority (chapter 01), the escalation ladder's terminal-step ownership (chapter 01), the operating model's programme-accountability seat (chapter 02), the hiring plan's carrier-to-CFO (chapter 03), the communications architecture's external repackager (chapter 04), the change-management plan's emergency authority (chapter 05), the certifications portfolio's cross-reference against the head's own portfolio (chapter 06), and the standards-community posture's external face (chapter 07) — all defer to the same two seats. This chapter draws the line between them and the level-50 architect (this role), so the architect knows what to do, what to draft-for-the-head, what to escalate, and what to leave alone.

The failure mode this chapter designs against is the one where the level-50 architect either *encroaches* into work the head-of-AI-governance or the CAO owns — engaging a regulator directly, taking a board-facing position, negotiating a budget shape with the CFO — or *retreats* below the level-50 scope and lets the head carry work the architect should be doing — drafting the AIMS scope statement, authoring the mod-102 control library, drafting the assurance architecture. Either failure mode produces an operating model where the two seats' work overlaps, the audit committee sees inconsistent readouts, and either the head or the architect becomes redundant.

The architectural correction is a **boundary specification** — a first-class artefact that names, for each work product and each external interface, which seat owns it and which seat carries the co-drafting or informing role. The boundary composes with the operating model's RACI (chapter 02); it is more granular because the two seats sit adjacent in the RACI's second-line column and the RACI shorthand does not fully distinguish them.

## The head-of-AI-governance (level 60) — the seat above

### What the head is

The head-of-AI-governance is the seat that carries the enterprise's AI-governance programme at the working leadership level. The head is the seat the CEO points at when asked "who runs AI governance at this company?"; the head is the seat the board's audit committee's chair calls when a regulator's letter arrives on the AI dossier; the head is the seat the CFO negotiates the programme budget with. The head is *not* the seat that authors the control library, the AIMS scope, the assurance architecture, or any of the level-50 deliverables — those are the architect's. The head is the seat that carries the outputs the architect authors to the audiences the architect does not write for directly.

The seat's five defining features:

- **Board reporting authority.** The head is the enterprise's designated seat for AI-governance reporting into the board's audit committee, risk committee, and — where an AI-specific board committee exists — that committee. The head prepares the board pack (with the architect's technical contribution), presents at the committee, and carries the committee's questions back into the programme. The architect does not attend the board committee except by invitation on a specific technical topic; the head is the seat present at every session.
- **Regulator engagement authority.** The head is the enterprise's designated seat for engagement with sector regulators, competent authorities, national data protection authorities, and standards-community bodies at the leadership tier. When the EU Article 26 obligations trigger a competent-authority interaction, the head carries the interaction; when the sector regulator schedules an examination, the head is the interface. The architect drafts the technical response and briefs the head; the head carries the response.
- **Budget ownership.** The head owns the AI-governance programme budget. The CFO negotiates the budget with the head, not with the architect. The architect defends the technical need (the operating model's seat inventory, the hiring plan's headcount, the platform's TCO); the head defends the enterprise-level shape (the programme's scope, the trajectory, the trade-offs against other enterprise priorities).
- **Line-management authority.** The head is the line manager of the level-50 architect, the level-40 agentic-safety engineer, the level-35 AI-evaluation-engineer, the level-35 AI-infra-security seat, the level-25 AI-risk-engineer, and the level-15 AI-governance-analyst cohort at the enterprise scale where all these seats exist. The head hires into the seats (chapter 03's hiring plan), sets objectives, and manages performance. The architect is a technical leader in the programme; the head is the leader-of-leaders.
- **External-face-of-the-programme authority.** The head is the seat the enterprise's peer heads at other enterprises engage with — informally through professional networks, formally through industry-body membership, and publicly through speaking engagements. The architect participates in professional networks and speaks at technical fora with the head's endorsement; the head speaks at leadership fora and represents the enterprise's public posture.

### What the head is not

The head is *not* a technical peer of the level-50 architect. Enterprises that hire heads with a strong technical background can blur the line; over time the blur is corrosive because the head cannot spend the time on technical depth that the architect does, and the head's technical calls drift out of date. The architectural discipline is that the head consults the architect on technical questions and defers to the architect's judgement on architectural matters, and the architect consults the head on programme questions and defers to the head's judgement on enterprise-level matters. The two-way deference is what keeps the two seats collaborating rather than duplicating.

The head is *not* the AI-accountable executive under ISO/IEC 42001 (mod-105 chapter 03). The AI-accountable executive is typically the CAO where the seat exists, otherwise the executive to whom the head reports (COO, CIO, CRO). The head *carries* the AI-accountable executive's programme at the working leadership level, but the accountability itself is executive-tier.

### The head-vs-architect boundary — the operational specification

The following table specifies the boundary for the level-50 architect's principal deliverables. The convention: **A** for accountable (the seat that owns the work product), **D** for drafter (the seat that authors the technical content), **R** for reviewer (the seat that reviews before publication), **C** for the carrier-to-external-audience (the seat that presents the work product to the audience outside the enterprise), and — for context — the audience-facing derivative each work product acquires when it leaves the enterprise.

| Deliverable | A | D | R | C | External derivative |
|---|---|---|---|---|---|
| mod-102 control library | architect | architect | head | architect + head at ARB | 42001 certifier packaging |
| mod-103 policy taxonomy | architect | architect | head + gc | head | audit-committee summary |
| mod-104 jurisdiction-reconciled control set | architect | architect | gc + head | head | regulator-facing derivative |
| mod-105 AIMS scope statement (Clause 4.3) | architect | architect | head | head | 42001 certifier / audit committee |
| mod-105 Clause 9.3 management review record | architect + head | analyst (drafter) + architect (technical) | head | head | audit committee / 42001 certifier |
| mod-106 risk register + appetite statement | architect | architect + risk engineer | head + cro | head | audit committee / risk committee / regulator |
| mod-107 assurance architecture | architect | architect | head + cia | head | audit committee |
| mod-107 pre-deployment gate charter | architect | architect | head | head | 42001 certifier |
| mod-107 external-audit-interface plan | head | architect (draft) + head (co-author) | gc | head | 42001 certifier / independent auditor |
| mod-108 evidence architecture | architect | architect | head | head | 42001 certifier |
| mod-109 third-party programme | architect | architect | head + gc + ciso + procurement | head | regulator / audit committee / provider |
| mod-110 PMS architecture | architect | architect | head + ciso | head | audit committee / regulator / SOC |
| mod-110 Article 73 serious-incident SOP | architect + head | architect | gc + head | head | competent authority under EU AI Act |
| mod-111 GRC-for-AI reference architecture | architect | architect | head + cio | head | audit committee / CIO |
| mod-112 ch 01 council charter | architect | architect | head + gc | head | audit committee (via chair) |
| mod-112 ch 02 operating model | architect | architect | head | head | audit committee / CHRO |
| mod-112 ch 03 hiring plan | head (owner) | architect (drafter) | head | head | CFO / audit committee |
| mod-112 ch 04 communications architecture | architect | architect | head | head | comms function |
| mod-112 ch 05 change-management plan | architect | architect | head + chro | head | audit committee / employee cascade |
| mod-112 ch 07 standards-community posture | architect | architect | head + gc | head (at leadership tier); architect (at working tier) | ISO / CEN-CENELEC / IEEE / NIST / PAI / OECD |
| mod-113 sector blueprint (per-sector) | architect | architect | head + sector-adjacent gc | head | sector regulator |
| Regulator-facing position paper (novel question) | architect + gc + head | architect (draft) + gc (co-draft) | head | head | regulator |
| Audit-committee annual programme report | head | architect (technical content) + head (framing) | cia (for context) | head | audit committee |
| CEO one-page AI-risk posture summary | head + cao | architect (source content) + head (repackaging) | cao | cao (to ceo) | ceo |
| Enterprise AI-strategy paper | cao | strategy office / cao | head + architect (technical review) | cao | ceo + board |

Three disciplines the table makes visible.

**The architect drafts; the head carries; the CAO strategises.** The architect is the technical author of nearly every deliverable in the table; the head is the reviewer, the interface to executive and audit-committee audiences, and the accountable seat for the seat-inventory-and-hiring-plan; the CAO is the accountable seat for enterprise AI strategy and the highest-tier external face. The three-way distinction — draft / carry / strategise — is what prevents any single seat from carrying more than it can defend.

**External interfaces terminate at the head, not at the architect.** With one small exception at the working tier of standards-community engagement (chapter 07's technical-committee attendance where the architect represents the enterprise on a specific standard's drafting), every external audience is reached through the head. Regulator engagement is the head's; 42001 certification-body engagement is the head's; independent-auditor engagement is the head's; the customer-facing programme summaries flow through the head; the board's audit committee sees the head, not the architect.

**Strategic AI content is the CAO's, not the head's.** The CAO owns the AI strategy paper, the CEO's AI-portfolio positioning, the AI-P&L alignment work, and the market-facing AI narrative. The head informs strategy with programme-level assurance data (what the programme can attest to; what it cannot; what it costs); the CAO composes the strategy against the programme's shape. The architect is not accountable for strategy content; the architect provides the technical realism (what the programme can and cannot do at the shape the strategy proposes).

### The head's emergency authority — the chapter-05 reference

Chapter 05 references the head's emergency authority for change management. The boundary is:

- **Emergency changes** are ratified by the AI governance council under its ad-hoc-meeting provision (chapter 01) where the council can be convened. Where the change cannot wait even for an ad-hoc meeting (defined as: 24-hour or shorter urgency, or an incident-response requirement), the head-of-AI-governance may ratify the change under emergency authority. The head's emergency authority is bounded — the head must minute the decision, notify the chair within one business day, and place the emergency change on the next council standing meeting's agenda for retroactive ratification.
- **The head's emergency authority is bounded to programme-affecting changes**, not to strategic-posture changes. A regulator's letter that provokes a serious-incident response is inside the head's emergency scope; a market-positioning question that arises from a public statement is not — that is the CAO's.
- **The head's emergency authority cannot be delegated to the architect.** The architect can prepare the emergency package (the technical response, the recommended disposition, the pack for retroactive ratification) but the head is the seat that ratifies. Where the head is unreachable, the emergency escalates to the chair (typically the CAO or the AI-accountable executive per mod-105 chapter 03), not to the architect.

## The chief-AI-officer (level 70) — the seat above the head

### What the CAO is

The CAO is the enterprise's C-suite executive for AI. The seat is a relatively new one — the market has not converged on a single title (Chief AI Officer, Chief AI and Data Officer, Chief Digital and AI Officer, VP of AI in some enterprise structures) — but the accountability is the same: enterprise AI strategy, P&L alignment for AI initiatives, executive-tier external positioning, and the board's principal AI interface. `<!-- needs-research: verify the labour-market convergence on CAO title as of authoring date; the title landscape is evolving faster than the shape of the seat -->`

The seat's four defining features:

- **AI strategy authority.** The CAO owns the enterprise's AI strategy — what the enterprise builds, buys, partners on, invests in, and declines. The strategy composes with the AI-governance programme (which is the CAO's own — the head reports to the CAO in enterprises that have a CAO seat) but the strategy's contents are the CAO's not the head's or the architect's.
- **P&L alignment authority.** The CAO owns the AI-related P&L — the revenue attributable to AI initiatives, the cost of the AI programme (including governance), and the trade-offs between them. The CAO is the seat the CFO negotiates AI-portfolio budgets with at the strategy level; the head negotiates the governance-programme budget at the operational level within the strategy the CAO has set.
- **Executive-tier external positioning.** The CAO is the enterprise's public face on AI. Public statements about AI strategy, board-level AI decisions the enterprise announces, investor-facing AI content, and the enterprise's positioning at industry leadership fora are the CAO's. The head carries the operational and regulatory positioning; the CAO carries the strategic positioning.
- **Board interface authority.** The CAO reports to the board on AI strategy at the frequency and depth the board requires. The head reports to the board's audit committee on AI-governance programme performance; the CAO reports to the full board on AI strategy and portfolio performance. The two interfaces compose without overlapping.

### What the CAO is not

The CAO is *not* accountable for the AI-governance programme's operational integrity. The head is. The CAO ratifies the programme's shape at the strategic level and confirms the resource envelope; the head runs the programme within that shape. Enterprises that collapse the head into the CAO — treating "the CAO does AI governance too" — find that the CAO's strategic bandwidth is inadequate for the operational discipline the programme requires. The seats compose; they do not substitute.

The CAO is *not* the chair of the AI governance council in all enterprise shapes. The chair is the AI-accountable executive under mod-105 chapter 03, which is typically the CAO where the seat exists but may be the COO, CIO, or CRO in enterprises without a CAO seat. Where the CAO is the chair, the head reports to the chair; where the CAO exists but is not the chair, the head reports to the chair (typically because the enterprise's AIMS accountability is placed with a broader executive scope for structural reasons) and the CAO holds the strategic-portfolio accountability without the AIMS-chair role.

### The CAO-vs-head boundary — the operational specification

The following table specifies the boundary between the CAO and the head for the deliverables that involve strategy-and-programme composition.

| Deliverable | CAO role | Head role | Boundary discipline |
|---|---|---|---|
| Enterprise AI strategy paper | Accountable + author | Consulted (on programme feasibility) | Strategy shape is CAO's; feasibility challenge is head's |
| AI-portfolio annual budget submission | Accountable | Drafts programme-budget component | CAO carries the composite to the CFO / board; head defends the programme component |
| CEO one-page quarterly AI-risk posture | Accountable | Drafts + reviews | CAO signs the summary; head vouches for technical accuracy |
| Board (full-board) AI strategy update | Accountable | Not present (programme material carried by head to audit committee separately) | Boards separate strategy from assurance; the two seats present separately |
| Board audit committee AI-governance report | Consulted (informed) | Accountable + carrier | Assurance report is the head's; CAO is informed |
| Serious-incident public statement | Accountable (composed with GC + CEO) | Consulted (on programme implications) | Public voice is executive; programme voice is operational |
| Regulator engagement on strategic posture (e.g., voluntary commitments) | Accountable | Consulted | Head carries examination-and-conformance engagement; CAO carries voluntary-commitment engagement |
| Regulator engagement on operational posture | Consulted | Accountable | Head is the operational face; CAO is informed |
| Standards-community leadership at strategy tier (e.g., a chair role at a policy-adjacent body) | Accountable (where relevant) | Consulted | Leadership at strategic tier is CAO's where the enterprise has such presence |
| Standards-community engagement at working tier | Consulted (informed) | Accountable | Working-tier engagement is the head's; the architect represents at technical drafting tier under the head's authority |
| AI-strategy alignment against jurisdictional obligations | Accountable | Consulted | Strategy fit to obligations is CAO's; programme discharge of obligations is head's |

### Where the CAO seat does not exist

Not every enterprise carries a CAO seat. Where the seat does not exist, the CAO's authority is distributed:

- **AI strategy authority** typically lands with the CEO directly, sometimes delegated to the COO, CIO, or CTO. The head is the operational seat but has no direct executive interlocutor for strategy — the head engages the CEO or the CEO's designee for strategic decisions.
- **P&L alignment authority** typically lands with the CFO in composition with the executive to whom the head reports.
- **Executive-tier external positioning** typically lands with the CEO for the highest-profile positioning and with the executive to whom the head reports for the mid-profile positioning.
- **Board interface authority** typically lands with the executive to whom the head reports.

The head's operational scope does not change with or without a CAO seat; the surrounding executive interfaces change. The architect's role does not change with or without a CAO seat; the architect always reports through the head and always defers strategic content to the CAO (where the seat exists) or to the CEO / delegated executive (where it does not).

## The three failure modes the boundary designs against

**Failure mode 1 — the architect-encroaches-upward pattern.** The level-50 architect responds to a regulator's information request directly, or presents at an industry leadership forum without the head's endorsement, or negotiates a budget component with the CFO directly, or takes a public position on AI policy at a conference. The material may be technically excellent; the enterprise's posture is compromised because the architect is speaking outside their sanctioned scope. The boundary's defence is the table's *carrier* column — the head carries external audiences; the architect informs the carrying.

**Failure mode 2 — the head-encroaches-downward pattern.** The head, often a strong technical hire, decides to author the AIMS scope statement, redraft the assurance architecture, or personally chair the pre-deployment gate. The architect is displaced from work the architect is accountable for; the head's time is spent on technical work the head cannot sustain at depth alongside the programme-leadership work. Within two quarters the head is behind on the board reporting and the technical work has drifted. The boundary's defence is the table's *drafter* column — the architect drafts; the head reviews.

**Failure mode 3 — the CAO-collapses-into-the-head pattern.** The enterprise names a "CAO" but the seat is functionally the head with a title change — the CAO drafts governance programme material, chairs the council in the operational sense, and does not carry the strategic content the CAO role requires. The board expects strategy from the CAO; the strategy is thin because the seat is occupied doing operational work. The boundary's defence is the CAO-vs-head table — strategy is the CAO's, programme is the head's, and the two are distinct seats even when the enterprise's CAO holds both by title.

## The boundary schematic — YAML shape

```yaml
boundary_specification:
  id: BOUNDARY-v1.0
  ratified_by: ai-governance-council (per chapter 01 reserved matter)
  composes_with:
    - mod-112 ch 02 operating model (raci)
    - mod-112 ch 01 council charter
    - mod-105 ch 03 ai-accountable-executive designation

  seats:
    architect:
      level: 50
      role: senior-ai-governance-architect
      reports_to: head-of-ai-governance
      accountable_for:
        - mod-102 control library
        - mod-103 policy taxonomy
        - mod-104 jurisdiction-reconciled control set
        - mod-105 aims scope + documented information (author)
        - mod-106 risk taxonomy + register schema
        - mod-107 assurance architecture
        - mod-108 evidence architecture
        - mod-109 third-party programme
        - mod-110 pms architecture
        - mod-111 grc-for-ai reference architecture
        - mod-112 chapters 01-08 (author)
        - mod-113 sector blueprints
      not_accountable_for:
        - board-reporting-carrying
        - regulator-engagement-carrying
        - budget-negotiation-with-cfo
        - line-management-of-cohort
        - executive-tier-external-positioning
        - ai-strategy
        - ai-p-and-l-alignment
    head:
      level: 60
      role: head-of-ai-governance
      reports_to: ai-accountable-executive (cao or delegated executive)
      accountable_for:
        - board-reporting-to-audit-and-risk-committees
        - regulator-engagement-at-programme-tier
        - budget-negotiation-with-cfo
        - line-management-of-second-line-cohort
        - external-face-of-programme
        - hiring-plan-ownership (drafter: architect)
        - external-audit-interface (co-author: architect)
        - article-73-serious-incident-notification
        - emergency-change-ratification-authority (bounded)
      not_accountable_for:
        - ai-strategy (cao)
        - ai-p-and-l-alignment (cao + cfo)
        - executive-tier-strategic-external-positioning (cao + ceo)
        - board-strategy-reporting (cao)
        - level-50-technical-deliverables (architect)
    cao:
      level: 70
      role: chief-ai-officer (or delegated executive where seat does not exist)
      reports_to: ceo
      accountable_for:
        - ai-strategy
        - ai-p-and-l-alignment
        - executive-tier-external-positioning
        - board-strategy-reporting (full board)
        - voluntary-commitments-engagement
        - ceo-facing-ai-portfolio-content
      not_accountable_for:
        - programme-operational-integrity (head)
        - assurance-architecture-content (architect)
        - regulator-examination-engagement (head)
        - audit-committee-assurance-reporting (head)

  deliverable_boundary_table: <see chapter table>

  emergency_authority:
    council_ad_hoc: 48-hour notice (chapter 01)
    head_emergency_authority: for <24 hour urgency; bounded to programme-affecting changes
    head_emergency_constraints:
      - minute the decision
      - notify chair within 1 business day
      - place on next council standing agenda for retroactive ratification
      - cannot be delegated to architect

  invariants:
    - id: I1
      description: architect drafts; head carries; cao strategises
      test: sample deliverables; find each seat in its column and not in the others
    - id: I2
      description: external interfaces terminate at the head (or at the cao for strategy tier)
      test: sample external engagements; find the head as carrier (or cao where strategy)
    - id: I3
      description: strategic content is the cao's
      test: sample strategy artefacts; find cao accountability
    - id: I4
      description: head's emergency authority is bounded and retroactively ratified
      test: sample emergency decisions; find retroactive council minutes
    - id: I5
      description: where cao seat does not exist, distribution is documented
      test: sample deliverables; find the seat that carries each cao-typical accountability
```

## Coordination — the roles the boundary interfaces with

- **The AI-accountable executive under mod-105 chapter 03** — the seat the AIMS names as accountable; typically the CAO where the seat exists.
- **The AI governance council chair (chapter 01)** — typically the AI-accountable executive, so typically the CAO where the seat exists.
- **The CEO** — the executive above the CAO (or the executive who carries CAO-typical accountability where the seat does not exist).
- **The board — full board and its committees** — the audit and risk committees receive from the head; the full board receives from the CAO on strategy.
- **The CFO** — negotiates the programme budget with the head, the AI-portfolio budget with the CAO.
- **The GC** — reviews regulator-facing and legal-exposed material at every tier; co-drafts regulatory posture with the architect and the head.
- **The CIA** — receives audit-committee-facing material from the head for context; is present at the council as a standing non-voting attendee (chapter 01).
- **The head's peers at other enterprises** — the informal network the head engages with; the architect participates by the head's endorsement.
- **The CAO's peers at other enterprises** — the CAO's engagement network; not the head's or the architect's.

## Summary

The boundary above the level-50 architect terminates on two seats: the head-of-AI-governance (level 60) and, where the enterprise has the seat, the chief-AI-officer (level 70). The head owns board reporting to audit and risk committees, regulator engagement at the programme tier, budget negotiation with the CFO, line management of the second-line cohort, the external face of the programme, and the emergency-change authority bounded to programme-affecting changes and retroactively ratified at the council. The CAO owns AI strategy, AI-P&L alignment, executive-tier strategic external positioning, and full-board strategy reporting. The architect drafts nearly every level-50 deliverable and briefs the head to carry each to its external audience; the head carries programme-level external audiences; the CAO carries strategic external audiences. The three failure modes — architect-encroaches-upward, head-encroaches-downward, CAO-collapses-into-head — are what the boundary defends against. Where the CAO seat does not exist, the CAO's authorities distribute across the CEO, the CFO, and the executive to whom the head reports; the head's scope and the architect's scope do not change with or without a CAO seat. The boundary is the last chapter of the module and closes the operating-model, hiring-plan, communications, change-management, certifications, and standards-community topics on a defined line of accountability the audit committee, the CEO, and the regulators can each read consistently.
