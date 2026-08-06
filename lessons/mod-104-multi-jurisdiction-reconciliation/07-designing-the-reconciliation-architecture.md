# Designing the reconciliation architecture — the obligation record, filter, contract, and deprecation path

## Why this chapter exists

Chapters 1 through 6 read the regimes. This chapter designs the architecture that carries them. Four artefacts do the work:

1. The **obligation record** — one entry per atomic regulatory requirement, keyed by regime and article/section, carrying addressee, trigger, demand, and consequence.
2. The **applicability filter** on each control — the boolean expression, in a controlled vocabulary, that decides *for a given system, does this control apply?*
3. The **evidence contract** on each control — the enumerated artefact set, per jurisdictional rendering, that satisfies each attached obligation.
4. The **deprecation path** on each obligation record — the state machine that captures amendment, supersession, and effective-date staging without breaking the crosswalks that reference the obligation.

Together these are the reconciliation architecture. This chapter designs each artefact concretely — schema, allowed values, worked example — and connects them to the mod-102 control library, the mod-105 AIMS system record, the mod-108 evidence architecture, and the mod-111 GRC-for-AI toolchain. If chapters 1–6 answered *what do we read?*, this chapter answers *what do we carry, in what shape, and how does it change?*

## The obligation record

An obligation record represents *one atomic regulatory requirement*. It is *not* an article or a section — an article often contains multiple atomic requirements. EU AI Act Article 13 contains at least three: the design-for-interpretability requirement, the instructions-for-use content requirement, and the specific enumeration of Article 13(3) items. Each becomes a distinct obligation record; the crosswalks reference the records, not the article.

The obligation record's schema — worked out in YAML because that is how mod-102 chapter 04 renders control entries and the two must interoperate:

```yaml
id: OBL-EUAI-Art14-oversight
regime:
  identifier: eu_ai_act
  version: reg-2024-1689
  publisher: eur-lex.europa.eu
citation:
  article: 14
  paragraphs: [1, 2, 3, 4, 5]
  # For a single-paragraph obligation, the field carries one paragraph;
  # for a multi-paragraph obligation, the whole grouping.
title: Human oversight design of high-risk AI systems
statement: |
  High-risk AI systems shall be designed and developed in such a way,
  including with appropriate human-machine interface tools, that they
  can be effectively overseen by natural persons during the period in
  which they are in use, with the aim of preventing or minimising the
  risks to health, safety, or fundamental rights.
addressee:
  role: provider
  # Enumerated: provider, deployer, importer, distributor,
  # authorised_representative, user, controller, processor,
  # data_fiduciary, developer, operator. The set is the union
  # across regimes; the applicability filter binds enterprise to role.
trigger:
  system_classification:
    - eu_ai_act.high_risk
  use_case: null   # null = does not restrict on use case
  place_of_user: null
  scale_threshold: null
demand:
  category: design_state
  # Enumerated: design_state (a testable property of the built system),
  # process (an ongoing operational practice), document (a produced
  # artefact), test (a measurement result), disclosure (a UX / output
  # artefact), notification (a delivered communication), filing
  # (a submission to a regulator).
  artefact_shapes:
    - hmi_specification
    - oversight_measure_enumeration_per_14_para_4
    - operator_competence_evidence
consequence:
  type: administrative_penalty
  # Enumerated: administrative_penalty, private_right_of_action,
  # supervisory_action, procurement_disqualification, criminal,
  # reputational, contractual.
  ceiling_reference: eu_ai_act_art_99_penalty_tier
effective_date: 2026-08-02   # Article 14 enters application in the
                              # high-risk tranche.
supersedes: null              # Filled when this record supersedes an
                              # earlier obligation record.
superseded_by: null           # Filled when a later obligation
                              # supersedes this one; carries a
                              # migration deadline and a
                              # superseded-evidence-valid-until window.
status: active
# Enumerated: proposed, active, superseded, withdrawn.
related_obligations:
  - OBL-GDPR-Art22-human-intervention
  - OBL-NIST-AIRMF-Govern-3-2
authoritative_interpretations:
  - source: european_commission_guidance
    identifier: <needs-research: any Commission guidance on Art 14>
watch_list_events: []
# Populated as amendments or delegated acts are anticipated.
```

Ten fields — regime, citation, title, statement, addressee, trigger, demand, consequence, effective_date, deprecation_pair — plus a small metadata set (related obligations, authoritative interpretations, watch list). The schema is a superset over regimes; every regime the enterprise reads populates the fields it uses and leaves others null.

**Design notes.**

- **The regime object is versioned.** `regime.version` carries the specific consolidated version the record was authored against. When the Regulation is amended, either the record is updated with a new version pin (and `supersedes` points at its prior self) or a new record is created (and the pair is linked).
- **The trigger object is enumerated per regime dimension.** `system_classification` names classification categories in a regime-specific namespace — `eu_ai_act.high_risk`, `colorado_sb24_205.high_risk`, `us_federal.high_impact_omb_m2521`, `korea_ai_basic_act.high_impact`. The reconciliation architecture does *not* try to normalise these across regimes; each regime uses its own vocabulary and the applicability filter references the specific-regime value.
- **The demand category is the axis-3 attribute from chapter 1.** It drives the evidence contract shape on the referencing control.
- **The consequence type drives risk-appetite decisions.** Mod-106's risk-appetite bands reference the consequence enumeration; the level-60 head-of-AI-governance and the AI committee set the bands; the architect ensures every obligation carries a defensible consequence value so the bands actually bite.
- **The `supersedes` / `superseded_by` pair is the deprecation state machine.** Section below.

## The applicability filter

The applicability filter is a boolean expression, using a controlled vocabulary, that a control's applicability check evaluates for each system. Mod-102 chapter 01 defined the mechanism at a general level. This chapter extends it with the jurisdiction-specific dimensions the international reconciliation requires.

The dimensions the level-50 architect must have decided by library-preface time:

- `system_kind` — classical_ml, foundation_model_provider, foundation_model_deployer, rag_application, agentic_system, ... (enterprise-specific enumeration).
- `system_tier` — enterprise tier from mod-106 (tier_1, tier_2, tier_3, tier_4 typically).
- `use_case` — enumerated set; commonly enterprise-specific with sector overlays.
- `data_class` — data-classification tags from the enterprise's data-classification standard.
- `jurisdiction` — ISO 3166 country codes plus supra-national buckets (`eu`, `council_of_europe`) plus sub-national attributes (`us_state.colorado`, `us_municipality.new_york_city`, ...).
- `addressee_role` — enterprise's role for the system in each jurisdiction (may be different per jurisdiction).
- `variant` — the deployment-variant identifier, where the system is deployed as different variants per market.
- `filing_entity` — the legal entity that would file / attest for the obligation, where different from the parent.
- `effective_date_reached` — a derived boolean per jurisdiction and per obligation, comparing the current date against the obligation's `effective_date`.
- `contract_flow_down` — for procurement-flowed obligations, the specific contract identifier.

A control's applicability filter is a boolean expression over these dimensions. Worked example — the filter on the enterprise's human-oversight control that discharges EU AI Act Article 14, GDPR Article 22 (partially), Colorado SB24-205 deployer duties, and Korea AI Basic Act oversight expectations:

```yaml
applicability:
  # This expression is evaluated per system.
  # If it returns true, the control applies to that system.
  expression: |
    system_tier in {tier_1, tier_2}
    AND (
      # EU AI Act Article 14 scope
      (
        eu_ai_act.high_risk == true
        AND addressee_role.for_jurisdiction("eu") == provider
        AND effective_date_reached.for_obligation("OBL-EUAI-Art14-oversight") == true
      )
      OR
      # GDPR Article 22 scope (deployer / controller side)
      (
        solely_automated_decision == true
        AND has_eu_data_subjects == true
        AND addressee_role.for_jurisdiction("eu") == controller
      )
      OR
      # Colorado SB24-205 deployer scope
      (
        colorado_sb24_205.consequential_decision == true
        AND addressee_role.for_jurisdiction("us_state.colorado") == deployer
        AND effective_date_reached.for_obligation("OBL-COLO-SB24-205-appeal") == true
      )
      OR
      # Korea AI Basic Act high-impact scope
      (
        korea_ai_basic_act.high_impact == true
        AND effective_date_reached.for_obligation("OBL-KR-AIBA-oversight") == true
      )
    )
```

**Design notes.**

- **The expression names obligation identifiers directly.** The control's applicability changes when an obligation is added to it, and — because obligation records carry their own effective-date — the filter's evaluation flips automatically at the date. There is no need for a separate "activate this control in this jurisdiction on this date" workflow.
- **The addressee-role attribute is per-jurisdiction.** A single enterprise can be the provider in the EU and the deployer in Korea for the same underlying model, and the filter must be able to encode that.
- **The applicability filter is closed-world** — a dimension not named in the expression is not evaluated. Mod-102 chapter 01's closed-world convention applies here without change.
- **The expression compiles.** The reconciliation architecture is not asking anyone to eyeball whether a system is in scope; the expression is machine-evaluated by the GRC-for-AI toolchain (mod-111), and the answer is auditable.

## The evidence contract per attached obligation

A single control commonly discharges multiple obligations across multiple jurisdictions. The evidence contract on that control must be able to produce the *specific artefact rendering* each attached obligation expects — same underlying data, potentially different form.

Extending the mod-102 chapter 07 evidence-contract schema with the per-obligation rendering:

```yaml
evidence_contract:
  base_artefacts:
    - hmi_specification_v_current
    - oversight_measure_matrix
    - operator_competence_register
    - decision_log_sample
  per_obligation_renderings:
    OBL-EUAI-Art14-oversight:
      required_form: technical_documentation_annex_iv_section_2_e
      language: en_or_official_language_of_placement
      retention: eu_ai_act_art_18_period
      recipient: internal_and_supervisory_authority_on_request
    OBL-GDPR-Art22-human-intervention:
      required_form: privacy_notice_data_subject_facing_disclosure
      language: language_of_data_subject
      retention: continuously_available
      recipient: data_subject_via_privacy_page_plus_intervention_channel
    OBL-COLO-SB24-205-appeal:
      required_form: consumer_notice_and_appeal_channel_disclosure
      language: en
      retention: retained_per_colorado_ag_rule
      recipient: colorado_consumer_via_notice_channel
    OBL-KR-AIBA-oversight:
      required_form: korean_language_operator_documentation
      language: ko
      retention: to_be_determined_per_enforcement_decree
      recipient: internal_and_ministry_of_science_and_ict_on_request
  sampling_rule:
    decision_log_sample: monthly_random_sample_at_the_greater_of_1_percent_or_100_records
  freshness_rule:
    hmi_specification_v_current: revalidated_within_12_months
    oversight_measure_matrix: revalidated_at_material_system_change
```

**Design notes.**

- **The `base_artefacts` set is common across obligations.** The rendering block specifies how each obligation *consumes* the base artefacts and packages them for the specific audience.
- **Language, retention, recipient are the three renderings that differ most across obligations.** The evidence architecture (mod-108) must budget for translation, for retention-tier storage, and for delivery-channel infrastructure.
- **Sampling and freshness rules are common across obligations** — the underlying data quality is the same regardless of who audits.
- **The evidence contract does not enumerate the specific evidence *instances*.** It enumerates the *classes* and rules for producing them. Mod-111 GRC-for-AI toolchain enumerates and stores instances.

## The deprecation path

Regulations change. The deprecation path is the state machine on the obligation record that captures the change coherently.

**Three change kinds the reconciliation architecture handles.**

- **Amendment in place.** The obligation's statement changes but the identifier survives. New `regime.version` on the record; a change-log entry captures the diff; controls that reference the obligation are inspected for whether their evidence contract remains satisfactory. Common example: an EU AI Act implementing act refines the shape of Article 12's log retention.
- **Supersession.** The obligation is replaced by a new obligation. Old record moves to `status: superseded`, carries `superseded_by: OBL-<new-id>` and a `migration_deadline`; new record is created with `supersedes: OBL-<old-id>` and an `effective_date`. The `superseded_evidence_valid_until` window on the old record captures the transition — pre-transition evidence continues to satisfy audits about pre-transition periods; post-transition evidence must be produced under the new obligation. Common example: OMB M-24-10 superseded by OMB M-25-21.
- **Withdrawal.** The obligation ceases to apply and is not replaced. `status: withdrawn`. Controls that referenced only the withdrawn obligation must be inspected — usually their applicability filter contracts and the control remains in force for other attached obligations, but occasionally a control has no remaining rationale and moves to deprecated (mod-102 chapter 06). Common example: EO 14110 was revoked in January 2025; obligation records keyed to EO 14110 moved to `superseded_by: OBL-EO14179-<successor>` where a successor exists and to `status: withdrawn` where none does.

**The state machine.**

```
proposed --> active --> [amendment loop: new version, same id]
                    \--> superseded --> retained-as-history
                    \--> withdrawn --> retained-as-history
```

**The migration window.** For supersession, the record carries `migration_deadline` (the date by which controls must have migrated their crosswalk to the new record) and `superseded_evidence_valid_until` (the date until which evidence produced under the old obligation remains audit-defensible for the pre-transition period). These are usually not the same date — evidence produced yesterday continues to describe yesterday's compliance state regardless of what happens today; the migration deadline is about *future* production under the new obligation.

**The watch-list.** Before an obligation is `proposed`, it may be *watched* — a bill under legislative consideration, a delegated act under EU Commission drafting, an amendment in draft. The watch list is a separate registry (in the same schema, with `status: watched`), reviewed on a quarterly cadence, with a per-item *trigger-to-active* checklist: what changes when the bill is signed? The pre-work identified in chapters 4, 5, and 6 is where the checklist gets filled in.

## The four artefacts, together

A single AI system moves through the reconciliation architecture as follows:

1. **System record (mod-105 AIMS)** carries the enterprise's classification of the system: tier, kind, use case, data class, addressee role per jurisdiction, variant per market, filing entity per obligation, effective-date-relevant facts (date of first placement on market, date of last material modification).
2. **Applicability engine (mod-111 GRC-for-AI toolchain)** evaluates every control's applicability filter against the system record. Produces the *active control set* for the system.
3. **Evidence architecture (mod-108)** takes each active control's evidence contract and produces the required artefacts, in each attached obligation's rendering.
4. **Obligation register (mod-104's contribution)** is the source of truth for which obligations are `active`, when their `effective_date` reaches, when their deprecation-path status changes. The applicability engine consults the register on every evaluation.
5. **Watch-list workflow** feeds the obligation register with new `proposed` records as bills advance and with `watched` entries for bills pre-legislation.

The control library (mod-102) is the *stable* artefact. The obligation register is the *volatile* artefact. The applicability filter and evidence contract are how they meet. Getting this split right — controls stable, obligations volatile — is the single most consequential architectural discipline in the module. Enterprises that put jurisdiction in the control identifier (`AIC-EUAI-Art14-*`) mix stable and volatile and end up rewriting controls on every regulatory change; enterprises that put jurisdiction in the applicability filter and the obligation register do not.

## The role interfaces around this architecture

- **Level-60 head-of-AI-governance.** Sponsors and ratifies the reconciliation architecture; sets risk-appetite bands per consequence type; escalates novel-obligation questions.
- **Level-50 architect (you).** Authors the obligation-record schema, the applicability-filter vocabulary, and the evidence-contract shape; maintains the obligation register at the schema and workflow level; publishes the reconciliation architecture as an artefact reviewable by internal audit.
- **Legal.** Interprets regulatory text; authors and reviews the *statement*, *addressee*, *trigger*, and *consequence* fields on each obligation record; owns the deprecation-path decisions on legal grounds; provides the authoritative-interpretations citations.
- **Level-25 ai-risk-engineer.** Implements the applicability engine and the evidence-artefact producers; owns the control-side rendering code.
- **Level-15 ai-governance-analyst.** Maintains the obligation register operationally — adds `proposed` records for newly-published regulations, moves records through the deprecation state machine, runs the quarterly watch-list review.
- **Internal audit.** Tests the architecture end-to-end — samples a system, evaluates its active control set, samples the evidence, samples the register for currency.

The reconciliation architecture is one of the few artefacts in the whole senior-AI-governance-architect track where the architect *owns the schema personally* — legal cannot draft the schema, engineering cannot own the shape, and analysts cannot maintain it without the shape in place. Chapter 1's insistence on the architect's lens becomes concrete here.

## Two failure modes

**Failure mode 1 — obligation records that mirror articles rather than atomic requirements.** One obligation record per EU AI Act article produces a register that is easy to build and impossible to reference. Article 15's three atomic requirements (accuracy, robustness, cybersecurity) each need their own record because their evidence contracts diverge — an adversarial-robustness measurement satisfies the robustness requirement but not the accuracy requirement, and the record-level granularity must reflect that.

**Failure mode 2 — a schema without deprecation.** The obligation register is authored, populated with 200 records, and treated as static. Six months later a regulation is amended and there is no state machine to catch it — records are edited in place, evidence loses provenance to the version it was produced against, and the audit trail fractures. The deprecation-path fields are *not optional*; they must be in the schema from day one and populated even for records where `superseded_by` is null.

## Summary

The reconciliation architecture is four artefacts: the obligation record (one per atomic regulatory requirement, keyed by regime and citation, carrying addressee / trigger / demand / consequence and a deprecation-path state machine); the applicability filter on each control (a boolean expression in a controlled vocabulary over system, jurisdiction, addressee-role, variant, filing-entity, effective-date attributes, referencing obligation identifiers directly); the evidence contract per attached obligation (base artefacts common across obligations, plus per-obligation renderings that differ on language, retention, and recipient); the deprecation path on each obligation (amendment / supersession / withdrawal state machine, migration deadline, superseded-evidence-valid-until window). Together they let a single control library carry the EU AI Act, the US federal frame, the US state and municipal patchwork, and the international patchwork without duplication. The architecture consciously puts jurisdiction in the applicability filter and the obligation register — never in the control identifier — so controls stay stable while obligations remain volatile. Getting this split right is what turns the naïve 60-programme problem from chapter 1 into a filter-and-crosswalk problem. Chapter `08-cen-cenelec-jtc-21-and-the-future-state.md` closes the module with the presumption-of-conformity pathway that reduces even this apparatus's operational cost over the next several years.
