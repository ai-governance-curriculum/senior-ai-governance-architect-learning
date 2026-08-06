# mod-110-monitoring-and-post-market-surveillance-architecture: Enterprise Post-Market Surveillance and Monitoring Architecture

This is the level-50 architect's module for the *post-market* half of the AI governance system — the architecture the enterprise runs *after* the pre-deployment gate has closed and the system is in production. It designs the post-market surveillance (PMS) system that discharges EU AI Act Article 72 at enterprise scale, the monitoring-to-risk-register contract that keeps the mod-106 register fresh, the observability-platform wiring that avoids three sources of truth, the Article 73 serious-incident workflow, the enterprise SOC interface for AI-specific signals, and the external-corpora calibration cadence that keeps the enterprise's PMS honest against the public record. Downstream modules (mod-111 GRC platform, mod-112 program design, mod-113 sector blueprints) all bind to the artefacts this module authors.

**Estimated effort:** 14 hours

## Learning objectives

- Design the enterprise post-market surveillance architecture — EU AI Act Article 72 obligations at enterprise scale, incident aggregation into a single source of truth, near-miss data handling, regulatory-reporting cadence, deviation-from-appetite alarms
- Design the monitoring-to-risk-register contract at enterprise scale — which monitor feeds which risk-register field, at what cadence, with what threshold, routing to which owner — extending the team-scope contract `ai-risk-engineer` (level 25) authors to enterprise scope
- Design the wiring across model-observability platforms (Fiddler AI, Arthur AI, WhyLabs, Evidently AI) into the GRC-for-AI system of record and the enterprise data-lake, without producing three sources of truth
- Design the EU AI Act Article 73 serious-incident reporting workflow — internal escalation, coordination with `head-of-ai-governance` (level 60) for regulator-facing filing, coordination with legal + PR + SecOps; the architect designs the workflow, does not run the filing
- Design the interface with the enterprise Security Operations Center (SOC) — AI-specific signal handoff, jailbreak / prompt-injection incident routing, red-team-finding to detection-signal handoff — coordinating with `security-learning` / `ai-infra-security` (level 35) on SOC-side runbook depth (out of scope for this role)
- Layer AI Incident Database + OECD.AI incidents + MIT AI Risk Repository as external calibration references — how the enterprise post-market surveillance corpus compares to public incidents

## Chapters

- [`01-eu-ai-act-article-72-and-the-enterprise-post-market-surveillance-shape.md`](./01-eu-ai-act-article-72-and-the-enterprise-post-market-surveillance-shape.md) — the Article 72 anchor and the six invariants an enterprise PMS system must hold; the two failure modes (three sources of truth, near-miss-blindness) the architecture is designed against.
- [`02-monitoring-to-risk-register-contract-at-enterprise-scale.md`](./02-monitoring-to-risk-register-contract-at-enterprise-scale.md) — the enterprise-scope contract that fixes the shape every team-scope contract must fit; the five fields per monitor-to-register edge; cardinality and sprawl-prevention.
- [`03-observability-platform-wiring-and-the-single-source-of-truth.md`](./03-observability-platform-wiring-and-the-single-source-of-truth.md) — the two-store convergence (GRC system of record + enterprise data lake) and the normalisation layer that prevents platform lock and replay blindness.
- [`04-article-73-serious-incident-reporting-workflow-design.md`](./04-article-73-serious-incident-reporting-workflow-design.md) — the internal-escalation SOP, the parallel-obligation fan-out (Article 73, GDPR Article 33, SEC materiality, sector regimes, customer contracts), and the role separation between architect, head-of-ai-governance, legal, PR, and SecOps.
- [`05-the-soc-interface-and-ai-specific-signal-handoff.md`](./05-the-soc-interface-and-ai-specific-signal-handoff.md) — the coordination contract with the enterprise SOC covering SOC→PMS, PMS→SOC, and red-team→detection handoffs; the boundary with `ai-infra-security` (level 35).
- [`06-external-incident-corpora-as-calibration-references.md`](./06-external-incident-corpora-as-calibration-references.md) — the AIID / OECD.AI / MIT AI Risk Repository calibration cadence; the three calibration activities (taxonomy coverage, severity calibration, near-miss-pattern review) that keep the enterprise PMS honest.

## Exercises

- [`exercises/exercise-01-eu-ai-act-article-72-enterprise-shape-drill.md`](./exercises/exercise-01-eu-ai-act-article-72-enterprise-shape-drill.md) — author the enterprise PMS shape proposal, schematic, and incident+near-miss taxonomy for one of three enterprise scenarios.
- [`exercises/exercise-02-monitoring-to-risk-register-contract-enterprise.md`](./exercises/exercise-02-monitoring-to-risk-register-contract-enterprise.md) — author the enterprise-scope monitoring-to-register contract and at least twelve worked edges across drift, performance, fairness, adversarial-robustness, LLM-quality, and SOC-sourced categories.
- [`exercises/exercise-03-observability-platform-wiring-drill.md`](./exercises/exercise-03-observability-platform-wiring-drill.md) — author the two-store wiring architecture, the normalised event schema, and the replay-and-retention plan for a realistic multi-platform mix.
- [`exercises/exercise-04-article-73-serious-incident-workflow-design.md`](./exercises/exercise-04-article-73-serious-incident-workflow-design.md) — author the Article 73 serious-incident SOP, RACI matrix, clock-start log schema, and parallel-obligations fan-out.
- [`exercises/exercise-05-soc-interface-design-drill.md`](./exercises/exercise-05-soc-interface-design-drill.md) — author the SOC-interface coordination contract, signal-handoff schema, red-team-to-detection registry, and MITRE ATLAS crosswalk.

## Structure

- `01-…md` … `06-…md`: lecture chapters.
- `exercises/`: per-exercise prompts. Solutions live in the paired `-solutions` repo.
- `labs/`: long-form hands-on labs (scaffolded; content lands on a subsequent cycle).
- `quizzes/`: knowledge checks (scaffolded; content lands on a subsequent cycle).
- `resources.md`: external references — primary regulation, standards, observability-platform pointers, security-side references, external incident corpora, and links to sibling modules in this track.
