# Control inheritance and compensating-control shape

## Why this chapter exists

The library from chapters 01–04 sits at one level of the enterprise: it is *the* enterprise AI catalog. In practice, the systems that consume the library are not all owned by the same team, not all deployed in the same cloud account, not all bound by the same jurisdictional obligations, and not all built on the same platform. A tier-1 credit-decisioning model in the retail bank runs on a different platform, in a different regulatory posture, than a tier-3 internal HR summariser in the corporate functions org.

Two design levers let the same library serve both without forking:

1. **Inheritance** — a control's satisfaction can be inherited from a *parent scope* (an enterprise platform, a group-level programme, a shared service) so that individual system owners do not need to re-implement or re-evidence the same control. Common at scale in FedRAMP shared-responsibility posture and in ISO 27001 group-certification posture.
2. **Compensating controls** — when a control cannot be satisfied *as authored*, an alternate control that achieves an equivalent outcome can be substituted, with the deviation named and justified. Common in every mature control regime (SP 800-53, PCI DSS, ISO 27001) and mandatory in any regime that expects to work in the real world.

This chapter defines both mechanisms for the AI library — how inheritance is represented, when it is legitimate, how compensating controls are structured, how each is auditable, and where the architect draws the line between inheritance / compensation and *waiver* (which is a policy-taxonomy concept, covered in mod-103).

## Inheritance — three legitimate patterns

Not every "we do this centrally" is inheritance. Three specific patterns qualify.

**Pattern A: platform-provided.** A shared platform component (model registry, feature store, inference runtime, guardrail service) implements the control on behalf of every system that consumes it. The classic AI examples: the model registry provides training-data provenance capture for every model registered against it (`AIC-DAT-014`); the guardrail service provides prompt-injection isolation for every LLM routed through it (`AIC-ENG-041`); the observability stack provides drift-monitoring for every model whose predictions it ingests.

**Pattern B: group-level shared.** A group-level programme owns the control and produces evidence that covers all in-scope entities. Enterprise information-security awareness training satisfies `AIC-GOV-*` awareness controls for every business unit; enterprise incident-management infrastructure satisfies `AIC-OPS-*` incident-response controls; a group AIMS satisfies management-review controls for every business unit inside the AIMS scope (mod-105).

**Pattern C: contract-satisfied.** A third-party provider is contractually required to satisfy the control on the customer's behalf, and provides evidence (SOC 2 report, ISO/IEC 42001 certification, MSA control mapping) that the customer inherits. The foundation-model provider's model-security programme satisfies parts of `AIC-SEC-*` model-protection controls (mod-109 designs the third-party contract shape).

Patterns A and B are *internal* inheritance; pattern C is *external* inheritance. Both are legitimate and both need explicit representation.

## Representing inheritance in the library

Inheritance is a *property of the SSP for a specific system*, not a rewriting of the control statement in the catalog. The catalog states the outcome; the SSP names how that outcome is achieved *for this system*, including whether it inherits from a parent scope.

The SSP template the architect ships has an `implementation-status` and `inheritance` block per control:

```yaml
control_implementation:
  control_id: AIC-DAT-014
  implementation_status: inherited
  inheritance:
    kind: platform-provided
    parent_scope: ENT-PLATFORM-REG-08
    inherited_evidence:
      - artifact: platform-provenance-attestation-2026-Q3.json
        producer: platform-model-registry-team
        cadence: quarterly
    delta_this_system:
      - "Confirm the model registry entry for this system exists (AIC-DAT-014-T1 local step)."
      - "Verify integrity hash link on release (AIC-DAT-014-T2 local step)."
```

Three fields do the work:

**`kind`** — one of `platform-provided`, `group-shared`, `contract-satisfied`. Names which pattern applies; drives who is on the hook to produce the base evidence.

**`parent_scope`** — the entity, service, or contract that owns the underlying satisfaction. Must be resolvable; do not write "the platform team" — write the specific service ID (`ENT-PLATFORM-REG-08`) or the specific contract (`MSA-VENDOR-14 §7.2`).

**`delta_this_system`** — the residual work this system's owner still does, even under inheritance. This is the field most inheritance implementations forget, and its omission is the most common audit finding at this layer. Inheritance rarely means "no local work"; it means "the base satisfaction is elsewhere, and here is the reduced local scope."

## When inheritance is legitimate — the four tests

Not every plausible inheritance claim is defensible. Before the architect signs off on inheritance being *addable to the SSP template*, the parent scope must pass four tests.

**Test 1: the parent scope actually implements the control.** Sounds obvious. It is not. Many claimed inheritances rest on a parent scope that *could* implement the control but does not, or does so only for a subset of systems. The parent scope must have its own control-satisfaction evidence, of the same shape as any other control in the library, before it can be inherited from.

**Test 2: the parent scope's evidence covers this system's scope.** A platform provenance store that only captures registered models does not satisfy provenance for a model that was fine-tuned offline and never registered. The applicability filters on the parent-scope evidence (chapter 01) must be a superset of the child system's scope.

**Test 3: the parent scope's cadence is at least as strict as the control's cadence.** If `AIC-DAT-014-T1` runs quarterly and the platform team only produces provenance attestations annually, the inheritance is broken. Cadence mismatches are silent failures — the system owner assumes inheritance works, the auditor asks for the last quarterly evidence, no one has it.

**Test 4: change to the parent scope propagates.** If the platform team retires the provenance store or changes its schema, every inheriting SSP is now stale. The library requires a *change-propagation contract* from every parent scope: notification to inheriting systems, minimum notice period, migration guidance. Without this contract, inheritance is a landmine.

The architect maintains the *inheritance registry* — the list of parent scopes that have passed the four tests and are eligible sources of inheritance — and updates it as parent scopes onboard, change, or degrade. Downstream SSPs may only claim inheritance from registered parents.

## Compensating controls — when the outcome is right but the mechanism differs

A compensating control is *a different control that achieves an equivalent outcome*. It is legitimate; it is not a waiver. Two use cases dominate in the AI library:

**Use case 1: technical constraint.** The library's default mechanism is not available for this system (a legacy inference stack that does not accept the guardrail service; a foundation-model provider whose contract does not allow evaluation-access; an on-prem sensor whose runtime does not run the enterprise observability agent). The compensating control uses a different mechanism to deliver the same outcome.

**Use case 2: overlap with existing controls.** A pre-existing enterprise control (from the ISMS, the SOC catalog, the SR 11-7 MRM programme) already delivers the outcome the AI control targets. Rather than duplicate, the AI SSP names the existing control as compensating.

## Shape of a compensating-control entry in the SSP

The SSP template's compensating-control shape:

```yaml
control_implementation:
  control_id: AIC-EVD-022
  implementation_status: compensating
  compensating:
    justification: >
      The legacy pricing model runs on Platform-X, which does not
      support the enterprise audit-log agent (AIC-EVD-022 default
      mechanism). The compensating control uses Platform-X's native
      audit-export pipeline into the enterprise SIEM.
    alternate_mechanism:
      description: >
        Platform-X native audit export → enterprise SIEM (SIEM-Ops-14)
        → 10-year immutable retention in the compliance archive.
      artefacts:
        - platform-x-audit-export-config.md
        - siem-ingestion-attestation-2026-Q3.json
    equivalence_argument: >
      The compensating mechanism captures every inference request and
      response with the same immutability and retention guarantees
      AIC-EVD-022's evidence contract requires (append-only, 10-year
      retention, cryptographic integrity). The auditable trail is
      structurally equivalent; only the transport differs.
    residual_risk:
      description: >
        Platform-X's export cadence is 15-minute batched vs. the
        default control's real-time stream, introducing at most a
        15-minute reconstruction window on incident.
      accepted_by: bu-risk-committee
      review_cadence: annual
    expiry: 2027-12-31
```

Six fields do the work:

- **`justification`** — why the default cannot be satisfied. Concrete, not "operational reasons."
- **`alternate_mechanism`** — the substitute, with pointer to artefacts.
- **`equivalence_argument`** — the *outcome-level* argument that the alternate delivers what the control statement requires. This is the load-bearing field at audit; the architect's job is to make sure the template makes it hard to leave weak.
- **`residual_risk`** — where the compensating mechanism is not quite equivalent, name the gap, size it, and route it to the risk-appetite framework (mod-106) rather than pretending it does not exist.
- **`accepted_by`** — the risk-owning body that signed the residual risk. Must resolve to a body with authority; a director cannot accept residual risk that belongs to the risk committee.
- **`expiry`** — every compensating control has a review date. Compensations that were legitimate in 2025 (legacy platform in flight of migration) may not be legitimate in 2028; the expiry forces the review.

## The architect's compensating-control review posture

Compensating controls are the mechanism through which the library survives contact with reality. They are also the mechanism through which discipline erodes: every "yes, compensate" that is granted casually degrades the equivalence bar for the next request.

Three review disciplines keep the bar high:

**Pattern the compensations.** When two SSPs propose the same compensating control for the same reason, that is not two individual grants; it is a signal the library should adopt the compensating mechanism as a first-class control. Add it to the catalog, retire the ad-hoc compensations.

**Reject weak equivalence arguments.** "The alternate mechanism also captures logs" is not equivalence. Equivalence means the alternate mechanism, tested against the control's testing procedure, yields the same evidence with the same properties. If the equivalence argument does not describe how the mechanism satisfies the testing procedure, send it back.

**Track compensations as a first-class portfolio metric.** The number of compensating controls per system, per business unit, per platform is a health signal. A tier-1 system with more than a handful of compensations is either genuinely hard to fit the library or a candidate to be reshaped. The architect publishes this metric alongside the library release.

## Where inheritance / compensation ends and waiver begins

A waiver — covered in mod-103 — is *the deliberate acceptance that a control will not be satisfied*, for a bounded time, at a named risk. Waivers are not inheritance and not compensation. Three distinguishing questions:

- **Is the outcome achieved?** If yes → inheritance (elsewhere) or compensation (by another mechanism). If no → waiver.
- **Is there an alternate mechanism producing equivalent evidence?** If yes → compensation. If no → waiver.
- **Is the base satisfaction owned by a resolvable parent scope?** If yes → inheritance. If no → compensation or waiver.

The library's SSP template enforces this taxonomy: the `implementation_status` field is one of `implemented`, `inherited`, `compensating`, `waived`, `not-applicable`. Each maps to a distinct downstream workflow, and mixing them (a "compensating waiver," an "inherited compensation") is a schema error the template rejects.

## Multi-library composition — group and business-unit libraries

For very large enterprises, a single library across a global business is not always workable. Two shapes work at scale, both compatible with the library architecture chapter 01 assumed.

**Shape 1: single library, multiple profiles.** The catalog remains monolithic; jurisdictional / sector / business-unit variation lives in profiles (chapter 04). Preferred default — it keeps a single source of truth for what a control statement says.

**Shape 2: group catalog + business-unit overlay catalogs.** A group catalog owns cross-cutting controls (governance, risk, evidence, third-party); each business-unit catalog imports it and adds BU-specific controls. Used when a business unit's regulatory regime is materially different (e.g., healthcare BU vs. corporate functions) and the BU needs its own catalog release cadence.

Shape 2's inheritance model is straightforward in OSCAL: the BU catalog imports the group catalog as an OSCAL resource; controls the BU inherits from the group carry an explicit `link rel="inherits-from"` back to the group control. The architect resists shape 3 (per-BU forked catalogs with independent versions of the same controls) — that is the shape mod-101 warned about.

## Two common shape mistakes

**Mistake 1 — Inheritance without a parent-scope contract.** Claiming a control is "provided by the platform team" without the four tests passed and without a change-propagation contract. The claim looks fine until the platform team refactors, and then every SSP that inherited quietly stops satisfying.

**Mistake 2 — Compensating control that changes the outcome.** Substituting a "similar" mechanism whose actual output is not what the control's evidence contract needs. The compensation looks reasonable read individually and is caught only when the auditor asks for the evidence-contract artefact and the alternate mechanism has never produced one. Always test compensations against the *evidence contract*, not against the guidance.

## Summary

Inheritance and compensating controls let the same AI catalog serve heterogeneous systems without forking. Inheritance is legitimate when the parent scope passes four tests (implements the control, applicability covers the child, cadence at least as strict, change-propagation contract in place); compensation is legitimate when an alternate mechanism achieves the outcome and the equivalence argument holds against the control's testing procedure. Both live in the SSP, not in a rewrite of the catalog. Waivers are different — they concede the outcome is not achieved — and belong to policy (mod-103), not to the library. Multi-library composition uses profiles by default and group / BU overlays only where regulatory difference forces it. Get these mechanisms right and the same catalog scales from a 20-model start-up posture to a global multi-BU enterprise without fork.
