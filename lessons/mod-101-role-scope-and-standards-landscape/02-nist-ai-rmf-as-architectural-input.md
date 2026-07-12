# Reading NIST AI RMF 1.0 as an architectural input

## Why this chapter exists

Most people who open NIST AI RMF 1.0 for the first time read it as a checklist: they scan for MEASURE.2.5 and ask "did we do that yet?" That reading is *wrong for this role*. The Framework is not a compliance list — the document says so, explicitly and repeatedly: participation is voluntary, the sub-categories are outcome statements, and the Playbook is a *suggested* set of actions, not the audit script.

At level 50 you are not consuming the Framework to *tick* it. You are consuming it to *architect against* it. Every function — GOVERN, MAP, MEASURE, MANAGE — has to land in an artifact that a level-25 engineer or a level-15 analyst can *do work inside*, or the Framework is a shelf ornament in your org.

This chapter teaches how to translate NIST AI RMF 1.0 (with its Playbook and the Generative AI Profile, AI 600-1) into architectural inputs — control-library entries, taxonomy nodes, RACI cells, evidence contracts — and where in the enterprise operating model each function belongs.

## Framework structure in one page

NIST AI RMF 1.0 (NIST AI 100-1, January 2023) is organised around four *functions*:

- **GOVERN** — the cross-cutting culture / policy / accountability function. Categories about roles, structures, risk-management processes, accountability, workforce, third-party.
- **MAP** — establish context. Categories about system categorisation, use context, expected benefits/costs, impacted parties, requirements gathering.
- **MEASURE** — assess, analyse, track. Categories about test / evaluation / verification / validation (TEVV), metrics, tracking risks over time, feedback loops.
- **MANAGE** — prioritise and act on risks. Categories about risk treatment, resource allocation, incident response, communication.

Each function contains **categories** (e.g. GOVERN-1) and **sub-categories** (e.g. GOVERN-1.1) that are written as *outcome statements* — "policies, processes, procedures, and practices across the organization related to the mapping, measuring, and managing of AI risks are in place." Nothing in the base Framework tells you *how* to achieve those outcomes.

The **Playbook** (a separate NIST living document) supplies suggested tactics, questions, and references per sub-category. It is not normative — you can adopt one action from a sub-category, or three, or a completely different action from your own control library.

The **Generative AI Profile** (NIST AI 600-1, July 2024) is a companion Profile that overlays generative AI risks — twelve risk categories including confabulation, dangerous / violent / hateful content, data privacy, CBRN uplift, human-AI configuration, IP, obscene content, offensive cyber, harmful bias, environmental impact, information integrity, value chain / component integration — and maps *actions* per NIST AI RMF sub-category for each risk.

## The architect's translation move

The single most important architectural move is: **each sub-category in the Framework becomes at least one control library entry in your enterprise catalog.**

Not "the Framework says X, so we need X." That reading skips the design step. The move is: "GOVERN-1.1 requires organisational policy artefacts across the AI lifecycle → *in our enterprise*, that maps to control entries `AIC-GOV-001` (AI Policy Register), `AIC-GOV-002` (AI Ethics Committee Charter), `AIC-GOV-003` (Risk-Management Framework Reference), each with applicability filters, implementation guidance, testing procedure, and evidence contract."

Sub-category → control entries is a *one-to-many* mapping in practice. A single sub-category often needs 2–4 controls at enterprise scope because the outcome cuts across policy, process, tooling, and evidence.

Here is a concrete illustration in pseudo-OSCAL structure:

```yaml
# derived-from: NIST AI RMF 1.0 sub-category GOVERN-1.1
control:
  id: AIC-GOV-001
  title: AI Policy Register
  applicability:
    system_tier: [tier-1, tier-2, tier-3]
    jurisdictions: [all]
  statement: >
    The organization maintains a policy register that lists each binding AI
    policy, its owner, its version, and the AI systems it applies to.
  implementation_guidance:
    - "Policies are hierarchy-classified per mod-103 taxonomy."
    - "Register is the source of truth cited by mod-105 AIMS SoA."
  testing_procedure:
    - "Quarterly reconciliation against the AI system inventory."
    - "Every tier-1 system links to at least one binding policy."
  evidence_contract:
    - artifact: policy-register.csv
      owner: ai-governance-analyst
      retention_years: 7
    - artifact: quarterly-reconciliation-report
      owner: ai-governance-analyst
      retention_years: 7
  crosswalk:
    nist_ai_rmf: [GOVERN-1.1]
    iso_iec_42001: [A.2.2, A.5.2]
    eu_ai_act: [Article 17]
```

That block is the deliverable. Nothing in NIST AI RMF told you to structure it that way; the *architecture* did.

## Locating each function in the enterprise operating model

The four functions do not sit in one team. Locating them correctly is architecture:

- **GOVERN** sits with the AI Governance Council, the head of AI governance, and — for policy artefacts — the architect. It is not owned by any one product team. If GOVERN outcomes only exist inside one product team's docs, the org is missing an enterprise policy layer.
- **MAP** sits with a *joint* body: intake analysts (level 15) collect the context; the risk engineer (level 25) categorises the AI system; the architect (level 50) has designed the tiering criteria and the categorisation schema they are using. MAP outcomes fail when intake and categorisation are not wired together.
- **MEASURE** sits primarily with the evaluation engineer (level 35) and — for agent / dangerous-capability evaluations — the agentic safety engineer (level 40). The architect owns the *evidence contract* that says which metrics MEASURE has to emit, in which format, and where they get stored. The architect does not run the eval.
- **MANAGE** sits with a *joint* body: the risk engineer (level 25) executes treatment; the AI governance council decides prioritisation; the head of AI governance owns the escalation and incident-communication surface. The architect designed the risk-treatment schema, the appetite thresholds, and the escalation ladder they are all reading from.

You will see this pattern re-appear in mod-107 (assurance architecture) and mod-112 (operating model). Locate the function correctly here, once, and the later modules only refine.

## Reading the Playbook without turning into a checklist

The Playbook is useful in one specific mode: as an *idea corpus* for control-library entries you have not designed yet. When you draft `AIC-XYZ-042` for a new control, checking the Playbook's suggested actions for the matching sub-category surfaces authored options and cited references you can either adopt or note as a rejected alternative.

The Playbook is *not* useful as: a definitive list of actions your org must take; a scoring rubric against which auditors grade you; a substitute for enterprise-specific control authorship.

Distinguishing "reading the Playbook for ideas" from "adopting the Playbook as our program" is a real cultural fight in orgs where governance grew out of compliance. Winning that fight is part of the role.

## The Generative AI Profile — a superset applicability filter, not a fork

The AI 600-1 Generative AI Profile is often mis-read as a second, separate program. It is not. Architecturally, the Profile is a *superset applicability filter* on the control library: for AI systems whose classification includes a generative component, additional actions attach to the sub-categories most exposed to generative risk.

Two examples of how this looks in the control library:

- Sub-category MEASURE-2.11 (fairness / bias) already has an enterprise control for classical ML systems. The Profile adds actions relevant to LLM outputs (dialect discrimination, refusal-rate imbalance across protected classes). In OSCAL that becomes an *extension* of the existing control's implementation guidance under a new applicability filter (`system_kind: generative`) rather than a new control.
- Confabulation is a Profile-defined risk category with no direct pre-existing sub-category. It joins as a *new* control (`AIC-GEN-021` — Confabulation Rate Monitoring) with applicability restricted to generative systems, and it is *crosswalked* to MEASURE-2.13 and MANAGE-2.3.

Getting this right avoids the two failure modes: (a) parallel programs for gen-AI that lose cross-cutting coherence, and (b) a single program that ignores gen-AI-specific harms because they don't match a legacy sub-category cleanly.

## Concrete example — annotating a sub-category

Here is a worked example of the annotation you will practise in exercise-02.

**Sub-category**: MANAGE-2.3 (Sustained value of deployed AI systems is supported through incident response and prevention).

**Enterprise operating-model location**: MANAGE (function) → jointly owned by AI risk engineer (level 25) for treatment execution and head of AI governance (level 60) for external communications; architected by this role.

**Existing enterprise controls that would satisfy**: `AIC-INC-014` (AI Incident Response Playbook), `AIC-INC-015` (Post-Incident Review Schema), `AIC-INC-016` (Regulator-Reporting Timeline).

**Gaps at enterprise scope**: no control currently links the internal incident record to EU AI Act Article 73 reporting cadence. Draft new control `AIC-INC-017` (EU AI Act Article 73 Serious-Incident Bridge) with applicability filter `jurisdiction: EU AND ai_act_risk_tier: high`.

**Crosswalk**: NIST AI RMF MANAGE-2.3; ISO/IEC 42001 A.9.3; EU AI Act Article 73; SR 11-7 (Model risk incidents).

**Evidence contract**: incident record artefact (structured), post-incident review document, regulator notification proof if applicable.

That annotation is one row in a table you will build across dozens of sub-categories. Get the annotation shape right on ten and the pattern will scale.

## Summary

NIST AI RMF 1.0 is an *architectural input* — outcome statements the architect turns into control-library entries, taxonomy nodes, RACI cells, and evidence contracts. Read the base document for the four functions and their categories; use the Playbook as an idea corpus, not a scorecard; treat the Generative AI Profile as a superset filter, not a fork. Locate each function in the operating model: GOVERN with the council + architect, MAP with intake + risk-engineer + architect's schema, MEASURE with evaluation-engineer + architect's evidence contract, MANAGE with risk-engineer + head + architect's escalation ladder. Exercise-02 puts this into practice on a fixed set of sub-categories.
