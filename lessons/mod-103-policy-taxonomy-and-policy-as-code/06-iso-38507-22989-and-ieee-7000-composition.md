# Reference frames — ISO/IEC 38507, ISO/IEC 22989, and the IEEE 7000-series

## Why this chapter exists

The policy taxonomy that chapters `01-policy-hierarchy-authoring-drill.md` through `05-policy-change-communications-flow.md` build — Responsible AI principles at the top, binding policy underneath, standards below that, procedures and work instructions at the leaves, with a policy-as-code slice cutting laterally through — does not exist in a vacuum. It sits inside three external reference frames that the enterprise did not author and must not attempt to re-author:

- **ISO/IEC 22989** gives the taxonomy its shared *vocabulary*. Every time a policy says "AI system", "machine learning", "training data", "inference", or "explainability", there is a 22989 definition it should be pointing at rather than re-defining locally.
- **ISO/IEC 38507** gives the taxonomy its *governance-body positioning*. It is the standard that tells the board and the audit committee what "governance of AI" looks like at their tier, above the AIMS (mod-105) and above the control library (mod-102).
- The **IEEE 7000-series** gives the taxonomy its *ethics-methodology backing*. It is where design-time surfacing of ethical concerns comes from — the discipline that populates procedures with real ethical content rather than boilerplate.

Ignore any of these frames and one of three predictable failures follows: the enterprise reinvents terms that already have standardised definitions (and loses crosswalk with sibling programmes), it loses the language its own board expects to hear, or it uses ethical language in policy without a methodology to back it up. This chapter positions the three frames so chapters `01`–`05` compose with them cleanly rather than accidentally competing with them.

The composition rule this chapter closes with — vocabulary, then governance-body, then management-system, then ethics-methodology, then enterprise-normative, then control-library — is the mental map to keep in mind while reading.

## ISO/IEC 22989:2022 — the vocabulary layer

**What it is.** *ISO/IEC 22989:2022 — Information technology — Artificial intelligence — Artificial intelligence concepts and terminology* is the ISO/IEC vocabulary standard for AI. It was published jointly with *ISO/IEC 23053:2022 — Framework for artificial intelligence (AI) systems using machine learning (ML)*, which supplies a reference architecture for ML-based AI systems. Together they are the foundational vocabulary layer for the entire ISO/IEC AI series: ISO/IEC 42001:2023 (AIMS), ISO/IEC 23894:2023 (AI risk-management guidance), ISO/IEC 42005 (AI system impact assessment), and the ISO/IEC 5259 series (data quality for analytics and ML) all defer to 22989 for their terms. (Both 22989 and 23053 are paywalled ISO/IEC deliverables.)

Mod-101 chapter 03 (`03-iso-42001-family-and-vocabulary-layer.md`) framed 22989 as the vocabulary layer under the whole family. This chapter narrows the framing to a taxonomy-authoring rule.

**How it composes with the policy taxonomy.** Anchor the *definitions* section of every binding policy and every standard to 22989 terms. Do not redefine "AI system", "machine learning", "training data", "inference", "model", "explainability", or "transparency" in enterprise policy language. Adopt and cite 22989.

The mechanical pattern in a binding policy's opening pages:

```markdown
## 2. Definitions

For the purposes of this policy, the following terms have the meanings
assigned in ISO/IEC 22989:2022, at the clauses indicated:

- **AI system** — ISO/IEC 22989:2022, clause <!-- needs-research: confirm current clause number against the published standard -->.
- **Machine learning** — ISO/IEC 22989:2022, clause <!-- needs-research: confirm current clause number against the published standard -->.
- **Training data** — ISO/IEC 22989:2022, clause <!-- needs-research: confirm current clause number against the published standard -->.
- **Inference** — ISO/IEC 22989:2022, clause <!-- needs-research: confirm current clause number against the published standard -->.
- **Explainability** — ISO/IEC 22989:2022, clause <!-- needs-research: confirm current clause number against the published standard -->.
- **Transparency** — ISO/IEC 22989:2022, clause <!-- needs-research: confirm current clause number against the published standard -->.

Where a term is required by this policy that is not defined in
ISO/IEC 22989:2022, it is defined locally in Annex A of this policy
and its local status is noted.
```

Two disciplines make this work.

First, do not paraphrase 22989 into policy prose. The moment "AI system" is *both* defined by policy and cited to 22989, drift is guaranteed — the next revision of 22989 will shift the boundary and the policy's local paraphrase will not follow. Cite the clause; do not restate it.

Second, be explicit about the local-definitions escape hatch. Enterprises always need at least a few terms that 22989 does not carry (for example, tier labels for the enterprise's AI risk tiering scheme in mod-106, or the internal name of an oversight forum). Put those in an annex, mark them clearly as local, and keep the count small. Every local term is a term the crosswalk to a sibling programme has to translate by hand.

**Where 22989 does *not* cover the enterprise need.** 22989 is deliberately vocabulary. It carries definitions, not obligations. It cannot serve as a policy source; only as a definitional source. If a draft policy contains a phrase like "in accordance with ISO/IEC 22989 the organisation shall…", the phrase is malformed — 22989 issues no shall statements. Move the obligation to a proper source (ISO/IEC 42001, ISO/IEC 23894, the EU AI Act, the enterprise's own binding policy) and leave 22989 to define the nouns.

The related standard **ISO/IEC 23053:2022** is worth calling out here even though it is a framework rather than a vocabulary. When a standard in the taxonomy needs to reason about ML-system *components* — data pipelines, training pipelines, inference services, model registries — 23053's reference architecture is the frame to cite for the component names and relationships, so procedures across engineering teams talk about the same parts.

## ISO/IEC 38507:2022 — the governance-body layer

**What it is.** *ISO/IEC 38507:2022 — Information technology — Governance of IT — Governance implications of the use of artificial intelligence by organizations* is a governance-tier standard. It belongs to the ISO/IEC 38500 family — the governance-of-IT family whose subject is the *governing body* (the board, or the equivalent top-tier accountable body), not the operational management system. 38507 addresses how the governing body exercises governance over the organisation's use of AI: the questions it should be asking, the accountabilities it must own, the principles it should be applying, and the assurance it should be demanding from management.

Critically, 38507 is *not* a management-system standard. It is not certifiable in the way ISO/IEC 42001 is. It sits above the AIMS at the governance-body layer and speaks a board-tier vocabulary. (Paywalled ISO/IEC deliverable.)

**How it composes with the policy taxonomy.** 38507 gives the taxonomy two things chapters `01`–`05` cannot supply from within:

- **Governance-body vocabulary.** The words the board and the audit committee use for AI oversight — accountability of the governing body, risk appetite as a governance instrument, the governing body's duty to require reporting — should track 38507's language. When the binding policy's preamble sets out *why* the enterprise has an AI policy, that "why" is 38507's governance-implications frame rendered in the enterprise's voice.
- **A shape for the Responsible AI principles.** The enterprise's Responsible AI principles (chapter `01`) are, in effect, the enterprise's expression of "principles for the use of AI by the organisation" — the topic 38507 addresses. Shape the principle set against 38507's guidance rather than inventing the categorisation from scratch. The mapping is enterprise-specific and belongs in the crosswalk exercise (`exercise-02-principle-to-policy-to-control-crosswalk.md`), not in this chapter, but the reference frame is 38507.

Both the binding policy authored in chapter `01` and the principle-to-policy-to-control traceability chain in chapter `02` should cite 38507 as their governance frame. Concretely:

```markdown
## 1. Purpose and governance context

This policy is issued by the board of directors under the governance
framework described in ISO/IEC 38507:2022. It gives effect to the
governing body's accountability for the organisation's use of AI and
sets the expectations against which management is required to report
under Clause 9 of the AI Management System (mod-105).
```

The board AI-committee charter — the artefact that stands up the AI oversight forum at governance-body tier — should reference 38507's guidance on governing-body responsibilities directly. A worked mapping of a charter's oversight responsibilities to 38507 clauses is on the exercise sheet for this chapter; the point here is the *positioning* — the charter is a 38507-shaped artefact, not a 42001-shaped one.

**Positioning against 42001.** ISO/IEC 42001 sits inside the *management-system layer* (the AIMS, worked in mod-105); 38507 sits *above* it at the governance-body layer. The policy taxonomy in mod-103 is the bridge between the two: it inherits its shape at the top from 38507's governance-body frame, and it feeds into the AIMS's documented-information set (42001 Clause 7.5) at the bottom.

If a draft artefact confuses these two tiers — for example, an AI committee charter that cites 42001 clauses for its own authority rather than 38507 clauses — the artefact is arriving at the wrong altitude. 42001 is *about* what management does; it is not the authority under which the board acts.

**Worked example — mapping a board AI-committee charter to 38507.** The charter names oversight responsibilities such as "review and approve the enterprise Responsible AI principles annually", "receive quarterly AI risk-posture reporting from the AIMS management review", and "approve exceptions to binding AI policy above a defined risk-threshold". Each of these responsibilities has a 38507 clause it operationalises. The exercise for this chapter walks through the mapping and marks the specific clause numbers with `<!-- needs-research: confirm current clause numbers against the published standard -->` — the point of the worked example is not the exact clause numbers (which move between editions) but the *shape* of the traceability: charter responsibility → 38507 clause → binding-policy commitment → AIMS artefact.

## The IEEE 7000-series — the ethics-methodology layer

**Overview of the family.** The IEEE 7000-series is the ethics-methodology reference frame the taxonomy composes with. The anchor standard is **IEEE 7000-2021 — *Model Process for Addressing Ethical Concerns during System Design*** — an IEEE standard defining a process by which product teams surface ethical concerns during system design and translate those concerns into technical requirements.

The wider family covers specific concerns and is under continuous development. Members whose scope is worth naming in the reference frame:

- **IEEE 7001** — transparency of autonomous systems. <!-- needs-research: confirm current publication status and exact title -->
- **IEEE 7002** — data privacy process. <!-- needs-research: confirm current publication status and exact title -->
- **IEEE 7003** — algorithmic bias considerations. <!-- needs-research: confirm current publication status and exact title -->
- **IEEE 7005** — employer data governance. <!-- needs-research: confirm current publication status and exact title -->
- **IEEE 7007** — ontological standards for ethically driven robotics and automation systems. <!-- needs-research: confirm current publication status and exact title -->
- **IEEE 7010** — well-being metrics for autonomous and intelligent systems. <!-- needs-research: confirm current publication status and exact title -->
- **IEEE 7014** — emulated empathy in autonomous and intelligent systems. <!-- needs-research: confirm current publication status and exact title -->

Other numbered members of the series exist and are in various stages of development, ballot, or publication. Do *not* cite a specific IEEE 7000-series number in policy without confirming its current status; mark the citation with a `<!-- needs-research: ... -->` until confirmed.

**How it composes with the policy taxonomy.** IEEE 7000-2021 is a *methodology* — a defined process for eliciting ethical values from stakeholders, exploring the concept of operations, tracing ethical concerns into system requirements, and validating that the resulting system honours those concerns. That places IEEE 7000 in the *design-time procedure* layer of the taxonomy (the layer whose authoring is anchored in chapter `01`), not in the policy layer.

The composition rule: the enterprise's binding AI policy (chapter `01`) says "for tier-1 and tier-2 AI systems, an ethics review is completed at design time using the procedure defined in `ENT-PROC-ETHICS-01`". The procedure `ENT-PROC-ETHICS-01` then references IEEE 7000-2021 as the methodology it operationalises — inheriting IEEE 7000's stages (stakeholder identification, elicitation of ethical values, concept-of-operations exploration, translation into system requirements, and traceability back to the elicited values).

Written the other way — putting IEEE 7000 in the policy layer as if it were a source of obligations — is a shape mistake. IEEE 7000 does not issue "the organisation shall" statements the way ISO/IEC 42001 does. It is a *how-to* for a design-time activity. The obligation to *run* that activity comes from the enterprise's binding policy; the methodology inside the activity comes from IEEE 7000.

**Where the IEEE frame is stronger than ISO/IEC.** The IEEE 7000-series brings capabilities the ISO/IEC family is deliberately thinner on:

- Surfacing *new* ethical concerns in novel product contexts, rather than checking against a fixed control catalog.
- Documenting *ethical-value elicitation* with stakeholder groups — a process step ISO/IEC standards reference but do not prescribe in detail.
- Supporting *concept-of-operations (ConOps)* reviews before technical requirements are frozen — a stage upstream of where most ISO/IEC controls attach.

**Where the ISO/IEC frame is stronger than IEEE.** Symmetrically, ISO/IEC is stronger on:

- *Organisational accountability* — management-system requirements, roles, responsibilities, documented information (42001 Clauses 5 and 7).
- *Auditability* — the SoA + evidence-contract discipline (mod-102, mod-105).
- *Certification* — third-party certification against 42001 by 42006-conformant bodies (mod-101 chapter 03).

The two frames are complementary. Neither substitutes for the other. The taxonomy uses IEEE 7000 to make sure ethical concerns get *surfaced* early and translated into requirements; it uses ISO/IEC 42001 to make sure the surfacing *happened*, produced *documented information*, and was reviewed at the right cadence.

**Worked example — pre-build ethics review procedure.** The enterprise's pre-build ethics-review procedure (a level-4 artefact in the hierarchy of chapter `01`) attaches at the concept-of-operations stage of a tier-1 or tier-2 AI system. Its process section says, in effect:

```markdown
## 4. Process — pre-build ethics review

The ethics review operationalises the Concept of Operations
exploration stage described in IEEE 7000-2021, adapted to the
enterprise's AI product lifecycle. The reviewer completes the
following stages, producing documented information at each:

- Stakeholder identification — the reviewer identifies the affected
  parties (users, non-user affected persons, operators, regulators)
  in accordance with the enterprise stakeholder register.
- Ethical value elicitation — the reviewer facilitates a session
  with the stakeholder groups (or their proxies) to elicit the
  ethical values at stake, following the elicitation stage of
  IEEE 7000-2021.
- Concept of Operations review — the reviewer walks the proposed
  system ConOps against the elicited ethical values, identifying
  concerns to be addressed at design time.
- Translation to system requirements — each concern is translated
  into a system requirement or a mitigation, cross-referenced to
  the AI-library control that will govern its evidence.
- Traceability record — the reviewer produces the traceability
  record `IA-ETH-<system-id>-v<n>.md` and files it against the
  system's AIMS impact-assessment record (per ISO/IEC 42005 and
  mod-105).
```

The procedure's authority chain is: binding policy requires an ethics review → standard sets the applicability threshold and the evidence contract → procedure describes the methodology (IEEE 7000-2021) → work instruction supplies the reviewer's session-facilitation checklist.

## The composition rule — sequence the frames; do not pick a winner

The three frames above, plus ISO/IEC 42001, plus the enterprise's own normative layer, plus the AI control library from mod-102, form a stack. Sequence them. Do not attempt to elect one as canonical.

1. **Vocabulary layer** → ISO/IEC 22989 (and ISO/IEC 23053 where the taxonomy needs to name ML-system components).
2. **Governance-body layer** → ISO/IEC 38507. Board- and audit-committee tier; shape of Responsible AI principles; charter of the board AI committee.
3. **Management-system layer** → ISO/IEC 42001 (previewed in mod-101 chapter 03, worked in detail in mod-105). Shape of the AIMS; SoA discipline; documented-information set.
4. **Ethics-methodology layer** → IEEE 7000-series. Design-time surfacing of ethical concerns and their translation into requirements; sits inside the procedure layer of the taxonomy.
5. **Enterprise-specific normative layer** → the binding policy and standards authored in chapters `01-policy-hierarchy-authoring-drill.md` and `02-principle-to-policy-to-control-crosswalk.md`. This is where the enterprise's "shall" statements live.
6. **Control-library layer** → the `AIC-*` control entries authored in mod-102 (chapter `02-composing-nist-rmf-iso-42001-eu-ai-act.md` and siblings). Testable outcomes with evidence contracts.

Each layer speaks in its own genre and each cross-references the layers above and below. The taxonomy does not paraphrase any of them.

## Related frames the architect should be aware of

There are further ISO/IEC standards the architect must recognise on sight so as not to accidentally re-invent them in the taxonomy, even though the taxonomy is not *built around* them. Reference them briefly, cite them by title, and know where they sit:

- **ISO/IEC 5338** — AI system life-cycle processes. Reference frame for how AI systems are built and operated as a lifecycle; complements 23053. <!-- needs-research: confirm current publication year -->
- **ISO/IEC 5259 series** — data quality for analytics and machine learning. Multi-part standard; feeds data-quality standards and procedures under the taxonomy's data domain. <!-- needs-research: confirm currently-published parts -->
- **ISO/IEC 42005** — AI system impact assessment. The process standard the enterprise impact-assessment schema conforms to (mod-101 chapter 03; mod-105). <!-- needs-research: confirm current publication year -->
- **ISO/IEC 23894:2023** — AI risk management guidance. Bridges ISO 31000 to the AI context; anchors the risk taxonomy in mod-106.
- **ISO/IEC TR 24028:2020** — Overview of trustworthiness in AI. Older and more surveyable than the newer standards; useful as an on-ramp for teams not yet comfortable with the vocabulary.

Each of these is paywalled ISO/IEC content. Cite them where the taxonomy references them; do not attempt to substitute for them in enterprise text.

## Two common shape mistakes

**Shape mistake 1 — citing ISO/IEC 42001 where ISO/IEC 38507 is the correct governance-level frame.** The board AI-committee charter's authority section reads "the committee acts under ISO/IEC 42001 Clause 5". The clause is a management-system leadership clause; it addresses top management's role *inside* the AIMS. The board committee sits *above* the AIMS. The correct citation is ISO/IEC 38507, framed as the governing-body-of-IT extension for AI. The 42001 citation is a genre error — the standard being cited is not the one the artefact is operating at.

**Shape mistake 2 — embedding a private definition of "AI system" in policy instead of citing ISO/IEC 22989.** The binding policy opens with a hand-crafted definition of "AI system" written by legal, which subtly differs from the 22989 definition (and, downstream, from the EU AI Act's Article 3 definition). Every subsequent crosswalk to NIST AI RMF, to ISO/IEC 42001, to the EU AI Act, has to reconcile the private definition with the source's definition. Within a year the private definition has drifted and the crosswalk is producing false positives and false negatives in scope determination. The fix is to cite 22989 at authoring time, defer local terms to an annex, and translate to the EU AI Act's Article 3 definition explicitly rather than by paraphrase (mod-104 will pick up the multi-jurisdiction reconciliation).

**Shape mistake 3 — using IEEE 7000 as a policy source when it is a methodology source.** The binding policy contains the clause "In accordance with IEEE 7000-2021, the organisation shall complete an ethics review for all high-risk AI systems." The obligation is fine; the citation is malformed. IEEE 7000-2021 does not obligate the organisation to do the review — the enterprise's binding policy does. IEEE 7000-2021 supplies the *methodology* the review uses. The corrected clause: "the organisation shall complete an ethics review, using the methodology defined in `ENT-PROC-ETHICS-01`, which operationalises IEEE 7000-2021, for all tier-1 and tier-2 AI systems." Now the obligation and the methodology sit at their respective layers.

## Summary

ISO/IEC 22989 gives the policy taxonomy its vocabulary, ISO/IEC 38507 gives it its governance-body positioning, and the IEEE 7000-series gives it its ethics-methodology backing — each sitting at a different layer of the same stack, with ISO/IEC 42001 as the management-system layer between them and the enterprise's own binding policy and control library beneath. Sequence the frames rather than choosing one as canonical; cite each at the altitude it operates at; adopt 22989 definitions rather than re-defining terms locally; treat IEEE 7000 as a methodology reference inside the procedure layer rather than as a policy source; and remember that 38507 is a governing-body standard, not a management-system standard. Get the composition right and chapters `01`–`05` inherit a working vocabulary, a working governance frame, and a working ethics methodology without importing three years of taxonomy drift.
