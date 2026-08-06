# The pre-deployment assurance gate — the second line's authoritative launch decision

## Why this chapter exists

The pre-deployment assurance gate is the single most consequential forum in the AI assurance architecture. It is the point at which the enterprise, through its second line, makes an *authoritative decision* — under whose signature and against what evidence — that a specific AI system is fit to enter (or return to) the deployment population. It is the artefact the certification body will ask to see first when it samples "how do you decide a system is ready to launch." It is the artefact the sector regulator's examiner will ask to see when a post-deployment incident requires proof that the enterprise's assurance discipline was sound at the time of launch. It is the artefact the plaintiff's lawyer will subpoena when litigation follows a consequential-decision harm.

An enterprise without a pre-deployment gate is not lacking a process — it is lacking the *architecture*. Some team is inevitably making launch decisions; without the gate the decisions are scattered, unrecorded, and unowned. This chapter designs the gate as an architectural artefact: what evidence must exist, what reviews must pass, what stakeholders must sign, what escalation paths open, how the gate coordinates with the `ai-evaluation-engineer` (peer, level 35) who runs the release-assurance methodology inside the gate, and how the gate's decision record composes upstream (into the AIMS operations calendar) and downstream (into the ongoing assurance programme of chapter 03).

## What the gate is, structurally

The pre-deployment assurance gate is:

- A **second-line-owned forum** — the head of AI governance and the level-50 architect design it; the head or a designated seat chairs it; the level-35 evaluation engineer executes the methodology; the level-25 risk engineer files the scoring; the level-15 analysts collect the analyst-tier evidence.
- Executing **against a defined evidence contract** — the gate does not decide "does this look ready?"; it decides "does the evidence contract hold?" and, if it does, "is the residual risk acceptable under the appetite architecture?"
- **Producing an authoritative record** — the gate's output is a decision-record with named signatures, a bounded scope of what was decided, and a cross-reference into the risk register, the SoA, and the AIA register.
- **A stage of the enterprise's SDLC** — the gate is not something extra bolted on; it is a required stage in the AI-system SDLC that the enterprise engineers cannot ship past. Chapter 03 walks the parallel post-deployment re-assessment triggers that keep the gate's decision current.

The gate is *not*:

- **A first-line ship review.** The engineering team's ship-readiness review can and should happen; it is not the assurance gate. Confusing the two is invariant-1 failure in chapter 01.
- **A rubber-stamp of first-line evidence.** The second line's independence responsibility means the gate re-derives claims where re-derivation is possible; where re-derivation requires infrastructure the second line lacks, the gate specifies compensating sampling that the third line will later verify.
- **Sufficient in isolation.** The gate decides launch; the ongoing assurance programme (chapter 03) keeps the decision fresh. A gate without an ongoing programme produces the "certified on day one, unmonitored forever after" failure the EU AI Act's Article 72 post-market monitoring obligation is written to prevent.

## The evidence contract — what must exist before the gate can convene

The gate convenes only when the evidence contract is *complete for the system's tier*. Different tiers require different evidence bundles; the architecture fixes the mapping. The chapter uses the four-tier shape common across the track (tier-1 lowest-risk internal-only through tier-4 highest-risk consequential-decision or safety-critical); adapt to your enterprise's actual tier taxonomy.

**Base evidence bundle — required for every system, every tier.**

| Artefact | Owner (first line) | Reviewer (second line) | Reference |
|---|---|---|---|
| System-purpose statement / model card | Model owner | Governance analyst reads for coherence with claims | mod-108 |
| Data provenance record | Data engineering | Governance analyst spot-checks lineage | mod-108 |
| Control-library implementation evidence per applicable SoA row | Platform / model owner | Analyst collects; evaluation engineer verifies sampled controls | mod-102/108 |
| AIA (AI Impact Assessment) at current version | Model owner authors; second line reviewer | Second-line reviewer files | mod-105 chapter 04 |
| Risk register entry per applicable taxonomy category, with inherent / residual / control-defeated triple | Model owner + risk engineer | Risk engineer files against quantification contract | mod-106 |
| Third-party dependency register (models, datasets, tools) | Model owner | Third-party governance function | mod-109 |
| Change-management record | Platform team | Governance analyst | mod-105 chapter 06 |

**Overlays for higher-risk tiers.**

- **Tier-3 and tier-4 (consequential decisions, safety-critical, regulator-facing).** Independent evaluation run by the level-35 evaluation engineer against a pre-registered eval set (chapter 06); red-team engagement report where applicable (typically for GenAI systems with user-facing dialogue or agentic action); fairness or disparate-impact measurement per protected characteristic where the deployment context requires it; robustness / adversarial-ML measurement where the threat model requires it; interpretability or reason-code evidence where the regulatory obligation requires it (e.g., Equal Credit Opportunity Act adverse-action reason codes in US consumer credit).
- **Tier-4 only (highest-risk).** Post-market monitoring plan pre-registered (mod-110); serious-incident response plan pre-registered (mod-110); rollback / kill-switch plan pre-registered and demonstrated; sector-specific regulatory attestations where applicable (e.g., FDA 510(k) clearance for medical AI, notified-body attestation under the EU AI Act Article 43 conformity assessment procedure for regulated high-risk systems).

**AI Act-specific overlays.** For systems in scope of the EU AI Act as high-risk (Annex III use cases): the technical documentation to Article 11 shape; the automatically generated logs plan to Article 12 shape; the risk management system evidence per Article 9; the data-and-data-governance evidence per Article 10; the human-oversight design per Article 14; the accuracy / robustness / cybersecurity evidence per Article 15. The gate cannot convene on a high-risk EU AI Act system without these; the gate's decision record explicitly names the Article coverage. <!-- needs-research: verify Article 11 through Article 15 obligation scope against the final Regulation (EU) 2024/1689 text as consolidated. -->

**Evidence-quality gates the architect owns.** The evidence contract is checked before the gate convenes; the check is analyst-tier work with an evaluation-engineer spot-check. Five quality gates (adapted from mod-106 chapter 06's scoring quality gates):

1. **Completeness against tier.** Every artefact in the tier's bundle is filed.
2. **Freshness.** Every artefact is within its currency window (typically 90 days for most; 30 days for evaluation runs on rapidly changing systems; ad-hoc where the artefact is one-time such as the model card at first launch).
3. **Traceability.** Each artefact cross-references the artefacts it depends on (the risk register entry cites the AIA; the AIA cites the control library rows; the model card cites the eval set).
4. **Reproducibility.** Where the artefact reports a measurement, the measurement is reproducible from the reported inputs — the eval set is versioned, the seed is fixed, the reporting includes the artefact hash.
5. **Independence-of-authorship.** For tier-3 and tier-4, the second-line artefacts (evaluation reports, second-line review of AIA) are authored by roles distinct from the first-line model owner.

Failure of any gate blocks convene. The gate does not begin with "we'll figure out the missing artefact during the review."

## The gate's review — the six sequenced reviews

Once the evidence contract is complete, the gate itself sequences six reviews. The sequence matters: earlier reviews establish the foundation later reviews depend on.

**Review 1 — scope and applicability.** Confirm the system is in the scope of the AIMS (per mod-105 chapter 02), name the applicable taxonomy categories (per mod-106), name the applicable jurisdictions (per mod-104), determine the risk tier. If the system is out-of-scope or the tier is wrong, the review stops here and the system re-enters at the correct point.

**Review 2 — evidence-contract discharge.** Walk the base bundle plus tier-appropriate overlays. Each artefact is confirmed present, fresh, traceable, reproducible, and independently authored where required. Missing or deficient artefacts either produce a *conditional* decision (defer until artefact is remediated) or a *blocked* decision (the system does not launch). Rubber-stamping is prevented by the evaluation engineer's independent verification of a sampled subset — chapter 06 walks the sampling contract.

**Review 3 — control-implementation state against SoA.** For every SoA row applicable to the system (per the applicability filter from mod-102 chapter 01), confirm the control is implemented, evidence is on file, and the control's status is `in-place` (not `partially-in-place` or `planned` — the two latter statuses require a compensating-control declaration or an accepted-residual declaration that the risk engineer files).

**Review 4 — residual-risk reading against appetite.** The risk engineer's residual scores per category are compared against the tolerance table (per mod-106 chapter 03). Scores within tolerance pass this review. Scores outside tolerance either produce a *rejected* decision or trigger the escalation path — escalation to the head of AI governance for tier-3, to the AI-accountable executive for tier-4, to the audit committee for stop-shipping-threshold breaches. The gate does not accept residuals outside tolerance under its own authority.

**Review 5 — human-oversight and rollback readiness.** Confirm the human-oversight design is implemented and the operator population has been trained. Confirm the rollback / kill-switch design is implemented and the on-call population has been rehearsed. For tier-4 systems, review the pre-registered post-market monitoring plan and the pre-registered serious-incident response plan.

**Review 6 — regulator-facing packaging.** For systems in scope of an external regime, confirm the regulator-facing artefacts (EU AI Act Article 11 technical documentation, FDA 510(k) submission, notified-body-facing conformity assessment file, SR 11-7 model documentation, ATRS record for UK public-sector deployments) are complete and filed with the compliance function. The gate does not decide launch on a system whose regulator-facing artefacts are not filed.

Each review has a *pass / conditional / block* outcome the reviewer records. The gate's overall decision is the join: all six pass → *approved*; any conditional → *conditional*; any block → *blocked*.

## Who signs — the signature block

The decision-record carries signatures with specific scope. A signature is not a courtesy — it is the assertion the signer is making under the enterprise's governance framework and under the applicable regulatory obligations.

**Required signatures for every tier.**

- **Second-line reviewer (chair of the gate, typically a designated senior seat within the governance office).** Asserts the evidence contract is discharged and the six reviews were conducted per the charter.
- **Evaluation engineer (peer, level 35).** Asserts the release-assurance methodology (chapter 06) was executed as designed and the eval-set findings are represented accurately in the decision.
- **Risk engineer (level 25).** Asserts the residual scoring is filed against the current taxonomy and contract version and the tolerance-table reading is correct.

**Additional signatures by tier.**

- **Tier-2.** Head of first-line function whose system is being deployed (typically head of engineering or head of product for the affected line).
- **Tier-3.** Head of AI governance (level 60) — the operator of the AIMS at the operational level signs a tier-3 launch personally.
- **Tier-4.** AI-accountable executive (top-management sponsor of the AIMS) — the AI-accountable executive signs a tier-4 launch personally; the audit committee is informed (not asked to sign) at the next scheduled meeting.
- **Regulator-facing systems.** General counsel or a designated legal seat asserts the regulator-facing packaging is complete and consistent with the enterprise's regulatory posture.

Signatures that are missing or non-original block the decision-record from becoming authoritative. Enterprises with digital signature infrastructure (Sigstore, a corporate PKI, an integrated GRC platform signing capability) use it; enterprises without run wet signatures on printed decision-records with the record filed in the AIMS documented-information register.

## The escalation paths — what opens when the gate cannot decide

The gate's authority is bounded. Four escalation paths open, each with a defined destination and reversal rules.

**Escalation 1 — residual outside tolerance, within stop-shipping threshold.** The gate escalates the decision to the head of AI governance (tier-3) or to the AI-accountable executive (tier-4). The escalation destination may *accept the residual* with an accepted-residual declaration filed in the risk register (per mod-106 chapter 03), *reject the launch*, or *return the decision to the gate with additional conditions* (typically additional controls to be implemented). The gate does not reconvene until the additional conditions are discharged.

**Escalation 2 — residual at or above stop-shipping threshold.** The gate escalates to the audit committee. Stop-shipping-threshold breaches are, by design of the appetite architecture (mod-106 chapter 03), the board's decision; the gate does not have authority to accept. If the audit committee returns "do not accept," the system does not launch until controls are added and the residual falls below threshold.

**Escalation 3 — evidence-contract discharge fails on an artefact the second line judges the first line cannot remediate.** The gate escalates to the head of AI governance, who either commissions remediation work with named owner and timing, or rejects the launch outright. This escalation typically fires when a first-line team wants to ship without the artefact and the second-line reviewer refuses; the escalation forces the accountability into the record.

**Escalation 4 — regulatory-posture ambiguity.** Where the applicable regulatory posture is unclear (a new jurisdiction, a novel use-case whose classification under EU AI Act is uncertain, a sector-specific regulator whose expectations are not yet published), the gate escalates to general counsel plus the AI-accountable executive. This escalation typically produces an interim decision (launch with additional monitoring; defer until the regulatory question is resolved; launch in a narrower scope) plus a filing to the risk register documenting the ambiguity.

Every escalation has a defined *time bound* — the escalation must resolve within a stated interval (typically 5 business days for escalation 1, 10 for escalation 2, 3 for escalation 3, 10 for escalation 4), or the escalation itself becomes a systemic finding for the CAPA process.

## The decision-record — the authoritative artefact

The gate's decision is recorded on a defined artefact. The artefact composes upward into the AIMS operations calendar (as a documented-information register entry) and downward into the ongoing assurance programme (as the reference point future re-assessments compare against).

**Decision-record shape.**

```yaml
decision_record:
  id: PDG-2026-04-0087
  system: cust-facing-chat-v2.3.1
  system_tier: 3
  scope_of_decision: >
    Approval to deploy cust-facing-chat-v2.3.1 into the North America
    consumer product surface (US + Canada), Colorado and NYC specific
    disclosures live, non-EU population only until AIA re-review for
    EU expansion at next Q gate.
  evidence_contract_discharge:
    base_bundle_complete: true
    tier_overlay_complete: true
    quality_gates:
      completeness: pass
      freshness: pass (last eval: 2026-04-11; window 30d for tier-3)
      traceability: pass
      reproducibility: pass (eval-set hash e3b0c...; seed 42)
      independence_of_authorship: pass
  reviews:
    - id: R1 (scope)
      outcome: pass
      reviewer: analyst-2
    - id: R2 (evidence)
      outcome: pass
      reviewer: evaluation-engineer
    - id: R3 (SoA)
      outcome: pass
      reviewer: analyst-1
    - id: R4 (residual)
      outcome: pass  # residual 84/125 on discriminatory-decision cat, in tolerance for tier-3 NA scope
      reviewer: risk-engineer
    - id: R5 (oversight & rollback)
      outcome: pass
      reviewer: analyst-2
    - id: R6 (regulator packaging)
      outcome: pass  # Colorado AI Act filings current; NYC LL144 bias audit filed
      reviewer: legal-seat
  overall_decision: approved
  conditions_attached: none
  signatures:
    - role: chair (gate)
      seat: gov-office-senior-1
      timestamp: 2026-04-14T14:22Z
    - role: evaluation-engineer
      seat: eval-eng-2
      timestamp: 2026-04-14T14:24Z
    - role: risk-engineer
      seat: risk-eng-1
      timestamp: 2026-04-14T14:25Z
    - role: head-of-ai-governance
      seat: head-gov
      timestamp: 2026-04-14T15:10Z
    - role: general-counsel-seat
      seat: legal-2
      timestamp: 2026-04-14T15:35Z
  cross_references:
    - risk_register_entries: [RR-2026-1442, RR-2026-1443, RR-2026-1444]
    - aia_id: AIA-2026-0187 v1.2
    - soa_version: 2026-Q1 v3
    - control_library_versions_referenced: 2026.03.15
  next_reassessment:
    scheduled: 2026-10-14 (per tier-3 semi-annual cadence, chapter 03)
    triggers_beyond_schedule:
      - material change: any Article 11 element change
      - drift: MTR fairness delta > 3 pp on protected class
      - incident: any tier-3 category incident on this system
      - regulatory: new state Act relevant to this scope
```

The decision-record is the artefact the certification body samples in chapter 05's stage-2 audit. It is the artefact the sector regulator's examiner asks for during an inquiry. It is the artefact the plaintiff's lawyer subpoenas in litigation. The architect designs the shape so that all three consumers find what they need without the enterprise having to hand-assemble an answer.

## Coordination with the level-35 evaluation engineer

The pre-deployment assurance gate is the primary venue in which the coordination between the level-50 architect (who designs the gate) and the peer-level-35 `ai-evaluation-engineer` (who executes the release-assurance methodology inside it) is exercised. Chapter 06 walks the contract in detail; the summary for this chapter is:

- The architect *designs the gate's shape* — the sequenced reviews, the evidence contract, the signature block, the escalation paths, the decision-record artefact.
- The evaluation engineer *designs the methodology that runs inside* the gate — the eval-set choice, the reproducibility discipline, the red-team engagement shape, the sampling of first-line evaluation runs, the delta-from-previous-release analysis.
- The two disciplines meet at the evidence contract's tier-3 and tier-4 overlays and at the sampling depth the second-line reviewer verifies against.

An architect who designs the gate without consulting the evaluation engineer produces a gate whose review 2 (evidence-contract discharge) is either too shallow (findings that pass the gate but should not) or too deep (findings that block launch on artefacts the enterprise cannot realistically produce). An evaluation engineer who designs the methodology without consulting the architect produces a methodology that is technically excellent but does not deliver a decision the gate can hang a signature on. The joint design is the mod-107 to `ai-evaluation-engineer` module interface.

## Two failure modes to design against

**Failure mode 1 — the gate that convenes without the evidence.** The first-line team pushes for launch; the second-line chair convenes the gate; the evidence contract has holes; the chair convenes anyway "because the launch is Thursday"; the missing artefacts are promised as "post-launch clean-up." This is the most common gate failure. The decision-record either omits the holes (fraudulent) or documents them (which the certification body and the plaintiff both find in the archive). The fix is architectural: the evidence-contract completeness check is a *pre-gate gate* — the gate does not appear on the calendar until the completeness check has passed. The seat that runs the completeness check is not the same seat that chairs the gate — separation prevents the "chair waives their own check" pathology.

**Failure mode 2 — the gate that rubber-stamps first-line evidence.** The gate convenes on schedule; every artefact is on file; the chair reads the summary sheet and signs. No sampling, no re-derivation, no challenge questions. The second-line signature carries no weight; the third-line internal audit engagement (chapter 04) will find that the gate had authority but no substance. The fix is the *sampling quota* — the gate charter specifies that the evaluation engineer will independently reproduce at least *k* percent of the tier-appropriate evaluation runs and challenge at least *m* claims from the model card per gate meeting. The quota is small enough to be executable in the gate's time budget and large enough to change the gate's culture from ratification to assurance.

## Summary

The pre-deployment assurance gate is the second-line-authoritative forum that decides whether a specific AI system may enter or return to deployment. Its evidence contract fixes what must exist before it can convene, sequenced across a base bundle and tier-specific overlays; its five quality gates prevent shallow reviews. The six sequenced reviews (scope, evidence discharge, SoA state, residual against appetite, oversight and rollback, regulator packaging) each carry a defined outcome; the join of the six is the gate's decision. Signatures are scope-bounded and role-anchored, with additional signatures required at higher tiers. Four escalation paths (residual within tolerance-band boundary, stop-shipping breach, evidence-contract failure the first line cannot remediate, regulatory-posture ambiguity) route out-of-gate decisions to the appropriate authority. The decision-record composes upward into the AIMS operations calendar and downward into the ongoing assurance programme. Coordination with the level-35 evaluation engineer, walked in chapter 06, is the primary interface at the tier-3 and tier-4 overlays. Two failure modes — convening without evidence, rubber-stamping evidence — are common and both architectural. The next chapter walks the ongoing assurance programme that keeps the gate's decision current after the launch.
