# The exception / waiver workflow

## Why this chapter exists

Waivers are inevitable. A policy library that never issues one is either brand-new, unenforced, or written so loosely that nobody needs relief from it. Every mature control regime — SOX, PCI DSS, ISO/IEC 27001, SR 11-7, FedRAMP — has a formal exception mechanism because the alternative is worse: teams route around the policy silently, and the architect discovers non-compliance at audit rather than at request.

The failure mode splits two ways. **Refuse waivers on principle** and product teams learn that the honest path is closed; they either miss delivery dates (and blame governance) or find a workaround that never touches the register (and the risk becomes invisible). **Approve waivers casually** and the policy quietly becomes advisory; every subsequent request references the last easy grant as precedent, and within two cycles the exception surface is larger than the compliant surface.

The architect's job is to design the workflow that keeps waivers **rare, time-bounded, and visible**. Rare because every waiver represents an outcome the enterprise agreed to and is now temporarily conceding. Time-bounded because a waiver without an expiry is a policy amendment nobody voted on. Visible because the risk-register (previewed here, owned in mod-106) is where the enterprise sees the aggregate posture of every open concession.

This chapter sits inside mod-103. `01-...md` framed the policy taxonomy — policy, standard, control, procedure. `02-...md` framed the verb hierarchy — must / should / may. `03-...md` framed policy-as-code. This chapter defines the escape valve: how a system deviates from a *must* without breaking the taxonomy. `05-...md` and `06-...md` cover the change-management and publication workflow the waiver register lives alongside.

Two adjacent chapters are load-bearing to read first. Mod-102 chapter 05 (`05-control-inheritance-and-compensating-controls.md`) defines the compensating-control shape a waiver almost always routes to. Mod-106 defines the risk-appetite bands and the risk register the waiver hooks into.

## The seven fields every waiver record carries

A waiver is a machine-readable record, not a memo. It ships with seven fields; a waiver missing any of them is a schema error, and the tooling (below) refuses to open it.

```yaml
waiver:
  waiver_id: WVR-2026-0184
  scope:
    policy_ref: POL-AI-OUT-04 §3.2      # specific clause
    standard_ref: STD-AI-OUT-04.1       # specific standard
    control_refs: [AIC-OUT-031]         # specific control(s)
    systems:
      - system_id: SYS-CX-CHAT-07
        version: 3.4.x
    business_units: [retail-cx]
  justification: >
    The output-classification confidence-threshold logging required
    by STD-AI-OUT-04.1 §2 depends on a vendor-SDK telemetry hook
    (VendorX InferenceSDK ≥ 4.2). VendorX has committed to ship 4.2
    in their Q4 release; the current pinned version is 4.1.3 and
    upgrade is blocked by an unrelated breaking change in their
    tokenizer API.
  compensating_controls:
    - control_ref: AIC-OUT-031-COMP-A
      description: >
        Weekly manual sampling of 200 output classifications by the
        ai-governance-analyst, cross-checked against the standard's
        threshold, with results logged in the evidence store.
      owner_role: ai-governance-analyst
      cadence: weekly
      evidence_artifact: manual-sampling-report.md
  residual_risk:
    description: >
      Reduced coverage vs. continuous automated logging: manual
      sampling covers ~1.4% of weekly volume vs. 100% under the
      standard. Detection latency for a threshold-drift event
      increases from <1h (automated) to up to 7 days (sampling).
    risk_register_id: RR-2026-CX-0044
    accepting_authority: head-of-retail-cx-risk
  expiry: 2026-12-04                    # ISO date; 120 days
  approvals:
    - role: line-manager-cx-chat
      name_at_time: <recorded-at-approval>
      date: 2026-08-06
    - role: ai-governance-analyst
      name_at_time: <recorded-at-approval>
      date: 2026-08-06
    - role: senior-ai-governance-architect
      name_at_time: <recorded-at-approval>
      date: 2026-08-07
```

Each field carries load. Take them one at a time.

**Field 1 — `waiver_id`.** Stable, monotonic, never reissued. Convention: `WVR-<year>-<sequence>`. When a waiver is renewed, the renewal gets a *new* ID with a `supersedes` back-pointer; the old ID is not reused, edited, or extended in place. Renewal-as-edit is the audit trail's most common failure mode — you lose the history of how many times the same underlying deviation has been granted.

**Field 2 — `scope`.** A waiver is never global. It names the specific policy clause, standard, or control (from the taxonomy in `01-...md`) *and* the specific system, product, or version the waiver applies to. A waiver written against "the output-classification policy" full stop is a policy amendment; a waiver written against `POL-AI-OUT-04 §3.2` for `SYS-CX-CHAT-07 v3.4.x` is legitimate. The tooling requires both axes to resolve — a policy clause that does not exist, or a system ID that is not in the AI inventory (mod-105), fails validation. Waivers that apply to "a business unit" or "a product line" are not waivers; they are exceptions (see the three-way distinction below).

**Field 3 — `justification`.** Concrete, verifiable, external where possible. The rule the architect enforces: *"inconvenient" is not a justification; "vendor cannot deliver X by Y" is*. Compare:

- **Bad**: "The team does not currently have bandwidth to implement the standard."
- **Bad**: "The control is not applicable to our use case." (If true, it is an applicability question, not a waiver.)
- **Good**: "Vendor SDK ≥ 4.2 required for telemetry hook; current pinned 4.1.3; vendor Q4 roadmap commits 4.2; upgrade blocked by unrelated tokenizer breaking change."

Good justifications name a *constraint* (technical, contractual, temporal) and a *resolution path*. If there is no resolution path, the waiver will not close — which means the honest artefact is a policy amendment, not a waiver.

**Field 4 — `compensating_controls`.** One or more entries referencing the compensating-control shape from mod-102 chapter 05 (`05-control-inheritance-and-compensating-controls.md`). This is the load-bearing field. A waiver with no compensating control is permissible *only* if the residual risk sits within the enterprise's declared risk appetite (mod-106) — and even then the architect's default posture is to require one. The compensating control is what distinguishes "we are temporarily excused" from "we are pretending the risk does not exist."

The compensating control's mechanism does not have to be equivalent to the original (if it were, this would be a compensating control per mod-102, not a waiver); it has to *materially reduce* the residual risk relative to doing nothing. Manual weekly sampling instead of continuous automated logging is not equivalent — but it is materially better than nothing, and the equivalence gap is exactly what `residual_risk` names.

**Field 5 — `residual_risk`.** Every waiver carries a residual-risk statement with three sub-fields: the *description* of the gap, a *risk-register entry ID* (mod-106), and a *named accepting authority*. The accepting authority must have the authority to accept the risk band the gap sits in — the approval matrix below governs that mapping. "Accepted by the team" is not an accepting authority; "accepted by the head of retail-cx risk" is.

**Field 6 — `expiry`.** An ISO date, capped by the risk-band maximum lifetime (below). Waivers with no expiry, or with an expiry beyond the cap for their band, are a finding — the tooling rejects them at open time. The expiry is the mechanism that forces the review: when the date arrives, the waiver either lapses, closes (because the underlying gap is resolved), renews (with a new ID and a fresh approval round), or is promoted to a permanent exception via policy amendment.

**Field 7 — `approvals`.** The chain of approvers who signed, with the role at time of signing, the name recorded at approval, and the date. The chain is set by the approval matrix; the tooling refuses to activate a waiver until the required signatures for its risk band are present. Approvers cannot delegate silently — a delegated signature must itself be recorded as a delegation, with the delegating authority named.

## Approval matrix — who approves what, by risk band

Suggested defaults; every enterprise tunes these against its own risk-appetite framework (mod-106) and its regulatory posture. Name these as the starting shape, not universal law.

| Risk band / duration | Approvers required | Notification |
|---|---|---|
| Low-risk / short-life (< 30 days) | line manager + `ai-governance-analyst` | ai-governance-architect (informed) |
| Moderate | `senior-ai-governance-architect` (level 50) + head-of-AI-governance (level 60) | risk-committee (informed at next cycle) |
| High / critical | head-of-AI-governance + CRO or equivalent | board risk / audit committee (notified within the cycle) |
| Cross-jurisdictional or regulator-facing | legal + head-of-AI-governance (+ CRO if high/critical) | forward-reference mod-104 for the jurisdictional overlay |

Three properties of the matrix matter.

**Approvers scale with risk band, not with visibility.** A high-risk waiver requested by a small team gets the same approval chain as a high-risk waiver requested by a flagship product. The matrix is set by the *risk band of the residual risk*, not by the political weight of the requesting business.

**No self-approval.** The requesting team cannot supply any of the required approvers. The line manager referenced in the low-risk row is the line manager of the requesting team; the ai-governance-analyst is the analyst assigned to the business unit but not embedded in the product squad. The tooling encodes this — an approver whose role resolves inside the requesting scope fails validation.

**Cross-jurisdictional means legal.** Any waiver whose scope touches more than one jurisdiction routes to legal in addition to the risk-band chain. This is where mod-104 (multi-jurisdiction reconciliation) meets the waiver workflow: a waiver against a control that satisfies both the EU AI Act and a US-state obligation cannot be approved by an internal chain alone.

## Maximum-lifetime cap by risk band

The expiry field is bounded, per band, by a hard ceiling. Suggested defaults:

| Risk band | Max initial term | Max renewals before amendment |
|---|---|---|
| Low | 90 days | 2 |
| Moderate | 180 days | 2 |
| High / critical | 365 days | 1 |

The renewal cap is the architect's discipline lever. A waiver renewed twice is a signal — the underlying constraint is not temporary; the honest artefact is one of two things: a **policy amendment** (the policy as written is wrong and needs updating; mod-103 `05-...md` covers the change-management path) or a **permanent exception** (the policy is right but this class of systems is legitimately outside its applicability; see the three-way distinction below). Renewing indefinitely is neither, and it is the mechanism through which "waiver" quietly becomes "policy variant with no vote."

The cap is enforceable: the tooling refuses to open a waiver whose expiry exceeds the band's cap, and refuses to open renewal N+1 for any waiver that has hit the renewal cap. The renewal count is on the waiver record; it survives the `supersedes` chain.

## Routing to compensating controls

Every waiver is presumed to require a compensating control. The presumption is defeasible only when the residual risk, without any compensating mechanism, sits within the enterprise's declared risk appetite for the system's tier and use case (mod-106). Even then, the architect's default posture is to require one — the risk-appetite check is not a licence to skip the design, it is a floor on when omission is permissible.

The compensating control referenced in the waiver record follows the shape from mod-102 chapter 05 (`05-control-inheritance-and-compensating-controls.md`): justification, alternate mechanism, equivalence argument (or, for a waiver, a *materiality* argument — how the compensating mechanism materially reduces the residual risk), residual risk, accepted-by, expiry. The waiver's compensating control inherits the waiver's expiry as its upper bound; it can be shorter (e.g., a compensating control tied to a manual process that only runs for the first 30 days) but not longer.

Two mechanical rules:

- **The compensating control must be assigned to an owner role, not the requesting team.** The `ai-governance-analyst` running weekly sampling is a different role from the product squad requesting the waiver. Assigning the compensating work to the requesting team is a conflict — the team most incentivised for the waiver to succeed cannot be the team producing the assurance that it is working.
- **The compensating control has its own evidence contract.** The evidence-contract shape from mod-102 chapter 01 applies. If manual weekly sampling produces a `manual-sampling-report.md`, that artefact is captured, retained, and reviewed on the same cadence as any other control evidence. A compensating control with no evidence contract is decorative.

## Risk-register integration

Every open waiver has a corresponding risk-register entry. The register (previewed here, owned in mod-106) is the enterprise's single view of open risk; the waiver register and the risk register are joined by the `residual_risk.risk_register_id` field on the waiver record.

Three integration rules:

- **Opening a waiver opens (or updates) a risk-register entry.** The residual-risk description on the waiver is transcribed to the register with the waiver's ID as a back-reference. If a matching risk-register entry already exists (because the same gap was previously registered on a different waiver, or because the risk was known and untreated), the waiver's entry links to it rather than duplicating.
- **Closing the waiver closes the register entry** — provided no other waiver, and no un-remediated finding, references the same underlying risk. If the underlying risk persists (because other systems still deviate against the same standard), the register entry stays open with the waiver's back-reference removed.
- **Expiry without renewal or promotion to permanent exception creates a "waiver overrun" finding.** This is the enforcement teeth. A lapsed waiver is not a soft state — it is a finding on the risk register with an SLA for remediation, and it appears in the mod-112 executive reporting alongside other findings. The tooling generates the finding automatically at expiry+0 if no renewal, closure, or promotion event has been recorded.

## Waiver lifecycle

Prose diagram:

```
   ┌──────────┐
   │ requested│  <-- team submits with draft fields
   └────┬─────┘
        │  scope + justification + draft compensating control
        v
   ┌──────────┐
   │  triaged │  <-- ai-governance-analyst validates fields,
   └────┬─────┘      resolves scope, sets risk band
        │
        v
   ┌────────────────────────┐
   │ compensating-control   │  <-- architect (or delegate) reviews
   │      assigned          │      mechanism + materiality argument
   └────┬───────────────────┘
        │
        v
   ┌──────────┐          ┌──────────┐
   │ approved │  or ---> │ rejected │  (route to remediation plan
   └────┬─────┘          └──────────┘   or policy-amendment intake)
        │
        v
   ┌──────────┐
   │  active  │  <-- risk-register entry opened,
   └────┬─────┘      compensating-control evidence begins
        │
        │ at expiry-30d (or interim trigger)
        v
   ┌──────────────┐
   │ under review │  <-- owner + analyst assess:
   └────┬─────────┘      close, renew, or promote
        │
   ┌────┼─────────────────┐
   v    v                 v
 closed renewed        lapsed
        │  (new WVR-ID     │
        │   supersedes)    │
        v                  v
     active            finding on
                       risk register
                       ("waiver overrun")
```

The `under review` state is the discipline point. A waiver that reaches expiry without an active review record is an operational failure — the tooling raises the pre-expiry review 30 days out (configurable per band) and escalates if the review has not been recorded by expiry-7.

## The three-way distinction — waiver, exception, compensating control

The words are used interchangeably in the wild. The architect must not. Three separate mechanisms handle three separate situations, and confusing them produces artefacts nobody trusts.

Take one underlying policy: **`POL-AI-OUT-04 — customer-facing generative outputs must be logged with confidence-threshold telemetry sufficient to detect classifier drift within one hour.`**

**Waiver.** `SYS-CX-CHAT-07 v3.4.x` cannot meet the one-hour-detection requirement because the vendor SDK does not yet expose the required telemetry. The policy still applies to `SYS-CX-CHAT-07`; the team is temporarily excused, with a manual-sampling compensating control (~1.4% coverage, up-to-7-day detection latency), for 120 days, pending vendor SDK 4.2. The waiver record is the artefact just walked through. When SDK 4.2 lands, the waiver closes and the policy is once again fully satisfied for this system.

**Exception.** `SYS-INT-CODE-ASSIST-02` is an internal-facing developer coding assistant that produces code suggestions consumed only by engineers who review each suggestion before commit. The policy's outcome — protecting *customer-facing* generative outputs from classifier drift — does not apply to this class of system at all; the applicability filter in the underlying control (`AIC-OUT-031`) is written to exclude `use_case: internal-developer-tooling`. This is encoded once, in the applicability filter, at policy time. It is *not* a waiver: no team requests it, no expiry attaches to it, no compensating control substitutes for it. The exception is expressed as an applicability predicate (mod-102 chapter 01 covers the shape). A team asking for a "waiver because the policy does not apply" is asking for the wrong artefact — the correct artefact is an applicability review that either confirms the exception or amends the filter.

**Compensating control.** `SYS-CX-VOICE-11` is a voice interface whose transport layer cannot carry the standard's confidence-threshold JSON structure but *can* carry an equivalent binary telemetry frame that a converter re-hydrates into the standard schema before it hits the observability stack. The outcome — one-hour detection of classifier drift — is achieved in full; the mechanism differs. This is a compensating control per mod-102 chapter 05, not a waiver. There is no residual risk to accept (the equivalence argument holds) and no expiry beyond the review cadence any compensating control carries. A team routing this through the waiver workflow is over-escalating and burning approval capacity that should be reserved for real deviations.

The one-line test:

- **Outcome not achieved, temporarily** → **waiver**.
- **Outcome does not apply to this class of system** → **exception** (an applicability property, not a workflow).
- **Outcome achieved through a different mechanism** → **compensating control**.

## Tooling shape

The architect designs the fields, the workflow, and the enforcement rules; the platform team (or `ai-risk-engineer` at level 25, working with a platform group) builds the tooling. The downstream home is the GRC-for-AI platform in mod-111 — waivers and their risk-register entries are first-class objects there, alongside controls, evidence, and findings.

Architect's requirements for the register, independent of vendor:

- **Machine-readable.** The record is structured (YAML or JSON in the examples above; the wire format matches the OSCAL-adjacent shapes mod-102 chapter 04 laid out). Free-text-only waiver systems fail every downstream query — you cannot ask "how many high-band waivers expire in the next 30 days" of a Word document.
- **Versioned.** Waiver records are immutable once approved; renewals produce new records with `supersedes` back-references. Edits to an active waiver require a re-approval event that is itself recorded.
- **Queryable along five axes.** By scope (which policy / standard / control), by system, by risk band, by expiry window, by approving authority. These five queries drive the operational reviews: the pre-expiry sweep, the policy-review sweep (how many waivers against this clause — is it a policy-amendment signal?), the accepting-authority sweep (who is accepting how much residual risk?), the system-portfolio sweep (which systems are heaviest in waivers?), and the risk-band sweep.
- **Joined to the AI inventory (mod-105) and the risk register (mod-106).** Scope IDs resolve to inventory entries; `residual_risk.risk_register_id` resolves to a live register entry. Broken joins are validation errors, not warnings.
- **Integrated with policy-as-code (`03-...md`).** When a waiver is active for a control that policy-as-code enforces at CI/CD, the automated check consults the waiver register and either annotates the failure ("gated by WVR-2026-0184, expires 2026-12-04") or gates on it explicitly. Waivers that policy-as-code does not know about are ignored by policy-as-code, and teams learn to route around both mechanisms.

## Worked example — Northbrook-style scenario

A retail-banking business unit ("Northbrook Retail") ships a customer-facing chat assistant, `SYS-CX-CHAT-07`, on a vendor stack. The enterprise standard `STD-AI-OUT-04.1` — anchored in policy `POL-AI-OUT-04` — requires that every output classification carry a confidence-threshold telemetry event, logged in the enterprise observability stack, sufficient to detect classifier drift within one hour.

`SYS-CX-CHAT-07` cannot meet the standard on its current vendor-SDK version (4.1.3). VendorX's SDK 4.2, in their Q4 roadmap, adds the required telemetry hook; the upgrade path is blocked by an unrelated tokenizer API change VendorX has committed to smooth in the same release.

The waiver:

- **`waiver_id`**: `WVR-2026-0184`.
- **`scope`**: policy `POL-AI-OUT-04 §3.2`, standard `STD-AI-OUT-04.1`, control `AIC-OUT-031`; system `SYS-CX-CHAT-07 v3.4.x`; business unit `retail-cx`.
- **`justification`**: Vendor SDK ≥ 4.2 required for the telemetry hook. Current pinned 4.1.3; vendor Q4 roadmap commits 4.2; upgrade blocked by an unrelated tokenizer API change VendorX is smoothing in the same release. Not a design deficiency; a temporal vendor constraint.
- **`compensating_controls`**: `AIC-OUT-031-COMP-A` — weekly manual sampling of 200 output classifications by the `ai-governance-analyst`, cross-checked against the standard's threshold, results logged to `manual-sampling-report.md`. Owner role: `ai-governance-analyst`. Cadence: weekly. Materiality argument: reduces detection-latency exposure from unbounded (no sampling) to a bounded weekly review; catches sustained drift; will not catch short-lived transient drift.
- **`residual_risk`**: sampling covers ~1.4% of weekly volume vs. 100% under the standard; detection latency for a threshold-drift event increases from <1h (automated, per standard) to up to 7 days (weekly sampling). Risk-register entry `RR-2026-CX-0044`. Accepting authority: head-of-retail-cx-risk.
- **`expiry`**: 2026-12-04 (120 days from open; within the moderate-band 180-day cap; VendorX Q4 roadmap sits at week-of 2026-10-15, giving ~7 weeks post-vendor-release for the team to upgrade, validate, and close).
- **`approvals`**: line-manager (CX chat squad), `ai-governance-analyst` (retail-cx), `senior-ai-governance-architect` — moderate band per the matrix. Because this waiver is scoped to a single jurisdiction (UK retail) it does not require the legal / mod-104 branch.

Lifecycle: opened 2026-08-06, triaged and compensating-control-assigned 2026-08-06, approved 2026-08-07, active. Pre-expiry review scheduled 2026-11-04 (expiry-30). Two paths from there — VendorX ships 4.2 on schedule and the team upgrades, in which case the waiver closes and `RR-2026-CX-0044` closes with it; VendorX slips, in which case the team requests renewal (`WVR-2026-0184` counts as renewal one of two allowed; a second renewal is the last permissible before the architect requires either a policy amendment or a permanent exception).

## Two common shape mistakes

**Mistake 1 — Open-ended waivers.** A waiver written with expiry "TBD," "pending vendor timeline," "until further notice," or (most commonly) a five-year date the team never expects to hit. Open-ended waivers do not force the review, do not create the pressure to close, and — because they are cheaper to write than the honest artefacts (policy amendment, permanent exception) — silently accumulate. The tooling rejects them at open time; the architect rejects them at review time. Cap by risk band; enforce; escalate at expiry-30 without exception.

**Mistake 2 — Blanket waivers to a product line, or approving without a compensating control.** Two variants of the same failure. The blanket-waiver variant grants a waiver against a policy for "all systems in the retail BU" — which is not a waiver but a de facto policy amendment or applicability change, made by the approving authority alone rather than the change-management process (`05-...md`). The no-compensating-control variant grants relief with only a promise to remediate; the residual risk is un-mitigated, the risk register carries the full weight, and if the residual sits outside appetite (mod-106) the enterprise is now knowingly out-of-appetite with no plan. Both fail the "keep waivers rare, time-bounded, and visible" test — the first fails visibility (the individual system's posture is lost in the blanket), the second fails materiality (there is no bounded reduction of residual risk).

## Summary

The waiver workflow is the escape valve that keeps the policy taxonomy honest. Seven fields make a waiver record: stable ID, scoped clause-and-system, concrete justification, compensating control (per mod-102 chapter 05), residual risk with a named accepting authority and risk-register entry (mod-106), an ISO expiry capped by risk-band ceiling, and an approval chain matched to the risk band. Waivers are distinct from **exceptions** (applicability properties encoded once in the control's filter, not workflow items) and from **compensating controls** (equivalent-outcome mechanisms that do not concede the outcome); one worked example against the same underlying policy makes the difference legible. Renewals are capped; expiry without renewal or promotion generates a waiver-overrun finding; the register is machine-readable, versioned, queryable, and joined to inventory (mod-105), risk register (mod-106), policy-as-code (`03-...md`), and the GRC-for-AI platform (mod-111). Get this workflow right and the enterprise sees every concession; get it wrong and the concessions accumulate in shadows until the auditor finds them first.
