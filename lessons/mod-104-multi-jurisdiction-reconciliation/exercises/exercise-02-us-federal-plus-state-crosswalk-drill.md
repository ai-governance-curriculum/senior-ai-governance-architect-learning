# exercise-02: US federal-plus-state crosswalk drill

**Estimated effort:** 3 hours

## Objective

Produce the **US-federal-plus-state crosswalk** for an enterprise control library — the applicability-filter attributes, the crosswalk edges, and the small set of shape-B controls the multi-jurisdiction overlay actually requires — for a single AI system operating across the US federal frame *and* three or more state regimes simultaneously.

The point of this drill is the overlay decision-making, not the memorisation. When the same underlying human-oversight practice must discharge OMB M-25-21 (federal-agency customer), Colorado SB24-205 (deployer of a consequential-decision system in Colorado), NYC LL 144 (employer using an AEDT for candidates in NYC), and Illinois HB 3773 (employer using AI in an IHRA-covered employment decision), a naïve four-controls-per-regime architecture produces four duplicated adverse-impact monitors and four inconsistent notice templates. The reconciliation architecture produces one adverse-impact monitor, one notice template with four renderings, and one applicability filter that carries the four jurisdictions cleanly. This exercise makes you do the overlay decomposition by hand so the pattern is durable.

The deliverable is what the level-50 architect would hand to internal legal, the head-of-AI-governance, and the ai-risk-engineer at the start of a multi-state rollout so scope is agreed *before* any control-side implementation begins.

## Prerequisites

- Chapter [`04-the-us-federal-frame.md`](../04-the-us-federal-frame.md) read (executive-order transitions, OMB memoranda, US AISI methodology, sector regulators).
- Chapter [`05-the-us-state-and-municipal-patchwork.md`](../05-the-us-state-and-municipal-patchwork.md) read (Colorado, NYC LL 144, EEOC guidance, California trio, Utah, Texas TRAIGA, Illinois, and the watch list).
- Chapter [`07-designing-the-reconciliation-architecture.md`](../07-designing-the-reconciliation-architecture.md) skimmed for the obligation-record, applicability-filter, and evidence-contract schemas — you will produce fragments in those shapes.
- Exercise [`exercise-01-eu-ai-act-articles-to-controls-map.md`](exercise-01-eu-ai-act-articles-to-controls-map.md) completed or reviewed — the atomic-requirement decomposition discipline is the same here.
- Primary sources: OMB M-25-21 and M-25-22 (federal-agency AI use and acquisition), Colorado SB24-205, NYC LL 144 and DCWP rules, EEOC 2023 adverse-impact technical assistance, California SB 942 and AB 2013 and the CPPA ADMT rulemaking record, Illinois HB 3773. Links live in [`../resources.md`](../resources.md).

## Scenario

You are the level-50 architect at **Northbrook Financial Services** (the scenario used in mod-102 exercise-01 — a US regional bank; extend it for this drill). The bank has decided to launch **Northbrook Career Match**, an AI-assisted internal-mobility platform, in Q2. Northbrook Career Match:

- Screens internal-employee résumés against internally-posted requisitions and produces a *ranked shortlist* of candidates plus a *narrative rationale* for each ranking; a human recruiter makes the final decision.
- Is trained on Northbrook's own historical hiring data plus a licensed third-party skills-taxonomy dataset.
- Is deployed initially in Northbrook's offices in New York City (headquarters), Chicago, Denver, Los Angeles, Dallas, and Salt Lake City, with a plan to add Boston and Seattle in Q4.
- Is also being offered as a *white-label service* to Northbrook's US federal government client (a single agency using it for internal federal-workforce mobility, procured under a contract governed by OMB M-25-22 flow-down clauses). That deployment is under an FY26 contract.
- Uses a foundation-model backend from a third-party GPAI provider (which is itself a signatory to the Hiroshima Code and holds a US AISI voluntary agreement) accessed via API, not fine-tuned by Northbrook.

Assume Northbrook already has a working control library carrying the families named in mod-102: `AIC-GOV-*`, `AIC-DAT-*`, `AIC-DOC-*`, `AIC-LOG-*`, `AIC-HOV-*`, `AIC-ROB-*`, `AIC-SEC-*`, `AIC-TRP-*`, `AIC-RSK-*`, `AIC-INC-*`, and an `AIC-EMP-*` employment-AI family that predates the Career Match project. The library already carries EU AI Act crosswalks from exercise-01. This exercise is the US overlay on top.

## Deliverables

1. **`us-jurisdictional-scope-matrix.md`** — the matrix of every US regime attaching to Career Match, per deployment site.
2. **`us-overlay-obligation-decomposition.md`** — the atomic-requirement decomposition for the eight regimes the scenario surfaces.
3. **`overlay-shape-decisions.yaml`** — YAML sketches of the applicability-filter fragments and the two-to-four shape-B controls the overlay actually needs.
4. **`review-brief.md`** — a one-page brief to the head-of-AI-governance.

## Requirements

### `us-jurisdictional-scope-matrix.md`

Produce a matrix whose rows are the eight regimes the scenario surfaces and whose columns are the deployment sites plus the federal customer:

| Regime | Northbrook role | NYC HQ | Chicago | Denver | LA | Dallas | Salt Lake City | Boston (Q4) | Seattle (Q4) | Federal client |
|---|---|---|---|---|---|---|---|---|---|---|
| OMB M-25-21 (federal-agency AI use) | vendor | | | | | | | | | ✓ |
| OMB M-25-22 (federal AI acquisition, flowed-down) | vendor | | | | | | | | | ✓ |
| NYC LL 144 (AEDT bias audit + notice) | employer | ✓ | | | | | | | | |
| Colorado SB24-205 (consequential-decision, developer + deployer) | ? | | | ✓ | | | | | | |
| Illinois HB 3773 (IHRA amendments) | employer | | ✓ | | | | | | | |
| California CPPA ADMT (significant-decision ADMT) | ? | | | | ✓ | | | | | |
| Utah SB 149 (GenAI disclosure in regulated occupations / consumer transactions) | ? | | | | | | ✓ | | | |
| Texas TRAIGA (HB 149) | ? | | | | | ✓ | | | | |
| EEOC 2023 adverse-impact guidance | employer | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |

For each regime, fill:

- **Northbrook role** — developer / deployer / employer / vendor / controller / GPAI-integrator (choose from the enumerated set in chapter 07). Note that Northbrook may play *different roles* per regime — as employer for its own hiring, as vendor for the federal client. Where the role is ambiguous, mark `?` and add an open question for legal.
- **The site cells** — mark ✓ where the regime attaches, ⚠ where the regime attaches conditionally (state a one-line condition), and leave blank where it clearly does not attach. Do not mark a cell without a reason.

Below the matrix, capture:

- **Effective-date staging** — the effective date of each regime as of authoring time (marked with `<!-- needs-research: ... -->` for the ones that have moved since your primary source was published).
- **Trigger-differentiation notes** — where two regimes look similar but trigger differently (LL 144 attaches on AEDT-shape *tools*; Illinois HB 3773 attaches on *employer use of AI* in employment decisions; California CPPA ADMT attaches on *businesses subject to CCPA* using ADMT for significant decisions — the differentiation is where the exercise pays off).

### `us-overlay-obligation-decomposition.md`

For each of the eight regimes, decompose the *provisions Career Match hits* into atomic requirements — do not attempt to decompose the whole regime, only the requirements this system triggers. For each atomic requirement capture, using the same table shape as exercise-01:

| Field | Content |
|---|---|
| Obligation identifier (proposed) | `OBL-<regime-short>-<citation>-<short-slug>` |
| Atomic requirement | One-sentence restatement |
| Addressee | Northbrook's role for this obligation |
| Trigger (site + system attribute) | The applicability-filter conjunction that turns it on |
| Demand category | design_state / process / document / test / disclosure / notification / filing |
| Existing control family (if shape A) | The `AIC-*` family the crosswalk edge attaches to |
| New control id (if shape B) | Proposed id |
| Effective date | Per your matrix |
| Consequence type | administrative_penalty / private_right_of_action / supervisory_action / procurement_disqualification / reputational / contractual |

Cover at minimum:

- **OMB M-25-21 / M-25-22** — the *flow-down* subset that lands in Northbrook's federal contract. Which practices does the federal customer expect Northbrook to *look like*, and which artefacts must Northbrook produce pre-award and during performance? Cite the specific NIST AI RMF sub-categories the memoranda anchor to. Mark unverified specifics with `<!-- needs-research: ... -->`.
- **Colorado SB24-205** — the developer-vs-deployer question for Northbrook. Because Career Match makes a *ranked shortlist* that a human recruiter uses, is the Colorado *substantial-factor* test met? Flag as legal question but produce a mapping under *both* interpretations so the mapping is decision-ready.
- **NYC LL 144** — the bias-audit-by-independent-auditor obligation, the public summary, the ten-business-day candidate notice. Where does each land? Note the independent-auditor definition and its interaction with any Northbrook-internal audit function.
- **Illinois HB 3773** — the notice-to-employee obligation and the discrimination prohibition. How does it compose with the NYC LL 144 notice? Same notice, two renderings, or two notices?
- **California CPPA ADMT** — the significant-decision pre-use notice, the opt-out mechanism (state whether it applies to internal-mobility use cases), and the risk-assessment shape. Mark the ADMT adoption-status question with `<!-- needs-research: ... -->`.
- **Utah SB 149** — does the regulated-occupation clause attach to internal-mobility screening? Almost certainly no; state your reason. Does the consumer-transaction clause attach if a rejected internal candidate is treated as a "consumer" under Utah's definition? Flag as legal question.
- **Texas TRAIGA** — the impact-assessment and disclosure obligations for the Dallas office deployment. Which of TRAIGA's prohibitions could Career Match plausibly implicate (probably none, but state your reason)?
- **EEOC 2023 guidance** — the four-fifths-rule presumptive trigger and the reasonable-accommodation obligation under ADA. This is federal *and* applies everywhere; where does it enter the crosswalk?

### `overlay-shape-decisions.yaml`

Produce YAML fragments — not full library entries — for:

1. **The multi-jurisdictional applicability filter** for Career Match's *adverse-impact-monitoring* control (the single control that discharges LL 144's bias audit obligation and EEOC's adverse-impact analysis and Colorado's algorithmic-discrimination monitoring and Illinois's discrimination monitoring). Follow the chapter-07 applicability-filter shape. Include an `evidence_mode` extension if you can foresee where the standards route matters (usually irrelevant for this drill; note if so).

2. **The multi-jurisdictional applicability filter** for Career Match's *candidate-notice* control (which discharges LL 144's ten-business-day notice, Illinois HB 3773's employee notice, Colorado's consumer-notice for a consequential decision if Northbrook is the deployer, California CPPA ADMT pre-use notice if applicable, and Utah's disclosure if applicable).

3. **Two or three shape-B control skeletons** — id, one-sentence statement, applicability filter, evidence contract with per-obligation renderings. Choose from:
   - `AIC-EMP-INDEP-AUDIT-<n>` — the LL 144 independent-auditor bias audit and published summary (which does not compose cleanly with an internal audit function).
   - `AIC-EMP-FED-VENDOR-ARTEFACTS-<n>` — the pre-award and in-performance artefact set the federal customer expects under M-25-22 flow-down (model card in the memorandum's shape, monitoring narrative, incident-notification channel).
   - `AIC-EMP-COLO-AG-NOTICE-<n>` — the Colorado attorney-general notification obligation for known/foreseeable algorithmic discrimination.
   - `AIC-EMP-SB942-DETECTION-<n>` — probably *not* triggered for Career Match; note explicitly *why not* to demonstrate you evaluated it.

For each shape-B skeleton, include the per-obligation-rendering block from chapter 07 for at least the recipient / language / retention differences. Do not fully author the implementation-guidance or testing-procedure sections — a skeleton is fine.

### `review-brief.md`

One page. Must contain:

- **The three most consequential shape decisions** (which regime forced a shape-B control? which two regimes ended up on the same shape-A control? which shape-B control was *rejected* and why?).
- **The open questions for legal** — role attribution (developer vs deployer for Colorado; controller / business-under-CCPA for California ADMT), scope of "consumer" for Utah SB 149 in an internal-mobility context, whether the Q4 Boston / Seattle deployment triggers a Boston or Massachusetts-specific regime not on the matrix.
- **The open questions for the head-of-AI-governance** — should Northbrook decline the federal client for Career Match given the M-25-22 flow-down cost, or is the artefact set covered by the shape-A extensions already planned? Should Northbrook designate an internal independent-auditor function or contract an external auditor for LL 144?
- **The executive-order-transition posture** — the one paragraph stating how the mapping survives the next administration change (should be trivial if you anchored to memoranda and NIST AI RMF, not to the executive order).

## Starter guidance

- **Start from the site, not the regime.** For each deployment site (NYC, Chicago, Denver, LA, Dallas, Salt Lake City, Boston, Seattle, federal customer), enumerate the regimes that attach and *then* aggregate to the regime view. This surfaces overlap and prevents you from missing federal + state stacking.
- **Do not create one control per state.** The failure mode chapter 5 warns about is exactly the one the drill invites. Every time you are tempted to author a Colorado adverse-impact control alongside a New York adverse-impact control, stop and extend the applicability filter of the underlying control.
- **The four-fifths rule is EEOC, not a state statute.** Do not attach the four-fifths rule to LL 144 or Illinois HB 3773 directly. It is a *methodology* referenced across employment-AI regimes; it belongs in the evidence contract on the adverse-impact control, not in the applicability filter.
- **Foundation-model integration matters.** Northbrook uses a third-party GPAI backend. Which of the eight regimes attach to *Northbrook's use of a third-party model* vs *the model provider's own obligations*? OMB M-25-22 flow-down almost certainly makes Northbrook responsible for evidence about the model even where it does not own the model — the mod-109 supply-chain-governance discussion is relevant.
- **Effective-date staging is not decorative.** Career Match launches in Q2. If Colorado SB24-205's 1 February 2026 effective date has passed at rollout, controls apply from day one; if a regime's effective date is after rollout, note that the applicability filter is inactive until the date and the auditor cannot credit controls prospectively.
- **Mark unverified specifics with `<!-- needs-research: ... -->`.** Do not invent memorandum paragraph numbers, DCWP rule references, or CPPA rule sections you have not verified against the primary source at authoring time.

## Acceptance criteria

- [ ] The jurisdictional-scope matrix covers all eight regimes for all nine deployment locations (federal client included) with every cell justified either by a ✓/⚠ or by a reasoned blank.
- [ ] Northbrook's role is stated explicitly for each regime and — where Northbrook plays multiple roles — the multiplicity is called out.
- [ ] The atomic-requirement decomposition treats *this system's provisions*, not the whole regime — no over-coverage, no under-coverage.
- [ ] Every shape-A decision names the existing `AIC-*` family the crosswalk edge attaches to.
- [ ] Every shape-B decision carries a written rationale that argues why shape A is not sufficient. Shape-B choices without rationale are not accepted.
- [ ] The two multi-jurisdictional applicability filters compile as boolean expressions in the chapter-07 vocabulary — the reader can trace which regime turns which branch on.
- [ ] The shape-B control skeletons include per-obligation renderings that differ where they should (Colorado AG in English, ADMT rendering in California-facing content, etc.) and share what should be shared (base artefacts).
- [ ] The review brief carries at least one legal open question and one governance open question.
- [ ] The executive-order-transition paragraph names the memoranda and framework anchors, not the executive orders themselves.
- [ ] Regulatory citations that could not be verified against the primary source at authoring time are marked `<!-- needs-research: ... -->` rather than invented.

## Stretch goals

- Add a *seventh-state watch-list entry* — one Virginia or Connecticut or Washington bill under legislative consideration that would attach to Career Match if enacted — with the pre-signature pre-work fields chapter 5's watch-list mechanism requires.
- Sketch what changes if Northbrook adds a *ninth deployment site* in an unfamiliar state (choose one — Florida, Ohio, Arizona) and how the applicability filter absorbs it. Two paragraphs.
- Produce a *contract-flow-down mapping* for the federal client's specific FAR-equivalent clauses (or the FAR AI clauses if you have access to the memoranda's clause language) onto the atomic requirements. Mark memoranda clause references with `<!-- needs-research: ... -->` if not verified.
- Draft the *sector-regulator overlay* for the federal-client deployment given Northbrook is a bank — how does SR 11-7 (Federal Reserve model risk management) compose with M-25-21 for an AI system used inside the federal workforce? One paragraph plus a shape-A crosswalk-edge note.
