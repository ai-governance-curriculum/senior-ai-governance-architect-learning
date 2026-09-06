# exercise-01: Policy Hierarchy Authoring Drill

**Estimated effort:** 3 hours

## Objective

Author a **five-layer taxonomy slice** end-to-end for one AI-governance topic — a Responsible AI principle at the top, a binding-policy clause under it, a standard under that, a procedure under that, and a work instruction at the leaf — so the pattern from chapter [`01-ai-policy-hierarchy-authority-and-cadence.md`](../01-ai-policy-hierarchy-authority-and-cadence.md)'s "Worked scenario — human oversight, all the way down" is instantiated on a fresh topic *you* choose. The drill is where you feel the verb-strength gradient in your fingers rather than just recognising it on a page — collapse two layers together and the audit trail from mod-102 chapter 08 (delegation) will not compose downstream in mod-105 (AIMS documented information) or mod-108 (evidence).

## Prerequisites

- Chapter [`01-ai-policy-hierarchy-authority-and-cadence.md`](../01-ai-policy-hierarchy-authority-and-cadence.md) read once, with the comparison table absorbed.
- Skim of the mod-102 catalog anatomy in [`../../mod-102-ai-control-library-architecture/01-anatomy-of-a-control-library-entry.md`](../../mod-102-ai-control-library-architecture/01-anatomy-of-a-control-library-entry.md) so the leaf-side control ID you cite matches the library shape.
- The Northbrook Financial scenario carried through the track (US regional bank, SR 11-7 MRM, ~40 AI systems, Colorado + NYC deployments, EU expansion in-flight).

## Scenario

You are the level-50 architect at Northbrook. The head-of-AI-governance (level 60) has asked you to produce a full taxonomy slice for *one* topic as a reference exemplar the two ai-governance-analysts (level 15) will use as a shape template when they take over authoring for the rest of the standards catalogue over the next two quarters. Pick **one** topic — the exemplar is judged on layering discipline, not topic breadth:

- AI-generated customer communications (GenAI-authored outbound emails, chat suggestions).
- Third-party AI risk-tiering (how a vendor-integrated AI SaaS is classified against the internal tiering scheme).
- Use of generative AI on regulated model outputs (SR 11-7 model output subsequently altered or summarised by GenAI).
- Automated adverse-action decisioning (fraud declines, application declines).

Pick the topic before you write anything else. The topic drives the modality catalogue at layer 3 and the tool names at layer 4/5; picking mid-draft guarantees the layers drift.

## Deliverables

Author three artefacts in a working directory of your choice:

1. **`taxonomy-slice.md`** — the five layered artefacts, each in its own H2 section, top to bottom.
2. **`layer-audit.md`** — the self-audit table.
3. **`taxonomy-review-note.md`** — the one-page defence brief for the AI Governance Council.

## Requirements

### `taxonomy-slice.md`

Five sections, one per layer. Each section carries: the artefact text itself, a stable ID, the authority body, the verb strength observed, the review cadence (with change triggers, not calendar-only), and the exception path.

- **Layer 1 — principle.** 4–8 lines. Declarative present tense; no `shall` / `must`. Anchor to at least one characteristic in each of the three source frames (OECD AI Principles; UNESCO Recommendation 2021; NIST AI RMF trustworthy characteristics) — cite each explicitly. Board or Board AI Committee is the ratifier. Exception path: `None`.
- **Layer 2 — binding-policy clause.** `POL-<topic>-<n>` ID. `shall` verb only. 5–15 lines. Names the outcome the organisation commits to and delegates the modality catalogue to the standard below. States that exceptions require Board or ExCo waiver, are time-boxed, and register in the risk register (forward reference to mod-106).
- **Layer 3 — standard.** `STD-<topic>-<n>` ID. `must` verb only. Enumerates the modalities that satisfy the policy clause (like the M1/M2/M3 catalogue in chapter 01's worked scenario), and the tier-to-modality mapping. Ratified by the AI Governance Council. Exception path: documented waiver at AIGC level (forward reference to chapter [`04-exception-and-waiver-workflow.md`](../04-exception-and-waiver-workflow.md)).
- **Layer 4 — procedure.** `P-<topic>-<n>` ID. Imperative voice. Names the executing role (ai-governance-analyst for governance workflows, or an engineering role for engineering-heavy work), the input, the output, the tool it consumes. Points at 2–3 work instructions for the fiddly steps. Exception path: `Change request against the procedure`.
- **Layer 5 — work instruction.** `WI-<topic>-<n>` ID. Screen-by-screen or command-by-command imperative. Tool name, exact field names, exact command flags. Owned by the executing team. Exception path: `Defect report`.

### `layer-audit.md`

A single table with five rows (one per layer) and columns for: layer, ID, verb forms actually used (list every `shall` / `must` / imperative construction observed), authority body, review cadence (`<months> months + <triggers>`), exception path, and "at altitude?" (yes/no with one-sentence rationale). Add a short prose section beneath the table that names the two shape mistakes from chapter 01 (collapsing standards into policy; `shall` everywhere or calendar-only review) and states one concrete check you ran against your slice to prove you did not commit them.

### `taxonomy-review-note.md`

One page. Must contain:

- The topic and why the head-of-AI-governance should approve the slice as the reference exemplar.
- The *two decisions* an AI Governance Council reviewer is most likely to push back on (e.g., "why is the modality catalogue in a standard rather than in the policy?" or "why does the procedure not name the runtime PDP?") and the one-paragraph counter-argument for each.
- One *open question* you need the head-of-AI-governance to resolve before publication — there is always at least one; surface it.

## Starter guidance

- Draft top-down. If layer 1 is not stable, layers 2–5 will drift on every re-read of the principle.
- Read your layer 1 aloud with the word `shall` inserted; if it still reads like a principle, you have written a policy clause and mis-labelled it.
- The moment your standard names a tool or a button, it has become a procedure. Move the tool reference down and abstract the standard back up to modalities and preconditions.
- Cadence is *calendar + triggers*. A row that reads "annual" and nothing else fails the audit.
- Forward references to chapter [`02-principle-to-policy-to-standard-to-control-traceability.md`](../02-principle-to-policy-to-standard-to-control-traceability.md) matter: your slice will be walked as a traceability chain later. Make the IDs stable now so the mapping matrix in exercise-02 can point at them.

## Acceptance criteria

- [ ] All five layers present, each in its own H2 section, each with a stable ID (`POL-*`, `STD-*`, `P-*`, `WI-*`) and the layer-1 principle carrying an ID like `RAI-<n>`.
- [ ] No principle text uses `shall`; no policy clause uses `must`; no standard uses imperative work-instruction voice; no procedure or work instruction uses `shall`.
- [ ] Each layer names the authority body, the verb strength, the review cadence (calendar plus at least one named trigger), and the exception path.
- [ ] The principle cites at least one anchor characteristic from each of OECD / UNESCO / NIST AI RMF.
- [ ] The binding-policy clause delegates the modality catalogue to the standard rather than enumerating modalities in policy.
- [ ] The standard enumerates modalities with preconditions and a tier-to-modality mapping.
- [ ] The procedure names an executing role and points at at least one work instruction.
- [ ] The work instruction is screen- or command-level concrete and names the exact tool.
- [ ] `layer-audit.md` marks every row `at altitude? yes` with a defensible rationale; if any row is `no`, the taxonomy slice has been edited to fix it before submission.
- [ ] `layer-audit.md` names both chapter 01 shape mistakes and the concrete check that showed the slice did not commit them.
- [ ] `taxonomy-review-note.md` fits on one page, names two consequential decisions plus their counter-arguments, and surfaces at least one open question.

## Stretch goals

- Sketch the review-triggers table naming the change events (framework revision, regulator publication, incident retrospective, tooling change) that force out-of-cycle review at each layer.
- Map each authored layer to at least one mod-102 control ID (real or `AIC-<family>-<planned>`) so the slice composes into the control-library shape from mod-102 chapter 01.
- Draft the routing list you would use if the *standard* drafted here needed a major revision — forward reference to chapter [`05-policy-change-communications-and-deprecation-windows.md`](../05-policy-change-communications-and-deprecation-windows.md); name the roles marked "major" in that chapter's routing-list table.
- Propose the `principle_ids` back-reference the leaf control's `crosswalk` field would carry, per chapter [`02-principle-to-policy-to-standard-to-control-traceability.md`](../02-principle-to-policy-to-standard-to-control-traceability.md). One line is enough.
