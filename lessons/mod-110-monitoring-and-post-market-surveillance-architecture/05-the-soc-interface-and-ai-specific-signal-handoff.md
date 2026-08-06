# The Security Operations Center interface — AI-specific signal handoff

## Why this chapter exists

An enterprise that ships AI systems is producing two categories of signal from those systems at all times, and the two categories are the same signal seen from different angles. A sustained pattern of prompt-injection strings arriving at a customer-facing agent is, to the Security Operations Center (SOC), a content-inspection detection with an attacker-behaviour signature; to the AI-governance function, it is a shift in the residual on the "prompt-injection-succeeded" risk category and a candidate appetite-alarm. An output-content flag on a governance monitor showing what looks like internal document contents in the model's reply is, to the governance function, a data-classification alert; to the SOC, it is an active exfiltration channel that ought to be hunted. In both directions, half of the incident lives on one side of the enterprise and half on the other, and neither side alone sees the whole thing.

The architect who has already fixed the post-market surveillance (PMS) store as the enterprise's single source of truth for AI-nexus operational signal (chapter 01), and who has designed the Article 73 escalation workflow (chapter 04), has to fix the *coordination interface* with the SOC — because without it, the PMS store is missing the security signals it needs, the SIEM is missing the governance context that would make its detections useful, and the red-team findings that keep both sides honest die in Jira tickets. The SOC is not adjacent to the AI-governance function; it is a first-class producer of AI-nexus signal into the PMS store and a first-class consumer of AI-nexus signal from it. This chapter designs the interface that makes that true.

The level-50 architect owns the *interface shape* — the schema, the routing, the escalation contracts, the versioning, the shared-id space. The architect does not own the SOC-side runbook depth: the playbooks per signal type, the detection engineering, the threat-intel integration, and the red-team-to-detection engineering are owned by `ai-infra-security` (level 35) and, upstream of that role, by the CISO function. The distinction matters because the interface is the object the two functions ratify jointly; the runbooks and detections are the SOC's to design under that interface. This chapter walks the interface architecture the AI-governance side commits to and the coordination discipline that keeps the two sides in lockstep without either function reaching into the other's operating model.

## Why the SOC is a first-class producer/consumer, not adjacent

Classical enterprise SOCs were built for network intrusions, endpoint compromise, credential abuse, insider threats, and content-based data loss. The reference frames — ISO/IEC 27035 for incident management as a process, NIST SP 800-61 Rev. 2 for computer security incident handling, MITRE ATT&CK for the technique taxonomy on the classical attack surface — assume an attacker acting against classical IT assets. AI systems introduce a set of signals those frames do not, on their own, name: jailbreak attempts, prompt-injection attempts against agents and RAG systems, data exfiltration through the LLM output channel, model-extraction reconnaissance against inference endpoints, adversarial-example patterns designed to move a classifier's decision boundary, and agentic-tool-misuse where the attacker's leverage is the model's tool-calling authority rather than the attacker's own credentials.

MITRE ATLAS — the Adversarial Threat Landscape for Artificial Intelligence Systems — is the AI-specific TTP matrix the security community aligns to; it is the shared classification frame the SOC and the governance function can both point to when they talk about what happened. The architect does not build a private TTP taxonomy for the enterprise; the architect commits the interface to the ATLAS frame and to the classical frames alongside it, and the crosswalk to the mod-106 governance taxonomy is what the interface documents.

<!-- needs-research: verify current ATLAS matrix version and top-level tactic list at time of reading -->

The point is not that the SOC needs to learn AI; the SOC likely already runs some detections on some of these signal types. The point is that those signals need to land in the PMS store *as well as* the SIEM, and the reverse — governance-detected signals need to reach the SIEM — because a residual-shift on the "prompt-injection-succeeded" category depends on the SOC's detection count, and a hunt for an exfiltration channel depends on the governance monitor's content-flag rate. Without a designed interface, each side sees only half of each incident, and both sides make decisions on incomplete evidence.

## Role separation the architect must fix

The interface has two co-owners and two ratifying executives, and the architect must fix which role owns which side of the boundary. Getting this wrong produces either governance overreach into detection engineering (the architect starts writing SIEM rules) or SOC overreach into governance semantics (the SOC starts reclassifying mod-106 categories to match its own severity ladders). Neither works, and both are visible failure patterns.

- The **level-50 architect** designs the *interface* — the schema every handoff event carries, the routing rules that determine which handoff type fires, the escalation contracts that name the SLA in each direction, the versioning discipline that governs how the interface itself changes.
- The **`ai-infra-security` role (level 35)** and the CISO function own the SOC-side runbook depth — the per-signal-type playbooks, the detection engineering that produces the SIEM rules, the threat-intel integration that keeps the detections current against evolving attacker TTPs, the red-team-to-detection engineering discipline that turns adversarial findings into productionised detections.
- The **`ai-evaluation-engineer` role (peer, level 35)** and any contracted red-team owners produce the red-team findings that feed the third handoff type; they are neither the architect's report nor the security lead's, but they are producers into the interface.
- The **head of AI governance (level 60)** and the CISO jointly ratify the interface version. Neither can amend it unilaterally; the interface is the shared surface between the two functions and its stability is what keeps both functions coordinable.

The reference frames the architect cites when talking with the SOC-side function are ISO/IEC 27035 (incident-management process reference), NIST SP 800-61 Rev. 2 (Computer Security Incident Handling Guide), and MITRE ATLAS (the AI-specific TTP matrix). These are cited at title level in the interface document; specific clause references and specific technique IDs are marked `<!-- needs-research: ... -->` at authoring time and verified against the current published versions before the interface goes to ratification.

## The three signal handoff patterns

Every AI-nexus signal the enterprise produces or consumes fits into one of three handoff patterns, and the interface names all three explicitly.

### Pattern A — SOC to PMS

A SOC detection with governance implications flows from the SIEM into the PMS store. The trigger is a security detection whose subject is an AI system in the enterprise inventory — the detection carries an `ai_system_id` that identifies the affected system. The example that recurs: SOC content-inspection rules detect a sustained pattern of prompt-injection strings against a customer-facing agent's inbound message stream. The SOC's own runbook handles the classical response (rate-limit the source, alert the on-call, correlate with other traffic from the same origin). The governance function *also* needs to know because the residual on the mod-106 "prompt-injection-succeeded" category shifts even when no injection actually succeeds — the surface is under active probing, the appetite-alarm on that category may need to fire, and the risk-treatment plan for the affected system may need re-visiting.

Without the SOC→PMS handoff, the governance side's register carries a stale residual on a category the SOC has good reason to believe is under active attack. The register is wrong, and the wrongness is invisible until the injection succeeds and the incident-response retrospective finds that the SOC had been detecting the precursor pattern for months.

### Pattern B — PMS to SOC

A governance signal with security implications flows from the PMS store (or the observability platform feeding it) into the SIEM. The trigger is a governance monitor or observability rule that produces a signal the SOC needs to act on — a content-signal exceeding an SOC-referral threshold, or an attack-pattern signature the governance monitor can identify but only the SOC can hunt against. The example that recurs: a governance monitor on an assistant's output stream flags model output that contains what looks like exfiltrated internal document content. The governance side classifies the finding against the mod-106 taxonomy and records it in the PMS store; the SOC needs the handoff because *the exfiltration channel is the model*, and only the SOC can hunt for the attacker who is exploiting it — session correlation, source-IP analysis, credential-abuse patterns, lateral-movement signals leading up to the flagged output.

Without the PMS→SOC handoff, the governance side has a well-classified incident record and no attacker attribution; the SOC never sees the pattern; the attacker's next exfiltration attempt succeeds against the same channel because nobody is hunting the source.

### Pattern C — Red-team to detection

A red-team or evaluation finding — produced by `ai-evaluation-engineer` (peer, level 35), by an internal AI red-team, or by a contracted external red-team — becomes both a SOC detection rule *and* a governance monitor. The trigger is a finding classified as productionise-able: the red-team demonstrated an attack path that works, the reproduction steps are documented, and the proposed detection signature is at least sketched. The architect designs the *handoff contract* — who owns turning the finding into a detection, who owns turning it into a monitor, and what the SLA on that productionisation is — so that findings do not die in a Jira ticket.

The failure mode this pattern is designed against is the one every mature security programme has seen: a red-team engagement produces twenty findings; five are patched immediately; ten are ticketed and forgotten; five are ticketed with a note that says "productionise as detection" and nothing happens for six months; the same attack path re-emerges as a production incident and the retrospective finds the finding was already known. The interface makes the "and nothing happens" impossible by requiring named owners on both sides and an SLA on the decision (build the detection, build the monitor, or document the decision not to).

## The coordination contract with `ai-infra-security`

The interface is a joint artefact; the architect and the `ai-infra-security` lead co-own it. The contract commits to the following:

- **A shared incident-id space.** A single incident id — `INC-YYYY-NNNNNNN` shape, minted by whichever side detected first — spans both systems. The SIEM record and the PMS record each carry the id and reference the other. Portfolio-level queries can join across.
- **A payload schema every handoff event uses.** The schema is versioned; every handoff event carries the schema version. The schema names the fields both sides need: the shared id, the affected `ai_system_id`, the detecting-side classification (SOC severity + ATLAS technique on the SOC side; mod-106 category + tier on the governance side), the SLA clock start, the initial payload of evidence, and the referral-side handoff-type flag.
- **A schema-crosswalk between the SOC incident-classification and the governance taxonomy.** SOC severity levels, kill-chain phase (Lockheed Martin Cyber Kill Chain or MITRE ATT&CK phase), and MITRE ATLAS technique where applicable each map to entries in the mod-106 taxonomy. The crosswalk is a table maintained by both co-owners jointly.
- **An SLA for handoff acknowledgement in each direction.** The SOC acknowledges a PMS→SOC handoff within a stated number of business hours; the PMS acknowledges a SOC→PMS handoff within a stated number of business hours. Missed acks fire the interface's own alarm to both co-owners.
- **A versioning discipline.** The interface has a semver-shaped version. Changes are ratified jointly by the head of AI governance and the CISO. Neither side amends unilaterally.

What the interface explicitly does *not* cover, so that both functions can operate their own domain without waiting on the other:

- SOC-internal detection tooling, tuning, and playbook internals.
- SOC-internal analyst tiering, escalation ladders, and rotation schedules.
- The design of specific detection rules — that is the `ai-infra-security` (level 35) role's domain, and mod-102's `AIC-SEC-*` control family binds to it.
- The governance-side classification workflow inside the PMS store — that is chapter 01's shape, and it is the governance function's to run.

## The classification alignment — a worked crosswalk

The interface documents the crosswalk so a single incident carries both classifications and neither side loses information at the handoff boundary. A representative table (with plausible entries; specific ATLAS technique IDs marked for verification):

| SOC classification | MITRE ATLAS technique | Governance taxonomy category (mod-106) |
|---|---|---|
| Sev-2 credential-abuse | <!-- needs-research: verify current ATLAS technique for LLM prompt injection --> | RSK-CAT-unauthorised-tool-call |
| Sev-3 data-exfiltration | <!-- needs-research: verify current ATLAS technique for model output as exfiltration channel --> | RSK-CAT-confidential-info-exfiltration |
| Sev-1 model-integrity-compromise | <!-- needs-research: verify current ATLAS technique for model backdoor / poisoning --> | RSK-CAT-training-data-poisoning-realised |

The crosswalk is not one-to-one in all directions; some SOC severities map to multiple governance categories depending on the affected system's capability tier, and some governance categories are reached through several ATLAS techniques. The crosswalk table names the primary mapping; the classification-workflow inside each system carries the qualification logic for the multi-way cases.

## The four invariants the interface holds

Four invariants are testable, and each has a failure mode named against it.

**Invariant 1 — every AI-specific signal type has a designated home.** The interface's signal-registry names, for every AI-specific signal type the enterprise has decided is in scope (from the initial inventory: jailbreak attempts, prompt-injection detections, data-exfiltration-via-output flags, model-extraction reconnaissance, agentic-tool-misuse patterns, adversarial-example patterns), which side owns the primary handling, which side is the required secondary recipient, and which handoff pattern applies. No signal is orphaned in "someone will figure it out." When a new signal type emerges (a novel attack technique published, a new agentic-tool-misuse category surfaced by red-team), the interface's amendment process assigns it a home before the signal starts arriving.

**Invariant 2 — every red-team finding produces at least one detection candidate + one governance-monitor candidate.** The pattern C handoff carries this: for every productionise-able red-team finding, both a proposed detection signature and a proposed governance monitor are produced, and both have named owners with an SLA on productionisation. A finding may be closed as "not productionise-able" — the decision is documented and reviewed — but it cannot sit unowned. The failure mode this prevents is the red-team-findings-in-Jira pattern.

**Invariant 3 — SOC-detected AI-specific incidents write to the PMS store.** The SOC does not close an AI-nexus incident without a PMS record referenced by the shared incident id. This is enforced at the SIEM-side closure workflow — the closure action is blocked until the PMS acknowledgement is present. Failure mode: SOC closes an incident, no PMS record is ever created, the governance-side register is stale, and the next post-market surveillance report to the regulator (chapter 03's cadence) undercounts the real signal volume.

**Invariant 4 — a shared incident id spans both systems.** The SIEM record and the PMS record reference each other via the shared id. Portfolio-level queries — "how many incidents in the last quarter carried both a Sev-1 SOC classification and a tier-1 governance classification against the same customer-facing system?" — can join across. Failure mode: two independent incident-id spaces produce two independent portfolio views that cannot be reconciled without hand-work; the audit committee (mod-107) receives two versions of the truth.

## The three failure modes to design against

**Failure mode 1 — silent SOC.** Jailbreak attempts and prompt-injection scans accumulate in the SIEM and never reach the PMS store. The SOC handles them on its own runbook; the governance side's residual on "prompt-injection-succeeded" is unchanged in the register even though the threat surface has escalated by an order of magnitude. When the first successful injection lands, the appetite-alarm fires against a residual that has been wrong for months, and the retrospective — done by internal audit under mod-107 — finds the interface missing.

**Failure mode 2 — silent governance.** Output-content flags accumulate in the governance monitor and never reach the SOC. The governance function has good classification records and no attacker attribution. The SOC never hunts for the attacker behind the pattern; the next exfiltration succeeds against the same channel because nobody is looking upstream of the model output.

**Failure mode 3 — red-team-findings-in-Jira.** Findings from a red-team engagement sit in a ticket queue. Some get patched; most get triaged, tagged "productionise as detection," and forgotten. Six months later, the same attack path is exploited in production. The incident-response retrospective finds the finding was known; the audit committee finding is that the enterprise had the evidence and did not act on it. The interface's pattern C handoff, with its named owners and productionisation SLA, is what this failure mode is designed against.

## A YAML schematic of the signal handoff contract

```yaml
interface:
  id: PMS-SOC-INT-v1.4.0
  co_owners: [ senior-ai-governance-architect, ai-infra-security-lead ]
  ratified_by: [ head-of-ai-governance, ciso ]
  shared_id_space:
    format: INC-YYYY-NNNNNNN
    minted_by: whichever_side_detected_first
    joined_ack_sla: 2-business-hours
  handoff_types:
    - id: SOC_TO_PMS
      trigger: SOC detection with ai_system_id nexus
      payload_schema: signal-handoff-schema-v1.4.yaml
      pms_ingest_sla: 15-minutes-automated + 1-business-day-analyst-classification
    - id: PMS_TO_SOC
      trigger: governance monitor with attack-pattern signature OR content-signal exceeding SOC-referral threshold
      payload_schema: signal-handoff-schema-v1.4.yaml
      soc_ingest_sla: 15-minutes-automated + 4-business-hours-analyst-triage
    - id: REDTEAM_TO_DETECTION
      trigger: ai-evaluation-engineer or external-red-team finding classified productionise-able
      payload: finding_ref + reproduction_steps + proposed_detection_signature + proposed_governance_monitor
      productionisation_sla: 15-business-days-decision (either detection built + monitor built, or documented decision not to)
  crosswalk_tables:
    - soc_severity_to_governance_tier
    - atlas_technique_to_mod106_category
    - killchain_phase_to_pms_urgency_class
  versioning: semver; joint-ratify by both co-owners
```

The schematic is the artefact both co-owners refer to when a signal arrives that neither side is quite sure how to handle — the signal-registry, the handoff pattern, the SLA clock, and the crosswalk together give a deterministic answer.

## Cross-references

The interface does not stand alone; it sits inside the wider PMS and assurance architecture the track has been building.

- **Chapter 01** established the PMS store as the single source of truth for AI-nexus operational signal. The SOC→PMS handoff is what keeps security-detected signal from being missing from that store.
- **Chapter 04** designed the Article 73 serious-incident escalation workflow. When a signal handled through this interface crosses the Article 73 severity threshold, the escalation workflow takes over — the interface is the upstream feed into the escalation.
- **Mod-102** built the enterprise AI control library; many of the `AIC-SEC-*` and `AIC-ROB-*` controls target the same techniques the SOC detects. The interface's crosswalk points at control failures as well as risk categories.
- **Mod-106** built the risk taxonomy; the crosswalk to the mod-106 category vocabulary is what makes SOC-detected signal countable against the enterprise appetite.
- **Mod-111** will build the GRC-for-AI platform, which hosts the interface's shared-id registry and the crosswalk tables as first-class objects the audit committee can query.
- **Mod-109** covers the third-party attack-path considerations — a compromise at the frontier-model provider produces signal that arrives through both the SOC (via the provider's disclosure) and the PMS store (via the provider's model-behaviour changes); the interface's handoff patterns extend to third-party origins as well as internal ones.

## Summary

The Security Operations Center is a first-class producer and consumer of AI-nexus signal, not adjacent to the AI-governance function. AI-specific attack patterns — jailbreak attempts, prompt injection, output-channel exfiltration, model-extraction reconnaissance, agentic-tool-misuse, adversarial examples — are simultaneously security signals landing in the SIEM and governance signals that must land in the PMS store; without a designed interface, each side sees half of each incident. The level-50 architect designs the *interface* — schema, routing, escalation contracts, versioning, shared-id space — and co-owns it with the `ai-infra-security` lead (level 35); SOC-side runbook depth, detection engineering, and playbook internals stay with the security function. Reference frames used to speak with the SOC-side are ISO/IEC 27035, NIST SP 800-61 Rev. 2, and MITRE ATLAS. Three handoff patterns — SOC→PMS, PMS→SOC, and red-team→detection — cover every AI-nexus signal the enterprise handles; four invariants (designated home per signal type, red-team findings produce both a detection and a monitor candidate, SOC-detected AI incidents write to PMS, shared incident id) are testable; three failure modes (silent SOC, silent governance, red-team-findings-in-Jira) are common enough that the interface is specifically designed against them. The YAML schematic is the joint artefact both co-owners refer to; the crosswalk tables are what keep the classifications aligned across the two functions.
