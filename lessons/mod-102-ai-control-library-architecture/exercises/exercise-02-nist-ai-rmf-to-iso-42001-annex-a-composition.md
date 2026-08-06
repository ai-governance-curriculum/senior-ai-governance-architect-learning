# exercise-02: NIST AI RMF → ISO/IEC 42001 Annex A → EU AI Act Composition

**Estimated effort:** 3 hours

## Objective

Practise the *one entry per outcome, multi-parented via crosswalk* composition move from chapter 02. You will take four outcomes that show up in all three governance-family sources — NIST AI RMF, ISO/IEC 42001 Annex A, EU AI Act Articles 9-15 — and produce single library entries that satisfy all three sources without duplication, without paraphrase, and without silent conflation.

Then you will refactor a deliberately broken *parallel-libraries* artefact so it collapses onto your composed catalog. The refactor is the point of the exercise: composing from scratch is easier than inheriting the failure mode most enterprises actually have.

## Prerequisites

- Chapter [`02-composing-nist-rmf-iso-42001-eu-ai-act.md`](../02-composing-nist-rmf-iso-42001-eu-ai-act.md) read once.
- Chapter [`01-anatomy-of-a-control-library-entry.md`](../01-anatomy-of-a-control-library-entry.md) — you will re-use the seven-field shape.
- NIST AI RMF 1.0 (AI 100-1) and the Generative AI Profile (AI 600-1), open in a tab.
- ISO/IEC 42001:2023 Annex A — Annex A titles at minimum. If you do not have the paywalled standard, use the ISO information page plus reputable secondary summaries and mark uncertain mappings with `<!-- needs-research: ... -->`.
- Regulation (EU) 2024/1689 — the EU AI Act canonical text.

## Scenario

You continue as the Northbrook Financial architect from exercise-01. The head-of-AI-governance has ratified the shape proposal and asked you to compose the first four cross-source entries so the pattern is visible before you scale it across the catalog.

Legal has also handed you a *parallel-libraries* artefact left by the previous consultant: `north-bridge-controls.xlsx` — three sheets, one per source (NIST AI RMF, ISO/IEC 42001, EU AI Act), each with drifted restatements of the same outcomes. You have been asked to fold it into your composed catalog and quantify the reduction in entry count.

## Deliverables

Author three artefacts in a working directory of your choice:

1. **`composed-entries.yaml`** — four fully-composed library entries.
2. **`composition-worksheet.md`** — the per-outcome composition working: which sub-categories, Annex A controls, and articles land in each entry, and why.
3. **`parallel-libraries-refactor.md`** — the refactor artefact: what folded into what, entry-count reduction, and the follow-up backlog.

## Requirements

### Outcomes to compose

Compose one library entry per outcome for **all four** of the following. The parent-source hints are starting points; the composition working is yours to defend.

| # | Outcome | Governance-family parents (hints) |
|---|---|---|
| 1 | **Training-data provenance recorded and verifiable for every in-scope training or fine-tuning event.** | NIST MAP-2.3, MEASURE-2.8; ISO/IEC 42001 Annex A clause on data for AI systems; EU AI Act Article 10. |
| 2 | **Human oversight assignment for every tier-1 or tier-2 system, with the assigned person's authority and escalation path recorded.** | NIST GOVERN-2, MANAGE-3.2; ISO/IEC 42001 Annex A clause on organizational roles / responsibilities; EU AI Act Article 14 and Article 26. |
| 3 | **Automatic operational logs captured, retained, and integrity-protected for high-risk deployments.** | NIST MEASURE-2.7, MEASURE-2.10; ISO/IEC 42001 Annex A clause on information for interested parties / documented information; EU AI Act Article 12. |
| 4 | **Post-deployment monitoring produces drift, robustness, and incident signals that route into the AI risk register on a defined cadence.** | NIST MEASURE-4, MANAGE-2, MANAGE-4; ISO/IEC 42001 Annex A clause on AI system lifecycle / monitoring; EU AI Act Article 72; and — as a preview of chapter 03 — MITRE ATLAS mitigations. |

### `composed-entries.yaml`

For each outcome, one entry with all seven fields from chapter 01, plus:

- Applicability filter that decides *option A vs. option B* per chapter 02 (one high bar globally vs. profile-per-jurisdiction). State which option you chose per entry in the entry's `notes` block.
- Crosswalk block that names *every* parent source you claim, at the sub-category / Annex-A-control / article grain — no rolled-up "NIST" or "ISO" claims.
- No verbatim quotes from any source in the `statement` field. If you find yourself quoting, rewrite as the enterprise's outcome.
- One-sentence defence per crosswalk edge, in a `crosswalk_notes` block — "This control satisfies A.<x> because …" — chapter 02 says you should be able to defend each edge in one sentence to an auditor.

### `composition-worksheet.md`

The working shown. Per outcome:

- The three-column source table: NIST sub-categories | ISO Annex A controls | EU AI Act articles you claim as parents.
- The *decision to mint one entry vs. split into 2-3*. Chapter 02 says the SoA driver is Annex A, and one Annex A control can be satisfied by 1-N enterprise controls. Justify your choice per outcome.
- The applicability-filter option A vs. B decision, with the trade-off named (over-commitment vs. jurisdictional strictness).
- One paragraph per outcome on *what you rejected as an incorrect parent*. A parent you considered and consciously rejected is more evidence of understanding than a parent you claimed defensibly.

### `parallel-libraries-refactor.md`

Assume `north-bridge-controls.xlsx` has 60 rows split across three sheets — 20 NIST-flavoured, 25 ISO-flavoured, 15 EU-AI-Act-flavoured — of which roughly 40 outcomes are duplicated across at least two sheets. Produce:

- The **collapse map** for the four outcomes above: which of the 60 rows fold into each of your four entries, and which of them are dropped as pure paraphrases.
- The **entry-count reduction estimate** for the whole 60-row spreadsheet, extrapolated from the four worked outcomes with the assumption stated.
- The **residual backlog** — rows that do *not* fold into the four outcomes and either need their own composed entry, or are legitimately in a different family (evidence, third-party, transparency) and should be routed to a future pass.
- A **one-paragraph audit-narrative** you would send to the ISO/IEC 42001 lead auditor explaining why the SoA that used to point at 60 spreadsheet rows now points at ~20 catalog entries, and how the two-way traceability still works. The paragraph is where chapter 02's "reads through three lenses" claim is either credible or not.

## Starter guidance

- Draft outcome #1 (training-data provenance) first; you can adapt chapter 01's worked `AIC-DAT-014` example. The point is that composing means *one entry*, so if your version drifts from the worked example, decide whether the drift is warranted or an inconsistency to reconcile.
- For outcome #2, notice that Article 14 (human oversight) is a *provider* obligation and Article 26 (deployer obligations for high-risk systems) is a *deployer* obligation. Northbrook is often both. Compose so a deployer-only SoA can still pull the entry.
- For outcome #3, EU AI Act Article 12 sets a minimum retention *floor* the enterprise must not undercut in the evidence contract. Do not weaken retention because the current archive tier is expensive — that trade-off is a POA&M entry, not a catalog decision. See chapter 07.
- For outcome #4, MITRE ATLAS mitigation crosswalks are a *preview* of chapter 03. Do not populate the OWASP / SAIF / CISA / ENISA edges yet — you will layer those in exercise-03 territory. But do add the MITRE edge if you can defend it.
- On the refactor: do not silently drop rows without recording *why*. A dropped row that later turns out to have named a real outcome you missed is a governance embarrassment; a dropped row logged as "paraphrase of Annex A.7 already covered by AIC-DAT-014" is defensible.

## Acceptance criteria

- [ ] Four composed entries produced, each with all seven fields plus `notes` and `crosswalk_notes`.
- [ ] Every entry names ≥ 1 NIST AI RMF sub-category, ≥ 1 ISO/IEC 42001 Annex A control, and ≥ 1 EU AI Act article in its crosswalk.
- [ ] No entry statement contains a verbatim quote from a source.
- [ ] Each crosswalk edge is defended in one sentence in `crosswalk_notes`.
- [ ] Each entry's applicability filter is annotated with option A vs. option B and the trade-off named.
- [ ] The composition worksheet names at least one *rejected* parent per outcome, with justification.
- [ ] The refactor artefact contains a collapse map for all four outcomes, an extrapolated entry-count reduction, a residual backlog, and the auditor-narrative paragraph.

## Stretch goals

- Add an *EU-tier1-high-risk* profile fragment (OSCAL-shape sketch is fine; full drill lives in exercise-03) that tailors outcome #1 with the Article-10 stricter data-quality parameters. Preview of chapter 04.
- Take the *Generative AI Profile* (NIST AI 600-1) and check whether it adds *superset* applicability filters to any of your four entries per the "superset, not fork" rule. Note the additions in-line.
- Extend the refactor artefact with a *deprecation plan* for the spreadsheet itself — who is told, when, and what the migration guidance says for the teams that were consuming it. Preview of chapter 06.
