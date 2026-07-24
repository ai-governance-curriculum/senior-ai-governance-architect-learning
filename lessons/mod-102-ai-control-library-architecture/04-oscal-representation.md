# Serialising the AI control library in OSCAL

## Why this chapter exists

The library shape from chapter 01 and the composition moves from chapters 02–03 gave you a *human-readable* catalog of AI control entries with statements, applicability filters, testing procedures, evidence contracts, and crosswalks. To be *architecturally* useful — reused by the analyst's tracker, the risk engineer's automation, the AIMS Statement of Applicability, the GRC-for-AI platform, and eventually a regulator submission — it has to also be *machine-readable*, in a schema that adjacent teams and vendors already speak.

The industry-standard schema for that job is OSCAL — the Open Security Controls Assessment Language, published and maintained by NIST. OSCAL models control catalogs, profiles (selections + tailorings of a catalog), system security plans, assessment plans, assessment results, and plans of action & milestones. It is the schema NIST SP 800-53 rev5 is published in, the schema FedRAMP is standardising on, and — for the AI architect's purposes — the schema that lets the AI library plug into an enterprise's existing catalog toolchain without inventing something bespoke.

This chapter is not an OSCAL tutorial. It is the *architectural decisions* an AI-library owner has to make when representing the library in OSCAL: which OSCAL models to use, how the seven fields from chapter 01 land in OSCAL primitives, how crosswalks survive, how applicability filters survive, and how versioning + tailoring compose.

Read the OSCAL reference material at [pages.nist.gov/OSCAL](https://pages.nist.gov/OSCAL/) alongside this chapter — the schemas are the source of truth and evolve on their own cadence. <!-- needs-research: pin the specific OSCAL schema version the library serialises against once the project selects one; keep the pinned version in the library preface. -->

## The five OSCAL models — and which two you own

OSCAL defines a small set of models that fit together into a lifecycle:

| Model | Purpose | Who owns this in the AI library lifecycle |
|---|---|---|
| **Catalog** | The universe of controls. | *Architect owns.* This is the AI control library itself. |
| **Profile** | A selection + tailoring of controls from one or more catalogs, for a specific applicability. | *Architect owns per baseline; deployment teams import and tailor further.* |
| System Security Plan (SSP) | A specific system's implementation description against a profile. | Deployment / analyst level (15) authors; architect designs the SSP template. |
| Assessment Plan / Results | The assessment approach and its findings. | Assurance / evaluation (level 35) owns; architect designs the evidence contract that assessment output must satisfy. |
| Plan of Action & Milestones (POA&M) | Tracked deficiencies + remediation. | Analyst (level 15) executes; architect designs the schema. |

The architect *authors* the Catalog and the baseline Profiles. Everything downstream is a consumer. The rest of this chapter is about the Catalog and Profile models.

## Mapping the seven library fields onto the OSCAL catalog model

An OSCAL catalog contains `groups` and `controls`. A control has `id`, `title`, `params`, `props`, `links`, `parts`, and can nest sub-controls. The seven fields from chapter 01 land as follows:

| Library field | OSCAL representation | Notes |
|---|---|---|
| `id` / `title` / `version` | `control.id` / `control.title`; version pinned at the catalog `metadata.version` and on the individual control via a `prop` (`name: version`) | Catalog metadata carries the library-wide version; per-control version deltas ride on control props. |
| `statement` | A `part` with `name: statement` | The primary control statement is the canonical OSCAL location; use CommonMark. |
| `applicability` | Named `props` (`name: applicability.system_tier`, `.jurisdictions`, etc.) *and* profile-level `include-controls`/`exclude-controls` for coarse selection | Fine-grained predicates ride on props; coarse "does this profile use this control at all" rides on the profile. |
| `implementation_guidance` | A `part` with `name: guidance` | Non-normative colour — auditors know to read this as guidance rather than commitment. |
| `testing_procedure` | A `part` with `name: assessment` containing `parts` per test method (owner role, cadence, sampling) | Machine-readable but conservatively shaped; deeper structure moves to the Assessment Plan model. |
| `evidence_contract` | `links` to schema-registry entries plus a `part` with `name: objective` describing the assessment objective the evidence satisfies | Evidence-contract detail lives in a parallel schema registry the architect owns; OSCAL carries pointers. |
| `crosswalk` | `links` with a `rel` per source (`rel: related`, plus `href` to the source anchor) and `props` naming the source ID and version | Every crosswalk edge is a first-class link, so downstream tools can traverse. |

Two design choices deserve emphasis.

**One `part` per role.** OSCAL `part` names are the primary lever for structured content. Standardise your library's part names across every control — `statement`, `guidance`, `objective`, `assessment`, `evidence`, `notes` — and document the convention in the catalog preface. A downstream renderer or diff tool that expects these names will work across every control; a library with ad-hoc part names produces one-off scripts per control family.

**Props for applicability predicates.** OSCAL props are name / value pairs, optionally namespaced with `ns`. Applicability predicates ride on props: `<prop name="system_tier" value="tier-1" ns="urn:enterprise:aic:applicability"/>`. Namespace the applicability props so downstream automation can distinguish enterprise-authored predicates from NIST-defined props on inherited SP 800-53 controls.

## Catalog structure — how the library organises inside a single OSCAL catalog

Every library entry from chapters 01–03 lives inside one catalog. The catalog is grouped hierarchically:

```xml
<catalog uuid="...">
  <metadata>
    <title>Enterprise AI Control Library</title>
    <version>2026.1.0</version>
    <last-modified>2026-07-01T00:00:00Z</last-modified>
    <oscal-version>1.1.2</oscal-version>  <!-- needs-research: pin to the OSCAL revision the toolchain supports -->
  </metadata>

  <group id="AIC-GOV">
    <title>Governance</title>
    <control id="aic-gov-001"> ... </control>
    ...
  </group>

  <group id="AIC-DAT">
    <title>Data lifecycle</title>
    <control id="aic-dat-014">
      <title>Training-data provenance record</title>

      <prop name="version" value="1.2.0"/>
      <prop name="system_tier" value="tier-1" ns="urn:enterprise:aic:applicability"/>
      <prop name="system_tier" value="tier-2" ns="urn:enterprise:aic:applicability"/>
      <prop name="jurisdiction" value="EU"    ns="urn:enterprise:aic:applicability"/>
      <prop name="jurisdiction" value="UK"    ns="urn:enterprise:aic:applicability"/>

      <link rel="related" href="#nist-ai-rmf.map-2.3"/>
      <link rel="related" href="#iso-42001.annex-a.7.2"/>
      <link rel="related" href="#eu-ai-act.article-10"/>
      <link rel="reference" href="urn:enterprise:schema:dprov#1.4"/>

      <part name="statement">
        <p>For each AI system in scope, the organization maintains a
           provenance record of every training dataset — source, license,
           lawful basis, collection date, transformation history, and
           integrity hash — sufficient to satisfy EU AI Act Article 10
           data-governance obligations and ISO/IEC 42001 A.7 data-lifecycle
           obligations.</p>
      </part>

      <part name="guidance">
        <p>Provenance records live in the model-registry provenance store
           (see <a href="urn:enterprise:platform:reg-08">ENT-PLATFORM-REG-08</a>).</p>
      </part>

      <part name="objective">
        <p>Given a randomly sampled tier-1 or tier-2 in-scope training
           dataset, the associated provenance record is retrievable, valid,
           and complete against the enterprise data-provenance schema.</p>
      </part>

      <part name="assessment">
        <part name="method">
          <prop name="method" value="EXAMINE"/>
          <prop name="cadence" value="quarterly"/>
          <prop name="owner_role" value="ai-governance-analyst"/>
          <p>Sample five in-scope training datasets per quarter and confirm
             the provenance record contains all seven mandatory fields.</p>
        </part>
        <part name="method">
          <prop name="method" value="TEST"/>
          <prop name="cadence" value="pre-release-and-quarterly"/>
          <prop name="owner_role" value="ai-risk-engineer"/>
          <p>For each tier-1 system, verify integrity hash matches
             the registry record.</p>
        </part>
      </part>
    </control>
    ...
  </group>

  <group id="AIC-ENG"> ... </group>
  <group id="AIC-SEC"> ... </group>
  ...

  <back-matter>
    <resource uuid="nist-ai-rmf.map-2.3">
      <title>NIST AI RMF 1.0 MAP-2.3</title>
      <citation><text>NIST AI Risk Management Framework 1.0</text></citation>
      <rlink href="https://www.nist.gov/itl/ai-risk-management-framework"/>
    </resource>
    <resource uuid="iso-42001.annex-a.7.2"> ... </resource>
    <resource uuid="eu-ai-act.article-10"> ... </resource>
    ...
  </back-matter>
</catalog>
```

Two things to note in that fragment. First, *every crosswalk is a link* to a `back-matter/resource`, and every back-matter resource is a real, pinned reference to the source (with a URL you can dereference). That is what makes crosswalks survive: the resource records the source at a specific version, so an audit two years from now can still resolve the reference. Second, *assessment methods* use OSCAL's controlled vocabulary (EXAMINE / INTERVIEW / TEST) as a `prop`; the cadence and owner role are additional props on the same method part. That combination is the minimum needed for a downstream assessment tool to schedule and route the test.

## The profile model — jurisdictional and tier baselines

The catalog is the universe. A **profile** is *a selection + tailoring of controls for a specific applicability*. The architect authors baseline profiles that consumer teams import:

- `profile-eu-tier1-high-risk` — controls for tier-1 EU-deployed high-risk systems (the union of governance-family and threat-family entries whose applicability filters match).
- `profile-us-federal-agency-facing` — controls for systems supplied to US federal agencies (OMB M-24-18 / M-25-22 shape from mod-109).
- `profile-financial-tier1` — controls under SR 11-7 shape for tier-1 financial systems (mod-113 blueprint).

An OSCAL profile is roughly:

```xml
<profile uuid="...">
  <metadata>
    <title>Enterprise AI Baseline — EU Tier-1 High-Risk</title>
    <version>2026.1.0</version>
    ...
  </metadata>

  <import href="urn:enterprise:aic-catalog#2026.1.0">
    <include-controls>
      <with-id>aic-dat-014</with-id>
      <with-id>aic-eng-041</with-id>
      ...
    </include-controls>
  </import>

  <modify>
    <alter control-id="aic-dat-014">
      <add position="ending" by-id="assessment">
        <part name="method">
          <prop name="method" value="EXAMINE"/>
          <prop name="cadence" value="pre-market-audit"/>
          <prop name="owner_role" value="ai-evaluation-engineer"/>
          <p>Additional EU pre-market conformity examination step.</p>
        </part>
      </add>
    </alter>
  </modify>

  <back-matter>
    <resource uuid="eu-ai-act.pre-market-conformity">
      <title>EU AI Act pre-market conformity assessment</title>
      <rlink href="https://eur-lex.europa.eu/eli/reg/2024/1689/oj"/>
    </resource>
  </back-matter>
</profile>
```

Three profile design decisions:

**Selection semantics.** Prefer `include-controls` with explicit IDs over `include-all` + `exclude-controls`. Explicit inclusion makes the profile self-explanatory at audit time and stops silent drift when the catalog adds new controls.

**Tailoring semantics.** `modify` is powerful. Use it sparingly, and only to *add* profile-specific parts or props (a jurisdictional cadence, a jurisdictional evidence obligation). Never use `modify` to change the statement of a control — if the outcome changes, that is a new control, not a tailoring.

**Baseline set.** Publish a *small* set of baseline profiles that cover the org's dominant deployment shapes and let downstream teams import and further tailor. A hundred bespoke profiles is a smell — either the applicability filter design (chapter 01) is too coarse, or the baseline set duplicates itself.

## Evidence-contract schemas outside OSCAL

The `evidence_contract` field in the library entry is richer than OSCAL's built-in vocabulary supports. OSCAL carries pointers via `link rel="reference"` and via a `part name="evidence"`; the actual evidence-schema definitions live in a parallel schema registry the architect maintains — JSON Schema, XSD, or equivalent — with its own versioning.

The registry's schemas are what `ai-governance-analyst` uses to validate collected evidence, what `ai-risk-engineer` produces, and what the GRC-for-AI platform (mod-111) reads. OSCAL's role is to *point* at them from the catalog so a tool traversing the catalog can find them; OSCAL is not their home. Mod-108 designs the evidence schema library itself.

The convention: every OSCAL `link rel="reference"` in an assessment part resolves via a URN (`urn:enterprise:schema:dprov#1.4`) into the registry. That single indirection lets the catalog release cadence and the evidence-schema release cadence differ.

## Cross-catalog composition — importing SP 800-53 and CSF 2.0

The AI control library is *not* the only catalog in the enterprise. SP 800-53 rev5 is published as an OSCAL catalog by NIST directly, CSF 2.0 has an OSCAL representation, and many enterprises have internal SOC / IT catalogs already in OSCAL. Composition uses OSCAL's *import* mechanism:

- The AI catalog *references* SP 800-53 controls where an AI outcome is a specialisation of a broader SP 800-53 control (SP 800-53 AU-2 audit events becomes the parent of `AIC-EVD-*` audit-log entries in mod-108).
- The reference is a `link rel="related" href="urn:oscal:800-53r5:au-2"` in the AI control, plus a matching `back-matter/resource` that names the pinned SP 800-53 revision.
- The AI catalog does *not* copy SP 800-53 controls into itself. Import + link avoids duplication and keeps the SP 800-53 authoritative source separate.

For CSF 2.0, the crosswalk is the reverse-mapping the CIO organisation uses — CSF subcategory → AI catalog controls that participate in that outcome. The link direction is from AI catalog controls *to* CSF subcategories; the CSF catalog is not imported wholesale.

## Versioning and the release contract

Two version streams matter:

- **OSCAL schema version** — pinned in `catalog.metadata.oscal-version`. Upgrade only when the toolchain (validators, renderers, GRC-for-AI platform, downstream consumers) all support the new version. The library preface publishes the pin.
- **Library version** — pinned in `catalog.metadata.version` (`2026.1.0`), and per-control in a `prop name="version"`. Semver-ish per chapter 01; breaking changes bump major.

The release contract chapter 06 spells out is: a library release is a full OSCAL catalog + baseline profile set at a pinned OSCAL version, published to an immutable location (a URI you can dereference in ten years), with a change log that names every added / modified / deprecated control and profile. Downstream consumers pin against the released version, not against `latest`.

## Two common shape mistakes

**Mistake 1 — Free-text applicability inside `statement`.** Writing "For each AI system in scope (tier-1 or tier-2, deployed in EU or UK)" inside the statement rather than as `props`. Machine-readability dies: no downstream automation can select controls for a given system without regex-parsing the statement. Put predicates in props; keep the statement outcome-oriented.

**Mistake 2 — Crosswalk edges as prose, not links.** Writing "This control satisfies NIST AI RMF MAP-2.3 and ISO/IEC 42001 A.7.2" inside a `part name="notes"`. That looks fine to a reader; a machine traversing the catalog cannot follow it, and version pinning is impossible. Use `link` with a resolvable `href` to a pinned back-matter resource, always.

## Summary

The AI control library serialises as an OSCAL catalog (architect owns) plus a small set of baseline profiles (architect owns) plus per-system SSPs (deployment / analyst produces from a template the architect designs). The seven library fields land on OSCAL primitives: statement / guidance / objective / assessment / evidence as `part`s with a house-standard naming convention; applicability as namespaced `prop`s; crosswalks as `link`s to pinned `back-matter` resources. Cross-catalog composition imports SP 800-53 and CSF 2.0 via reference, never by copy. Evidence-schema definitions live in a parallel registry that OSCAL points at rather than embeds. Pin the OSCAL schema version and the library version explicitly and release against them; do not ship "latest." Get this serialisation right and the library is consumable by every downstream tool the enterprise already has without inventing a bespoke format for the AI slice.
