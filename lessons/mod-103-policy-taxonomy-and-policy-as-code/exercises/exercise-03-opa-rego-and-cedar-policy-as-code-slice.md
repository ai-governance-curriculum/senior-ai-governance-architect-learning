# exercise-03: OPA/Rego and Cedar Policy-as-Code Slice

**Estimated effort:** 4 hours

## Objective

Place a ten-policy slice of Northbrook's AI policy corpus across the three enforcement tiers from chapter [`03-policy-as-code-enforcement-tiers.md`](../03-policy-as-code-enforcement-tiers.md) — **runtime** (OPA + Rego, or AWS Cedar), **CI/CD gate**, or **attestation-only** — using the five-question decision framework, and hand-author at least three concrete artefacts (one runtime policy with tests, one gate policy with a pipeline invocation, one attestation record) so the placement is not just a spreadsheet exercise. The drill is where the architect *feels* the developer-experience cost of each tier rather than only reciting it.

## Prerequisites

- Chapter [`03-policy-as-code-enforcement-tiers.md`](../03-policy-as-code-enforcement-tiers.md) read once, with the five-question framework and the DX-cost profile absorbed.
- Local install of the OPA CLI (`opa` — `opa eval`, `opa test`) and optionally the Cedar CLI (`cedar` — `cedar validate`, `cedar authorize`). At minimum you must be able to run `opa test` on the runtime and gate fragments you author.
- The Northbrook Financial scenario carried through the track.

## Scenario

Northbrook's platform team has asked the level-50 architect to publish the *placement* of the initial AI policy-as-code slice before any Rego is written. The team is willing to build what you specify, but is (rightly) refusing to be handed a bag of `.rego` files with no design record — they have been burned before by shadow policy engines that nobody could audit and nobody could remove. Your job in this exercise is to produce the placement doc plus a small worked slice of runnable artefacts that anchors the design.

## Deliverables

Author four artefacts in a working directory of your choice:

1. **`enforcement-tier-placement.md`** — the placement decision document.
2. **`policies/`** — a directory with at least three runnable artefacts (one runtime, one gate, one attestation).
3. **`pdp-pep-boundary.md`** — the PDP / PEP interface spec for the runtime policy.
4. **`bundle-pinning-note.md`** — half a page on bundle signing, versioning, pinning, and hot-reload.

## Requirements

### `enforcement-tier-placement.md`

Take these eight starter policies plus two more you author for Northbrook (name them explicitly):

- P1 — "Only tier-2-cleared users may invoke tier-1 models on requests classified `customer-pii`."
- P2 — "A tier-1 model deploy manifest must reference a signed evaluation report ≤ 90 days old and a complete model card."
- P3 — "The Responsible AI review board convenes quarterly with quorum for every tier-1 deployment."
- P4 — "Requests classified `customer-pii` may not egress to an unapproved third-party embedding endpoint."
- P5 — "Every generative response longer than 500 tokens on the customer-facing chat traverses guardrail policy pack `v2.x` before delivery."
- P6 — "Model cards for tier-1 systems name at least one designated human-oversight modality from the AI Human Oversight Standard."
- P7 — "The head-of-AI-governance attests quarterly that every tier-1 system in production had an oversight-adequacy review in the period."
- P8 — "Fairness posture for hiring-adjacent AI systems has been reviewed against a use-case-appropriate metric set within the last two quarters."

For each of the ten policies, the placement doc names: the tier chosen (`runtime` / `gate` / `attestation`), the engine (OPA/Rego, Cedar, Sigstore-cosign attestation, or `none`), the walk through **all five** questions from chapter 03's framework (each answered explicitly with one sentence), the estimated DX-cost band (`low` / `medium` / `high`, with a one-line rationale), and where the resulting evidence lands (a control ID from the mod-102 shape, or a placeholder marked `<!-- planned -->`).

Cover the placement of at least one policy at each tier — no all-runtime or all-attestation slices.

### `policies/`

Three concrete runnable artefacts (or more; pick the two engines you cover such that at least one Rego and one attestation are present):

- **Runtime fragment** — either a Rego module (`policies/runtime/<name>.rego`) with a companion test file (`<name>_test.rego`) or a Cedar policy + schema pair (`policies/runtime/<name>.cedar`, `<name>.cedarschema`) with a Cedar test cases file. Rules must return a *structured deny* — a message naming the offending field and, where possible, the fix — not a bare `deny: true`. The test suite must cover a positive (permit) path, a negative (deny) path per rule, and one regression slot.
- **Gate fragment** — a Rego module (`policies/gate/<name>.rego`) plus its pipeline-invocation shape (a GitHub Actions `.yaml` stub is fine; GitLab CI or Buildkite equally acceptable). The gate must be runnable locally as a pre-commit hook against the same bundle — include the command line.
- **Attestation template** — a YAML record (`policies/attestation/<name>.yaml`) with all fields from chapter 03's example: `id`, `policy_ref`, `scope` (business unit, period start/end, systems), `signer` (role, signed_at), `attestation_statement`, `evidence_pointers` (dereferenceable URNs), `next_review_due`, and `signature` (method + placeholder bundle field). The scope must be *time- and system-bounded* — never "the whole company forever".

You must be able to run `opa test policies/` (or equivalent) locally and see the test suite pass. If Cedar is used, `cedar validate --schema <schema> --policies <policies>` must succeed.

### `pdp-pep-boundary.md`

Half a page (ASCII diagram or prose) on the PDP / PEP split for the runtime policy you authored:

- What component is the PEP (model gateway, service-mesh sidecar, admission-controller webhook, SDK).
- What component is the PDP (OPA sidecar, OPA server, Cedar library).
- The decision shape returned (permit / deny plus a structured reason code and a human message).
- How the decision is logged into the evidence pipeline mod-108 designs — name the log fields.
- The safe-fail posture on PDP outage (fail-closed for authorisation slices; fail-open only for auditing slices; document the choice and the review that approved it).

### `bundle-pinning-note.md`

Half a page on:

- Bundle signing (Sigstore cosign against an OIDC identity; or equivalent) and where the trust root lives.
- Immutable storage location and the version identifier scheme.
- How the PEP pins to a specific bundle version at start-up; how a bundle upgrade rolls out; how rollback works.
- How hot-reload composes with the change-communications flow from chapter [`05-policy-change-communications-and-deprecation-windows.md`](../05-policy-change-communications-and-deprecation-windows.md) — specifically, how a new signed bundle is dual-checked against the previous during the deprecation window before authority flips.

## Starter guidance

- Answer question 1 (observability at request time) before you look at the engine. It kicks half the corpus out of runtime immediately.
- Do not encode the whole policy document in Rego. One clause per module; the traceability from clause to module lives in the matrix from exercise-02. Multiple 3,000-line `.rego` files with the whole policy transcribed inline is a smell.
- Error-message quality is the single lever that determines whether a gate is followed or bypassed. `deny: true` gets you bypassed by the next hotfix. `sprintf("evaluation report is %.0f days old; max is 90", [age_days])` gets you followed.
- The attestation-only tier is not the lazy option. It is expensive at the accountable-role level. If you assign more than two attestation policies to the same role, sanity-check that the role can genuinely do the work.

## Acceptance criteria

- [ ] All ten policies (eight given, two added) have a tier assignment with an explicit answer to each of the five chapter-03 questions and a DX-cost band with rationale.
- [ ] At least one policy is placed at each of the three tiers.
- [ ] The runtime fragment returns a structured deny message and ships with a test suite (positive path, one negative per rule, one regression slot) that passes under `opa test` (or `cedar authorize` for the Cedar variant).
- [ ] The gate fragment ships with the pipeline invocation and a local pre-commit invocation, and its deny messages name the offending field and the fix.
- [ ] The attestation record has all fields from chapter 03's example and its scope is time- and system-bounded.
- [ ] `pdp-pep-boundary.md` names the PEP, the PDP, the decision shape, the log fields, and the safe-fail posture.
- [ ] `bundle-pinning-note.md` covers signing, versioning, pinning, and hot-reload — with an explicit statement that unpinned production is a finding.
- [ ] Neither of chapter 03's shape mistakes is committed. The placement doc names, for each mistake, one policy where the mistake was tempting and the reasoning that resisted it.
- [ ] Every runtime and gate fragment is syntactically valid (test-runner exit code 0).

## Stretch goals

- Belt-and-braces one of the ten policies (runtime *and* gate) and defend the case-by-case override of the one-tier-per-policy default. Half a page.
- Design the dual-check pattern for one of the policies so it can migrate cleanly on a policy revision — write both the old rule and a new-rule variant in the same bundle and describe how the CI logs the divergence before the new rule flips to authoritative.
- Budget the accountable-role attestation load per quarter across the ten policies. Identify the role most at risk of rubber-stamping and propose a refactor.
- Author the OSCAL component-definition fragment that expresses one of the runtime policies as a component satisfying the mod-102 control. Cite the OSCAL model reference; use `<!-- needs-research -->` where the field name is uncertain.
