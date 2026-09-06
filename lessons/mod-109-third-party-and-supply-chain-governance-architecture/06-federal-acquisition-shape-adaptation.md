# Federal-acquisition shape adaptation — OMB M-24-18, M-25-22, and what the enterprise inherits

## Why this chapter exists

The US federal executive branch has, over the 2024–2025 memoranda cycle, published the most concrete AI-specific *procurement* shape a policymaker has yet published anywhere. Enterprises that sell to the US federal government inherit these obligations by operation of contract. Enterprises that do not sell to the federal government inherit the *shape* — the discipline the memoranda encode is what a well-designed enterprise AI procurement programme looks like, whether or not the enterprise is federally regulated.

The pair of memoranda the level-50 architect studies:

- **OMB M-24-18** (October 2024): *Advancing the Responsible Acquisition of Artificial Intelligence in Government.* Companion to the March 2024 M-24-10 governance memorandum. Sets the pre-contract, contract-formation, and post-award obligations federal agencies must impose when acquiring AI systems and AI-enabled services. <!-- needs-research: verify M-24-18 issuance date, section structure, and specific list of minimum practices against the OMB source document; the memorandum has an executive-branch designation and a specific issuance ID that should be confirmed. -->
- **OMB M-25-22** (April 2025): *Driving Efficient Acquisition of Artificial Intelligence in Government.* Successor memorandum issued under the incoming administration; revises the 2024 shape with an efficiency-and-innovation framing while retaining the risk-and-performance disciplines relevant to consequential AI. <!-- needs-research: verify M-25-22 issuance date, the specific revisions from M-24-18, and the list of minimum practices retained, added, or dropped against the OMB source document. Successor memoranda in a new administration can materially reshape prior obligations. -->

Both memoranda sit on top of the broader federal AI shape — the AI in Government Act of 2020, the Advancing American AI Act of 2022 (in the FY23 NDAA), NIST AI 100-1 (AI RMF), NIST AI 600-1 (Generative AI Profile), FAR reforms under way to codify AI-specific acquisition clauses, and the sectoral overlays (federal financial regulator guidance, HHS / VA / DoD AI-specific guidance). The memoranda are what turn "agencies should manage AI risk" into "agencies must include the following clauses when they buy AI-enabled products."

Enterprises that do not sell to federal customers may skip a specific FAR-clause discussion, but the memoranda's *shape* — the pre-solicitation risk assessment, the minimum acquisition practices, the contract-clause set, the vendor-representation regime, the delivery-and-acceptance discipline for AI outcomes, the post-award performance-monitoring shape, and the appeal / redress obligation — is the same shape the architect designs against for enterprise procurement in chapters 02 through 05. This chapter's contribution is to make the composition explicit: what the enterprise inherits from the federal shape, what it adapts, and what it deliberately does not carry.

Exercise-05 walks the drill.

## What "adaptation of the federal shape" is, structurally

Adaptation of the federal shape is the enterprise's *authored derivation* of an internal acquisition discipline from the federal memoranda's obligations. It consists of:

- **A mapping.** Each federal minimum-practice, contract-clause, and post-award obligation is mapped either to (i) an existing element of the enterprise programme (chapter 02 tiering, chapter 03 DDQ, chapter 04 contract-template controls, chapter 05 monitoring), (ii) a new element the enterprise adds, or (iii) an explicit non-adoption with rationale.
- **A rights-impacting / safety-impacting distinction.** The M-24-18 / M-24-10 distinction between *rights-impacting* and *safety-impacting* AI is inherited as an enterprise designation — the enterprise's tier-3 and tier-4 designations compose with this distinction rather than replacing it.
- **A minimum-practices catalog.** The federal minimum practices (pre-deployment testing, ongoing monitoring, human oversight for consequential decisions, notification to affected individuals, opt-out / appeal, plain-language documentation, and others) become enterprise-mandatory practices at the corresponding tier.
- **A vendor-representation regime.** Vendors are required to represent, at contract formation, the applicable AI-specific facts (whether the offering incorporates AI, whether it uses generative AI, whether the training data included copyrighted material without licence, the applicable model and version, the safety-evaluation posture, the supply-chain evidence posture). The enterprise's onboarding workflow ingests the representations as contract-schedule content.
- **A federal-customer schedule.** For enterprises that *do* sell to the federal government, a schedule composes onto the enterprise's own MSA / DPA / addendum to carry the FAR-flowdown obligations without polluting the enterprise-general shape.
- **A watchdog on federal-side change.** OMB memoranda revise on administration cycles; the enterprise's regulatory-tracking discipline (mod-104) monitors and re-derives the mapping.

## What it is not

- **A copy of the memoranda for internal use.** The memoranda are executive-branch instructions to federal agencies; they are not directly applicable to enterprise procurement. Copying the language is neither necessary nor sufficient — the enterprise has to *derive* the shape into its own control library.
- **FAR-clause drafting.** The FAR itself carries the enforceable acquisition clauses federal agencies use. When and how those clauses codify the M-24-18 / M-25-22 shape is a FAR Council process on its own cadence. <!-- needs-research: verify the current status of FAR AI-specific case openings, proposed rules, and interim rules at reading time. --> The enterprise reads the memoranda for shape; the enterprise's federal-facing legal counsel tracks the FAR proceedings.
- **A substitute for the enterprise's own risk-based approach.** Federal minimum practices are a floor; the enterprise's tier-4 posture (chapter 02) will often exceed the federal floor on specific dimensions. Adopting the federal floor as the enterprise ceiling is a compliance regression.
- **A cross-cutting compliance obligation for the enterprise.** Even where the enterprise sells to the federal government, only the specific contracts carry FAR-flowdown obligations. The enterprise does not become subject to the memoranda in the abstract.

## The federal-side context — what the memoranda actually do

At their core the memoranda impose an *acquisition discipline* on federal agencies buying AI, layered on top of the M-24-10 governance discipline on federal agencies *using* AI. The relevant obligations for the architect's adaptation are:

### Pre-solicitation — market research and requirements definition

Agencies must, before soliciting an AI-enabled product or service:

- Conduct AI-specific market research covering available capabilities, disclosed safety and performance evidence, vendor evaluation practices, and known risks.
- Identify whether the intended use is *rights-impacting* (affects individuals' rights, benefits, or access to essential services) or *safety-impacting* (affects physical safety or critical infrastructure), or both.
- Define required contract deliverables, evaluation criteria, and performance metrics against the intended use — not generic AI-capability descriptions.
- Define the human-oversight, notification, and redress requirements that the acquisition must support at delivery. <!-- needs-research: verify the pre-solicitation requirements against the M-24-18 / M-25-22 texts; the specific enumeration and any changes across the two memoranda should be confirmed. -->

### Solicitation — vendor-representation and evaluation-criteria requirements

Solicitations must:

- Require vendors to represent whether the offering incorporates AI (and, in some regimes, whether it uses generative AI), and to disclose supply-chain and training-data information sufficient to support the agency's diligence.
- Include AI-specific evaluation criteria — the vendor's safety-evaluation practices, red-team practices, incident-notification commitments, supply-chain evidence, and demonstrated performance on the agency's intended use.
- Include the applicable IP, data-use, and rights posture — the agency's rights in enterprise-generated inputs and vendor-generated outputs, the vendor's IP in the underlying model, and the composition of these against federal-data handling rules. <!-- needs-research: verify the solicitation-content requirements against the memoranda; specific list items may change between M-24-18 and M-25-22. -->

### Contract-formation — minimum acquisition practices and clauses

Contracts for AI-enabled products or services (with additional obligations for rights-impacting or safety-impacting AI) must include:

- **AI performance monitoring and testing rights.** The agency retains the right to test, evaluate, and monitor the AI's performance on an ongoing basis with the vendor's cooperation.
- **Incident-notification obligations.** Vendors commit to notify the agency of incidents affecting the AI's performance, safety, or security within defined timelines.
- **Supply-chain evidence and provenance.** Vendors provide evidence about the AI's training data, model provenance, and supply-chain integrity to the extent applicable to the offering.
- **Documentation.** Vendors deliver documentation sufficient for the agency to understand the AI's capabilities, limitations, and intended use.
- **Human-oversight and appeal support.** For rights-impacting or safety-impacting AI, the offering supports the agency's human-oversight, affected-individual notification, and appeal / redress obligations.
- **Data-use limits and IP posture.** Vendor's use of agency-provided data is bounded; the agency's data-rights posture on inputs, outputs, and derived artefacts is declared. <!-- needs-research: verify the contract-formation minimum practices against the specific memorandum text; the M-24-18 attachments and the M-25-22 revisions likely enumerate specific practices that should be confirmed. -->

### Post-award — performance monitoring and continuous risk management

After award:

- Agencies monitor the AI's performance against the acquired-performance metrics and the applicable minimum practices; performance shortfalls trigger contract-remedy processes.
- Vendors deliver periodic reporting on the AI's continued conformance with acquisition-time commitments.
- Waivers of specific minimum practices are permitted only through documented processes that carry rationale and expiry. <!-- needs-research: verify the post-award obligations and waiver-process shape against the memoranda; specific implementation guidance from OMB and agency-level policies fills in the detail. -->

### The rights-impacting vs safety-impacting distinction (M-24-10 heritage)

The M-24-10 governance memorandum introduced the *rights-impacting* and *safety-impacting* designations, and M-24-18 / M-25-22 inherit them for the acquisition context. Rights-impacting AI is broadly AI whose outputs meaningfully influence decisions about individuals' rights, benefits, access to services, or freedoms; safety-impacting AI is broadly AI whose outputs meaningfully influence physical safety or the reliability of critical infrastructure. Both designations carry additional minimum practices — expanded pre-deployment testing, ongoing monitoring, human-oversight, notification, and appeal obligations — beyond the baseline for AI generally. The M-24-10 attachments enumerate specific example use cases per designation. <!-- needs-research: verify the M-24-10 rights-impacting / safety-impacting definitions and example lists at authoring time; the M-25 revision may refine or extend. -->

## The enterprise adaptation — six mapping domains

The architect's adaptation of the federal shape lands as a mapping across six domains. Each domain identifies what the enterprise inherits from the federal shape, how it composes with the elements chapters 02–05 designed, and where the enterprise deliberately departs.

### Domain 1 — Rights-impacting / safety-impacting designation → tiering augmentation

The federal shape's rights-impacting / safety-impacting designation is inherited as an *enterprise designation field* on the vendor register, composed with the chapter 02 tier.

- Any vendor whose outputs enter a rights-impacting or safety-impacting use case for the enterprise inherits the designation regardless of tier.
- The designation is *additive* to the tier — a tier-3 vendor with a rights-impacting designation is subject to the tier-3 bundle *plus* the rights-impacting minimum practices.
- The designation composes with EU AI Act Article 6 / Annex III high-risk designations (a rights-impacting use is very often also an Annex III use); the enterprise carries the union of the two designations.
- The designation triggers automatic tier lift: a rights-impacting or safety-impacting use is at least tier-3 (auto-tiering rule A3 from chapter 02 already reaches most of this population, but the designation makes the trigger explicit against a widely-recognised US shape).

Adaptation move: extend the chapter 02 tiering worksheet with two boolean fields — `rights_impacting_designation` and `safety_impacting_designation` — with scoped definitions, and extend the chapter 04 contract-template control set with clauses that fire only when the designation is set.

### Domain 2 — Vendor-representation regime → DDQ front-matter

The federal shape's vendor-representation obligation (vendors must represent, at contract formation, that the offering incorporates AI, uses generative AI, was trained on specific data classes, has particular supply-chain provenance) maps to a *DDQ front-matter section* that every tier's questionnaire (chapter 03) opens with.

- The representation section is *tier-neutral* — every AI vendor across every tier answers it.
- The representation is a *contract schedule*, not a marketing statement: the vendor's answer at contract formation becomes a binding representation the enterprise can enforce against later change.
- The representation includes: AI-incorporation yes/no; generative-AI yes/no; model / component identity and version; training-data category disclosure; supply-chain evidence commitment; safety-evaluation commitment; incident-notification commitment; data-use commitment on enterprise inputs.
- Vendors that misrepresent — the offering that says "no AI" but incorporates a generative model in the backend — face contract-remedy paths and, where the enterprise's downstream customer is federal or regulated, potential cascading exposure.

Adaptation move: extend chapter 03's DDQ front-matter with a *federal-shape representation section*; extend chapter 04's contract-template controls with a *representation-and-warranty* clause that binds the DDQ answers as contract representations.

### Domain 3 — Minimum-practices catalog → enterprise mandatory-practice bundle

The federal minimum practices for AI generally, and the augmented practices for rights-impacting / safety-impacting AI specifically, map to an *enterprise mandatory-practice bundle* that the tier-appropriate onboarding workflow enforces.

Illustrative bundle (shape, not exhaustive):

```yaml
enterprise_mandatory_practices:
  version: 1.0.0
  derivation: adapted from OMB M-24-18 minimum practices and M-25-22 revisions <!-- needs-research: verify enumeration against source text -->

  # Applies to all AI vendor engagements (any tier, any designation)
  all_engagements:
    - MP-01: pre-deployment testing on the enterprise's intended use, with results captured as evidence artefact
    - MP-02: ongoing performance monitoring against defined metrics (mod-107 ch03)
    - MP-03: documentation sufficient for the enterprise to understand capabilities, limitations, intended use (DDQ + vendor-supplied cards, chapter 03 + chapter 07)
    - MP-04: incident-notification (chapter 04 IN family)
    - MP-05: data-use limits (chapter 04 DU family)

  # Additional practices for rights-impacting AI
  rights_impacting_additions:
    - MP-R-01: pre-deployment impact / risk assessment covering rights-impact analysis
    - MP-R-02: human-oversight capability that supports the enterprise's declared oversight discipline
    - MP-R-03: notification-to-affected-individuals capability (or the enterprise's own capability supported by vendor's identification of AI-generated content / decisions)
    - MP-R-04: appeal / redress capability (or the enterprise's own capability supported by vendor's decision-explanation, where feasible)
    - MP-R-05: monitoring for disparate-impact against declared protected classes; evidence retention for regulator-facing bias-audit
    - MP-R-06: plain-language notice to affected individuals that AI is used and how

  # Additional practices for safety-impacting AI
  safety_impacting_additions:
    - MP-S-01: pre-deployment safety testing including failure-mode analysis
    - MP-S-02: monitoring for safety-relevant performance degradation
    - MP-S-03: fallback / degraded-mode / manual-override capability
    - MP-S-04: incident-notification with hour-scale timelines for safety events (composes with EU AI Act Article 73 for reportable serious incidents in the EU)
    - MP-S-05: post-market surveillance shape (mod-110 composition)

  # Waiver policy
  waiver_policy:
    permitted_scope: individual practice per engagement, not the practice as a category
    authoriser: ai-accountable-executive (rights-impacting) OR head-of-ai-governance + accountable-executive joint (safety-impacting)
    expiry: 12 months maximum; renewal requires re-justification
    substrate: waiver record on vendor register + risk register entry (mod-106)
```

Adaptation move: publish the mandatory-practice bundle as an artefact of chapter 04's control set; every engagement's contract addendum incorporates the applicable bundle by reference; the vendor's DDQ answers document the vendor-side capability supporting each mandatory practice.

### Domain 4 — Contract-clause set → chapter 04 augmentations

The federal contract-clause obligations map onto chapter 04's control families with modest additions:

- Chapter 04 DU (data-use) family covers most of the federal data-use posture.
- Chapter 04 EA (evaluation-access) family covers the federal performance-monitoring / testing-rights posture.
- Chapter 04 IN (incident-notification) family covers the federal incident-notification posture with a rights-impacting / safety-impacting overlay that tightens timelines.
- Chapter 04 EV (evidence-access) family covers the federal documentation and supply-chain evidence obligations.
- Chapter 04 EX (exit) family covers most of the federal data-portability posture; extended-retention obligations that federal customers commonly impose (typically 3–7 years post-termination for the acquisition record) compose here.
- Chapter 04 OP (operational) family covers the federal material-change and subprocessor obligations.

*New additions* the federal shape motivates:

- **FED-01 — Representation-and-warranty schedule.** The vendor's DDQ front-matter representations bind as contract representations; misrepresentation triggers a defined remedy path.
- **FED-02 — Rights-impacting minimum-practices commitment.** For rights-impacting engagements, the vendor commits to supporting each MP-R practice in the enterprise's mandatory bundle; failure triggers contract-remedy.
- **FED-03 — Safety-impacting minimum-practices commitment.** Same for safety-impacting.
- **FED-04 — Federal-customer flowdown, where applicable.** For enterprises that resell to federal customers, the vendor accepts flowdown of the specific FAR clauses the enterprise's federal contract imposes; the schedule composes with vendor's own FedRAMP / StateRAMP / IL4-6 posture where applicable.
- **FED-05 — Waiver documentation and expiry.** Every accepted waiver of a mandatory practice or a federal-derived clause carries a substrate record with expiry.

Adaptation move: add these five controls to the chapter 04 catalog as the *federal-adaptation control family*.

### Domain 5 — Post-award performance monitoring → chapter 05 augmentations

The federal post-award monitoring shape adds three specifics to chapter 05's monitoring schedule:

- **Federally-mandated periodic reporting.** Where the enterprise or its customer is subject to the federal shape, the vendor delivers periodic reporting on the AI's continued conformance with acquisition-time commitments (typically quarterly for rights-impacting or safety-impacting, annually for others). The report becomes a substrate artefact under mod-108.
- **Performance-shortfall remedy path.** A performance shortfall detected under mod-107 ongoing assurance triggers, in addition to the enterprise's own risk-register process, a contract-remedy path where the vendor's performance commitment binds a specific SLA-like obligation. Chapter 04 OP-01 material-change and OP-05 indemnity compose.
- **Waiver-expiry sweep.** The register carries waiver-expiry dates; the operating rhythm queries for expiring waivers and re-opens either the waived practice or the waiver itself.

Adaptation move: extend chapter 05's operating-rhythm queries with the *federal-shape queries* — expiring waivers, overdue periodic reports, performance-shortfall remedies in flight.

### Domain 6 — Public / stakeholder-facing transparency → composition with the enterprise's transparency programme

The federal shape imposes public-transparency obligations on federal agencies (AI use-case inventories, notification to affected individuals, plain-language disclosures). Enterprises inherit the shape rather than the specific obligation: the enterprise's own transparency programme (which composes with mod-108 evidence architecture, and with sector-specific bias-audit publication regimes like NYC Local Law 144) carries the same shape.

Adaptation move: where the enterprise operates a public-facing AI use-case inventory, published model / system cards, or plain-language notices, the vendor's contribution to those artefacts is a chapter 03 DDQ field and a chapter 04 EA-04 evidence-access commitment.

## The federal-customer composition — for enterprises that sell to the US federal government

Enterprises that *sell* AI-enabled products or services to US federal customers carry additional obligations that flow *down* from the federal customer's FAR-implemented obligations. The programme composes with (not replaces) the enterprise's federal-facing schedule:

```yaml
federal_customer_composition:
  layers:
    - enterprise-general-msa
    - enterprise-general-dpa
    - enterprise-general-ai-governance-addendum (chapters 03-05)
    - federal-customer-schedule:
        content:
          - FAR AI-specific clauses that the federal customer's contract flows down
          - federal-agency-specific supplements (DFARS, VAAR, HHSAR, etc.) where applicable
          - FedRAMP / StateRAMP / IL2-6 authorisation posture, where the offering runs in a federal-cloud environment
          - specific rights-impacting / safety-impacting designations the federal customer has applied
          - specific minimum-practices bundle the federal customer requires
          - specific supply-chain-evidence obligations (SBOMs, ML-BOMs, SLSA levels)
        precedence:
          - federal-customer schedule overrides enterprise-general addendum on the specific federal engagement
          - enterprise-general terms apply where the schedule is silent
        signing_authority:
          - federal-contracts function (typically distinct from enterprise procurement) executes
          - AI governance architect ratifies the AI-specific overrides
        substrate:
          - federal-customer contracts are a distinct partition of the vendor register with additional query surfaces (contract-award / option-year windows, agency-specific reporting cadences, FAR-clause version tracking)
```

Enterprises that do not sell to federal customers omit the federal-customer schedule layer entirely; the general shape derived above is what the enterprise operates against.

## The watchdog on federal-side change

OMB memoranda evolve on administration cycles; FAR proceedings run on regulatory-process cycles; agency-level implementation guidance runs on agency cycles. The enterprise's mod-104 multi-jurisdictional reconciliation carries the tracking discipline; this chapter's contribution is to name the specific artefacts to watch:

- **OMB AI memoranda.** Track the currently-effective memorandum applicable to federal-customer contracts (M-24-10 governance, M-24-18 acquisition, M-25-22 acquisition-successor, and future revisions).
- **FAR Council proceedings on AI-specific cases.** Track proposed rules and interim rules that codify the memoranda into enforceable acquisition-regulation text.
- **NIST guidance updates.** AI 100-1, AI 600-1, and successor documents provide the technical shape the memoranda reference.
- **Agency-specific supplements.** DFARS AI clauses, VA AI clauses, HHS AI clauses, etc., as they land.
- **Standards updates.** ISO/IEC 42001, 42005, 42006, 5259 (data quality), and IEEE 7000-series compose with the federal shape where the memoranda reference standards.

Adaptation move: publish the watchdog as a mod-104 tracking artefact; re-derive the mapping on any material change; the register carries the mapping version each vendor engagement was signed against.

## A schematic of the federal-shape mapping

The mapping as an artefact the architect authors and defends against the AI-accountable executive:

```yaml
federal_shape_adaptation:
  version: 1.0.0
  owner: senior-ai-governance-architect (level 50)
  ratifies:
    - head-of-ai-governance (level 60) — operational
    - ai-accountable-executive — annually
    - general-counsel — for federal-customer composition
    - federal-contracts-lead — for federal-customer schedule composition

  memoranda_referenced:
    - OMB M-24-10 (2024-03) — AI governance for federal agencies
    - OMB M-24-18 (2024-10) — AI acquisition for federal agencies
    - OMB M-25-22 (2025-04) — AI acquisition efficiency successor
    - and successors — tracked by mod-104

  designation_scheme:
    rights_impacting: derived from M-24-10 definitions
    safety_impacting: derived from M-24-10 definitions
    composition_with_tier: additive (see domain 1)
    composition_with_eu_ai_act_high_risk: union (Article 6 / Annex III composed)

  vendor_representation_regime:
    front_matter_ddq_section: FED-REP
    binding_as_contract_representation: yes (FED-01 in chapter 04 catalog)

  mandatory_practices_bundle:
    all_engagements: [MP-01 .. MP-05]
    rights_impacting_additions: [MP-R-01 .. MP-R-06]
    safety_impacting_additions: [MP-S-01 .. MP-S-05]
    waiver_policy: named authoriser, 12-month expiry, substrate record

  contract_catalog_additions:
    - FED-01 representation-and-warranty
    - FED-02 rights-impacting-minimum-practices commitment
    - FED-03 safety-impacting-minimum-practices commitment
    - FED-04 federal-customer-flowdown (where applicable)
    - FED-05 waiver-documentation-and-expiry

  monitoring_augmentations:
    - federally-mandated periodic reporting cadence
    - performance-shortfall remedy path
    - waiver-expiry sweep in operating rhythm queries

  transparency_composition:
    - enterprise public use-case inventory, published cards, plain-language notices
    - vendor contribution is a DDQ field + EA-04 evidence-access commitment

  federal_customer_composition:
    - additional schedule layer for enterprises selling to federal customers
    - FedRAMP / StateRAMP / IL authorisation posture composition
    - agency-supplement composition (DFARS, VAAR, HHSAR, etc.)

  watchdog:
    - mod-104 tracking artefact monitors OMB, FAR, NIST, agency guidance
    - mapping version tracked per vendor engagement

  invariants:
    - id: I1
      description: rights-impacting and safety-impacting designations are queryable per vendor
    - id: I2
      description: representation-as-contract-schedule is present for every AI vendor
    - id: I3
      description: mandatory-practices bundle is enforced by tier + designation
    - id: I4
      description: waivers carry authorised expiry and are re-swept
    - id: I5
      description: federal-customer schedule is separated from enterprise-general terms
    - id: I6
      description: the mapping is versioned and re-derived on memorandum change
```

## The six invariants the adaptation holds

**Invariant 1 — rights-impacting and safety-impacting designations are queryable per vendor.** Every vendor has the two designation fields; the register carries them; the operating rhythm queries them. Failure mode: the enterprise adopts the language but not the field — a "rights-impacting" designation lives in a Slack thread the head of AI governance recalls; a subsequent audit asks "how many rights-impacting vendors do you have?" and no one can answer without a manual sweep.

**Invariant 2 — vendor representations bind as contract schedules.** The DDQ front-matter representation is a schedule of the executed contract; misrepresentation triggers a defined remedy path. Failure mode: the vendor's DDQ says "no generative AI in the offering"; six quarters later the enterprise discovers a generative-AI subcomponent has been added; the DDQ answer was informational only; the enterprise has no contract lever.

**Invariant 3 — mandatory-practices bundle is enforced by tier + designation.** The bundle attaches automatically; no engagement onboards without the tier-and-designation-appropriate practices in place. Failure mode: the bundle exists in the control library; the workflow does not enforce; specific engagements omit specific practices without a waiver.

**Invariant 4 — waivers carry authorised expiry and are re-swept.** Every waiver has a named authoriser, an expiry date, and a re-sweep in the operating rhythm; waivers do not accumulate silently. Failure mode: the enterprise accepts a waiver on MP-R-04 (appeal support) for a specific vendor "just for this engagement, we'll revisit"; the waiver has no expiry; three years later the accumulation is what surfaces at a regulator's engagement.

**Invariant 5 — federal-customer schedule is separated from enterprise-general terms.** The federal-customer obligations do not pollute the enterprise-general shape; other customers do not inherit federal-specific clauses. Failure mode: the enterprise's general addendum accretes federal-specific language over time; non-federal vendors push back at negotiation on clauses that do not apply to them; the addendum becomes negotiation-friction that harms onboarding cycle time.

**Invariant 6 — the mapping is versioned and re-derived on memorandum change.** Every vendor engagement is signed against a specific mapping version; a memorandum revision triggers a re-derivation and a decision (migrate on renewal, migrate immediately, or defer with rationale). Failure mode: the memoranda evolved through two revisions; the enterprise's addendum still references the 2024 shape; renewals inherit the stale mapping.

## Two failure modes to design against

**Failure mode 1 — copying the memoranda into an internal document without derivation.** The enterprise's compliance function reads M-24-18, extracts the minimum-practices list into a Word document, calls it "our AI acquisition policy," and files it. Nothing downstream changes. The tiering scheme (chapter 02) still runs without a rights-impacting field; the DDQ (chapter 03) still runs without a representation front-matter; the contract catalog (chapter 04) still lacks the federal-adaptation family; the monitoring schedule (chapter 05) still lacks the waiver sweep. The "policy" exists; the *programme* does not. The fix is architectural: derivation is a specific engineering activity — mapping each obligation to an existing element, a new element, or an explicit non-adoption; the derivation ships as edits to chapters 02–05 artefacts, not as a parallel Word file.

**Failure mode 2 — treating a memorandum revision as a document update.** M-24-18 is superseded by M-25-22; the enterprise's compliance team updates the reference in the "AI acquisition policy" document; nothing else changes. But M-25-22 modified specific minimum practices (loosened some, added others); waivers granted under the M-24-18 shape now sit against a different mandatory-practice bundle; some clauses in the enterprise's addendum reference the earlier memorandum's specific language; the enterprise's federal customers' flowdown clauses shift. The fix is architectural: memorandum revisions trigger the *re-derivation* — the mapping is re-authored; each vendor engagement's substrate binding is checked; renewals plan the migration; the substrate carries the mapping version so an auditor can trace which vendors are on which mapping.

## Summary

The US federal executive-branch memoranda (M-24-18, M-25-22, and their governance ancestor M-24-10) are the concrete AI-procurement shape enterprises inherit whether or not they sell to federal customers. The adaptation is a mapping across six domains: rights-impacting / safety-impacting designation (composed with chapter 02 tiering), vendor-representation regime (added as DDQ front-matter and chapter 04 FED-01 clause), mandatory-practices bundle (published as chapter 04 catalog artefact and enforced at onboarding), contract-clause augmentations (chapter 04 FED-01 through FED-05), post-award monitoring augmentations (chapter 05 operating-rhythm additions), and transparency composition (with the enterprise's own transparency programme). Enterprises that sell to federal customers carry an additional federal-customer schedule layer without polluting the enterprise-general terms. A watchdog under mod-104 tracks memorandum, FAR, NIST, and agency-guidance evolution; the mapping is versioned and re-derived on change. Six invariants (designations queryable, representations bind, practices enforced, waivers expire, federal schedule separated, mapping versioned) and two failure modes (copy-not-derive, revision-as-document-update) shape the discipline. Exercise-05 walks the drill. The next chapter designs the supply-chain evidence contract that gates ingestion of every third-party AI artefact.
