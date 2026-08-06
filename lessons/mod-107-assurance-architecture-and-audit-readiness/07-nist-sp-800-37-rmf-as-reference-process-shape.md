# NIST SP 800-37 RMF as the reference process shape

## Why this chapter exists

The assurance architecture the module has designed — the three lines (chapter 01), the pre-deployment gate (chapter 02), the ongoing programme (chapter 03), the third-line audit (chapter 04), the external-provider interface (chapter 05), the coordination contracts (chapter 06) — is *the enterprise's* assurance system. It is designed against ISO/IEC 42001 (as the management-system standard the AIMS conforms to), against ISO/IEC 42006 (as the certification-body standard the AIMS is audited under), against the IIA Three Lines Model (as the governance-shape reference), against SR 11-7 (as the validation-independence reference for AI models where MRM applies), and against ForHumanity IAAIS and BABL AI (as the independent-auditor shape references). One reference is missing from that list and deserves its own chapter: NIST SP 800-37, the *Risk Management Framework* (RMF), which is the US federal government's reference process shape for authorising and continuously monitoring information systems in operation. The RMF is not an AI-specific document, but its seven-step process shape — Prepare, Categorize, Select, Implement, Assess, Authorize, Monitor — is the shape a generation of US federal system-authorisation processes has been built against, and it composes cleanly with the AI assurance architecture the module has just designed.

This chapter walks the composition. It shows how the RMF's steps map onto the assurance architecture's artefacts, where the AI system's specificities extend the RMF, and where the enterprise's assurance system can *cite* the RMF as the process shape it composes with — a citation that is especially valuable for enterprises operating in US federal contexts (federal contractors, GovCon, systems handling federal data, systems supporting federal-facing consumer processes) but that also lands for private-sector enterprises whose assurance systems need a defensible process-shape reference. NIST SP 1270's guidance on managing bias in AI and NIST AI 100-1 (the AI RMF) itself are AI-specific companions to SP 800-37, and NIST SP 800-53 provides the control catalog the RMF references — this chapter walks the composition briefly for context but its focus is the process shape.

## The RMF, briefly

NIST SP 800-37 Revision 2 (September 2018) — *Risk Management Framework for Information Systems and Organizations* — specifies a seven-step process an organisation runs to bring a system into authorised operation and to keep it authorised over its lifecycle:

- **Step 0 — Prepare.** Organisation-level and system-level preparation activities. Roles are assigned; risk-management strategy is set; the common controls the enterprise inherits are enumerated; the system's boundary is defined.
- **Step 1 — Categorize.** The system is categorised for confidentiality, integrity, and availability against the FIPS 199 impact levels (low / moderate / high). Categorisation drives control-baseline selection.
- **Step 2 — Select.** Controls are selected from the SP 800-53 catalog, tailored to the system, and organised into a security and privacy plan (SSP).
- **Step 3 — Implement.** The controls are implemented; the SSP is updated to document what was implemented and how.
- **Step 4 — Assess.** An independent assessor (the "control assessor") evaluates the controls' effectiveness against the assessment procedures in SP 800-53A and produces a Security Assessment Report (SAR).
- **Step 5 — Authorize.** An authorising official (a senior-management figure with sufficient authority to accept the risk) reviews the SSP, SAR, and Plan of Action and Milestones (POA&M) and grants (or denies, or grants with conditions) an *authorisation to operate* (ATO).
- **Step 6 — Monitor.** Continuous monitoring — the operational discipline of tracking control effectiveness, system changes, and residual risk against the ATO, with re-authorisation on defined triggers.

The RMF's process shape composes with the AIMS-based assurance architecture the module has designed. The next section walks the composition step by step.

## The composition, step by step

The composition is not a wholesale adoption — the RMF is a US-federal-context document and the enterprise's AIMS operates against ISO/IEC 42001, not SP 800-37. But the RMF's steps map cleanly onto the assurance architecture's artefacts, and where the mapping is clean the enterprise's process-shape claim to auditors and regulators is stronger.

### Step 0 — Prepare ↔ AIMS scope, policy, three-lines architecture

RMF's Prepare step establishes the organisational context, roles, and risk-management strategy. In the assurance architecture:

- The AIMS scope statement (mod-105 chapter 02) is the RMF's system-boundary equivalent for the whole management system.
- The AI policy (mod-103) is the RMF's risk-management strategy equivalent for AI.
- The three-lines architecture (chapter 01) is the RMF's role-assignment equivalent, specialised for AI's independence requirements.
- The common controls the enterprise inherits (platform controls, shared-service controls) are the RMF's common controls equivalent — mod-102 chapter 05 walked the control-inheritance model.

The AI-specific extension: RMF Prepare does not have a dedicated *taxonomy* step; the AI assurance architecture's risk taxonomy (mod-106) sits at Prepare and shapes every subsequent step.

### Step 1 — Categorize ↔ system tiering + AIA

RMF Categorize sets confidentiality / integrity / availability impact levels per FIPS 199. In the assurance architecture:

- The enterprise's system-tiering scheme (tier-1 through tier-4 in most of this module's examples) plays the CIA-level role for AI, extended for AI-specific harm dimensions (consequential-decision impact, human-rights exposure, safety-critical-status). Tiering is what drives evidence-contract overlays (chapter 02), re-assessment cadence (chapter 03), and audit sampling depth (chapter 04).
- The AI Impact Assessment (AIA per ISO/IEC 42005 and mod-105 chapter 04) is the AI-specific analog to FIPS 199 — a documented categorisation of the system's impact potential against defined dimensions.

The AI-specific extension: RMF Categorize is one-shot at Prepare; the AIA is re-executed on trigger (chapter 03's ongoing programme). The AI assurance architecture is more explicit about re-categorisation than the RMF is.

### Step 2 — Select ↔ SoA + control library applicability filter

RMF Select tailors SP 800-53 controls to the system, producing the SSP. In the assurance architecture:

- The SoA (mod-105 chapter 05) is the enterprise-wide equivalent — the tailoring of ISO 42001 Annex A (plus the enterprise control library from mod-102) to the AIMS scope.
- The enterprise control library's applicability filter (mod-102 chapter 01) is what selects controls per system within the SoA.
- The System-Specific Plan / System Security Plan analog per system (mod-108 will design the enterprise's shape) is the RMF SSP equivalent.

The AI-specific extension: the applicability filter carries AI-specific dimensions (system_tier, use_case, deployment context) that RMF SP 800-53 tailoring does not natively carry.

### Step 3 — Implement ↔ first-line control implementation

RMF Implement is the first-line work of actually implementing the selected controls. In the assurance architecture this is the first-line responsibility from chapter 01: model owners, platform teams, MLOps implement the controls the SoA and the applicability filter selected, and file the evidence per mod-108.

No AI-specific extension is needed here; the mapping is direct.

### Step 4 — Assess ↔ pre-deployment gate + evaluation engineer methodology

RMF Assess is the independent-assessor role, producing the Security Assessment Report. In the assurance architecture:

- The pre-deployment gate (chapter 02) plays the Assess role, extended for AI's post-deployment surveillance requirement.
- The evaluation engineer's methodology (chapter 06) is the AI-specific analog to SP 800-53A assessment procedures — the *specific technical procedures* by which control effectiveness is measured.
- The gate's decision-record is the SAR analog.

The AI-specific extension: SP 800-53A is a catalog of assessment procedures for classical information-security controls. AI-specific controls require AI-specific procedures — evaluation-set-based measurement, red-team engagement, fairness measurement, robustness measurement. The evaluation engineer authors these; the architect ensures they land in the gate.

### Step 5 — Authorize ↔ pre-deployment gate decision-record + signatures

RMF Authorize is the authorising official's decision to accept the residual risk and grant ATO. In the assurance architecture:

- The pre-deployment gate's signature block (chapter 02) — with escalation to head of AI governance for tier-3 and to the AI-accountable executive for tier-4 — is the ATO-granting authority.
- The escalation to the audit committee for stop-shipping-threshold breaches is analogous to the RMF's escalation of high residual to senior organisational leadership.

The AI-specific extension: the ATO in classical RMF is a one-shot grant subject to re-authorisation triggers; the AI assurance architecture's launch decision is *paired* with the ongoing-assurance programme from the start — chapter 03 walks the re-assessment shape that keeps the decision live.

### Step 6 — Monitor ↔ ongoing assurance programme

RMF Monitor is continuous monitoring — control-effectiveness assessment on cadence, event-driven re-authorisation, and continuous-monitoring-strategy documentation. In the assurance architecture:

- The ongoing assurance programme (chapter 03) is the RMF Monitor analog, extended for AI-specific drift, incident, and regulatory-change triggers.
- Post-market monitoring (mod-110) is the first-line operational monitoring that feeds the ongoing programme, analogous to the RMF's continuous-monitoring instrumentation.
- Re-authorisation triggers (a stale ATO, a material change, a serious finding) map onto the ongoing programme's re-assessment triggers.

The AI-specific extension: RMF Monitor was designed before agentic AI, before continuous frontier-model updates, before the specific drift patterns AI systems exhibit. The AI assurance architecture's four-trigger-type structure (periodic, drift, incident, regulatory-change) is a specialisation of RMF Monitor that RMF alone does not carry.

## Where the RMF adds value beyond the ISO/IEC 42001 shape

The AIMS built to ISO/IEC 42001 is complete on its own. Why compose it with the RMF?

**Value 1 — a defensible process shape for US federal-facing enterprises.** Federal contractors, GovCon, systems handling federal data (FedRAMP scope, CJIS scope, Controlled Unclassified Information scope), and AI systems supporting federal government functions all touch RMF-shaped authorisation processes. An enterprise whose assurance system explicitly composes with the RMF process shape has an easier time producing evidence that a federal assessor recognises.

**Value 2 — a clean process-shape citation for external auditors and regulators.** When an external audit or regulator asks "how do you decide a system is fit to operate?", the answer is stronger if it can reference RMF as the process shape. This is especially true for banking (where SR 11-7 has RMF-influenced ancestry), for insurance (where state regulators increasingly reference RMF-shaped processes), and for healthcare (where FDA guidance for AI/ML devices increasingly references RMF concepts).

**Value 3 — a bridge to SP 800-53 for control-catalog composition.** The enterprise control library (mod-102) composes with SP 800-53 via OSCAL. RMF's step 2 (Select) uses SP 800-53. The composition path is: enterprise control library → SP 800-53 crosswalk in OSCAL → RMF Select tailoring for a system in federal scope. Where the enterprise has no federal scope, the crosswalk is still useful for cross-catalog composition with the CSF 2.0 executive overlay.

**Value 4 — an ATO analog for internal use.** Some enterprises adopt an internal "ATO" language for AI systems — the pre-deployment gate's decision-record is labelled an "AI ATO" and the re-assessment cadence is labelled "continuous ATO monitoring." The language is optional; where the enterprise's stakeholders are comfortable with the RMF vocabulary, adopting it lowers the friction of explaining the assurance system.

## Where the RMF does not compose cleanly

The RMF was designed for classical information-security systems. Three areas where the AI assurance architecture must extend beyond the RMF rather than compose with it:

**Extension 1 — the taxonomy step.** The RMF's Prepare step does not include a risk-taxonomy activity. AI systems require the taxonomy (mod-106) to make risk assessment defensible. The architecture keeps the taxonomy as a distinct architectural artefact; the RMF composition treats the taxonomy as an input to the RMF's risk-management-strategy sub-step, not as an RMF sub-step itself.

**Extension 2 — the impact assessment.** The RMF's Categorize step is CIA-triaged; the AIA (per ISO/IEC 42005) covers dimensions the CIA triage does not (impact on rights, on human autonomy, on protected populations, on the environment). The architecture keeps the AIA as a distinct AI-specific artefact; the RMF composition treats the AIA as a specialisation of Categorize for AI systems.

**Extension 3 — the third-line audit programme.** The RMF's Monitor step includes continuous assessment, but the RMF is not designed as a management system with a third-line internal audit programme. The enterprise's third-line audit (chapter 04) is an AIMS artefact under ISO/IEC 42001 clause 9.2 (and IIA and SR 11-7), not an RMF artefact. Where the composition is claimed, the composition is at the assurance-architecture level, not at the RMF level.

## A schematic — the composition matrix

The composition can be presented as a matrix the architect maintains as an assurance-architecture appendix.

```yaml
composition_rmf_to_assurance_architecture:
  version: 1.1.0
  rmf_reference: NIST SP 800-37 Revision 2 (Sep 2018)
  primary_assurance_reference: ISO/IEC 42001:2023
  mapping:
    - rmf_step: Prepare
      assurance_artefacts:
        - AIMS scope statement (mod-105 ch. 02)
        - AI policy (mod-103)
        - three-lines architecture (mod-107 ch. 01)
        - risk taxonomy (mod-106) [AI extension beyond RMF Prepare]
        - common-controls inheritance (mod-102 ch. 05)
    - rmf_step: Categorize
      assurance_artefacts:
        - system tiering scheme (enterprise-defined)
        - AIA per ISO/IEC 42005 (mod-105 ch. 04) [AI extension of FIPS 199]
    - rmf_step: Select
      assurance_artefacts:
        - SoA (mod-105 ch. 05)
        - enterprise control library applicability filter (mod-102 ch. 01)
        - SSP-per-system analog (mod-108)
    - rmf_step: Implement
      assurance_artefacts:
        - first-line control implementation
        - evidence artefacts per mod-108 evidence contract
    - rmf_step: Assess
      assurance_artefacts:
        - pre-deployment gate (mod-107 ch. 02)
        - evaluation-engineer methodology (mod-107 ch. 06)
        - SAR analog: the gate's decision-record
    - rmf_step: Authorize
      assurance_artefacts:
        - gate signature block (mod-107 ch. 02)
        - escalation to head-of-ai-governance (tier-3) or AI-accountable executive
          (tier-4) or audit committee (stop-shipping threshold)
        - ATO analog: the gate's decision-record with signatures
    - rmf_step: Monitor
      assurance_artefacts:
        - ongoing assurance programme (mod-107 ch. 03)
        - post-market monitoring (mod-110) as first-line instrumentation
        - re-assessment triggers (periodic / drift / incident / regulatory-change)
  ai_extensions_not_in_rmf:
    - taxonomy step (mod-106)
    - AIA extension of Categorize (mod-105 ch. 04)
    - third-line audit programme (mod-107 ch. 04)
    - external-audit-provider interface (mod-107 ch. 05)
  citation_use_cases:
    - federal-facing systems: explicit RMF-composition claim
    - external audit and regulator: RMF as citable process-shape reference
    - internal vocabulary: optional "AI ATO" language for the gate's decision
```

## Two failure modes to design against

**Failure mode 1 — the wholesale-adoption error.** The enterprise adopts the RMF wholesale, treating it as the primary reference process, and treats ISO/IEC 42001 as a compliance overlay. The mismatch is visible: the RMF is a US-federal-context document, not a management-system standard; the certification body's stage-2 audit under 42006 finds gaps because the AIMS was not designed against ISO 42001 but against RMF. The fix is *ISO/IEC 42001 as primary, RMF as reference composition*: the AIMS is designed to conform to ISO 42001 (this track's premise); the RMF is a composition the assurance architecture explicitly cites where the composition is clean; RMF is not a substitute for the AIMS.

**Failure mode 2 — the naming-only composition.** The enterprise renames its pre-deployment gate to "AI ATO" and its ongoing programme to "AI continuous monitoring" but does not actually compose the RMF process shape with the assurance system's actual operations. The vocabulary changes; the practice does not; RMF-familiar assessors and regulators notice within one interview. The fix is *composition or nothing*: if the enterprise cites RMF, the composition is real (the mapping matrix is authored, the artefacts trace to RMF steps, the AI extensions are documented); if the enterprise does not have the appetite for the composition, the enterprise cites ISO 42001 alone and does not adopt RMF vocabulary.

## Summary

NIST SP 800-37 RMF is a reference process shape the assurance architecture composes with. The seven RMF steps (Prepare, Categorize, Select, Implement, Assess, Authorize, Monitor) map onto the assurance architecture's artefacts — the AIMS scope and three-lines architecture at Prepare, the tiering and AIA at Categorize, the SoA and applicability filter at Select, the first-line implementation at Implement, the pre-deployment gate and evaluation methodology at Assess, the gate signatures and escalation at Authorize, the ongoing programme at Monitor. Three areas require AI-specific extension beyond the RMF — the taxonomy step at Prepare, the AIA extension at Categorize, the third-line audit programme that is not part of RMF at all. The composition adds value where the enterprise faces US federal-context stakeholders, wants a process-shape citation for external auditors and regulators, needs a bridge to SP 800-53 for control-catalog composition, or wants an internal ATO-analog vocabulary. Two failure modes — wholesale adoption of RMF instead of ISO 42001; naming-only composition without practice change — are architectural and preventable. With this chapter the module closes: an assurance architecture with a three-lines base (chapter 01), a pre-deployment gate (chapter 02), an ongoing programme (chapter 03), a third-line audit (chapter 04), an external-provider interface (chapter 05), coordination contracts with the two dependent roles (chapter 06), and a process-shape composition with RMF (chapter 07) is the architecture the enterprise's assurance system runs against.
