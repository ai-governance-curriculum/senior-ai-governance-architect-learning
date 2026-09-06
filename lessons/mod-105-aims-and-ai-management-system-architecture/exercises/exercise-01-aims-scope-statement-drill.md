# exercise-01: AIMS scope statement drill

**Estimated effort:** 2 hours

## Objective

Author a **defensible ISO/IEC 42001 AIMS scope statement** for a specified enterprise, anchored in the ISO/IEC 22989 definition of "AI system" and the ISO/IEC 23053 reference architecture, and paired with the Clause 4.1 issues register fragment and the Clause 4.2 interested-parties register fragment that back it up. The scope statement is the *anchor artefact* of the entire AIMS — the certification body reads it first, bounds their sampling with it, and holds every downstream artefact (SoA, risk-treatment plan, internal audit programme, management review) accountable to it. The point of this drill is to make the *scope decisions* explicit — what is in, what is out, what the exclusions cost, and how the scope will survive an acquisition, a divestiture, or a jurisdictional expansion.

This is the artefact the level-50 architect drafts, legal reviews, the head of AI governance signs, and top management commits to under Clause 5. The drafting is *architectural* work because every sentence in the scope statement produces obligations downstream — a phrase like "all lifecycle stages" commits the AIMS to control coverage for training pipelines it may not run, and a phrase like "the AI systems named in Appendix A" commits the AIMS to keeping Appendix A current on a defined cadence.

## Prerequisites

- Chapter [`01-the-aims-as-architectural-artefact.md`](../01-the-aims-as-architectural-artefact.md) read (why the AIMS is an architectural artefact; the six-part shape; the sibling-standards composition).
- Chapter [`02-scope-context-and-the-23053-reference-architecture.md`](../02-scope-context-and-the-23053-reference-architecture.md) read and internalised — this exercise implements what that chapter designs.
- Chapter [`03-leadership-policy-and-planning-clauses-5-and-6-1.md`](../03-leadership-policy-and-planning-clauses-5-and-6-1.md) skimmed — the scope statement bounds the risk-and-opportunity planning process, so the learner should have Clause 5 / 6.1 context.
- mod-101 chapter 06 (role-scope map) read as background — the architect designs the scope, the head of AI governance signs and operates it.
- Primary references: ISO/IEC 42001:2023 Clause 4; ISO/IEC 22989:2022 definition of "AI system"; ISO/IEC 23053:2022 reference architecture. Links in [`../resources.md`](../resources.md). Paywalled standards can be consulted via published summaries or an enterprise-purchased copy; do not paraphrase the standard text.

## Scenario

You are the level-50 AI-governance architect at **Halden Insurance Group**, a mid-sized European multi-line insurer. The group's operating shape:

- **Parent** — Halden ASA (Norway; publicly listed on Oslo Børs; roughly 6,000 FTE; EEA operating licence).
- **Subsidiaries** — Halden UK Ltd. (London-based specialty and reinsurance broker; PRA-regulated), Halden Deutschland GmbH (Munich; life and health), and Halden Specialty Inc. (Delaware; US non-admitted excess-and-surplus lines; the group's only US corporate entity).
- **Recent acquisition** — Fjord Analytics AS, a Bergen-based analytics vendor (~90 FTE; acquired 2026-04-30; integration in progress, closing FY2027).
- **Existing management systems** — a certified ISO/IEC 27001 ISMS covering the parent and the UK and Germany entities (US entity added in the last surveillance cycle); a nascent enterprise-risk-management (ERM) function based on ISO 31000. No AIMS today. Certification target for the AIMS: initial certification stage-1 in Q3 next year.
- **AI portfolio in production** — (a) three in-house pricing and reserving models used by Halden's actuarial function across P&C and life; (b) a claims-triage assistant used by the Norwegian and German claims operations (built in-house on top of a third-party GenAI foundation model API); (c) a fraud-detection classifier fleet used across all P&C claims; (d) a broker-facing conversational assistant piloted by Halden UK; (e) an embedded AI feature in a licensed policy-administration platform that the US specialty book turned on last year but did not procure separately. Fjord Analytics brings four additional models used by its consulting clients that Halden will decide about during integration.
- **Jurisdictional exposure** — EU-27 and EEA (parent and Germany); UK (subsidiary); US (Delaware, plus non-admitted operations touching most states); and Fjord's international consulting clients.
- **Regulatory environment** — EU AI Act (with Annex III insurance-and-life-and-health high-risk classification for the pricing and reserving models); UK regulator guidance (FCA and PRA on AI); US state-level (NAIC Model Bulletin 2023, adopted or in adoption in most states relevant to the US book); GDPR / UK GDPR / Norwegian personal-data law; sector-specific (Solvency II model-governance expectations for the pricing/reserving models).

Assume that the AIMS is *newly being established* and that this scope statement is the first draft submitted to the audit programme.

## Deliverables

1. **`aims-scope-statement.md`** — the one-to-three-page scope statement itself, in the shape of the worked example in chapter 02.
2. **`clause-4-1-issues-register-fragment.yaml`** — six to ten Clause 4.1 issue records (external + internal) that back the scope decisions.
3. **`clause-4-2-interested-parties-register-fragment.yaml`** — six to ten Clause 4.2 party records with relevant requirements.
4. **`scope-decisions-brief.md`** — a one-page brief that names the consequential scope decisions and their rationale.

## Requirements

### `aims-scope-statement.md`

Author the scope statement itself. Follow the chapter 02 worked-example shape. Every section is mandatory; every explicit exclusion must carry a stated justification.

At minimum, the statement must name:

- **The definitional anchor** — the sentence anchoring "AI system" in ISO/IEC 22989 and the reference architecture in ISO/IEC 23053 (and, if the portfolio includes non-ML AI, the enterprise addendum that covers it).
- **The AI systems in scope** — enumerated by product family / system class, not by individual model instance. Include the pricing and reserving model family, the claims-triage assistant, the fraud-detection classifier fleet, the broker-facing conversational assistant, and any others you decide belong in scope. Where a system is *third-party-embedded* (the US policy-administration platform's AI feature), decide whether it is in scope and state why.
- **The organisational units in scope** — by legal entity. Halden ASA, Halden UK Ltd., Halden Deutschland GmbH, Halden Specialty Inc. — plus a decision on Fjord Analytics AS. If Fjord is in scope, state as of when; if out of scope, state under what integration milestone it comes in and what interim governance holds.
- **The geographies in scope** — the AIMS's operational footprint, distinct from where individual obligations attach. State how the geographies compose with the US specialty book's multi-state non-admitted operations.
- **The lifecycle stages in scope** — design, development, evaluation, deployment, operation, decommissioning. Where a stage is undertaken by a third party (the foundation-model provider trains; Halden fine-tunes and deploys), state how the AIMS covers the enterprise's role in specifying, supervising, and accepting the third-party deliverable (cross-reference mod-109).
- **The explicit exclusions with justification** — R&D prototypes, non-AI statistical models used by finance, the Fjord Analytics client-serving portfolio pending integration, any pilot the enterprise has decided to exclude for a stated reason and a stated integration date.
- **The interfaces to the ISO/IEC 27001 ISMS** — one paragraph stating that the AIMS shares scope with the ISMS for its documented-information set, its internal audit programme, and its management review under the integrated-management-system pattern that mod-105 chapter 10 will detail. Do not duplicate the ISMS scope; reference it.

Length: one-to-three pages of prose plus enumerated lists — resist the temptation to write a five-page manifesto.

### `clause-4-1-issues-register-fragment.yaml`

Author six to ten Clause 4.1 issue records covering both *external* and *internal* categories. Use the YAML shape from chapter 02. Cover at minimum:

- **External — regulatory** — the EU AI Act high-risk-insurance-and-life-and-health classification and its downstream provider / deployer obligations.
- **External — regulatory** — Solvency II model-governance expectations for the pricing / reserving models (sector overlay on the AIMS).
- **External — standards** — the ISO/IEC 42001 family (42005 for impact assessment, 23894 for risk guidance, 23053 for reference architecture) and the enterprise's commitment to third-party certification.
- **External — stakeholder** — customer / broker / regulator expectation that AI-driven claims and pricing decisions are documented and contestable.
- **External — technology** — dependency on a third-party GenAI foundation model API for the claims-triage assistant, with concentration and continuity risk.
- **Internal — governance** — the sibling ISO/IEC 27001 ISMS and the ERM function, and the integration posture the AIMS assumes with each.
- **Internal — talent** — competence-and-role coverage across the group; where competence gaps exist that Clause 7 will have to close (see chapter 07 for the shape the plan takes).
- **Internal — portfolio** — the mix of first-party and third-party AI, the presence of a third-party-embedded AI feature the group turned on without a procurement event, the pending Fjord Analytics integration.

Each record: `issue_id`, `scope`, `category`, `title`, `description`, `relevance_to_aims` (state which downstream AIMS artefact this feeds — SoA control, risk criterion, communications channel, and so on), `owner_role`, `last_reviewed`, `review_cadence`.

### `clause-4-2-interested-parties-register-fragment.yaml`

Author six to ten Clause 4.2 interested-party records. Use the YAML shape from chapter 02. Cover at minimum:

- **Users and non-user affected persons** — policyholders, claimants, broker-side users of the conversational assistant, non-user affected persons (family members named on policies).
- **Customers** — brokers, corporate policyholders, reinsurance counterparties.
- **Regulators** — Finanstilsynet (Norwegian FSA), BaFin (Germany), FCA + PRA (UK), the EU market-surveillance authorities per member state (as they stand up under the AI Act), the Delaware Department of Insurance and the state insurance departments in the US book's material states, EIOPA for supervisory-convergence expectations.
- **Employees** — actuaries, underwriters, claims adjusters, service operations, model-development and model-validation staff.
- **Business partners** — the foundation-model provider, the policy-administration platform vendor, brokers as distribution partners.
- **Investors and analysts** — Oslo Børs disclosure regime, rating agencies.
- **Standards bodies** — ISO/IEC JTC 1/SC 42, national mirror committees the enterprise participates in.

For each record: `party_id`, `category`, `name`, `territorial_scope`, `relevant_requirements` (the *specific requirements* of this party that the AIMS is expected to meet — cite article / section where a regulatory obligation attaches), `aims_touchpoints` (which downstream AIMS artefact addresses this), `review_cadence`.

### `scope-decisions-brief.md`

One page. Must contain:

- **The three consequential scope decisions** — what you decided, and why. Common candidates from the scenario: (i) the third-party-embedded AI feature in the US policy-administration platform — in scope or excluded pending vendor cooperation? (ii) Fjord Analytics client-serving portfolio — inside the AIMS or run under an interim sub-scope AIMS extension? (iii) research prototypes — where is the tollgate at which they enter the AIMS?
- **The two exclusions the auditor is likeliest to challenge** — the ones you would defend first, with your defence pre-written. State the auditor's line of attack ("why is Fjord out?") and your response.
- **The interface to the ISMS scope statement** — one paragraph naming which AIMS-scope-statement clauses reference-and-defer to the ISMS scope statement, and where the two documents overlap (they should — that is the point of integrated management systems).
- **The scope-refresh cadence** — how often the scope is reviewed, what events trigger an off-cycle review, who owns the review, and what artefacts change downstream when the scope changes (SoA revisit? risk-register scope rebound?).

## Starter guidance

- **Do not start with "all AI at Halden".** The chapter-02 failure mode 1 is exactly the trap this drill invites. Start from the definitional anchor (ISO/IEC 22989) and the reference architecture (ISO/IEC 23053) and work outward — an AI system is a system meeting the 22989 definition, referenced against the 23053 architecture; the AIMS covers the systems meeting that definition in the named product families.
- **Enumerate by product family, not by individual model.** Individual models change monthly; product families change quarterly. The scope statement should not require monthly amendment.
- **The third-party-embedded feature is the trap.** The US specialty book's policy-administration platform has an embedded AI feature the group turned on without a procurement event. Deciding whether it is in scope is a *governance* decision, not a *technical* one — if it is out of scope, the group has an ungoverned AI system in production, which is exactly what an AIMS is supposed to prevent. Do not exclude it silently.
- **Fjord Analytics is a real problem.** A recent acquisition adds systems the parent has not audited. The chapter-02 pattern is to name the entity in the exclusion list with a stated integration date. Do not write "TBD" — write "integration in progress; brought into scope on FY2027 stage-1 audit under integration milestone X".
- **The Clause 4.1 issues register is a *trace*, not a manifesto.** Every issue should trace forward to something downstream in the AIMS — an Annex A control, a risk criterion, a communications channel, a competence-plan gap. If you cannot state the downstream trace, the issue does not belong in the register.
- **The Clause 4.2 interested-parties register is *specific*, not exhaustive.** "The public" is not an interested party in the sense Clause 4.2 uses. "The Finanstilsynet supervisory function" is. "Policyholders in EU-27 markets whose claims are triaged by the assistant" is. Prefer specificity.
- **Use `<!-- needs-research: ... -->` freely.** Do not invent a Solvency II article number or an EIOPA guidance document title you have not verified. Mark it and move on.

## Acceptance criteria

- [ ] The scope statement anchors "AI system" in ISO/IEC 22989 and the reference architecture in ISO/IEC 23053 in a single sentence at the top of the document.
- [ ] Every AI system in the scenario portfolio is named in one of: the in-scope list, the exclusion list (with justification), or the deferred-integration list (with an integration milestone). Nothing is silently omitted.
- [ ] Every organisational unit in the scenario is named in-scope or explicitly excluded. Fjord Analytics is handled with an integration milestone; the US specialty book's embedded AI feature is decided explicitly.
- [ ] The scope statement runs one-to-three pages; it does not sprawl and it does not underspecify.
- [ ] The Clause 4.1 register has at least six records, covers both external and internal, and every record names a downstream AIMS touchpoint (SoA control, risk criterion, communications channel, competence gap, or similar).
- [ ] The Clause 4.2 register has at least six records and, for each, states the specific requirements the party has of the AIMS — not vague sentiment, but citable requirements where the party is a regulator.
- [ ] The scope-decisions brief names three consequential decisions and the auditor's two most-likely challenges with a pre-written defence for each.
- [ ] The scope statement references the ISO/IEC 27001 ISMS scope for shared documentation and audit programme rather than duplicating it.
- [ ] No unverifiable citation is invented; unverified references are marked with `<!-- needs-research: ... -->`.

## Stretch goals

- **Author a *scope-change control procedure*** — the one-page procedure the head of AI governance uses when the scope needs to change (Fjord integration lands, a new subsidiary is acquired, a portfolio segment is divested, a new jurisdiction is entered). State who initiates, who reviews, who signs, and which downstream artefacts must be re-baselined.
- **Sketch the *AIMS glossary fragment*** — the ten-to-fifteen terms the scope statement uses that need enterprise-specific definitions ("AI system", "AI model", "AI product family", "material change", "third-party AI", "embedded AI feature", "consequential decision"), anchored where possible in ISO/IEC 22989 and 23053, and marked where the enterprise extends the standard vocabulary.
- **Produce a *stage-1 auditor's-eye read* of your scope statement** — a half-page annotation identifying the five questions an ISO/IEC 42006-conformant auditor would ask on day one of stage-1 and how the scope statement answers each.
- **Draft the *integration diagram*** — a one-page diagram showing how the AIMS scope, the ISMS scope, and (where applicable) any privacy-management-system scope compose. Use the mod-105 chapter 10 pattern as reference.
- **Handle a *counter-scenario*** — assume Halden decides to divest the US specialty book in Q2 next year. What one-paragraph amendment to the scope statement would you propose, and what downstream artefacts change?
