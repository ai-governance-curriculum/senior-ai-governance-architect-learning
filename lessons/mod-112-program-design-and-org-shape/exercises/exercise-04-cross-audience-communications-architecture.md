# exercise-04: Cross-Audience Communications Architecture

**Estimated effort:** 2.5 hours

## Objective

Author the **communications architecture** for the enterprise chosen in exercise-02 and — as the applied test of the architecture — draft a worked **position paper** on a novel-question regulatory posture along with three audience-specific derivatives (audit-committee brief, CEO one-page summary, employee-facing change note). Chapter 04 designs the architecture; this exercise makes the architect produce it and produce writing under it.

The deliverable set is a nine-audience map with per-audience reading style and document classes, the document-and-audience matrix crosswalking the architect's design deliverables against primary and derivative audiences, the ARB charter for the internal design-authority forum, and the worked position paper plus its three audience derivatives. The correctness spine is chapter 04's four invariants (primary audience before drafting; external documents have named repackager and review path; technical accuracy survives repackaging; novel-question papers are council-ratified before external engagement) and its three failure modes (one-document-many-audiences push; architect-goes-direct; council-after-the-fact).

## Prerequisites

- Chapter [`04-cross-audience-communications-architecture.md`](../04-cross-audience-communications-architecture.md) read once, with the nine audiences, the document-and-audience matrix shape, the ARB definition, the position-paper six-section structure, the invariants, and the failure modes marked.
- Chapter [`01-ai-governance-council-charter-and-decision-forum.md`](../01-ai-governance-council-charter-and-decision-forum.md) skimmed — the position paper's council ratification is a chapter-01 reserved matter.
- Chapter [`08-boundary-to-head-of-ai-governance-and-chief-ai-officer.md`](../08-boundary-to-head-of-ai-governance-and-chief-ai-officer.md) skimmed — the head-of-AI-governance is the external carrier for every audience the architect does not write to directly; the CAO carries strategy-tier positioning.
- The mod-104 jurisdiction-reconciled control set — the position paper's substrate for a regulator-facing novel question typically composes with mod-104 content.
- Chapter [`05-change-management-plan-for-policy-and-standard-rollouts.md`](../05-change-management-plan-for-policy-and-standard-rollouts.md) skimmed — the employee-facing derivative is a change-management artefact the change plan carries.

## Scenario

Carry the enterprise chosen in exercise-02 forward. The novel-question posture the position paper addresses is scenario-specific:

- **Bank scenario.** A sector regulator (Federal Reserve, OCC, FDIC) has requested the enterprise's position on the application of SR 11-7 model-risk-management discipline to generative-AI-enabled internal decision-support tools (e.g., loan-officer copilots) that do not directly issue credit decisions but materially inform them. The novel question: are these tools "models" under SR 11-7, are they inside the MRM perimeter, and what enterprise-side governance applies. `<!-- needs-research: verify current Federal Reserve / OCC / FDIC guidance on generative-AI applicability to SR 11-7 at authoring date; the guidance landscape is evolving fast -->`
- **SaaS vendor scenario.** A national competent authority under the EU AI Act has requested the enterprise's position on whether a specific shipped capability (a general-purpose GenAI copilot embedded in the enterprise's platform) crosses the EU AI Act's "high-risk" threshold when deployed by an enterprise customer for a use case not fully anticipated at shipment. The novel question: does responsibility for the classification-time assessment sit with the vendor (provider), the deployer (customer), or the composition, and what documented-information (Article 11 technical documentation, Article 13 transparency) satisfies the request?
- **Healthcare scenario.** The FDA has requested the enterprise's position on the applicability of the predetermined change control plan (PCCP) framework to an AI-enabled clinical-decision-support tool the enterprise ships (or uses internally) that was not initially designed against the PCCP guidance. The novel question: can the enterprise retroactively fit a PCCP to the existing tool, and what post-market surveillance obligations attach in the interim.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`audience-map-v1.0.md`** — the nine-audience map with per-audience reading style, decision-making culture, prior context, document classes, and shape / reading-level guidance.
2. **`document-audience-matrix-v1.0.md`** — the crosswalk of the architect's design deliverables (modules 102 through 112) against primary audiences and adjacent-audience derivatives.
3. **`arb-charter-v1.0.yaml`** — the architecture-review-board charter — attendees, reserved matters, cadence, ratification threshold below which the ARB lands decisions and above which it escalates to council.
4. **`position-paper-<topic>-v1.0.md`** — the worked position paper on the scenario's novel-question, structured against chapter 04's six-section shape.
5. **`derivative-set/`** — three audience-specific derivatives of the position paper:
   - `audit-committee-brief.md` — structured report for the audit committee.
   - `ceo-one-page-summary.md` — one-page executive summary.
   - `employee-change-note.md` — task-oriented internal communication for affected employees.

## Requirements

### `audience-map-v1.0.md`

One entry per chapter-04 audience (CISO, GC, CIO, CFO, CEO, audit committee, regulator, employee, customer). Every entry carries:

- **Reads for.** The audience's primary decision-driver (integrity, exposure, fit, cost-throughput-defensibility, strategy alignment, assurance completeness, conformance, applicability, assurance-of-vendor).
- **Decision-making culture.** The shape of the audience's decision process (engineering-first, position-paper-oriented, portfolio-first, budget-first, strategy-first, oversight-first, examination-first, task-first, procurement-first).
- **Prior context.** The audience's baseline knowledge (strong on X, moderate on Y, catching up on Z).
- **Document classes the architect writes.** The specific artefacts the architect produces for this audience.
- **Shape.** How each document is structured (technical-adversarial, position-paper, reference-architecture, cost/benefit, one-page-executive, structured-report, evidence-anchored, task-oriented, disclosure-oriented).
- **Reading level.** The technical depth of the audience.
- **Carrier.** The seat that carries the document to the audience (per chapter 08's boundary).

Add a scenario-specific expansion for the audiences most affected by the chosen scenario (e.g., the bank's Federal Reserve examiner as a regulator sub-shape; the SaaS vendor's enterprise-customer procurement function as a customer sub-shape; the healthcare enterprise's FDA reviewer as a regulator sub-shape).

### `document-audience-matrix-v1.0.md`

The crosswalk table with one row per architect deliverable (from chapter 04's matrix table plus any scenario-specific additions). Every row carries:

- **Document.** The design artefact (e.g., mod-104 jurisdiction-reconciled control set; mod-107 pre-deployment gate charter).
- **Primary audience.** The audience the document is drafted for.
- **Author.** The seat that drafts (typically the architect for design artefacts).
- **Repackager.** The seat that repackages for adjacent audiences (typically the head-of-AI-governance).
- **Derivative for adjacent audiences.** Named derivatives — e.g., a mod-107 gate charter has a derivative CIO fit-check and a derivative audit-committee independence-check.
- **Review path.** The seats that review before the document reaches its primary audience (typically the head; for legal-exposed material, the GC).

Include a "novel-question position paper" row where the paper being drafted in this exercise sits, so that the exercise's own artefact is placed in the matrix.

### `arb-charter-v1.0.yaml`

The ARB charter. Structure:

- **Ratifying body.** Typically the AI governance council (chapter 01) as a reserved-matter delegation; the ARB is a design-authority forum below the council.
- **Chair.** The level-50 architect.
- **Attendees.** Per chapter 04's ARB attendee list — head-of-AI-governance (attendee, not chair); level-15 analysts, level-25 risk engineer, level-35 evaluation engineer, level-40 agentic-safety engineer, level-35 AI-infra-security (voting); platform lead, MLOps lead, data engineering lead, enterprise IAM lead (voting on their scope); CISO's technical delegate (attendee on security items); CPO/DPO's technical delegate (attendee on privacy items).
- **Cadence.** Weekly or biweekly (state which and why).
- **Reserved matters.** Control-library updates below council threshold; mod-108 evidence-contract updates; mod-109 third-party programme changes below council threshold; mod-111 reference-architecture edge amendments; mod-102 SoA row updates; design work feeding into upcoming council reserved-matter items.
- **Escalation to council.** The path items that exceed ARB authority take.
- **Minute discipline.** The ARB's minutes are technical-decision-of-record artefacts written to the GRC-for-AI platform; they inform the council's pack but do not substitute.

### `position-paper-<topic>-v1.0.md`

The worked position paper. Structured against chapter 04's six-section shape:

1. **Question or occasion.** One paragraph naming the specific request that triggered the paper (Federal Reserve request; competent-authority request; FDA request), with dates and cross-reference to the correspondence identifier.
2. **Applicable frameworks.** The regulatory, standards, and internal-policy frameworks that bear. Cite specifically — SR 11-7 clause and page reference (or `<!-- needs-research -->`), EU AI Act Article and Recital numbers, ISO/IEC 42001 clauses, mod-102 control-family identifiers, mod-104 jurisdiction-reconciliation entries. Do not paraphrase framework language; if you cannot verify the exact citation, mark it.
3. **Options considered.** Three or more realistic options the enterprise considered. Each option: name, shape (what the option would commit the enterprise to), residual (what risk it carries), estimated cost / effort. Include at minimum one option the enterprise ultimately rejects with reasons.
4. **Recommended position.** The enterprise's proposed position — a paragraph clearly stating what the enterprise is proposing to do or claim. Rationale for choosing this over the alternatives. Where the position involves a novel argument (e.g., a specific interpretation of SR 11-7 applicability to generative-AI decision-support tools), state the argument clearly and cite its supporting basis.
5. **Residual exposure.** The residual regulatory / legal / operational risk the enterprise carries under the recommended position. State plainly — do not soften.
6. **Cross-references.** The internal artefacts the position rests on (mod-102 control-family versions, mod-104 reconciliation entries, mod-105 AIMS scope statement, mod-106 risk-register entries, mod-107 assurance-architecture artefacts, mod-109 third-party inventory entries, mod-110 PMS records), the seat that owns each, and the council minute id (placeholder) that will ratify the position.

At the top of the paper, name: the paper version, the drafter (architect), the co-drafter (GC), the reviewer (head-of-AI-governance), the required council ratification date (before external engagement per invariant 4), and the audience carrier (head-of-AI-governance).

### `derivative-set/`

Three files, each a repackaging of the position paper for a specific audience.

**`audit-committee-brief.md`.** Structured-report shape. Includes: the question, the enterprise's position (in the committee's oversight vocabulary), the residual exposure, the assurance the committee is being asked to note or endorse, and the third-line's role (the CIA should be attending the audit-committee meeting where this is discussed per chapter 01; the brief acknowledges this). Length: 2–3 pages.

**`ceo-one-page-summary.md`.** One-page executive shape. Includes: the question in one sentence; the recommended position in one sentence; the strategic-context implications the CAO / CEO care about (public-positioning implications, portfolio implications, precedent-setting implications); the residual exposure in one bullet; the action being requested of the CEO (typically none — the position paper is council-ratified below CEO tier — but the summary informs). Length: strict one page.

**`employee-change-note.md`.** Task-oriented shape. Includes: what changes for the employee (typically the affected employee cohort is a specific first-line group — e.g., loan officers, product managers using the affected capability, clinicians using the AI-enabled decision-support); what they should do differently starting from a defined date; where to go with questions; cross-reference to the applicable policy or standard update the change-management plan (chapter 05) rolls out. Length: 1 page.

**Invariant test in the derivative set.** Below the three derivatives, include a short `technical-accuracy-check.md` that names, for each derivative, the specific technical claims from the position paper that appear in the derivative (in altered form or full form) and confirms that the alteration did not change the substance. Chapter 04's invariant 3 (technical accuracy survives repackaging) tests against this check.

## Starter guidance

Draft the audience map first. Chapter 04's map is the floor; scenario-specific expansion is what makes the map operational. The bank's regulator sub-shape is not "regulator generally" — it is the Federal Reserve examiner's specific decision culture, which differs from the OCC examiner's, which differs from the FDIC examiner's, which differs from an SEC staff attorney's, which differs from a state banking commissioner's. Chapter 04's invariant 1 is that the primary audience is named before drafting; if the primary audience is "regulator", the paper will land badly.

The position paper's six-section structure is deceptively rigid. The temptation is to blend option analysis into the recommended-position section, or to soften residual exposure to make the recommendation more palatable. Chapter 04's failure-mode-3 defence (novel-question papers are council-ratified before external engagement) works only if the paper is honest about the residual — a council ratifying a soft residual is a council that has been misled, and the head-of-AI-governance's carrying the paper externally then commits the enterprise to a posture the council did not actually endorse. Draft the residual first, then reverse-engineer the recommendation from what residual the enterprise can defend.

The three derivatives are the invariant-3 test. If the CEO one-pager overstates the enterprise's confidence relative to the position paper (typical failure), the CEO is speaking publicly from a stronger posture than the enterprise's paper committed to. If the employee change note understates the impact (also typical), the affected employees have no reason to adopt the change, and adoption fails. Chapter 04 warns that the head's repackaging is where technical accuracy is most often lost; the architect's job is to review derivatives before publication and flag any substance drift.

The ARB is the venue where the position paper's first internal read happens. A paper that struggles at the ARB is not ready for council. Chapter 04's ARB shape is deliberate — the architect chairs, the head attends but does not chair, the technical seats vote. If the ARB's minutes on the position paper record substantive questions unanswered, the paper returns to drafting; if the ARB rubber-stamps, the ARB is failing at its function and the council will not catch the deficit.

The chapter-04 invariant 4 (novel-question papers are council-ratified before external engagement) is the discipline that most enterprises get wrong. The regulator's letter arrives, the enterprise responds within the requested window, the council ratifies the position at its next standing meeting — after the response has been sent. The response is defensible in form but the council's role is hollow. The exercise's position paper must be drafted so that ratification is possible before the response window closes; where the window is too tight, chapter 01's ad-hoc-meeting provision applies and the head convenes the council on 48 hours' notice. Skipping ratification is not an option — the position becomes the enterprise's posture, and future regulator engagements inherit it.

## Acceptance criteria

- [ ] Chosen scenario is stated at the top of `position-paper-<topic>-v1.0.md`; every artefact is coherent against it.
- [ ] `audience-map-v1.0.md` includes one entry per chapter-04 audience with reads-for, decision-making culture, prior context, document classes, shape, reading level, and carrier; scenario-specific audience expansion is present for the audience most affected by the scenario.
- [ ] `document-audience-matrix-v1.0.md` includes one row per chapter-04 default document plus the novel-question position paper; every row has document, primary audience, author, repackager, derivative for adjacent audiences, and review path.
- [ ] `arb-charter-v1.0.yaml` names ratifying body, chair (level-50 architect), attendees per chapter 04, cadence with rationale, reserved matters below council threshold, escalation path to council, and minute discipline (writes to GRC-for-AI platform).
- [ ] `position-paper-<topic>-v1.0.md` follows chapter 04's six-section shape with cited frameworks (marked with `<!-- needs-research -->` where unverified), three or more realistic options with residuals, clear recommendation, plainly stated residual exposure, and cross-references to internal artefacts and council minute id placeholder; top of paper names drafter, co-drafter, reviewer, required ratification date, and audience carrier.
- [ ] `derivative-set/` includes the three derivatives (audit-committee brief 2–3 pages, CEO one-page, employee change note 1 page) plus `technical-accuracy-check.md` confirming derivative claims match position-paper substance per invariant 3.
- [ ] Every design choice is pinnable to a chapter-04 invariant (I1–I4) as enforcer or a failure mode (one-document-many-audiences; architect-goes-direct; council-after-the-fact) as defence; a `pinning:` block or footnote in the audience map or the position paper makes this explicit.
- [ ] Every unverified specific — SR 11-7 clause references, EU AI Act article numbers, FDA guidance titles, dates, regulator programme names — carries `<!-- needs-research -->` rather than a guessed value.

## Stretch goals

- **Adversarial-review exercise.** Have a peer (or your own second read after a break) attempt to argue against the paper's recommended position using the applicable-frameworks list. Author a short response memo addressing the strongest counter-argument. The exercise tests whether the paper's residual-exposure section actually names the argument the counter-party would raise.
- **Regulator-facing cover letter.** Author the two-paragraph cover letter the head-of-AI-governance signs when submitting the position paper to the regulator. The cover letter states the enterprise's engagement posture (cooperative, timeline-committed, information-request-follow-up-available), the paper's version, and the enterprise's audit-committee-notification status.
- **Customer-facing brief.** Where the scenario has customer implications (SaaS vendor scenario particularly), author a fourth derivative — the customer-facing brief the product function issues to enterprise customers whose deployment is affected. Structure: what changes for the customer, what the vendor is doing, what the customer is being asked to do, contact for follow-up. The chapter-04 customer-audience shape applies.
