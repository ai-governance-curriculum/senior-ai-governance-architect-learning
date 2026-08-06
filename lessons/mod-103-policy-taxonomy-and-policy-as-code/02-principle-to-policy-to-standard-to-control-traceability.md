# Principle-to-policy-to-standard-to-control traceability

## Why this chapter exists

Almost every enterprise Responsible AI programme opens with a set of principles. Fairness. Accountability. Transparency. Human oversight. Safety. Privacy. Robustness. They appear on the board pack, on the careers page, on the vendor questionnaire, and in the CEO keynote. They are, at that moment, decorative. They become non-decorative only when every principle dereferences — through a binding policy clause, a standard, and a testable control — into an evidence artefact that some named role produces on a stated cadence.

The failure mode has a name: the *principle wall*. A board publishes seven beautiful RAI principles. Two years later an auditor, a regulator, or a plaintiff's counsel asks the obvious question — "show me, for the *fairness* principle, the policy clause it produced, the standard that operationalises that clause, the control that tests the standard, and the last evidence artefact that control emitted." If the answer is a slide deck, the principle is window dressing. Regulators, auditors, and (eventually) plaintiffs increasingly treat undereferenced principles as evidence that the programme is aspirational rather than operational, and reason accordingly about liability and enforcement posture. <!-- needs-research: primary-source examples of regulators or courts explicitly citing undereferenced RAI principles as evidence of window dressing -->

The remedy is traceability — a directed graph, maintained deliberately, from every principle down to at least one control and its evidence contract, and from every control back up to at least one principle. This chapter defines the traceability chain, the mapping matrix that stores it, the four rules that keep it well-formed, the composition with the mod-102 control library, and the quarterly discipline (`ai-governance-analyst`-run, architect-signed) that keeps it honest. Chapter `01-ai-policy-hierarchy-authority-and-cadence.md` established the hierarchy this chain runs down; chapter `03-policy-as-code-enforcement-tiers.md` shows how the leaf controls are enforced; chapter `04-exception-and-waiver-workflow.md` shows what happens when a control does not fire; chapter `05-policy-change-communications-and-deprecation-windows.md` covers how changes to any link propagate; chapter `06-iso-38507-22989-and-ieee-7000-composition.md` composes external principle sources into the same chain.

## The traceability chain

The chain has four link types and one terminal artefact.

```
Principle  ─┐
            ├─► Policy clause  ─┐
            │                   ├─► Standard clause  ─► Control ID  ─► Evidence artefact
            │                   │
Principle  ─┘                   │
                                │
Policy clause ──────────────────┘
```

Read left to right: a *principle* is a values-level commitment (e.g., "AI systems used in decisions about individuals shall be fair and non-discriminatory"). A *policy clause* is the binding, board-authorised sentence (chapter `01-ai-policy-hierarchy-authority-and-cadence.md`) that turns the principle into an enterprise obligation ("The organisation shall assess disparate impact on protected classes before deploying any AI system that materially affects individuals."). A *standard clause* is the specific normative requirement ("Disparate-impact analysis uses metric M on populations P and reports thresholds T…") that a control can test against. A *control ID* is a mod-102 control library entry, whose testing procedure verifies the standard clause. An *evidence artefact* is the machine-readable output the control's evidence contract produces.

Crucially, this is a *directed acyclic graph* with N:M edges, not a strict tree:

- One principle produces many policy clauses (fairness produces one clause about training-data representativeness, another about disparate-impact testing, another about redress).
- One policy clause implements many standard clauses, and one standard clause implements many policy clauses.
- One control satisfies many standard clauses, and often traces back to many principles (a training-data provenance control simultaneously supports fairness, accountability, and privacy).
- One evidence artefact is often shared across many controls (a single model-card artefact serves transparency, human-oversight, and accountability controls).

Modelling the chain as a tree — one principle owns one policy owns one standard — is the most common shape mistake, addressed at the end of this chapter.

## The mapping matrix — how to author and maintain it

The mapping lives in a single machine-readable file, versioned in the same repository as the control library and the policy corpus. Every row is one edge assertion: "this principle is (partially) implemented by this policy clause, which is (partially) implemented by this standard clause, which is tested by this control, which produces this evidence artefact, and I verified it on this date."

Six columns are mandatory. A minimal row in YAML:

```yaml
- principle_id: RAI-04
  policy_clause_id: POL-RAI-3.2
  standard_clause_id: STD-FAIR-2.1
  control_id: AIC-FAIR-041
  evidence_artefact_ref: fairness-disparate-impact-report.json@ENT-SCHEMA-FDIR-1.1
  verified_date: 2026-06-14
```

The same row as CSV, for the analysts who will actually maintain it:

```csv
principle_id,policy_clause_id,standard_clause_id,control_id,evidence_artefact_ref,verified_date
RAI-04,POL-RAI-3.2,STD-FAIR-2.1,AIC-FAIR-041,fairness-disparate-impact-report.json@ENT-SCHEMA-FDIR-1.1,2026-06-14
```

Column semantics:

- **`principle_id`** — a stable identifier from the enterprise RAI principle register (e.g., `RAI-04` for fairness / non-discrimination). Principles carry IDs for exactly the same reasons controls do (chapter 01 of mod-102): the wording changes; the ID does not.
- **`policy_clause_id`** — a stable identifier for the specific clause of a specific policy (not the policy as a whole). The addressing scheme in chapter `01-ai-policy-hierarchy-authority-and-cadence.md` gives every clause its own ID.
- **`standard_clause_id`** — a stable identifier for the specific normative requirement inside a standard. Standards, like policies, are addressed at the clause level so that a single standard update does not orphan the whole matrix.
- **`control_id`** — a mod-102 control-library entry, e.g., `AIC-FAIR-041`. The control is where the chain becomes testable.
- **`evidence_artefact_ref`** — a reference of the form `<artefact-name>@<schema-ref>` that matches the evidence contract inside the control entry (mod-102 chapter 01, field 6). This is what an auditor is actually served.
- **`verified_date`** — the last date the traceability review (see below) confirmed this row still holds. A row whose `verified_date` is stale by more than one review cycle is itself a finding.

Optional but useful columns: `edge_kind` (`derives_from` vs. `assists_with`, discussed below); `owning_role` (the person who signed off on the assertion); `notes`. Keep the mandatory set to six so the matrix stays legible in a text editor.

The whole file is versioned alongside the control library and the policy corpus. Chapter `05-policy-change-communications-and-deprecation-windows.md` covers how changes to any of the four IDs in a row propagate to the matrix.

## Four traceability rules

The mapping matrix is well-formed when four rules hold. Each rule has a query that a `ai-governance-analyst` can run against the matrix at review time; each violation is a finding.

**Rule 1 — Every principle has downstream coverage.** For every `principle_id` in the RAI principle register, the matrix contains at least one row with (a) a non-null `policy_clause_id`, (b) a non-null `standard_clause_id`, and (c) a non-null `control_id`. A principle with zero downstream rows is a *zero-degree principle* — pure decoration — and is a finding. A principle that has policy coverage but no standard, or standard but no control, is a *partial-degree principle* and is also a finding, at a lower severity.

**Rule 2 — Every binding-policy clause names its parents.** For every `policy_clause_id` in the enterprise policy corpus that carries binding language (`shall` / `must`; chapter `01-ai-policy-hierarchy-authority-and-cadence.md` defines the verb set), the matrix contains at least one row that names (a) the principle it derives from and (b) at least one standard that implements it. A binding policy clause that does not trace up to a principle is a *foundling* — someone imposed an obligation without a values-level commitment behind it, which is either an omission in the principle register or an over-reach in the policy — and either way is a finding.

**Rule 3 — Every standard clause maps to at least one control.** For every `standard_clause_id` in the enterprise standards corpus, the matrix contains at least one row that names a `control_id` from the mod-102 library. A standard clause with no downstream control is *untested by construction* and is a finding. Note that "at least one" is a floor, not a ceiling; one standard clause frequently maps to several controls, one per testing surface.

**Rule 4 — Every control's crosswalk carries a principle back-reference.** Every mod-102 control entry's crosswalk (field 7, chapter 01) contains a `principle_ids` list naming the principles it participates in. This is the *reverse edge* of rule 1 and is the mechanism that makes principle changes propagate: revoking or amending a principle triggers a review of every control whose `principle_ids` list contains that principle. A control with no upstream principle in its crosswalk is a *widow* (see the "orphan and widow detectors" section below) and is a finding.

Rules 1 and 4 are a pair — one is queried on the principle register and one on the control library — and both must hold. A programme that only enforces rule 1 finds decorative principles but does not find controls that are running without a values-level warrant.

## Composition with the mod-102 control library

The mod-102 control library entry defined in chapter `01-anatomy-of-a-control-library-entry.md` gains one new key inside the `crosswalk` field: `principle_ids`. The revised crosswalk field on the running `AIC-DAT-014` example from mod-102 becomes:

```yaml
crosswalk:
  nist_ai_rmf: [MAP-2.3, MEASURE-2.8]
  iso_iec_42001_annex_a: [A.7.2, A.7.3]
  eu_ai_act: [Article 10]
  owasp_llm_top_10: [LLM03]
  mitre_atlas: [T0020]
  csf_v2: [ID.AM-05, GV.SC-07]
  principle_ids: [RAI-02, RAI-04, RAI-06]  # accountability, fairness, privacy
```

Three things worth naming about this composition.

First, `principle_ids` is a *list*, not a scalar. A training-data provenance control legitimately participates in several principles at once. Forcing a 1:1 assignment either understates coverage or over-attributes controls to a single principle.

Second, the list is *internal*. The mod-102 chapter 01 crosswalk points *outward* at external frameworks (NIST, ISO, EU). `principle_ids` points *inward*, at the enterprise's own principle register. Keeping both under the same `crosswalk` key preserves the intuition that the field lists all the parent artefacts a control participates in, regardless of whether they are external or internal.

Third, `principle_ids` is the trigger surface for change propagation. When RAI-04 is amended, the review query is a one-liner against the control library: "list every control whose `crosswalk.principle_ids` contains RAI-04." Every returned control is queued for a lightweight review — statement still valid? testing still appropriate? evidence still relevant? — coordinated with the propagation workflow in chapter `05-policy-change-communications-and-deprecation-windows.md`.

## The traceability review — cadence, ownership, mechanics

Traceability is not a one-time set-up; it is a discipline. Two triggers cause a review:

- **Quarterly baseline review.** Every quarter, the mapping matrix is walked end-to-end and every row's `verified_date` is either refreshed (if the row still holds) or flagged (if any of its four IDs no longer exists or no longer means what it did).
- **On-change review.** Any change to a principle, a binding policy clause, a standard clause, or a control triggers a scoped review of the affected rows only. The change-propagation mechanics are in chapter `05-policy-change-communications-and-deprecation-windows.md`.

**Who runs it.** The `ai-governance-analyst` (level 15) executes the review — walks the matrix, runs the queries, opens the findings. The architect (level 50) signs off. The signature matters: an unsigned review is a spreadsheet nobody reads; a signed review is an accountability record that the architect stakes their name on and that regulators will ask to see.

**What breaks it.** Three failure modes recur:

1. *Principle amended without matrix update.* Legal or the RAI council refines a principle; the register is updated; the matrix is not walked. Every row referencing the old wording is silently stale.
2. *Control retired without back-reference cleanup.* A control is superseded (mod-102 chapter 06) and the matrix retains rows pointing at the retired ID. The queries return dangling references; the on-call analyst spends the quarterly review chasing broken pointers.
3. *New control added with no upstream registration.* A control is drafted, published, and starts producing evidence, but no matrix row is added. The control is orphaned upward — a rule 4 violation — and no principle "owns" its output.

None of these failures is exotic. All three are caught by two standing queries.

**The orphan detector.** A query over the matrix and the principle register: "list every `principle_id` in the register that has no row in the matrix with a non-null `control_id`." Non-empty output is a rule-1 violation. This is the query the board actually cares about — it is the direct answer to "which of our published principles have zero teeth?"

**The widow detector.** A query over the control library: "list every `control_id` whose `crosswalk.principle_ids` is empty or missing." Non-empty output is a rule-4 violation and identifies controls that are executing without a values-level warrant. These often reveal *engineering-side* controls that were authored by the risk engineer (mod-102 chapter 08 delegation) without an architect-side upstream pass.

Both detectors run on every review, and their output is a fixed section of the quarterly traceability report. Both should trend toward zero and stay there; a sudden non-zero result is the signal.

## Worked example — tracing the fairness principle end-to-end

To make the chain concrete, trace one principle through every link.

**Principle.** `RAI-04` — *Fairness and non-discrimination*. Statement (values-level): "AI systems used to make or materially influence decisions about individuals shall not produce unjustified disparate outcomes for legally protected classes or for classes the enterprise has designated as sensitive."

**Binding policy clause.** `POL-RAI-3.2` — from the enterprise Responsible AI Policy, section 3 (Fairness), clause 2. Statement (binding): "The organisation shall assess and document disparate impact on protected classes for every AI system whose outputs are used to make or materially influence decisions about individuals, prior to production release and at least annually thereafter." Verb is `shall`; authoriser is the board per chapter `01-ai-policy-hierarchy-authority-and-cadence.md`; matrix row asserts `POL-RAI-3.2` `derives_from` `RAI-04`.

**Standard clause.** `STD-FAIR-2.1` — from the enterprise AI Fairness Standard, section 2 (Disparate-impact testing), clause 1. Statement (normative and testable): "Disparate-impact testing uses the enterprise-approved metric set for the relevant decision type; results are compared against the enterprise threshold for each protected class; results below threshold require a documented remediation plan approved by the model-risk owner before release." <!-- needs-research: which specific disparate-impact metric families are on the enterprise-approved list; deliberately unnamed here to avoid inventing enterprise-internal facts -->

**Control(s).** Two mod-102 controls implement `STD-FAIR-2.1`:

- `AIC-FAIR-041` — *Pre-release disparate-impact assessment*. Statement: "For each in-scope AI system, the organization produces a disparate-impact assessment against the enterprise-approved metric set for each designated protected class prior to production release." Testing procedure samples pre-release packages and confirms the assessment exists, uses the approved metrics, and names remediation for any below-threshold result.
- `AIC-FAIR-042` — *Annual disparate-impact re-assessment*. Statement: "For each in-scope AI system in production, the organization re-runs the disparate-impact assessment at least annually and on material change to training data or model architecture."

Both controls carry `crosswalk.principle_ids: [RAI-04]` at minimum; `AIC-FAIR-041` additionally lists `RAI-02` (accountability) because release gating is an accountability outcome too.

**Evidence artefact.** `fairness-disparate-impact-report.json`, conforming to schema `ENT-SCHEMA-FDIR-1.1`, produced per-release by `ai-evaluation-engineer` and owned by `ai-governance-analyst` on a retention of 10 years, immutability append-only. This is the actual file an auditor is served when asked "show me the evidence that principle RAI-04 has teeth for system X."

Two matrix rows now express the whole trace:

```yaml
- principle_id: RAI-04
  policy_clause_id: POL-RAI-3.2
  standard_clause_id: STD-FAIR-2.1
  control_id: AIC-FAIR-041
  evidence_artefact_ref: fairness-disparate-impact-report.json@ENT-SCHEMA-FDIR-1.1
  verified_date: 2026-06-14
- principle_id: RAI-04
  policy_clause_id: POL-RAI-3.2
  standard_clause_id: STD-FAIR-2.1
  control_id: AIC-FAIR-042
  evidence_artefact_ref: fairness-disparate-impact-report.json@ENT-SCHEMA-FDIR-1.1
  verified_date: 2026-06-14
```

The board question — "does *fairness* dereference into anything testable?" — now has a two-row answer, each row terminating in a file that a real role really produces on a real cadence with a real retention. That is what "not decorative" looks like.

## Where the mapping lives

The mapping matrix is a machine-readable file, versioned adjacent to the control library and the policy corpus, in the same repository, under the same review discipline. The concrete form has two defensible options.

**Option A — YAML/CSV index.** A single YAML or CSV file (`traceability-matrix.yaml` / `.csv`) with one row per edge assertion. Cheap, diff-able, script-able, human-legible. This is the pragmatic starting form and is sufficient for most enterprises, especially early in the programme's life. All the queries in this chapter (orphan detector, widow detector, `verified_date` staleness) are one-liners over this file with standard tooling.

**Option B — OSCAL profile part relationships.** OSCAL (mod-102 chapter 04 introduces it as the serialisation format for the control library) supports profile *parts* and cross-references that can express principle → control relationships as first-class links between OSCAL documents. <!-- needs-research: whether OSCAL 1.x already ships a first-class construct for "principle" as an artefact class, or whether the modelling requires representing principles as a bespoke catalog whose controls are the principles themselves — worth stating precisely against the current OSCAL 1.x spec --> This is heavier but composes cleanly with the OSCAL-serialised control library.

Both options can coexist during a transition: the YAML/CSV file remains the authoring surface, and an OSCAL projection is generated for consumption by downstream OSCAL-aware tooling. Whichever form you pick, the invariants matter more than the format: the file is versioned with the corpus, every row carries the six mandatory columns, every column value is a stable ID, and the four traceability rules can be checked by query.

## Two common shape mistakes

**Mistake 1 — Complete-bipartite mapping ("everything traces to everything").** The team, wanting the matrix to look comprehensive, maps every principle to every control. Every principle then technically has downstream coverage; every control has an upstream reference; rule 1 and rule 4 both pass. The matrix has become useless: change propagation now signals a review of the entire library on every principle amendment, so nobody reviews anything. The corrective is the `edge_kind` column and honest discipline about it — a control is either a substantive implementer of a principle (`derives_from`) or it isn't. "This control kind of relates to fairness" is not a mapping; it is noise.

**Mistake 2 — Conflating "derives from" with "assists with".** A privacy control (`AIC-PRV-071`, say, differential-privacy noise injection on a training pipeline) genuinely assists fairness — because DP tends to reduce memorisation of minority-class outliers and therefore has fairness-adjacent effects. It does not, however, *derive from* the fairness principle; it derives from the privacy principle and *assists with* the fairness principle. If the matrix treats "assists with" edges as if they were "derives from" edges, two things break: (a) revoking the fairness principle wrongly triggers a review of every privacy control, and (b) the fairness coverage query overstates real fairness coverage because DP controls appear as if they were fairness implementers. Model both edge kinds explicitly (`edge_kind: derives_from` vs `edge_kind: assists_with`) and query on the former for coverage, the latter for defence-in-depth reporting.

Both mistakes are cheap to make in a spreadsheet and expensive to leave in place; both are caught at the first serious review, if the review actually runs.

## Summary

Every enterprise Responsible AI principle must dereference — through at least one binding policy clause, at least one standard clause, and at least one testable control — into a named evidence artefact, or it is decorative and treated as such by anyone with cause to look. The traceability chain is a directed acyclic graph with N:M edges, stored as a versioned mapping matrix whose six mandatory columns (`principle_id`, `policy_clause_id`, `standard_clause_id`, `control_id`, `evidence_artefact_ref`, `verified_date`) are queried against four rules — no zero-degree principles, no foundling policy clauses, no untested standard clauses, no widow controls — every quarter and on every change. Composing with mod-102 adds a single `principle_ids` list to the control crosswalk, which becomes the trigger surface for change propagation. The `ai-governance-analyst` runs the review; the architect signs it. Orphan and widow detectors are the two standing queries. Get the traceability discipline right and the principle wall becomes a principle *graph* — walkable in either direction, testable at every link, defensible in every audience.
