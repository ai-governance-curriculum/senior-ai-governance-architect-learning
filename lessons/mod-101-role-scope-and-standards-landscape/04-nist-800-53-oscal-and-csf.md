# Composing with NIST SP 800-53, SP 800-37, CSF 2.0, and OSCAL

## Why this chapter exists

Nearly every enterprise you will architect an AI control library for already has an *existing control catalog* — a mature one, in many cases: NIST SP 800-53 rev5 in federal / regulated environments, ISO/IEC 27001 Annex A in the ISO-first shops, and NIST Cybersecurity Framework (CSF) 2.0 as the executive-facing overlay in most US enterprises. Ignoring that catalog and shipping an AI-only library that lives beside it produces two catalogs the org has to reconcile forever. That is a defeat for architecture — it multiplies work rather than composing it.

The correct move is composition: the AI control library *plugs into* the existing catalog. Where the enterprise control already covers the AI risk, the AI library *inherits* and *extends*. Where the risk is AI-specific with no parent, the AI library *adds* a new entry that references the parent catalog for context. This chapter teaches the vocabulary and the machinery — SP 800-53 rev5 as the control catalog, SP 800-37 rev2 as the RMF process, CSF 2.0 as the executive overlay, OSCAL as the machine-readable representation — that make the composition tractable.

## The four pieces, and what each contributes

| Piece | What it is | What it gives the architect |
|---|---|---|
| **NIST SP 800-53 rev5** | Security and privacy controls catalog for federal information systems and organizations. ~1,000 controls organised in 20 control families. | The parent control catalog the AI library composes against. Also the source of vocabulary for control statements, guidance, and enhancements. |
| **NIST SP 800-37 rev2** | Risk Management Framework (RMF) process. Seven steps: Prepare, Categorize, Select, Implement, Assess, Authorize, Monitor. | The process shell around control selection and authorization. Mods 105–107 write against it. |
| **NIST CSF 2.0** | Cybersecurity Framework (Feb 2024). Six functions: GOVERN, IDENTIFY, PROTECT, DETECT, RESPOND, RECOVER. Executive-facing, category / subcategory structure. | The overlay executives, boards, and cross-org stakeholders read. Useful for RACI + comms language. |
| **OSCAL** | Open Security Controls Assessment Language. A NIST-published, machine-readable representation of catalogs, profiles, component definitions, SSPs, assessment plans / results, POA&Ms. | The serialization format that makes the control library sharable, tooling-friendly, and diff-able. |

If your enterprise is not federal-facing, SP 800-53 may live as ISO/IEC 27002 or a hybrid instead. The architectural move is the same; the tables just have different IDs.

## SP 800-53 rev5 — how to read it

The catalog is huge. Do not read it front-to-back; that is not the workflow. Read it *by control family* and *by control call-out*.

The 20 families most relevant to an AI library:

- **AC** — Access Control
- **AT** — Awareness and Training
- **AU** — Audit and Accountability
- **CA** — Assessment, Authorization, and Monitoring
- **CM** — Configuration Management
- **CP** — Contingency Planning
- **IA** — Identification and Authentication
- **IR** — Incident Response
- **PL** — Planning
- **PM** — Program Management
- **PS** — Personnel Security
- **PT** — PII Processing and Transparency
- **RA** — Risk Assessment
- **SA** — System and Services Acquisition
- **SC** — System and Communications Protection
- **SI** — System and Information Integrity
- **SR** — Supply Chain Risk Management

The 800-53 rev5 controls are catalog *entries*: each has a control statement, discussion, related controls, and control enhancements. An AI-library control that composes with 800-53 references the parent(s) by ID.

Two concrete examples of composition:

- The AI risk of **training data poisoning** already has a parent in the SI (System and Information Integrity) family. Instead of writing an AI-only training-data-poisoning control from scratch, extend SI-related controls with AI-specific implementation guidance and testing procedure. Your control statement is *terse* because SI already carried the framing; your enhancement is *AI-specific* because 800-53 does not know about training data.
- The AI risk of **prompt injection into agent tool-use** has no direct parent (the closest is SI-10 information input validation, but the fit is loose). This one *adds* a new AI-library entry that references SI-10 in "related controls" but stands on its own.

The architectural discipline is: inherit before you extend, and extend before you add. Adding without checking the parent produces a duplicate library.

## SP 800-37 rev2 — the RMF process shell

800-37's seven-step RMF is the process the enterprise runs *around* a system: Prepare → Categorize → Select controls → Implement → Assess → Authorize → Monitor. Mod-107 (assurance architecture) will formalise this in the enterprise AI context. Mod-101's job is to know that:

- **Categorize** is where the AI system's tier (low / moderate / high / critical) is chosen. Your enterprise's AI system-tiering criteria — level of autonomy, harm potential, jurisdictional scope, exposure to fundamental-rights risks — attaches here.
- **Select** is where the applicable controls from the library are chosen for a given system. The applicability filters in the control library entries drive this. In a well-architected library, Select is *automated* off filters and the SoA is generated, not hand-authored per system.
- **Assess** is where evidence is collected against the evidence contracts. Mod-108 (evidence architecture) makes this concrete.
- **Authorize** is where a named accountable executive signs off on residual risk. In the AI context, that is often the head of AI governance (level 60) or CAO (level 70), depending on tier.
- **Monitor** is the post-authorization lifecycle — ongoing assessment, incident feedback, re-authorization triggers. Mod-110 (post-market surveillance) writes against this.

You do not have to *reinvent* the process for AI; you have to *specialise* it. That specialisation is a level-50 deliverable.

## CSF 2.0 — the audience-facing overlay

NIST CSF 2.0, released February 2024, restructured the earlier CSF (1.0/1.1) with an explicit **GOVERN** function. The six functions are:

- **GOVERN (GV)** — organizational context, risk-management strategy, roles and responsibilities, policy, oversight, cybersecurity supply chain risk management.
- **IDENTIFY (ID)** — asset management, risk assessment, improvement.
- **PROTECT (PR)** — identity management, awareness / training, data security, platform security, technology infrastructure resilience.
- **DETECT (DE)** — continuous monitoring, adverse event analysis.
- **RESPOND (RS)** — incident management, analysis, response reporting and communication, mitigation.
- **RECOVER (RC)** — incident recovery plan execution, communication.

CSF 2.0 is *not a control catalog*. It is a categorisation / language framework whose GOVERN function was explicitly aligned to make CSF conversant with enterprise governance (including AI governance). The architectural value: when you brief a CIO / CISO or when your AI control library entries have to be found by a broader security-org audience, CSF categories are the language they use.

Practical move: annotate each AI control library entry with the CSF 2.0 subcategory it supports (e.g. `csf_v2: [GV.OC-01, DE.AE-04]`). The security org can now pull the AI library through their CSF lens without a schema fight.

## OSCAL — the machine-readable representation

OSCAL is NIST's XML/JSON/YAML representation of catalogs, profiles, component definitions, system security plans, assessment plans, assessment results, and plans of action & milestones (POA&Ms). If you have not seen it before, the mental model is:

- **Catalog** — a set of controls (e.g. SP 800-53 rev5 in OSCAL is officially published).
- **Profile** — a *selection* from one or more catalogs, with parameter values (e.g. "our high-tier AI profile picks these AC / AU / IR / PT controls with these parameter values").
- **Component definition** — controls *implemented by* a given component (e.g. "our guardrail service implements SI-10 and AIC-GEN-021").
- **System security plan (SSP)** — the composition: given a system, which components implement which controls with which evidence pointers.
- **Assessment plan / results** — how the controls were assessed, and what was found.
- **POA&M** — what is outstanding.

For an AI library, OSCAL gives you three things you cannot easily fake in Word / Confluence:

1. **Machine-readable applicability filters** — a `props` block on each control that describes when it applies (`system_tier`, `jurisdiction`, `system_kind`). Automation across mod-102 (library), mod-103 (policy-as-code), mod-104 (jurisdiction reconciliation) reads these directly.
2. **Diff-able change history** — controls live in Git; changes review through pull requests; audit trail of authorship is native.
3. **Machine-readable crosswalks** — a `links` block per control mapping to NIST AI RMF sub-categories, ISO/IEC 42001 Annex A, EU AI Act articles, OWASP LLM Top 10 entries, MITRE ATLAS techniques. This is what makes mod-104's reconciliation architecture computable.

A stripped example, in OSCAL JSON shape:

```json
{
  "control": {
    "id": "aic-gen-021",
    "class": "AIC",
    "title": "Confabulation Rate Monitoring",
    "props": [
      {"name": "system_kind", "value": "generative"},
      {"name": "system_tier", "value": "tier-1,tier-2"}
    ],
    "parts": [
      {"name": "statement", "prose": "The organization monitors confabulation rate for generative AI systems and treats sustained deviation from baseline as an incident trigger."},
      {"name": "guidance", "prose": "See ENT-PROC-GEN-11 for monitoring cadence."}
    ],
    "links": [
      {"rel": "related", "href": "#si-10"},
      {"rel": "reference", "href": "https://airc.nist.gov/AI_RMF_Knowledge_Base/Playbook/MEASURE/2/13"},
      {"rel": "reference", "text": "ISO/IEC 42001 Annex A.6.2.6"},
      {"rel": "reference", "text": "EU AI Act Article 15"}
    ]
  }
}
```

You will produce entries in this shape throughout mod-102 and again for jurisdictional profiles in mod-104. Making the applicability filters and crosswalks machine-readable is not stylistic preference — it is what buys the library the reusability that justifies its cost.

## Composition patterns for the AI library

Three composition patterns come up over and over. Name them and you will spot them in review:

**Pattern A — Extend**
The parent 800-53 (or 27002) control almost covers the AI risk; the AI library adds AI-specific implementation guidance / testing procedure / evidence contract as an *extension* under a filter (e.g. `system_kind: generative`). Statement stays terse. Best for classical-ML risks whose analogy in traditional software already sits in the parent catalog: change management, access control, logging, incident response.

**Pattern B — Inherit-and-augment**
The parent covers the risk at a coarser granularity; the AI library adds a *new* entry that references the parent as "related", refines applicability, and adds evidence contract. Best for risks like model drift (parent: SI-4 information system monitoring; AI extension: drift-monitoring-specific artefacts).

**Pattern C — Add**
The AI risk has no meaningful parent. A new entry stands on its own. Best for AI-specific risks with no traditional analogy — prompt injection into agentic tool-use, confabulation, generative-model IP contamination.

A healthy library will run ~60/25/15 across Extend / Inherit-and-augment / Add. If you find yourself at 5/5/90, you are probably duplicating the parent catalog without knowing it. If you find yourself at 100/0/0, you probably wrote AI-flavoured wallpaper rather than an actual AI library.

## Where SP 800-53 rev5 already carries AI in flight

Note that SP 800-53 rev5 was published in Sep 2020 (with subsequent updates), before NIST AI RMF and before the EU AI Act. Its coverage of AI-specific risks is patchy. Do not expect the catalog to name "training data poisoning" or "model card"; the AI library must add those. Do expect the catalog to fully carry AI-relevant risks around access, audit, incident, supply-chain, and privacy — this is where composition wins.

<!-- needs-research: If a NIST 800-53 rev6 with AI-native additions has been published by the time this module is next re-authored, refresh this section to reference it rather than "rev5 coverage is patchy". -->

## Summary

The AI control library composes with the enterprise's existing control catalog — SP 800-53 rev5 in most federal-facing shops — under three patterns: extend, inherit-and-augment, add. SP 800-37 rev2's seven-step RMF is the process shell; specialise it, don't reinvent it. CSF 2.0 is the executive-facing overlay whose GOVERN function is your audience language; annotate control entries with CSF subcategories so the security org can consume the library through its own lens. OSCAL is the machine-readable representation that makes applicability filters, crosswalks, and diff-able change history real. Exercise-04 puts the composition into practice on a small set of controls.
