# Composing NIST AI RMF, ISO/IEC 42001 Annex A, and EU AI Act Articles 9–15

## Why this chapter exists

Three governance-family sources — NIST AI RMF, ISO/IEC 42001 Annex A, and the operative articles of the EU AI Act — carry between 70% and 80% of the substance in a mature AI control library. Every architect starts here whether they meant to or not: the moment an executive asks "what does 'align to NIST AI RMF' actually mean in our program?", the answer is a set of control entries. The moment legal reads Article 9 of the AI Act, the request that lands on your desk is "how do we implement Article 9?" — the answer is again a set of control entries.

Chapter 01 gave you the *shape* of an entry. This chapter shows you how the three governance sources land inside that shape *without* producing three duplicated libraries. The right composition move is one entry per outcome, with crosswalks to every source that references that outcome. The wrong move — three parallel libraries, one per source — is the dominant failure mode among enterprises that stood up governance in a rush and never refactored.

Chapter 03 does the same exercise for the threat-facing sources (OWASP LLM Top 10, MITRE ATLAS, Google SAIF, CISA/NCSC, ENISA); mod-104 handles the multi-jurisdiction extension of what this chapter starts.

## The three sources — what each brings

Each source has a different *grain* and a different *speech act*.

| Source | Grain | Speech act |
|---|---|---|
| NIST AI RMF 1.0 (+ Generative AI Profile) | Sub-categories inside four functions (GOVERN / MAP / MEASURE / MANAGE) | *Outcome statement* — "policies…are in place." |
| ISO/IEC 42001 Annex A | Annexed control objectives + individual controls organised by clause | *Control objective + control* — "The organization shall…" |
| EU AI Act Articles 9–15 | Legal obligations for providers and deployers of high-risk AI systems | *Legal duty* — "Providers of high-risk AI systems shall establish…" |

Composing them is *not* a matter of picking one to be canonical and translating the others into it. Each is an authoritative expression in its own genre and each will be quoted, verbatim, by different audiences: NIST language by security and CIO organisations, ISO language by ISMS/AIMS auditors, EU AI Act language by legal and the regulator. Your library entries have to be readable through all three lenses without paraphrasing any of them.

## The core move: one control entry per outcome, multi-parented

Any risk mitigable at all can be phrased as an *outcome* — "training data provenance is recorded and verifiable" — and every one of the three sources will speak to that outcome from its own angle:

- NIST AI RMF MAP-2.3 wants the *organisation to understand* system context, including data.
- ISO/IEC 42001 Annex A.7 wants the *data lifecycle* to be governed with documented information.
- EU AI Act Article 10 imposes *legal obligations* on training / validation / testing data for high-risk systems.

The composition move: a single AI-library entry (e.g., `AIC-DAT-014` from chapter 01) whose statement expresses the outcome once, whose applicability filter says who it applies to (all tier-1/2 AI systems, all jurisdictions with EU deployments filtered on), whose evidence contract is the same regardless of which source is asking, and whose *crosswalk* names all three sources:

```yaml
crosswalk:
  nist_ai_rmf: [MAP-2.3, MEASURE-2.8]
  iso_iec_42001_annex_a: [A.7.2, A.7.3]
  eu_ai_act: [Article 10]
```

Now the ISO auditor reading the SoA finds A.7.2 traced to `AIC-DAT-014` with its evidence contract; the regulator reading an EU AI Act Article 11 technical documentation packet finds Article 10 traced to `AIC-DAT-014` with the same evidence; the internal CIO briefing pulls the NIST AI RMF MAP-2.3 posture and sees `AIC-DAT-014`. One evidence set, three audiences, one control.

## Reading NIST AI RMF into control entries

Chapter 02 of mod-101 already established that NIST AI RMF sub-categories become control entries in the library through a one-to-many mapping. Mod-102 adds the composition discipline: when the sub-category composes with an ISO/IEC 42001 or EU AI Act obligation, the *same* control entry carries all three references rather than a fresh entry per source.

The pattern is worth naming with an example. NIST AI RMF GOVERN-2.1 (roles / responsibilities documented and communicated). Three composition candidates from the other sources:

- ISO/IEC 42001 Annex A.3 (organizational roles, responsibilities, and authorities).
- ISO/IEC 42001 Annex A.4 (resources for the AI management system).
- EU AI Act Article 26 (obligations of deployers) — which requires the deployer to designate a natural person for human oversight.

You do *not* draft three controls for GOVERN-2.1, one for A.3, one for A.4, and one for Article 26. You draft one to three enterprise controls that satisfy the underlying outcome (e.g. `AIC-GOV-001` AI RACI Register; `AIC-GOV-002` Human Oversight Assignment; `AIC-GOV-003` AI Governance Council Charter) and each carries a crosswalk that names *all* the parent sub-categories, Annex A entries, and articles it participates in.

When you write a new AI-library entry, run this check: for each field in the crosswalk, can you defend the mapping in one sentence to an auditor? "This control satisfies A.3 because it names the accountable role for every AI system." "This control satisfies Article 26 because assignment of the human-oversight person is a mandatory field on the register." If a mapping needs three paragraphs, either the crosswalk is over-claimed or the control is doing too much.

## Reading ISO/IEC 42001 Annex A into control entries

ISO/IEC 42001 Annex A is organised by clause. The Annex A headings in the published 2023 standard map to areas any AIMS operating standard would recognise — policies for AI, organisation for AI, resources for AI, AI system lifecycle, data for AI systems, information for interested parties, use of AI systems, third-party relationships. Each heading contains one or more numbered controls with a short objective and a control statement.

Two disciplines when composing Annex A into your library:

**Discipline 1: The AIMS Statement of Applicability drives Annex A inclusion, not the library.** Every Annex A control must appear on the SoA (mod-105) as included or excluded with justification. But an included Annex A control does not need a one-to-one enterprise control — it needs enterprise controls that *satisfy* it. When you author the SoA row, you point at 1..N enterprise controls that implement the Annex A objective. That is the crosswalk in the reverse direction: SoA → library → parent.

**Discipline 2: Do not re-flavor the Annex A statement into your library statement.** The library statement is *your* outcome. The Annex A statement is the AIMS's outcome. They are similar and often overlap; they are not the same. If your library statement is a lightly reworded Annex A control, you have not architected — you have transcribed. Ask: "if we deleted this Annex A reference, would the enterprise still need this control?" If the honest answer is no, the entry is decorative.

## Reading EU AI Act Articles 9–15 into control entries

Articles 9 through 15 of Regulation (EU) 2024/1689 impose the operative technical obligations on providers of high-risk AI systems. In order:

- **Article 9** — risk management system across the AI system lifecycle.
- **Article 10** — data governance for training, validation, and testing datasets.
- **Article 11** — technical documentation (see also Annex IV for the required contents).
- **Article 12** — record-keeping (automatic logs).
- **Article 13** — transparency and provision of information to deployers.
- **Article 14** — human oversight.
- **Article 15** — accuracy, robustness, and cybersecurity.

Each of these composes with multiple NIST AI RMF sub-categories and multiple ISO/IEC 42001 Annex A entries. Some worked composition landings:

- **Article 9** → NIST AI RMF MAP-1 through MAP-5 (context and risk identification), MANAGE-1 through MANAGE-4 (treatment) → ISO/IEC 42001 Clause 6 (planning) and Annex A entries under risk. Enterprise controls: `AIC-RSK-001` AI Risk Register Schema, `AIC-RSK-002` Risk Treatment Plan Cadence, `AIC-RSK-003` Residual Risk Sign-Off.
- **Article 10** → NIST AI RMF MAP-2.3, MAP-4.1, MEASURE-2.8, MEASURE-2.11 → ISO/IEC 42001 Annex A.7 → `AIC-DAT-014` (training-data provenance record from chapter 01), `AIC-DAT-015` (bias-relevant characteristics documentation), `AIC-DAT-016` (data-quality thresholds and remediation).
- **Article 11** → NIST AI RMF MAP-4.1, MANAGE-2 → ISO/IEC 42001 Annex A.6 → `AIC-DOC-021` Technical Documentation Package, `AIC-DOC-022` Documentation Update-on-Change Rule.
- **Article 12** → NIST AI RMF MEASURE-2.7, MEASURE-2.10 → ISO/IEC 42001 Annex A.6 and A.8 → `AIC-LOG-031` Automatic Log Retention, `AIC-LOG-032` Log Integrity, `AIC-LOG-033` Log-Access Control.
- **Article 13** → NIST AI RMF GOVERN-5, MEASURE-3.3, MANAGE-4.3 → ISO/IEC 42001 Annex A.6 → `AIC-TXP-041` Instructions for Use, `AIC-TXP-042` System Characteristics Disclosure.
- **Article 14** → NIST AI RMF GOVERN-2, MANAGE-3.2 → ISO/IEC 42001 Annex A.3 → `AIC-OVS-051` Human Oversight Assignment, `AIC-OVS-052` Oversight Effectiveness Testing, `AIC-OVS-053` Escalation and Override Procedure.
- **Article 15** → NIST AI RMF MEASURE-2.5, MEASURE-2.6, MEASURE-2.7, MANAGE-2 → ISO/IEC 42001 Annex A.8 → `AIC-ROB-061` Accuracy Threshold, `AIC-ROB-062` Robustness Testing, `AIC-ROB-063` Cybersecurity Baseline (composes with the OWASP LLM Top 10 / MITRE ATLAS content in chapter 03).

The italicised discipline is *composition, not translation*. Every article is landing in a small handful of enterprise controls, each of which the library already needs regardless of the article — because the same outcome is required by NIST AI RMF, by ISO/IEC 42001, by SR 11-7, by internal risk policy. The article is one more voice pointing at the same control.

## An applicability discipline for jurisdictional obligations

Articles 9–15 apply *only* to high-risk AI systems (as defined by Article 6 and Annex III). If you set the applicability filter on `AIC-DAT-014` to *all* tier-1 and tier-2 systems in *all* jurisdictions, you are asserting the control globally — which is often what you want strategically, but not what Article 10 legally requires. The distinction matters at audit time.

Two ways to model this correctly:

**Option A — one control, one applicability filter.** The enterprise decides to hold *all* qualifying AI systems to the same standard globally. Applicability filter is "tier-1 or tier-2" without a jurisdictional predicate. The crosswalk still names EU AI Act Article 10 to signal that the control *also* satisfies EU law where applicable. The rationale that this is an over-commitment (not just an EU obligation) lives in the catalog preface.

**Option B — one control, two applicability profiles.** The catalog carries one baseline control (`AIC-DAT-014`) whose applicability is "all tier-1/2 globally," and a jurisdictional profile (mod-104) that adds an EU-specific stricter parameterisation (e.g. tighter data-quality thresholds, retention of the specific Article 10 documentation set). Chapter 04 covers how OSCAL profiles express this.

Option A is simpler and defensible for enterprises that have chosen a "one high bar" posture. Option B is essential when the EU obligation is *stricter* than the enterprise baseline (which happens in specific corners like biometric categorisation) or when the enterprise is honestly at a lower baseline outside the EU. Either is fine; the failure mode is *silent conflation*, where the library implies option A while operationally running option B.

## Two composition patterns to avoid

**Anti-pattern 1 — Parallel libraries per source.** The enterprise has a `NIST-AI-RMF-controls.xlsx`, an `ISO-42001-controls.xlsx`, and an `EU-AI-Act-controls.docx`. Each restates the same outcomes with slightly different language, slightly different owners, and drifts from the others quarter by quarter. Every SoA update means updating three spreadsheets by hand; every audit finds an inconsistency in the delta. The fix is a single library with per-source crosswalks; the transition cost is real but pays back the first audit cycle.

**Anti-pattern 2 — Verbatim-quotation library.** Every control statement is a verbatim quote from one of the three sources. The library becomes an index of external text, not an enterprise commitment. Auditors reasonably ask "yes, that is what NIST said — what did *you* commit to doing?" — and the answer is nowhere in the library.

Chapter 01's discipline of the statement as an enterprise outcome claim, not a paraphrase, is what defeats both anti-patterns.

## Signals the composition is working

You can tell the composition is working when:

- The count of AI-library entries is *roughly independent* of the number of source frameworks. Adding a new framework (or a new jurisdiction — mod-104) adds *crosswalk targets*, not many new controls.
- An SoA row for an ISO/IEC 42001 Annex A control lists 1–3 enterprise-control implementers, not 0 (uncovered) and not 8 (over-decomposed).
- An EU AI Act Article 11 technical documentation packet is generated by *querying* the library for `eu_ai_act: [Article 11]` and pulling evidence artefacts from each matching control's evidence contract. If the packet is hand-assembled from three parallel spreadsheets, the composition is not there yet.

## Summary

NIST AI RMF, ISO/IEC 42001 Annex A, and EU AI Act Articles 9–15 compose into one library through one-to-many crosswalks on enterprise control entries whose statements express outcomes in the enterprise's own voice. Each source retains its distinct grain and speech act; the library does not paraphrase any of them. The Annex A SoA drives inclusion; jurisdictional applicability filters express whether the enterprise is running "one high bar" or "profile per jurisdiction." Get this composition right and the same evidence set defends the same posture to three different audiences.
