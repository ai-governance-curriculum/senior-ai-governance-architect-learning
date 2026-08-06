# The AIMS as an architectural artefact — not a policy folder

## Why this chapter exists

An AI Management System built to ISO/IEC 42001:2023 is a *management system*. The distinction matters because "management system" in the ISO harmonised structure (Annex SL) has a specific technical meaning — a defined set of interlocking clauses, obligations, and artefacts that together let an organisation *plan, do, check, and act* against a specified subject matter (in 42001's case, the organisation's use, development, provision, and stewardship of AI systems). That specific meaning is what makes the AIMS *certifiable* — an accredited certification body operating under ISO/IEC 42006:2025 can sample the artefacts, test their conformity, and issue a certificate of conformance. Nothing else in the AI governance stack has that property.

The level-50 architect owns the *shape* of this management system. Not the content of every risk-treatment plan entry (that is `ai-risk-engineer` at level 25). Not the running of the internal audit rounds (that is internal audit, informed by `head-of-ai-governance` at level 60). Not the drafting of every AI policy clause (that is legal in partnership with the head of AI governance). The architect designs the *interlocks* — which artefact feeds which, what the SoA-to-control-library binding looks like, how the AIA process feeds the risk-treatment plan, how the AIMS management review reads outputs from the ISMS management review — and hands the operating model to the head of AI governance to run.

This chapter frames the AIMS as an architectural artefact rather than a compliance checklist, walks the Annex SL clause skeleton the standard inherits, positions 42001 among its siblings (42005, 42006, 23053, 23894, 27001, 31000, 38507, 5259), and closes with the composition rule the ten chapters that follow enact.

## What "management system" means in the ISO harmonised structure

ISO's management-system standards — ISO 9001 (quality), 14001 (environment), 27001 (information security), 22301 (business continuity), 45001 (occupational health and safety), 42001 (AI) — all share the same top-level clause skeleton, imposed by Annex SL of the ISO/IEC Directives Part 1. The skeleton is:

- **Clauses 1–3** — Scope, normative references, terms and definitions. Front-matter.
- **Clause 4** — Context of the organisation. External and internal issues, interested parties, scope of the management system, the management system itself.
- **Clause 5** — Leadership. Top-management commitment, policy, roles and responsibilities.
- **Clause 6** — Planning. Actions to address risks and opportunities, objectives, planning of changes.
- **Clause 7** — Support. Resources, competence, awareness, communication, documented information.
- **Clause 8** — Operation. Operational planning and control.
- **Clause 9** — Performance evaluation. Monitoring, measurement, analysis, evaluation; internal audit; management review.
- **Clause 10** — Improvement. Non-conformity and corrective action; continual improvement.

An organisation that already runs an ISMS to ISO/IEC 27001 will recognise every one of these clauses; the shape is identical because it *is* the same shape. The AI-specific content is what fills the shape. The architect's first move — before ever authoring an SoA row or a risk-treatment plan entry — is to internalise this skeleton, because every subsequent decision (where a communications plan goes, where an AIA hooks in, where non-conformity handling lives) is a decision about *which clause of the skeleton it satisfies*.

## What the AIMS is — six inseparable artefacts

Underneath the clause skeleton, an AIMS is six artefacts that must all exist and all cross-reference. Miss one and the AIMS is not conformant. Duplicate one across independent silos and the AIMS is not operable.

1. **The scope statement** (Clause 4.3). One paragraph — sometimes two — that names the boundary of the AIMS. Which AI systems are in scope, which organisational units, which geographies, which lifecycle stages, and which explicitly are *out*. The scope statement is the anchor artefact of the entire system; every other artefact refers back to it. Chapter `02-scope-context-and-the-23053-reference-architecture.md` walks how to author it.

2. **The Statement of Applicability** (Clause 6.1.3.d, referring to Annex A). A table that walks every one of the Annex A controls, records whether it is *included* or *excluded* from the AIMS, and — for each — records a justification. The SoA is the *most-sampled artefact* in a stage-2 certification audit. Chapter `05-statement-of-applicability-and-annex-a.md` walks the discipline.

3. **The risk-and-impact-assessment process + the risk-treatment plan** (Clauses 6.1.2 and 6.1.3). The AIMS's risk process, composing ISO 31000 as the parent framework with ISO/IEC 23894 as AI-specific guidance and ISO/IEC 42005 as the impact-assessment process; the treatment plan is the artefact that records, per identified risk, what treatment is chosen (avoid / reduce / transfer / accept), which Annex A controls implement the treatment, who owns the treatment, and what residual risk was accepted by whom. Chapters `04-risk-and-impact-assessment-composition.md` and `06-risk-treatment-plan-and-operational-clauses.md` split the process and the artefact.

4. **The competence and communications plans** (Clause 7). Documented plans for who must be competent to operate the AIMS (roles, competence definitions, competence-assessment cadence), and how the AIMS communicates internally and externally (channels, audiences, cadences, escalations). Chapter `07-support-competence-awareness-communications-and-documented-information.md` walks the shape.

5. **The internal audit programme + management review** (Clause 9). A defined multi-year internal audit programme that samples the AIMS clause by clause; a defined management-review cadence (typically annual, sometimes semi-annual) that top management demonstrably attends and demonstrably acts on. Chapter `08-performance-evaluation-internal-audit-and-management-review.md` walks the operations calendar this creates.

6. **The non-conformity and corrective-action (CAPA) process** (Clause 10). The defined process by which non-conformities are logged, root-caused, corrective action taken, effectiveness verified, and the corrective-action record retained. Chapter `09-non-conformity-corrective-action-and-continual-improvement.md` walks the shape.

These six artefacts are not modular. Remove the SoA and the risk-treatment plan floats free from the Annex A control set. Remove the internal audit programme and the management review has no independent input other than management's own opinion. Remove the CAPA process and non-conformities discovered by internal audit or by the certification body have nowhere to go. The architect designs the six artefacts *together* as a single interlocked system, not one by one.

## Composition — how 42001 sits inside the standards stack

The AIMS does not stand alone. It composes with a specific set of sibling standards, each of which occupies a distinct layer. Get the composition right and the AIMS inherits vocabulary, risk-management shape, impact-assessment methodology, and auditability from proven sources. Get it wrong and the AIMS re-invents pieces that already exist and drifts from the sibling ISMS the enterprise already runs.

**Vocabulary layer** — **ISO/IEC 22989:2022** *(Artificial intelligence concepts and terminology)*. The AIMS uses 22989 definitions for "AI system", "machine learning", "training data", "inference", "explainability", "transparency". It does not re-define them. Mod-103 chapter 06 argued this at length; mod-105 obeys it.

**Reference-architecture layer** — **ISO/IEC 23053:2022** *(Framework for artificial intelligence (AI) systems using machine learning (ML))*. 23053 supplies the *component names and relationships* the AIMS reasons about: data pipelines, training pipelines, inference services, model registries, and their interfaces. The scope statement is written *against* 23053 — the boundary is drawn in 23053 terms so the certification body reads it against a known reference architecture. Chapter `02-scope-context-and-the-23053-reference-architecture.md` walks this.

**Risk-management parent** — **ISO 31000:2018** *(Risk management — Guidelines)*. 31000 is the generic risk-management guidance every ISO management-system standard defers to for the shape of its risk process (context establishment, risk identification, risk analysis, risk evaluation, risk treatment, monitoring and review, communication and consultation). The AIMS risk process (Clause 6.1) is a 31000-shaped process, specialised to AI. The enterprise ERM function almost certainly already uses 31000 in some form; the AIMS composes with that rather than forking.

**AI risk guidance** — **ISO/IEC 23894:2023** *(Guidance on risk management)*. 23894 is the AI-specific companion to 31000. It carries the AI-specific risk-source taxonomy, the AI-specific factors to consider during risk analysis (e.g. model drift, training-data provenance, adversarial robustness), and the AI-specific stakeholders. The AIMS risk process (Clause 6.1) *references* 23894 as its guidance layer; the risk-treatment plan (Clause 6.1.3) *implements* the treatments 23894 informs. Chapter `04-risk-and-impact-assessment-composition.md` walks the composition.

**AI impact-assessment process** — **ISO/IEC 42005:2025** *(AI system impact assessment)*. 42005 is the process standard for AI-specific impact assessments (AIAs) — the artefact that examines a system's foreseeable consequences on individuals, groups, and society, and translates those consequences into concrete mitigations. Under 42001 Clause 6.1.4 (impact assessment on individuals, groups, and society), 42005 is the *how*. The AIA output is a first-class input to the AIMS risk-treatment plan. Chapter `04-risk-and-impact-assessment-composition.md` details the wire-up. <!-- needs-research: confirm ISO/IEC 42005 publication year — was published or nearing publication in 2025 at the time of writing; verify final date. -->

**Auditor's rulebook** — **ISO/IEC 42006:2025** *(Requirements for bodies providing audit and certification of AI management systems)*. 42006 is the requirements standard for the certification bodies that audit AIMS conformance. The AIMS architect does *not* implement 42006 — the certification body does — but reads it in reverse: what will the auditor be told to look for, at what sampling depth, with what competence? The AIMS is designed to *survive* a 42006-shaped audit. Chapter `11-designing-for-third-party-audit-iso-42006.md` walks the read. <!-- needs-research: confirm ISO/IEC 42006 publication year. -->

**Sibling management-system standard** — **ISO/IEC 27001:2022** *(Information security management systems — Requirements)*. Every enterprise that has an AIMS also has an ISMS. The Annex SL structure means the two clause-skeletons are identical; the enterprise's opportunity is to run *one integrated management system* rather than two disconnected ones with duplicated documentation, duplicated audit programme, and duplicated management review. Chapter `10-integrating-the-aims-with-the-iso-27001-isms.md` walks the integration.

**Governance-body layer** — **ISO/IEC 38507:2022** *(Governance implications of the use of artificial intelligence by organizations)*. 38507 sits *above* the AIMS at the governance-body (board / audit-committee) tier. The AIMS reports up to the governance body under 38507's frame; the governance body demands assurance from the AIMS. Mod-103 chapter 06 framed this positioning; this module inherits it.

**Data-quality layer** — **ISO/IEC 5259 series** *(Data quality for analytics and machine learning)*. The AIMS's data-quality obligations (Annex A controls on data governance and data quality; Clause 8 operational controls on data pipelines) reference the 5259 parts for the *definitions* of data-quality dimensions, the measurement methods, and the reporting shape. The AIMS does not re-invent data-quality terminology. <!-- needs-research: confirm currently-published parts of the ISO/IEC 5259 series. -->

## The two failure modes to design against

**Failure mode 1 — the AIMS as a policy folder.** The organisation stands up a SharePoint (or Confluence) site called "AI Management System", drops the AI policy, the responsible-AI principles, and a stack of standards documents into it, points to the folder, and calls it certifiable. A stage-1 audit will conclude within three hours that no management system exists: there is no SoA, no risk-treatment plan, no internal audit programme, no management-review record, no CAPA process. A management system is not a document set; it is a *set of processes producing documented information*. The distinction is the entire point.

**Failure mode 2 — the AIMS as a parallel universe to the ISMS.** The information-security team runs a mature ISMS with its own scope statement, SoA, risk register, audit programme, management review, and CAPA process. The AI-governance team stands up a completely independent AIMS with a *different* scope statement, a *different* risk register, a *different* audit programme, a *different* management review, and a *different* CAPA process. Documentation is duplicated. Auditors visit twice. The two management-review forums make contradictory decisions on shared controls (encryption at rest, access management, incident response). Within a year the enterprise is worse off than it was with just the ISMS. Chapter `10-integrating-the-aims-with-the-iso-27001-isms.md` details the integrated design that avoids this.

## The composition rule the module enacts

The remainder of this module walks a single composition rule, chapter by chapter:

1. Anchor the AIMS *scope* in ISO/IEC 23053 vocabulary so the certification body reads it against a known reference architecture (chapter 02).
2. Compose the AIMS *risk process* on ISO 31000, specialised by ISO/IEC 23894, extended by ISO/IEC 42005 for impact assessment (chapters 03–04).
3. Bind the AIMS *SoA* to the enterprise AI control library (mod-102) so Annex A inclusion and exclusion is traceable to a testable control (chapter 05).
4. Author the AIMS *risk-treatment plan* as the crossroads of the risk process, the SoA, and the AIA outputs (chapter 06).
5. Design the AIMS *support* layer (competence, awareness, communications, documented information) so the operating tier (head of AI governance and below) can run the system without re-authoring it (chapter 07).
6. Design the AIMS *performance-evaluation* layer (internal audit, management review) as the operations calendar the head of AI governance runs and as the input pipe for continual improvement (chapters 08–09).
7. Integrate the AIMS with the enterprise ISMS so the enterprise runs *one* management system with an AI facet, not two management systems with duplicated overhead (chapter 10).
8. Read ISO/IEC 42006 in reverse to design an AIMS that a third-party certification body can audit without needing to hunt for evidence (chapter 11).

Each of the six inseparable artefacts named above will be authored explicitly by the end of the module. Each will be positioned against the specific clause of Annex SL it satisfies. Each will be tied to the sibling standards it composes with. The exercises are where the architect drafts the artefacts against a specific enterprise scenario; the chapters are the reasoning they run on.

## Summary

The AIMS is a *management system* in the Annex SL sense — a defined set of clauses satisfied by six inseparable artefacts (scope, SoA, risk-and-impact-assessment process + risk-treatment plan, competence and communications plans, internal audit programme + management review, non-conformity and corrective-action process) that together let a third-party certification body audit conformance under ISO/IEC 42006. The architect owns the shape of the system, not the day-to-day operation, and hands the operating model to the head of AI governance to run. The system composes with a specific stack of sibling standards — 22989 for vocabulary, 23053 for the ML reference architecture, 31000 for the risk-management parent, 23894 for AI-specific risk guidance, 42005 for AI impact assessment, 42006 as the auditor's rulebook, 27001 as the sibling management-system, 38507 as the governance-body layer above, and the 5259 series for data-quality — and re-inventing any of these locally is a failure mode. The next ten chapters build the AIMS clause by clause and land in an integrated management system that a certification body can audit and the head of AI governance can operate.
