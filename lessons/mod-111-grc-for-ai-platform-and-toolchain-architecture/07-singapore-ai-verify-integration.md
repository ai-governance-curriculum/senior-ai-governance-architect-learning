# Singapore AI Verify as the reference open-source assurance testing framework

## Why this chapter exists

An enterprise defending its AI-governance posture under external audit is, in the end, defending the evidence its control-family attestations cite. A fairness attestation on a tier-3 customer-facing model is defensible when the artefact it points to is a fairness test whose methodology the auditor can independently reason about; the attestation is fragile when the artefact is a screenshot of a proprietary vendor tool's dashboard whose scoring the auditor cannot reproduce. The distinction is not academic — it is the difference between a certification body accepting the enterprise's ISO/IEC 42001 evidence pack on first pass and returning it with a non-conformity that says "insufficient evidence of methodology".

The level-50 architect who has already fixed the GRC-for-AI reference architecture (chapter 01), the workflow layer (chapter 03), the observability integrations (chapter 06), and the runtime-security integrations (chapter 05) faces one further shape decision the reference architecture will be evaluated on: whether the enterprise stands up its own testing framework from first principles for every testable attribute in its control library, or whether it integrates a **neutral, open-source, community-maintained testing framework** as the reference toolkit for a defined set of testable attributes and lets the enterprise's own control attestations cite that framework's outputs. The AI Verify Foundation's AI Verify toolkit, published in association with Singapore's Infocomm Media Development Authority (IMDA), is the reference example of the second shape.

This chapter fixes the *integration shape* — how AI Verify hooks into the reference architecture's workflow layer, where its outputs land in the mod-108 evidence architecture, which mod-102 controls its testable-attribute vocabulary crosswalks against, and how the architect keeps it complementary to the observability and runtime-security platforms it does not replace. The chapter does not teach AI Verify itself — the toolkit's own documentation, release notes, and testable-attribute catalogue are the primary reference for that, and specific version and coverage claims in this chapter carry `<!-- needs-research: ... -->` markers. What the chapter fixes is the substitute-vs-testing-framework distinction that is the first failure mode against AI Verify and the reason enterprises misuse it when they do.

## What AI Verify is

AI Verify is an **assurance testing toolkit** — a software framework that instruments a set of testable attributes across the AI system lifecycle and produces machine-consumable reports the enterprise can bind to control-family attestations. It is published by the AI Verify Foundation, a non-profit associated with Singapore's IMDA; the framework's provenance and open-source licence are its two structurally significant properties for the reference architecture. `<!-- needs-research: verify AI Verify Foundation current release version at time of authoring, the exact set of testable attributes covered (technical tests around robustness, fairness, explainability, and adjacent classes; process checks), the toolkit's supported ML frameworks and model families, and the exact reporting schema and output artefact format -->`

Two properties of the toolkit are what make it a candidate for the reference architecture's integration slot:

- **Open-source and neutral.** The framework's methodology is auditable — the auditor can read the source, reason about the test implementation, and reproduce the run. This is the property that lets the enterprise's control attestations cite AI Verify test outputs without the auditor needing to trust a proprietary tool's opaque scoring. It is the same argument the enterprise makes for citing NIST SP 800-53 controls or ISO/IEC 27001 clauses in its security evidence — the reference frame is externally scrutinisable.
- **Testable-attribute vocabulary.** The framework names the attributes it tests — robustness, fairness, explainability, and adjacent classes — in a vocabulary the enterprise can map its own mod-102 control-family names against. A fairness attestation in the enterprise catalogue whose test path was previously "run the data-science team's ad-hoc notebook" can, after mapping, cite an AI Verify fairness test whose methodology has been reviewed by the framework's contributor community.

Neither property makes AI Verify a governance framework. Singapore has published a Model AI Governance Framework separately; the framework itself is a mod-104 (multi-jurisdiction reconciliation) reference point alongside the OECD AI Principles, NIST AI RMF, and adjacent jurisdictional frames. **AI Verify is the testing toolkit that sits under that framework**; it is not itself a governance framework, and the chapter's first failure mode is what happens when it is treated as one.

## The integration shape

The architect commits the reference architecture to a specific integration shape for AI Verify, and the shape is the same shape any assurance testing framework the enterprise adopts under the same slot must fit. The shape has five points, in workflow order.

**1. Workflow trigger.** AI Verify runs are executable events triggered by the mod-111 workflow layer (chapter 03), not by ad-hoc scripts a data-science team runs when it feels the need. The trigger is one of a defined set: a scheduled cadence bound to the control-testing flow, a drift-triggered re-run whose input is an observability signal above a threshold, a pre-deployment gate re-run bound to a version increment, or an on-demand run initiated by second-line assurance under a documented trigger reason. Every run carries the trigger reason as a first-class field.

**2. Run execution.** The run itself is executed in the enterprise's test environment against the artefact under test — the model version, the dataset slice, the evaluation harness the run is scoped to. Execution is deterministic to the extent the toolkit permits — the same inputs against the same test at the same framework version produce the same output — and the run captures its own reproducibility inputs (framework version, test-suite version, input artefact hashes, environment fingerprint) so a future re-run under audit can be reproduced.

**3. Output.** The run produces a machine-consumable report in the framework's native output schema. The report is not the enterprise's canonical evidence artefact yet; it is the framework-native artefact the enterprise will normalise before storing.

**4. Evidence index write.** A schema-mapping adapter (analogous to the normalisation layer in mod-110 chapter 03) maps the framework-native report into the enterprise's canonical evidence-artefact shape — the mod-108 evidence-artefact schema — and writes the resulting artefact to the mod-108 evidence index. The write carries the mod-102 control-family reference(s) whose attestation the artefact substantiates, the freshness window governing the artefact's validity, the lineage back to the framework-native output, and the trigger reason that produced the run.

**5. Control attestation reference.** The mod-102 control-family attestation that the artefact substantiates references the mod-108 evidence-artefact id — not the framework-native report, not the run log, and never a screenshot. When the certification body samples the attestation, the auditor traverses attestation → evidence artefact → framework-native report → run inputs, and the reproducibility chain holds end to end.

The shape isolates the framework-specific from the enterprise-canonical at the schema-mapping adapter, which is the same architectural move the observability normalisation layer makes for its platforms. The enterprise does not re-instrument AI Verify's native format inside the register; the framework-native shape lives in the lineage tail, and the register holds the canonical form.

## Complementarity with observability (chapter 06) and runtime security (chapter 05)

AI Verify is complementary to the observability platforms and to the runtime-security platforms the reference architecture already integrates, and the architect must be explicit about the complementarity so that neither the enterprise nor the auditor mistakes the three for redundant coverage.

- **Observability (chapter 06).** The observability platforms — Fiddler, Arthur, WhyLabs, Evidently, and successors — hold **continuous production monitoring**: drift signals, performance signals, data-quality signals, fairness signals, LLM-quality signals emitted at the cadence production traffic is served. AI Verify is invoked as a **testing event** at defined moments in the workflow — on a schedule, on a drift trigger, at a version increment. The cadence is different; the signal class overlaps in areas like fairness but the shape differs (a continuous drift trajectory versus a point-in-time test run against a controlled input). Both feed the same mod-108 evidence-artefact index, and the index's canonical shape is what lets a fairness attestation cite a continuous observability signal *and* a point-in-time AI Verify test result as complementary evidence.
- **Runtime security (chapter 05).** The runtime-security platforms — Robust Intelligence, Lakera Guard, Calypso AI, HiddenLayer, Protect AI, and successors — hold **attack-surface signal**: prompt-injection detections, jailbreak attempts, output-channel exfiltration flags, adversarial-example patterns. AI Verify's testable attributes are largely orthogonal to this signal class; robustness testing under AI Verify does not replace prompt-injection detection at the inference boundary, and prompt-injection detection does not replace robustness testing under controlled adversarial input. Both are consumed as evidence sources under different control families — AI Verify's outputs bind to `AIC-ROB-*` and `AIC-FAIR-*` families; runtime-security signals bind to `AIC-SEC-*` families and the SOC interface (mod-110 chapter 05) governs their handoff.

The unifying property across the three is that they all land as mod-108 evidence artefacts in the same index, referenced by the mod-102 attestations they substantiate. The **evidence-artefact index is the single point of convergence**; the three producers keep their own cadences, their own signal classes, and their own native shapes upstream of the schema-mapping adapters that feed the index.

## The failure modes

Three failure modes recur when enterprises integrate an open-source assurance testing framework like AI Verify, and each has a shape the architect designs against.

**Failure mode (a) — policy substitute.** The enterprise treats AI Verify as a governance framework rather than as a testing toolkit. Executives cite the framework's presence in the reference architecture as evidence of the enterprise's AI-governance maturity; internal communications describe the enterprise as "aligned to AI Verify" as though the framework were the policy. The framework is a testing toolkit, not a policy — it does not name the enterprise's risk categories (mod-106), it does not define the enterprise's control library (mod-102), it does not commit the enterprise to a jurisdiction-reconciled control set (mod-104), and it does not stand as the AIMS reference under ISO/IEC 42001 (mod-105). The failure mode is visible when an auditor asks "what is your AI governance framework?" and the response cites AI Verify; the correct response cites the enterprise's own AIMS documentation and the frames it reconciles against, with AI Verify named as one of the testing toolkits under that architecture.

**Failure mode (b) — free-form report.** AI Verify runs are executed but their outputs land as PDF reports in a shared drive, in a team Confluence page, or as attachments to a Slack thread. The runs happen; the outputs exist; nothing is bound to a mod-108 evidence artefact with lineage. When a control attestation cites "the AI Verify fairness report from Q3", the citation is a filename in a shared drive whose freshness the register cannot track, whose lineage back to the run inputs is not retained, and whose retention window is whatever the shared drive's default policy is. The mod-108 evidence-artefact index write (point 4 of the integration shape) is the invariant this failure mode violates. The fix is that no AI Verify run is considered complete until the schema-mapping adapter has written the canonical evidence artefact to the index and the attestation citation points to that id.

**Failure mode (c) — unrationalised competing tests.** The enterprise has AI Verify integrated *and* its own proprietary tests — a data-science team's fairness notebook, a platform team's robustness harness, a red-team's custom evaluation suite — running against overlapping testable attributes. Two or three test outputs land against the same control attestation, and no rationalised claim explains why the attestation cites one and not the others, or which is the primary evidence and which the corroborating. When the auditor asks "which of these three fairness numbers is the one your attestation depends on?", the enterprise cannot answer. The fix is that each control attestation names its **primary evidence source** and its rationale for that choice; competing tests are either explicitly corroborating (with a documented reason for the second test to exist) or explicitly deprecated. The architect owns the rationalisation; individual teams do not get to add tests to an attestation's evidence set without the control owner's ratification.

## YAML-shaped integration schematic

The following is illustrative of the integration shape the reference architecture commits to. Field names and enumerations are the enterprise's to fix; the shape is what generalises.

```yaml
ai_verify_integration:
  framework:
    name: AI Verify
    publisher: AI Verify Foundation (associated with Singapore IMDA)
    version: <!-- needs-research: current release version -->
    licence: open-source
  workflow_trigger:
    trigger_id: WF-CTL-TEST-fairness-tier3-scheduled
    trigger_reason: scheduled-quarterly-fairness-testing
    invoked_by: mod-111-workflow-layer
    scope:
      system_id: SYS-2027-0042
      version: v3.4.1
      dataset_slice: quarterly-eval-slice-2027Q3
  run:
    run_id: AIV-RUN-2027-08-06T09:15:00Z-SYS-2027-0042-fairness
    framework_version: <!-- needs-research: current release version -->
    test_suite: <!-- needs-research: exact testable-attribute suite -->
    reproducibility:
      input_artefact_hashes: [ <hash1>, <hash2> ]
      environment_fingerprint: <fingerprint>
  output:
    framework_native_report_ref: <path in test-artefact store>
    schema: ai-verify-native
  evidence_index_write:
    adapter: ai-verify-to-mod108-adapter-v1.2
    evidence_artefact_id: EVA-2027-Q3-SYS-2027-0042-fairness-aiverify
    canonical_schema: mod108-evidence-artefact-v3
    control_refs: [ AIC-FAIR-004, AIC-FAIR-007 ]
    freshness_window: 90d
    lineage_ref: AIV-RUN-2027-08-06T09:15:00Z-SYS-2027-0042-fairness
    trigger_reason_carried: scheduled-quarterly-fairness-testing
  attestation_reference:
    attestation_id: ATT-2027-Q3-SYS-2027-0042-AIC-FAIR-004
    primary_evidence_ref: EVA-2027-Q3-SYS-2027-0042-fairness-aiverify
    corroborating_evidence_refs: [ <observability continuous fairness signal ref> ]
    primary_rationale: framework-native reproducible test against controlled input
```

The schematic makes the invariants readable. The `workflow_trigger` block fixes the run as a workflow-layer event with a named reason. The `run` block carries reproducibility inputs sufficient for a future re-run under audit. The `output` block holds the framework-native artefact in the test-artefact store, and only there. The `evidence_index_write` block is where the schema-mapping adapter produces the canonical mod-108 artefact and binds it to the mod-102 control references it substantiates. The `attestation_reference` block names the primary evidence source with rationale, rationalising the attestation's evidence set explicitly.

## Invariants

Three invariants hold across every AI Verify integration the reference architecture ratifies.

**Invariant 1 — AI Verify is a testing event, not a policy.** The framework is instrumented as a testing toolkit under the enterprise's AI-governance architecture, not as a substitute for that architecture. AIMS documentation (mod-105), the enterprise risk taxonomy (mod-106), the control library (mod-102), and the jurisdiction-reconciled control set (mod-104) are the enterprise's; AI Verify is a testing framework those artefacts cite as one of the toolkits their controls' evidence paths use. Failure mode (a) is what happens when this invariant is dropped.

**Invariant 2 — outputs land in the mod-108 evidence index.** No AI Verify run is complete until its output has been mapped through the schema-mapping adapter into the enterprise's canonical evidence-artefact shape and written to the mod-108 evidence-artefact index, with the control-family reference(s), freshness window, lineage, and trigger reason attached. Framework-native reports in shared drives are not evidence for the purposes of the enterprise's attestation architecture; only the canonical artefact in the index is. Failure mode (b) is what happens when this invariant is dropped.

**Invariant 3 — runs are triggered by the workflow layer, not by ad-hoc scripts.** Every AI Verify run has a named trigger — scheduled cadence, drift-triggered re-run, pre-deployment gate re-run, on-demand under a documented trigger reason — and the trigger is invoked by the mod-111 workflow layer (chapter 03), not by a data-science team's cron job or a platform engineer's manual invocation. The trigger reason is carried through the run and into the evidence artefact, so an auditor sampling the artefact can reconstruct why the test happened when it did. Ungoverned runs — outputs that exist but whose provenance the register cannot explain — are the invariant this closes off.

## Summary

AI Verify, published by the AI Verify Foundation in association with Singapore's IMDA, is the reference open-source assurance testing framework the mod-111 architect integrates into the GRC-for-AI reference architecture for testable-attribute reporting. Its open-source, community-maintained methodology is what lets the enterprise's control-family attestations cite its outputs without asking the auditor to trust a proprietary vendor's opaque scoring; its testable-attribute vocabulary is a candidate crosswalk source for the mod-102 control library. The integration shape has five points: a workflow-layer trigger (chapter 03), a reproducible run against a defined artefact under test, a framework-native output, a schema-mapping adapter that writes a canonical mod-108 evidence artefact, and a mod-102 control-family attestation reference to that artefact. AI Verify is complementary to the observability platforms (chapter 06), which hold continuous production monitoring, and to the runtime-security platforms (chapter 05), which hold attack-surface signal; the three keep their own cadences and native shapes upstream of the evidence-artefact index that is their point of convergence. Three failure modes recur — treating AI Verify as a policy substitute, letting outputs land as free-form reports outside the evidence index, and running unrationalised competing tests without a primary-evidence rationale — and three invariants close them off: AI Verify is a testing event and not a policy; outputs land in the mod-108 evidence index; runs are triggered by the workflow layer and not by ad-hoc scripts. The Model AI Governance Framework Singapore publishes separately is a mod-104 jurisdiction reference; AI Verify itself is the assurance testing toolkit that sits under such a framework, and the chapter's shape is portable to any successor open-source testing framework the enterprise adopts under the same architectural slot.
