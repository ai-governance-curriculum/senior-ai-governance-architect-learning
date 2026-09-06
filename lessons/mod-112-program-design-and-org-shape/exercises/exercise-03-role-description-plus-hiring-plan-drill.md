# exercise-03: Role Description and Hiring Plan Drill

**Estimated effort:** 2.5 hours

## Objective

Author the **seat-level job descriptions and the 24-month hiring plan** for the enterprise chosen in exercise-02 — the artefacts chapter 03 designs against, ratified by the AI governance council as a reserved matter (chapter 01), owned by the head-of-AI-governance and carried to the CFO. The deliverable set is a job-description catalog with one entry per second-line role packet (`ai-governance-analyst` through `chief-ai-officer`), the machine-readable hiring plan with 24-month trajectories, sequencing and dependencies, and a CFO-facing defence memo that answers the three anti-patterns chapter 03 names.

The correctness spine is chapter 03's five invariants (every seat maps to a role packet; every seat has a defensible credential bar; sequencing respects the seat-dependency graph; budget shape is CFO-defensible; plan is council-ratified) and its three anti-patterns (the one-AI-person plan; the consultants-for-a-year plan; the frontier-lab-researcher-first plan). Every design choice must be pinnable.

## Prerequisites

- Chapter [`03-role-descriptions-and-hiring-plan.md`](../03-role-descriptions-and-hiring-plan.md) read once, with the per-seat operating-model use, headcount-scaling rationale, hire-market shape, credential bar, and hire-vs-develop-vs-cosource defaults marked.
- Chapter [`02-three-lines-of-defence-for-ai-operating-model.md`](../02-three-lines-of-defence-for-ai-operating-model.md) read once (or use the seat inventory from exercise-02 if completed) — the hiring plan populates the operating-model seat inventory and its invariant 4 (every seat staffed or has a plan-to-staff).
- Chapter [`06-certifications-portfolio-positioning.md`](../06-certifications-portfolio-positioning.md) skimmed — the credential bar per seat references chapter 06's positioning framework.
- Chapter [`08-boundary-to-head-of-ai-governance-and-chief-ai-officer.md`](../08-boundary-to-head-of-ai-governance-and-chief-ai-officer.md) skimmed — the head is the plan owner; the CAO (where the seat exists) is the strategic-context supplier.
- The parent curriculum's role-packet references — `ai-governance-analyst` (15), `ai-risk-engineer` (25), `ai-eval-engineer` and `model-evaluation-engineer` (30), `ai-evaluation-engineer` (35), `ai-infra-security` / `security-learning` (35), `agentic-safety-engineer` (40), `head-of-ai-governance` (60), `chief-ai-officer` (70). Use the `<!-- needs-research -->` marker for packet paths not yet verified.

## Scenario

Carry the enterprise chosen in exercise-02 forward (US regional bank; European B2B SaaS platform vendor; global healthcare payer / provider). If you did not complete exercise-02, choose here and state the choice at the top of the hiring-plan YAML.

The scenario's implications for the plan:

- **Bank scenario.** The MRM function is already staffed; the plan does not hire an MRM function, but it does state how the AI-risk-engineer and AI-evaluation-engineer seats compose with MRM. The credential bar for the AI-evaluation-engineer includes MRM familiarity (typically demonstrated through work history or GARP FRM adjacent).
- **SaaS vendor scenario.** The customer-facing X coordinator (per the operating model's X register in exercise-02) is a distinct role — either a dedicated second-line seat or a scoped hat on the head-of-AI-governance. The plan must state the choice.
- **Healthcare scenario.** The clinical-safety liaison is a distinct role — not staffed from within the second-line, but a bridge seat that requires clinical-informatics familiarity plus AI-governance grounding. The plan states whether this is a hire or a rotation-from-CMO-side and the credential bar accordingly.

## Deliverables

Author four artefacts in a working directory of your choice.

1. **`job-description-catalog.md`** — one job description per second-line role packet in scope for the scenario. Each JD is the artefact the CHRO's talent-acquisition function posts against.
2. **`hiring-plan-v1.0.yaml`** — the machine-readable 24-month hiring plan with seat-by-seat headcount, source mix, sequencing, and budget shape.
3. **`sequencing-and-dependency-graph.md`** — the seat-dependency graph the plan's sequencing respects, with the rationale per dependency.
4. **`cfo-defence-memo.md`** — the memo the head-of-AI-governance carries to the CFO, answering the three chapter-03 anti-patterns and justifying the total-compensation ranges against the operating-model workload.

## Requirements

### `job-description-catalog.md`

One JD per role packet in scope (at minimum: `ai-governance-analyst` L15, `ai-risk-engineer` L25, `ai-evaluation-engineer` L35, `agentic-safety-engineer` L40 where in scope, `ai-infra-security` L35, `head-of-ai-governance` L60). Add `chief-ai-officer` L70 where the enterprise has or plans the seat. Include `ai-eval-engineer` L30 and `model-evaluation-engineer` L30 first-line JDs where the scenario's first-line topology requires distinct hires for these seats.

Every JD carries:

- **Title.** The enterprise's job title (may differ from the role-packet name — state the mapping explicitly to prevent the chapter-03 hire-market drift).
- **Role packet reference.** The parent-curriculum role packet the JD composes against; where the packet path is not yet verified, `<!-- needs-research -->`.
- **Reports to.** Line-management reference (typically the head-of-AI-governance for second-line seats; the model-owner's manager for first-line seats).
- **Operating-model responsibilities.** The R / A / C cells from the exercise-02 RACI this seat carries; state them explicitly by cell reference rather than in prose paraphrase.
- **Day-to-day work products.** The specific artefacts the seat produces (drafts council minutes; authors risk-register entries; chairs the pre-deployment gate; etc.).
- **Credential bar.** Required and preferred credentials, per chapter 06's positioning framework. AIGP is load-bearing across the L15–L35 seats; ISO 42001 Lead Auditor is a preferred credential for L35 and above; CIPP/E or CIPP/US is a required credential where the personal-data-processing nexus is material.
- **Experience shape.** Years and adjacent-experience descriptors matched against chapter 03's hire-market discussion.
- **Location and compensation range.** Placeholder or scenario-specific range; where a range is stated, cite the benchmark source (e.g., "internal comp benchmark v2026Q1" or a public salary-survey citation with `<!-- needs-research -->`).
- **Anti-drift note.** A short paragraph stating what the seat is *not* — for example, the ai-evaluation-engineer is not the model-owner's peer for evaluation-engineering execution (that is the L30 evaluation engineer's role); the head-of-AI-governance is not the technical peer of the level-50 architect; the CAO is not the operational lead of the AI-governance programme.

### `hiring-plan-v1.0.yaml`

The machine-readable plan. Structure per chapter 03's schematic, adapted to the scenario:

- **Metadata.** `id`, `horizon` (24 months from ratification), `ratified_by` (AI governance council), `head_of_plan` (head-of-AI-governance), `scenario` (bank / SaaS / healthcare).
- **Seats.** One entry per role packet in scope. Each entry carries: `seat_id`, `role_packet`, `current_headcount`, `target_headcount` (at 24 months), `trajectory` (quarter-by-quarter or event-driven — first hire at month N, second at month M), `source_mix` (percentages summing to 100: hire / develop / cosource / consolidate-with-existing-seat), `hire_market` (short descriptor of the labour-market pool), `credential_bar`, `rationale` (references the operating-model workload from exercise-02).
- **Sequencing.** A `sequencing:` block naming the seat-dependency graph in cross-reference to `sequencing-and-dependency-graph.md`. At minimum: the head-of-AI-governance is filled early (recruits the cohort); the ai-evaluation-engineer is not filled before the mod-107 assurance architecture is ratified; the ai-governance-analyst cohort is typically first-in for operational throughput.
- **Budget shape.** Total-compensation ranges per seat (may be scenario-typical rather than enterprise-specific), burden and benefits shape (typically 20–35% loading, cite source), and the intangible-benefit case (development-track existence, retention-profile expectation).
- **Cosource plan.** Where the source mix includes cosource (typically the agentic-safety-engineer at L40, sometimes the level-35 seats for specialised engagements), the plan names the cosource partner class and the engagement shape (retainer / project / on-call).
- **Invariants block.** All five chapter-03 invariants with a `test:` per invariant.

### `sequencing-and-dependency-graph.md`

The seat-dependency graph. Nodes are seats; edges are "seat X cannot be filled effectively before seat Y or artefact Z exists". Chapter 03 names the load-bearing dependencies:

- **Head-of-AI-governance → all second-line hires.** The head recruits the cohort; hiring second-line seats without a head produces onboarding drift.
- **Mod-107 assurance-architecture ratification → AI-evaluation-engineer hire.** The evaluation-engineer's job description references the architecture; hiring before ratification is hiring against a moving target.
- **Mod-102 control-library v1 → AI-governance-analyst cohort growth.** The analyst's day-to-day work is control-testing execution; growing the cohort before the control library stabilises produces analysts with unclear scope.
- **Mod-111 GRC-for-AI reference architecture → ai-infra-security dedicated hire.** The runtime-security adjacencies require the reference architecture's platform-composition to be settled first.
- **Mod-104 jurisdiction-reconciled control-set v1 → head-of-AI-governance's regulator-engagement scope.** The head cannot carry regulator engagement effectively without the reconciled control set as the enterprise's stated posture.

State scenario-specific dependencies (e.g., the bank's MRM composition must be reconciled before the AI-evaluation-engineer's job description is finalised, otherwise the JD's operating-model responsibilities double-book with MRM validation).

### `cfo-defence-memo.md`

The memo the head-of-AI-governance takes to the CFO. Structure:

- **Opening framing.** One paragraph — what the plan is, what the council has ratified, what the CFO is being asked to fund.
- **The programme workload.** A summary table of the operating model's workload (control families, gate cadence, inventory size, jurisdiction coverage) — the CFO's basis for evaluating whether headcount is proportional.
- **Anti-pattern 1 defence — the "one AI person" plan.** State the anti-pattern; state why the operating-model workload does not permit it; state which control-family attestations do not get produced under the one-person plan (per exercise-02's RACI).
- **Anti-pattern 2 defence — the "consultants for a year" plan.** State the anti-pattern; state where cosourcing is accepted (specialist scarcity — agentic-safety, specialist red-team) and where it is rejected (throughput seats — analyst, risk engineer, evaluation engineer).
- **Anti-pattern 3 defence — the "frontier-lab researcher first" plan.** State the anti-pattern; state that hires are against the operating-model seat rationale, not against the individual candidate's profile.
- **Budget summary.** Total-compensation-plus-burden shape per year 1 and year 2, referenced against the seat-by-seat entries in the hiring plan.
- **Sensitivity analysis.** Two scenarios — CFO-approved-in-full and CFO-approved-at-70% — with the shape of the operating-model workload that gets deferred under the 70% scenario. Chapter 03's anti-pattern 1 defence lives here in explicit form.
- **Ask.** The specific dollar amount, headcount, and sequencing the head is requesting the CFO endorse.

## Starter guidance

Draft the JDs against the exercise-02 RACI, not against generic role descriptors. A JD that lists "own the AI evaluation methodology" is unpinned; a JD that lists "A on `AIC-FAIR-*`, `AIC-ROB-*`, `AIC-EXPL-*`, `AIC-HITL-*`, and `AIC-PMS-*` in the exercise-02 operating model" is pinned to specific work products the operating model has ratified. The pinning is what protects the seat from the chapter-03 hire-market drift — the ML engineer who reads the pinned JD understands the work; the ML engineer who reads the generic JD hires in expecting a different role.

The head-of-AI-governance is the first hire even when the plan is drafted before the head is in seat. In that shape, the level-50 architect and the ratifying executive (typically the AI-accountable executive under mod-105 chapter 03) co-draft the plan; the head, once hired, ratifies and takes ownership. The plan's `head_of_plan` field is the seat, not the person, and the seat's authority attaches to whoever fills it. If the head is not filled at ratification time, state this explicitly and name the interim owner (typically the AI-accountable executive).

Chapter 03's anti-pattern 1 defence is the most valuable part of the CFO-defence memo. The CFO's default when a new programme's budget arrives is to ask whether the ambition can be pared back. The head's response is neither "yes" nor "no" — it is "here is which specific operating-model workloads do not get discharged under the pared-back plan, and here is the audit-committee-visible consequence". Chapter 03 walks the pattern; the memo is where the architect operationalises it. The memo cannot be a plea for the requested headcount; it is a structural response that translates the CFO's ask into operating-model consequences the CFO can evaluate.

Cosourcing has a well-known trap: the cosource engagement generates strong outputs during the engagement and no institutional context after. The plan's `cosource_plan` block must state the transition path — for scarce specialisations (agentic-safety-engineer), the cosource is medium-term with a permanent hire triggered when frontier-model exposure crosses a defined threshold; for throughput seats, cosource is not accepted. Chapter 03's anti-pattern 2 discussion applies.

The frontier-lab-researcher hire is a tempting shortcut. The candidate's profile is impressive, the salary premium is affordable at senior level, and the seat "will define itself once the candidate is in". Six months later the candidate is doing research the enterprise cannot use, and the operating model's `AIC-AGENT-*` A is unfilled. The plan hires against the operating-model rationale. If the enterprise's frontier-model exposure is not continuous, the seat cosources; if continuous, the seat's JD is written before the candidate is identified. Chapter 03's anti-pattern 3 discussion applies.

## Acceptance criteria

- [ ] Chosen scenario is stated at the top of `hiring-plan-v1.0.yaml`; every artefact is coherent against it.
- [ ] `job-description-catalog.md` includes one JD per role packet in scope; every JD carries title, role-packet reference, reports-to, operating-model responsibilities (cell references from exercise-02 RACI), day-to-day work products, credential bar cross-referenced to chapter 06, experience shape, compensation range with benchmark source, and anti-drift note.
- [ ] `hiring-plan-v1.0.yaml` has metadata block, one seat entry per role packet, sequencing block, budget shape, cosource plan, and invariants block with test fields.
- [ ] `sequencing-and-dependency-graph.md` names the chapter-03 default dependencies plus scenario-specific dependencies with rationale.
- [ ] `cfo-defence-memo.md` includes opening framing, programme-workload summary, defences against all three chapter-03 anti-patterns, budget summary, sensitivity analysis (full and 70% scenarios), and specific ask.
- [ ] Every seat named in the hiring plan appears in the exercise-02 seat inventory (or in a stated addition), and invariant 4 (every seat staffed or has a plan-to-staff) is satisfied when the seat inventory is cross-checked against the hiring plan.
- [ ] Every design choice is pinnable to a chapter-03 invariant (I1–I5) as enforcer or a chapter-03 anti-pattern as defence; a `pinning:` block or footnote in the hiring-plan YAML makes this explicit.
- [ ] Every unverified specific — role-packet paths, compensation benchmarks, credential body-of-knowledge references — carries `<!-- needs-research -->` rather than a guessed value.

## Stretch goals

- **Diversity-and-inclusion overlay.** Author a short overlay to the hiring plan naming the D&I metrics the plan tracks against, the pipeline-building actions the head-of-AI-governance carries with the CHRO, and the calibration-against-target discipline. The overlay is orthogonal to the operating-model rationale but composes with it; the CFO memo touches on retention profile.
- **Attrition-and-succession plan.** Extend the hiring plan with a succession block per seat — who steps up if the seat becomes vacant, what the transition looks like, how the operating model's invariants hold during the transition. The block is what chapter 08's boundary discussion tests against when the level-60 head departs.
- **Cosource-partner selection matrix.** Author the matrix the head-of-AI-governance uses to evaluate cosource partners for the agentic-safety-engineer seat: qualification bar, engagement-shape flexibility, IP-and-confidentiality shape, coordination-cost estimate, alignment with the frontier-model-provider adjacency. Chapter 03's cosource discussion is the reference.
