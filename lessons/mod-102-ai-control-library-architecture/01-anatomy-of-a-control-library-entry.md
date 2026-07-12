# Anatomy of an AI control library entry

## Why this chapter exists

A control library entry is the smallest reusable unit of the enterprise AI governance program. Everything above it — policies, statements of applicability, jurisdictional profiles, assurance evidence packages, board reports — dereferences into control entries the way source code dereferences into functions. Get the shape of a single entry right and the rest of the library composes cleanly; get it wrong and every downstream artefact inherits the flaw, forever.

Most control catalogs an architect will inherit from mature enterprises get this shape *partially* right. They have a control ID and a statement. They have implementation guidance. They are usually missing three things: machine-readable applicability filters, an evidence contract with owners and cadences, and a written testing procedure that is not a paraphrase of the control statement. This chapter defines the seven fields every AI-library entry carries and states, per field, why omitting it is expensive.

The remaining chapters of mod-102 then build on this shape: chapter 02 shows how to compose governance frameworks into the shape, chapter 03 does the same for threat frameworks, chapter 04 serialises the shape in OSCAL, chapter 05 layers inheritance onto it, chapter 06 governs its lifecycle, chapter 07 hardens the evidence-contract field, and chapter 08 hands the engineering-side entries off to `ai-risk-engineer`.

## The seven fields — a running example

Every AI-library entry ships with the same seven fields. Here is one worked example we will refer back to throughout the chapter.

```yaml
control:
  id: AIC-DAT-014
  title: Training-data provenance record
  version: 1.2.0

  statement: >
    For each AI system in scope, the organization maintains a
    provenance record of every training dataset — source, license,
    lawful basis, collection date, transformation history, and
    integrity hash — sufficient to satisfy EU AI Act Article 10
    data-governance obligations and ISO/IEC 42001 A.7 data-lifecycle
    obligations.

  applicability:
    system_tier: [tier-1, tier-2]
    system_kind: [classical-ml, generative, foundation-model-adapter]
    jurisdictions: [EU, UK, US-Colorado]
    risk_appetite_band: [moderate, high, critical]

  implementation_guidance:
    - "Provenance records live in the model-registry provenance store; see ENT-PLATFORM-REG-08."
    - "Third-party dataset licenses are stored alongside the record with a copy captured at ingest."
    - "Integrity hashes are recomputed at every fine-tuning event."

  testing_procedure:
    - id: AIC-DAT-014-T1
      description: >
        Sample five in-scope training datasets per quarter and
        confirm the provenance record contains all seven mandatory
        fields.
      cadence: quarterly
      owner_role: ai-governance-analyst
    - id: AIC-DAT-014-T2
      description: >
        For each tier-1 system, verify integrity hash matches
        registry record.
      cadence: pre-release-and-quarterly
      owner_role: ai-risk-engineer

  evidence_contract:
    - artifact: dataset-provenance-record.json
      schema_ref: ENT-SCHEMA-DPROV-1.4
      owner_role: ai-governance-analyst
      producer_role: ai-risk-engineer
      cadence: per-training-run
      retention_years: 10
      immutability: append-only
    - artifact: sample-testing-report.md
      owner_role: ai-governance-analyst
      cadence: quarterly
      retention_years: 7

  crosswalk:
    nist_ai_rmf: [MAP-2.3, MEASURE-2.8]
    iso_iec_42001_annex_a: [A.7.2, A.7.3]
    eu_ai_act: [Article 10]
    owasp_llm_top_10: [LLM03]
    mitre_atlas: [T0020]
    csf_v2: [ID.AM-05, GV.SC-07]

  ownership:
    architect_owns: [statement, applicability, evidence_contract]
    ai_risk_engineer_owns: [implementation_guidance, testing_procedure]
    ai_governance_analyst_owns: [evidence-collection execution]

  status: published
  supersedes: []
  superseded_by: null
```

Every field earns its place. Let's take them one at a time.

## Field 1 — Identity: `id`, `title`, `version`

The identifier is a *stable* handle. Rename the title all you like; the ID is immutable for the life of the control. When the control is deprecated (chapter 06), the ID is reserved forever; no new control ever re-uses it.

**Naming convention.** Prefer a three-part ID: `<library>-<family>-<sequence>`, e.g. `AIC-DAT-014`. `AIC` scopes the library so multiple libraries can coexist (chapter 05 covers group / business-unit libraries). `DAT` is a family (data-lifecycle) that clusters related entries. `014` is monotonic within the family and is *not* reassigned.

**Version.** Semantic-ish versioning is enough: major bump on breaking changes to statement or applicability, minor bump on additive change, patch on editorial fixes. The lifecycle in chapter 06 defines *breaking* precisely; for the anatomy chapter it is enough to know that a version field is required because downstream artefacts pin against a version.

Omitting versioning is the single most common failure mode in inherited catalogs. Every SoA, every SSP, every audit packet is then pinned to *whatever the catalog said at the time we exported it* — a version-in-your-head that becomes uncheckable at audit time.

## Field 2 — Statement

The statement says what outcome the organisation commits to. It is *not* implementation ("we use SageMaker Model Registry"), *not* rationale ("because EU AI Act says…"), and *not* aspiration ("we should…"). Compare:

- **Bad**: "The organisation should record training data sources."
- **Bad**: "Training data provenance is tracked in the model registry."
- **Good**: "For each AI system in scope, the organization maintains a provenance record of every training dataset — source, license, lawful basis, collection date, transformation history, and integrity hash."

The good version names *who* (the organisation), *what* (a provenance record), *of what* (every training dataset), *with what fields* (the seven), and *for what systems* ("in scope" — resolved by the applicability filter below).

The rule: a statement should be readable by an auditor with no prior context and yield an unambiguous testable claim. If reading the statement does not produce a yes/no testable claim, rewrite.

**Length.** One to three sentences. Longer than that is almost always guidance masquerading as statement.

**Voice.** Present-tense declarative. Not "shall" (that is a policy verb; the policy taxonomy in mod-103 handles verbs). Not "should" (aspiration). The organisation *does* the thing.

## Field 3 — Applicability filter

This is the single most under-specified field in inherited catalogs — the one whose absence produces *every* Statement of Applicability fight, *every* over-scoping incident, and *every* "this control does not apply to us" waiver request.

Applicability is machine-readable. It is a set of predicates against attributes of the AI system:

- **System tier** — the harm-potential / autonomy / audience-scale tier the enterprise assigns (mod-106 defines these).
- **System kind** — classical-ML, generative, foundation-model-adapter, agent-with-tool-use, embedded, etc.
- **Use case** — customer-facing, employee-facing, HR/recruiting, healthcare, credit-decisioning, biometric, safety-critical, and so on.
- **Jurisdiction** — where the system is deployed *or* whose residents it processes data about.
- **Risk-appetite band** — the appetite tier under which the deploying business unit sits (mod-106 again).
- **Data class** — PII, PHI, CHD, IP, confidential, public.

Two properties matter:

**Predicates are AND across dimensions, OR within a dimension.** The control applies when *every* dimension you named is satisfied, and each dimension is satisfied when *any* of its listed values matches. If you write `system_tier: [tier-1, tier-2]` and `jurisdictions: [EU]`, the control applies to tier-1-OR-tier-2 systems in the EU. If you meant "tier-1 systems everywhere OR any tier in the EU," you needed two controls or a disjunctive filter shape; do not fake it with a wall of prose.

**Applicability is closed-world.** If a dimension is missing from the filter, the control applies regardless of that dimension. Do not populate every dimension defensively; populate the ones that constrain, leave the rest omitted, and document the closed-world convention once in the catalog preface.

An applicability filter is what lets downstream automation *select* controls for a given system without human triage. Chapter 04 shows the OSCAL representation; chapter 05 shows how filters compose with inheritance.

## Field 4 — Implementation guidance

Guidance is where the enterprise's opinionated choices land — "use the model registry provenance store," "hashes recomputed at fine-tune," "third-party licenses captured at ingest." Guidance changes with the platform. Statement does not.

Two rules:

**Reference, do not copy.** If ENT-PLATFORM-REG-08 already describes the provenance store, link to it; do not re-explain it. Guidance that duplicates a platform doc rots the moment the platform doc updates.

**Guidance is non-normative in the audit sense.** An auditor tests the statement and the testing procedure; guidance is optional colour that helps implementers pick a defensible path. If your control says "must" or "shall" in the guidance, that language belongs in the statement.

This is also where the `ai-risk-engineer` (level 25) most heavily contributes. The architect authors the field but the engineering role is the primary co-author for engineering-side controls — chapter 08 spells out that co-authorship contract.

## Field 5 — Testing procedure

The testing procedure states, *concretely*, how the outcome in the statement is verified. It has three obligatory sub-fields per test:

- **Description** — what the test does. Concrete enough that two independent testers produce the same evidence.
- **Cadence** — how often (per-release, quarterly, annually, on-trigger).
- **Owner role** — which role runs the test. Names a role, not a person.

Testing procedures answer three questions an auditor will ask cold: *how* did you check? *when* did you check? *who* did the checking? An entry whose testing procedure paraphrases the statement — "we check that we record data provenance" — fails all three.

**Number of tests.** Most controls need 1–3 tests. Fewer means the control is under-tested; more usually means the control is doing too much and should be split.

**Sampling.** For controls that touch large populations (all training runs, all inference requests), the testing procedure names the sampling method (random N per quarter, all tier-1, etc.). Do not pretend the auditor will look at every artefact.

## Field 6 — Evidence contract

The evidence contract is the API between the control and the ecosystem that consumes control output. Everything downstream — `ai-governance-analyst`'s evidence collection, `ai-evaluation-engineer`'s release-assurance package, the AIMS Statement of Applicability, EU AI Act Article 11 technical documentation, regulator submissions — reads evidence-contract fields.

Each artefact in the contract carries:

- **Name and schema reference** — human-readable filename plus a pointer to the machine-readable schema.
- **Producer role** — who generates the artefact (often the risk engineer or a platform component).
- **Owner role** — who is accountable for it existing (often the governance analyst; not always the same as producer).
- **Cadence** — per-release, per-training-run, quarterly, on-incident.
- **Retention** — how long it is kept, in years. Set by the longest applicable statute or the enterprise's baseline, whichever is longer.
- **Immutability** — append-only, write-once, mutable-with-log. This matters more than teams think; regulators expect append-only for tier-1 evidence.

Chapter 07 goes deep on the contract; the point in the anatomy chapter is that *the contract exists*, is *machine-readable*, and lives *inside the control entry* rather than in a separate document that will drift.

## Field 7 — Crosswalk

Every control library entry names every parent framework sub-category / obligation / technique / recommendation it composes with. Crosswalks are the connective tissue that makes the library reusable across:

- The Statement of Applicability (which uses crosswalks to justify inclusion of ISO/IEC 42001 Annex A controls, mod-105).
- Cross-jurisdictional reconciliation (which uses crosswalks to prove that one control covers three jurisdictions' obligations at once, mod-104).
- Assurance packages (which use crosswalks to point auditors at the evidence for a particular regulatory clause, mod-107 and mod-108).
- Executive reporting (which uses crosswalks to say "our CSF 2.0 GV.OC posture is covered by these 14 AI controls," mod-112).

Minimum crosswalk fields for a well-formed AI-library entry:

- **NIST AI RMF sub-categories** — always at least one.
- **ISO/IEC 42001 Annex A control IDs** — at least one where the control has an AIMS home.
- **EU AI Act articles** — where the control is triggered by an EU obligation.
- **OWASP LLM Top 10 / MITRE ATLAS / SAIF** — for engineering-side / threat-facing controls.
- **CSF 2.0 subcategories** — for executive-facing controls.

Crosswalks are *directional pointers*, not definitions. The primary source of truth for what MAP-2.3 says remains NIST AI RMF; the crosswalk simply asserts that this control participates in achieving that outcome. Do not paraphrase the source in the crosswalk.

## Ownership: architect vs. co-author roles

The seven fields above are not owned by the same role. The distinction matters for the delegation contract in chapter 08 and for the authoring-lifecycle in chapter 06.

| Field | Architect (level 50) | AI risk engineer (level 25) | AI governance analyst (level 15) |
|---|---|---|---|
| id / title / version | owns | consulted | informed |
| statement | owns | consulted | informed |
| applicability | owns | consulted | informed |
| implementation guidance | reviews | co-authors | informed |
| testing procedure | reviews | co-authors | executes |
| evidence contract | owns | reviews | executes collection |
| crosswalk | owns | consulted | informed |

Field-level ownership is the discipline that keeps the architect at architect scope. The architect *authors* the statement, applicability, evidence contract, and crosswalk; the engineer *co-authors* guidance and testing; the analyst *executes* against the contract. If the architect is filling in the testing procedure alone, the risk-engineer seat is not being used.

## Two common shape mistakes

**Mistake 1 — Statement + guidance conflation.** A statement of "Training data is validated via the ingest pipeline configured in ENT-PLATFORM-DAT-04" combines the outcome with the platform choice. Auditors then have to grade the platform choice, and if the platform changes, the statement needs a rewrite. Split: statement names the outcome, guidance names the platform.

**Mistake 2 — Testing procedure that reads back the statement.** A procedure of "Verify that training data is validated" is not a procedure; it is the statement in the imperative mood. A real procedure names an operation — sample five datasets, recompute hashes, compare — with a cadence and an owner.

Both mistakes are cheap to make and expensive to leave in place. Every downstream artefact — SSP, audit packet, regulator response — inherits the flaw.

## Summary

Every AI control library entry ships with seven fields: identity, statement, applicability filter, implementation guidance, testing procedure, evidence contract, and crosswalk. Identity is stable; statement is a testable outcome claim; applicability is a machine-readable predicate; guidance is opinionated and mutable; testing is concrete and owner-scoped; evidence is the API to the rest of the program; crosswalks are directional pointers to composed frameworks. Field-level ownership keeps architect, risk-engineer, and analyst in their seats. Get the anatomy right in a single entry and the rest of mod-102 is the composition, serialisation, inheritance, lifecycle, evidence, and delegation layered on top.
