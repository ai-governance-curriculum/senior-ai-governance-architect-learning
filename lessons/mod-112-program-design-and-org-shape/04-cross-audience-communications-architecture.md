# Cross-audience communications — the architect's writing surface

## Why this chapter exists

The level-50 architect authors many documents. Most of them will be read by people whose incentives, decision-making culture, and prior context differ enough that a single document reformulated for one audience is unreadable to another. The AIMS scope statement the certification body wants to see (mod-105) is not the AIMS scope paragraph the CFO wants to see; the residual-above-appetite briefing the AI governance council needs (chapter 01) is not the residual position paper the sector regulator's examiner will read; the AI-risk portfolio slide the CEO consults is not the portfolio deck the CRO's team consults.

The failure mode this chapter designs against is the one where the architect writes one document and pushes it upward and outward without audience-shaping. The document is technically correct at every level; it is unreadable to two out of the three audiences it reaches, and each of those audiences forms its own view from the parts it did understand — usually the wrong parts. Six months later, the CEO believes the enterprise's AI-risk posture is stronger than the CRO believes, the CFO believes it is more expensive than the CIO believes, and the audit committee has heard three different versions of the same underlying artefact from three different council meetings.

The architectural correction is a **communications architecture** — a first-class artefact naming the audiences the architect writes for, the documents each audience reads, the shape and reading level of each document, the review path each document travels before it reaches its audience, and the coordination role that carries the document from author to audience. The architecture is the *writing side* of the architect's job — the day-to-day surface across which the architect's design work becomes enterprise decision-making.

## The nine audiences

The architecture names nine audiences. The list is not exhaustive but each of the nine has a distinctive reading style that the architecture's document shapes are designed for.

### 1. Chief Information Security Officer (CISO)

Reads for **integrity** — will the design hold up under adversarial pressure, does it compose with the existing security programme, what are the residual security exposures the enterprise carries. Decision-making culture is engineering-first: shows me the threat model, shows me the countermeasures, shows me the residual. Prior context: strong on classical security, moderate on ML security, catching up on AI-specific.

**Documents the architect writes for the CISO audience:**

- **Security control-family design memos** for the mod-102 `AIC-SEC-*` and `AIC-AGENT-*` families.
- **Runtime-security adjacency briefings** (mod-111 chapter 05) — how the AI-runtime-security tools compose with the existing security programme.
- **Post-market surveillance SOC interface designs** (mod-110 chapter 05) — the shared-id space, the crosswalk tables, the escalation ladder.
- **Third-party AI provider security-posture assessments** (mod-109).

Shape: technical, adversarial-mindset, with the threat model foregrounded. Reading level: technical peer.

### 2. General Counsel (GC)

Reads for **exposure** — what is the enterprise's regulatory posture, what obligations attach, where is the residual legal risk, what privilege-adjacent considerations bear. Decision-making culture is risk-and-exposure-first with a strong preference for defensible position papers. Prior context: strong on regulatory frameworks generally, moderate on AI-specific regulation and standards.

**Documents the architect writes for the GC audience:**

- **Jurisdiction-reconciled control-set version increment briefings** (mod-104).
- **Regulatory posture position papers** — the enterprise's stance on a novel regulatory question, drafted for the GC to take to council-level ratification and, where appropriate, external counsel or regulator engagement.
- **AIA / DPIA composition briefings** (mod-105 chapter 04).
- **Third-party contractual-risk briefings** (mod-109).
- **AI Act Article 26/50/72/73 obligation summaries** where the enterprise's posture is being fixed.

Shape: position-paper structure — question, applicable frameworks, options considered, recommended position, residual exposure. Reading level: technical-legal peer; the GC will typically consult external counsel and the technical detail must survive that consultation.

### 3. Chief Information Officer (CIO)

Reads for **fit** — how does the AI-governance stack compose with the existing enterprise-architecture stack, what platforms integrate, where are the build-vs-buy calls, what is the total-cost-of-ownership shape. Decision-making culture is portfolio-first with a strong preference for reference architectures and integration diagrams. Prior context: strong on enterprise architecture and integration patterns.

**Documents the architect writes for the CIO audience:**

- **GRC-for-AI reference architecture briefings** (mod-111 chapter 01).
- **Vendor-evaluation matrix summaries** (mod-111 chapter 02).
- **Composition-with-existing-enterprise-GRC-platform position papers** (mod-111 chapter 01).
- **Integration-edge design memos** (mod-111 chapters 03, 06).
- **RBAC + SoD model briefings** (mod-111 chapter 04).

Shape: reference-architecture-shaped, with the composition-with-existing-stack decisions foregrounded. Reading level: enterprise-architecture peer.

### 4. Chief Financial Officer (CFO)

Reads for **cost, throughput, and defensibility** — what does the programme cost, what work products does it produce, what is the cost-per-attestation trajectory, what is the risk of the programme being materially under-resourced. Decision-making culture is budget-first with a strong preference for cost / benefit tables and worker-productivity curves. Prior context: strong on finance and cost modelling, catching up on AI-specific.

**Documents the architect writes for the CFO audience:**

- **Hiring plan defence memos** (chapter 03).
- **Programme budget shape briefings** — the reserved-matter budget the council ratifies before the CFO submits to the board.
- **GRC-for-AI platform build-vs-buy TCO analyses** (mod-111 chapter 02).
- **Regulatory-obligation cost-forecast briefings** — when a jurisdiction adds an obligation, the cost of compliance projected forward.

Shape: cost / benefit, throughput, and sensitivity-analysis tables foregrounded. Reading level: finance peer with a technical appendix; the CFO delegates the technical read to their team.

### 5. Chief Executive Officer (CEO)

Reads for **strategy alignment and enterprise posture** — what does the AI-governance programme signal externally, what does it enable and what does it preclude in the AI-strategy portfolio, how does it position the enterprise on regulatory questions the CEO will be asked publicly. Decision-making culture is strategy-first with a strong preference for one-page executive summaries and clear position statements. Prior context: broad, non-technical.

**Documents the architect writes for the CEO audience (via head-of-AI-governance / CAO):**

- **Enterprise AI-risk posture executive summary** — one page, quarterly.
- **Regulatory-posture executive briefings** on novel questions with public-positioning implications.
- **Portfolio-level residual position** — where the enterprise stands relative to appetite, one page.

The architect rarely writes directly to the CEO; the head-of-AI-governance carries the document, and the CAO (where the seat exists) ratifies the strategic framing. The architect drafts and the head repackages; the architect's writing must survive being repackaged without losing its technical accuracy.

Shape: one-page executive summary with a technical appendix that will not be read but that must exist and be accurate. Reading level: executive.

### 6. Audit committee (of the board)

Reads for **assurance completeness and independence** — do the three lines work, are the invariants (mod-107 chapter 01) held, is internal audit competent to sample the assurance architecture, are the enterprise's disclosures to the market defensible. Decision-making culture is oversight-first with a strong preference for structured reports and clear risk-position statements. Prior context: strong on governance and controls, catching up on AI-specific.

**Documents the architect writes for the audit committee (via head-of-AI-governance):**

- **AIMS management review deck** (mod-105 chapter 07) — the Clause 9.3 output for the annual cycle.
- **Assurance architecture annual report** — the mod-107 architecture's state, the invariants held, the residual programme-level items.
- **Third-line independent audit programme annual report** — packaged from the internal audit function's own report.
- **External-audit engagement summaries** — the certification body's findings, the ForHumanity independent auditor's findings, the sector regulator examinations' outcomes.

Shape: structured report with a defensible-under-oversight tone. Reading level: senior non-executive with governance depth; the CIA is present at the reading and prepares the committee for the material.

### 7. Regulator (sector examiner or competent authority)

Reads for **conformance and traceability** — do the enterprise's controls satisfy the regulatory obligations, can the enterprise trace an obligation to a control to an attestation to evidence, is the enterprise's own view of its posture consistent with what the regulator sees. Decision-making culture is examination-first with a preference for structured, evidence-anchored responses. Prior context: strong on the specific regulatory regime, variable on AI-specific technical depth.

**Documents the architect writes for the regulator audience (via head-of-AI-governance):**

- **Position papers** — the enterprise's position on a regulator's information request or on a novel-question posture. Chapter 07 walks the writing shape.
- **Article 26 / 50 / 72 / 73 conformance packages** where EU AI Act applies.
- **SR 11-7 examination response packages** where banking-industry.
- **State-law conformance briefings** (Colorado SB24-205, NYC LL144, and adjacent) where applicable.

The architect drafts and the head reviews-and-carries. The architect's writing must be evidence-anchored — every claim traces to an artefact the regulator can be shown. Chapter 07 walks the standards-community-adjacent position-paper shape.

Shape: structured, evidence-anchored, traceable-to-artefact. Reading level: regulatory examiner peer; technical enough for the examiner's technical staff to verify but not so technical that the reviewing examiner cannot follow.

### 8. Employee (internal audience)

Reads for **applicability to their own work** — what does the AI-governance programme require of me, how does a policy change affect my day-to-day, where do I go for questions. Decision-making culture is task-first: what do I do differently on Monday. Prior context: variable, mostly non-specialist.

**Documents the architect writes for the employee audience (via HR, communications, and the head-of-AI-governance):**

- **Policy-change internal communications** — when a mod-103 policy changes, the internal briefing employees receive. Chapter 05 walks the change-management shape.
- **Training-content updates** — the syllabus updates that mod-105 chapter 06 competence programme runs.
- **Standard operating procedures** — the executable procedures that translate policy into work-instruction. The architect authors the standard-operating-procedure schema; the analyst-level and first-line seats author the specific procedures.
- **AI-use policy for employees** — the enterprise's policy on what employees may and may not do with AI tools.

Shape: task-oriented, direct, with cross-references to the underlying policy for the interested reader. Reading level: general-employee.

### 9. Customer (external audience)

Reads for **assurance-of-vendor and reputational-signal** — how does the vendor stand behind its AI systems, what disclosure and contractual commitments are on offer, what recourse exists. Decision-making culture is procurement-first for enterprise customers and consumer-protection-first for retail customers. Prior context: enterprise customers have their own second-line functions consuming this material; retail customers are general-public.

**Documents the architect writes for the customer audience (via product, legal, and the head-of-AI-governance):**

- **Customer-facing model cards / system cards** (mod-108) — the disclosure-oriented derivative of the internal card.
- **Customer DPA appendices** for AI-related processing (mod-105 chapter 04 composition).
- **Customer-facing assurance reports** — for B2B SaaS enterprises, the vendor-side assurance report the customer's second-line consumes.
- **Enterprise AI-use disclosures** — the enterprise's own public statement about how it uses AI, aligned with regulatory disclosure requirements (EU AI Act Article 50 among others).

Shape: disclosure-oriented, structured against regulatory disclosure conventions where they exist. Reading level: split — the enterprise customer's second-line reads at technical peer depth, the retail customer reads at general-public depth. The architecture requires both shapes.

## The document-and-audience matrix

The communications architecture crosswalks the documents the architect and the head-of-AI-governance produce against the audiences. A single document rarely serves a single audience alone; the matrix explicitly names each document's primary audience and the derivative shapes for adjacent audiences.

| Document (primary artefact) | Primary audience | Author | Repackager | Derivative for adjacent audiences |
|---|---|---|---|---|
| mod-102 control library — internal reference | Second-line seats | Architect | — | CIO briefing (fit), auditor read (packaged by internal audit) |
| mod-104 jurisdiction-reconciled control set | GC + head-of-AI-governance | Architect | Head-of-AI-governance | CFO cost-forecast, CEO strategy-alignment brief |
| mod-105 AIMS scope statement | 42001 certifier + audit committee | Architect | Head-of-AI-governance | CEO one-page summary, employee-facing scope statement |
| mod-106 risk register + appetite | CRO + council | Architect | Head-of-AI-governance | CFO portfolio-view, audit-committee report, CEO summary |
| mod-107 pre-deployment gate charter | Second-line seats + council | Architect | Head-of-AI-governance | CIO fit-check, audit-committee independence-check |
| mod-108 model card (internal) | Second-line seats + auditor | Model owner (per RACI) | Ai-evaluation-engineer | Customer-facing card, regulator-facing card |
| mod-109 third-party programme summary | GC + CIO + procurement | Architect | Head-of-AI-governance | CFO cost-forecast, audit-committee report |
| mod-110 PMS incident-response summary | Council + regulator | Ai-evaluation-engineer (methodology) + head-of-AI-governance (Article 73) | Head-of-AI-governance | CEO material-incident brief, audit committee, customer notification (per regime) |
| mod-111 GRC-for-AI reference architecture | CIO + council | Architect | Head-of-AI-governance | CFO TCO, CISO integration-fit |
| mod-112 chapter 01 council charter | Council + audit committee | Architect | Head-of-AI-governance | CEO governance-shape summary |
| mod-112 chapter 03 hiring plan | Council + CFO | Head-of-AI-governance (owner) + architect (drafter) | Head-of-AI-governance | Audit-committee assurance-of-competence, CHRO workforce-plan |
| Regulatory-posture position paper (novel-question) | Regulator + council | Architect (draft) + GC (co-draft) | Head-of-AI-governance | CEO/audit-committee positioning, external-counsel review |
| Standards-community position paper (chapter 07) | ISO SC 42 / CEN-CENELEC JTC 21 / IEEE etc. | Architect | Head-of-AI-governance | Council ratification, regulator-facing derivative |
| Employee AI-use policy | Employee | Architect + head-of-AI-governance + HR | Head-of-AI-governance + comms | Manager-facing training material, HR employee-relations material |
| Customer AI-disclosure | Customer | Product + head-of-AI-governance + architect | Product + GC | Regulator disclosure derivative (where required) |

The matrix's discipline is that every document names its primary audience *before* the architect starts writing. A document without a named primary audience acquires one by accident, and the accident is usually the first person to read it.

## The architecture-review-board (ARB) session — the architect's staple forum

The architecture-review-board (ARB) session is the internal forum the architect runs weekly or biweekly to bring design decisions into the operating model. The ARB is the *design-authority* forum below the AI governance council — the council ratifies enterprise-level positions, the ARB reviews architectural work that will feed into council items.

**ARB attendees:** the level-50 architect (chair); the head-of-AI-governance (attendee, not chair); the level-15 analysts, level-25 risk engineer, level-35 evaluation engineer, level-40 agentic-safety engineer, level-35 AI-infra-security seats (voting); the platform lead, MLOps lead, data engineering lead, enterprise IAM lead (voting on their scope); the CISO's technical delegate (attendee on security items); the CPO/DPO's technical delegate (attendee on privacy items).

**ARB reserved matters:** control-library updates below the council-ratification threshold, mod-108 evidence-contract updates, mod-109 third-party programme changes below the council threshold, mod-111 reference-architecture edge amendments, mod-102 SoA row updates, and the design work that will feed into upcoming council reserved-matter items.

**ARB output:** the ARB's minutes are technical-decision-of-record artefacts written to the GRC-for-AI platform (mod-111). They inform the council's pack; they do not substitute for it. Items the ARB cannot land within its authority escalate to the council through the head-of-AI-governance.

The ARB is the venue where most of the architect's writing gets its first internal read. Documents that survive the ARB well are ready for the head-of-AI-governance's repackaging into audience-shaped derivatives; documents that struggle at the ARB rarely improve when pushed upward without revision.

## The position paper — the architect's core writing artefact for external audiences

The position paper is the architect's core writing artefact for regulator, standards-community, and audit-committee audiences. It has a fixed structure the architect writes to reliably. Six sections:

1. **Question or occasion.** The specific question the paper answers or the occasion that prompted it. One paragraph.
2. **Applicable frameworks.** The regulatory, standards, and internal-policy frameworks that bear. Cite specifically; do not paraphrase.
3. **Options considered.** The realistic options the enterprise considered. Each option gets a paragraph with its shape and rationale.
4. **Recommended position.** The enterprise's proposed position, with the rationale for choosing it over the alternatives.
5. **Residual exposure.** The residual risk the enterprise carries under the recommended position. State it plainly.
6. **Cross-references.** The internal artefacts the position rests on and the seat that owns each.

Position papers are ratified by the AI governance council when they establish enterprise posture on a novel-question or standards-community-contribution matter (chapter 07). The council's minute references the paper version; the paper's cross-references map back into the artefacts the council can consult if the position is challenged later.

## The four communications-architecture invariants

Four invariants hold across the communications architecture; each is testable.

**Invariant 1 — every document has a named primary audience before drafting starts.** The audience is stated in the document header. Documents that acquired their audience during drafting or after publication are revised. Failure mode: the mod-107 gate charter is drafted for "senior stakeholders" and the CFO reads it as a budget request while the council reads it as a technical charter.

**Invariant 2 — every document that reaches an external audience has a named repackager and a review path.** The architect's technical draft does not reach the regulator, the certification body, or the customer directly; the head-of-AI-governance (or an equivalent role for customer-facing material) reviews, repackages if needed, and carries. Failure mode: the architect emails the certification body directly on a technical point; the certifier receives a document that has not been reviewed for regulatory-posture consistency.

**Invariant 3 — technical accuracy survives repackaging.** The head-of-AI-governance's repackaging for the CEO or the audit committee changes the shape but not the technical claims. The architect reviews repackaged derivatives before they are used. Failure mode: the head simplifies a residual-position statement in a way that overstates the enterprise's posture; the CEO acts on the overstatement and the CRO finds the misalignment two quarters later.

**Invariant 4 — position papers on novel questions are council-ratified before they leave the enterprise.** Every regulatory-posture or standards-community position that reaches an external audience is ratified by the AI governance council under the reserved-matters process. Failure mode: the head engages a regulator on a novel question without prior council ratification; the enterprise's position is committed by the meeting.

## The three failure modes to design against

**Failure mode 1 — the "one document, many audiences" push.** The architect writes a thorough document, the head-of-AI-governance forwards it to eight recipients, and each recipient reads a different subset. The document was correct at every level; the aggregate outcome is that the enterprise cannot answer what its position is on any specific point, because each stakeholder heard a different subset. The defence is invariant 1 — every document has a primary audience, and derivative shapes are produced for adjacent audiences.

**Failure mode 2 — the "architect goes direct".** The architect responds to a regulator's information request, or presents at a standards-community meeting, without the head-of-AI-governance's review. The material may be technically excellent; the enterprise's posture may be inconsistent with what the head is carrying elsewhere. The defence is invariant 2 — the head owns external interfaces, and the architect's role at external forums is to inform the head's carrying, not to substitute for it.

**Failure mode 3 — the "council-after-the-fact" position paper.** The enterprise's position on a novel regulatory question is committed by the head's engagement with the regulator; the council is asked to ratify the position after the enterprise's posture is already known externally. The council's ratification role is hollow. The defence is invariant 4 — novel-question position papers are ratified before external engagement, not after.

## Coordination — the roles the communications architecture interfaces with

- **AI governance council (chapter 01)** — ratifies position papers on novel questions and standards-community postures.
- **Head-of-AI-governance (level 60)** — owns external interfaces; repackages architect drafts for executive and external audiences; carries documents to regulators, certification bodies, and standards communities.
- **CAO (level 70)** — carries strategic framing and CEO-facing repackaging where the seat exists.
- **General counsel** — reviews all regulator-facing and legal-exposed material; co-drafts position papers on regulatory questions.
- **Chief communications officer** — carries employee-facing and customer-facing external communications.
- **CIA / head of internal audit** — receives audit-committee-facing material for context; does not co-author (invariant 4 of mod-107 chapter 01).
- **Product + GC** — carry customer-facing disclosures.
- **HR + communications** — carry employee-facing change communications; chapter 05 walks the change-management shape.

## Summary

The communications architecture names the nine audiences the level-50 architect writes for — CISO, GC, CIO, CFO, CEO, audit committee, regulator, employee, customer — and specifies the document classes each audience reads, the shape and reading level each document takes, the review path each document travels, and the coordination role that carries it. The document-and-audience matrix crosswalks the architect's design artefacts against the primary and derivative audiences. The architecture-review-board (ARB) is the internal weekly-or-biweekly design-authority forum the architect chairs; council-ratified reserved matters escalate through the head-of-AI-governance. The position paper is the architect's core writing artefact for external audiences and has a six-section fixed structure — question, frameworks, options, recommendation, residual, cross-references. Four invariants hold — primary audience before drafting, external documents have a named repackager, technical accuracy survives repackaging, novel-question position papers are council-ratified before external engagement — and three failure modes recur — one-document-many-audiences, architect-goes-direct, council-after-the-fact. Chapter 05 walks the change-management shape for policy and standard changes that the employee-facing communication carries; chapter 07 walks the standards-community-facing writing in more detail; chapter 08 walks the boundary to the head-of-AI-governance and CAO that the architecture terminates on.
