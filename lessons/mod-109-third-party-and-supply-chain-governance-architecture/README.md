# mod-109-third-party-and-supply-chain-governance-architecture: Third-Party and AI Supply-Chain Governance Program Architecture

> Scaffolded by `aicg org execute-plan`. Lecture chapters and exercise content are authored on subsequent autonomous cycles.

**Estimated effort:** 16 hours

## Learning objectives

- Design the third-party AI governance programme — SR 23-4 shape at architect scope applied to foundation-model providers (Anthropic, OpenAI, Google, Meta, xAI, open-weights vendors), guardrail vendors, AI-observability vendors, GRC-for-AI platform vendors, dataset providers
- Design the tiering criteria for third-party AI vendors — data sensitivity, decision materiality, replaceability, jurisdictional exposure — and the due-diligence questionnaire attached to each tier
- Design the contract-template controls — data-use limits, evaluation-access rights, incident-notification obligations, evidence-access rights, exit / portability clauses — coordinated with procurement + legal (out-of-scope for authorship, in-scope for template design)
- Design the ongoing vendor-monitoring schedule — periodic re-attestation, drift-based re-assessment, vendor-incident routing into the enterprise risk register, contract-renewal review
- Read OMB M-24-18 (2024) and M-25-22 (2025) as the federal-facing AI acquisition shape; adapt the shape to enterprise procurement
- Design the AI supply-chain assurance slice at architect scope — CycloneDX ML-BOM + SPDX 3.0 AI profile + SLSA levels + Sigstore as the evidence contract for every third-party model artifact; coordinate with `ai-infra-security` (level 35) on runtime supply-chain / signing platform depth

## Structure

- `01-…md` … `0N-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs.
- `quizzes/`: knowledge checks.
- `resources.md`: external references.
