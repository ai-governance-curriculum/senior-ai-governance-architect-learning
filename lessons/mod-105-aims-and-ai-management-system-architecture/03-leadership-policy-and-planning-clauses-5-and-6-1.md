# Leadership, policy, and the start of planning — Clauses 5 and 6.1

## Why this chapter exists

Clause 5 (*Leadership*) and Clause 6.1 (*Actions to address risks and opportunities* — the front half of Clause 6) are the two clauses of the AIMS where *top management* has to be visibly, evidencedly involved. If Clause 4 was the load-bearing wall, Clause 5 is the roof: without it the whole building is exposed. Auditors sample Clause 5 evidence hard — meeting minutes, signed policy, delegation records, resource-allocation decisions — because a management system with no top-management engagement is by definition not a management system. The architect's job is to design the artefacts that let top management demonstrate its role without turning every management meeting into an AI governance ceremony.

Clause 6.1 begins the *planning* work of the AIMS — the risk-and-opportunity determination that Clause 6.1.2 turns into the AIMS's risk-assessment process and Clause 6.1.3 turns into the risk-treatment plan and the SoA. This chapter frames that process; chapters `04-risk-and-impact-assessment-composition.md`, `05-statement-of-applicability-and-annex-a.md`, and `06-risk-treatment-plan-and-operational-clauses.md` walk the mechanics.

## Clause 5.1 — leadership and commitment

Clause 5.1 requires top management to demonstrate leadership and commitment with respect to the AIMS by *taking accountability for its effectiveness*, ensuring an AI policy and AI objectives are established and are compatible with the strategic direction, ensuring integration into the organisation's business processes, ensuring resources are available, communicating the importance of the AIMS, ensuring the AIMS achieves its intended results, directing and supporting persons who contribute, promoting continual improvement, and supporting other relevant management roles.

This is not a checkbox. The auditor will ask for:

- **A written accountability statement** signed by top management (typically the CEO or a designated executive — the *AI-accountable executive*, in many enterprises the chief AI officer at level 70 or the head of AI governance at level 60 acting under delegated authority) affirming the AIMS's effectiveness is a top-management responsibility.
- **Board or executive committee minutes** showing the AIMS was discussed at least once in the current cycle. "Discussed" means the minutes record a substantive decision or acceptance, not a "for information" line.
- **Resource decisions** showing top management funded the AIMS. Budget lines for the head of AI governance's team, for internal audit's AI-audit capacity, for competence-development spend, for the GRC-for-AI platform (mod-111). If the AIMS has no budget line, the accountability statement is decorative.
- **Communication records** — town-hall recordings, all-hands emails, internal newsletter items — showing top management has visibly signalled the importance of the AIMS to the workforce.
- **Delegation records** — the memo or charter by which top management delegates operational responsibility for the AIMS to the head of AI governance (level 60), preserving accountability at top-management level. Chapter cross-reference: mod-101 chapter 06 authored the engagement contract with the head of AI governance; the delegation record is that contract's Clause 5.1-facing face.

The architect designs the *shape* of these artefacts and hands the drafting to the head of AI governance and to executive communications. The architect does not draft the CEO's town-hall script.

## Clause 5.2 — the AI policy

Clause 5.2 requires an *AI policy* — a documented statement of top-management intent that:

- Is appropriate to the purpose of the organisation.
- Provides a framework for setting AI objectives.
- Includes a commitment to satisfy applicable requirements.
- Includes a commitment to continual improvement of the AIMS.
- Is available as documented information, communicated within the organisation, and available to interested parties as appropriate.

The AI policy is *not* the enterprise's Responsible AI principles (though it references them). It is not the full enterprise AI governance standard (though it authorises it). It is the *top-level normative statement* — one to three pages — that other AI-governance artefacts point to as their source of authority. Mod-103 chapter 01 walked the policy hierarchy in detail; the AIMS's Clause 5.2 policy is the *binding policy* tier of that hierarchy, one specific instance of it.

A defensible AI policy for Clause 5.2 contains, at minimum:

- **Purpose and scope.** What AI the policy governs (referencing the Clause 4.3 AIMS scope).
- **Governance context.** ISO/IEC 38507 frame — this policy operationalises the governing body's oversight of AI (mod-103 chapter 06).
- **Commitments.** Compliance with applicable requirements (legal, regulatory, contractual, standard-conformance); commitment to continual improvement of the AIMS; commitment to the enterprise's Responsible AI principles; commitment to protection of individuals and groups affected by the enterprise's AI.
- **Objectives framework.** The categories of AI objectives the AIMS pursues, with the specific measurable objectives set in an annex or in the Clause 6.2 objectives register.
- **Accountability.** Who is accountable (the AI-accountable executive); who has operational responsibility (the head of AI governance); how conflicts are escalated.
- **Approval.** Signed by the AI-accountable executive; dated; version-controlled; review cadence stated.

The architect does not draft the policy alone. Legal reviews for enforceability and consistency with contractual and regulatory obligations. The head of AI governance drafts the substantive commitments. Executive communications reviews the framing. The architect *designs* — determines which sections must appear, at what altitude, cross-referencing which other artefacts. The output is a policy the enterprise could put on its investor-relations website without embarrassment and could hand to an auditor who would find it complete.

## Clause 5.3 — organisational roles, responsibilities, and authorities

Clause 5.3 requires that responsibilities and authorities for roles relevant to the AIMS are assigned and communicated. In practice this is where the *AIMS role register* lives.

The register enumerates every role the AIMS depends on — AI-accountable executive, head of AI governance, AI-governance analyst, AI-risk engineer, AI-evaluation engineer, internal auditor with AI competence, and so on — and for each records the responsibilities and authorities under the AIMS. The register does not re-author the enterprise's HR job descriptions; it *maps* AIMS clauses to role holders.

```yaml
role_id: AIMS-ROLE-002
role_name: head-of-ai-governance
family_level: 60
aims_responsibilities:
  - Operation of the AIMS day-to-day (Clause 4.4).
  - Owning the risk-and-impact-assessment process (Clause 6.1).
  - Owning the risk-treatment plan and its refresh cadence (Clause 6.1.3).
  - Owning the SoA and its refresh cadence (Clause 6.1.3.d).
  - Chairing the management review (Clause 9.3).
  - Owning the CAPA record (Clause 10.2).
aims_authorities:
  - Accept residual risk within the enterprise risk appetite
    thresholds set by the AI-accountable executive.
  - Approve exceptions to enterprise AI standards up to the
    threshold defined in the exception-workflow procedure.
  - Escalate to the AI-accountable executive above threshold.
delegates_to:
  - AI-governance analyst (level 15): AIMS documented-information
    maintenance, calendar coordination.
  - AI-risk engineer (level 25): implementation of specific
    Annex A controls.
escalates_to:
  - AI-accountable executive (top management).
```

The register is one of the most useful artefacts to keep current — it is the input to Clause 7.2 (competence), Clause 7.3 (awareness), and every process-owner attribution in Clauses 8, 9, and 10. Every audit will trace from a specific responsibility to the role register and from the role register to the role holder.

## Clause 6.1.1 — determining risks and opportunities

Clause 6.1.1 opens Clause 6 by requiring the organisation to determine the risks and opportunities that need to be addressed to give assurance that the AIMS can achieve its intended outcomes, prevent or reduce undesired effects, and achieve continual improvement. This is the *front matter* of the risk process — it says the process must consider risks (things that could impair the AIMS's outcomes) *and* opportunities (things that could enhance them).

Two clauses follow it that split the work:

- **Clause 6.1.2** — the AI risk assessment (the process by which the enterprise identifies, analyses, and evaluates AI risks). Chapter `04-risk-and-impact-assessment-composition.md` walks the composition with ISO 31000, ISO/IEC 23894, and ISO/IEC 42005.
- **Clause 6.1.3** — the AI risk treatment (the process by which the enterprise selects treatment options, determines controls, produces the SoA, and formulates the risk-treatment plan). Chapter `05-statement-of-applicability-and-annex-a.md` walks the SoA; chapter `06-risk-treatment-plan-and-operational-clauses.md` walks the treatment plan.

Clause 6.1.4 — where present in the 42001 structure — addresses *AI system impact assessment*, requiring the organisation to define and document how and when impact assessments are performed on individuals, groups, or society. This is where ISO/IEC 42005 attaches; chapter `04-risk-and-impact-assessment-composition.md` details the wire-up. <!-- needs-research: confirm whether ISO/IEC 42001:2023 numbers this as Clause 6.1.4 exactly, or under a different sub-clause; the substantive requirement is present in the standard, the numbering to be verified against the published text. -->

## Clause 6.2 — AI objectives and planning to achieve them

Clause 6.2 requires the organisation to establish AI objectives at relevant functions and levels, that are consistent with the AI policy, measurable, monitored, communicated, and updated as appropriate. The organisation must document the objectives and plan how to achieve them (what, resources, responsible, timing, evaluation).

The AIMS objectives are not the enterprise's *product* objectives — they are the *management system's* objectives. They live in a register maintained by the head of AI governance and are reviewed at each management review.

A typical AIMS objective:

```yaml
objective_id: AIMS-OBJ-004
category: control-coverage
statement: >
  All tier-1 AI systems in scope have a completed AI impact
  assessment (per ISO/IEC 42005 and the enterprise AIA
  procedure) with the outputs recorded in the AIMS risk-treatment
  plan, by 2027-03-31.
policy_link: >
  AI Policy §3.2 (commitment to protection of individuals and
  groups affected by the enterprise's AI).
measurable: yes
measure: >
  Fraction of tier-1 systems with completed AIA in the AIMS
  register / total tier-1 systems in the SSP catalogue.
target: 100%
current_baseline: 62%
resources:
  - AI-risk engineers: 2 FTE effort over Q4-Q2.
  - AI-governance analysts: 0.5 FTE for register maintenance.
  - Legal review capacity: 1 FTE-week per AIA.
responsible: head-of-ai-governance
timing: 2027-03-31
evaluation: quarterly at AIMS operational review, annually at management review.
```

Objectives that fail the measurable / responsible / timing / evaluation test are not AIMS objectives — they are aspirations, and the auditor will flag them.

## Clause 6.3 — planning of changes

Clause 6.3 requires that when the organisation determines the need for changes to the AIMS, the changes are carried out in a planned manner. This is the *change-management-of-the-AIMS-itself* clause — distinct from operational change-management of individual AI systems (Clause 8). It attaches to changes such as:

- Adding a newly-acquired legal entity into the AIMS scope.
- Adopting a newly-published Annex A control after a standard revision.
- Reshaping the internal audit programme after an audit finding.
- Retiring a control that the SoA marks as no-longer-applicable.

The architect designs an AIMS-change control record and a defined approval path (typically head of AI governance approves routine changes, AI-accountable executive approves material changes such as scope alterations). The auditor will look for the record when a Clause 4.3 scope statement has changed between two audits without a corresponding Clause 6.3 change record.

## The composition rule that begins here

Clauses 5 and 6.1 establish two things the architecture must respect through the remaining chapters:

1. **Top management is accountable.** Every substantive AIMS decision must trace, in some way, to a Clause 5 leadership artefact — the policy, the role register, the delegation record, the objectives register, the management-review minutes. The AIMS is not a technical governance layer floating above the rest of the enterprise; it is *how the enterprise governs its use of AI*, with top management on the hook.

2. **Risk drives control, not the other way around.** Clause 6.1.1 says risks and opportunities are *determined*; Clause 6.1.2 says the risk assessment identifies, analyses, and evaluates; Clause 6.1.3 says treatment selects controls to address the assessed risks; Clause 6.1.3.d says the SoA records which Annex A controls are applicable. The direction is risk → treatment → control. An enterprise that reads Annex A first and adopts controls it likes, then back-fills a risk register to justify them, has inverted the flow. The auditor will notice; the certification body will require re-work.

## Two failure modes

**Failure mode 1 — top management as rubber-stamp.** The AI policy is drafted by the head of AI governance, signed by the CEO in a batch of routine signatures, and never referenced again. The management review consists of a slide deck the head of AI governance presents to a room of executives whose attention is elsewhere. No minutes are taken beyond "AIMS review — no exceptions raised." The stage-2 auditor will interview two executives at random about the AIMS. If they do not know what it is or what the enterprise's AI-related risks are, Clause 5 has failed. The fix is not more slides; it is designing the management review as a decision-forcing event with prepared inputs and pre-committed outputs (chapter `08-performance-evaluation-internal-audit-and-management-review.md`).

**Failure mode 2 — the risk register that never grew.** Clause 6.1.2 gets a one-time draft during AIMS stand-up. Six months later the enterprise deploys three new AI systems, retires one, and starts a GenAI product line. The risk register has not been updated. The SoA has drifted. The risk-treatment plan references treatments for systems that no longer exist and misses systems that are live. The auditor asks the head of AI governance for the last three updates to the risk register and finds none. The fix is a *cadence* — the risk register is refreshed on a defined trigger (system onboarding, quarterly review, incident, regulatory change) and the refresh is evidenced in an update log.

## Summary

Clause 5 puts *top management* on the AIMS's certificate — through a written accountability statement, a signed AI policy, an assigned role register, a resourcing decision, and a communication record. Clause 6.1 begins the *risk-and-opportunity* determination that the following chapters detail: 6.1.2 is the risk assessment (chapter 04), 6.1.3 is the risk treatment and the SoA (chapters 05 and 06), 6.1.4 is the AI impact assessment (chapter 04). Objectives (Clause 6.2) are measurable AIMS-level goals set against the policy; changes to the AIMS itself (Clause 6.3) are planned and recorded. The composition rule the module enacts is stated here for the first time: top management is accountable, and risk drives control, not the other way around. The next chapter walks the risk-and-impact-assessment composition — ISO 31000, ISO/IEC 23894, ISO/IEC 42005, and 42001 Clause 6.1 — in detail.
