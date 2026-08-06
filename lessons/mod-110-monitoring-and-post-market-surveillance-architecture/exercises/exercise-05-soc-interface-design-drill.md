# exercise-05: SOC Interface Design Drill

**Estimated effort:** 2 hours

## Objective

Produce the **AI-governance-to-SOC coordination interface** for a specified enterprise scenario — the artefact the head of AI governance and the CISO would jointly ratify before either function can claim it operates coherently on AI-nexus signal. The interface is what makes the SOC a first-class producer/consumer of AI-nexus signal into the post-market surveillance (PMS) store rather than a function adjacent to governance that each side hopes the other is paying attention to.

The deliverable is a coordination contract, a shared handoff schema, a red-team-to-detection registry, a classification crosswalk, and a closed-world signal registry. Downstream, this interface feeds the chapter 04 Article 73 escalation workflow when signals cross the serious-incident threshold, and it draws on the mod-102 control library (`AIC-SEC-*` and `AIC-ROB-*` families) and the mod-106 risk taxonomy for its category vocabulary. Get the interface right and both functions can operate their own runbook depth under a stable joint surface; get it wrong and the SOC and the governance function each see half of every AI-nexus incident.

The design boundary is strict. You author the interface only. SOC-side detection engineering, playbook internals, tuning, and analyst-tier design are the `ai-infra-security` (level 35) role's scope — you name that boundary in the contract and refuse to encroach on it.

## Prerequisites

- Chapter [`05-the-soc-interface-and-ai-specific-signal-handoff.md`](../05-the-soc-interface-and-ai-specific-signal-handoff.md) read once, with the four invariants and three failure modes marked.
- Chapter [`01-eu-ai-act-article-72-and-the-enterprise-post-market-surveillance-shape.md`](../01-eu-ai-act-article-72-and-the-enterprise-post-market-surveillance-shape.md) for the PMS store as the single source of truth the SOC→PMS handoff writes into.
- Chapter [`04-article-73-serious-incident-reporting-workflow-design.md`](../04-article-73-serious-incident-reporting-workflow-design.md) for the escalation pathway an interface-handled incident enters when it crosses the serious threshold.
- The mod-102 enterprise AI control library — specifically the `AIC-SEC-*` and `AIC-ROB-*` families the crosswalk points at when a SOC detection maps to a governance control failure.
- The mod-106 risk taxonomy — the category vocabulary the crosswalk lands in on the governance side.
- The level-35 `ai-infra-security` role scope in this track's role tree — the boundary the interface must not cross.
- Access to primary references — ISO/IEC 27035 (incident-management process reference), NIST SP 800-61 Rev. 2 (Computer Security Incident Handling Guide), MITRE ATLAS (Adversarial Threat Landscape for Artificial Intelligence Systems), Lockheed Martin Cyber Kill Chain or MITRE ATT&CK for kill-chain phase. See [`../resources.md`](../resources.md).

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose SOC-to-governance interface shape you are least familiar with; that is where the exercise will teach you most. State your choice at the top of the deliverable.

- **A US regional bank** (Northbrook Financial-style, per prior modules) with ~40 AI systems: fraud classifiers, document extraction, an internal RAG legal assistant, a customer-facing generative chat, third-party AI SaaS integrations. An established enterprise SOC aligned to ISO/IEC 27035 and NIST SP 800-61, running a mature SIEM with content-inspection tooling and DLP; an incident-response function with a documented severity ladder and analyst tiering. AI-specific detection engineering is nascent; the CISO reports to the CRO.
- **A global healthcare payer / provider** with clinical-decision-support pilots, patient-facing chat, coding automation, and utilisation-management AI. SOC is 24x7, aligned to NIST SP 800-61, with HIPAA-driven incident-reporting muscle already in place; a clinical safety oversight committee also handles adverse-event reporting. Red-team engagements are procured externally on a per-year cadence.
- **A B2B SaaS platform vendor** shipping GenAI-augmented HR-tech capabilities into enterprise customers across the US, UK, EU, and Singapore. SOC is smaller, co-managed with an MSSP for out-of-hours coverage; incident-response is aligned to ISO/IEC 27035 as part of the enterprise's ISO/IEC 27001 posture; enterprise customers demand security-incident notification with contractual SLAs measured in hours, and AI-nexus incident semantics are increasingly in the customer's DPA appendices.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`soc-interface-contract-v1.0.0.md`** — the coordination contract: scope, schema reference, SLA per handoff direction, escalation, versioning discipline. Explicitly names the boundary with `ai-infra-security` (level 35) — what the interface covers and what remains SOC-internal.
2. **`signal-handoff-schema.yaml`** — the normalised schema every handoff event uses: shared incident-id, both taxonomies' classifications (SOC severity + kill-chain phase + MITRE ATLAS technique; mod-106 category), timestamps, references to raw telemetry in the data lake (per chapter 03 of this module).
3. **`red-team-to-detection-registry.md`** — at least SIX worked examples where a red-team / evaluation finding becomes a SOC detection candidate **and** a governance-monitor candidate. Each entry names finding description, proposed detection signature, proposed governance monitor, SOC-side owner, governance-side owner, productionisation SLA.
4. **`crosswalk-table.md`** — the mapping from the SOC's incident-taxonomy severity and MITRE ATLAS TTP labels to the mod-106 category set. Every mapping row cites the ATLAS technique by ID (verified against the current matrix) or marks `<!-- needs-research: ... -->`.
5. **`signal-registry.md`** — the closed-world registry of AI-specific signal types with their designated home (SOC, governance, or both), the applicable handoff pattern, and the schema version at which the signal type was admitted. No orphaned signals.

## Requirements

### `soc-interface-contract-v1.0.0.md`

Decide and justify **each** of the following:

- **Scope statement.** What the interface covers (schema, routing, escalation contracts, versioning, shared-id space, crosswalk tables, signal registry) and what it explicitly does not (SOC-internal detection tooling, tuning, playbook internals, analyst tiering, rotation schedules; governance-side classification workflow inside the PMS store). Name the level-35 `ai-infra-security` boundary in the exclusion list.
- **Shared incident-id space.** Format, minting rule (whichever side detected first), the acknowledgement SLA on the reciprocal-side write. This is the first section of the contract; every other section composes off it.
- **The three handoff patterns.** SOC→PMS, PMS→SOC, and red-team→detection — for each, name the trigger, the payload contract (by reference to the schema), the acknowledgement SLA in the receiving direction, and the closure discipline. State the analyst-triage SLA on the receiving side without prescribing analyst tiering.
- **Reference-frame alignment.** State the ISO/IEC 27035 process alignment and the NIST SP 800-61 handling alignment at the interface boundary, where the enterprise SOC in your scenario follows those references. Cite MITRE ATLAS as the AI-specific TTP taxonomy the crosswalk uses.
- **Four-invariant enforcement.** For each of the four chapter-05 invariants (designated home per signal type; red-team finding produces both a detection and a monitor candidate; SOC-detected AI-nexus incidents write to PMS; shared incident id spans both systems), name the specific contractual clause or design choice that enforces it and the test that would detect its violation.
- **Three-failure-mode defence.** For each of the three chapter-05 failure modes (silent SOC, silent governance, red-team-findings-in-Jira), name at least one architectural move the interface makes to prevent it.
- **Versioning discipline.** Semver shape, joint-ratifier list (`head-of-ai-governance` + `CISO`), amendment protocol, deprecation notice on schema fields, backward-compatibility window. Neither side amends unilaterally.
- **Escalation into Article 73.** The handoff into the chapter 04 escalation workflow when an interface-handled incident crosses the serious-incident threshold. Interface-side responsibility ends where the Article 73 workflow's clock starts; state that boundary.
- **Non-scope.** At least three things the interface deliberately does not cover and why. Candidates: SOC-vendor product configuration; per-signal-type detection tuning; governance-side risk-treatment plans on affected systems.

### `signal-handoff-schema.yaml`

Every handoff event uses this schema. Include at minimum:

- `schema_version` — semver; every event carries the version it was serialised under.
- `handoff_type` — one of `SOC_TO_PMS`, `PMS_TO_SOC`, `REDTEAM_TO_DETECTION`.
- `shared_incident_id` — the `INC-YYYY-NNNNNNN` shape from the contract.
- `ai_system_id` — the affected AI system from the enterprise inventory.
- `soc_classification` — SOC severity level, kill-chain phase (Lockheed Martin or MITRE ATT&CK phase), MITRE ATLAS technique id if applicable.
- `governance_classification` — mod-106 category, tier of the affected system, appetite-alarm implication if the classification is known at handoff time.
- `timestamps` — detection-time, handoff-fired-time, receiving-side-ack-time, closure-time. Ack SLA clocks bind to these.
- `raw_telemetry_refs` — pointers to the SIEM record, the PMS record, and to raw telemetry in the chapter-03 data lake so both sides can inspect the same evidence.
- `handoff_state` — one of `open`, `acked`, `in_investigation`, `closed_actioned`, `closed_no_action`, `closed_referred_to_article_73`.
- `owner_seats` — the SOC-side and governance-side named seats for this handoff instance.

Do not over-engineer the schema. Every field must be one both sides materially use; fields present for future-proofing without a stated use are removed.

### `red-team-to-detection-registry.md`

Author at least six worked examples of a red-team / evaluation finding that becomes both a SOC detection candidate and a governance monitor candidate. Each entry:

- **Finding description** — the attack path in one or two sentences. Draw from the AI-specific attack surface: prompt injection variants against RAG systems and agents, jailbreak-then-exfiltrate chains, agentic-tool-misuse via prompt-provided instructions, model-extraction reconnaissance patterns against inference endpoints, adversarial-example patterns designed to move a classifier's decision boundary, training-data-poisoning demonstrations against fine-tuning pipelines.
- **Proposed detection signature** — an English-language sketch of what the SIEM would look for. Do not write vendor-specific rule syntax; you are the architect, not the detection engineer.
- **Proposed governance monitor** — the observability rule or PMS-store trigger the governance side would run.
- **SOC-side owner** — the seat inside the `ai-infra-security` (level 35) chain that owns the detection build.
- **Governance-side owner** — the seat that owns the monitor build.
- **Productionisation SLA** — the business-day decision window (build the detection + build the monitor, or documented decision not to). Match the SLA to the contract.
- **Reference to affected control family** — the mod-102 `AIC-SEC-*` or `AIC-ROB-*` control the detection and monitor together evidence.

### `crosswalk-table.md`

A table with columns: SOC severity, kill-chain phase, MITRE ATLAS technique (with ID), mod-106 category, notes. Cover at least eight representative combinations that the scenario's system inventory actually produces. For every ATLAS technique cell, one of two things must be true: the technique id is verified from the current ATLAS matrix (with citation), or the cell carries `<!-- needs-research: verify current ATLAS technique id for ... -->`. Do not cite ATLAS IDs from memory.

The crosswalk is not one-to-one in all directions; document where a SOC severity maps to multiple governance categories depending on the affected system's capability tier, and where a governance category is reachable through multiple ATLAS techniques. Name the primary mapping and note the qualification logic that resolves the multi-way case.

### `signal-registry.md`

The closed-world registry of AI-specific signal types the enterprise has decided are in scope. Each row: signal-type name, short description, designated home (`SOC` / `governance` / `both`), applicable handoff pattern, schema version admitted, deprecated flag with sunset date if applicable. Cover at minimum the chapter-05 initial inventory (jailbreak attempts, prompt-injection detections, data-exfiltration-via-output flags, model-extraction reconnaissance, agentic-tool-misuse patterns, adversarial-example patterns) and any additional signal types the scenario's system inventory implies. No signal type is left unassigned or ambiguous; if a signal type is admitted to the registry, it has a home.

State the amendment protocol for admitting a new signal type — who proposes it, who ratifies it, and how the interface version bumps.

## Starter guidance

- Design the shared-id space FIRST. Every other artefact composes off it. If the SOC and governance mint different ids for the same incident, you have already lost — the crosswalk, the registry, and the handoff schema all become unjoinable and the audit committee receives two versions of the truth.
- The red-team-to-detection registry is where most enterprise programmes actually break. Ticket-in-Jira is not a productionised detection; a finding without both a named SOC-side owner and a named governance-side owner and an SLA on the decision is a finding that will be forgotten.
- Do not encroach on SOC-internal detection engineering — that is the `ai-infra-security` (level 35) role. Design the interface only. If you find yourself writing SIEM rule syntax or specifying analyst-tier assignments, you have crossed the boundary; step back and describe the outcome the interface requires instead.
- MITRE ATLAS technique IDs are versioned; do not cite them from memory. Verify against the current ATLAS matrix at authoring time or mark `<!-- needs-research: ... -->`. The same discipline applies to specific ISO/IEC 27035 clause numbers, NIST SP 800-61 subsection numbers, and ATT&CK tactic identifiers.
- The signal-registry closed-world discipline is what prevents orphaned signals. Every AI-specific detection or governance monitor either appears in the registry with a designated home or is explicitly deprecated. If a signal type shows up in production that is not in the registry, the interface's own alarm fires — that is the enforcement mechanism.
- The interface's own SLA-miss alarm — when either side fails to acknowledge a handoff within the stated window — must fire to both co-owners, not just the receiving side. Silent-SOC and silent-governance both look like a missed ack to the interface's own monitoring.
- Do not invent SOC vendor product features. The interface commits to outcomes (schema, SLA, closure discipline); the SOC vendor's product delivers them through whatever configuration the SOC-side lead chooses.

## Acceptance criteria

- [ ] Scenario is stated at the top of `soc-interface-contract-v1.0.0.md`; the interface is coherent against it.
- [ ] Shared incident-id space is designed and specified before any other section of the contract.
- [ ] All three handoff patterns (SOC→PMS, PMS→SOC, red-team→detection) are named with trigger, payload schema reference, receiving-side ack SLA, and closure discipline.
- [ ] Each of the four chapter-05 invariants has a named contractual clause or design choice that enforces it, plus the test that would detect its violation.
- [ ] Each of the three chapter-05 failure modes (silent SOC, silent governance, red-team-findings-in-Jira) has at least one architectural defence in the contract.
- [ ] The `ai-infra-security` (level 35) boundary is named explicitly in the scope statement and in the non-scope list. No section of the contract or the registries prescribes SOC-internal detection engineering, tuning, playbook internals, or analyst tiering.
- [ ] ISO/IEC 27035 and NIST SP 800-61 alignment is stated where the scenario's SOC follows those references. MITRE ATLAS is cited as the AI-specific TTP taxonomy.
- [ ] `signal-handoff-schema.yaml` includes shared id, both classifications, timestamps, raw-telemetry references to the chapter-03 data lake, handoff state, owner seats. Every field has a stated material use.
- [ ] `red-team-to-detection-registry.md` contains at least six worked examples; every example has finding description, proposed detection signature, proposed governance monitor, SOC-side owner, governance-side owner, productionisation SLA, and reference to the affected mod-102 control family.
- [ ] `crosswalk-table.md` covers at least eight combinations. **Every MITRE ATLAS technique cited by ID is either verified from the current ATLAS matrix (with citation) or marked `<!-- needs-research: verify current ATLAS technique id for ... -->`.**
- [ ] `signal-registry.md` covers at minimum the chapter-05 initial inventory plus any scenario-implied additions; every entry has a designated home; no signal is orphaned; the amendment protocol for admitting a new signal type is stated.
- [ ] Versioning discipline is semver-shaped; `head-of-ai-governance` and `CISO` are named as joint ratifiers; the amendment protocol and backward-compatibility window are specified.
- [ ] The Article 73 handoff into the chapter 04 escalation workflow is specified with the boundary where the interface-side responsibility ends.
- [ ] Non-scope section names at least three things deliberately excluded and why.
- [ ] Every unverified citation to ISO/IEC 27035, NIST SP 800-61, MITRE ATLAS, MITRE ATT&CK, or a scenario-specific regulation is marked `<!-- needs-research: ... -->` — no invented dates, clause numbers, technique IDs, or agency names.

## Stretch goals

- **Live prompt-injection tabletop.** Run a tabletop for a live prompt-injection incident traversing the interface end-to-end: SOC content-inspection detection fires, the SOC→PMS handoff writes to the store, the governance side classifies against mod-106 and updates the residual on the affected category, a follow-on governance monitor flags suspected successful injection, the PMS→SOC handoff fires the hunt, and the incident either closes or escalates into the chapter 04 Article 73 workflow. Produce the timeline with SLA clocks marked at each handoff.
- **Monthly interface metrics.** Design a metric set the interface exports monthly to both the head of AI governance and the CISO: handoff volume by pattern, ack-SLA compliance per direction, closure-state distribution, red-team-to-production time distribution, signal-registry admission rate, SLA-miss alarm firing count. Name the seat that produces the metric report and the audit committee reporting cadence per mod-107.
- **Article 73 integration walkthrough.** Extend the chapter 04 Article 73 workflow specification with the interface-originated escalation path — a SOC-detected incident that qualifies as serious per the Article 73 thresholds triggers the reporting workflow, and the interface's shared incident id is the id the regulator's report carries. Specify the transition of ownership from the interface co-owners to the Article 73 report owner.
- **Third-party origin extension.** Sketch how the interface's three handoff patterns extend to incidents originating at a third-party frontier-model provider — provider-disclosed compromise (SOC→PMS-style handoff), provider-side model-behaviour change surfacing as governance signal (PMS→SOC-style), and provider-run red-team findings shared under contract (red-team→detection-style). Cross-reference to mod-109.
