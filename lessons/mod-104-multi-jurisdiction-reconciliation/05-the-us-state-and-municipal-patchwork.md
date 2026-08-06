# The US state and municipal patchwork

## Why this chapter exists

The United States regulates AI vertically at the federal level (chapter `04-the-us-federal-frame.md` — sector regulators plus executive-order-derived guidance) and horizontally at the *state* level, where a patchwork of statutes, city ordinances, and privacy-agency regulations has emerged since 2022 and continues to grow. The patchwork is not converging on a common shape. Colorado uses "algorithmic discrimination" and *consequential decision*. New York City uses *automated employment decision tool*. California CPPA uses *automated decisionmaking technology* under CCPA/CPRA. Utah frames obligations through consumer protection and regulated-professions channels. Texas uses TRAIGA and its own definitions. Illinois uses amendments to its existing Human Rights Act. Each of these is a distinct trigger, distinct scope, distinct evidence expectation.

The level-50 architect's job with the state patchwork is to (a) know the enumerated set of state / municipal regimes the enterprise is subject to (a moving target), (b) design the applicability filter so a single AI system, deployed in multiple states, discharges each state's obligations without duplicated controls, and (c) build an *incoming-legislation watch* so the architecture is not surprised by Virginia's, Connecticut's, or Washington's next move. This chapter walks the regimes the objectives name, pins each to its addressee / trigger / demand / consequence, and lands them in the reconciliation architecture.

## Colorado AI Act (SB24-205)

Signed 17 May 2024, effective 1 February 2026. First comprehensive state horizontal AI law. Targets *algorithmic discrimination* by *high-risk artificial intelligence systems* in *consequential decisions*.

**Key definitions.** *High-risk AI system* — a system that, when deployed, makes or is a substantial factor in making a *consequential decision*. *Consequential decision* — a decision that has a material legal or similarly significant effect on the provision or denial to any consumer of, or the cost or terms of, education, employment, financial or lending services, essential government services, healthcare services, housing, insurance, or legal services. *Algorithmic discrimination* — unlawful differential treatment or impact that disfavours an individual or group based on a protected class.

**Developer duties.** Reasonable care to protect consumers from any known or reasonably foreseeable risks of algorithmic discrimination. Provide a statement to deployers describing the system's intended uses, known limitations, purpose, benefits, and how the developer evaluated performance and mitigated risks. Public statement summarising types of high-risk systems developed and how discrimination risks are managed. Disclose to the attorney general any known or reasonably foreseeable risk of algorithmic discrimination within specified windows.

**Deployer duties.** Reasonable care in the same shape. Implement a risk-management policy and program. Conduct annual impact assessments (and after intentional and substantial modifications). Notify consumers when a high-risk system is used to make a consequential decision concerning them, and provide certain disclosures. Provide affected consumers an opportunity to correct incorrect personal data used to make the decision, and to appeal an adverse decision (with review by a human where technically feasible). Public statement summarising deployed systems.

**Enforcement.** Colorado attorney general exclusive authority; no private right of action. Sixty-day right to cure. Rebuttable presumption of reasonable care where the developer/deployer conforms to specified frameworks (NIST AI RMF and comparable) and specified conditions.

**Architectural takeaway — Colorado is where the reconciliation architecture pays for itself.** Colorado's obligation set overlaps heavily with EU AI Act (risk management, impact assessment, transparency, human oversight for adverse decisions), NYC LL 144 (impact assessment analog for employment), and NIST AI RMF (framework-alignment rebuttable presumption). The library carries the underlying controls once; Colorado attaches as:

1. A new applicability-filter attribute: `us_state.colorado_sb24_205_applies` — true when the system is (deployer scope) making or is a substantial factor in a consequential decision affecting a Colorado consumer.
2. Crosswalk edges from Colorado sections to existing risk-management, impact-assessment, transparency, consumer-notice, appeal-channel, and human-review controls (shape A dominant).
3. A small number of shape-B controls specific to Colorado: the *attorney general notification of known algorithmic discrimination* (60/90-day windows, specific recipient) and the *public statement* obligation whose evidence is a published web page and a change history.

## New York City Local Law 144

Effective July 2023 (with enforcement commenced 5 July 2023 after DCWP finalised rules). Regulates *automated employment decision tools* (AEDTs) used by employers and employment agencies in the city.

**Key requirements.** An AEDT that has been subject to a *bias audit* within the past year is a precondition for use to substantially assist or replace discretionary decision-making in employment decisions. The bias audit is conducted by an *independent auditor* and computes specified impact-ratio metrics across race/ethnicity and sex categories, and intersectional categories where sufficient data. A *summary of the results* of the most recent audit is publicly published. Candidates or employees who reside in the city are notified at least ten business days before the AEDT is used, that an AEDT will be used, and given information about the categories of characteristics evaluated and the type of data collected and its source.

**Enforcement.** DCWP; civil penalties per violation, per day.

**Architectural takeaway.** LL 144 is narrow (employment only, city only) but *sharp* (specific metrics, specific auditor independence, specific notice timing). The reconciliation architecture:

1. An applicability-filter attribute for `nyc_ll_144_applies` — true for AEDTs used in NYC employment scope.
2. A shape-B control for the *bias-audit-by-independent-auditor* obligation, whose evidence is the auditor engagement letter, the audit report, the impact-ratio computations, and the annual re-audit trigger.
3. A shape-A extension of a candidate-notice control, if the enterprise operates one for other jurisdictions; otherwise shape-B.
4. A shape-B control for the *publicly published summary*, whose evidence is the URL, the publication date, and the retention of prior summaries for the specified period.

Where the enterprise also operates under Colorado SB24-205 or Illinois HB 3773 employment provisions, most of the underlying bias-monitoring practice is common; LL 144's independent-auditor-plus-published-summary shape is genuinely novel and does warrant the shape-B control.

## EEOC AI guidance

*Assessing Adverse Impact in Software, Algorithms, and Artificial Intelligence Used in Employment Selection Procedures* — technical assistance issued 18 May 2023 by the EEOC. Applies Title VII's adverse-impact framework to AI-driven employment selection tools, using the Uniform Guidelines on Employee Selection Procedures (UGESP) "four-fifths rule" as a rebuttable trigger for further analysis. Also *The Americans with Disabilities Act and the Use of Software, Algorithms, and Artificial Intelligence to Assess Job Applicants and Employees* — technical assistance issued 12 May 2022 addressing ADA obligations.

<!-- needs-research: check whether the EEOC's AI-focused technical-assistance webpages remain the primary references, or whether they have been consolidated into a newer publication under the current EEOC posture. -->

**Architectural takeaway.** EEOC guidance is federal, not state, but it sits *with* the state employment regimes because its enforcement authority runs alongside them and its adverse-impact analysis is what the state statutes implicitly reference. The reconciliation architecture treats EEOC guidance as:

1. A crosswalk-edge source on the same adverse-impact-analysis and reasonable-accommodation controls that carry LL 144 / Illinois HB 3773 / Colorado employment scope.
2. A methodology reference in the evidence contract on the adverse-impact control (four-fifths rule as the presumptive trigger, with documentation of the methodology used where the enterprise deviates).

No shape-B controls typically. But the *reasonable-accommodation-during-assessment* control (ADA) is often overlooked in AI-hiring-tool implementations and is a common finding — call it out as its own line item in the evidence contract rather than folding it into general accessibility.

## California — CPPA ADMT, SB 942, AB 2013

Three distinct instruments, all reaching AI, all with different addressees.

**California Privacy Protection Agency — Automated Decisionmaking Technology (ADMT) regulations.** Rulemaking under CCPA/CPRA authority. As of the current-cycle status, the CPPA has adopted regulations governing (a) pre-use notice to consumers when a business uses ADMT for a *significant decision*, (b) consumer rights to opt out of ADMT for certain purposes and to access information about ADMT used, and (c) risk-assessment requirements for certain uses of ADMT. Enforcement by the CPPA under CCPA/CPRA authorities.

<!-- needs-research: verify the current adoption / effective-date status of the CPPA ADMT regulations, the exact definition of "significant decision" as adopted, the enumerated scope of risk-assessment triggers, and whether the training-of-ADMT provisions from earlier drafts were retained in the adopted rule. -->

**California SB 942 — the California AI Transparency Act.** Signed 19 September 2024, effective 1 January 2026. Applies to *covered providers* of *generative AI systems* with monthly users above a specified threshold. Requires the provider to make available a *free AI detection tool* that determines whether image, video, or audio content was created or altered by the provider's system, and to include a *manifest disclosure* (a visible disclosure indicating content is AI-generated) in outputs at the user's request, plus a *latent disclosure* (a persistent, machine-readable disclosure) in outputs. Enforcement by the California attorney general.

**California AB 2013 — Generative AI: training data transparency.** Signed 28 September 2024, effective 1 January 2026 (for GenAI systems made available on or after 1 January 2022). Requires developers of GenAI systems made available to Californians to *post on the developer's internet website* a summary of the datasets used in the system's training, following an enumerated set of content requirements (dataset sources, purpose, whether they include personal information, whether they include copyrighted material, ...).

**Architectural takeaway.** Three California instruments, three different targets:

- **CPPA ADMT** targets *businesses subject to CCPA* that use ADMT for significant decisions — this is a *deployer-facing* regulation, using GDPR-like vocabulary. Shape A for the risk-assessment obligation (adds to the enterprise's DPIA / FRIA suite); shape A for the pre-use notice (adds to the transparency-to-consumer control); shape B for the opt-out mechanism (specific opt-out shape, specific handling window). Applicability-filter attribute `us_state.california_cppa_admt_applies`.
- **SB 942** targets *large GenAI providers*. Shape B for the *AI detection tool* — no prior enterprise control carried this specific obligation. Shape A for the *latent disclosure* extension of the Article-50 category-2 machine-readable marking control (chapter `02-reading-the-eu-ai-act-as-architectural-input.md`); the two obligations use different technical mechanisms but the same evidence pattern (code-path snapshot plus output-inspection sample).
- **AB 2013** targets *GenAI developers*. Shape A for the *training-content summary* control — the EU AI Act Article 53(1)(d) training-content summary obligation and California AB 2013 map onto the same underlying enterprise practice, with two different published-artefact renderings. One control, two applicability-filter attributes, one evidence contract that covers both artefact renderings.

Where the enterprise is both a business subject to CCPA and a GenAI provider, all three attach.

## Utah SB 149 — Artificial Intelligence Policy Act

Signed 13 March 2024, effective 1 May 2024, with amendments in 2025. Establishes an *Office of Artificial Intelligence Policy* and *Artificial Intelligence Learning Laboratory Program* (a regulatory-sandbox mechanism), and imposes disclosure obligations on certain uses of GenAI. Specifically: a person who uses a GenAI system in interactions in a *regulated occupation* must proactively disclose to the individual that they are interacting with GenAI; in *consumer transactions*, disclosure is required in response to an inquiry.

<!-- needs-research: verify the 2025 amendments to Utah SB 149 (SB 226 or successor) and any adjustment to the regulated-occupation vs consumer-transaction disclosure rules. -->

**Architectural takeaway.** Utah is narrow — a disclosure obligation on GenAI use in specific contexts, plus a sandbox mechanism the level-50 architect may want to use. Shape A on the enterprise's disclosure-to-user control (extended applicability-filter attribute `us_state.utah_sb149_applies`, extended disclosure-modality enumeration to include "proactive" vs "reactive" per Utah's regulated-occupation vs consumer-transaction distinction). No shape B typically. The sandbox mechanism is a *strategy* input rather than a control — where the enterprise wants to pilot a novel use, the sandbox may make Utah the pilot jurisdiction of choice, and the reconciliation architecture should note the sandbox as an applicability *modifier* rather than as an obligation.

## Texas HB 149 — Texas Responsible AI Governance Act (TRAIGA)

Signed 22 June 2025, effective 1 January 2026. Prohibits certain uses of AI (social scoring by government; systems intentionally developed or deployed to promote self-harm or violence; certain intentional discrimination scenarios), requires developer/deployer disclosures and impact assessments for certain classes of AI, establishes an AI *regulatory sandbox* administered by the Texas Department of Information Resources, and provides for AG enforcement.

<!-- needs-research: pin the exact signed date, effective date, and the final scope of TRAIGA's prohibitions and duties. Between the House-Senate versions and the signed instrument, several provisions changed; the chapter's characterisation should be verified against the enrolled bill and any subsequent regulations. -->

**Architectural takeaway.** TRAIGA is broader than Utah, narrower than Colorado. Shape A for the impact-assessment and disclosure controls (crosswalk edges to the enterprise's existing FRIA / DPIA / consumer-transparency controls). Shape B for the *AG notification of prohibited-use discovery* if the enterprise's product could be misused into a prohibited category — the notification shape has no clean pre-existing analogue. Applicability-filter attribute `us_state.texas_traiga_applies`. The sandbox, like Utah's, is a strategy input.

## Illinois HB 3773 — Human Rights Act amendments

Signed 9 August 2024, effective 1 January 2026. Amends the Illinois Human Rights Act (IHRA) to add specific provisions covering employer use of AI in employment decisions. Prohibits an employer from using AI that subjects employees to discrimination on the basis of protected classes with respect to recruitment, hiring, promotion, and related employment decisions. Requires notice to employees when AI is used for these purposes. Enforced by the Illinois Department of Human Rights.

**Architectural takeaway.** Illinois is the second employment-AI regime after LL 144 that carries teeth. Shape A on the underlying adverse-impact-monitoring and notice-to-candidate/employee controls; the LL 144 controls already exist and Illinois attaches as a crosswalk edge and an applicability-filter extension. Note that Illinois also carries the *Biometric Information Privacy Act* (BIPA), which has a *private right of action* and has produced substantial enforcement — if the AI system uses biometric data (facial recognition, voiceprint, keystroke dynamics), BIPA attaches independently and is a very high-consequence obligation.

## Emerging state activity — Virginia, Connecticut, Washington

**Virginia — HB 2094 / consumer-protection AI amendments.** Virginia has moved variously on AI legislation, with proposed bills covering algorithmic-discrimination duties in shapes similar to Colorado. Status varies by session. <!-- needs-research: current status of Virginia HB 2094 or its successor bill; whether it was signed, vetoed, or carried over. -->

**Connecticut — SB 2 and successors.** Connecticut has considered comprehensive AI-governance legislation with algorithmic-discrimination and impact-assessment provisions in a Colorado-like shape. Status varies by session. <!-- needs-research: current status of Connecticut SB 2 or its successor bill. -->

**Washington — HB 1951 and related.** Washington State has moved variously on AI-in-employment and AI-governance bills. <!-- needs-research: current status of Washington HB 1951 or its successor. -->

**Architectural takeaway.** These three states are on the *watch list*, not the *active list*. The reconciliation architecture should carry a formal watch-list mechanism (chapter `07-designing-the-reconciliation-architecture.md`) that records:

1. The bill number and session.
2. The obligation shape (Colorado-analog, LL 144-analog, novel).
3. The trigger event that would move it from watch to active (signature into law; effective date).
4. The pre-work the architect should do *before* the trigger — usually, identifying which applicability-filter attribute and which crosswalk edges will attach, so post-signature implementation is filter-and-crosswalk rather than a control-authoring sprint.

## Designing the multi-state overlay

For an enterprise operating in more than three or four states, the overlay design is more consequential than any individual state's compliance work. Four design decisions the architect makes once:

**Decision 1 — Jurisdiction enumeration and hierarchy.** The applicability-filter jurisdiction attribute needs a controlled vocabulary. `us_state.colorado`, `us_state.california`, `us_municipality.new_york_city`, `us_state.illinois`, and so on. Municipality is one level below state; a system operating in Los Angeles is subject to California-state regimes but not to NYC LL 144. The taxonomy also needs *federal* as a distinct top-level for OMB-flowed obligations (chapter `04-the-us-federal-frame.md`).

**Decision 2 — Overlap resolution when two regimes attach to the same practice.** Where Colorado and California ADMT both require a risk assessment, the enterprise runs *one* risk assessment against *both* templates, or *one* assessment against a *superset* template and produces two renderings. The reconciliation architecture picks the superset-with-renderings approach — one artefact, two evidence renderings; the underlying practice is common. Only where the two regimes demand *different content* (Colorado's algorithmic-discrimination framing vs California's ADMT-privacy framing) does the artefact need distinct sections.

**Decision 3 — Effective-date staging.** The state regimes come into force on different dates. The applicability filter must carry an *effective-date* attribute per jurisdiction; controls are inactive against that jurisdiction until the date is reached. Auditors do not credit controls prospectively.

**Decision 4 — Right of action and consequence.** State regimes vary sharply on private right of action. Illinois BIPA (private right, statutory damages) is the outlier that has produced the largest settlements in AI-adjacent enforcement. Colorado (AG-only) and NYC LL 144 (regulator penalty) carry different consequence bands. The obligation record carries the consequence type; risk appetite (mod-106) drives the tolerance for reduced-form controls; the architect does not decide risk appetite unilaterally but must expose it in the record so the level-60 head-of-AI-governance and the AI committee can.

## Two failure modes

**Failure mode 1 — one control per state.** The library grows one control per state per obligation. Colorado risk assessment, California risk assessment, Texas risk assessment. Within four states, the risk-assessment family has 18 controls with identical statements and slightly different applicability filters. The fix is shape A by default — one risk-assessment control with a multi-jurisdictional applicability filter and a per-jurisdiction evidence rendering.

**Failure mode 2 — treating watch-list states as noise.** The enterprise ignores Virginia / Connecticut / Washington until a bill is signed, then spends a quarter authoring controls. The fix is the pre-signature pre-work: map the bill's obligation shape onto the applicability-filter and crosswalk you *would* use, so signature-to-effective becomes a filter update, not a rewrite.

## Summary

The US state and municipal patchwork is Colorado (comprehensive, algorithmic-discrimination framing, AG enforcement), NYC LL 144 (employment-only, independent auditor plus published summary), EEOC guidance (federal but sits with state employment regimes), California (three distinct instruments — CPPA ADMT for CCPA-scope deployers, SB 942 for large GenAI providers, AB 2013 for GenAI developers), Utah (narrow disclosure plus regulatory sandbox), Texas TRAIGA (prohibitions plus impact assessments plus sandbox), Illinois HB 3773 (employment-scope IHRA amendments, backed by BIPA where biometric data is involved), and a watch list of Virginia / Connecticut / Washington activity. The reconciliation architecture handles the patchwork by treating each regime as an applicability-filter attribute and a crosswalk-edge source on existing controls (shape A dominant), spending its shape-B budget on the genuinely novel obligations (LL 144's independent-auditor requirement; SB 942's AI-detection tool; the Colorado AG-notification obligation), designing the jurisdiction taxonomy at library-preface time so state and municipality nest correctly, and running a formal watch-list workflow so no state moves from bill to law without pre-work already in place. Get this right and adding a tenth state is a week's work; get it wrong and it is a quarter's.
