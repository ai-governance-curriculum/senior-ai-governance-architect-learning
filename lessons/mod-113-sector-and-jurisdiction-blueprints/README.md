# mod-113-sector-and-jurisdiction-blueprints: Sector and Jurisdiction Reference Blueprints

This is the sector-adaptation module of the Senior AI Governance / Risk Architect track. It takes the mod-101 through mod-112 reference architecture — one control library, one AIMS, one risk taxonomy, one assurance architecture, one third-party programme, one PMS shape, one GRC platform, one AI governance council — and instantiates it for six major sectors (US G-SIB banking, US health system, insurance carrier, pharmaceutical, US federal agency contractor, critical-infrastructure operator) plus a public-sector-adjacencies chapter, all governed by a single sector-adaptation methodology. Every sector chapter fills in the same sector-adaptation-record schematic and defends its choices against the same six invariants, so a reviewer moving between banking, health, insurance, pharma, federal, and critical-infrastructure blueprints reads them in one shape and can trace each sector-specific overlay back to a preserved enterprise control rather than a forked control library.

**Estimated effort:** 16 hours

## Learning objectives

- Author a reference AIMS + control-library instantiation for a US G-SIB bank — SR 11-7 + OCC 2011-12 + SR 23-4 + EU AI Act + Colorado AI Act + OSFI E-23 (for Canadian FI) — including validation-vs-development independence and effective-challenge shape
- Author a reference AIMS + control-library instantiation for a US health system — FDA GMLP + FDA PCCP + HIPAA + EU AI Act Annex III + state medical-AI regulation — including SaMD change-control planning
- Author a reference AIMS + control-library instantiation for an insurance carrier — NAIC AI Model Bulletin + Colorado Insurance Regulation + EU AI Act + state DOI activity
- Author a reference AIMS + control-library instantiation for a pharmaceutical company — GxP + EU AI Act + FDA + EMA + regulatory-submission workflow
- Author a reference AIMS + control-library instantiation for a US federal agency contractor — OMB M-25-21 + M-25-22 + FedRAMP + NIST SP 800-53 + NIST AI RMF + US AISI methodology — including the Chief AI Officer designation shape
- Author a reference AIMS + control-library instantiation for a critical-infrastructure operator — CISA / NCSC Secure AI System Development + ENISA Multilayer Framework + NIST CSF 2.0 + sector-specific regulator
- Design the sector-adaptation methodology — how to instantiate the enterprise reference architecture for a new sector without forking the control library
- Position Canada TBS Directive on Automated Decision-Making + UK ATRS + Singapore AI Verify as adjacencies for public-sector-adjacent private-sector work

## Chapters

1. [01 — The sector-adaptation methodology](01-the-sector-adaptation-methodology.md) — six invariants, the sector-adaptation record schematic, and the two failure modes the methodology designs against.
2. [02 — US G-SIB banking blueprint](02-us-g-sib-banking-blueprint.md) — SR 11-7 / OCC 2011-12 as the composition anchor; SR 23-4 for third-party; OSFI E-23 for Canadian subsidiaries; EU AI Act Annex III paragraph 5(b) for EMEA operations; Colorado AI Act for consumer-lending overlay.
3. [03 — US health system blueprint](03-us-health-system-blueprint.md) — FDA GMLP + PCCP for SaMD change control; HIPAA as covered-entity boundary; EU AI Act Annex III for EMEA medical AI; state medical-AI regulation.
4. [04 — Insurance carrier blueprint](04-insurance-carrier-blueprint.md) — NAIC AI Model Bulletin as the composition anchor; Colorado Insurance Regulation 10-1-1; EU AI Act; state DOI market-conduct examination.
5. [05 — Pharmaceutical blueprint](05-pharmaceutical-blueprint.md) — GxP + Part 11 as predicate-rule frame; CSV/CSA composition; EMA reflection paper; FDA CDER/CBER; combination-product boundary to SaMD.
6. [06 — US federal agency contractor blueprint](06-us-federal-agency-contractor-blueprint.md) — OMB M-25-21 / M-25-22 + FedRAMP + NIST SP 800-53 + NIST AI RMF + US AISI methodology; agency-side CAIO designation as composition anchor.
7. [07 — Critical infrastructure operator blueprint](07-critical-infrastructure-operator-blueprint.md) — CISA/NCSC Secure AI System Development + ENISA Multilayer Framework + NIST CSF 2.0 + sector-specific regulator (NERC CIP / TSA SD / NIS2); AI-in-OT boundary discipline.
8. [08 — Public-sector adjacencies](08-public-sector-adjacencies-canada-uk-singapore.md) — Canada TBS Directive on ADM, UK ATRS, Singapore AI Verify as lightweight extensions to the reference architecture.

## Exercises

- [exercise-01 — Banking Blueprint Drill](exercises/exercise-01-banking-blueprint-drill.md)
- [exercise-02 — Health Blueprint Drill](exercises/exercise-02-health-blueprint-drill.md)
- [exercise-03 — Insurance plus Pharma Blueprint Drill](exercises/exercise-03-insurance-plus-pharma-blueprint-drill.md)
- [exercise-04 — Public Sector Blueprint Drill](exercises/exercise-04-public-sector-blueprint-drill.md)
- [exercise-05 — Critical Infrastructure Blueprint Drill](exercises/exercise-05-critical-infrastructure-blueprint-drill.md)

## Additional

- [resources.md](resources.md) — curated primary sources for the anchor regulations, frameworks, and standards.
- [labs/](labs/) — long-form hands-on labs (scaffolded).
- [quizzes/](quizzes/) — knowledge checks (scaffolded).
