# The GRC-for-AI reference architecture — the system of record inside the enterprise stack

## Why this chapter exists — motivation and the concrete failure the chapter designs against

An enterprise that has spent the last several modules building the constituent parts of an AI governance programme — the mod-102 control library, the mod-105 AIMS documented information, the mod-106 risk register, the mod-107 assurance artefacts, the mod-108 evidence architecture, the mod-109 third-party inventory, the mod-110 post-market surveillance store — has by now produced enough governance mass that where it all lives is no longer a deferrable question. Each module produced its own stores conceptually; the level-50 architect has to compose them into an enterprise-visible shape, terminate their integrations on real platforms the rest of the enterprise already runs, and answer the question the CIO is about to ask: "so is this a new system we are standing up, or is it an extension of what we already have?"

The failure mode this chapter designs against has two shapes and they are the two shapes the architect is most likely to be led into by the vendor-driven procurement conversation. The first is that the GRC-for-AI platform is stood up as an isolated island — the integrations are named in the vendor's product deck as "roadmap" or "available via API" and are treated as an afterthought during rollout. Six months later, the platform holds the AI control library and the risk register but nothing writes into it automatically; the model registry is a manual link in a comment field; the ticketing system does not know when a control test is due; the identity provider is out-of-band from the platform's own user table; and the platform's assurance value collapses because the artefacts it holds are stale within days of being loaded. The second failure mode is the opposite: the enterprise, having already invested heavily in a general-purpose enterprise GRC platform for financial, operational, and cyber risk, allows the AI programme to reproduce the enterprise GRC's shape inside a parallel AI-GRC platform. Now there are two systems of record for the enterprise's risk portfolio, two taxonomies that partially overlap, two workflow engines with different SLAs, and two dashboards the board is asked to consult. The reconciliation cost of that duplication is a permanent tax on the second and third lines, and it grows every quarter.

The architect's answer is a *reference architecture* — a target-state artefact that names the components, their interfaces, the coordination roles that own each edge, and the invariants that hold across them. The reference architecture is what makes the build-vs-buy conversation productive, because build-vs-buy is scoped against a concrete shape rather than against a vendor deck; and it is what makes the two failure modes visible early, because both of them are visible in the reference architecture as missing edges or as edges that terminate on a store the enterprise already runs. This chapter authors that architecture.

## The two-store composition — GRC-for-AI as system of record, enterprise GRC as composition target

The reference architecture opens with a load-bearing statement that the rest of the design derives from: the GRC-for-AI platform is the *system of record* for AI-nexus governance artefacts, and the existing enterprise GRC platform is *composed with, not replaced*.

The system-of-record designation means that the AI control library (mod-102), the AIMS documented information (mod-105), the AI risk register (mod-106), the evidence artefact index (mod-108), the third-party AI inventory (mod-109), the PMS event store (mod-110), and the assurance artefacts (mod-107) all live in the GRC-for-AI platform as their authoritative home. When the ISO/IEC 42001 certification body asks to see the AIMS documented information, the auditor is pointed at the GRC-for-AI platform. When the EU AI Act competent authority under Article 26 or Article 16 asks for the technical documentation and the post-market surveillance record, the platform is what is packaged for handover. When the SR 11-7 model validation lead pulls the validation record for an AI-nexus model, the platform is where the record lives. There is one home per artefact class, and it is the GRC-for-AI platform.

The composition-with-not-replacement of enterprise GRC is the discipline that prevents the parallel-platform failure mode. The enterprise GRC platform already holds the enterprise risk taxonomy at the top level, the enterprise controls catalogue for non-AI risk, the audit committee's board-facing dashboards, the third-line internal audit's working papers, and the enterprise-wide issues and actions register. The AI programme does not reproduce any of that. Instead, the GRC-for-AI platform's risk register carries an enterprise-taxonomy reference field on every entry (per mod-106) so that AI-nexus risks aggregate cleanly into the enterprise portfolio view held in the enterprise GRC platform; the AI programme's issues escalate into the enterprise issues register with a stable back-reference; the audit committee's board dashboard consumes an aggregation that the enterprise GRC platform builds by joining across both platforms.

The composition is bidirectional. The enterprise GRC platform sends the AI programme the enterprise policy library (so AI-specific policies inherit and reference enterprise policy), the enterprise risk taxonomy version pins (so mod-106 mappings are always resolvable), and the audit committee's cadence calendar (so the AI programme's audit-facing packaging aligns with the enterprise cycle). The GRC-for-AI platform sends the enterprise GRC platform the aggregated AI-nexus risk exposures at the granularity the enterprise portfolio view expects, the AI-nexus issues in a shape that maps onto the enterprise issues schema, and the AI-nexus incidents that cross the enterprise incident-materiality thresholds. Both directions are versioned, both are contract-shaped, and both are named in the reference architecture as first-class edges.

Naming vendors at the category level for orientation: enterprise GRC / integrated risk management (IRM) platforms in this class typically include RSA Archer, ServiceNow GRC/IRM, MetricStream, OneTrust, and LogicGate. `<!-- needs-research: verify which of these platforms explicitly market AI-governance modules or extensions as of authoring date and whether such modules obviate the GRC-for-AI-platform layer for a given enterprise's ambition level -->` The architect does not pick a winner; the architect designs the composition seam so that whichever enterprise GRC platform is already in use terminates on the GRC-for-AI platform through a stable contract.

## The integrations the reference architecture must terminate on

The reference architecture is not complete until every integration edge has a named counterparty on the enterprise side, a direction (bidirectional-by-design or explicitly one-way), an owner role, and a schema-versioning discipline. The following edges are the ones every enterprise deployment terminates on; adding others is possible but subtracting from this set is where the isolated-island failure mode begins.

### ML platforms — model registries, feature stores, evaluation harnesses

The GRC-for-AI platform's AI inventory is not the authoritative registry of models; the enterprise's ML platform is. Category-level: MLflow, Vertex AI Model Registry, SageMaker Model Registry, Databricks Model Registry, and Weights & Biases Model Registry are the shapes this integration terminates on. The GRC-for-AI platform *consumes* the registry as its inventory ground truth — the `system_id` in the reference architecture is joined against the registry's model identifier — and *emits back* to the registry the governance state that model registries can carry as tags or metadata: the tier classification (mod-106), the deployment-gate decision (mod-107 chapter 02), the current control-attestation state (mod-102), and the AIMS-scope flag (mod-105).

The edge is bidirectional-by-design. When a new model version is registered, the GRC-for-AI platform receives a webhook and creates or updates the corresponding governance record, seeding it with the required intake fields (mod-102 exercise 05's control-authoring intake shape applied here to a system rather than a control). When a deployment-gate decision is issued in the GRC-for-AI platform, the registry receives the update as a metadata write on the model version, so that platform-level deployment tooling can enforce the gate at the point of promotion. Similarly for the feature store — category-level: Feast, Tecton, Vertex AI Feature Store, SageMaker Feature Store — the GRC-for-AI platform consumes feature-set metadata to bind data-provenance controls (mod-102 `AIC-DATA-*` family) to their evidenced substrate; and for the evaluation harness — category-level: internal harnesses, ML platform-native evaluation runners, third-party evaluation services — the GRC-for-AI platform consumes evaluation-run artefacts as evidence per the mod-108 evidence contract.

### AI observability platforms

The AI observability layer produces the operational signal the mod-110 PMS store consumes; the wiring pattern is designed in detail in chapter 06 of this module (which walks the specific vendor-adjacency shape) and in mod-110 chapter 03 (which walks the two-store convergence and the normalisation layer). Category-level: Fiddler AI, Arthur AI, WhyLabs, Evidently AI, and adjacent offerings from Datadog, Dynatrace, and the ML platforms' native monitoring components. The GRC-for-AI platform terminates on the normalisation-layer output described in mod-110 chapter 03, not on the observability platform directly — the invariant is that the observability platform is a *producer*, the GRC-for-AI platform is a *consumer*, and neither is authoritative over the raw telemetry (which lives in the enterprise data lake).

### AI runtime security

The AI runtime security layer produces the security-shaped signal the SOC and the mod-110 PMS store both consume; the wiring pattern is designed in detail in chapter 05 of this module and in mod-110 chapter 05 (the SOC interface). Category-level: Robust Intelligence, Lakera Guard, Calypso AI, HiddenLayer, Protect AI, and adjacent capabilities emerging from established security vendors. The GRC-for-AI platform terminates on the shared-id space and the crosswalk tables the mod-110 chapter 05 interface defines; AI-nexus security incidents write back through the interface into the PMS store as first-class residents.

### Enterprise identity and access

The invariant here is single-source-of-truth: the enterprise identity provider (IdP) is the *only* source of identity for the GRC-for-AI platform, and group memberships are mastered elsewhere and provisioned in through SCIM. Category-level: Okta, Microsoft Entra ID, Ping Identity, and the enterprise's SSO layer sitting in front. The GRC-for-AI platform does not maintain its own user table; the RBAC + SoD model (chapter 04 of this module) is expressed as roles bound to IdP-mastered groups, and every role-assignment change is an IdP event, not a platform event.

The reason this invariant is load-bearing is joiners-movers-leavers hygiene: a user who leaves the enterprise must lose access to the GRC-for-AI platform as an automatic consequence of the IdP off-boarding event, not as a platform-side manual task. The mod-107 chapter 02 pre-deployment assurance gate depends on being able to prove that only currently-employed assurance reviewers signed off; if the platform holds a separate user table, that proof requires reconciliation the third-line audit will always find fault with.

### Ticketing systems

Control-testing tasks, exception approvals, evidence-collection work-items, and remediation actions all flow through the enterprise's existing ticketing platform, not through a GRC-for-AI-native task tracker. Category-level: Jira, ServiceNow ITSM, Azure DevOps Boards, and the enterprise's ticketing standard. The GRC-for-AI platform emits work-items into the ticketing platform as tickets tagged with the originating governance-record identifier; the ticketing platform emits status updates back into the GRC-for-AI platform through webhook. The workflow layer (chapter 03 of this module) walks the state-machine shape.

The invariant is that operational work does not fork: an analyst who lives in Jira for their day job does not have to also live in the GRC-for-AI platform's task view to see what is assigned to them. Duplicated task queues are one of the fastest paths to the isolated-island failure mode, because the platform's tasks are silently ignored by anyone whose primary tool is not the platform.

### Communications — Slack, Teams, email

Human-in-the-loop notifications, escalation alerts, appetite-alarm firings, and executive briefings route through the enterprise's communications platforms. Category-level: Slack, Microsoft Teams, and enterprise email. The GRC-for-AI platform does not build its own notification channel that competes with these; it uses them. The mod-110 chapter 04 escalation workflow and the chapter 04 RBAC model both derive from IdP-mastered group membership to determine notification routing, and the notification payload carries a deep link back into the GRC-for-AI platform for the acknowledgement action.

### Enterprise data lake

Per mod-110 chapter 03, raw telemetry lives in the enterprise data lake, not in the GRC-for-AI platform. The platform holds aggregated, register-tier state; the lake holds raw retention for regulator replay and third-line audit sampling. The edge is one-way from the platform's perspective — the platform reads lake pointers when needed and writes register-tier events that the lake ingests through the normalisation layer — and the retention windows on the lake are set by regime obligation, not by GRC-for-AI-platform storage economics.

### Enterprise document management

Evidence artefacts already produced by other enterprise functions — model cards produced by ML teams in Confluence, policy documents held in SharePoint, meeting minutes captured in Google Workspace, DPIAs held in the privacy office's document store — are *referenced* by the GRC-for-AI platform's evidence artefact index (mod-108), not re-uploaded into it. Category-level: SharePoint, Confluence, Google Workspace, Box. The invariant is that a document has one authoritative location; the GRC-for-AI platform's evidence index carries the pointer, the version-pin, the freshness contract, and the mod-108 attestation shape, but the document itself lives where the producing function keeps it.

## The reference-architecture schematic

The following YAML sketches the reference architecture at the shape the rest of this module fills in. It is illustrative; enterprises adapt the component naming to their existing vocabulary.

```yaml
reference_architecture:
  id: GRC-AI-REF-ARCH-v1.0
  system_of_record:
    platform: grc-for-ai-platform
    role: authoritative store for AI-nexus governance artefacts
    hosts:
      - mod-102-control-library
      - mod-105-aims-documented-information
      - mod-106-ai-risk-register
      - mod-107-assurance-artefacts
      - mod-108-evidence-artefact-index
      - mod-109-third-party-ai-inventory
      - mod-110-pms-event-store-register-tier
    identity_source: enterprise-idp (only)
    user_table: none (mastered upstream)

  composed_with:
    platform: enterprise-grc (RSA Archer / ServiceNow GRC / MetricStream / OneTrust / LogicGate class)
    role: enterprise system of record for non-AI risk; portfolio aggregation target
    contract:
      inbound_from_grc_for_ai:
        - aggregated_ai_risk_exposure_per_enterprise_taxonomy_node
        - ai_nexus_issues_in_enterprise_issue_schema
        - ai_nexus_incidents_crossing_enterprise_materiality
      outbound_to_grc_for_ai:
        - enterprise_policy_library
        - enterprise_risk_taxonomy_version_pins
        - audit_committee_cadence_calendar

  integrations:
    ml_platforms:
      counterparty: model-registry / feature-store / evaluation-harness
      category_examples: [ mlflow, vertex-ai, sagemaker, databricks, weights-and-biases ]
      direction: bidirectional
      owner_role: senior-ai-governance-architect + ml-platform-lead
      contract: system_id join; tier + gate-decision metadata write-back; evaluation-run artefact ingest

    ai_observability:
      counterparty: normalisation-layer output (per mod-110 ch 03)
      category_examples: [ fiddler, arthur, whylabs, evidently ]
      direction: one-way (consume normalised events)
      owner_role: senior-ai-governance-architect + data-platform-lead
      contract: normalised event schema v1.x; register-tier write per mod-110 ch 02

    ai_runtime_security:
      counterparty: security signal via mod-110 ch 05 interface
      category_examples: [ robust-intelligence, lakera-guard, calypso-ai, hiddenlayer, protect-ai ]
      direction: bidirectional (via SOC interface)
      owner_role: senior-ai-governance-architect + ai-infra-security-lead
      contract: PMS-SOC-INT shared-id space; ATLAS-to-mod-106 crosswalk

    identity:
      counterparty: enterprise-idp
      category_examples: [ okta, entra-id, ping ]
      direction: one-way (consume identity + group)
      owner_role: enterprise-iam
      contract: SSO (SAML / OIDC) + SCIM provisioning; no local user table

    ticketing:
      counterparty: enterprise-ticketing-platform
      category_examples: [ jira, servicenow-itsm, azure-devops-boards ]
      direction: bidirectional
      owner_role: senior-ai-governance-architect + ticketing-platform-owner
      contract: ticket-emit with governance-record-id; status webhook return

    communications:
      counterparty: enterprise-messaging + email
      category_examples: [ slack, teams, email ]
      direction: one-way (emit notifications)
      owner_role: enterprise-collaboration-platform-owner
      contract: deep-link payload; IdP-group-driven routing

    enterprise_data_lake:
      counterparty: mod-110 ch 03 two-tier lake
      direction: bidirectional (write register-tier via normalisation; read lake pointers on demand)
      owner_role: senior-ai-governance-architect + data-platform-lead
      contract: normalised event schema; lake pointer format; retention per regime obligation

    document_management:
      counterparty: enterprise-dms
      category_examples: [ sharepoint, confluence, google-workspace, box ]
      direction: one-way (reference pointers; do not upload copies)
      owner_role: enterprise-content-owner
      contract: mod-108 evidence-index pointer + version-pin + freshness contract

  invariants:
    - single system of record for AI-nexus governance artefacts (the grc-for-ai platform)
    - every integration edge is bidirectional-by-design or explicitly one-way (never accidental)
    - the enterprise IdP is the only source of identity; no local user table
    - the enterprise GRC is composed with, not replaced; portfolio aggregation is upstream
    - the enterprise data lake holds raw telemetry; the platform holds register-tier state
    - documents live where produced; the platform holds pointers, not copies

  coordination_roles:
    architect: senior-ai-governance-architect (level 50) — owns the reference architecture and every edge shape
    workflow: senior-ai-governance-architect + head-of-ai-governance (level 60) — ratifies the workflow layer (ch 03)
    rbac: senior-ai-governance-architect + enterprise-iam + head-of-ai-governance — ratifies the RBAC + SoD model (ch 04)
    security: senior-ai-governance-architect + ai-infra-security-lead (level 35) — co-owns the runtime-security adjacencies (ch 05)
    observability: senior-ai-governance-architect + data-platform-lead — co-owns the observability adjacencies (ch 06)
```

## The build-vs-buy conversation the architect owns

The reference architecture is the target-state artefact that scopes the build-vs-buy conversation. Absent the architecture, the conversation is a vendor-deck comparison: which platform has the longest feature list, which has the shiniest dashboard, which has the most credible logos on its customer slide. With the architecture, the conversation is a fit-and-gap: for each named component, integration, and invariant, does the candidate platform (or the build option) satisfy the requirement, satisfy it with modification, or fail to satisfy it? The vendor-evaluation matrix (chapter 02 of this module) takes this shape directly — every row is a requirement lifted from the reference architecture, and every column is a candidate platform.

The build side of the conversation is scoped equally by the architecture. Building the GRC-for-AI platform in-house does not mean building every component named above; it means building the *system of record* and the *workflow layer* and buying, adopting, or federating everything else. The identity-provider integration is not built; the enterprise IdP is used as-is. The document management is not built; the enterprise DMS is used as-is. The lake is not built; the enterprise data lake per mod-110 chapter 03 is used as-is. The reference architecture is what makes this decomposition visible, so that the build option is not accidentally scoped to include re-building the enterprise's identity plane or its content platform.

Three build-vs-buy anti-patterns the architecture makes visible:

- **The "we'll build it in ServiceNow" pattern**, where the enterprise decides to extend its existing enterprise GRC platform with AI-specific modules rather than stand up a dedicated GRC-for-AI platform. This is a legitimate composition in some enterprises — the enterprise GRC platform's workflow engine, RBAC model, and audit-facing packaging may be strong enough to host the AI programme's artefacts natively — but the architecture makes visible the specific requirements the enterprise GRC platform must satisfy: the evidence-schema flexibility to hold mod-108 evidence contracts with their freshness properties, the risk-register schema flexibility to hold the mod-106 taxonomy at the required granularity, and the observability-ingest capacity to receive the mod-110 normalised event stream at production volume. The composition is real; it is not free of engineering; and the architecture is what scopes the engineering.
- **The "we'll buy a vendor and integrate later" pattern**, where a GRC-for-AI vendor is procured with the integration work deferred to a later phase. This is the isolated-island failure mode in its most common commercial form. The architecture makes visible that the integrations are the majority of the value; deferring them is deferring the value, not deferring the work.
- **The "we already have observability, we don't need GRC-for-AI" pattern**, where the AI programme leans entirely on the observability platforms as its system of record. This confuses producer with authority — the observability platforms are producers, not sources of authority, per mod-110 chapter 03 — and it leaves the AIMS documented information, the control library, and the assurance artefacts without a home. The architecture makes visible that observability is one edge among many, not the whole surface.

## The invariants — testable and load-bearing

The invariants named in the schematic are not aspirational; each is testable, and the reference architecture is defective when any of them fails. The architect should be able to point at the artefact each invariant produces and demonstrate its enforcement.

**Invariant 1 — single system of record for AI-nexus governance artefacts.** The artefact is the platform's inventory of hosted artefact classes and the mapping of each class to its authoritative platform. Failure mode: the mod-105 AIMS documented information is held in a wiki and referenced from the platform. When the certification body asks to see the AIMS, the auditor is pointed at two places, and the version reconciliation between them becomes the audit finding.

**Invariant 2 — every integration edge is bidirectional-by-design or explicitly one-way.** The artefact is the integration table in the schematic, with the `direction` field populated for every edge. Failure mode: an edge is stood up as one-way for expediency (the ticketing integration emits work-items but never receives status updates back) and the platform's tasks silently diverge from the ticketing platform's reality within a quarter.

**Invariant 3 — the enterprise IdP is the only source of identity.** The artefact is the platform's user-table configuration set to "SCIM-provisioned, local user creation disabled." Failure mode: local users are permitted "for emergency access" or "for external auditors" and the joiners-movers-leavers hygiene collapses. When the third-line audit samples current access, the sample includes users whose enterprise employment ended months ago.

**Invariant 4 — enterprise GRC is composed with, not replaced.** The artefact is the composition contract in the schematic — the inbound and outbound field lists between the two platforms. Failure mode: the AI programme's risk register grows an enterprise-taxonomy-mirroring parallel taxonomy that the enterprise GRC platform is not aware of, and the audit committee receives two portfolio views with no reconciliation.

**Invariant 5 — the enterprise data lake holds raw telemetry; the platform holds register-tier state.** The artefact is the mod-110 chapter 03 two-tier retention configuration and the register-tier-only storage discipline on the GRC-for-AI platform. Failure mode: the platform accumulates raw telemetry in its own database, storage costs balloon, query performance degrades, and the regulator's point-in-time replay request cannot be served from either store because both hold partial views.

**Invariant 6 — documents live where produced.** The artefact is the mod-108 evidence-index pointer schema and the discipline that the index carries pointers, not copies. Failure mode: model cards produced in Confluence are copied into the platform's document store, drift, and the auditor is shown a stale copy while the model-owning team continues to update the Confluence original.

## Coordination — the peer and adjacent role interfaces

The reference architecture is the level-50 architect's artefact, but it terminates on components several other roles own. Naming the coordination interfaces explicitly:

- **Enterprise IAM lead** owns the IdP, the SCIM provisioning, and the group-membership discipline. The RBAC + SoD model (chapter 04) is co-designed and co-ratified with this role.
- **Data platform lead** owns the enterprise data lake, the normalisation layer (per mod-110 chapter 03), and the streaming infrastructure that carries observability events from producers to the lake and the platform. The observability adjacencies (chapter 06) are co-owned with this role.
- **ML platform lead** owns the model registry, the feature store, and the evaluation harness. The ML-platform integration edge is co-designed with this role and terminates on the registry's webhook and metadata-write surface.
- **`ai-infra-security` (level 35)** owns the runtime-security adjacencies and the SOC interface (per mod-110 chapter 05). Chapter 05 of this module walks the co-owned interface in detail.
- **Enterprise ticketing platform owner** owns the ticketing integration edge and the ticket-schema evolution.
- **Enterprise collaboration platform owner** owns the communications integration edge.
- **Head of AI governance (level 60)** ratifies the reference architecture at the version boundary. Changes to the architecture — a new integration edge added, an invariant modified, an ownership boundary redrawn — are ratified jointly with this role.
- **CIO and enterprise-architecture board** ratify the composition contract with the enterprise GRC platform, because that composition crosses the enterprise-architecture boundary and touches the enterprise portfolio-view that the CIO is accountable for.

The architect does not build alone, and the reference architecture is not a solo artefact; it is the ratified surface across which several roles coordinate. The stability of the surface is what keeps the coordination low-friction.

## Summary

The GRC-for-AI reference architecture is the target-state artefact the level-50 architect owns; it names the GRC-for-AI platform as the single system of record for AI-nexus governance artefacts (the mod-102 control library, the mod-105 AIMS documented information, the mod-106 risk register, the mod-107 assurance artefacts, the mod-108 evidence index, the mod-109 third-party inventory, and the mod-110 PMS register-tier state), it composes with the existing enterprise GRC platform rather than replacing it, and it terminates on named counterparties in the enterprise stack — ML platforms, AI observability, AI runtime security, identity, ticketing, communications, the data lake, and document management. The two failure modes the architecture is designed against are the isolated-island (integrations deferred, artefacts stale within days) and the parallel-platform duplication (enterprise GRC's shape reproduced inside a separate AI-GRC platform, producing two systems of record). Six invariants hold: single system of record, every edge intentional in direction, IdP as only identity source, enterprise GRC composed with rather than replaced, the lake for raw and the platform for register-tier, and documents referenced where produced rather than copied in. The schematic names components, interfaces, and coordination roles; the vendor-evaluation matrix (chapter 02), workflow layer (chapter 03), RBAC + SoD model (chapter 04), security adjacencies (chapter 05), observability adjacencies (chapter 06), and Singapore AI Verify integration (chapter 07) each fill in a face of the architecture the rest of this module walks in depth.
