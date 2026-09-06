# exercise-04: RBAC Plus SoD Model Design

**Estimated effort:** 3 hours

## Objective

Produce the **RBAC and segregation-of-duties model** for the GRC-for-AI platform in a specified scenario — the artefact the level-50 architect commits to the platform team, hands to the third-line audit function on request, and defends when the ISO/IEC 42001 certification body asks *show me who can do what, on what basis, and how you prove it*. The deliverable set is a machine-readable RBAC model, a pairwise SoD conflict matrix, a competence-profile catalog that discharges the AIMS Clause 7.2 obligation, a break-glass runbook, and an executable enforcement-test plan an internal auditor could run against the deployed platform.

The correctness spine is the chapter-04 invariants and the four failure modes the invariants close against. Every design choice must be pinnable to one of the invariants (as the choice that enforces it) or to one of the failure modes (as the choice that defends against it). The four invariants are: the enterprise IdP is the only identity source; SoD conflicts are blocked at both assignment time and action time; third-line audit is independent from first and second line; break-glass is a designed, logged, auto-expiring first-class event. The four failure modes are: the "analyst can do everything" shortcut; the auditor persona used for day-to-day work; role sprawl; identity mastered inside the platform.

Draft as a senior architect briefing an apprentice who will implement the RBAC in the platform's enforcement layer. The deliverables are contractual artefacts, not essays; the requirements below name what must be present, not how to phrase it. Where the RBAC composes downstream (the audit-facing packaging in chapter 05, the IdP-and-SIEM integration seams in chapter 06, the vendor-evaluation matrix in chapter 07), your artefacts should hold their shape without being rewritten.

## Prerequisites

- Chapter [`04-rbac-and-segregation-of-duties-model.md`](../04-rbac-and-segregation-of-duties-model.md) read once, with the ten personas, the four invariants, and the four failure modes marked.
- Chapter [`03-workflow-layer-intake-through-audit-packaging.md`](../03-workflow-layer-intake-through-audit-packaging.md) skimmed — the workflow transitions are the actions the RBAC capability bundles authorise; know the seams the bundles must cover.
- Chapter [`01-grc-for-ai-reference-architecture-and-enterprise-integration.md`](../01-grc-for-ai-reference-architecture-and-enterprise-integration.md) skimmed — the identity-as-invariant stance the RBAC model preserves.
- The mod-105 chapter on Clause 7.2 competence and the documented-information index — the competence profiles here reference the AIMS documented-information layer by ID, not by prose description.
- The mod-107 three-lines model — the RBAC's line assignment on each persona and the SoD matrix's blocked pairs implement that model. Get the three-lines shape wrong and the RBAC misprices independence.
- Primary references: SR 11-7 Federal Reserve supervisory guidance on model risk management (`<!-- needs-research: canonical federalreserve.gov URL for SR 11-7 -->`) and OCC Bulletin 2011-12 (`<!-- needs-research: canonical occ.treas.gov URL for Bulletin 2011-12 -->`) for the model-validation independence discipline the RBAC generalises to AI beyond the model-risk perimeter; ISO/IEC 42001:2023 Clause 7.2 for the competence obligation the competence-profile catalog discharges. See [`../resources.md`](../resources.md).

## Scenario

You are the level-50 architect at one of the following enterprises. Choose the one whose RBAC + SoD shape you are **least familiar with**; that is where the exercise will teach you most. State your choice at the top of the RBAC model YAML and carry it consistently across all five artefacts.

- **A US regional bank** with an SR 11-7-aligned Model Risk Management function already in place. The MRM head reports to the CRO; MRM validators are already independent of model developers per SR 11-7. Your RBAC must position the MRM function inside the SoD matrix — where does MRM sit relative to the AI Evaluation Engineer persona, and how does the RBAC avoid duplicating or contradicting the existing MRM independence discipline. Scenario-specific persona: **MRM Validator (SR 11-7 scope)**.
- **A global healthcare payer / provider** with a clinical-safety oversight committee that reports to the CMO. Clinical-safety review of AI-enabled clinical-decision-support is a first-line-or-second-line safety function per the mod-107 exercise-01 pattern; the platform RBAC must expose a liaison persona that carries actions across the platform on behalf of the committee without collapsing the committee into the platform. Scenario-specific persona: **Clinical Safety Committee Liaison**.
- **A B2B SaaS platform vendor** shipping GenAI-augmented capabilities into enterprise customers. Customer incidents, evidence requests, and vendor-review exchanges create a customer-facing surface the platform RBAC must model — a seat that speaks to customer risk / procurement / audit teams on behalf of the vendor, with scoped read access across the register and drafting rights on customer-visible communications, but no register-write authority on internal control-effectiveness attestations. Scenario-specific persona: **Customer-Facing Incident Liaison**.

## Deliverables

Author five artefacts in a working directory of your choice.

1. **`rbac-model-v1.0.0.yaml`** — the machine-readable RBAC model. Capability bundles, personas (at least the ten from chapter 04 plus the scenario-specific addition), the seat schema, a reference to the SoD conflict matrix, and the break-glass configuration. Every persona references its competence profile.
2. **`sod-conflict-matrix-v1.0.0.md`** — the full pairwise persona matrix (permitted / permitted-with-warning / blocked), with a rationale per blocked pair. The scenario-specific persona is included in every row.
3. **`competence-profile-catalog.md`** — one competence profile per persona; the catalog is what discharges ISO/IEC 42001 Clause 7.2. Training references, certifications, role-specific qualifications, an evidence-location field pointing at the AIMS documented-information index.
4. **`break-glass-runbook.md`** — eligible personas, the request procedure, auto-approval and auto-expiry rules (measured in hours, not days), mandatory post-event review scheduling, and audit-log flag discipline.
5. **`sod-enforcement-tests.md`** — the executable test plan an internal auditor could run against the deployed platform to verify every invariant and defend against every failure mode.

## Requirements

### `rbac-model-v1.0.0.yaml`

The machine-readable RBAC model. Every block below is required.

- **Version and ratification.** `version:` field with a semantic version; `ratified_by:` naming at least the head of AI governance and the senior AI governance architect.
- **Identity source block.** `identity_source:` naming the enterprise IdP as the only identity master, a `mastered_attributes:` list (subject id, display name, email, employment status, cost centre, IdP group memberships at minimum), and a literal invariant field: `invariant: platform holds no locally-mastered identity attributes`.
- **Capability bundles.** At least the bundles the ten chapter-04 personas depend on (intake-and-triage; monitor-authorship; evaluation-validation; external-attestation-signature; shape-authorship; workflow-authorship; sampling-and-audit-opinion; read-only-sampling; interface-ratification), plus any bundle the scenario-specific persona requires. Each bundle has a stable id, a version suffix, and an enumerated permission list. Bundles are not modified in place — evolving a bundle is a new version.
- **Personas.** At least the ten personas from chapter 04 (AI Governance Analyst L15, AI Risk Engineer L25, AI Evaluation Engineer L35, AI Infra Security Lead L35, Senior AI Governance Architect L50, Head of AI Governance L60, Third-line Internal Audit, External Assurance / Certification-body Auditor, Business System / Model Owner, Regulator-facing Counsel), plus the scenario-specific persona. Every persona carries: `line:` (first / second / third / architect / accountable-executive / external), `bundles:` (versioned references), `competence_profile:` (id into the competence-profile catalog), and `sod_blocks_against:` (persona ids the matrix blocks this persona from being co-held with).
- **Seats.** Seat schema (not a full inventory — the schema and at least one worked example). Every seat carries: `subject_ref:` (into the IdP, not a local id), `persona:`, `scope:` (one of `system-scope` with named systems / `business-unit-scope` with a BU id / `enterprise-scope`), `assigned_by:` (the seat that authorised the assignment), `assigned_at:`, `competence_evidence_ref:` (into the AIMS documented-information index), and `expires_at:` (every seat is time-bounded; no seat is granted "for as long as the person is at the enterprise").
- **SoD conflict matrix reference.** `sod_conflict_matrix_ref:` pointing at `sod-conflict-matrix-v1.0.0.md` by id and version.
- **Break-glass block.** `break_glass:` naming `eligible_personas:` (a small, ratified set), `max_duration:` (in hours), `auto_expiry: enforced`, `post_event_review:` (auto-scheduled within one business day), and `audit_log_flag:` naming `break_glass=true` as a first-class field on every action taken under the elevated seat.

### `sod-conflict-matrix-v1.0.0.md`

A pairwise persona matrix with a row and column for every persona in the RBAC model, including the scenario-specific persona. Cell values are one of `permitted` / `permitted-with-warning` / `blocked`. Every `blocked` cell carries a one-sentence rationale immediately below the table (or in a footnoted rationale block). At minimum, the following pairs must be `blocked` (per chapter 04):

- **AI Risk Engineer × AI Evaluation Engineer** — first-line vs second-line challenge on the same scope collapses the SR 11-7 independence discipline.
- **Senior AI Governance Architect × Business System / Model Owner** — the architect must not attest for scope-bound systems; conflating shape-authorship with case-by-case sign-off collapses the architect's neutrality.
- **Head of AI Governance × Third-line Internal Audit** — executive accountability vs independent audit is the three-lines invariant.
- **Third-line Internal Audit × Any first- or second-line persona** — the audit seat cannot be assigned first- or second-line responsibilities; the row for third-line audit blocks every first- and second-line persona.
- **External Assurance / Certification-body Auditor × Any internal persona** — external independence; the row for the external auditor blocks every internal persona.

The scenario-specific persona appears in every row and every column with an assigned cell value and, where blocked, a rationale.

### `competence-profile-catalog.md`

One competence profile per persona in the RBAC model. Each profile discharges ISO/IEC 42001 Clause 7.2 for the seats that hold this persona. Every profile carries:

- **Training references.** Named course IDs (internal LMS course id or external course id) the seat holder must have completed. "Internal training" without an id is the shape Clause 7.2 audits fail; every training reference must resolve to a record the AIMS documented-information index can produce.
- **Certifications.** Named certifications (name + issuing body + minimum vintage / recertification cycle) where the persona requires them. Third-line audit and external assurance personas will typically carry certifications; the AI Governance Analyst L15 typically will not — state either way explicitly per persona.
- **Role-specific qualifications.** Technical proficiencies (per persona — for example, familiarity with the mod-110 monitor authorship shape for the AI Risk Engineer; familiarity with SR 11-7 model-validation practice for the MRM Validator in the bank scenario), and jurisdictional-familiarity requirements where the persona interacts with regulator submissions.
- **Evidence-location field.** A `evidence_location_ref:` pointing at the AIMS documented-information index entry (mod-105) where the seat's competence-evidence record lives. This is the field the certification body's Clause 7.2 sampling reads from.
- **Refresh cadence.** How often the competence claim is re-verified (annual / on-role-change / on-certification-expiry / on-training-content-change), and the seat that owns the re-verification action.

### `break-glass-runbook.md`

- **Eligible personas.** A small, ratified set — chapter 04's default is the head of AI governance, the senior AI governance architect, and the AI Infra Security Lead. Deviations from the default must be justified.
- **Request procedure.** How a break-glass request is initiated, what it must specify (target scope, action set, requested duration in hours), and what audit event the request itself creates.
- **Auto-approval condition.** A defined trigger — not a human review under pressure — with the trigger's specification (for example, "an incident-severity classification at or above the pre-registered threshold plus a break-glass request from an eligible persona"). The condition is versioned and ratified in advance so that the approval decision is not made at incident time.
- **Auto-expiry.** Measured in hours (chapter 04's illustrative default is four hours). Expiry is enforced by the platform, not by a manual revocation action.
- **Mandatory post-event review.** Scheduled automatically at request time to occur within one business day. The runbook names the seat that convenes the review and the outcome-document schema that gets filed as documented information under the AIMS layer (mod-105).
- **Audit-log flag discipline.** Every action taken under an elevated seat carries `break_glass=true` as a first-class field (not a comment, not a suffix on the action name). The runbook states how the field appears in the action log and how the third-line audit function samples on it.
- **Exception log.** The break-glass exception log is filed as AIMS documented information; the runbook names the index entry.

### `sod-enforcement-tests.md`

The test plan an internal auditor could run in a working session against the deployed platform. Each test carries: **name**, **precondition** (what seats / systems / prior actions must exist), **action** (the specific attempt), **expected outcome** (blocked at the workflow-layer enforcement point, not merely at the UI — a UI-only block is bypassable and does not count as enforcement), and **evidence capture** (which log / event field the auditor screenshots or exports to prove the block occurred).

The plan must include tests for **all four chapter-04 invariants**:

- **Invariant 1 (IdP is only identity source).** IdP off-boarding revokes dependent seats within the SCIM cycle; a platform-local password creation attempt is refused; a platform-local subject-record mint attempt is refused.
- **Invariant 2 (SoD blocked at assignment AND action time).** Assignment-time: attempting to assign a person to two personas the matrix blocks is refused. Action-time: a seat that authored a first-line action against target T cannot then perform the corresponding second-line action against the same target T; the block is per-target, and the test proves both the block-on-same-target and the permit-on-different-target behaviours.
- **Invariant 3 (third-line audit independence).** An audit seat attempting a first- or second-line action is refused; the head-of-AI-governance exception path is invocable but itself an audit event.
- **Invariant 4 (break-glass discipline).** A break-glass authorisation auto-expires at the ratified duration; every action taken under the elevated seat carries `break_glass=true`; the post-event review is auto-scheduled; the exception log is filed as documented information.

The plan must additionally include tests explicitly against **all four chapter-04 failure modes**:

- **Failure (a) analyst-can-do-everything.** A test that attempts to compose a "power-analyst" seat combining intake, risk-scoring, control-effectiveness attestation, and closure — the composition is refused at bundle-assignment time.
- **Failure (b) auditor-doing-day-to-day.** A test that attempts an audit-seat register-draft or exception-approval action — refused at action time with the three-lines rationale surfaced in the refusal event.
- **Failure (c) role sprawl.** A test that attempts to add a new persona without a schema-evolution event (no version bump, no architect ratification) — refused; adding a persona is a ratified change, not a runtime action.
- **Failure (d) identity-in-platform.** A test that attempts to create a platform-local password or a platform-local subject record, or to leave a seat active after IdP off-boarding — all three refused.

**Citations requirement.** Every claim about SR 11-7 (Federal Reserve) and OCC Bulletin 2011-12 that specifies wording, clause language, or timeframes carries a `<!-- needs-research -->` marker if not verified from the primary source. Every claim about ISO/IEC 42001 Clause 7.2 language likewise carries `<!-- needs-research -->` where not verified. Do not invent clause wording.

**Explicit non-scope.** State the following exclusions in the RBAC model YAML (as a `non_scope:` block) and in the enforcement-test plan (as an "out-of-scope for this test plan" note):

- **Enterprise IdP internals.** The enterprise IdP is owned by the IAM team; this exercise specifies what the platform **consumes** from the IdP (mastered attributes, SCIM cycle, group memberships), not the IdP's own configuration, group-provisioning workflows, or SSO integrations.
- **Workflow-engine permission primitives.** The specific permission primitives the workflow engine exposes (guards, transitions, gates) are a chapter-03 exercise deliverable; this exercise references bundles by permission name and does not re-derive the workflow engine's internal permission model.

## Starter guidance

Draft the persona set first. The set is finite by design — the ten chapter-04 personas plus the one scenario-specific persona give you eleven. Adding a twelfth persona to fit an edge case is a schema-evolution event with the same weight as evolving the register schema; if the eleven do not cover the scenario, the fix is almost always a scope-and-bundle composition on an existing persona, not a new persona. The failure-mode-(c) role-sprawl pattern begins the moment a persona is added without ratification.

Author the SoD conflict matrix explicitly. It is not derivable from the persona list alone — two personas may look independent on paper but combine badly on a specific target, and the matrix is where that judgement is committed. Work every pair, even the obviously-permitted ones; a matrix with implicit cells is a matrix that reviewers will assume you skipped. The chapter-04 blocked pairs are the floor, not the ceiling; the scenario-specific persona will add blocked pairs of its own (an MRM Validator is not the same as an AI Evaluation Engineer and the matrix must say so; a Clinical Safety Committee Liaison must not double as a Business System Owner; a Customer-Facing Incident Liaison must not draft internal control-effectiveness attestations).

Assignment-time enforcement is easier to test than action-time enforcement, and action-time is where most failure modes live. A test suite that only exercises assignment-time will pass while the platform lets a risk engineer sign off on the effectiveness of a control substantiating the risk engineer's own monitor. The per-target dimension is the subtlety — the same seat can hold the same persona and perform the second action against a different system where no first-line-to-second-line collapse occurs; your tests must exercise both the block and the permit and prove the per-target logic works.

Competence profiles that reference "internal training" without an evidence-location field are the shape Clause 7.2 audits fail. Every training reference must resolve to a documented-information record the AIMS layer can produce on the certification body's request. Write the profiles as though a Clause 7.2 auditor is about to sample a seat's competence claim and reject it if the evidence trail is not one hop away.

Break-glass without an auto-expiry is not break-glass — it is a persistent super-user role by another name. Every eligible persona in the runbook must have its authorisation auto-expire in hours; the enforcement test that proves expiry ran must be one an auditor can execute in a session, not a promise in prose. Similarly, an audit-log flag that is a comment rather than a first-class field is not sampleable; the platform's action-log schema must carry `break_glass` as an indexed field or the third-line function cannot filter on it.

Do not confuse the platform RBAC with the enterprise IdP's own configuration. The IdP is a source the platform consumes; the platform never masters identity attributes and never mints local credentials. The non-scope block is where you make this explicit; without it, the reviewer cannot tell whether you have merely deferred the IdP question or absorbed it into scope.

For the healthcare scenario, do not fold the clinical-safety oversight committee into the platform RBAC. The committee is a specialised first-line-or-second-line safety body per the mod-107 exercise-01 pattern; the platform exposes a **liaison** persona through which the committee's decisions land in the platform's action log, but the committee's own governance stays outside the RBAC.

## Acceptance criteria

- [ ] Chosen scenario is stated at the top of `rbac-model-v1.0.0.yaml` and every artefact is coherent against it.
- [ ] `rbac-model-v1.0.0.yaml` carries `version:`, `ratified_by:`, `identity_source:` (with mastered-attributes list and the literal invariant), versioned `capability_bundles:`, the ten chapter-04 personas plus the scenario-specific persona (each with `line:`, `bundles:`, `competence_profile:`, `sod_blocks_against:`), a `seats:` schema with a worked example (including `subject_ref:`, `persona:`, `scope:`, `assigned_by:`, `competence_evidence_ref:`, `expires_at:`), a `sod_conflict_matrix_ref:`, and a `break_glass:` block with eligible personas, max duration in hours, auto-expiry, post-event review, and audit-log flag fields.
- [ ] `sod-conflict-matrix-v1.0.0.md` is a full pairwise matrix over every persona (including the scenario-specific persona), with `blocked` / `permitted-with-warning` / `permitted` values in every cell and a rationale for every `blocked` cell. All five required blocked pairs are present.
- [ ] `competence-profile-catalog.md` carries one profile per persona; every profile has training references (with resolvable ids), certifications (name + issuing body + vintage) where applicable, role-specific qualifications, an `evidence_location_ref:` pointing at the AIMS documented-information index, and a refresh cadence.
- [ ] `break-glass-runbook.md` names eligible personas, request procedure, auto-approval condition (a defined trigger), auto-expiry in hours, mandatory post-event review within one business day, audit-log flag `break_glass=true` as a first-class field, and the exception-log AIMS index entry.
- [ ] `sod-enforcement-tests.md` includes a test per chapter-04 invariant (I1 IdP-only identity, I2 assignment-time AND action-time SoD, I3 third-line audit independence, I4 break-glass auto-expiry and flag) and a test per chapter-04 failure mode (a analyst-can-do-everything, b auditor-day-to-day, c role sprawl, d identity-in-platform). Every test names precondition, action, expected outcome at the workflow layer (not UI only), and evidence capture.
- [ ] The scenario-specific persona is justified in the matrix, carries its own competence profile, appears in the seat-schema example if applicable, and is exercised by at least one enforcement test.
- [ ] SR 11-7 (Federal Reserve) and OCC Bulletin 2011-12 are cited for the model-validation independence discipline the RBAC generalises; ISO/IEC 42001:2023 Clause 7.2 is cited for the competence obligation. Every unverified specific — clause wording, URL, timeframe — carries `<!-- needs-research -->`.
- [ ] `non_scope:` block or equivalent excludes enterprise IdP internals (IAM-owned) and workflow-engine permission primitives (chapter-03 exercise).

## Stretch goals

- **Acting-for delegation without SoD collapse.** Extend the RBAC model to support a short-term "acting-for-a-persona" delegation (a seat temporarily discharging another persona's authority during leave or during a specific engagement) without collapsing SoD — the acting seat inherits the same `sod_blocks_against:` set, has its own hours-bounded expiry, and its actions carry an `acting_for:` audit-log field parallel to the `break_glass:` flag. Prove the composition with an added enforcement test.
- **Joiners-movers-leavers walkthrough.** Author a walkthrough that traces a single subject through joining the enterprise (IdP subject-record creation, first seat assignment with competence evidence), a role move (old seat revoked, new persona assigned, SoD re-evaluated, competence gap flagged if the new persona requires certifications the subject does not yet hold), and leaving (IdP off-boarding, dependent seats auto-revoked within the SCIM cycle, historical actions preserved). Prove the IdP-driven seat lifecycle is the single source of truth.
- **External-auditor narrative for SR 11-7 independence.** Produce the audit-body-facing narrative that walks an external auditor through why the platform's action logs are sufficient to prove the SR 11-7 independence claim across a full audit period — which log fields substantiate the "seat that attested to control C did not also perform activity A that C governs" claim; how the SoD matrix version at time-of-action is preserved so the auditor can reconstruct the enforcement decision retroactively; how the competence-evidence reference on the attesting seat is producible on demand.
