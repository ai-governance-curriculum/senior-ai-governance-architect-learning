# Role descriptions and the hiring plan — populating the operating model

## Why this chapter exists

The operating model in chapter 02 names the seats the enterprise's AI-governance programme requires; the AI governance council in chapter 01 ratifies who is accountable for what. Neither artefact fills the seats. Filling them is the level-50 architect's obligation because the operating model's invariant 4 (every seat named is staffed or has a named plan-to-staff) cannot be discharged by anyone else — the head-of-AI-governance (level 60) carries the budget and the CHRO carries the workforce process, but the *technical justification* for each seat and the mapping from seat to hire-market against a defensible role packet is architectural work.

The failure mode this chapter designs against is the one where the enterprise ratifies an operating model and then discovers, when it tries to hire, that the labour market does not carry the seat's job title. A "senior AI risk engineer with independent-fairness scoring authority for regulated-industry AI systems" does not appear on the job boards under that name. The enterprise ends up posting a generic "AI/ML engineer" job description, hiring a strong ML engineer, and then discovering six months in that the seat's work products (mod-106 quantification, mod-107 second-line evaluation) are not what the hire believed they were signing up for. Attrition follows; the RACI's invariant 4 breaks.

The architectural correction is to bind each operating-model seat to a *role packet* — a training track and job-shape reference the enterprise's job description composes against, and that the labour market recognises. This track's parent curriculum organises role packets by level; the operating model's seat inventory maps into that ladder. This chapter authors the mapping, states the level and role packet each seat composes against, states the hire-vs-develop-vs-cosource decision for each, and gives the hiring plan the head-of-AI-governance carries to the CFO.

## The role tree — the seats and their role packets

The senior AI governance architect track's role tree names the AI-governance family from level 15 (operational analyst) to level 70 (chief AI officer). The operating-model seat inventory (chapter 02) composes against that ladder. The mapping is the following.

### `ai-governance-analyst` (level 15)

**Role packet:** operational analyst seat — intake, inventory, framework crosswalk drafts, first-draft impact assessments, model / system / dataset cards at analyst tier, control tracking, jurisdictional tracking, reading eval / audit evidence into governance records, drafting council minutes.

**Where the operating model uses this seat:**

- Second-line R on the mod-102 control-testing programme's day-to-day execution.
- Second-line R on the pre-deployment gate's evidence-contract discharge check (drafts the "is the evidence complete?" review before the gate meeting).
- Drafter of the AI governance council's minutes (chapter 01).
- First-draft author of AIA / DPIA composed with the CPO/DPO office (mod-105 chapter 04).
- Owner of the AI inventory freshness (per mod-105 chapter 05 documented-information discipline).

**Enterprise headcount scaling:** typically 2–5 analysts per second-line function at mid-enterprise scale; the hiring plan scales against AI-inventory size and gate throughput, not against total enterprise headcount. `<!-- needs-research: scaling heuristic for governance-analyst headcount against AI inventory size is not standardised; enterprise-specific empirical calibration required -->`

**Hire market shape:** the analyst hire market has grown fast since ISO/IEC 42001 publication (2023) and the EU AI Act's adoption (2024). Candidates typically arrive from privacy-programme analyst backgrounds (IAPP CIPP/E, CIPP/US), audit-programme analyst backgrounds (Big Four assurance rotations), and MRM analyst backgrounds (banking-industry model-risk-management functions). AIGP is the emerging must-have credential for the AI-specific specialisation (chapter 06).

**Hire-vs-develop-vs-cosource:** hire, with development track pointing at this role packet's curriculum. Cosourcing analyst work is rarely economic — analyst seats are the day-to-day operational engine of the programme, and cosource analysts do not develop enterprise-specific context fast enough.

### `ai-risk-engineer` (level 25)

**Role packet:** hands-on engineering craft of AI risk — harm-model authoring, red-team / adversarial-ML / fairness / privacy / guardrail engineering, quantification, monitoring-to-risk-register wiring, incident RCA.

**Where the operating model uses this seat:**

- Second-line R on `AIC-FAIR-*`, `AIC-ROB-*`, `AIC-SEC-*` (co-owned with AI-infra-security), `AIC-3PP-*` engineering, and `AIC-PMS-*` incident RCA.
- Independent-fairness scoring at the pre-deployment gate (the second-line R that is not the model owner).
- Quantitative risk-register author (mod-106).
- Standing rotating advisor to the AI governance council (chapter 01) on items turning on quantitative-risk content.

**Enterprise headcount scaling:** 1 risk-engineer per 5–10 material AI systems in the inventory is a common rough shape; enterprises with concentrated tier-3 / tier-4 exposure carry richer ratios. `<!-- needs-research: risk-engineer-to-material-system ratio is enterprise-specific and unpublished across the sector; verify against enterprise's own historical operational load -->`

**Hire market shape:** the risk-engineer hire market composes ML engineering with adversarial-ML research and MRM operational discipline. Candidates arrive from adversarial-ML PhDs, ML-security consulting firms, MRM engineering functions in banking, and the "AI risk" specialisation that has emerged inside cybersecurity consultancy practices. The role packet's ai-risk-engineer curriculum is the reference training track; enterprises composing hiring plans against this operating model point to it.

**Hire-vs-develop-vs-cosource:** hire for the permanent seat set; cosource for scarce specialisations (advanced red-team for frontier-model attacks, specialist fairness scoring for regulated-sector novel-harm cases). Developing risk engineers from strong ML engineers is possible with an 18–24 month development track; enterprises with mature MRM functions have historically converted MRM engineers to AI-risk engineers with less time than that.

### `ai-evaluation-engineer` (level 35)

**Role packet:** release-assurance and audit-trail methodology — pre-deployment gates, audit-facing evidence packs, regulator-facing artefact production inside the assurance system.

**Where the operating model uses this seat:**

- Second-line A on control-family attestations for `AIC-FAIR-*`, `AIC-ROB-*`, `AIC-EXPL-*`, `AIC-HITL-*`, `AIC-PMS-*` (methodology).
- Chair of the pre-deployment gate (mod-107 chapter 02).
- Author of the mod-108 evidence artefacts binding first-line evidence into attestation-ready form.
- Standing rotating advisor to the AI governance council on evaluation-methodology items.
- Owner of the mod-107 external-audit-interface packaging discipline (mod-107 chapter 05).

**Enterprise headcount scaling:** 1 evaluation-engineer per 5–10 material AI systems, similar to the risk-engineer ratio, but distinct because the evaluation engineer is chair-of-gate and A on attestations — the seat's throughput scales against gate cadence, not against system count directly. Enterprises with a monthly gate cadence for tier-3 and tier-4 systems typically carry 2–4 evaluation engineers per second-line function.

**Hire market shape:** the evaluation-engineer market is where the AI evaluation research community, the MRM validation-team practitioners, and the assurance methodology practitioners intersect. AIGP + ISO 42001 Lead Auditor is a common credential combination. Candidates arrive from ML PhDs with evaluation focus, MRM validation functions, Big Four AI-assurance practice areas, and the growing set of AI-safety-oriented practitioners with methodology backgrounds.

**Hire-vs-develop-vs-cosource:** hire; the seat's authority (A on attestations, chair of gate) requires enterprise standing that cosource engagements rarely establish. Development from ML researchers with an interest in methodology is possible; development from analysts is uncommon (the level jump is too far).

### Related seat: `ai-eval-engineer` (level 30) and `model-evaluation-engineer` (level 30)

The parent curriculum carries two related role packets at level 30 that share the "AI evaluation" umbrella at a level below the assurance-methodology `ai-evaluation-engineer` at level 35.

- **`ai-eval-engineer` (level 30)** — evaluation-harness engineering, benchmark-construction, evaluation-tooling development. Builds and runs the evaluation infrastructure the ai-evaluation-engineer (level 35) consumes.
- **`model-evaluation-engineer` (level 30)** — model-side evaluation, systematic capability evaluation, evaluation-run authorship. The first-line seat that runs evaluation runs the ai-evaluation-engineer (level 35) reviews.

`<!-- needs-research: verify the exact scope split between ai-eval-engineer (level 30) and model-evaluation-engineer (level 30) role packets in the parent curriculum, as the two seats overlap in the labour market and enterprises may consolidate them under one hire -->`

The operating model uses these seats as:

- First-line R on `AIC-FAIR-*`, `AIC-ROB-*`, `AIC-EXPL-*` engineering execution — the model-evaluation-engineer sits on the model-owner side of the gate, the ai-eval-engineer sits on the platform-lead side.
- First-line C to the ai-evaluation-engineer (level 35) on methodology conformance — the level-30 seats know the technical implementation, the level-35 seat carries the assurance-methodology authority.

**Enterprise headcount scaling:** more level-30 evaluation engineers than level-35 evaluation engineers, typically 3–5 level-30 per level-35 depending on scale. The level-30 seats sit on the first-line side; the level-35 seat sits on the second-line side.

**Hire market shape:** healthier than the level-35 market — evaluation-harness engineering and model-side evaluation are more common ML-industry backgrounds, and the labour market carries more supply. AIGP is optional but growing at this level.

**Hire-vs-develop-vs-cosource:** hire and develop; the level-30 seats are natural development tracks from strong ML engineers with an interest in evaluation, and are the pipeline into level-35.

### `agentic-safety-engineer` (level 40)

**Role packet:** frontier-agent red-team methodology and dangerous-capability evaluation.

**Where the operating model uses this seat:**

- Second-line A on `AIC-AGENT-*` control family — agentic-system control, tool-use governance, autonomy limits, scaffolding governance.
- Second-line R on dangerous-capability evaluations on frontier models the enterprise consumes or fine-tunes.
- Standing rotating advisor to the AI governance council on frontier-model and agentic-system items.
- Co-owner (with `ai-evaluation-engineer`) of the pre-deployment gate for agentic and frontier systems.

**Enterprise headcount scaling:** typically 1–3 per second-line function; the seat is scarce and its work is deep rather than throughput-scaled. Enterprises without frontier-model or agentic-system exposure may not carry the seat at all; enterprises with material such exposure may carry more.

**Hire market shape:** small and specialised. Candidates arrive from frontier-lab safety teams (Anthropic, OpenAI, DeepMind — often researchers finishing a rotation), AI safety research organisations (METR, Apollo Research, and adjacent), and the small set of AI-safety-oriented practitioners with red-team background. `<!-- needs-research: labour-market sizing of the agentic-safety-engineer supply is not publicly quantified; verify enterprise's likely hire timelines against direct sourcing conversations -->`

**Hire-vs-develop-vs-cosource:** cosource is common at this seat because the labour market is thin and the enterprise's need is often intermittent (dangerous-capability evaluation runs at model-release cadence, not weekly). Cosourcing arrangements with frontier-lab safety teams or with organisations like METR, Apollo Research, and adjacent practitioners are the practical path where the enterprise has intermittent need. Hiring for permanent seats is worth doing where the enterprise's frontier-model exposure is continuous.

### `ai-infra-security` / `security-learning` (level 35 in this track's family map)

**Role packet:** platform-scale defence — AI runtime security, model-supply-chain security, adversarial-ML defence at the platform layer.

**Where the operating model uses this seat:**

- Second-line A on `AIC-SEC-*` — AI-specific security control family.
- Second-line R on the runtime-security adjacencies (mod-111 chapter 05) and the SOC interface (mod-110 chapter 05).
- Standing consulted seat on `AIC-DATA-*` (data poisoning defence) and `AIC-AGENT-*` (tool-use security).
- CISO's technical delegate on the AI governance council for AI-security items.

**Enterprise headcount scaling:** 1–3 per enterprise depending on the AI runtime-security tooling depth; the seat composes with the existing enterprise security function, so the seat count depends on how much the security function is willing to specialise its existing seats vs. hire dedicated AI-security seats.

**Hire market shape:** at the intersection of classical AppSec, ML-security research, and the emerging AI-runtime-security vendor community. Candidates arrive from AppSec practices retooling for AI, ML-security research backgrounds, and the specialised AI-security vendor communities (Robust Intelligence, Lakera, HiddenLayer, Protect AI ecosystems).

**Hire-vs-develop-vs-cosource:** hybrid; hire for permanent seat set, develop from strong AppSec engineers who want to specialise, cosource for specialised red-team engagements (prompt-injection specific work, model-extraction assessments).

### `head-of-ai-governance` (level 60)

**Role packet:** programme leadership — board reporting, regulator engagement, budget, escalation, external positioning at working level.

**Where the operating model uses this seat:**

- Voting member of the AI governance council; the seat that owns the pack quality.
- Owner of the mod-107 external-provider engagement (certification body, ForHumanity auditor, sector regulator).
- Owner of the standards-community contribution posture at the head-level (chapter 07 in this module).
- Signer of the AIMS management review record.
- Line-manager of the level-50 architect and the level-15/25/35/40 second-line seats.

**Enterprise headcount scaling:** one per enterprise. The seat may be part-time in smaller enterprises where the CRO or CPO/DPO carries the head-of-AI-governance responsibilities as a component of their scope, but the operating model names the accountability separately even if the person is dual-hatted.

**Hire market shape:** the head-market is populated by senior GRC executives who have specialised into AI, senior privacy officers who have broadened, MRM executives who have moved into the AI space, and a small set of ex-regulator practitioners. AIGP + ISO 42001 Lead Implementer + prior CISO / CPO / CRO experience is a common combination.

**Hire-vs-develop-vs-cosource:** hire, typically at seven-figure total-compensation depending on enterprise size and location; development from strong level-50 architects into level-60 is possible but rare at large enterprises where the head-role is a board-facing seat requiring political capital the architect role does not build.

Chapter 08 walks the boundary between the level-50 architect and the level-60 head in detail.

### `chief-ai-officer` (level 70)

**Role packet:** AI strategy, P&L alignment, executive positioning.

**Where the operating model uses this seat:**

- Chair of the AI governance council (where the enterprise has a CAO seat) or the AI-accountable executive to whom the head-of-AI-governance reports.
- Board-facing owner of AI strategy and P&L alignment.
- Signer of AIMS scope commitments at the enterprise level.

**Enterprise headcount scaling:** zero or one per enterprise. Whether the CAO seat exists is a strategy question the CEO answers; where it does not exist, the AI-accountable executive is typically COO, CIO, CTO, or CRO depending on enterprise shape (per mod-105 chapter 03).

**Hire market shape:** small — the seat is executive, its market is executive search, and candidates typically arrive from executive-track ML / AI backgrounds (senior VPs of AI at large tech companies, executives from AI startups, ex-regulator or ex-policy executives with AI depth). AIGP is common but not sufficient; ISO 42001 Lead credentials are useful but rarely load-bearing at this level.

**Hire-vs-develop-vs-cosource:** hire; the seat is not one the enterprise develops from within except where the internal candidate is already a C-suite executive.

Chapter 08 walks the boundary between the level-60 head and the level-70 CAO in detail.

## The hiring plan — the shape the head-of-AI-governance takes to the CFO

The hiring plan translates the operating-model seat inventory into a budgeted, timed workforce plan the CFO can consult and the AI governance council can ratify. Its structure has four elements.

**Element 1 — the seat-by-seat headcount over the plan horizon.** For each operating-model seat and its role-packet level, the plan states current staffing, target staffing at the plan horizon (typically 12–24 months), and the staffing trajectory across the interval. Rationale for each target references the operating-model workload (control families, gate cadence, inventory size).

**Element 2 — the source mix.** For each seat, the plan states the intended mix of hire / develop / cosource / consolidate-with-existing-seat. The rationale references the labour-market shape stated in this chapter and the enterprise's own historical patterns.

**Element 3 — the sequencing.** The hiring plan sequences the seats so that dependencies are respected. The `ai-evaluation-engineer` seat cannot be filled effectively before the mod-107 assurance architecture is ratified — the seat's job description references the architecture. The `ai-governance-analyst` seat is often the first hire because the analysts do the day-to-day work the other seats build on. The `head-of-AI-governance` seat is usually filled early because the head recruits the other second-line seats.

**Element 4 — the budget shape.** For each seat, the plan states the total-compensation range with a defensible-market reference, the burden and benefits shape the CFO consults, and the intangible-benefit case (development-track existence, career progression, retention profile). The head-of-AI-governance is the seat that carries the plan to the CFO and defends it.

The hiring plan is a document, not a spreadsheet. The spreadsheet is a subordinate artefact; the document is what the AI governance council ratifies as a reserved matter and what the head takes to the CFO.

## The hiring-plan schematic — YAML shape

```yaml
hiring_plan:
  id: HIRING-PLAN-v1.0
  horizon: 24 months from ratification
  ratified_by: ai-governance-council (per chapter 01 reserved matter)
  head_of_plan: head-of-ai-governance (level 60)

  seats:
    - seat_id: gov-analyst-cohort
      role_packet: ai-governance-analyst (level 15)
      current_headcount: 1
      target_headcount: 3
      trajectory: hire one per calendar quarter over three quarters
      source_mix: 100% hire
      hire_market: privacy-analyst + audit-analyst + MRM-analyst backgrounds
      credential_bar: IAPP AIGP (in-role) + CIPP/US or /E where scope demands
      rationale: analyst throughput scales with AI-inventory freshness cadence; current inventory of ~40 systems requires 3 analysts to sustain quarterly freshness at target
    - seat_id: risk-engineer-cohort
      role_packet: ai-risk-engineer (level 25)
      current_headcount: 0
      target_headcount: 3
      trajectory: hire first in month 2, second in month 6, third in month 12
      source_mix: 66% hire, 33% develop from senior MRM engineer
      rationale: mod-106 quantification cannot proceed without dedicated risk-engineering capacity; MRM development path is enterprise-specific
    - seat_id: evaluation-engineer-cohort
      role_packet: ai-evaluation-engineer (level 35)
      current_headcount: 0
      target_headcount: 2
      trajectory: hire first in month 4 (after gate charter ratified), second in month 12
      source_mix: 100% hire
      credential_bar: IAPP AIGP + ISO 42001 Lead Auditor
      rationale: chair-of-gate seat requires enterprise standing that development does not build fast enough
    - seat_id: eval-engineer-level-30
      role_packet: ai-eval-engineer / model-evaluation-engineer (level 30)
      current_headcount: 1
      target_headcount: 4
      trajectory: hire two, develop two from existing ML engineers
      source_mix: 50% hire, 50% develop
      rationale: first-line evaluation execution scales with model release cadence; level-30 pipeline supplies level-35 promotion candidates
    - seat_id: agentic-safety-engineer
      role_packet: agentic-safety-engineer (level 40)
      current_headcount: 0
      target_headcount: 1
      trajectory: cosource until frontier-model-exposure surface justifies permanent hire; permanent seat in month 18 subject to review
      source_mix: cosource with a named external partner
      rationale: labour market too thin for guaranteed hire; enterprise's frontier-model exposure not yet continuous enough to justify permanent seat immediately
    - seat_id: ai-infra-security-cohort
      role_packet: ai-infra-security (level 35)
      current_headcount: 1 (dual-hatted from AppSec)
      target_headcount: 2 (one dedicated, one dual-hatted)
      trajectory: dedicated hire in month 6
      source_mix: hire dedicated seat; retain dual-hat for coverage
      rationale: runtime-security adjacencies require dedicated capacity; SOC interface (mod-110 ch 05) cannot be sustained from AppSec side-work
    - seat_id: head-of-ai-governance
      role_packet: head-of-ai-governance (level 60)
      current_headcount: 0
      target_headcount: 1
      trajectory: hire in month 1; seat recruits second-line cohort
      source_mix: executive search
      credential_bar: IAPP AIGP + ISO 42001 Lead Implementer + prior CISO/CPO/CRO experience or equivalent
      rationale: head-role is precondition for the other second-line hires; without a head, the seats have no line-management shape

  invariants:
    - id: I1
      description: every operating-model seat maps to a role packet
      test: cross-check operating-model seat inventory against this plan
    - id: I2
      description: every seat has a defensible credential bar
      test: cross-check credentials against chapter 06 certifications portfolio
    - id: I3
      description: sequencing respects the seat-dependency graph
      test: no seat scheduled to be filled before its prerequisite architecture is ratified
    - id: I4
      description: budget shape is CFO-defensible
      test: total-compensation ranges reference the enterprise's benchmark data with citations
    - id: I5
      description: plan is council-ratified as a reserved matter
      test: council minute references the plan version
```

## The three anti-patterns that break hiring plans

**Anti-pattern 1 — the "one AI person" plan.** The CFO reads the operating model, notes the ambition, and asks whether one senior AI-governance hire can carry the load "at least for the first year". The hiring plan's defence is invariant 3 — sequencing — and the operating-model workload. One hire cannot chair the gate, run the risk register, sample first-line evidence, and coordinate the certification body simultaneously. The head-of-AI-governance's response to the CFO is to walk the operating-model workload and show which control-family attestations do not get produced under the one-person plan. The council's ratification of the reserved matter is what makes the response stick.

**Anti-pattern 2 — the "consultants for a year, hire later" plan.** The CFO proposes staffing the initial ramp entirely through consultants and hiring permanent seats "when the shape stabilises". The plan looks cheaper in cash terms and defers the permanent-headcount commitment. Two problems: consultants do not develop enterprise-specific context fast enough for the ai-evaluation-engineer (chair-of-gate) and ai-risk-engineer (portfolio owner) seats to be effective, and consultant engagements without a permanent seat waiting to onboard from them lose the developed context on rotation. The head's response is to accept cosourcing for scarce specialisations (agentic-safety-engineer, specialist red-team) and reject it for the throughput seats.

**Anti-pattern 3 — the "hire the frontier-lab safety researcher" plan.** The enterprise reads a paper on dangerous-capability evaluation, decides to hire the researcher who wrote it, and offers a compensation package that assumes the researcher will define the seat's scope after joining. The seat has no operating-model rationale — the enterprise's frontier-model exposure may not justify permanent agentic-safety-engineer headcount — and the researcher's expectation of a research-heavy role is often incompatible with the operating model's release-assurance work. Six months later, either the researcher leaves or the seat has drifted away from the operating model. The architectural defence is to hire against the operating model's seat rationale, not against the individual candidate's profile.

## Coordination — the roles the hiring plan interfaces with

- **AI governance council** — ratifies the plan as a reserved matter (chapter 01).
- **Head-of-AI-governance (level 60)** — owns the plan; carries it to the CFO; recruits into the seats.
- **CFO** — approves the budget shape; consults on total-compensation calibration.
- **CHRO** — carries the workforce process; owns the job-description drafting composed against the role-packet references; owns the offer-and-onboarding workflow.
- **Head of talent acquisition** — sources against the role-packet references.
- **Line managers of the current seats** — carry the development-plan execution for the develop-from-within share of the plan.
- **External executive-search partners** — for the level-60 and level-70 seats.
- **Cosource partners** — the specialist AI-audit firm, the frontier-safety research organisation, the specialised evaluation consultancy the plan cosources from.

## Summary

The hiring plan populates the operating model's seat inventory by binding each seat to a role packet at a defined level in the AI-governance family — `ai-governance-analyst` (15), `ai-risk-engineer` (25), `ai-eval-engineer` and `model-evaluation-engineer` (30), `ai-evaluation-engineer` (35), `ai-infra-security` (35), `agentic-safety-engineer` (40), `head-of-ai-governance` (60), `chief-ai-officer` (70) — and translating the seat-by-seat headcount into a hire / develop / cosource / consolidate mix, a sequencing that respects the seat-dependency graph, and a budget shape defensible to the CFO. The chapter walks each seat's operating-model use, headcount-scaling rationale, hire-market shape, credential bar, and hire-vs-develop-vs-cosource default. The hiring plan is a council-ratified reserved matter (chapter 01); the head-of-AI-governance owns it and takes it to the CFO. Three anti-patterns recur — the one-AI-person plan, the consultants-for-a-year plan, and the frontier-lab-researcher-first plan — and the architectural defences are sequencing discipline, throughput-seat protection, and hire-against-the-operating-model-not-against-the-candidate discipline. Chapter 04 walks how the architect writes the case for the plan to the CFO, CIO, and CEO audiences; chapter 06 walks the certifications portfolio the credential bars reference; chapter 08 walks the boundary between the level-50 architect (this role) and the head-of-AI-governance and chief-AI-officer above.
