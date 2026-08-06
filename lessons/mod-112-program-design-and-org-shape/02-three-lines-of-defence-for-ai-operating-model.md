# The three lines of defence as an operating model — RACI at each control

## Why this chapter exists

Module 107 designed the *assurance architecture* — the three-lines structure, its independence invariants, the pre-deployment gate, the ongoing assurance cadence, the third-line internal audit programme, the external-audit interface. What that module did not do — because it was outside its scope — is walk the operating model that stands the three lines *up as an organisation*. The assurance architecture answers "who inspects whom?"; the operating model answers "who does what work at each control the enterprise implements, in a way that a new hire can be onboarded into and a departing seat can be handed off from without every attestation lapsing."

The level-50 architect who has ratified the mod-107 assurance architecture and stood up the AI governance council (chapter 01 of this module) now composes the operating model that terminates on the enterprise's actual seats and the actual work products the seats produce. The composition target is the enterprise's control library (mod-102) — the operating model must name, for every control family, which seat carries which responsibility so that the mod-102 evidence contract (mod-108) has an unambiguous owner-of-record at every point of authorship, review, and audit.

The failure mode this chapter designs against is the operating-model gap between an assurance architecture (the shape) and the enterprise's actual seat topology (the workforce). An assurance architecture that names "the second line" and "the first line" without naming which seat inside the second line owns which control-family review is only half-designed; when the external auditor asks "who reviewed the fairness attestation on system SYS-2027-0042?", the enterprise cannot answer because the shape names a line, not a seat. This chapter closes the gap.

## The operating-model artefact — what it is and what it is not

The operating model is a **seat-and-work-product RACI** ratified against the enterprise control library, tied to the mod-107 three-lines shape, and re-ratified whenever the control library increments or the seat topology changes. It is *not*:

- **Not the org chart.** Org charts show reporting lines and headcount. RACIs show accountability for work products. The two overlap but are not the same artefact — an operating model that is just an org chart cannot answer "who reviews the mod-102 `AIC-FAIR-*` control-family attestations?" because reporting lines do not name work products.
- **Not the mod-107 assurance architecture.** That architecture names the three lines and their invariants; this operating model composes the invariants against the enterprise's actual seats. The architecture is line-typed (first, second, third); the operating model is seat-named.
- **Not the mod-102 control library.** The control library names the controls, the evidence contract, and the applicability filter. This operating model *terminates on* the control library with a RACI for every control family, but it does not author the controls themselves.
- **Not the level-15 analyst's operational runbook.** The analyst's runbook is a per-task procedure; the operating model is a structural artefact ratified at the level-50 architect's level with the council's endorsement.

The operating model is what makes the assurance architecture *executable*. It is what the head-of-AI-governance (level 60) hands the CFO when the CFO asks "why do we need this many seats?"; it is what the audit committee reads to satisfy itself that the mod-107 invariants are structurally enforced; it is what a new joiner reads to know exactly what their seat is accountable for and where their work handoffs go.

## The RACI convention this operating model uses

The chapter uses the classical RACI convention with an AI-specific specialisation.

- **R (Responsible)** — the seat that *does* the work. There may be more than one R per work product where the work is genuinely shared; the operating model discourages this and prefers a single-R model where possible.
- **A (Accountable)** — the seat that is *accountable* for the work product's quality and existence. There is exactly one A per work product; multiple As is a defect in the RACI. A cannot equal R for second-line-quality-gated work products (i.e., the seat doing the work is not the seat accountable for the work's independence-of-review), and A cannot equal R across the first/second-line boundary for the pre-deployment gate outputs (mod-107 chapter 02).
- **C (Consulted)** — the seats that are *consulted* before the work product is finalised. Consultation is a two-way exchange; C's input is expected to influence the work product.
- **I (Informed)** — the seats that are *informed* of the work product's completion. I is a one-way notification; I's input is not expected to influence the work product.

The AI-specific specialisation is a fifth designation:

- **X (eXternal)** — a seat that is *external to the enterprise* (a certification body, an independent auditor, a sector regulator's examiner, a customer-facing assurance recipient, a frontier-model provider). X is not staffed by the enterprise; the operating model names X so that the enterprise-side coordination role is explicit and the interface's cadence is defensible under audit.

The RACI is authored at the control-family level (mod-102's `AIC-*` families) for the ongoing operating work, and at the individual-artefact level for the pre-deployment gate outputs and the AIMS documented-information artefacts. The two granularities compose — the control-family RACI carries the ongoing shape; the artefact-level RACI carries the point-in-time gate and management-review shape.

## The seat inventory — the operating model's terms of reference

The operating model names its seats in a canonical inventory so that the RACI is unambiguous. The inventory below is the shape most senior-AI-governance-architect deployments carry; individual enterprises tailor to their vocabulary but the shape generalises. Each seat is named with its role-tree level and the role packet it composes against; chapter 03 walks the role packets in detail.

### First-line seats

- **Model owner / system owner** — the accountable seat for the specific AI system's operation. Named per system, not per team. Often a senior engineer or product manager.
- **Product owner** — the accountable seat for the product the AI system sits inside. Distinct from model owner where products contain multiple AI systems.
- **Platform lead** — the seat that owns the enterprise's ML platform (registry, feature store, evaluation harness, serving, monitoring). Named once per platform; supplies platform-level evidence for many systems.
- **MLOps lead** — the seat that owns the deployment pipelines, drift monitors, and model observability instrumentation. Distinct from platform lead where the enterprise separates concerns.
- **Data engineering lead** — the seat that owns the data pipelines feeding AI systems. Named once per pipeline; supplies data-lineage evidence.
- **Enterprise IAM lead** — the seat that owns the enterprise identity provider and the SCIM provisioning (mod-111 chapter 01). Named as first-line for the identity control-family that the RBAC + SoD model depends on.

### Second-line seats

- **Senior AI governance architect (level 50, this role).** Owns the design artefacts — the control library (mod-102), the policy taxonomy (mod-103), the jurisdiction-reconciled control set (mod-104), the AIMS (mod-105), the risk taxonomy (mod-106), the assurance architecture (mod-107), the evidence architecture (mod-108), the third-party programme (mod-109), the PMS shape (mod-110), the GRC-for-AI reference architecture (mod-111), the operating model this chapter authors, and the sector blueprints (mod-113).
- **Head of AI governance (level 60).** Owns the AIMS operations at the enterprise level; carries the audit-committee reporting, the regulator engagement, and the budget accountability. Chapter 08 walks the boundary.
- **Ai governance analyst (level 15).** Owns operational analyst work — intake, inventory, framework crosswalk drafts, first-draft impact assessments, cards at analyst tier, control tracking, jurisdictional tracking. Named as second-line for the operational execution of the mod-102 control-testing programme.
- **Ai risk engineer (level 25).** Owns hands-on engineering craft of AI risk — harm-model authoring, red-team / adversarial-ML / fairness / privacy / guardrail engineering, quantification, monitoring-to-risk-register wiring, incident RCA. Named as second-line for the mod-106 risk-quantification work.
- **Ai evaluation engineer (level 35).** Owns release-assurance and audit-trail methodology — pre-deployment gates, audit-facing evidence packs, regulator-facing artefact production inside the assurance system. Named as second-line for the mod-107 pre-deployment gate execution and the mod-108 evidence-artefact production.
- **Agentic safety engineer (level 40).** Owns frontier-agent red-team methodology and dangerous-capability evaluation. Named as second-line for the agentic-system control families (`AIC-AGENT-*`) and for dangerous-capability evaluation on frontier models.
- **Ai-infra-security seat (level 35 in this track's family map).** Owns platform-scale defence — the runtime-security adjacencies (mod-111 chapter 05) and the SOC interface (mod-110 chapter 05). Named as second-line for the mod-102 security control-family (`AIC-SEC-*`).
- **Chief privacy officer / DPO office.** Owns the DPIA composition with the AIA (mod-105 chapter 04) and the mod-102 privacy control-family (`AIC-PRIV-*`).
- **Model risk management (MRM) function.** Where SR 11-7 applies — banking, insurance in some jurisdictions — the classical MRM function named in the mod-107 chapter 01 second-line specification. The overlap resolution with the AI evaluation engineer is decided in mod-107 exercise-01 and inherited here.

### Third-line seats

- **Internal audit lead (AI scope).** Owns the third-line audit programme (mod-107 chapter 04). Functional reporting to the audit committee.
- **Internal audit engagement team** — the seat set on the specific engagement. Sampling authority into first- and second-line evidence.
- **Co-source partner** — where the internal audit function has a gap in AI competence, the named external partner (an accounting-firm advisory practice or a specialised AI-audit provider) filling the gap. Named as X in the RACI.

### External seats

- **ISO/IEC 42001 certification body** — the audit body accredited under ISO/IEC 42006 the enterprise engages for its AIMS certification. Named X.
- **ForHumanity Independent AI Auditor** — where the enterprise engages a ForHumanity-accredited independent auditor for a jurisdiction that recognises the credential or for internal-assurance ambition beyond certification. Named X. `<!-- needs-research: verify current recognition status of ForHumanity Independent AI Audit / Assurance Standard (IAAIS) across US, EU, UK jurisdictions -->`
- **Sector regulator examiner** — Federal Reserve examiner, PRA supervisor, state insurance-commissioner examiner, FDA reviewer, competent authority under EU AI Act Article 26 (depending on sector and jurisdiction).
- **Statutory financial auditor** — where AI-related activity touches material financial reporting.
- **Frontier-model provider audit / attestation team** — where the enterprise consumes third-party attestations (SOC 2, ISO 27001) from providers, the enterprise-side coordination seat that consumes the attestations (mod-109).
- **Customer-facing assurance recipient** — for B2B SaaS enterprises, the customer's own second-line function that consumes the vendor's assurance report.

## The control-family RACI — the operating model's core table

The control-family RACI is the operating model's central artefact. The table below is illustrative of the shape; each enterprise fills in its own control-family names against its own seat inventory.

| Control family | R (does the work) | A (accountable) | C (consulted) | I (informed) | X (external) |
|---|---|---|---|---|---|
| `AIC-DATA-*` (data governance, provenance, quality) | model owner + data engineering lead | ai-governance-analyst (mod-108 evidence write) | ai-risk-engineer, cpo/dpo | head-of-ai-governance | 42001 certifier |
| `AIC-FAIR-*` (fairness, bias, non-discrimination) | model owner + ai-risk-engineer (independent scoring) | ai-evaluation-engineer (release-assurance methodology) | senior-ai-governance-architect, cpo/dpo, gc | head-of-ai-governance, ai-governance-council | forhumanity auditor (where engaged) |
| `AIC-ROB-*` (robustness, adversarial, distribution shift) | model owner + ai-risk-engineer | ai-evaluation-engineer | agentic-safety-engineer (where applicable) | head-of-ai-governance | 42001 certifier |
| `AIC-SEC-*` (AI-specific security, prompt-injection, jailbreak, model-supply-chain) | model owner + platform lead | ai-infra-security lead | ciso, ai-risk-engineer | head-of-ai-governance, ai-governance-council | frontier-model-provider attestation |
| `AIC-EXPL-*` (explainability, interpretability, contestability) | model owner + product owner | ai-evaluation-engineer | cpo/dpo, gc | head-of-ai-governance | forhumanity auditor |
| `AIC-PRIV-*` (privacy, DPIA, data-minimisation) | model owner + data engineering lead | cpo/dpo | ai-governance-analyst, gc | head-of-ai-governance | 42001 certifier, DPA (where applicable) |
| `AIC-HITL-*` (human oversight, human-in-the-loop, contestability process) | product owner + model owner | ai-evaluation-engineer | senior-ai-governance-architect, gc, cpo/dpo | head-of-ai-governance, ai-governance-council | sector regulator (where applicable) |
| `AIC-AGENT-*` (agentic-system control family — tool use, autonomy, scaffolding) | model owner + platform lead | agentic-safety-engineer | ai-risk-engineer, ai-evaluation-engineer, ai-infra-security | head-of-ai-governance, ai-governance-council | 42001 certifier |
| `AIC-3PP-*` (third-party AI, frontier-provider adjacency, vendor risk) | product owner + procurement + platform lead | senior-ai-governance-architect (mod-109 owner) | gc, ciso, cpo/dpo, ai-risk-engineer | head-of-ai-governance, ai-governance-council | provider (attestation), 42001 certifier |
| `AIC-PMS-*` (post-market surveillance, incident-response) | mlops lead + model owner + soc | ai-evaluation-engineer (methodology); head-of-ai-governance (Article 73 serious-incident reporting) | ciso, ai-risk-engineer, gc | ai-governance-council | sector regulator, competent authority |
| `AIC-DOC-*` (documentation — model card, system card, technical documentation) | model owner + product owner + ai-governance-analyst | senior-ai-governance-architect (schema); model owner (contents) | ai-evaluation-engineer, cpo/dpo | head-of-ai-governance | 42001 certifier, competent authority |
| `AIC-IAM-*` (identity, RBAC, segregation of duties on the GRC-for-AI platform) | enterprise iam lead | senior-ai-governance-architect (co-owned with iam) | head-of-ai-governance, ciso | audit committee | 42001 certifier, internal audit |

Two disciplines the table makes visible.

**A is never R for the same control-family attestation.** The seat doing the work does not attest to its own independence-of-review. `AIC-FAIR-*` shows the pattern: the model owner and the ai-risk-engineer are R (they do the fairness engineering and the independent scoring), and the ai-evaluation-engineer is A (they own the release-assurance methodology that binds R's output to a defensible attestation). This is mod-107 chapter 01's invariant-1 (independence per line) enforced at the seat level.

**The `senior-ai-governance-architect` is A on the schemas, C on the attestations, I on operational execution.** The architect designs the shape (the schema in `AIC-DOC-*`, the platform IAM model in `AIC-IAM-*`, the third-party programme in `AIC-3PP-*`), consults on family-level design questions in `AIC-FAIR-*` and `AIC-HITL-*` where policy and ethics considerations bear, and receives notifications of operational execution. The architect is not the operational executor of anything on this table — chapter 03 walks the hiring plan that populates the seats that are.

## The pre-deployment gate RACI — artefact-level

The pre-deployment gate (mod-107 chapter 02) produces artefact-level RACIs at each gate meeting. The operating model composes with the gate's charter to specify:

| Gate artefact | R | A | C | I |
|---|---|---|---|---|
| Evidence-contract discharge check | ai-governance-analyst | ai-evaluation-engineer | senior-ai-governance-architect | head-of-ai-governance |
| Sequenced review — technical | ai-evaluation-engineer + ai-risk-engineer | ai-evaluation-engineer | agentic-safety-engineer (where applicable), ai-infra-security | head-of-ai-governance |
| Sequenced review — legal / privacy | gc-nominee + cpo/dpo | gc | senior-ai-governance-architect | head-of-ai-governance |
| Sequenced review — residual against appetite | ai-risk-engineer | ai-evaluation-engineer | senior-ai-governance-architect (portfolio) | head-of-ai-governance, ai-governance-council |
| Sequenced review — third-party posture | senior-ai-governance-architect (mod-109 owner) | senior-ai-governance-architect | procurement, gc | head-of-ai-governance |
| Gate decision-record signature | ai-evaluation-engineer (chair) + tier-signatory set | ai-evaluation-engineer (chair) | senior-ai-governance-architect | head-of-ai-governance, model owner, product owner |

The gate decision-record's signature block is the exception to the single-A rule — multiple signatories carry accountability for their own sign-off scope, and the ai-evaluation-engineer as chair carries accountability for the meeting having run to charter.

## The AIMS management review RACI — artefact-level

The AIMS Clause 9.3 management review (mod-105 chapter 07) produces the annual management-review-record. The operating model specifies:

| Management-review artefact | R | A | C | I |
|---|---|---|---|---|
| Management-review input pack (Clause 9.3.2) | head-of-ai-governance + senior-ai-governance-architect | head-of-ai-governance | ai-governance-analyst | ai-governance-council |
| Management-review meeting (agenda item at council quarterly) | chair + head-of-ai-governance | chair (of ai-governance-council) | voting members, ciso, cpo/dpo, gc | audit committee (via cia), 42001 certifier |
| Management-review record (Clause 9.3.3) | ai-governance-analyst (drafter) + head-of-ai-governance (reviewer) | chair | senior-ai-governance-architect | audit committee, 42001 certifier |
| Management-review-triggered changes to AIMS | senior-ai-governance-architect | head-of-ai-governance | affected first-line seats | ai-governance-council |

## Invariants — testable operating-model discipline

Six invariants hold across the operating model; each is testable, and the model is defective when any of them fails.

**Invariant 1 — every control-family and every named artefact has a single A.** The RACI table has exactly one A per row for the ongoing shape; multiple As is a drafting defect. Failure mode: two seats each think the other owns the fairness attestation; when the certification body samples, the enterprise cannot point to one.

**Invariant 2 — A is not R for second-line-quality-gated work.** For control-family attestations, the seat accountable is not the seat doing the work; for gate outputs, the accountable seat is not one of the first-line signatories. Failure mode: the first-line signatory attests to the second-line review's completeness; the auditor spots the inversion.

**Invariant 3 — every X is coordinated by a named enterprise-side seat.** No external counterparty appears in the RACI without an enterprise-side coordinator. Failure mode: the certification body's engagement is coordinated ad-hoc; the certifier's information requests reach whoever they met last time.

**Invariant 4 — every seat named is staffed or has a named plan-to-staff.** A seat that appears in the RACI without a filled role is either currently staffed or has a hiring / co-source plan with an owner and a timeline. Chapter 03 walks the hiring plan. Failure mode: the RACI names an ai-evaluation-engineer as A on release-assurance, but the enterprise has not hired one; the attestation cannot be produced.

**Invariant 5 — the RACI composes with mod-105 documented-information.** Every artefact named in the RACI as a work product corresponds to an AIMS documented-information item (per mod-105 chapter 05). Failure mode: the operating model names artefacts the AIMS does not; the certification body finds documented-information gaps.

**Invariant 6 — the operating model is versioned and re-ratified.** Adding a seat, retiring a seat, moving accountability across the first/second-line boundary, or introducing a new X counterparty is a versioned change ratified by the AI governance council (chapter 01) as a reserved-matter charter amendment. Failure mode: the RACI on paper drifts from the RACI in practice; the auditor finds the divergence at the next audit.

## The operating-model schematic — YAML shape

```yaml
operating_model:
  id: OPMOD-v1.0
  ratified_by: ai-governance-council (per mod-112 ch 01 charter)
  composes_with:
    - mod-102 control library
    - mod-105 AIMS documented information
    - mod-107 assurance architecture (three lines)
    - mod-111 GRC-for-AI reference architecture

  seat_inventory:
    first_line:
      - model-owner
      - product-owner
      - platform-lead
      - mlops-lead
      - data-engineering-lead
      - enterprise-iam-lead
    second_line:
      - senior-ai-governance-architect (level 50)
      - head-of-ai-governance (level 60)
      - ai-governance-analyst (level 15)
      - ai-risk-engineer (level 25)
      - ai-evaluation-engineer (level 35)
      - agentic-safety-engineer (level 40)
      - ai-infra-security (level 35)
      - cpo-or-dpo-office
      - mrm-function (where SR 11-7 applies)
    third_line:
      - internal-audit-lead (ai scope)
      - internal-audit-engagement-team
      - co-source-partner (where required)
    external:
      - iso-42001-certification-body
      - forhumanity-independent-auditor
      - sector-regulator-examiner
      - statutory-financial-auditor
      - frontier-model-provider (attestation-consumer interface)
      - customer-facing-assurance-recipient (b2b saas)

  control_family_raci: <see chapter table>
  pre_deployment_gate_raci: <see chapter table>
  aims_management_review_raci: <see chapter table>

  invariants:
    - id: I1
      description: single A per row for ongoing-shape rows
      test: sample the RACI; find multiple As on any row
    - id: I2
      description: A is not R for second-line-quality-gated work products
      test: sample AIC-FAIR-*, AIC-ROB-*; check A != R
    - id: I3
      description: every X has an enterprise-side coordinator
      test: sample X entries; find the coordinator
    - id: I4
      description: every seat is staffed or has a plan-to-staff
      test: cross-check against chapter 03 hiring plan
    - id: I5
      description: RACI composes with mod-105 documented information
      test: sample RACI work products; find AIMS documented-information items
    - id: I6
      description: operating model is versioned and council-ratified
      test: check version history; each amendment cites a council minute
```

## Two failure modes to design against

**Failure mode 1 — the shape-without-seats operating model.** The enterprise ratifies a mod-107-shaped assurance architecture and stops there. When the certification body asks "who is the seat that reviewed the fairness attestation for SYS-2027-0042?", the enterprise names "the second line" and the certifier notes the finding. The architectural defence is *artefact-level RACI resolution* — every control family and every named gate artefact has a seat-level R and A, not a line-level R and A.

**Failure mode 2 — the R-equals-A collapse.** A well-intentioned attempt to reduce process overhead in an operating model draft names the same seat as R and A for control-family attestations. The model owner is R because they do the work, and A because they are the accountable engineering lead. Under mod-107 chapter 01's independence invariant, the RACI is broken — the seat doing the work is attesting to its own independence-of-review. Under external audit, the finding is systemic. The architectural defence is *invariant 2 tested at ratification* — every A that equals R for a second-line-quality-gated row is a defect the council does not ratify.

Both failure modes are architectural. Neither is prevented by charter language alone; the invariant tests are what make the model operate.

## Coordination — the roles the operating model interfaces with

- **AI governance council (chapter 01)** — ratifies the operating model at each version, including seat additions, retirements, and cross-boundary moves.
- **Head of AI governance (level 60)** — carries programme-level accountability for the seats named in the model; owns the hiring plan (chapter 03) at the enterprise level.
- **Chief people officer / CHRO** — carries the workforce plan the hiring plan lands on; owns the change-management plan for seat introductions (chapter 05).
- **Enterprise IAM lead** — carries the RBAC + SoD model (mod-111 chapter 04) that the operating model's seat inventory drives.
- **Internal audit** — samples the operating model at the annual engagement to test the six invariants.
- **The mod-102 control library** — the operating model terminates on the control library's family list; every family has a RACI row.
- **The mod-105 AIMS documented information** — every work product in the RACI corresponds to a documented-information item.

## Summary

The operating model composes the mod-107 assurance architecture (three lines, independence invariants) with the enterprise's actual seat topology into a seat-and-work-product RACI that has a single A per work product and a defined R, C, I, and (where relevant) X. The core artefact is the control-family RACI keyed against the mod-102 `AIC-*` families; the pre-deployment gate RACI and the AIMS management-review RACI are artefact-level companions. The seat inventory names first-line seats (model owner, product owner, platform lead, MLOps, data engineering, enterprise IAM), second-line seats (senior AI governance architect, head of AI governance, AI governance analyst, AI risk engineer, AI evaluation engineer, agentic safety engineer, AI-infra-security, CPO/DPO, MRM), third-line seats (internal audit lead, engagement team, co-source partner), and external seats (certification body, ForHumanity, sector regulator, statutory auditor, frontier-model provider, customer-facing assurance recipient). Six invariants hold — single A per row, A ≠ R for second-line-quality-gated work, X has enterprise-side coordinator, every seat staffed or plan-to-staff, RACI composes with AIMS documented information, model is versioned and council-ratified. Two failure modes — shape-without-seats and R-equals-A collapse — are what the invariants defend against. The chapter's schematic is what exercise-02 fills in and defends; the operating model is the artefact chapter 03 hires against and chapter 05 change-manages through the enterprise when it evolves.
