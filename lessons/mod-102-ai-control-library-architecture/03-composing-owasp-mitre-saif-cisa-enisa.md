# Composing OWASP LLM Top 10, MITRE ATLAS, Google SAIF, CISA/NCSC, and ENISA

## Why this chapter exists

Chapter 02 composed the three *governance-family* sources — NIST AI RMF, ISO/IEC 42001 Annex A, and EU AI Act Articles 9–15 — into the control-library shape. Those sources say *what outcomes the organization commits to*. They do not tell you *what adversaries do* to the AI system, nor *what engineering practice* is needed to withstand them.

That work is done by a second, parallel corpus:

- OWASP Top 10 for LLM Applications — the community-maintained threat list for LLM-shaped systems.
- MITRE ATLAS — the adversarial tactics / techniques knowledge base for ML.
- Google Secure AI Framework (SAIF) — a defender-side reference framework organised by risk area.
- CISA / NCSC "Guidelines for Secure AI System Development" — the joint agency guidance for provider-side secure development.
- ENISA "Multilayer Framework for Good Cybersecurity Practices for AI" — the EU agency reference for layered AI cybersecurity practices.

The architect's job is not to pick one of these as canonical. It is to compose them into the *same* library the governance-family sources already populated, so that (a) engineering-side controls are grounded in real adversary behaviour, (b) each threat and each recommendation traces to at least one library entry, and (c) audit and pen-test evidence dereferences into the same catalog the AIMS uses. This chapter shows how.

## The five sources — grain and speech act

Each source has a distinct grain and speaks a distinct dialect. Read the table as a lens map before doing any composition:

| Source | Grain | Speech act | Primary audience |
|---|---|---|---|
| OWASP LLM Top 10 | Named risk categories (LLM01…LLM10) for LLM apps | *Risk description + example attack + prevention*. | AppSec, LLM app teams. |
| MITRE ATLAS | Tactics, techniques, procedures, mitigations, case studies for ML | *Adversary behaviour catalog* — "adversary does X to achieve Y." | Red team, threat intel, detection engineering. |
| Google SAIF | Risk areas + mitigations organised around the AI development lifecycle | *Defender reference architecture* — "to address risk X, apply mitigation set Y." | Security architects, platform teams. |
| CISA / NCSC Secure AI SDLC | Provider-side guideline broken into design / development / deployment / operation & maintenance | *Recommendation* — "you should…". | Providers of AI systems, national infrastructure. |
| ENISA multilayer framework | Layered practice model (organisational, technical, procedural) | *Layered practice* — "at layer X apply practices A, B, C." | EU-facing operators, especially critical sectors. |

Every one of these speaks in its own genre and will be cited verbatim by different audiences. Never paraphrase them into a single house dialect and drop the originals — you lose your evidence trail the moment an auditor or a red-team lead asks for the source.

## The composition move: threat-side entries reuse governance-side entries where they overlap, add fresh entries where they do not

The governance-family composition move from chapter 02 was *one entry per outcome, multi-parented via crosswalk*. The threat-family move is a superset:

1. For every threat / recommendation, ask: does an existing governance-family entry already deliver the outcome that would mitigate this threat?
2. If yes, extend that entry's `crosswalk` to name the threat source; enrich `implementation_guidance` and `testing_procedure` with the engineering practice the threat source implies; do *not* mint a fresh entry.
3. If no, mint a new entry — usually in a new *engineering* family (`AIC-ENG-*`, `AIC-SEC-*`, `AIC-INF-*`) — whose statement expresses the defensive outcome and whose crosswalks reach the threat source(s) plus any governance source that agrees.

Case one is the dominant pattern for LLM03 (Training Data Poisoning), which reuses the data-provenance entry from chapter 01 and layers integrity-attestation guidance on top. Case two is the dominant pattern for LLM01 (Prompt Injection), which typically demands a new entry in the runtime-guardrails family because none of the governance-family sources speak concretely enough about prompt-injection defence to pre-populate one.

The failure mode this move rules out: standing up a *parallel* threat-side catalog (`SEC-AI-*`) that duplicates governance-side controls under new IDs. Two catalogs, two SoAs, two audit-trace maps, twice the drift.

## Reading OWASP LLM Top 10 into control entries

The OWASP Top 10 for LLM Applications enumerates the highest-impact risk categories for applications that consume LLMs — currently prompt injection, sensitive information disclosure, supply chain, data and model poisoning, improper output handling, excessive agency, system-prompt leakage, vector / embedding weaknesses, misinformation, and unbounded consumption. The list is community-maintained and versioned; pin your crosswalk to a specific list version, do not to "the top 10" in the abstract, because entries and IDs move between releases.

Every entry in the list is essentially a shape: `risk name → example attack scenarios → prevention / mitigation strategies`. The composition procedure:

1. **Map to existing entries.** For each LLM-NN, list every governance-family entry whose statement, if realised, would materially reduce that risk. LLM02 (Sensitive Information Disclosure) maps onto every data-classification-and-egress entry you already own. LLM03 / LLM04 (Data / Model Poisoning) maps onto data-provenance + integrity-hash entries. LLM10 (Unbounded Consumption) maps onto rate-limit / quota entries in the platform library.
2. **Add threat crosswalk.** In each matched entry's crosswalk, add an `owasp_llm_top_10: [LLM02, LLM10]` line pinning the exact list version. Now the SoA and the pen-test brief share a lookup key.
3. **Extract prevention items.** For each unmapped prevention item, either add it as `implementation_guidance` to an existing entry (if it is a *how*) or open a new entry (if it is a distinct outcome the org has not yet committed to). Do *not* copy OWASP prevention lists verbatim into `implementation_guidance` — reference them.
4. **Mint gap entries.** LLM01 (Prompt Injection) and LLM07 (System Prompt Leakage) usually have no governance-family parent that lands specifically enough. Mint `AIC-ENG-*` entries whose statements express the defensive outcome ("the system refuses tool-use instructions delivered via untrusted content") and whose crosswalks name both OWASP and the closest governance-family parent (usually NIST AI RMF MEASURE-2.7 for TEVV).

The by-product of this pass is a *coverage map* — LLM-NN → set of AIC entries covering it — that the pen-test team consumes as its baseline: any LLM-NN with an empty coverage set is either an out-of-scope risk (documented) or a library gap (backlogged).

## Reading MITRE ATLAS into control entries

ATLAS is structured like ATT&CK — tactics down the top, techniques inside each tactic, sub-techniques inside those, mitigations linking across — plus a growing case-study corpus of real-world ML incidents. Its speech act is "adversary does X to achieve Y"; its consumers are red team, threat intel, detection engineering, and the ai-risk-engineer at level 25.

Two compositions matter for the control library:

**Mitigation → control entry.** ATLAS attaches mitigations to techniques (e.g., "Restrict Library Loading," "Model Hardening," "Input Restoration"). Each mitigation is a candidate for a control-library entry. For the ones your organisation adopts, add a crosswalk field:

```yaml
crosswalk:
  mitre_atlas_mitigations: [AML.M0000, AML.M00nn]  # <!-- needs-research: confirm current mitigation IDs against the live ATLAS matrix at the version pinned by the library preface -->
```

Do not mint one control per mitigation. Multiple ATLAS mitigations often collapse onto the same enterprise outcome (e.g., several supply-chain mitigations collapse onto your ML-BOM + Sigstore attestation control). Multi-parent the crosswalk instead.

**Technique → testing procedure and evidence.** ATLAS techniques (e.g., `AML.T0043 Craft Adversarial Data`, `AML.T0051 LLM Prompt Injection`) directly drive testing procedures on the corresponding defensive control. The convention: the *statement* remains outcome-oriented ("the system's output pipeline does not execute instructions delivered via input content"); the *testing procedure* enumerates the ATLAS techniques the red team must attempt, at what cadence, with what pass/fail bar. This preserves the separation between "what we commit to" and "what we try to break."

The evidence contract for red-team-facing controls is where the AIID / OECD.AI / MIT AI Risk Repository incident corpora (mod-106, mod-110) plug in as *external calibration* references — your test-scenario library samples from real incidents, not only from ATLAS techniques.

## Reading Google SAIF into control entries

SAIF is organised as *risk areas* (data poisoning, unauthorized training data, model source tampering, model exfiltration, model deployment tampering, denial of ML service, model reverse engineering, insecure integrated component, prompt injection, model evasion, sensitive data disclosure, inferred sensitive data, insecure model output, rogue actions) with each risk area proposing a mitigation set. The SAIF risk map is designed to be dereferenced against the AI development lifecycle: data → model → application → infrastructure → assurance → governance.

The composition move: SAIF risk areas are *coarser* than OWASP items and MITRE techniques, so they usually cover several library entries at once. The right posture is:

- Treat each SAIF risk area as a *family header* under which multiple library entries sit.
- In each library entry, add `crosswalk: google_saif: [<risk-area-name>]` — the framework is not versioned with dense IDs, so name the risk area explicitly, and pin to a document version in the library preface.
- Use SAIF's mitigations as prompts when authoring the `implementation_guidance` field; do not copy them verbatim.
- Use SAIF's lifecycle framing (data → model → app → infra → assurance → governance) to *sanity-check family coverage* — if you have zero controls in an entire lifecycle stage, you have a coverage gap the architect must resolve or explicitly out-of-scope.

## Reading the CISA / NCSC "Guidelines for Secure AI System Development"

CISA and NCSC jointly publish this guidance in four sections aligned to the SDLC: secure design, secure development, secure deployment, secure operation and maintenance. The document is written as a recommendation set and is co-endorsed by a wide list of national cybersecurity authorities, which makes it a defensible reference when negotiating requirements with regulators or customers who cite "internationally aligned secure AI practice."

Composition move:

1. Map each numbered recommendation to at least one library entry whose statement, if realised, delivers the recommendation. The mapping is often 1:many; supply-chain recommendations in the *secure deployment* section collapse onto the ML-BOM + attestation entries.
2. Add `crosswalk: cisa_ncsc_secure_ai: [design.<n>, dev.<n>, deploy.<n>, ops.<n>]` — pin to the document version in the library preface. <!-- needs-research: confirm the current section numbering scheme against the latest joint publication; the four-part shape is stable but section IDs can shift between revisions. -->
3. For recommendations that have no matching entry, mint one. The most common gap is around *secure design* recommendations (threat modelling before build, adversarial requirements in the spec) — many enterprises' governance-family libraries never mandated a pre-build threat model. Mint an `AIC-ENG-*` entry whose statement names the outcome ("an AI threat model exists for the system before development starts and is reviewed before deployment").

Because CISA/NCSC is provider-side by framing, the mapping also produces a natural filter for *deployer-only* controls: recommendations that assume you build the system will be absent from a deployer-only SoA and should be justified in the SoA notes rather than silently dropped.

## Reading the ENISA "Multilayer Framework for Good Cybersecurity Practices for AI"

ENISA publishes an AI cybersecurity practice framework organised as a *layered* model — organisational, technical, and procedural practices, each with sub-practices — and it is the reference EU regulators and EU-facing operators most often expect to see cited alongside the AI Act's security-of-high-risk-systems obligations (AI Act Article 15). <!-- needs-research: confirm current ENISA publication title and section shape against the live agency library, as ENISA revises AI-related deliverables regularly. -->

Composition move:

1. Treat the three layers as an *evaluation harness* for library coverage, not as three families that force restructuring. Your library still organises by outcome family (data, model, integration, monitoring, incident, governance), and ENISA is used to *audit* whether each layer's practices are covered.
2. In library entries where a specific ENISA practice is the closest external reference, add `crosswalk: enisa_ai_practices: [<layer>.<practice-id>]` pinned to the publication version.
3. For controls that respond to AI Act Article 15 obligations (accuracy, robustness, cybersecurity of high-risk systems), the ENISA crosswalk is the pattern that lets an EU auditor reading the SoA jump from Article 15 to the ENISA-recommended practice to the library entry that implements it. That three-step traversal is what a well-composed threat-side crosswalk buys you.

## Worked example: prompt-injection defence entry

To make the composition move concrete, here is a fresh entry minted because none of the governance-family sources land specifically enough on prompt-injection defence.

```yaml
control:
  id: AIC-ENG-041
  title: Untrusted-content instruction isolation

  statement: >
    For each AI system in scope that consumes tool-use instructions
    or performs privileged actions on behalf of a user, the system
    treats instructions embedded in retrieved / uploaded / third-party
    content as untrusted data and does not execute those instructions
    without an explicit, authenticated human authorisation for the
    specific action.

  applicability:
    system_tier: [tier-1, tier-2]
    system_kind: [generative, foundation-model-adapter, agent-with-tool-use]
    use_case: [customer-facing, employee-productivity, agentic]

  implementation_guidance:
    - "Guardrail authoring lives with `ai-risk-engineer` per AIC-ENG-041 delegation; see chapter 08."
    - "Reference the enterprise instruction-isolation pattern (ENT-PLATFORM-GRD-11)."

  testing_procedure:
    - id: AIC-ENG-041-T1
      description: >
        Execute the enterprise prompt-injection test suite (covering
        MITRE ATLAS techniques and the current OWASP LLM01 scenario
        library, pinned in the suite manifest) against the system's
        untrusted-content surfaces.
      cadence: pre-release-and-quarterly
      owner_role: ai-risk-engineer

  evidence_contract:
    - artifact: prompt-injection-test-report.md
      schema_ref: ENT-SCHEMA-PITEST-1.1
      owner_role: ai-governance-analyst
      producer_role: ai-risk-engineer
      cadence: pre-release-and-quarterly
      retention_years: 7
      immutability: append-only

  crosswalk:
    nist_ai_rmf: [MEASURE-2.7, MANAGE-2.3]
    iso_iec_42001_annex_a: [A.6.2]                    # <!-- needs-research: confirm the closest Annex A mapping against the current 42001 revision -->
    eu_ai_act: [Article 15]
    owasp_llm_top_10: [LLM01, LLM06]
    mitre_atlas_techniques: [AML.T0051]                 # <!-- needs-research: verify current ATLAS technique IDs against live matrix -->
    google_saif: [prompt-injection, rogue-actions]
    cisa_ncsc_secure_ai: [design, deploy]
    enisa_ai_practices: [technical.input-validation]
```

Note what is *not* in the statement: it does not name a vendor, it does not name a specific mitigation ("we use LLM-Guard"), and it does not paraphrase OWASP. It names the outcome the org commits to, then reaches to every relevant threat and governance source through crosswalk.

## Coverage as a testable property of the composition

Once the five threat-side sources are composed, the coverage-map artefact is a first-class deliverable:

- **OWASP LLM Top 10 coverage** — every LLM-NN → non-empty set of AIC entries, or an explicit "not applicable — reason" line.
- **MITRE ATLAS technique coverage** — every technique the org considers in-scope → at least one AIC entry with a testing procedure that attempts the technique.
- **SAIF risk-area coverage** — every SAIF risk area → non-empty AIC set or explicit out-of-scope.
- **CISA / NCSC recommendation coverage** — every numbered recommendation → non-empty AIC set or explicit out-of-scope, framed as provider- or deployer-only where relevant.
- **ENISA layer coverage** — each of the three layers has a non-empty AIC set (a whole empty layer is almost always a real gap).

The architect publishes these coverage maps alongside the catalog release (chapter 06 governs the release). Coverage is the property that lets the head-of-AI-governance (level 60) answer a board question honestly: "which of the community-standard threat frameworks do we cover, and where are the gaps."

## Two common shape mistakes

**Mistake 1 — Parallel threat-side catalog.** Standing up `SEC-AI-*` alongside `AIC-*` and duplicating entries because "security wants their own catalog." Two catalogs desynchronise within one release cycle. Instead: one library, family prefixes for engineering-side entries (`AIC-ENG-*`), and a crosswalk that reaches every framework.

**Mistake 2 — Verbatim OWASP / SAIF text in `statement`.** The statement is the *organisation's* commitment. If it reads like a paraphrase of OWASP LLM01, it can never be more precise than OWASP LLM01 — but the org can and should be more precise (naming its own tiers, its own applicability, its own testable outcome). Reference the source in the crosswalk, do not embed it in the statement.

## Summary

Five threat-side sources — OWASP LLM Top 10, MITRE ATLAS, Google SAIF, CISA / NCSC Secure AI SDLC, and ENISA's multilayer framework — compose into the *same* control library the governance-family sources populated. The move is: reuse where an outcome already exists, mint fresh engineering-family entries where it does not, always multi-parent via crosswalk, always pin to source versions, always publish coverage maps as first-class artefacts. Do not fork a parallel security catalog and do not paraphrase source dialects into the statement field. Get this pass right and the library talks to red team, pen test, EU auditor, and NIST-facing CIO from one set of entries.
