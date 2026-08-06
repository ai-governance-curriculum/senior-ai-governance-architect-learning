# Risk and impact assessment — composing ISO 31000, ISO/IEC 23894, ISO/IEC 42005, and 42001 Clause 6.1

## Why this chapter exists

The AIMS's *risk process* — the thing that identifies AI risks, analyses them, evaluates them against criteria, and hands the outputs to the risk-treatment plan — is not something 42001 defines from scratch. 42001's Clause 6.1 is a specialisation, in the AI context, of a general risk-management pattern already codified in ISO 31000:2018. ISO/IEC 23894:2023 sits between them, translating 31000 into AI-specific guidance. And ISO/IEC 42005 provides the *impact-assessment* process — the AI-specific analogue to a DPIA — that Clause 6.1.4 requires. Getting the composition right is what separates an AIMS with a real risk process from one with a risk *document*.

The architect's job in this chapter is to design the composition explicitly: how the enterprise's ISO 31000-shaped risk-management framework carries the AI risk process; how the AI risk process picks up 23894's AI-specific taxonomy and guidance; how the AIA process under 42005 hooks in as a first-class input; and how the outputs flow into Clause 6.1.3 for treatment. The chapter avoids saying "run a risk workshop" — that is what a facilitator does — and focuses on the *artefact shapes*, the *interfaces*, and the *ownership assignments* the architect designs.

## The four standards, positioned

**ISO 31000:2018 — Risk management — Guidelines.** The generic parent. It defines the risk-management framework (mandate, integration, design, implementation, evaluation, improvement) and the risk-management process (scope, context, criteria; risk identification; risk analysis; risk evaluation; risk treatment; recording and reporting; monitoring and review; communication and consultation). It is *guidance*, not requirements; not certifiable on its own. Most enterprises' ERM function has already adopted an ISO 31000-shaped framework, and the AIMS's risk process composes with it rather than forking.

**ISO/IEC 23894:2023 — Information technology — Artificial intelligence — Guidance on risk management.** The AI-specific companion. Written as guidance layered on top of 31000. Carries an AI-specific risk-source taxonomy (unintended bias in training data, model drift, adversarial manipulation, over-reliance on AI outputs, opacity of decision-making, misuse of AI capabilities, dependency on third-party AI, data leakage from generative models, unsafe emergent behaviour, and so on), AI-specific factors to consider during analysis (impact severity to individuals and groups, reversibility, scale of deployment, autonomy of the system, degree of human oversight), and AI-specific treatment considerations. Not certifiable; used as *guidance* alongside 42001. Paywalled ISO/IEC deliverable.

**ISO/IEC 42005:2025 — AI system impact assessment.** The AI-specific process standard for impact assessments — analogous to the GDPR's DPIA process, but broader in scope and specific to AI. Structures the AIA around: what is being assessed (system description, purpose, deployment context), who is affected (individuals, groups, society, environment), what impacts are foreseeable (positive and negative, direct and indirect, first-order and downstream), how the impacts are analysed and evaluated, and what mitigations are proposed and tracked. The AIA is a documented artefact per system; the process is a defined enterprise capability. <!-- needs-research: confirm final publication year and any structural specifics of ISO/IEC 42005 against the published text. -->

**ISO/IEC 42001:2023 Clause 6.1.** The 42001 clause the composition satisfies. Clause 6.1.1 sets the frame (risks and opportunities). Clause 6.1.2 specifies the AI risk assessment (identify, analyse, evaluate against criteria). Clause 6.1.3 specifies the AI risk treatment (select options, determine controls, produce the SoA, formulate the risk-treatment plan). Clause 6.1.4 specifies the AI system impact assessment — where 42005 attaches. <!-- needs-research: reconfirm the exact sub-clause numbering in the published ISO/IEC 42001:2023. -->

## Composition, in order — the four-layer stack

The composition is a stack, and the direction of composition matters.

**Layer 1 — ISO 31000 supplies the framework.** The enterprise ERM programme defines the risk-management framework — mandate, integration into governance structures, resources, evaluation. The AIMS's risk process operates *inside* this framework, not alongside it. Concretely, the AIMS risk process reports up to the enterprise Chief Risk Officer's function (or equivalent) using the risk categories, likelihood-and-impact scales, and appetite thresholds the enterprise ERM function has set. Where the ERM framework has gaps for AI-specific concerns (novel harm categories, unfamiliar likelihood shapes), the AIMS proposes extensions to the ERM function, who owns them.

**Layer 2 — ISO/IEC 23894 supplies the AI-specific risk taxonomy and guidance.** The AIMS's risk-identification step uses the 23894 risk-source taxonomy as its input. When the risk-identification workshop for a new AI system starts, the facilitator (typically an AI-risk engineer at level 25) walks the 23894 taxonomy and asks, for each source, "does this apply to the system under review, and if so, at what materiality?" This is what turns a generic risk workshop into an AI-specific one. Similarly, when risks are *analysed*, 23894's guidance on impact factors (severity to individuals, scale, reversibility, degree of autonomy, opacity) shapes the analysis criteria.

**Layer 3 — ISO/IEC 42005 supplies the impact-assessment methodology.** The AIA process (Clause 6.1.4) is *shaped by* 42005. It is not just another risk assessment — it is an *impact* assessment, meaning its subject is the foreseeable consequences on individuals, groups, and society, and its purpose is to surface concerns early enough to translate into mitigations. AIA outputs feed into the risk register (Layer 2) and into the risk-treatment plan (Layer 4).

**Layer 4 — ISO/IEC 42001 Clause 6.1 supplies the AIMS-specific requirements.** 42001 requires that the risk process is *documented*, *maintained*, *repeatable*, and *produces an SoA and a risk-treatment plan*. It requires the assessment to consider — at minimum — the risks the AI systems in scope pose to the organisation, the risks they pose to others, and the risks arising from the AIMS itself (e.g. inadequate resourcing). The AIMS-specific requirements are how the stack lands on a certifiable artefact.

## The risk-management process, layer by layer

Applied to the AIMS's risk process, the ISO 31000 sequence looks like this. Each step names *who* does it, *what* they produce, and *how* the AI-specific standards specialise it.

**Step 1 — Communication and consultation.** Continuous throughout the process. The AIMS communications plan (Clause 7.4) specifies the channels; the interested-parties register (Clause 4.2) specifies the audiences.

**Step 2 — Scope, context, and criteria.** For each risk-assessment cycle, the scope is set by Clause 4.3 (the AIMS scope) further narrowed to the system(s), portfolio segment, or organisational unit under assessment. The context is inherited from Clause 4.1 (external and internal issues) and Clause 4.2 (interested parties). The *criteria* — the likelihood and impact scales and the risk appetite thresholds — come from the enterprise ERM framework, extended for AI-specific factors per 23894. Mod-106 will walk the AI risk taxonomy and the appetite architecture in detail; the point at Clause 6.1 is that the criteria *exist* and are *documented*.

**Step 3 — Risk identification.** For each system or portfolio segment in scope, the facilitator (AI-risk engineer) walks the 23894 risk-source taxonomy plus the enterprise-specific extensions. Outputs are logged as candidate risks in a risk register with a stable identifier, a description, the system(s) affected, the risk source, and the initial owner.

**Step 4 — Risk analysis.** For each identified risk, the facilitator estimates likelihood and impact using the criteria from Step 2. AI-specific factors from 23894 — reversibility, degree of autonomy, scale of deployment, opacity, presence and effectiveness of human oversight — shape the impact estimate. Where the risk touches individuals or groups in a material way, the AIA (Layer 3) is triggered and its output feeds the analysis for those risks.

**Step 5 — Risk evaluation.** Each analysed risk is compared to the risk appetite thresholds. The output is a decision: acceptable as-is; requires treatment; requires escalation (i.e. exceeds appetite). Escalated risks go to the AI-accountable executive (or to the enterprise CRO under the ERM framework, depending on where the AI-specific appetite thresholds are set).

**Step 6 — Risk treatment.** Chapter `06-risk-treatment-plan-and-operational-clauses.md` walks this in detail. The output at this step is the *risk-treatment plan* and the *SoA*.

**Step 7 — Monitoring and review.** The risk register is refreshed on a defined cadence — typically at least annually, with event-driven refreshes on system onboarding, material system change, incident, regulatory change, or audit finding. Refreshes are logged; the internal audit programme samples the refresh log.

**Step 8 — Recording and reporting.** The AIMS documented-information set (Clause 7.5) includes the risk register, the risk-treatment plan, the SoA, and the AIA records. Reporting upward — to the management review, to the ERM function, to the governance body under 38507 — follows the communications plan.

## The AIA — how 42005 attaches

The AIA is the artefact that carries the most weight in a Clause 6.1.4 audit. The architect designs the AIA *process* — the trigger, the authoring, the review, the output shape, and the wire-up to the risk-treatment plan.

**Trigger.** The AIA fires on a defined trigger. The three triggers to design for:

1. *New AI system in scope of the AIMS is being considered for development or deployment.* The AIA is authored during the concept-of-operations stage, before development commits. Cross-reference: mod-103 chapter 06 (IEEE 7000 as methodology) supplies the concept-of-operations ethics-review methodology, whose output feeds the AIA.
2. *Existing AI system in scope undergoes a material change.* Material change is defined explicitly — new use case, new user population, model architecture change beyond a threshold, training-data source change, new deployment jurisdiction. Every material change re-fires the AIA (or an incremental re-assessment).
3. *An event occurs that changes the impact context.* An incident, a regulatory change, a discovered emergent behaviour, a change in the third-party model the system depends on. The AIMS's incident-management process (mod-110) triggers a re-assessment.

**Author.** The AIA is *authored* by the AI-risk engineer (level 25) who owns the system, with contributions from product management (business context), legal (regulatory context), the AI-evaluation engineer (level 35 — technical performance and known limitations), and stakeholders representing the affected populations where reasonably obtainable. The head of AI governance is a *reviewer*, not the author.

**Review.** The AIA is reviewed by a *risk-and-impact review committee* — a standing forum chaired by the head of AI governance, with representation from legal, security, privacy, ethics (where the enterprise has an ethics function), and the relevant business unit's head of risk. The review either accepts the AIA and its proposed mitigations, requires revision, or escalates to the AI-accountable executive.

**Output shape.** The AIA output is a documented record with, at minimum:

```yaml
aia_id: AIA-2026-047
system_id: SYS-CHAT-001
system_name: Customer-facing generative assistant (external)
version_of_system_under_assessment: v3.2.0
authored_by: AI-risk engineer L. Nguyen (level 25)
authored_at: 2026-03-14
review_committee: risk-and-impact review committee
reviewed_at: 2026-04-02
review_outcome: accepted-with-conditions

system_description:
  purpose: External customer-service assistant handling product-support and account-management enquiries.
  deployment_context: EU-27, UK, US; ~2.4M monthly active users; ~90M interactions/month.
  autonomy: assistive; final actions taken by user; agentic workflows disabled for accounts-affecting changes.
  human_oversight: real-time monitoring by service-operations team; escalation to human agent on trigger keywords, model-uncertainty threshold, and user request.

affected_parties:
  - Customer end-users (EU-27, UK, US).
  - Customer-service agents (workflow displacement risk).
  - Non-user affected persons (family members impersonated in social-engineering attempts).

foreseeable_impacts:
  - id: IMP-01
    category: incorrect information provided to user (hallucination)
    directness: direct
    severity: moderate (financial or process consequence in edge cases)
    likelihood: material (LLMs hallucinate at non-negligible rates on out-of-distribution queries)
    reversibility: reversible in most cases; irreversible where user acts on incorrect advice before correction.
    scale: population-wide; per-interaction low probability, but 90M interactions/month means aggregate exposure is material.
    mitigations_proposed:
      - Retrieval augmentation grounded in enterprise product KB.
      - Grounded-answer requirement — model instructed to say "I don't know" on out-of-context queries.
      - Continuous hallucination monitoring per mod-110.
      - Escalation-to-human on user disagreement.
      - Transparency notice on every session that responses may be incorrect and users should verify important information.

  - id: IMP-02
    category: bias in resolution quality across customer segments
    directness: direct
    severity: material (potential systemic disadvantage to a customer segment)
    likelihood: low-to-material (bias documented in similar deployments; unknown until measured in our data)
    reversibility: reversible with model correction; the harm to affected customers may not be.
    scale: population-wide; per-interaction low, but if a bias exists it is persistent until corrected.
    mitigations_proposed:
      - Bias evaluation on the deployed system's actual traffic across demographic proxies allowed by data-protection law (mod-108 evidence contract for bias measurement).
      - Quarterly review by AI-evaluation engineer with report to risk-and-impact review committee.
      - Bias remediation path defined (retraining, prompt-engineering, escalation-rule adjustment) with SLA.

# ... additional impacts as identified
        
risks_registered:
  - AI-RISK-2026-089 — hallucination-driven customer harm.
  - AI-RISK-2026-090 — cross-segment bias in resolution quality.

risk_treatment_plan_links:
  - RTP-2026-Q2-014 — implements IMP-01 mitigations.
  - RTP-2026-Q2-015 — implements IMP-02 mitigations.

conditions_of_acceptance:
  - Complete first bias evaluation and file with committee by 2026-06-30.
  - Reduce grounded-answer refusal rate below 5% within 90 days or return to committee.

next_review_trigger:
  - Material change per definition, or 2027-04-02 (annual), whichever earlier.
```

The AIA is neither a marketing document nor a legal memo. It is an *engineering artefact with legal and ethical content* — precise about impacts, honest about uncertainty, explicit about mitigations, and traceable to the risk register and the risk-treatment plan.

**Wire-up to the risk-treatment plan.** Every material impact identified in the AIA produces at least one entry in the risk register. Every risk register entry produced by an AIA is linked to the AIA's identifier. Every mitigation the AIA proposes appears as a treatment in the risk-treatment plan, linked back to the AIA and forward to the Annex A controls it operationalises. The linkage is what turns AIAs from decorative artefacts into risk-treatment inputs.

## Two failure modes in the risk-and-impact composition

**Failure mode 1 — the AIA that lives beside the risk register instead of feeding it.** The enterprise stands up an AIA process because 42001 Clause 6.1.4 requires one. AIAs get authored, reviewed, filed. The risk register, meanwhile, is populated from a separate risk workshop that never reads the AIAs. The two artefacts drift; the AIA identifies impacts the risk register does not carry; the risk register carries risks the AIA never surfaced. Auditors find the seam within an hour. The fix is the *wire-up* — every AIA impact must trace to a risk register entry, and the risk register must have a field pointing back to the AIA that identified it.

**Failure mode 2 — collapsing 23894 into "we did a risk assessment" and skipping the AI-specific taxonomy.** The risk workshop uses the generic ERM likelihood-and-impact grid, identifies "AI risk" as one line item, gives it an owner, and moves on. Nothing about 23894 is invoked. The AI-specific risk sources — model drift, adversarial manipulation, training-data provenance, opacity, dependence on third-party providers — are not surfaced because no one asked. The AIMS ships with a risk register that says "AI is risky" and nothing more useful. The fix is discipline: the identification step must walk the 23894 taxonomy, and the register must contain the AI-specific risk sources it surfaces, not just enterprise-generic categories.

## Summary

The AIMS's risk process composes four standards in stack: ISO 31000 as the parent framework, ISO/IEC 23894 as the AI-specific risk-management guidance, ISO/IEC 42005 as the AI impact-assessment methodology, and ISO/IEC 42001 Clause 6.1 as the AIMS-specific requirements. The direction is risk → treatment → control; the AIA is a first-class input to the risk register and the risk-treatment plan; the process runs on a documented cadence and produces documented information. The two classic failures — AIAs that never feed the register, risk assessments that skip the AI-specific taxonomy — are prevented by explicit wire-up between the AIA process and the register, and by disciplined use of the 23894 taxonomy in the identification step. The next chapter walks the SoA — the artefact that records which Annex A controls the AIMS applies, which it excludes, and why.
