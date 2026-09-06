# The critical-infrastructure operator blueprint — CISA / NCSC Secure AI System Development, ENISA Multilayer Framework, NIST CSF 2.0, and the sector-regulator overlay

## Why this chapter exists

The level-50 architect who arrives at a critical-infrastructure operator — an electric utility running a bulk-electric-system asset base, an oil-and-gas midstream operator running pipeline and terminal control systems, a water and wastewater utility, a Class I railroad, a pipeline operator regulated by the Transportation Security Administration, an EU essential entity under NIS2 — does not inherit a greenfield governance frame. They inherit a *security-first* governance frame authored over decades against ICS/OT threat models, sector-regulator inspection cycles, and continuity-of-service obligations that make an incorrect shutdown as consequential as an incorrect start. The pre-existing programme is an ICS/OT security programme composed against a sector-specific cybersecurity regime — NERC CIP for the North American bulk electric system, TSA Security Directives for pipelines and passenger rail, the NIS2 Directive for EU essential and important entities, sector-specific expectations from AWIA and EPA for water utilities, from the FAA for aviation. The AI programme composes onto that frame; it does not compete with it, and it does not attempt to reinvent the ICS/OT change-control regime.

The composition anchor is *cyber-physical safety and continuity-of-service*. What AI is allowed to do inside the operator's control-system boundary is subordinated to those two constraints — an AI system that would improve efficiency at the cost of a demonstrable increase in cyber-physical risk or continuity-of-service risk does not clear the pre-deployment gate, regardless of how well it satisfies the horizontal frame. The composition instrument is a stack the level-50 architect reads together: **NIST Cybersecurity Framework 2.0** as the outer governance and risk-management scaffold, **NIST SP 800-82 Rev. 3** as the OT-specific application of that scaffold, the **CISA / NCSC Guidelines for Secure AI System Development** as the AI-specific life-cycle guidance, the **ENISA Multilayer Framework for Good Cybersecurity Practices for AI** as the layered practice reference (particularly for EU operators), and the sector regulator's own instrument (NERC CIP, TSA SDs, NIS2, sector-specific US federal rules) as the enforceable overlay. This chapter walks each layer, states the AI-in-OT boundary discipline the sector cannot compromise on, and fills in the chapter 01 sector adaptation record for a critical-infrastructure operator.

The failure mode the chapter designs against is the one where the AI programme is authored without composing with the existing ICS/OT security programme — producing two conflicting change-control regimes, two conflicting inventories, two conflicting incident-response playbooks, and a sector-regulator dialogue in which the AI team and the ICS/OT security team give the inspector different answers to the same question. The second failure mode is the one where the applicability filter does not carry the safety-instrumented-system-scope dimension, and a generative-AI inference reaches the SIS boundary because nothing in the compiled control set said it must not.

## The CISA / NCSC Guidelines for Secure AI System Development

The **Guidelines for Secure AI System Development** were published on 26 November 2023 <!-- needs-research: verify exact publication date --> by the UK's National Cyber Security Centre (NCSC) with the US Cybersecurity and Infrastructure Security Agency (CISA) as co-seal and a group of international partner agencies as co-signatories. The guidelines are non-binding but read by CISA and NCSC as the reference articulation of secure-by-design practice for AI system development and are increasingly cited in sector-regulator communications as the expected shape.

The guidelines organise recommendations across four life-cycle stages. Each stage is a composition point for the mod-102 control library and the mod-108 evidence architecture.

- **Secure design.** Threat modelling that is *AI-aware* — the model of an AI system is not the model of a conventional application. Adversarial-input threats (evasion, prompt injection, prompt-context exfiltration), training-data poisoning (label poisoning, backdoor insertion), model theft, and inference-endpoint abuse are named as first-class threats. The architect adds threat-modelling-for-AI-systems as an evidence-contract line item on the mod-102 design-control entry, with the threat model itself as the mod-108 artefact and the AI-specific threat catalogue as its schema constraint. The design stage also names dependency-risk analysis on training data sources, model providers, and inference dependencies — feeding directly into the mod-109 third-party programme.
- **Secure development.** Supply-chain security for models — provenance of training data, provenance of pre-trained weights, provenance of libraries, signed and integrity-checked artefacts moving through the build pipeline. The mod-108 model-documentation artefact carries a model SBOM (software bill of materials for model constituents) as an evidence-contract line item; the mod-109 third-party programme carries the provider's supply-chain attestations as inbound evidence. Secure development also names secure coding for AI-adjacent components (data pipelines, evaluation harnesses, agentic orchestration) at the standard consumed by the enterprise SDLC.
- **Secure deployment.** Infrastructure security for the inference stack — segmentation of inference boundaries, secrets management for model access, hardened container and orchestration configuration, protection of the model itself as a sensitive asset. Continuous vulnerability management on the inference-serving stack is required, and the guidelines make explicit that the model is an asset requiring the same asset-protection discipline as any other production system. The mod-102 catalog's deployment-control family acquires an AI-inference-deployment-hardening line item; the mod-108 evidence contract requires deployment-configuration evidence per release.
- **Secure operation and maintenance.** Continuous behaviour-drift and adversarial-input monitoring, logging of inputs and outputs sufficient to reconstruct incidents, incident-response integration so that AI-specific incidents route into the enterprise incident-response function without a parallel pathway, information sharing with sector partners, and disclosure of vulnerabilities to affected parties. The operational stage composes with mod-110 post-market surveillance directly — the AI-specific monitoring signals are PMS-native artefacts, and the incident pipeline is one pipeline with AI-specific severity attributes rather than a separate AI-incident pipeline.

The composition move the sector-adaptation record makes explicit is that CISA/NCSC's four stages are *not* a new life cycle running parallel to the enterprise SDLC; they are recommendations that overlay the existing life cycle. The mod-102 control library carries CISA/NCSC-derived line items on the corresponding stage of the enterprise SDLC controls, and the mod-108 evidence contract renders one artefact family per life-cycle activity that satisfies both the enterprise standard and the CISA/NCSC guideline.

## The ENISA Multilayer Framework for Good Cybersecurity Practices for AI

The **Multilayer Framework for Good Cybersecurity Practices for AI** was published by ENISA <!-- needs-research: verify exact publication date; ENISA published a multilayer framework on cybersecurity for AI in the 2023 timeframe with subsequent updates --> as a layered reference for cybersecurity practices applicable to AI systems, structured as three composed layers:

- **Layer 1 — cybersecurity foundations.** The general information-security controls that any AI system inherits by virtue of being an information system — access control, cryptography, logging, vulnerability management. These are already carried by the enterprise ISMS (typically ISO/IEC 27001-anchored) and by mod-102 catalog entries the AIMS references through the mod-105 ISMS-AIMS interface (see mod-105 chapter 04).
- **Layer 2 — AI-specific practices.** The controls that are only well-formed when the asset is an AI system — training-data governance, model evaluation for robustness under adversarial conditions, monitoring for behaviour drift, model provenance and integrity. These map onto the mod-102 AI-specific control families with ENISA's line items registered as evidence-contract extensions on the relevant controls.
- **Layer 3 — sector-specific practices.** Layered on top of the first two, the sector-specific controls that reflect the operator's regulatory frame and the sector's threat landscape. This is the layer the sector-adaptation record is written for; it is where the sector-regulator overlay is registered against the enterprise reference profile.

The framework composes with the **NIS2 Directive**'s Article 21 cybersecurity risk-management measures directly. Article 21 requires essential and important entities to adopt appropriate and proportionate technical, operational, and organisational measures to manage the risks posed to the security of the network and information systems used for their activities, including a minimum set of measures the article enumerates (risk-analysis and information-system security policies, incident handling, business continuity, supply-chain security, security in acquisition and development, effectiveness testing, cryptography, human-resources security, access control, multi-factor authentication and secured communications). Where the operator uses AI in scope of NIS2, the Article 21 measures inherit the ENISA multilayer framework as the reference practice shape, and the mod-102 catalog entries the operator instantiates for NIS2 compliance carry the ENISA-Layer-2 and Layer-3 line items as evidence-contract extensions.

ENISA also publishes an annual **threat-landscape report** and a series of AI-focused threat-landscape publications. These are read by the mod-106 risk-taxonomy owner as inputs to the risk-register — the reports name emerging attack techniques (evolutions in prompt-injection, novel model-theft techniques, sector-targeted adversarial campaigns) that inform the risk-appetite triggers and the sector-augmented risk categories the taxonomy carries. The reports are *inputs*, not obligations; the discipline is that the mod-106 taxonomy-registry owner (a level-50 architect concern) treats each ENISA report as a triage input on the taxonomy at each publication cadence.

## NIST Cybersecurity Framework 2.0

**NIST Cybersecurity Framework 2.0** was published on 26 February 2024 as the successor to CSF 1.1. The 2.0 revision is architecturally significant for the AI-in-OT programme for two reasons: it broadens applicability beyond critical infrastructure to all organisations, and it introduces the **Govern** function as a peer of the five original functions.

CSF 2.0 organises outcomes across six functions:

- **Govern (GV)** — new in 2.0. The organisational context, risk-management strategy, roles and responsibilities, policy, oversight, and supply-chain risk-management outcomes that establish and monitor the enterprise's cybersecurity risk-management strategy. This is the AIMS composition point: the mod-105 AIMS reads NIST CSF Govern as the interested-party requirement shaping the Clause 4.1 organisational context and the Clause 5 leadership outcomes, and the AIMS Clause 9.3 management review incorporates CSF Govern reporting into its inputs.
- **Identify (ID)** — asset management, business environment, governance, risk assessment, risk-management strategy, supply-chain risk management. The AI inventory and the AI-provider register (mod-109) attach here.
- **Protect (PR)** — identity management and access control, awareness and training, data security, information-protection processes, maintenance, protective technology. AI-specific protections (inference-endpoint segmentation, model-asset protection, adversarial-input filtering) are Protect-function outcomes.
- **Detect (DE)** — anomalies and events, security continuous monitoring, detection processes. Behaviour-drift and adversarial-input monitoring are Detect outcomes; they compose with the mod-110 PMS mechanisms as one detection surface.
- **Respond (RS)** — response planning, communications, analysis, mitigation, improvements. The AI-incident pipeline (mod-110) is the enterprise incident-response pipeline with AI-specific attributes; it does not run parallel to the enterprise Respond capability.
- **Recover (RC)** — recovery planning, improvements, communications. Continuity-of-service planning for AI-in-OT is a Recover outcome and interacts with the sector-regulator's specific continuity obligations (e.g., NERC CIP-008/CIP-009).

CSF 2.0 continues to support **profiles** as the mechanism for tailoring the framework to a specific implementation. Sector-specific NIST CSF profiles exist for a number of sectors and sub-sectors <!-- needs-research: verify current published sector-CSF-profile inventory (e.g., manufacturing, EV/XFC, communications, hybrid satellite networks) --> and are read by the operator's programme as the sector-tuned articulation of CSF outcomes. Where a sector CSF profile exists, the operator's control library carries the profile's outcomes as the reference shape for that sector; where none exists, the operator authors a profile against the enterprise reference and the sector regulator's expectations.

The composition move is that CSF 2.0 is the outer scaffold the AIMS composes into; the mod-102 catalog carries CSF-2.0-function edges on every control (Govern, Identify, Protect, Detect, Respond, Recover) so that the operator can render a CSF-profile view of the catalog on demand for the sector regulator's inspection.

## NIST SP 800-82 Rev. 3 — Guide to Operational Technology (OT) Security

**NIST SP 800-82 Rev. 3, "Guide to Operational Technology (OT) Security,"** was issued in 2023 <!-- needs-research: verify Rev. 3 exact publication date --> as a substantial revision of the guide, broadening from industrial control systems to operational technology more generally and updating the security guidance for modern OT environments. SP 800-82 Rev. 3 references NIST CSF and NIST SP 800-53 as the underlying control catalog and provides OT-specific tailoring guidance.

The document is architecturally load-bearing for the AI-in-OT decision surface because it fixes the OT-specific constraints that shape what AI is allowed to do inside the control-system boundary:

- **Segmentation and defence-in-depth.** The Purdue Enterprise Reference Architecture levels (Levels 0 through 5) and the segmentation between the enterprise (IT) zone and the OT zone are foundational. Generative-AI and ML inference services are typically enterprise-zone assets; their reach into OT is constrained by segmentation. Where an AI service must consume OT telemetry, the direction of data flow is OT-to-enterprise through a well-defined boundary (a data diode, a unidirectional gateway, a monitored DMZ). Where an AI service must influence OT, the direction of flow inbound to OT is subject to the sector-regulator's approval regime and to the operator's ICS change-control process.
- **Air-gap and near-air-gap constraints.** Certain OT sub-environments are operated as air-gapped or near-air-gapped for safety or regulatory reasons. AI inference in these environments is limited to models that can be pre-loaded, executed locally without inference-time network egress, and monitored without exfiltrating telemetry outside the boundary. Foundation-model inference against a hosted provider is not generally available in these environments; on-premises inference on hardened, validated model artefacts is the compensating pattern.
- **Availability primacy.** OT security prioritises availability where IT security often prioritises confidentiality. An AI system whose operation could delay or interrupt a control loop is a higher-impact system in the OT context than in the IT context; the mod-107 assurance-tier and the mod-102 applicability filter must carry the OT-cyber-physical-tier dimension so that the assurance depth reflects availability-primacy.
- **Change-control rigour.** OT change control is stricter than IT change control — planned outages are scheduled around production and against sector-regulator constraints, and unplanned changes are treated as incidents. An AI system deployed into OT is subject to the OT change-control regime, not the enterprise IT change-management process. The mod-107 pre-deployment gate for an AI-in-OT system is co-composed with the OT change-control forum; the AI-in-OT change is minuted once against both regimes.

SP 800-82 Rev. 3's tailoring of NIST 800-53 controls to the OT context reads directly onto the mod-102 catalog: entries whose applicability filter includes OT-scoped systems carry SP 800-82-derived tailoring guidance as evidence-contract extensions, and the mod-107 assurance path for an OT-scoped AI system is co-composed with the OT change-control regime the operator already runs.

## Sector-regulator overlays

The horizontal frame (CISA/NCSC, ENISA, CSF 2.0, SP 800-82) is not the enforceable regime; the sector regulator's instrument is. The level-50 architect at a critical-infrastructure operator carries the sector-specific overlay as first-class content in the sector adaptation record.

### NERC CIP for the North American bulk electric system

The **North American Electric Reliability Corporation Critical Infrastructure Protection (NERC CIP)** standards apply to registered entities operating bulk-electric-system facilities in North America (US, Canada, portions of Mexico). The active CIP standard set covers, at minimum, CIP-002 through CIP-014, <!-- needs-research: verify current active standard set; NERC has adopted or is progressing additional standards including CIP-015 addressing internal network security monitoring, and revisions to individual standards are ongoing --> covering critical-cyber-asset identification and categorisation (CIP-002), security-management controls (CIP-003), personnel and training (CIP-004), electronic security perimeters (CIP-005), physical security of BES cyber systems (CIP-006), systems security management (CIP-007), incident reporting and response planning (CIP-008), recovery plans (CIP-009), configuration change management and vulnerability assessments (CIP-010), information protection (CIP-011), supply-chain risk management (CIP-013), and physical security of critical transmission stations and substations (CIP-014).

The CIP framework is enforced through NERC's compliance monitoring and enforcement program with regional entities (WECC, RF, MRO, NPCC, SERC, Texas RE) conducting audits. Non-compliance carries financial penalties assessed against a violation severity level and violation risk factor matrix.

For an AI system inside the operator's BES cyber-system boundary, CIP composition means:

- **CIP-002 categorisation.** An AI system attached to a BES cyber asset inherits the asset's impact rating (High, Medium, Low). The mod-102 applicability filter carries the CIP-impact-rating value as a dimension, and the assurance depth is a function of the rating.
- **CIP-005/CIP-007 boundary and hardening.** An AI system inside the electronic security perimeter is subject to CIP-005 access-control and CIP-007 systems-security-management requirements. Foundation-model inference against an internet-exposed provider from inside the ESP is not generally permitted; the operator either performs inference outside the ESP and consumes results across the boundary under controlled conditions, or performs inference on-premises on validated model artefacts.
- **CIP-010 change management.** An AI system's deployment, configuration change, or update is a CIP-010 change subject to baseline configuration, monitoring, and vulnerability-assessment requirements. The mod-107 pre-deployment gate co-composes with the CIP-010 change-management process.
- **CIP-013 supply-chain.** An AI provider is a CIP-013 vendor. The mod-109 third-party programme carries CIP-013-required diligence and contractual elements for AI providers whose products are used in BES cyber systems.
- **CIP-008 incident reporting.** A reportable cyber-security incident involving an AI-adjacent BES cyber system is a CIP-008-reportable event with the associated timelines and reporting posture. The AI-incident pipeline routes CIP-008 incidents through the CIP-008 reporting path.

### TSA Security Directives for pipelines and rail

The **Transportation Security Administration** exercises cybersecurity authority over surface transportation modes including pipelines and rail. TSA has issued a series of **Security Directives** — the pipeline SDs originally issued in 2021 following the Colonial Pipeline incident and iteratively re-issued and revised, and the freight-rail and passenger-rail SDs issued subsequently. <!-- needs-research: verify current TSA SD numbers, revision dates, and scope; the SDs are re-issued periodically and are typically designated by SD-Pipeline-2021-01, SD-Pipeline-2021-02, SD-Pipeline-2021-02C series, SD-Surface-Transportation, and successor designations --> The SDs require covered entities to implement a cybersecurity implementation plan, conduct annual cybersecurity assessments, report cybersecurity incidents to CISA within defined timelines, and designate a cybersecurity coordinator available at all times.

For an AI system used by a covered pipeline or rail entity, TSA SD composition means:

- **Cybersecurity implementation plan.** The plan enumerates the technical and procedural measures the entity applies; AI-in-OT systems are inventoried and their protections stated. The mod-102 catalog entries the entity applies to its TSA-covered systems carry SD-required attributes.
- **Assessment cadence.** The annual cybersecurity assessment covers AI-adjacent systems inside the TSA-covered environment. The mod-107 assurance programme's cadence for these systems aligns to the SD's annual expectation at minimum.
- **Incident reporting.** SD-defined reporting timelines to CISA apply; the AI-incident pipeline routes SD-reportable incidents through the SD path with the sector-regulator liaison role driving the report.

### EU NIS2 Directive

The **NIS2 Directive** (Directive (EU) 2022/2555) supersedes the original NIS Directive and expands scope significantly across essential and important entities in a broad set of sectors. The transposition deadline for member states was 17 October 2024, and member-state transposition laws have been enacted at varying pace since. <!-- needs-research: verify current transposition status across member states; transposition has been uneven and some member states have missed the October 2024 deadline -->

Two articles are architecturally load-bearing for the operator's AI programme:

- **Article 21 — cybersecurity risk-management measures.** Enumerates the minimum measures essential and important entities must adopt, spanning risk analysis, incident handling, business continuity and crisis management, supply-chain security, security in network and information systems acquisition/development/maintenance, effectiveness assessment, cryptography, human-resources security, access-control policies and asset management, multi-factor authentication and secure communications. Where the entity operates AI in scope of NIS2, the Article 21 measures compose with the ENISA multilayer framework as the reference practice shape.
- **Article 23 — incident reporting.** Requires essential and important entities to notify the competent authority or the CSIRT of *significant* incidents without undue delay and in any event within specific windows: an *early warning* within 24 hours of becoming aware of a significant incident (indicating whether unlawful or malicious acts are suspected), an *incident notification* within 72 hours with an initial assessment, and a *final report* within one month (or, if the incident is ongoing, a progress report at one month and a final report within one month of handling closure). <!-- needs-research: verify exact Article 23 reporting-window language against the adopted directive text --> The AI-incident pipeline routes NIS2-reportable incidents through the Article 23 path with the sector-regulator liaison role driving the notification cadence and the incident commander with AI scope holding the operational role.

### ISACs — intelligence-sharing partners, not regulators

Sector Information Sharing and Analysis Centres — the **Electricity Information Sharing and Analysis Center (E-ISAC)** for the electric sector, **WaterISAC** for the water and wastewater sector, the **Downstream Natural Gas ISAC (DNG-ISAC)** and the **Oil and Natural Gas ISAC (ONG-ISAC)** for oil and gas, others for aviation, rail, and maritime — are threat-intelligence and information-sharing partners the operator engages with. They are *not* regulators; participation is not mandatory and their communications are not enforceable instruments. The composition move the sector adaptation record makes is to name the operator's ISAC memberships as intelligence-sharing partners under mod-106 (risk taxonomy inputs) and mod-110 (PMS-incident intelligence exchange), and to keep the ISAC channel out of the sector-regulator overlay proper. Confusing an ISAC advisory with a regulatory obligation is a category error that erodes governance discipline.

## The AI-in-OT boundary discipline — three invariants

The sector's most consequential architectural decision is the boundary between AI systems and the operational-technology surface. The decision cannot be made once and forgotten; it is enforced at every deployment. Three invariants apply.

**Invariant 1 — no-write-to-safety-instrumented-system.** A safety-instrumented system (SIS) — the layer of instrumentation that brings a process to a safe state when the basic process control system fails, per IEC 61511 and IEC 61508 — is not a legitimate write target for an AI inference. The reason is architectural: an SIS is engineered to a safety-integrity-level (SIL) using proven-in-use, deterministic components with verified failure modes; an AI inference is not deterministic in the sense the SIL calculation depends on and cannot be substituted into an SIS without invalidating the SIL. The invariant is enforced as a policy-as-code guard registered against mod-103's egress-boundary policy template, parameterised per sub-sector (wind-farm operator's SIS scope differs from rail signalling's SIS scope, from a refinery's SIS scope, from a water-treatment plant's SIS scope), with the enforcement point at the inference-serving egress.

**Invariant 2 — human-in-the-loop-for-actuation.** Where AI inference informs an actuation decision that reaches the physical process, a human authorised operator retains the actuation decision. The AI system produces a recommendation, an alert, or a prioritised action list; the operator issues the actuation command. This applies especially to autonomous or semi-autonomous behaviour patterns that would otherwise close the loop from inference to actuation without human intermediation. The invariant is enforced as a policy-as-code guard on inference systems whose output routes to actuation surfaces, with the enforcement point at the actuation pathway rather than the inference itself.

**Invariant 3 — segmented-inference-boundary.** AI inference services and the OT environment are segmented; the boundary is a well-defined, monitored, minimally permissive interface. Inference services are not permitted to reach into OT arbitrarily; OT-to-inference telemetry flows through controlled paths; inference-to-OT recommendation flows through the operator's control-system interfaces subject to the operator's change-control regime. Air-gap crossing — where an air-gapped or near-air-gapped OT environment ingests inference results from an enterprise-zone service, or emits telemetry outside the boundary for enterprise-zone inference — is a policy-as-code-guarded exception subject to explicit council-level authorisation.

Each of the three invariants is authored as a parameterised policy-as-code guard (mod-103 chapter 05 fixes the template shape). The parameterisation covers the sub-sector, the specific SIS scope, the specific actuation surfaces, and the specific segmentation topology; the guard body is stable across the operator's environments, and only the parameters change. This is I4 from chapter 01 — sector-specific behaviour comes from parameters on shared templates, not from sector-forked policy programmes.

## The sector adaptation record — critical-infrastructure operator

Filling in the schematic from chapter 01 for a critical-infrastructure operator, the sector adaptation record reads:

```yaml
sector_adaptation_record:
  id: SAR-CRITICAL-INFRA-OP-v1.0
  ratified_by: ai-governance-council (mod-112 ch 01) on <ratification-date>
  reference_architecture_version:
    control_catalog: mod-102 catalog v<X.Y>
    reference_profile: enterprise-reference-profile v<X.Y>
    aims_scope_statement: mod-105 ch 04 statement v<X.Y>
    risk_taxonomy: mod-106 taxonomy v<X.Y>
    obligation_register: mod-104 register v<X.Y>

  sector:
    identifier: critical-infrastructure-operator
    sub_sectors_in_scope:
      - electric-utility-bulk-electric-system
      - oil-and-gas-midstream-downstream
      - water-and-wastewater
      - transportation-pipeline
      - transportation-rail
      - transportation-aviation
      - nis2-essential-entity
    scope_narrative: |
      The enterprise operates critical-infrastructure assets subject to
      cyber-physical safety and continuity-of-service constraints. AI use is
      layered onto an existing ICS/OT security programme and composes with the
      sector regulator's specific instruments. In-scope AI systems include
      enterprise-zone analytics that consume OT telemetry, on-premises
      inference at or near the OT boundary, and any AI system whose output
      reaches an actuation, dispatch, or safety-adjacent decision.

  anchor_regulations:
    horizontal:
      - identifier: CISA-NCSC-Secure-AI-System-Development
        title: Guidelines for Secure AI System Development
        publisher: NCSC-UK with CISA and international partner agencies
        instrument_type: guidance
        obligation_register_ids: [ OBL-CISA-NCSC-SEC-AI-DEV-01 ]
      - identifier: ENISA-Multilayer-Framework-AI
        title: Multilayer Framework for Good Cybersecurity Practices for AI
        publisher: ENISA
        instrument_type: framework
        obligation_register_ids: [ OBL-ENISA-MULTILAYER-01 ]
      - identifier: NIST-CSF-2.0
        title: Cybersecurity Framework 2.0
        publisher: NIST
        instrument_type: framework
        obligation_register_ids: [ OBL-NIST-CSF-2-0-01 ]
      - identifier: NIST-SP-800-82-Rev-3
        title: Guide to Operational Technology (OT) Security
        publisher: NIST
        instrument_type: guidance
        obligation_register_ids: [ OBL-NIST-SP-800-82-R3-01 ]
      - identifier: EU-AI-Act-Annex-III
        title: EU AI Act — Annex III applicability where AI system is used in a
          management or operation of critical digital infrastructure, road
          traffic, or the supply of water, gas, heating and electricity
        publisher: European Union
        instrument_type: regulation
        obligation_register_ids: [ OBL-EU-AI-ACT-ANNEX-III-CI-01 ]
        # <!-- needs-research: verify exact Annex III item text and item number
        #      for critical-infrastructure use cases in the final adopted text -->
    sector_specific:
      - identifier: NERC-CIP-002-through-014
        title: NERC Critical Infrastructure Protection Standards
        jurisdiction: US-federal-plus-Canada
        supervisor: NERC and regional entities (WECC, RF, MRO, NPCC, SERC, Texas RE)
        instrument_type: reliability-standard
        applies_when: entity is a registered functional-entity operating BES assets
        obligation_register_ids: [ OBL-NERC-CIP-002-01, OBL-NERC-CIP-005-01,
          OBL-NERC-CIP-007-01, OBL-NERC-CIP-008-01, OBL-NERC-CIP-009-01,
          OBL-NERC-CIP-010-01, OBL-NERC-CIP-011-01, OBL-NERC-CIP-013-01,
          OBL-NERC-CIP-014-01 ]
      - identifier: TSA-SD-Pipeline-and-Rail
        title: TSA Security Directives for Pipelines and Rail
        jurisdiction: US-federal
        supervisor: Transportation Security Administration
        instrument_type: security-directive
        applies_when: entity is a TSA-covered pipeline or rail operator
        obligation_register_ids: [ OBL-TSA-SD-PIPELINE-01, OBL-TSA-SD-RAIL-01 ]
        # <!-- needs-research: pin current SD numbers and revision letters -->
      - identifier: NIS2-Directive
        title: Directive (EU) 2022/2555 on measures for a high common level of
          cybersecurity across the Union
        jurisdiction: EU (member-state transposition)
        supervisor: national competent authorities and CSIRTs
        instrument_type: directive-transposed-into-member-state-law
        applies_when: entity is an essential or important entity per the directive's
          scope
        obligation_register_ids: [ OBL-NIS2-ART-21-01, OBL-NIS2-ART-23-01 ]

  profile:
    id: PROFILE-CRITICAL-INFRA-OP-v1.0
    derives_from: enterprise-reference-profile v<X.Y>
    delta_summary: |
      The sector profile selects the enterprise reference set of controls,
      makes mandatory the ICS/OT-scoped subset that the reference profile
      leaves optional, and tunes the assurance-depth and evidence-cadence
      parameters upward for controls whose applicability filter matches the
      cyber-physical-tier and safety-instrumented-system-scope dimensions.
      Additional catalog entries are minimised; sector specificity is
      expressed as evidence-contract extensions on existing controls and as
      policy-as-code guards parameterised for the sub-sector.
    applicability_filter_overrides:
      - dimension: cyber-physical-tier
        added_values: [ enterprise-zone, dmz-zone, ot-supervisory, ot-control, ot-safety ]
        rationale: |
          The reference filter's system-tier dimension does not distinguish
          OT segmentation levels; the cyber-physical-tier dimension carries
          the Purdue-style level mapping required to score assurance depth
          under availability primacy.
      - dimension: safety-instrumented-system-scope
        added_values: [ in-sis-scope, adjacent-to-sis, out-of-sis-scope ]
        rationale: |
          The invariant-1 boundary (no-write-to-SIS) is unenforceable without
          an explicit filter value. This dimension carries the SIS-scope
          determination the policy-as-code guard compiles against.
      - dimension: sector-regulator-jurisdiction
        added_values: [ NERC-CIP, TSA-SD-pipeline, TSA-SD-rail, NIS2, EPA-AWIA,
          FAA-cybersecurity, none ]
        rationale: |
          The sector-regulator overlay differs materially by sub-sector; the
          applicability filter carries the regulator identity so the
          evidence-contract line items resolve correctly.
      - dimension: essential-entity-status
        added_values: [ nis2-essential, nis2-important, not-nis2 ]
        rationale: |
          NIS2 obligations differ by entity classification; the filter carries
          the classification for controls whose evidence-contract line items
          are NIS2-derived.

  evidence_contract_extensions:
    - control_id: AIC-DESIGN-THREAT-MODEL
      obligation_id: OBL-CISA-NCSC-SEC-AI-DEV-01
      added_line_items:
        - artefact: ai-threat-model
          schema: ai-threat-model-v1 (mod-108 registry)
          cadence: per-release plus on-material-change
          owner: OT-AI-security-engineer
          retention: 7-years-or-life-of-system
    - control_id: AIC-SUPPLY-CHAIN-PROVIDER-DILIGENCE
      obligation_id: OBL-CISA-NCSC-SEC-AI-DEV-01
      added_line_items:
        - artefact: model-sbom
          schema: model-sbom-v1
          cadence: per-release
          owner: OT-AI-security-engineer
          retention: 7-years-or-life-of-system
    - control_id: AIC-INCIDENT-RESPONSE-PLAYBOOK
      obligation_id: OBL-CISA-NCSC-SEC-AI-DEV-01
      added_line_items:
        - artefact: ai-incident-playbook-cross-reference
          schema: incident-playbook-cross-ref-v1
          cadence: annual-and-on-material-change
          owner: incident-commander-with-AI-scope
          retention: life-of-system-plus-3-years
    - control_id: AIC-CHANGE-MANAGEMENT-BASELINE
      obligation_id: OBL-NERC-CIP-010-01
      added_line_items:
        - artefact: cip-010-baseline-and-change-record
          schema: nerc-cip-010-baseline-v1
          cadence: continuous
          owner: OT-security-lead-with-AI-nexus
          retention: per-NERC-CIP-record-retention-requirements
    - control_id: AIC-INCIDENT-REPORTING
      obligation_id: OBL-NIS2-ART-23-01
      added_line_items:
        - artefact: nis2-article-23-notification-pack
          schema: nis2-notification-v1
          cadence: on-incident
          owner: sector-regulator-liaison
          retention: 10-years
    - control_id: AIC-INCIDENT-REPORTING
      obligation_id: OBL-NERC-CIP-008-01
      added_line_items:
        - artefact: cip-008-reportable-incident-record
          schema: nerc-cip-008-incident-v1
          cadence: on-incident
          owner: sector-regulator-liaison
          retention: per-NERC-CIP-008-record-retention

  policy_as_code_guards:
    - guard_id: PAC-CI-NO-WRITE-TO-SIS
      template: mod-103 egress-boundary-guard
      parameterisation:
        addressee_scope: safety-instrumented-system-scope in { in-sis-scope }
        egress_boundary: deny-all-inference-write
        sub_sector_variants:
          - wind-farm-operator: sis-scope = { turbine-emergency-stop, wtg-brake-actuation }
          - rail-signalling: sis-scope = { interlocking-command, level-crossing-command }
          - refinery: sis-scope = { esd-actuation, fire-and-gas-actuation }
          - water-treatment: sis-scope = { chlorination-shutdown, high-lift-pump-trip }
      enforcement_point: inference-serving-egress
    - guard_id: PAC-CI-AIR-GAP-CROSSING-GATE
      template: mod-103 boundary-crossing-guard
      parameterisation:
        boundary: enterprise-zone-to-air-gapped-ot-zone
        default: deny
        authorised_exceptions: council-approved-with-time-bound-authorisation
      enforcement_point: dmz-gateway
    - guard_id: PAC-CI-HUMAN-IN-LOOP-FOR-ACTUATION
      template: mod-103 actuation-guard
      parameterisation:
        applies_when: inference output routes to actuation surface
        required_control: authorised-operator-actuation-command
      enforcement_point: actuation-pathway
    - guard_id: PAC-CI-ADVERSARIAL-INPUT-DETECTION-REQUIRED
      template: mod-103 input-validation-guard
      parameterisation:
        applies_when: inference endpoint is internet-exposed or reachable from
          untrusted zone
        required_control: adversarial-input-detector-attached
      enforcement_point: inference-serving-ingress
    - guard_id: PAC-CI-SECTOR-REGULATOR-APPROVED-MODEL-ONLY
      template: mod-103 model-provenance-guard
      parameterisation:
        applies_when: cyber-physical-tier in { ot-supervisory, ot-control }
        required_control: model-artefact-on-approved-list
      enforcement_point: model-registry-load-time

  aims_scope_addendum:
    inclusions: |
      All AI systems inside the enterprise-zone that consume OT telemetry;
      all on-premises inference at or near the OT boundary; all AI systems
      whose output reaches an actuation, dispatch, or safety-adjacent
      decision; all AI systems the sector regulator's applicability defines
      as in-scope.
    exclusions: |
      AI systems whose entire scope is enterprise IT with no OT nexus and no
      actuation or dispatch surface (these remain in the enterprise AIMS
      without the sector addendum). Rationale: composition with the ICS/OT
      programme is not required where no OT nexus exists.
    interfaces:
      to_enterprise_scope: additive; the sector addendum extends the enterprise
        scope for the operator's activities without displacing the enterprise
        default for non-critical-infrastructure AI use.
      to_isms: the AIMS composes with the enterprise ISMS (mod-105 ch 04) and
        with the ICS/OT security programme's management system where the
        operator maintains one distinct from the ISMS.

  risk_taxonomy_augmentation:
    added_categories:
      - id: RISK-CI-SAFETY-OF-SERVICE
        parent_reference_category: safety
        definition: |
          Risk of physical harm to persons, property, or the environment
          arising from unsafe operation of the physical process the AI system
          participates in. Specialisation of the reference safety category
          for cyber-physical systems.
        severity_rubric: aligned to enterprise severity bands with additional
          calibration against SIL/SIF terminology
        owner: OT-AI-security-engineer with process-safety-engineer co-owner
      - id: RISK-CI-CONTINUITY-OF-SERVICE
        parent_reference_category: operational
        definition: |
          Risk of interruption, degradation, or loss of the essential service
          the operator provides (electricity delivery, water supply, gas
          transport, transportation service) arising from AI-adjacent cause.
        severity_rubric: aligned to enterprise severity bands with
          service-outage-minute and customers-affected augmentation
        owner: operations-continuity-lead
      - id: RISK-CI-ADVERSARIAL-MANIPULATION
        parent_reference_category: security
        definition: |
          Risk of an adversary manipulating an AI system's inputs, model, or
          inference behaviour to achieve an operational objective adverse to
          the operator. Specialisation of the reference security category
          with sector-specific threat scenarios.
        severity_rubric: aligned to enterprise severity bands with
          threat-scenario-referenced calibration from ENISA and E-ISAC / ICS-CERT
          threat reporting
        owner: OT-AI-security-engineer
    added_appetite_triggers:
      - trigger_id: APP-CI-SIS-BOUNDARY-BREACH
        threshold: any observed inference write attempt against SIS-scope target
        declaring_seat: chief-security-officer with council-notification
      - trigger_id: APP-CI-CONTINUITY-DEGRADATION
        threshold: service-outage attributable to AI-adjacent cause exceeding
          sector-specific tolerance (e.g., transmission LOL event, water
          service interruption above threshold, pipeline flow interruption)
        declaring_seat: chief-operations-officer with council-notification
      - trigger_id: APP-CI-REGULATOR-REPORTABLE-INCIDENT
        threshold: incident meeting CIP-008, TSA-SD, or NIS2 Article 23
          reportable threshold
        declaring_seat: sector-regulator-liaison with council-notification

  additional_roles:
    - role_identifier: ot-ai-security-engineer
      level: 40
      owner_packet_route: /roles/level-40/ot-ai-security-engineer
      responsibilities_delta: |
        Owns the composition of the AI programme with the ICS/OT security
        programme at the working level. Authors AI threat models for AI-in-OT
        systems, maintains model SBOMs, evaluates adversarial-input
        detection, and represents AI-in-OT concerns to the OT
        change-advisory board.
    - role_identifier: incident-commander-with-ai-scope
      level: 50
      owner_packet_route: /roles/level-50/incident-commander-ai-scope
      responsibilities_delta: |
        Extends the enterprise incident-command role with authority over AI
        incidents that reach the OT boundary or the sector regulator. Holds
        the operational role during NIS2 Article 23 or CIP-008 reportable
        incidents involving an AI system.
    - role_identifier: sector-regulator-liaison
      level: 50
      owner_packet_route: /roles/level-50/sector-regulator-liaison
      responsibilities_delta: |
        Single accountable seat for regulator-facing communications across
        NERC, TSA, and NIS2 competent-authority engagements. Drives NIS2
        Article 23 notification cadences (24h early warning, 72h notification,
        one-month final report), CIP-008 self-reports, and TSA-SD incident
        reports. Prepares the pack for the council's regulator-facing
        reserved matter and coordinates the operator's response to
        supervisory inspections.

  reserved_matters_additions:
    - matter: nerc-cip-self-report-disposition
      trigger: identification of a CIP compliance concern with self-report
        implications on an AI-adjacent BES cyber system
      pack_owner: sector-regulator-liaison
    - matter: tsa-sd-incident-report-disposition
      trigger: incident meeting TSA-SD reportable threshold involving an
        AI-adjacent covered system
      pack_owner: sector-regulator-liaison
    - matter: nis2-article-23-notification-disposition
      trigger: significant incident under NIS2 involving an AI-adjacent system
      pack_owner: sector-regulator-liaison
    - matter: cross-sector-attack-indicator-disposition
      trigger: threat intelligence indicating a coordinated adversary
        campaign against operators in the sector with AI-adjacent indicators
      pack_owner: ot-ai-security-engineer with support from mod-106 taxonomy owner
    - matter: air-gap-crossing-exception-approval
      trigger: business-driven request to route inference or telemetry across
        an air-gapped or near-air-gapped boundary
      pack_owner: ot-ai-security-engineer
    - matter: sis-boundary-exception-approval
      trigger: business-driven request to route inference output toward an
        SIS-scope target
      pack_owner: process-safety-engineer with OT-AI-security-engineer co-signer

  deprecation_path_notes:
    - regulation_id: TSA-SD-Pipeline (specific SD revision)
      superseded_by: successor SD revision (issued periodically)
      migration_deadline: per-SD-effective-date-schedule
      evidence_valid_until: per-SD-transition-provision
      handoff_notes: |
        TSA re-issues pipeline SDs periodically with amended requirements;
        the mod-104 reconciliation architecture carries the transition edge
        so that evidence produced under a superseded SD is preserved for the
        supervisory look-back window and new-SD evidence begins on the
        effective date.
    - regulation_id: NERC-CIP-standard (specific standard revision)
      superseded_by: successor standard revision
      migration_deadline: per-NERC-implementation-plan
      evidence_valid_until: per-NERC-implementation-plan
      handoff_notes: |
        NERC CIP standards are periodically revised via the standards
        development process with implementation plans that state the
        effective date and the window during which the prior version's
        evidence remains inspectable. The obligation register carries the
        transition edge and the sector-regulator liaison confirms the
        implementation-plan-consistent evidence disposition at each revision.
```

The record is versioned; it is a reserved matter at the AI governance council (mod-112 chapter 01) at each version increment. The sector-regulator liaison authors the pack that accompanies the record at each ratification; the OT-AI security engineer defends the applicability-filter overrides and the policy-as-code guard parameterisations.

## The reserved-matters overlay at the mod-112 council

The mod-112 council's reserved-matters list acquires a critical-infrastructure overlay that names the matters the council cannot delegate lower and that the sector-regulator liaison drives the pack for. The overlay includes at minimum:

- **Regulator-facing notification disposition.** Any NIS2 Article 23 notification, CIP-008 self-report, or TSA-SD incident report involving an AI-adjacent system is minuted at the council with the notification posture, the disposition of the operational finding, and the executional accountability for any remediation.
- **Air-gap-crossing exception approval.** Any business-driven request to route inference or telemetry across an air-gapped or near-air-gapped OT boundary is a council reserved matter. The pack states the business case, the compensating controls, the time-bound authorisation window, and the exit criteria.
- **SIS boundary exception approval.** Any business-driven request to route inference output toward an SIS-scope target is a council reserved matter with the process-safety engineer as co-signer on the pack. The council does not have unilateral authority to authorise; the sector regulator's approval regime may attach separately and the pack states the regulator-facing posture.
- **Sector-regulator inspection response.** Preparation of the operator's response to a NERC audit, a TSA inspection, or an NIS2 competent-authority inspection is a council reserved matter. The pack includes the inspector's scope statement, the operator's inventory of relevant systems, the evidence packages the inspector will inspect, and the pre-briefing to the executives who will meet the inspector.

These reserved matters extend rather than replace the mod-112 baseline. The council charter is amended to name the overlay explicitly, and the sector-regulator liaison is brought into standing-attendee status for the meetings at which these matters are addressed.

## Failure modes

**Failure mode 1 — authoring the AI programme without composing with the existing ICS/OT security programme.** A common pattern is that the AI programme is initiated by a chief-data-officer or head-of-AI reporting line and is authored against horizontal AI-governance references (NIST AI RMF, ISO/IEC 42001, EU AI Act) without recognising that the operator already runs an ICS/OT security programme with its own change-control regime, its own inventory, its own incident-response pathway, and its own sector-regulator relationship. The AI programme publishes a change-control process that conflicts with the OT change-advisory board's, an incident-response pathway that conflicts with the CIP-008 or NIS2 Article 23 pathway, an inventory that overlaps with but does not reconcile to the CIP-002 asset list, and a set of governance forums that produce decisions the OT security lead cannot honour. Within one supervisory cycle the sector regulator or the internal audit function discovers the incoherence, the AI programme is subordinated to the ICS/OT security programme by force, and two years of programme-build effort delivers less than the composed programme would have. The architectural defence is the composition-anchor decision this chapter argues: the AI programme composes onto the existing ICS/OT security programme from day one, sharing inventory, change-control, incident pathway, and sector-regulator liaison, with AI-specific evidence-contract extensions and policy-as-code guards layered on top.

**Failure mode 2 — generative-AI inference reaching the SIS boundary because the applicability filter did not carry the SIS-scope dimension.** A business team requests a generative-AI-based operator advisor that consumes OT telemetry and produces recommended actions. The AI system's applicability filter, in the absence of an explicit safety-instrumented-system-scope dimension, matches the enterprise reference set of controls plus the ICS/OT overlay for cyber-physical-tier. The controls compile; the pre-deployment gate rules on the assurance package; the system launches. What the filter never expressed is that the recommended actions include, for a defined subset of scenarios, actions that would target SIS-scope actuation surfaces. The policy-as-code guard for no-write-to-SIS is registered against a template but never resolves against the missing filter dimension, so the guard evaluates against a permissive default. The system, in production, produces a recommendation that an operator — trusting the system's assurance package — enacts against an SIS-scope target. The safety-integrity-level calculation of the SIS is invalidated retroactively; the process-safety programme is in crisis; the sector regulator opens an inspection. The architectural defence is the applicability-filter-dimension budgeting from chapter 01 combined with the invariant discipline of this chapter: the safety-instrumented-system-scope dimension is required, its values are enumerated at profile ratification, and the no-write-to-SIS guard resolves against the dimension at every deployment. Filter-dimension parsimony is a discipline; but the SIS-scope dimension is not a candidate for parsimony trimming — its absence is a category error.

## Summary

The critical-infrastructure operator's AI programme composes onto an existing security-first governance frame authored against ICS/OT threat models and a sector-specific regulatory regime. The composition anchor is *cyber-physical safety and continuity-of-service*: what AI is allowed to do is subordinated to those constraints. The composition instruments read together: NIST CSF 2.0 as the outer scaffold with its new Govern function as the AIMS composition point, NIST SP 800-82 Rev. 3 as the OT-specific tailoring, the CISA / NCSC Guidelines for Secure AI System Development as the four-stage life-cycle recommendations composed onto the enterprise SDLC, the ENISA Multilayer Framework as the layered practice reference (particularly for NIS2 essential and important entities), and the sector regulator's own instrument — NERC CIP for the bulk electric system, TSA Security Directives for pipelines and rail, NIS2 Article 21 and Article 23 for EU essential entities, EPA/AWIA for water, FAA cybersecurity for aviation — as the enforceable overlay. ISACs are threat-intelligence partners and are not confused with regulators. Three invariants discipline the AI-in-OT boundary: no-write-to-safety-instrumented-system, human-in-the-loop-for-actuation, and segmented-inference-boundary; each is a parameterised policy-as-code guard whose parameters carry the sub-sector-specific SIS scope, actuation surfaces, and segmentation topology. The sector adaptation record carries anchor regulations, a profile delta adding cyber-physical-tier, safety-instrumented-system-scope, sector-regulator-jurisdiction, and essential-entity-status filter dimensions, evidence-contract extensions for AI threat modelling and model SBOMs and regulator-facing incident records, policy-as-code guards for the three invariants plus air-gap-crossing and adversarial-input detection and sector-regulator-approved-model-only, an AIMS scope addendum for OT-nexus AI systems, a risk-taxonomy augmentation adding safety-of-service, continuity-of-service, and adversarial-manipulation categories, additional roles (OT-AI security engineer, incident commander with AI scope, sector-regulator liaison), and reserved-matters additions at the mod-112 council for regulator-facing notifications, air-gap-crossing exceptions, SIS boundary exceptions, and sector-regulator inspection response. Two failure modes recur — a parallel AI programme that does not compose with the ICS/OT security programme, and a missing safety-instrumented-system-scope filter dimension that lets generative-AI inference reach the SIS boundary — and both are prevented by the composition-anchor decision and the invariant discipline the chapter fixes. Compose the AI programme onto the ICS/OT security programme, name the SIS-scope filter dimension explicitly, and the critical-infrastructure operator blueprint holds.
