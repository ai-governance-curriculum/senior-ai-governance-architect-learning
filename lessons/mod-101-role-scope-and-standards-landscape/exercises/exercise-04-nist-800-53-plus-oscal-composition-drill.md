# exercise-04: NIST SP 800-53 + OSCAL Composition Drill

**Estimated effort:** 3 hours

## Objective

Practise the three composition patterns — **Extend**, **Inherit-and-augment**, **Add** — from chapter 04 by taking four AI-specific risks and producing OSCAL-shaped control entries that compose against NIST SP 800-53 rev5 parents (or explicitly declare no parent). This is the tightest technical drill in the module; it is also the exercise that will most immediately transfer into mod-102 (control-library architecture).

## Prerequisites

- Chapter [`04-nist-800-53-oscal-and-csf.md`](../04-nist-800-53-oscal-and-csf.md) read once.
- Access to NIST SP 800-53 rev5: <https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final>.
- OSCAL model reference: <https://pages.nist.gov/OSCAL/reference/latest/> — you do not need to be fluent, but you should know where to look up the JSON / YAML schema for a `control`, a `profile`, and a `component-definition`.
- Skim access to NIST SP 800-53 rev5 in OSCAL: <https://github.com/usnistgov/oscal-content> (the `nist.gov/SP800-53/rev5` catalog).
- Chapter [`03-iso-42001-family-and-vocabulary-layer.md`](../03-iso-42001-family-and-vocabulary-layer.md) for crosswalk targets.

## Scenario

You are architecting the AI control library for Northbrook Financial (the scenario from exercise-02). You have OSCAL-native tooling in place. For each of the four risks below, produce the correct composition against SP 800-53 rev5, in the correct OSCAL shape, with the correct crosswalk.

## Risks to compose against

Compose control entries for **all four** of the following:

| # | AI-specific risk | Hint on likely pattern |
|---|---|---|
| 1 | **Access to training data and inference results by insiders** — misuse and exfiltration of sensitive data during AI system operation. | Likely Extend — parents in AC / AU / PT families are near-complete. |
| 2 | **Model drift in a deployed classifier as production traffic evolves.** | Likely Inherit-and-augment — parent in SI-4 (information system monitoring) sits at the right family but coarser granularity. |
| 3 | **Prompt injection into an agentic tool-use assistant that has access to internal systems.** | Likely Add — no clean SP 800-53 parent. |
| 4 | **Third-party foundation-model provider changes model weights without notice and without providing an evaluation-access window.** | Likely Inherit-and-augment — parents in SA / SR families cover software-supply-chain third-party at the wrong shape. |

## Deliverables

Produce a directory containing:

1. **`aic-controls.json`** or **`aic-controls.yaml`** — the four AI-library control entries in OSCAL `control` shape.
2. **`aic-profile.json`** or **`aic-profile.yaml`** — a minimal OSCAL `profile` that pulls the four AI-library controls plus the SP 800-53 parents they extend / relate to.
3. **`composition-notes.md`** — narrative notes covering the composition decisions.

## Requirements

### `aic-controls.json` / `.yaml`

For each of the four risks, produce a `control` entry with:

- **`id`** in `aic-<family>-<seq>` format (e.g. `aic-si-101`).
- **`class`** set to `AIC`.
- **`title`** — a short human-readable title.
- **`props`** at minimum for `system_tier`, `system_kind`, `jurisdiction`. Use realistic values for the scenario.
- **`parts`** for `statement`, `guidance`, and `assessment` (drafting a testing procedure at the level chapter 04 describes).
- **`links`** for at least: any SP 800-53 rev5 parent(s) (with `rel: related`), NIST AI RMF sub-category (with `rel: reference`), ISO/IEC 42001 Annex A control (with `rel: reference`), CSF 2.0 subcategory (with `rel: reference`). Where an EU AI Act article applies, include it.
- A comment or annotation naming the composition pattern (Extend / Inherit-and-augment / Add) for that entry.

The four entries in aggregate must exercise **all three** composition patterns. If your four all come out Extend, revise — you have missed the point of the drill.

### `aic-profile.json` / `.yaml`

- A `profile` entry that `imports` the SP 800-53 rev5 catalog and the AIC controls file.
- `include-controls` calling out both the SP 800-53 parents referenced above and the four AIC entries.
- A parameter override on at least one SP 800-53 control (e.g. tightening AU-11 audit-record retention to a value defensible under EU AI Act evidence-retention obligations).
- A minimal `metadata` block with `title`, `version`, and a party who is responsible.

You do not need to produce a full working OSCAL bundle. Well-formed JSON / YAML that a reader who knows OSCAL can validate on inspection is sufficient. If you produce actual validating OSCAL, note the tool you used.

### `composition-notes.md`

- For each of the four risks: which pattern you chose, why, and what the alternative pattern would have looked like.
- The applicability filter values you chose for each entry, and *why those and not others*.
- Any place where you had to invent a new control family prefix under `AIC-` and the rationale.
- One paragraph on **what breaks if we forget to fill in the CSF 2.0 subcategory crosswalk** — testing that you understand chapter 04's audience-lens argument.

## Starter guidance

- Start by writing the `composition-notes.md` outline (one paragraph per risk) *before* writing OSCAL. If you cannot explain the composition in plain English, the OSCAL will be wrong.
- For risk 1, verify SP 800-53 rev5 parent IDs by looking them up in the catalog itself — do not guess AC-6, PT-2 IDs. Wrong IDs invalidate the composition.
- For risk 3, resist the urge to shoehorn a parent. Chapter 04 says Add is a legitimate pattern; use it.
- For risk 4, look at SR (Supply Chain Risk Management) family controls in SP 800-53 rev5. That is where the composition seam is, even though the risk feels engineering-heavy.
- Use the OSCAL model reference page as your validation for what fields exist. Do not invent OSCAL fields.

## Acceptance criteria

- [ ] Four `control` entries produced in OSCAL JSON or YAML.
- [ ] The four entries collectively exercise all three composition patterns (Extend, Inherit-and-augment, Add).
- [ ] Every entry has `id`, `class: AIC`, `title`, `props`, `parts` (`statement`, `guidance`, `assessment`), and `links`.
- [ ] Every entry links to at least one NIST AI RMF sub-category and one ISO/IEC 42001 Annex A control, both verified against primary source.
- [ ] Every entry has a CSF 2.0 subcategory crosswalk.
- [ ] Parent SP 800-53 rev5 control IDs (when cited) are verified against the catalog, not paraphrased.
- [ ] The profile imports the SP 800-53 catalog and the AIC controls, includes both, and has at least one parameter override.
- [ ] Composition notes explain each pattern choice, the applicability filter choices, and the CSF-crosswalk audience argument.

## Stretch goals

- Produce a `component-definition` (OSCAL) for a hypothetical **guardrail service** that implements one AIC control and one SP 800-53 parent — sketching how the control-library entries land against a real component.
- Add a `poam-item` (OSCAL) for a *deliberately unresolved* gap where the risk is real but the enterprise cannot implement the control within the current quarter, and note what the appetite deviation would look like.
- Extend the profile to include an EU-specific variant that overrides one parameter (e.g. incident-notification cadence) to satisfy EU AI Act Article 73 — a preview of mod-104 (cross-jurisdiction reconciliation).
