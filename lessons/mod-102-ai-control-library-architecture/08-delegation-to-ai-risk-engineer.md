# Delegating engineering-side controls to the AI risk engineer

## Why this chapter exists

Not every control in the library is architected end-to-end from a level-50 seat. Roughly a third of any mature AI catalog — the *engineering-side* controls on data poisoning defence, adversarial-ML hardening, differential-privacy noise budgets, membership-inference resistance, guardrail authoring, red-team scenario libraries, monitoring instrumentation — depends on hands-on engineering craft that the AI risk engineer (level 25) owns and the architect does not.

The architect's job on these controls is not to abdicate ("engineering handles it") and not to overreach ("I will spec the noise budget"). It is to *design the delegation contract* — which fields the architect writes, which fields the risk engineer co-authors, how the two roles negotiate at proposal time, how updates propagate, and how the architect stays in the seat that keeps the library coherent without ending up doing engineering work in an architecture seat.

Mod-101 chapter 06 already established the general engagement contract with the risk-engineer role. This chapter is the *control-library-specific* delegation: what changes hands, what stays with the architect, and how the artefact-level boundary is drawn.

## Which controls are "engineering-side"

Three tests identify an engineering-side control.

1. **The `implementation_guidance` field is dominated by engineering practice** — vector stores, sampling procedures, sanitisation pipelines, adversarial-input generators, noise-injection budgets, sidecar guardrails, drift detectors.
2. **The `testing_procedure` requires an engineering harness** — the test is not "read the record" or "interview the owner" but "run the adversarial suite against the deployed endpoint" or "measure epsilon of the DP mechanism."
3. **The evidence artefact is an engineering artefact** — a red-team scenario report, a guardrail latency-vs-block-rate profile, a monitoring metric time series, a differential-privacy accountant printout.

Family prefixes make the split visible in the catalog: `AIC-ENG-*`, `AIC-SEC-*`, `AIC-MON-*` cluster the engineering-side controls; `AIC-GOV-*`, `AIC-EVD-*`, `AIC-THI-*` (third-party), `AIC-POL-*` cluster architecture-only controls. The split is not clean at the margin (a governance control can have an engineering testing step), and the field-level ownership from chapter 01 is the arbiter — the family prefix is a hint, not a rule.

## The field-level delegation matrix — restated for engineering-side controls

Chapter 01's ownership table for a generic control:

| Field | Architect | Risk engineer | Analyst |
|---|---|---|---|
| id / title / version | owns | consulted | informed |
| statement | owns | consulted | informed |
| applicability | owns | consulted | informed |
| implementation guidance | reviews | co-authors | informed |
| testing procedure | reviews | co-authors | executes (or engineer executes on eng-only tests) |
| evidence contract | owns | reviews | executes collection |
| crosswalk | owns | consulted | informed |

For *engineering-side* controls, one adjustment matters: the *testing procedure* is authored by the risk engineer, not co-authored. The architect reviews for shape (does it name a concrete test, cadence, owner) but the substance — which techniques from ATLAS to execute, which epsilon threshold to gate on, which perturbation library to sample from — is the risk engineer's craft. If the architect is filling that in alone, the delegation is failing.

The `evidence_contract` field remains architect-owned even on engineering-side controls. The architect specifies which artefacts must be produced, at what cadence, retained how long, in what immutability posture; the risk engineer implements the pipeline that produces the artefact and confirms the pipeline can meet the contract. If the engineer says "no pipeline can produce that artefact at that cadence," the negotiation happens *at proposal time* (below), not silently after publication.

## The proposal-time negotiation

Every engineering-side control proposal goes through a short structured negotiation between architect and risk engineer before the chapter 06 stage-2 review.

**Round 1 — architect drafts the outcome.** Statement, applicability, evidence contract, crosswalks. This is deliberately done first without engineering input; the outcome is what the organisation commits to, and the engineering feasibility question is the *next* question, not the first.

**Round 2 — risk engineer drafts the mechanism.** Implementation guidance, testing procedure, evidence pipeline design. In this round the engineer confirms the outcome is achievable and returns one of three responses:

- *Achievable as authored* — proceed.
- *Achievable with cadence / retention / sampling modification* — negotiate the modification with the architect. If the modification does not weaken the outcome (a stricter cadence, a broader sample) the architect usually accepts; if it weakens (a looser retention, a smaller sample) the negotiation focuses on the risk trade-off.
- *Not achievable* — the outcome as written cannot be delivered by any known engineering. The architect either (a) revises the outcome to what is achievable, (b) accepts the engineering constraint as a residual risk and marks the control accordingly, or (c) escalates the gap to the head-of-AI-governance for a strategic decision (invest in the missing capability, buy a vendor, defer the outcome).

**Round 3 — joint review.** The paired draft goes to stage-2 architect review as one artefact with both authors named. Approval or rejection is on the whole entry.

The rule: the risk engineer never authors the statement alone; the architect never authors the testing procedure alone. Both roles are on record.

## Case study — an adversarial-robustness control

To make the delegation concrete, walk through one worked example.

**Draft outcome (architect, round 1):**

```yaml
control:
  id: AIC-ENG-058  # reserved
  title: Adversarial-input robustness for tier-1 vision systems
  statement: >
    For each tier-1 in-scope classical-ML vision system, the model's
    prediction distribution under a defined adversarial-perturbation
    suite differs from its distribution on the clean evaluation set
    within a bounded, published tolerance.
  applicability:
    system_tier: [tier-1]
    system_kind: [classical-ml]
    use_case: [vision, biometric]
  evidence_contract:
    - artifact: adversarial-robustness-report.json
      schema_ref: urn:enterprise:schema:advrob#1.0  # to be authored
      producer_role: ai-risk-engineer
      owner_role: ai-governance-analyst
      cadence: pre-release-and-quarterly
      retention_years: 7
      immutability: append-only
      sampling: all-tier-1-in-scope
      regulator_facing: true
  crosswalk:
    nist_ai_rmf: [MEASURE-2.7]
    iso_iec_42001_annex_a: [A.6.2]  # <!-- needs-research: confirm current Annex A ID -->
    eu_ai_act: [Article 15]
    owasp_llm_top_10: []
    mitre_atlas_techniques: [AML.T0043]  # Craft Adversarial Data
    google_saif: [model-evasion]
```

**Round 2 (risk engineer):**

The engineer confirms the outcome is achievable, drafts:

```yaml
implementation_guidance:
  - "Use the enterprise adversarial-perturbation suite v3.4 (PGD +
    C&W + patch attacks; suite manifest in ENG-ADV-SUITE-3.4)."
  - "Baseline: clean test-set accuracy and calibration."
  - "Report per-attack accuracy delta, per-attack top-5 stability, and
    per-attack calibration ECE delta."
testing_procedure:
  - id: AIC-ENG-058-T1
    description: >
      Run enterprise adversarial suite v3.4 against the deployed
      endpoint with parameters from ENG-ADV-PARAMS-tier1.yaml.
    cadence: pre-release
    owner_role: ai-risk-engineer
  - id: AIC-ENG-058-T2
    description: >
      Quarterly re-run against the current deployed endpoint using
      the same parameter file, drift check against the pre-release
      baseline.
    cadence: quarterly
    owner_role: ai-risk-engineer
```

The engineer also flags: the `schema_ref` needs to be authored — the current registry has no `advrob` schema. The negotiation:

- Architect: agreed; the schema will be authored in the same release; block the control on the schema being registered.
- Engineer: agreed; will contribute the required-fields list (per-attack accuracy delta, per-attack calibration ECE, sampling manifest hash) as the schema input.

Round 3 review is joint; the release ships both the control and the schema together.

## Where the architect stays firm

Three fields are where the delegation temptation is greatest and the architect must stay firm.

**Statement.** The engineer will often be tempted to embed the mechanism in the statement ("The model has adversarial training performed via PGD-20"). Push back: the mechanism is guidance; the statement is the outcome. The mechanism will change with the state of the art; the outcome should not.

**Applicability.** The engineer will often want to narrow the applicability to systems the engineer already knows how to test ("only tier-1 vision systems for biometric use cases"). Push back: applicability is set from the outcome need, not from tooling reach. If the tooling does not cover the whole applicability, the *tooling* is the gap, and gets a POA&M entry — the applicability filter does not shrink to hide the gap.

**Retention and immutability.** Engineers reason in performance and storage terms. Governance reasoning at retention lives on the regulatory clock (chapter 07). Do not accept "seven years is too much storage" as a reason to weaken retention.

## Where the risk engineer stays firm

Two fields are where the architect's temptation to over-specify is highest and the risk engineer should push back.

**Testing procedure specifics.** If the architect writes "use PGD with epsilon 8/255 for 20 steps," the architect has entered engineering craft. The right architect-level content is "use the enterprise adversarial suite at the pre-release parameter profile"; the engineer picks the suite and the profile. Freeze the outcome; delegate the technique.

**Implementation guidance specifics.** "Use TensorFlow Serving 2.11 with the guardrail sidecar" belongs to the engineer, or to the platform reference architecture, not to the control library. The architect's guidance pointer is "see the enterprise inference-serving reference architecture v2"; the engineer maintains the reference.

## Handling engineering-side updates

Post-publication updates on engineering-side controls flow slightly differently from governance-family updates.

- **Guidance and testing procedure updates** are typically minor (chapter 06 classification). The risk engineer proposes; the architect reviews for shape; the release notes attribute the update to the engineer.
- **Statement and applicability updates** are typically major. Even if the engineer's tooling changed, the statement is the org's commitment — bumping it is an architect-owned decision that goes through the full stage-2 review.
- **Evidence contract updates** are the trickiest. A schema evolution (adding a required field to the artefact) is a minor if backwards-compatible for old artefacts, a major if not. The engineer and architect co-author the change; the architect owns the classification.

## The two shared artefacts — the delegation matrix and the joint drafts folder

The delegation is not just an idea; it lives in two artefacts.

**The delegation matrix.** A per-control table in the library preface listing every engineering-side control, its co-authors, and the state of the contract (drafted / negotiated / published / under-revision). The head-of-AI-governance uses this at the quarterly review to spot control families where the delegation is silently absent (an engineering-side control whose "co-author" field is empty is a control the architect has been authoring alone, and either the risk-engineer seat is missing or the engineering slot has drifted up).

**The joint drafts folder.** Round-1 / round-2 / round-3 negotiation artefacts are preserved. This is not for its own sake; it is for the audit narrative when a regulator asks "how did the organisation decide the adversarial-robustness tolerance was set at delta=0.05 rather than 0.01?" The negotiation record is the answer.

## Two common shape mistakes

**Mistake 1 — Architect writes the whole engineering control and asks the engineer to "implement it."** The architect ends up authoring epsilon values and library versions; the engineer either rubber-stamps (bad) or push-back with no productive channel (worse). Fix: enforce the three-round negotiation as a proposal-shape requirement, not an optional courtesy.

**Mistake 2 — Engineer authors the whole control including statement.** The library gains an entry whose statement paraphrases the mechanism ("the org applies PGD-based adversarial training"). Six months later PGD is not the state of the art, and the entry's outcome is unclear because the outcome was never named. Fix: hold the architect-owned rounds firm.

## Summary

Roughly a third of the AI control library is engineering-side, and those entries are co-authored with the AI risk engineer at level 25 through a structured three-round negotiation: architect drafts the outcome, engineer drafts the mechanism, joint review at stage-2. Statement / applicability / evidence contract / crosswalks stay architect-owned; guidance / testing procedure / mechanism-selection stay engineer-owned. Two fields — retention and immutability on one side, testing-procedure specifics on the other — are the ones where each role most needs to stay firm against the other's drift. A visible delegation matrix and a preserved negotiation record are the two artefacts that keep the delegation honest at scale. Get this delegation right and engineering-side controls in the library are genuinely both grounded in engineering practice and genuinely committed at architect level, without either role doing the other's job.
