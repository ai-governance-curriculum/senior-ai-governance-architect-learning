# The RBAC and segregation-of-duties model — SR 11-7 independence and ISO 42001 competence

## Why this chapter exists

Two shortcuts appear on every GRC-for-AI programme in its first year, and both look reasonable in the meeting that authorises them. The first is the "analyst can do everything" shortcut: intake volume is high, the team is small, and someone senior signs off on giving the AI governance analyst the ability to open a record, populate it, score its risk, attach the evidence, mark the control effective, and close it — because otherwise nothing moves. The second is the "auditor doing day-to-day" shortcut: the third-line internal audit function is short-staffed, an audit seat is quietly let draft register entries or approve exceptions because the audit team happens to know the material best. Both shortcuts collapse the segregation the platform's whole purpose is to enforce.

A third pattern is slower and just as corrosive. Role sprawl: over eighteen months, thirty custom roles accumulate — one for the fraud model owner, one for the retail-agent reviewer, one for a pilot exception, one for the temporary access an outgoing head of AI governance kept — none of them under a shape the architect ratified, and each a permission drift the next external audit will find. Within a year no one can answer "who can approve a tier-3 attestation?" without a bespoke report.

The level-50 architect owns the RBAC model that prevents all three. The chapter names the two external constraints, walks the persona set, distinguishes capability bundles from seats from scopes, specifies the SoD conflict matrix and its enforcement at both assignment time and action time, fixes the identity source and break-glass discipline, and closes with the YAML schematic, invariants, and failure modes. The material assumes chapter 01's stance on enterprise identity as the only source of subject records and chapter 03's stance on workflows as versioned first-class objects — the RBAC model sits on top of both.

## The two constraints the model satisfies

Two external constraints set the frame. Neither is optional.

**SR 11-7 model risk management — validation independent of development.** The US federal-banking supervisory letter on model risk management (SR 11-7, issued jointly by the Federal Reserve and the OCC) frames independent model validation as a first-order control: the function that validates a model must be independent of the function that developed it, and the validation opinion must be defensible to the supervisor as an independent challenge rather than a rubber-stamp. `<!-- needs-research: verify exact SR 11-7 wording on validation independence and the parallel OCC Bulletin 2011-12 wording -->` The RBAC model extends this to AI more broadly than the model-risk framing alone requires: the seat that attests to a control cannot be the seat that performs the controlled activity; the seat that signs an evaluation report cannot be the seat that authored the evaluation. The architect commits the RBAC model to the generalisation enterprise-wide, not just on the model-risk perimeter.

**ISO/IEC 42001 competence, Clause 7.2.** The AIMS standard requires the organisation to determine the competence necessary for persons doing work that affects the AIMS performance, to ensure those persons are competent on the basis of appropriate education, training, or experience, and to retain documented information as evidence. `<!-- needs-research: verify Clause 7.2 wording and the specific documented-information obligation -->` The RBAC model discharges Clause 7.2 by carrying competence as attributes on the seat, not as folklore in a spreadsheet the audit team hunts for at examination time. A seat's competence attributes reference training records, certifications, and role-specific qualifications the AIMS documented-information layer indexes. When the certification body samples a control attestation, the seat that signed it has its competence claim accessible from the same platform.

The two constraints are complementary. SR 11-7 is a *segregation* discipline. Clause 7.2 is a *competence* discipline. The RBAC model satisfies both simultaneously because it carries both on the same object.

The three-lines shape mod-107 established is the frame the model implements: first line owns and runs, second line assures and challenges, third line independently audits both. The RBAC expresses the three lines as first-class attributes on personas and enforces that a seat cannot straddle lines on the same object — no seat approves what it produced, no auditor drafts what it will later sample.

## The persona set

The track commits to the following persona set. Level numbers indicate role seniority and the track's canonical mapping, not the seat's authority in the platform. Authority is defined by the capability bundle attached to the persona.

- **AI Governance Analyst (level 15).** First-line intake. Opens records against the AI inventory; performs initial triage against the mod-106 taxonomy; collects evidence artefacts the mod-108 index expects. Does *not* score residual risk beyond a proposed value, mark a control effective, or sign an attestation.

- **AI Risk Engineer (level 25).** First-line risk analysis. Authors monitors under the mod-110 chapter 02 contract; performs quantitative risk scoring; proposes control designs; runs first-line experiments to test control effectiveness. Does *not* sign off on control effectiveness or close incidents that reach the appetite-alarm threshold.

- **AI Evaluation Engineer (peer, level 35).** Second-line technical challenge. Validates first-line evaluations by re-derivation from raw telemetry in the lake tier (mod-110 chapter 03); challenges the risk engineer's monitor authorship; contributes red-team findings into the mod-110 chapter 05 pattern-C handoff. Second-line by design; cannot hold first-line responsibilities on the same system.

- **AI Infra Security lead (level 35).** SOC-interface co-owner from mod-110 chapter 05. Owns the SIEM side of the AI-signal handoff, detection engineering, and runbook depth against MITRE ATLAS techniques. Ratifies the interface version jointly with the head of AI governance and the CISO. Reads the register for signal correlation; does not amend register entries outside the interface's designed handoffs.

- **Senior AI Governance Architect (level 50).** Authors shapes, contracts, workflows, the RBAC model itself, schema evolutions, and integration seams. Does *not* sign attestations for individual systems — the architect owns the *shape* of the platform, not the case-by-case authority to say a control is effective for a named system.

- **Head of AI Governance (level 60).** The accountable executive. Signs attestations that leave the enterprise perimeter (regulator submissions, certification-body evidence packages, external assurance opinions); ratifies interface changes with the CISO; approves role expansions beyond the default least-privilege posture; is the delegate-of-last-resort on break-glass authorisations.

- **Third-line Internal Audit.** The independent audit seat. Holds sampling authority against every register entry and every seat's action log; can compel documented information under the AIMS layer; produces the audit opinion the board audit committee reads. Cannot be assigned first- or second-line responsibilities — enforcement blocks the combination at assignment and refuses first- or second-line actions from an audit seat.

- **External assurance / certification-body auditor.** Read-only, sampling-scoped, time-bounded. Access provisioned per engagement — a date range, a scope, a set of read permissions. No write permissions; cannot self-extend; access expires automatically on the ratified end date.

- **Business system owner / model owner.** First-line accountability for a specific system. Named on the AI inventory record; holds the workflow-owner position on intake and impact-assessment flows for the owned system; countersigns first-line attestations. Scope-bounded to the specific system(s) the owner is accountable for.

- **Regulator-facing counsel.** The legal seat that drafts filings to regulators, certification bodies, and external counterparties. Reads across the register at engagement scope; drafts external documents referencing register content; holds legal-hold and privilege-management authority for the platform's external correspondence.

The persona set is deliberately finite. Adding an eleventh persona is a schema-evolution event with the same weight as evolving the register schema itself.

## Capability bundles vs seats vs scopes — attribute-driven RBAC

The RBAC model separates four concerns and each is a first-class attribute.

**A capability bundle** is a named set of permissions. `bundle:intake-and-triage` carries the analyst's permissions; `bundle:monitor-authorship` carries the risk engineer's; `bundle:evaluation-validation` carries the evaluation engineer's. Bundles are versioned under the architect's ratification; downstream consumers (workflow gates, action-time enforcement) reference bundles by version. Bundles are not modified in place — a bundle-v3 is a new bundle a persona is re-assigned to. This is the shape that makes RBAC diffable and audit-traceable.

**A persona** is a named combination of bundles expressing one of the roles above. Personas are versioned in the same shape as bundles. A persona also carries competence attributes (the documented Clause 7.2 claim a seat holding this persona must meet) and the SoD conflict edges the model enforces.

**A seat** is a person occupying a persona within a scope. The seat is the object the identity provider (chapter 01) maps to; it holds a persona reference, a scope, an assignment timestamp, an authoriser reference, a competence-evidence reference (the training or certification record the AIMS layer indexes), and an expiry timestamp. Seats are always time-bounded — no seat is granted "for as long as the person is at the enterprise".

**A scope** is the boundary within which a seat's permissions apply. Three kinds:
- *System-scope* — permissions apply only within named AI inventory entries. The retail-agent model owner holds a system-scoped seat covering the retail-agent system(s) and nothing else.
- *Business-unit-scope* — permissions apply across a BU's inventory. The head of AI governance for retail-banking holds a BU-scoped seat covering the division's systems.
- *Enterprise-scope* — permissions apply enterprise-wide. The architect, the head of AI governance, third-line audit, and regulator-facing counsel typically hold enterprise-scoped seats; system and BU owners do not.

Attribute-driven RBAC in this shape means the platform never asks "does user U have permission P?" as a flat question. It asks: "does user U hold a seat S; is S in scope for target T; does S's persona reference a bundle carrying permission P; and does the action pass the SoD conflict check against S's other seats and the target's prior actions?" The composed check is what the enforcement layer runs.

## SoD conflict matrix — assignment-time and action-time enforcement

Segregation of duties is enforced at two moments: when a seat is *assigned* to a person who already holds other seats, and when a seat *acts* against a target the seat's other seats have prior authorship on. Both enforcements draw from the same conflict matrix.

The matrix names, for every pair of personas, whether the pairing is permitted, permitted-with-warning, or blocked. Representative entries (the full matrix is a first-class artefact under the architect's ratification):

| Persona A | Persona B | Same-person combination |
|---|---|---|
| AI Governance Analyst | AI Risk Engineer | permitted (both first-line) |
| AI Risk Engineer | AI Evaluation Engineer | blocked (first-line vs second-line challenge on same scope) |
| Senior AI Governance Architect | Business system owner | blocked (architect must not attest for scope-bound systems) |
| Head of AI Governance | Third-line Internal Audit | blocked (executive accountability vs independent audit) |
| Third-line Internal Audit | Any first- or second-line persona | blocked (three-lines invariant) |
| External assurance auditor | Any internal persona | blocked (external independence) |
| Regulator-facing counsel | AI Risk Engineer | permitted-with-warning (documented rationale required) |

*Assignment-time enforcement* runs the matrix check when a seat is proposed. A proposal that combines blocked personas is refused; forcing the exception requires the head of AI governance's explicit sign-off, which is itself an audit event and recorded against the persona's exceptions log.

*Action-time enforcement* runs the matrix check when a seat attempts to act. A risk engineer who authored monitor M against system S cannot then sign off on the effectiveness of the control that M substantiates — the action is refused with the conflicting action's reference. An analyst who opened intake record R cannot then close R at the appetite-alarm threshold. Enforcement is per-target: the same seat can perform both actions against different systems where no first-line-to-second-line collapse occurs on the same object.

Action-time enforcement closes the loophole assignment-time alone leaves. The SR 11-7 analogue applies to actions on the same object, not to permanent role prohibition across the enterprise. The matrix blocks simultaneous overlapping-scope combinations; action-time additionally blocks a seat from signing off on its own prior first-line action even where seats are held sequentially.

## Identity + break-glass discipline

Two disciplines close the model.

**Identity is mastered by the enterprise identity provider, not by the platform.** Chapter 01 fixed this as an invariant. The platform holds seats; seats reference subject records; subject records live in the enterprise IdP (Okta, Entra ID, Ping, or the enterprise's chosen provider). The platform never mints a local password, never allows local-only user creation, and never permits identity-attribute mastering (name, email, employment status, cost centre) inside itself. When a person leaves the enterprise, the IdP's off-boarding cycle terminates the subject record; dependent seats are automatically revoked; the audit trail preserves historical actions but the seat cannot act again. The failure mode this closes is dual-identity drift — a platform-local identity remaining active after enterprise off-boarding, or a platform-local role diverging from IdP group membership.

**Break-glass provisioning is designed, not omitted.** Every enterprise system encounters cases where the ordinary authorisation path is unavailable — the head of AI governance is unreachable in an incident window, a critical action must be taken before a review cycle can convene, incident-response requires cross-scope authority no single seat holds. The architect designs the break-glass path explicitly: a named set of seats can request break-glass authority; the request creates an auto-approved elevated seat with a defined scope and a hard expiry (measured in hours, not days); every action carries the break-glass flag through the audit log; a post-event review is auto-scheduled and its outcome is filed as documented information under the AIMS layer.

The failure a designed break-glass discipline prevents is the *undesigned* alternative — an admin backdoor, a shared credential someone senior "just uses", a persistent super-user role inherited from platform stand-up. Every undesigned alternative collapses the audit trail the moment it is used. A designed break-glass event is a first-class audit event and a first-class documented-information record; an undesigned one is invisible and produces a gap the next external audit finds.

## YAML-shaped RBAC schematic

The schematic below is illustrative of the shape. Enterprises adapt field names and enumerations; the shape is what generalises.

```yaml
rbac_model:
  version: RBAC-v3.2.0
  ratified_by: [ head-of-ai-governance, senior-ai-governance-architect ]
  identity_source:
    provider: enterprise-idp
    mastered_attributes: [ subject_id, display_name, email, employment_status, cost_centre, idp_group_memberships ]
    invariant: platform holds no locally-mastered identity attributes
  capability_bundles:
    - id: bundle:intake-and-triage-v2
      permissions:
        - workflow.intake.open
        - workflow.intake.populate
        - workflow.intake.attach_evidence
        - taxonomy.propose_classification
        - register.read
    - id: bundle:monitor-authorship-v4
      permissions:
        - monitor.author
        - monitor.deploy
        - risk.score.propose
        - register.write.residual.propose
    - id: bundle:evaluation-validation-v2
      permissions:
        - evaluation.rederive
        - evaluation.challenge
        - control.attest_effectiveness
        - register.write.residual.confirm
    - id: bundle:external-attestation-signature-v1
      permissions:
        - attestation.sign_external
        - regulator_submission.author
        - certification_evidence.package
  personas:
    - id: persona:ai-governance-analyst-v2
      line: first
      bundles: [ bundle:intake-and-triage-v2 ]
      competence_profile: comp-profile:analyst-v1
      sod_blocks_against: []
    - id: persona:ai-risk-engineer-v3
      line: first
      bundles: [ bundle:monitor-authorship-v4 ]
      competence_profile: comp-profile:risk-engineer-v2
      sod_blocks_against: [ persona:ai-evaluation-engineer-v2 ]
    - id: persona:ai-evaluation-engineer-v2
      line: second
      bundles: [ bundle:evaluation-validation-v2 ]
      competence_profile: comp-profile:evaluation-engineer-v1
      sod_blocks_against: [ persona:ai-risk-engineer-v3 ]
    - id: persona:senior-ai-governance-architect-v2
      line: architect
      bundles: [ bundle:shape-authorship-v1, bundle:workflow-authorship-v2 ]
      sod_blocks_against: [ persona:business-system-owner-v1, persona:head-of-ai-governance-v1 ]
    - id: persona:head-of-ai-governance-v1
      line: accountable-executive
      bundles: [ bundle:external-attestation-signature-v1, bundle:interface-ratification-v1 ]
      sod_blocks_against: [ persona:third-line-audit-v1, persona:senior-ai-governance-architect-v2 ]
    - id: persona:third-line-audit-v1
      line: third
      bundles: [ bundle:sampling-and-audit-opinion-v1 ]
      sod_blocks_against: [ any first-line or second-line persona ]
    - id: persona:external-assurance-auditor-v1
      line: external
      bundles: [ bundle:read-only-sampling-v1 ]
      time_bounded: true
      scope_bounded: per-engagement
      sod_blocks_against: [ any internal persona ]
  seats:
    example:
      seat_id: SEAT-2027-04-11-U-4438
      subject_ref: idp:sub:8f2b...
      persona: persona:ai-risk-engineer-v3
      scope:
        kind: system-scope
        systems: [ SYS-2027-0042, SYS-2027-0053 ]
      assigned_by: SEAT-2027-01-01-U-0001   # head of AI governance
      assigned_at: 2027-04-11T09:00:00Z
      competence_evidence_ref: DI-COMP-2027-U-4438-v1
      expires_at: 2028-04-11T09:00:00Z
  sod_conflict_matrix_ref: sod-matrix-v3.2.0
  break_glass:
    eligible_personas: [ persona:head-of-ai-governance-v1, persona:senior-ai-governance-architect-v2, persona:ai-infra-security-lead-v1 ]
    max_duration: 4-hours
    auto_expiry: enforced
    post_event_review: auto-scheduled-within-1-business-day
    audit_log_flag: break_glass = true (first-class field)
```

The schematic carries the invariants. `identity_source` fixes the IdP as the only identity master. `capability_bundles` are versioned. `personas` name their line assignment and their SoD blocks. `seats` are time-bounded, scope-bounded, and carry the competence-evidence reference the Clause 7.2 obligation depends on. `sod_conflict_matrix_ref` is a first-class artefact, versioned alongside the RBAC model. `break_glass` is designed with expiry, review, and audit-flagging.

## Invariants + failure modes

Four invariants the RBAC model holds:

**Invariant 1 — the enterprise IdP is the only identity source.** No platform-local passwords, subject records, or identity-attribute mastering. IdP off-boarding revokes dependent seats automatically. Preserves the chapter 01 invariant.

**Invariant 2 — SoD conflicts are blocked at both assignment time and action time.** Assignment-time blocks conflicting persona combinations; action-time blocks a seat from collapsing a first-line-to-second-line boundary on the same object, even where seats are held sequentially. Both draw from the same versioned SoD conflict matrix.

**Invariant 3 — third-line audit is independent from first and second line.** The audit persona cannot be assigned first- or second-line responsibilities; the exception path (with head-of-AI-governance sign-off) is available only for documented, time-bounded rationale that itself becomes an audit event. Preserves the three-lines shape mod-107 established.

**Invariant 4 — break-glass is a designed, logged, auto-expiring first-class event.** Every break-glass authorisation is time-bounded, flagged in the audit log with a dedicated field, and followed by a scheduled post-event review whose outcome is filed as documented information.

Four failure modes the invariants close against:

**Failure mode (a) — the "analyst can do everything" shortcut.** The RBAC is flattened so a single persona holds intake, risk-scoring, control-effectiveness attestation, and closure. SR 11-7 independence is nonexistent; the register carries entries whose whole lifecycle was performed by one seat with no second-line touch. Appears when the architect concedes to intake-throughput pressure and the platform team adds a "power-analyst" bundle no one revisits. Invariant 2 is what this fails.

**Failure mode (b) — auditor role used for day-to-day work.** The audit seat drafts register entries, approves exceptions, or scores risks because the audit team knows the material best and the second line is short-staffed. Third-line independence is defeated; the next external audit cannot rely on internal-audit's opinion because internal-audit was doing first- or second-line work. Invariant 3 fails.

**Failure mode (c) — role sprawl.** Thirty custom roles accumulate over eighteen months, each ratified by whoever needed it, none expressible in the invariant frame. Within a year the model is undocumented, permissions drift silently, and no one can answer basic questions about who can approve what. Prevented by treating persona addition as a schema-evolution event.

**Failure mode (d) — identity mastered inside the platform.** A local password store accumulates; a "service account" outlives its purpose; a departed employee's platform-local identity remains active after IdP off-boarding. Invariant 1 fails. Often invisible until a stale seat is used, at which point the audit trail cannot reconstruct the authorisation chain because the local identity has no IdP counterpart.

## Summary

The RBAC and segregation-of-duties model discharges two external constraints simultaneously: SR 11-7's validation-independent-of-development discipline, generalised to "the seat that attests to a control cannot be the seat that performs the controlled activity", and ISO/IEC 42001 Clause 7.2's competence requirement, carried as first-class attributes on seats rather than folklore in an offline spreadsheet. The three-lines shape mod-107 established is the frame the model implements; the ten-persona set expresses the lines and the specific roles the architect interacts with. Capability bundles, personas, seats, and scopes are separated concerns and each is versioned; assignment-time enforcement blocks conflicting persona combinations, action-time enforcement blocks a seat from collapsing a first-line-to-second-line boundary on the same object even where seats are held sequentially. The enterprise IdP is the only identity source. Break-glass provisioning is designed, time-bounded, auto-expiring, and logged as a first-class audit event with a scheduled post-event review. The YAML schematic ties bundles, personas, seats, the SoD conflict matrix, and the break-glass configuration into one artefact the architect ratifies and the audit function reads. Four invariants (single identity source, SoD blocked at both assignment and action, third-line independence, break-glass as designed first-class event) are testable; four failure modes (analyst-can-do-everything, auditor-doing-day-to-day, role sprawl, identity-in-platform) are common enough that the model is specifically designed against them. Chapter 05 walks the audit-facing packaging the RBAC's action logs and competence attributes feed; chapter 06 walks the integration seams that carry the IdP handshake and the SIEM handoff into the RBAC's enforcement layer; chapter 07 walks the vendor-evaluation matrix against which the enterprise's chosen platform's RBAC capability is scored.
