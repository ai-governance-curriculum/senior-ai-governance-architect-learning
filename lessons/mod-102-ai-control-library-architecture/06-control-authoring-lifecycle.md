# The control-authoring lifecycle

## Why this chapter exists

Chapters 01–05 defined the *shape* of the library: what a control entry looks like, how the governance-family and threat-family sources compose into it, how it serialises in OSCAL, and how inheritance and compensation lets it survive contact with real systems. All of that assumes the library is *published* — that a downstream analyst can pin against `AIC 2026.1.0` and know the catalog is stable at that version.

The library gets to that stability only through a lifecycle. Proposals arrive from every seat in the org — a risk engineer discovers a new attack pattern, a governance analyst hits a schema gap while collecting evidence, an evaluation engineer's audit trail is missing a field, a jurisdictional change lands a new obligation, a vendor introduces a new failure mode. If the lifecycle is not designed, the library either freezes (nothing ever changes) or churns (nothing ever stabilises). Both are unusable.

This chapter designs the lifecycle: the stages a control passes through, the change-classification that governs versioning, the deprecation and sunset discipline, the change-log and release-note contract, and the routing rules that keep architect, risk-engineer, and analyst in their seats.

## The five lifecycle stages

Every control lives at exactly one of five statuses at any time.

| Status | What it means | Who can consume it |
|---|---|---|
| `proposed` | A candidate control has an ID reservation and a drafted entry, but has not been approved. | No one — proposals are visible in the working branch only. |
| `published` | The control is in the current released catalog; downstream artefacts can pin to it. | All library consumers. |
| `deprecated` | The control is scheduled for removal; a `superseded_by` pointer names its replacement. | Existing SSPs continue to reference it during the deprecation window; new SSPs should reference the replacement. |
| `sunset` | The control is no longer authoritative; new pins are prohibited; existing SSPs must have migrated. | No new consumers; historical SSPs may still reference the sunset ID (immutable audit trail). |
| `withdrawn` | The control was proposed and rejected, or was published and then discovered to be wrong. Applies only pre-adoption or in the rare "we authored a bad control" case. | No one. |

The status field lives on the entry (`status: published`), and every transition is a governed event. `proposed → published`, `published → deprecated`, `deprecated → sunset`, and `proposed → withdrawn` are the legal transitions; nothing else. `sunset` is terminal.

## Stage 1 — proposal

Anyone in the AI governance program can raise a proposal. The proposal has a fixed shape:

- **Trigger** — the incident, obligation, evaluation finding, vendor change, or jurisdictional update that motivates the proposal. Concrete; not "we should have this."
- **Draft entry** — a first pass at the seven fields from chapter 01, with obvious gaps marked (`<!-- needs-research: … -->` is welcome).
- **Overlap check** — a scan against existing controls with a claim that no existing control's outcome already covers the proposed one, or a claim that the proposal is a *modification* of an existing control rather than a fresh one.
- **Affected profiles** — which baseline profiles would need to include or tailor the new control.
- **Downstream impact** — which analyst / engineer / evaluation roles will produce or consume evidence for it.

The proposal is opened as a PR (or the equivalent in whatever repo the catalog lives in) with the draft entry, the proposal metadata, and a suggested ID (reserved on merge).

## Stage 2 — architect review

Every proposal goes through architect review before adoption. The architect is not the sole author — proposals often originate with `ai-risk-engineer` at level 25 or `ai-governance-analyst` at level 15 — but the architect is the *approver* for the catalog. Review focuses on four questions:

1. **Is this a new outcome?** If an existing control's statement already commits to the outcome, the proposal is *not* a new control; it is either an update to that control (crosswalk addition, guidance enrichment) or a compensating-control shape (chapter 05).
2. **Is the statement outcome-oriented, testable, and free of mechanism?** Applying the discipline from chapter 01 field-by-field.
3. **Is the applicability filter tight?** A control that applies to "all AI systems" is almost always over-scoped and will breed waivers.
4. **Are the evidence and testing fields sufficient for the level-15 and level-25 roles to consume without a follow-up conversation?** If the analyst cannot collect against the evidence contract as written, the contract is not done.

Architect review is public in the proposal thread. Reject/revise/approve is explicit, and the review comments become part of the change history.

## Stage 3 — adoption ceremony

Approved proposals move into the *next* release. Approval is not a merge — it is a queue entry. The library ships on a cadence (chapter defines this below); at cut, all approved proposals in the queue become part of that release.

The adoption ceremony has three steps:

1. **ID allocation** — the reserved ID is finalised. Once allocated, the ID is immutable for the life of the library; it is never reassigned, even after sunset.
2. **Version stamping** — the control starts at `1.0.0`. The library's `metadata.version` is bumped per the release scheme (below).
3. **Release-notes entry** — every adopted proposal gets one paragraph in the release notes: what it commits the org to, why the org adopted it, which profiles include it, which roles produce evidence for it.

## Stage 4 — modification

Once published, a control changes only through a change proposal. The change is classified by *breaking impact*, using the semver-ish scheme from chapter 01:

- **Major (breaking)** — statement rewrite that changes the outcome; applicability filter narrowing or broadening; evidence-contract change that invalidates historical evidence; crosswalk deletion. Downstream SSPs must revisit before the release.
- **Minor (additive)** — additional crosswalk edge; additional testing method that supplements rather than replaces; guidance expansion; new evidence artefact added alongside existing ones.
- **Patch (editorial)** — typo, clarification that does not change interpretation, formatting fix.

Every classification is called out in the change proposal; the architect reviews the classification alongside the change. Downstream teams pin to major or major.minor and consume patch automatically; a mis-classified change (a major shipped as minor) is the fastest way to blow trust in the release contract, and the architect's job is to be picky here.

## Stage 5 — deprecation and sunset

Retirement is a two-phase move: deprecation, then sunset. Both are governed events.

### Deprecation

Deprecation is the announcement that the control is on its way out. It requires:

- A **`superseded_by`** pointer, if a replacement exists. A control that is deprecated with no successor implies the outcome is no longer commanded — that is a substantive decision, and should be traceable to a policy change (mod-103) or a jurisdictional change (mod-104), not to a catalog whim.
- A **migration guide** — what SSPs pinned to the deprecated control need to do to move to the successor (which fields translate 1:1, which need re-authoring, which evidence artefacts remain valid, which need re-collection).
- A **deprecation window** — a minimum period between deprecation and sunset. Different control families deserve different windows; a governance-family deprecation typically deserves 12 months, a threat-family engineering control 6 months, an editorial-only deprecation of a duplicate 3 months. The library preface publishes the minimum windows per family.

During the deprecation window, both the deprecated control and its successor are consumable. Existing SSPs are not broken; new SSPs should adopt the successor.

### Sunset

At end of the deprecation window, the control transitions to `sunset`. Three effects:

- No new SSP may pin to it. The OSCAL profile-authoring toolchain rejects new inclusions of a sunset control.
- Existing SSPs that still reference it are flagged as *not-migrated* in the governance analyst's tracker; a POA&M entry lands to migrate them.
- The sunset control's OSCAL entry is retained in the catalog for the life of the catalog. It is *not* deleted. This is critical: five-year-retention audit packets need to be able to dereference the control they were assessed against.

Sunset is terminal. No transition back. If the outcome the sunset control commanded later needs to return, a new control with a new ID is authored.

## Withdrawn — the "we authored a bad control" case

Occasionally a published control is discovered to be wrong — an incorrect crosswalk that pointed audiences to the wrong obligation, a statement that turned out to be impossible to satisfy anywhere in the org, an applicability filter that scoped in a whole population that was never in scope. Withdrawal is the escape hatch: the control transitions directly to `withdrawn`, a correction notice ships, and downstream artefacts pinned to it are flagged for re-authoring.

Withdrawal is rare and never the routine tool for change. If withdrawal is happening more than a few times a year, the review discipline in stage 2 is inadequate.

## The release cadence

A predictable cadence is the foundation of downstream trust. Two release streams work at architect scope:

- **Scheduled minor releases** — quarterly. Bundle all approved additive proposals + editorial fixes into one release; publish release notes; downstream teams plan around the cadence.
- **Unscheduled major releases** — as needed. A jurisdictional change (EU AI Act implementing act, new state AI act) or a serious incident often forces a major before the next scheduled minor. Communicate the reason for the out-of-cycle release explicitly.

Patches ride whichever release is next; they do not warrant their own release. Batching editorial fixes reduces downstream noise.

## The release-notes contract

Every release ships with a release-notes document that lists, per control, what changed and why. The shape:

```markdown
# Enterprise AI Control Library 2026.1.0 (2026-Q1 minor release)

## Added
- AIC-ENG-041 (Untrusted-content instruction isolation, 1.0.0) — new engineering-family
  control for prompt-injection defence. Reason: rising incident frequency in agent
  systems; new OWASP LLM01 alignment. Applies to tier-1 / tier-2 generative and agent
  systems.

## Modified
- AIC-DAT-014 (Training-data provenance record) — bumped 1.2.0 → 1.3.0 (minor).
  Added `mitre_atlas_techniques: [AML.T0020]` crosswalk edge; enriched guidance
  with pointer to enterprise dataset-license registry.

## Deprecated
- AIC-EVD-005 (Legacy training-log capture) — deprecated in favour of AIC-EVD-022
  (Enterprise inference audit trail). Deprecation window: 12 months. Migration
  guide: MIG-EVD-005-to-022.md.

## Sunset
- AIC-GOV-002 (Interim governance-committee minutes template) — sunset at end of
  its 6-month deprecation window; superseded by AIC-GOV-011.

## Editorial
- AIC-GOV-001 (Responsible AI principles reference) — patched 1.4.1 → 1.4.2
  (typo in guidance).
```

Every entry names the ID, version delta, what changed, and — for adds and mods — *why*. The why field is what downstream consumers use to decide whether to migrate proactively or wait; do not omit it.

## The change log and the audit trail

Beyond release notes, the library carries a *per-control change log* — every version, its diff, its release, its rationale, its approver. Kept in the OSCAL back-matter or in a companion file next to the catalog, the change log is what an auditor uses to answer "when did this control's statement become what it is today, and who approved it?" That question comes up more often than architects expect.

The immutability constraint: change-log entries are append-only. A bad change is corrected by a new change entry (a patch or a withdrawal), not by editing history.

## Routing rules — who does what during the lifecycle

The lifecycle only stays clean if the routing rules are explicit:

| Activity | Owned by | Reviewed by | Consulted |
|---|---|---|---|
| Proposal drafting | Any role (analyst, risk engineer, evaluation engineer, third party) | Architect | Adjacent roles (per proposal shape) |
| Overlap check | Proposer | Architect | Library curator (if named) |
| Statement / applicability / evidence-contract / crosswalk authorship | Architect | Domain experts | Risk engineer, analyst |
| Guidance / testing procedure authorship | Risk engineer (engineering controls) or analyst (analyst-collected controls) | Architect | Evaluation engineer |
| Adoption ceremony | Architect | Head of AI governance (for material adds) | Legal for regulatory-driven adds |
| Modification classification | Architect | Head of AI governance for major | — |
| Deprecation / sunset decision | Architect | Head of AI governance | Affected system owners |
| Withdrawal | Architect + Head of AI governance | AI governance council | Affected system owners |

The pattern: architect *owns* every lifecycle stage's decision; different roles *co-author* different fields; the head-of-AI-governance (level 60) is the escalation for material additions and for withdrawals — the architect does not approve a withdrawal alone.

## Two common shape mistakes

**Mistake 1 — Continuous main-branch releases.** Every merged change immediately updates the catalog at HEAD, no versioned release. Downstream artefacts pin to `latest` and quietly break; audit-trail dereferences at year N+3 return a different catalog than the one the assessment was made against. Cut versioned releases on a cadence, always.

**Mistake 2 — Deleting sunset controls to keep the catalog tidy.** Sunset controls stay in the catalog forever, marked with their status. Deleting them is the fastest way to lose an auditor's trust: last year's assessment referenced a control ID that no longer exists in the catalog, and no one can prove the reference was valid at the time.

## Summary

The AI control library moves through a five-stage lifecycle — proposed / published / deprecated / sunset / withdrawn — on a predictable release cadence (quarterly minors, on-demand majors, batched patches). Proposals originate anywhere; architect approves; modifications are classified by breaking impact; deprecation runs a family-appropriate window before sunset; sunset controls persist in the catalog forever. Release notes and per-control change logs make every version dereferenceable at audit time. Routing rules keep proposal drafting distributed but adoption decisions centralised in the architect seat, with head-of-AI-governance escalation for material changes. Cadence + versioning + immutable history is the trust contract with downstream — break any of the three and the library becomes unpinnable.
