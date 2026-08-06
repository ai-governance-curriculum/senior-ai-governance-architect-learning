# exercise-06: Evidence-Contract Hardening Drill

**Estimated effort:** 3 hours

## Objective

Harden the **evidence contract** on three controls of different families — one governance-family, one engineering-side, one third-party — so that each contract survives every one of the architect's four stage-2 review moves from chapter 07 (force the sampling question, force the immutability question, force the retention question, force the schema question). Then draft the SSP-level `evidence_binding` block that a specific Northbrook system produces against each contract.

The evidence contract is the API between the library and every downstream consumer of assurance, assessment, audit-pack assembly, and post-market surveillance. A weak contract propagates weakness everywhere it is consumed; a hardened one lets the level-15 analyst collect and the level-35 evaluation engineer review without a follow-up conversation. This exercise is the drill that separates the two.

## Prerequisites

- Chapter [`07-evidence-contract-per-control.md`](../07-evidence-contract-per-control.md) read once.
- Chapter [`01-anatomy-of-a-control-library-entry.md`](../01-anatomy-of-a-control-library-entry.md) for the six base sub-fields.
- Chapter [`08-delegation-to-ai-risk-engineer.md`](../08-delegation-to-ai-risk-engineer.md) for the ownership shape on engineering-side artefacts.
- Regulation (EU) 2024/1689 Articles 11, 12, 72 for retention floors and regulator-facing framing.
- Skim of the AI documentation-shape references in [`../resources.md`](../resources.md) (model cards, dataset datasheets, ATRS) — these are the artefact shapes the contract will name.

## Scenario

You continue as Northbrook's architect. Three specific controls are up for evidence-contract hardening this quarter:

- **Control A (governance-family)** — the training-data provenance entry from exercise-02 (call it `AIC-DAT-014`). The evidence contract is drafted but the six sub-fields are still the chapter 01 defaults; the level-15 analyst has flagged that they cannot collect against `retention_years: 10` without a specific archive-tier destination and cannot validate without a real schema URN.
- **Control B (engineering-side)** — the adversarial-robustness entry from chapter 08's worked example (`AIC-ENG-058`), which is *regulator-facing* (EU AI Act Article 15) and requires sampling across every tier-1 vision system.
- **Control C (third-party)** — a new `AIC-THI-071` covering *foundation-model provider notification obligations*: whenever the third-party foundation-model provider (assume a single, named provider abstractly — do not name a real vendor) changes model weights, alignment training, or system prompt, the provider must notify Northbrook within a bounded window with a specific evidence pack. The control has to sit inside Northbrook's library even though the *producer* of the evidence is external.

## Deliverables

Author three artefacts in a working directory of your choice:

1. **`evidence-contracts.yaml`** — the hardened evidence-contract blocks for all three controls.
2. **`evidence-schema-stubs.md`** — one paragraph per schema URN the contracts reference, following the shape from exercise-03.
3. **`ssp-evidence-bindings.yaml`** — three SSP-level `evidence_binding` blocks, one per control, for a specific Northbrook system in scope.
4. **`review-annotation.md`** — a short annotation showing each of the four architect review moves applied against each of the three contracts, with the specific push-back or acceptance decision.

## Requirements

### `evidence-contracts.yaml`

For each of the three controls, produce a fully-hardened `evidence_contract` block per chapter 07 with:

- **All six base sub-fields** — `artifact`, `schema_ref`, `producer_role`, `owner_role`, `cadence`, `retention_years`, `immutability`.
- **All three maturity sub-fields** — `producer_pipeline`, `sampling`, `regulator_facing`.
- Every enumerated value drawn from the standardised sets chapter 07 names (cadence tokens, immutability modes). Ad-hoc strings (`"every so often"`, `"as needed"`) fail the drill.
- **Producer role vs. owner role explicit and distinct** where they differ (chapter 07 is emphatic that these are two roles, not one).
- **Retention aligned to the longest applicable statute or enterprise baseline**, whichever is longer, with a comment naming the statute driving the value. For Control B (Article 15), for Control C (contract-driven + Article 25 provider obligations preview from mod-109 — mark uncertainty with `<!-- needs-research: … -->` where warranted).
- **Immutability chosen against artefact nature**, not against storage cost. For Control A's provenance record: `write-once-read-many`. For Control B's adversarial-robustness report: `append-only`. For Control C's third-party notification pack: pick and defend.
- **`schema_ref` URN pinned to a specific version**, appearing in `evidence-schema-stubs.md`. No dangling references.
- **`sampling` spelled out** for every control whose population is large (all provenance records; all tier-1 vision systems; potentially all provider change events). Chapter 07 says: without an explicit sampling shape, the analyst either collects everything, nothing, or a convenient subset — none defensible.

Contract fragments must be well-formed YAML the level-15 analyst could parse. Comment-driven schemas — the analyst reads the comments alongside the YAML — are encouraged but the six-plus-three sub-fields must be machine-readable.

### `evidence-schema-stubs.md`

For every `schema_ref` URN across the three contracts, one paragraph:

- URN with pinned version.
- What the schema describes at record level (one specific fact about one specific system at one specific time — chapter 07's definition).
- Producer (pipeline or role) and consumer (analyst tool / GRC-for-AI / regulator submission).
- Whether the schema exists today. If not, the schema-authoring task is on the same release as the control per chapter 07's "no dangling schema references" rule. State the target release.
- For third-party (Control C) schemas: whether the provider produces to *your* schema (preferred) or to their own (which you then have to validate against a translation profile). Chapter 07 treats this as a first-class contract question; do not paper over it.

### `ssp-evidence-bindings.yaml`

Per control, pick a specific Northbrook system and produce an SSP-level `evidence_binding` block per chapter 07's template shape. Suggested pairings (feel free to use your own if consistent):

- **Control A** → the customer-facing generative chat's fine-tune pipeline.
- **Control B** → the fraud-detection classifier fleet's flagship tier-1 model.
- **Control C** → the internal RAG assistant's foundation-model provider integration.

Each binding must include: `contract_ref` (pointing at the specific `evidence_contract` entry index on the control), `producer_pipeline_binding` (a URN naming the specific pipeline), `storage_location` (an evidence-archive URN), `retention_owner` (a role that resolves to a specific accountable body), `next_evidence_expected` (a real date consistent with the contract cadence), and `latest_evidence` (a stub artefact with a produced-at timestamp, an integrity hash, and a validation status).

### `review-annotation.md`

A short annotation, per control, showing the four architect review moves from chapter 07 applied against the drafted contract:

1. **The sampling question** — did the contract specify sampling? If no, what did you force back to the proposer?
2. **The immutability question** — is the immutability strong enough for the artefact's regulator-facing tier? If no, what did you push back on?
3. **The retention question** — is the retention aligned to the longest applicable statute? If the contract proposed weaker, what statute did you cite in the push-back?
4. **The schema question** — does the `schema_ref` resolve to a schema in the registry today? If not, what release did you couple the schema-authoring to?

Each move gets a one-sentence answer per control. Twelve moves total, twelve answers. If any move is answered "n/a," defend it — a real *n/a* is legitimate (Control A might not have a `regulator_facing: true` posture in every jurisdiction), but write the defence.

## Starter guidance

- Draft Control A's contract first; you can start from chapter 01's `AIC-DAT-014` fragment. The point of the hardening drill is to notice that the chapter 01 defaults leave the maturity sub-fields empty — the drill is what fills them.
- Control B (adversarial robustness) is where the *sampling* discipline bites hardest. "All tier-1 vision systems" is a legitimate sampling shape only if the tier-1 vision-system population is actually enumerable at the time evidence is due. Confirm the enumeration source.
- Control C (third-party notification) is where the producer / owner distinction is unavoidable. The producer is *the provider*; the owner (accountable that the notification exists) is *Northbrook governance analyst*; there is often a third party (the platform team) as an intermediate collector. Model all three roles.
- On the SSP binding artefacts, do not make up integrity hashes. Use placeholder `sha256:<placeholder>` with a comment naming what would be hashed — the point is to show the *shape*, not the content.
- On the review annotation, force yourself to write an explicit push-back for at least one move per control. If every move on every control comes back "yes, contract is fine," the drill is not working; the whole point is to find the weak fields.

## Acceptance criteria

- [ ] All three controls have contracts with all six base + three maturity sub-fields populated.
- [ ] No cadence, retention, or immutability field uses an ad-hoc string.
- [ ] Every retention value cites the statute or enterprise baseline driving it.
- [ ] Every `schema_ref` URN appears in `evidence-schema-stubs.md` at a pinned version.
- [ ] Producer role and owner role are explicit and distinct where they differ; identical only where the contract deliberately collapses them.
- [ ] Control C's contract addresses the *provider produces to whose schema* question explicitly.
- [ ] Three SSP evidence bindings produced, each with a specific pipeline URN, storage location, retention owner, and stubbed latest-evidence artefact.
- [ ] Review annotation contains an explicit answer to each of the four review moves for each of the three controls (12 total).
- [ ] At least one push-back per control is documented in the annotation.

## Stretch goals

- Extend Control A's contract with a *variant* for RAG-corpus provenance (data that is retrieved at inference time, not baked into weights). What changes in cadence, sampling, and immutability? Preview of the mod-108 evidence-schema library.
- Sketch the **evidence-freshness monitor** the level-15 analyst would run against Northbrook's `evidence_binding` blocks — a scheduled job that flags any binding whose `next_evidence_expected` has passed without a corresponding `latest_evidence` update. One paragraph of shape, not a script.
- Draft one paragraph of the *audit narrative* the head-of-AI-governance would send a regulator asking "how do you assure yourself the foundation-model provider is honouring the notification obligation?" The answer dereferences Control C's contract + binding + evidence-freshness monitor; write the paragraph that ties them together.
