# The evidence contract per control — hardened

## Why this chapter exists

Chapter 01 sketched the evidence contract as one of the seven library fields. Chapter 04 pointed at the parallel evidence-schema registry OSCAL references but does not own. This chapter goes deep on the contract itself: what an evidence artefact *is*, what an evidence *contract* commits to, why the six sub-fields introduced in chapter 01 (schema, producer role, owner role, cadence, retention, immutability) each earn their place, and how the contract fuses with the SSP and the assessment workflow at consumer time.

The evidence contract is the single most consequential field in the library. Everything downstream — the analyst's evidence collection at level 15, the risk engineer's automation at level 25, the evaluation engineer's assessment package at level 35, the AIMS Statement of Applicability at mod-105, the EU AI Act Article 11 technical documentation at mod-108, the regulator-facing submission at mod-110 — dereferences into evidence contracts. A weak evidence contract propagates weakness everywhere it is consumed; a well-shaped one lets every consumer pull ready-to-package artefacts without a follow-up conversation.

Mod-108 designs the *catalog* of evidence schemas that the contracts point at. This chapter designs the *contract shape* inside each control entry that ties a control to that catalog.

## What "evidence" means in this library — a working definition

An *evidence artefact* is a machine-readable, schema-validated, versioned, immutable object that demonstrates a specific fact about a specific system at a specific point in time. Read that definition carefully — each adjective is load-bearing:

- **Machine-readable** — a downstream tool can validate it, index it, aggregate it, and route it without human intervention.
- **Schema-validated** — its shape is defined by a named schema in the evidence-schema registry, at a pinned version.
- **Versioned** — the artefact carries a `schema_version`, and the schema itself carries a version history.
- **Immutable** — once produced, the artefact does not change. Corrections ship as new artefacts with `supersedes` pointers.
- **Specific fact / system / point in time** — no artefact aggregates across systems or across time. Aggregation happens in the reporting layer, from immutable base artefacts.
- **Demonstrates** — the artefact is not a claim ("we do X"); it is the *observation* that X was done ("here is the provenance record captured at 2026-07-03T09:14Z for training run TR-8842").

Everything the contract commits to derives from this definition. A "screenshot pasted into a Confluence page" is not evidence. A "Confluence page describing our process" is not evidence. A signed statement from a director that the process was followed is not evidence — it is *testimony*, which is a lower rung and belongs in the assessment record, not the evidence record.

## The six contract sub-fields — deep read

Chapter 01 introduced them. Here is what each is doing at contract level.

### `artifact` and `schema_ref`

`artifact` names the human-readable artefact filename (`dataset-provenance-record.json`). `schema_ref` is the pinned URN into the evidence-schema registry (`urn:enterprise:schema:dprov#1.4`). The URN is the load-bearing field:

- It says *which* schema validates the artefact.
- It pins to a *specific version* of that schema, so a schema evolution does not silently change what "valid" means.
- It is the pointer downstream automation resolves; the human-readable name is convenience only.

Failure mode when this is under-specified: an evidence set of `.json` files with no schema URN, downstream tooling cannot validate, and every consumer writes its own ad-hoc parser.

### `producer_role`

The role that *produces* the artefact. Named as a role, not a person, not a team. For engineering-side artefacts this is often `ai-risk-engineer` (level 25) or a platform component the risk engineer owns; for analyst-collected artefacts (survey attestations, third-party questionnaires) it is often `ai-governance-analyst` (level 15); for platform-emitted artefacts it is a service (`ENT-PLATFORM-REG-08`) with the platform team as the accountable owner.

Why "role, not person": every control persists across staff turnover. Naming a role keeps the contract intact through re-orgs, and lets the operating model (mod-112) resolve the role to the current owner.

### `owner_role`

The role *accountable for the artefact existing*, which is often not the producer. A platform emits inference-log artefacts continuously; the analyst is accountable that they *are* emitted, are retained, are collected into the evidence package. Two-role structure lets the contract distinguish "who makes the thing" from "who is on the hook if the thing is missing." Both matter and confusing them puts accountability in the wrong seat.

### `cadence`

How often the artefact is produced. Named values are cheap and worth standardising across the library:

- **`per-training-run`** — one artefact per training / fine-tuning event.
- **`per-release`** — one artefact per system release / deployment.
- **`continuous-window-<N>-minutes`** — one artefact per rolling N-minute window (typical for inference logs, drift metrics).
- **`daily` / `weekly` / `monthly` / `quarterly` / `annual`** — periodic.
- **`on-trigger:<trigger-name>`** — event-driven (e.g., `on-trigger:incident`, `on-trigger:vendor-recert`).
- **`pre-market` / `pre-release`** — one-time before market/release, replaced only on re-market or re-release.

The library catalog preface publishes the enumerated set. Ad-hoc cadence strings ("every so often," "as needed") die at consumer time.

### `retention_years`

How long the artefact is kept, in years. This is the field where regulatory obligation lives — set it to the *longest* applicable statute or the enterprise's baseline retention, whichever is longer. Concrete examples:

- EU AI Act Article 12 requires providers of high-risk systems to keep automatically generated logs for an appropriate period, in any case not less than six months, unless national law provides otherwise. <!-- needs-research: verify current Article 12 minimum-retention text against the consolidated Regulation 2024/1689. -->
- ISO/IEC 42001 requires retention of documented information appropriate to the AIMS scope; the specific years is not fixed in the standard.
- SR 11-7 evidence for tier-1 US G-SIB model risk management is typically retained for the life of the model plus a defined post-retirement period.
- Sector-specific rules (HIPAA, GDPR, PCI, FDA GxP) can extend the applicable retention far beyond the AI-specific minimum.

The evidence-contract retention field is *not* the place to negotiate the retention down; it is the place to *record* the longest applicable requirement. If the enterprise's retention infrastructure cannot support the required retention, that gap goes on the enterprise POA&M — the contract stays at the required value.

### `immutability`

One of `append-only`, `write-once-read-many`, `versioned-with-supersedes`, `mutable-with-audit-log`. Different artefacts warrant different treatments; the field forces the contract to say which.

- **`append-only`** — inference logs, audit logs, incident timelines. New records only append; existing records never mutate. Tier-1 evidence for regulator-facing purposes almost always demands append-only.
- **`write-once-read-many`** — model artefacts (weights, tokenizer, config), dataset provenance records at ingest. Once written, immutable; changes ship as new artefact with new `supersedes` pointer.
- **`versioned-with-supersedes`** — model cards, system cards, DPIAs / AIAs. Human-authored artefacts that legitimately revise; each revision is a new artefact, prior versions retained.
- **`mutable-with-audit-log`** — reserved for low-tier evidence where in-place edits carry an accompanying edit log; typically inadequate for tier-1.

Choose the strongest mode the artefact tolerates. Storage cost is not a valid reason to weaken immutability; a mis-classified immutability field is a finding at every audit.

## Extending the contract — three additional sub-fields the mature library carries

The six sub-fields cover the basics. Three more earn their place as the library matures.

### `producer_pipeline`

A pointer to the system / pipeline / job that emits the artefact, in URN form (`urn:enterprise:pipeline:provenance-capture-v2`). Two purposes: it lets an assessor go from evidence back to the mechanism that produced it (defensibility), and it lets the platform team track which downstream controls depend on the pipeline (change-management).

### `sampling`

For controls that touch large populations (all training runs, all inference requests, all customer prompts), the contract states the sampling shape: `random-N-per-quarter`, `all-tier-1`, `stratified-by-jurisdiction`, `boundary-cases-per-release`. Without an explicit sampling shape, the analyst either collects everything (unusable), collects nothing (undefensible), or collects a convenient subset (unrepeatable).

### `regulator_facing`

Boolean or enum indicating whether the artefact is *directly* consumed by a regulator. Tier-1 for artefacts named in EU AI Act Article 11 (technical documentation), Article 12 (logs), Article 72 (post-market monitoring) or in SR 11-7 model documentation. Tier-1 evidence gets stricter immutability, stricter retention, and priority in the evidence-archive tiering (mod-108). Explicitly naming it lets the platform team know where to invest.

## Fusion with the SSP — how the contract lives at consumer time

The catalog's contract is the *shape*. The SSP is where a specific system fills the shape in for its instance. The template shape:

```yaml
evidence_binding:
  control_id: AIC-DAT-014
  contract_ref: AIC-DAT-014.evidence_contract[0]  # dataset-provenance-record.json
  producer_pipeline_binding: urn:acme:pipeline:mrp-provenance-capture-v3.2
  storage_location: urn:acme:evidence-archive:tier-1/mrp/provenance
  retention_owner: bu-risk-officer
  next_evidence_expected: 2026-08-15T00:00:00Z
  latest_evidence:
    artifact: dprov-20260701-tr8842.json
    produced_at: 2026-07-01T09:14:12Z
    integrity_hash: sha256:8a...
    validation_status: passed
```

The binding is what the analyst tracks. `next_evidence_expected` becomes a scheduled tracker entry; `validation_status` fails when a schema-validation regression happens; `retention_owner` is the person paged when the retention infra flags a nearing-expiry.

Two design choices:

**Explicit binding, not implicit.** Every control × system pair has an explicit `evidence_binding`. "It's produced somewhere" without a resolvable location is not a binding. Analysts do not go hunting.

**Latest-evidence pointer.** The binding carries a pointer to the most recent artefact plus the produced-at timestamp. Downstream reporting and audit-pack assembly read the pointer; they never search the archive.

## The evidence-contract review discipline

Because the contract is the API to every downstream consumer, the architect's review discipline at chapter 06's stage-2 review is unusually strict on this field. Four review moves.

**Force the sampling question.** For any control whose population is large, if the contract has no `sampling` sub-field, the review is not done.

**Force the immutability question.** If the immutability is `mutable-with-audit-log` on a control that touches regulator-facing evidence, the review is not done. Push back to the proposer.

**Force the retention question.** If the retention is a placeholder (`retention_years: TBD`) or a "we'll pick this later," the review is not done. The architect knows the longest applicable statute at this scope; write it in.

**Force the schema question.** If the `schema_ref` points at a schema that does not exist in the evidence-schema registry yet, either the schema needs to be authored first (block the control) or the schema needs to be added to the same release (couple them). No dangling schema references.

These four moves are the difference between an evidence contract that survives a real audit and one that unravels the first time it is exercised.

## How the contract composes with assurance and post-market surveillance

The evidence contract is the *base layer*. Two higher layers consume it directly.

- **Assurance** (mod-107) — the pre-deployment gate consumes contracts to know which artefacts must be present, at what freshness, to pass release. The ongoing assurance programme (three-lines-of-defense second line) audits that contracts are being honoured. The third-line independent audit uses the contract as its documented control-test criteria.
- **Post-market surveillance** (mod-110) — continuous monitoring feeds new evidence artefacts into the contract's binding; the monitoring-to-risk-register wiring reads validation status and flags deviations.

The contract is upstream of both. If the contract is wrong, both are wrong. If the contract is right, both have a stable base to build on.

## Two common shape mistakes

**Mistake 1 — Prose evidence descriptions.** "We keep records of training data" is not an evidence contract. It is prose. It cannot be validated, cannot be scheduled, cannot be pinned to a schema, cannot be tracked. Force every evidence sentence into the six sub-fields; if the proposer cannot answer six questions, the control is not ready.

**Mistake 2 — One giant catch-all evidence artefact per control.** A `system-audit-package.zip` that "contains everything about the system" is not evidence; it is a shipping container. Break it into named artefacts with individual schemas. Package for shipping only at the assurance layer (mod-107), never at the base evidence layer.

## Summary

The evidence contract is the API between the control library and every downstream consumer of assurance, assessment, audit-pack assembly, and post-market surveillance. Its six base sub-fields (schema reference, producer role, owner role, cadence, retention years, immutability) plus three maturity sub-fields (producer pipeline, sampling, regulator-facing) commit the library to *machine-readable, schema-validated, versioned, immutable, specific-fact* artefacts — not to prose descriptions, not to testimony, not to shipping containers. At consumer time, the contract fuses with the SSP via an explicit evidence binding that the analyst tracks. The architect's review discipline on this field is strict for a reason: a weak contract propagates weakness into every downstream consumer, forever.
