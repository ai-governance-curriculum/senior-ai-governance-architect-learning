# mod-110-monitoring-and-post-market-surveillance-architecture: Enterprise Post-Market Surveillance and Monitoring Architecture

> Scaffolded by `aicg org execute-plan`. Lecture chapters and exercise content are authored on subsequent autonomous cycles.

**Estimated effort:** 14 hours

## Learning objectives

- Design the enterprise post-market surveillance architecture — EU AI Act Article 72 obligations at enterprise scale, incident aggregation into a single source of truth, near-miss data handling, regulatory-reporting cadence, deviation-from-appetite alarms
- Design the monitoring-to-risk-register contract at enterprise scale — which monitor feeds which risk-register field, at what cadence, with what threshold, routing to which owner — extending the team-scope contract `ai-risk-engineer` (level 25) authors to enterprise scope
- Design the wiring across model-observability platforms (Fiddler AI, Arthur AI, WhyLabs, Evidently AI) into the GRC-for-AI system of record and the enterprise data-lake, without producing three sources of truth
- Design the EU AI Act Article 73 serious-incident reporting workflow — internal escalation, coordination with `head-of-ai-governance` (level 60) for regulator-facing filing, coordination with legal + PR + SecOps; the architect designs the workflow, does not run the filing
- Design the interface with the enterprise Security Operations Center (SOC) — AI-specific signal handoff, jailbreak / prompt-injection incident routing, red-team-finding to detection-signal handoff — coordinating with `security-learning` / `ai-infra-security` (level 35) on SOC-side runbook depth (out of scope for this role)
- Layer AI Incident Database + OECD.AI incidents + MIT AI Risk Repository as external calibration references — how the enterprise post-market surveillance corpus compares to public incidents

## Structure

- `01-…md` … `0N-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs.
- `quizzes/`: knowledge checks.
- `resources.md`: external references.
