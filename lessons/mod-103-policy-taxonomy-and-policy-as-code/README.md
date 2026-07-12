# mod-103-policy-taxonomy-and-policy-as-code: AI Policy Taxonomy and Policy-as-Code Architecture

> Scaffolded by `aicg org execute-plan`. Lecture chapters and exercise content are authored on subsequent autonomous cycles.

**Estimated effort:** 16 hours

## Learning objectives

- Design the AI policy hierarchy — Responsible AI principles → binding policy → standards → procedures → work instructions — with clear authority levels, review cadences, and exception paths
- Map every enterprise Responsible AI principle to at least one binding policy, at least one standard, and at least one testable control in the control library — so the principle is not decorative
- Design the policy-as-code slice — which policies are enforced at runtime (via Open Policy Agent + Rego or Cedar), which are enforced at CI/CD gate time, which are enforced by attestation only — and reason about the developer-experience cost of each
- Design the exception / waiver workflow — who requests, who approves, how long the waiver lives, what compensating controls apply, how the waiver is tracked in the risk register
- Design the policy-change communications flow — who gets a heads-up before publication, how policy changes route to `ai-governance-analyst` (level 15) for control-tracking updates and to `ai-risk-engineer` (level 25) for engineering-side updates, and how deprecation windows are enforced
- Position ISO/IEC 38507 (governance implications of AI) + ISO/IEC 22989 (concepts and terminology) + IEEE 7000-series as the reference frames the policy taxonomy composes with

## Structure

- `01-…md` … `0N-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs.
- `quizzes/`: knowledge checks.
- `resources.md`: external references.
