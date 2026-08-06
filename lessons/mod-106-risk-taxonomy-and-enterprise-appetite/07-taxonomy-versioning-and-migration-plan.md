# Taxonomy versioning and migration — keeping the appetite architecture alive across releases

## Why this chapter exists

Chapters 02 through 06 build a working system: a three-axis taxonomy with a dependency map, an appetite architecture with four artefacts, an aggregation model with three portfolio views, a quantification contract with three-stance scoring and five quality gates, and a set of adaptations of frontier-lab shape templates. All of that is *release 1*. Release 1 will be wrong in specific places by release 2. Release 2 will be wrong in different places by release 3. The regulatory environment moves; the enterprise footprint moves; the underlying AI systems move; the research surfaces new dangerous capability categories; the calibration studies (chapter 06) surface MECE violations that require category re-cutting. If the taxonomy, tolerance table, aggregation model, and quantification contract cannot be *versioned and migrated* — cleanly, defensibly, with historical continuity — the entire architecture ossifies within a year and the enterprise is back to the free-text failure mode chapter 01 warned against.

The level-50 architect owns the versioning and migration process. Not the individual release cycle's content (that is proposed by the risk engineer, the evaluation engineer, and the head of AI governance based on their operational experience of the current release) but the *shape of the release process*: what constitutes a breaking change, how a new category is added, how a deprecated category is retired, how historical risk-register entries are re-classified, how the change is communicated to consumers of the taxonomy (the ai-risk-engineer, the ai-governance-analyst, internal audit, the model-risk committee, the audit committee, in some cases external assurance bodies).

This chapter is the closing chapter of the module. It walks the semver-shaped versioning scheme, the amendment-proposal workflow, the migration plan template, the two-way traceability guarantee across versions, and the change-communications contract. It composes with mod-103's policy-versioning discipline (which the enterprise already runs for policies) and mod-102's control-library-versioning discipline (chapter 06 of that module).

## The joint-versioning discipline — four artefacts, one release cycle

The taxonomy, the tolerance table, the aggregation model, and the quantification contract are four artefacts. They are *versioned jointly*. Release 3.0.0 of the taxonomy ships alongside release 3.0.0 of the tolerance table, release 3.0.0 of the aggregation model specification, and release 3.0.0 of the quantification contract. The four artefacts are inter-dependent:

- A new category in the taxonomy needs a row (or rows) in the tolerance table.
- A change in the tolerance table's normalisation banding requires a matching update in the quantification contract.
- A change in the dependency map affects the aggregation model.
- A change in the quality gates in the contract affects what the risk engineer files.

Shipping any one of the four artefacts independently leaves the others in a stale state and produces the class of bugs that read like "our aggregation model is now referencing a dependency edge that the taxonomy no longer carries" or "our risk register has scores against a category the tolerance table has no cell for." The joint-versioning discipline avoids these bugs by construction.

Joint versioning does not mean every release changes every artefact. A patch release might affect only the quantification contract (say, a calibration threshold adjustment). A minor release might add a category to the taxonomy, add rows to the tolerance table, and adjust the aggregation model to read the new category, without changing the contract. A major release usually touches all four. The four version numbers move in lockstep even when only one artefact changes materially — the version stamp is what makes the four artefacts locate-in-time together.

## The semver-shaped versioning scheme

The four artefacts use `MAJOR.MINOR.PATCH` semantics. The specific definitions:

- **PATCH** — editorial changes that do not affect any consumer's interpretation of any artefact. Fixing a typo in a category definition; clarifying wording in a tolerance-table rationale; updating a citation to a newer edition of a source; renumbering an internal cross-reference. No migration action required.

- **MINOR** — additive changes that new consumers must handle, existing consumers can continue to run against without breaking. Adding a new taxonomy category; adding new rows to the tolerance table for that category; adding a new escalation trigger; adding a new evaluation method to the contract's calibration section. Existing risk-register entries filed against the previous version remain valid. New system launches must file against the new version.

- **MAJOR** — breaking changes that all consumers must migrate to. Removing or renaming a taxonomy category; changing the tolerance-table normalisation banding; changing the composition function in the quantification contract; changing the semantics of a dependency edge kind; changing the meaning of a stance (inherent / residual / control-defeated). Historical risk-register entries must be re-mapped under the migration plan.

The taxonomy is going to have several minor releases per year in a healthy operational cadence — the field is moving too fast for a static enterprise taxonomy. Major releases should be rare (once every 18–24 months is the target), because each major release imposes a real migration cost. The versioning scheme is calibrated so that MAJOR is genuinely expensive; teams should not casually break categories the enterprise has been scoring against for a year.

## The amendment-proposal workflow

New categories, retirements, and dependency-map edits enter through a defined workflow. The workflow is small enough to run smoothly; it is not the compliance-workflow-that-swallows-a-quarter shape.

### Sources of amendment proposals

Amendments come from three legitimate sources. Any amendment that does not trace to one of the three should be interrogated:

- **Internal — from operations.** The risk engineer, the AI governance analyst, the model-risk committee, or the head of AI governance surface a category that does not fit an actual case they need to score. The proposal explains the case, the current taxonomy's failure to accommodate it, and the proposed change (usually a new category, a dependency edge, or a definition refinement).
- **Internal — from calibration.** The evaluation engineer's calibration studies (chapter 06) surface a MECE violation, a definition ambiguity that produces low inter-rater agreement, or a category that is unused. The proposal explains the finding and the proposed change (usually a category split, merge, or definition sharpening).
- **External — from environment change.** Regulation adds an obligation category the taxonomy did not carry (EU AI Act Article 50 obligations required a `synthetic-media-attribution` category the pre-2024 enterprise taxonomies typically did not have). A frontier lab publishes a dangerous-capability category the enterprise should track. AIID / OECD.AI incident data surfaces a new category of realised harm. The proposal explains the environment change and the enterprise-scope adaptation.

An amendment that does not identify one of these three sources is a category the architect probably invented for aesthetic reasons. Reject or send back with a source ask.

### The amendment record

Each proposal is filed as an amendment record. The record carries:

- **Source and driver.** Which of the three sources; what specifically prompted the proposal; who proposed it.
- **Proposed change.** New category / retirement / dependency-map edit / definition sharpening — with the specific text.
- **Impact assessment.** Which existing categories are affected; which tolerance table cells need to be authored, retired, or edited; which aggregation-model dependency edges change; which risk-register entries need to be re-mapped.
- **Cross-corpus mapping.** For a new category, which reference corpora (NIST GenAI Profile, ISO 23894, AIRO, MIT Repository, AIID / OECD.AI) it maps to. Categories with no reference-corpus mapping face higher scrutiny — the composition move (chapter 02) prefers derivable categories.
- **Migration effort estimate.** How many historical risk-register entries are affected; whether the migration is mechanical (a rename) or judgemental (a split that requires per-entry decisions); the estimated time to complete migration.
- **Communication plan.** Who is told; when; through what channels; what the training or refresher content looks like for downstream consumers.

### The review and ratification path

Amendments are reviewed by a small group — typically the architect, the head of AI governance, the risk engineer, and the evaluation engineer — at a defined cadence (monthly for a fast-moving programme; quarterly once stable). Reviews consolidate multiple amendments into a single release; releases go out on a defined schedule (say, every quarter for minor releases; every 18–24 months for major releases). Ratification is by the head of AI governance for minor and patch releases; major releases additionally involve the board / audit committee because they change the artefacts the board previously ratified.

## The migration plan template

Every release ships with a migration plan. Patch releases have a trivial one-paragraph migration plan ("no action required"). Minor releases have a scoped migration plan (new categories: what to file new entries against; tolerance-table additions: what cells are new). Major releases have a detailed migration plan; this section is about that.

The migration-plan template has seven sections:

**Section 1 — Change enumeration.** What changed, at what altitude, in each artefact. Not the entire release notes; the enumeration of changes with migration consequences.

**Section 2 — Historical risk-register entry re-mapping.** For each retired or renamed category, the per-entry re-mapping approach. Three flavours:

- **Mechanical re-mapping.** The category was renamed or split-with-a-clean-rule. Historical entries are re-mapped automatically by tooling; the migration is complete when the tooling runs.
- **Judgemental re-mapping.** The category was split into two or more successors whose distinction requires case-by-case judgement. Historical entries are queued for the risk engineer to re-map; the migration plan specifies the queue, the review cadence, and the deadline.
- **No re-mapping.** The category was retired without a successor (rare — usually happens when a category turns out never to have been used). Historical entries carry a deprecation stamp and are excluded from the current portfolio view; they remain in the historical record.

**Section 3 — Two-way traceability guarantee.** The migration plan promises that (a) every historical entry can be located under its original version's category, and (b) every historical entry can be located under the current version's mapped category. This is what makes the audit committee's "how has our exposure on category X evolved over time?" question answerable across versions. Traceability is bi-directional: given a current category, list all historical entries that mapped to it (possibly under different past names); given a historical category, list what it maps to now. Section 3 of the migration plan is where this guarantee is made concrete for the release.

**Section 4 — Tolerance table migration.** For breaking changes to the tolerance table (retolerance-banding shift; retirement of a cell; new stop-shipping thresholds), the migration plan specifies whether historical breach records are re-computed under the new bands or left under the old bands with a version stamp. The default is *re-compute* — the portfolio view under the new release reads the whole history through the new bands — but re-computation must be justified explicitly, because it changes the appearance of historical trends.

**Section 5 — Aggregation model migration.** Where dependency-map edges change, the migration plan specifies whether historical portfolio views are re-computed under the new edges. The default is *do not re-compute historical portfolio views* — the historical view under version N stays under version N — but the current view under version N+1 uses the new edges. This preserves the historical record; the audit committee reads historical views with the version stamp that produced them.

**Section 6 — Quantification-contract migration.** Where the contract changes (scale change, composition change, quality-gate change), historical scores may need to be re-scored under the new contract. The migration plan specifies: which historical entries are re-scored; on what cadence; by whom; how the intermediate state (some entries under old contract, some under new) is rendered on the portfolio view during the migration.

**Section 7 — Communication and training.** Who is told; when; through what channels. Downstream consumers — the risk engineer, the AI governance analyst, internal audit, the model-risk committee, in some cases the external assurance body — each receive a communication packet appropriate to their role. Training content is provided for material changes; the training is completed before the release becomes the effective operating version.

## The two-way traceability guarantee

The single most important operational feature of the versioning discipline is the two-way traceability guarantee across versions. Concretely: if the board asked in Q3-2026 "how has our exposure on the `discriminatory-decision-in-employment-context` category evolved since Q1-2024?", the query must be answerable even though (a) the category may have been named differently in Q1-2024 (say, `automated-hiring-decisions`), (b) some historical entries may have been re-scored under a new contract, (c) the tolerance banding may have been re-normalised in a major release, (d) the taxonomy may be on version 4.x and the historical scoring may be under versions 1.x through 3.x.

The two-way traceability is enforced by mod-108's evidence architecture: every historical entry carries its version stamp; the taxonomy carries the version-migration graph (which category-at-version-N maps to what category-at-version-N+1); the aggregation model can compose the graph to produce a versioned time series. The architect designs the graph shape and enforces the guarantee at every release. Skipping the migration graph on any release breaks the guarantee for that release and everything after.

## The change-communications contract

Different consumers of the taxonomy need different communication content. The change-communications contract enumerates who gets what.

| Consumer | Communication artefact | Cadence |
|---|---|---|
| AI risk engineer (level 25) | Full release notes; migration checklist; training on new categories | With each release; training completed before effective date |
| AI governance analyst (level 15) | Executive summary; new categories with definitions; changed workflow steps | With each release |
| Head of AI governance (level 60) | Executive summary; sponsorship brief for the audit committee for major releases | With each release; audit-committee brief for major only |
| Internal audit | Full release notes; migration completeness report at release + N days | With each release; completeness report at release + 60 days |
| Model risk committee | Executive summary; changes affecting current pipeline items | With each release |
| Audit committee / board | Major-release summary; new stop-shipping thresholds | Major releases only |
| External assurance body (if applicable) | Full release notes; migration plan; two-way traceability guarantee statement | Major releases only, ahead of surveillance audits |
| Peer teams (data-risk, cyber-risk) | Summary of dependency-map changes affecting shared risks | Minor and major releases |

The communication artefacts are drafted alongside the release; a release is not effective until the required communications are delivered. This is the same discipline mod-103 chapter 05 argued for at the policy level; the taxonomy inherits it.

## The retirement pathway

Categories retire. The retirement pathway has three stages:

**Stage 1 — Deprecation.** The category is marked deprecated in a minor release. Existing scores against it remain valid; new scores against it are not accepted; a suggested successor category is named. The deprecation period is at least two release cycles (typically six to twelve months) so downstream consumers have time to migrate practice.

**Stage 2 — Migration.** During the deprecation period, historical entries against the deprecated category are re-mapped per the migration plan. Where the re-mapping is judgemental, the risk engineer queues and completes it; the migration completeness is tracked and reported.

**Stage 3 — Retirement.** In a subsequent major release, the deprecated category is removed from the current taxonomy. Historical entries retain their deprecation stamp and their re-mapped current-category reference. The two-way traceability guarantee holds — the historical view under the release that carried the category still renders it; the current view under later releases renders the successor category.

Retiring without deprecation (skipping stages 1–2 and going straight to removal in a major release) is technically possible but is the version-management equivalent of a hostile refactor. Do not do it except for categories that were never used.

## Coordinating with the mod-102 control library, the mod-104 obligation register, and the mod-108 evidence architecture

The taxonomy does not version alone. Three sister artefacts move with it, and the migration plan must coordinate:

- **Mod-102 control library.** Controls reference taxonomy categories in their `mitigates` block (which category-risks they address). A retired category in the taxonomy causes stale references in the control library. The migration plan enumerates the affected controls and either re-references them to the successor category or opens a control-library amendment.
- **Mod-104 obligation register.** Regulatory obligations reference taxonomy categories where the obligation attaches to a specific harm shape. A retired category invalidates the obligation-to-category mapping. The migration plan coordinates with the obligation register's own versioning process (mod-104 chapter 07).
- **Mod-108 evidence architecture.** Every risk-register entry, every score, every portfolio view carries a taxonomy version stamp. The evidence architecture reads the migration graph the taxonomy release ships with; its consistency guarantees depend on the version stamps being present and the migration graph being complete.

The migration plan explicitly notes the coordination with each of the three. Where a coordination step is required (e.g., control library must be updated before the taxonomy release becomes effective), the sequence is enumerated in section 7 of the migration plan.

## A schematic release-notes fragment

```yaml
release:
  taxonomy_version: 2.0.0
  tolerance_table_version: 2.0.0
  aggregation_model_version: 2.0.0
  quantification_contract_version: 2.0.0
  release_type: major
  effective_from: 2026-Q4
  ratified_by:
    head_of_ai_governance: yes
    audit_committee_awareness_date: 2026-08-14
    board_awareness_date: 2026-08-28
  changes:
    - artefact: taxonomy
      change_kind: category-split
      predecessor_id: RSK-CAT-018
      successor_ids: [RSK-CAT-042, RSK-CAT-043]
      rationale: >
        Calibration Q2-2026 study showed alpha=0.44 on the combined category,
        far below the 0.70 threshold. Splitting into extraction-attack-driven
        and weights-exfiltration-driven categories restores MECE.
    - artefact: tolerance_table
      change_kind: banding-renormalisation
      from: [low: 1-3, moderate: 4-6, high: 7-9]
      to: [low: 1-27, moderate: 28-125, high: 126-343, severe: 344-729]
      rationale: >
        Quantification contract moves to 1-9 ordinal * 3-axis multiplicative
        composition, producing a 1-729 raw range that requires re-banding.
    - artefact: quantification_contract
      change_kind: composition-function-change
      from: additive-weighted
      to: multiplicative
      rationale: >
        Aggregation model view B (exposure-weighted) was over-weighted because
        exposure was double-counted in the weighted sum; multiplicative removes
        this defect.
  migration_plan_reference: migration-2.0.0.md
  two_way_traceability_guarantee: preserved via version-migration graph vmg-2.0.0.json
  communications:
    - audience: ai-risk-engineer
      artefact: release-notes-2.0.0-engineer.md
      training_required: yes
      training_completed_by: 2026-10-15
    - audience: audit-committee
      artefact: audit-brief-2.0.0.md
      delivered: 2026-08-14
```

Exercise-05 requires the authoring of a migration plan at this level of detail for a specific proposed change.

## The two failure modes to design against

**Failure mode 1 — the un-versioned taxonomy.** The taxonomy is a shared document that gets edited in place. Categories are renamed, definitions are refined, dependency edges are added, all without a version stamp. Historical risk-register entries reference categories by name; when the name changes, the reference silently rots. Six months in, the aggregation model produces mysterious inconsistencies; the audit committee's "how have we evolved on category X?" question cannot be answered because the historical data no longer knows what X was called last year. The two-way traceability guarantee, enforced by version stamps and a migration graph, is what prevents this.

**Failure mode 2 — the frozen taxonomy.** The taxonomy is versioned but never actually released beyond 1.0.0. Every proposal for a new category is deferred to "the next release, when we have bandwidth." The next release does not happen. Meanwhile the regulatory environment moves, the enterprise footprint moves, and the operational calibration studies pile up MECE-violation findings that go unaddressed. Two years in, the appetite architecture is running against a taxonomy that no longer matches reality; scores are being filed against categories that no operational person believes describe the actual risks. The fix is a *committed cadence* — minor releases every quarter, whether or not the amendment queue is full — and a *forcing function* that any category proposed by a legitimate source (operations / calibration / environment change) is either merged into the next release or explicitly rejected with a rationale. Silent deferral is the poison.

## Summary

The taxonomy, the tolerance table, the aggregation model, and the quantification contract version jointly on a semver-shaped scheme. Minor releases every quarter add categories, refine definitions, tune the contract; major releases every 18–24 months make breaking changes with a full migration plan. Amendments enter through a defined workflow from three legitimate sources (operations, calibration, environment change) and are ratified by the head of AI governance for minor releases, additionally by the board / audit committee for major. Every release ships a migration plan with a two-way traceability guarantee — historical entries locate under their original version's category *and* under the current version's mapped category, so the audit committee's evolution-over-time queries return truthful answers across versions. Retirement is a three-stage pathway (deprecation → migration → removal) that respects the traceability guarantee. Coordination with mod-102 control library, mod-104 obligation register, and mod-108 evidence architecture is explicit in every release's migration plan. Skipping the versioning discipline produces one of two failure modes — the un-versioned taxonomy that silently rots or the frozen taxonomy that ossifies — both of which land the appetite architecture back in the free-text failure mode chapter 01 was written to prevent. This chapter closes the module; the exercises drill each of the seven chapters against a concrete enterprise scenario, and the resources file cites the primary sources the whole module composes on.
