# The policy-as-code enforcement slice — runtime, gate, attestation

## Why this chapter exists

Most policies never become policy-as-code. They live as PDF paragraphs, as bullet points in a runbook, as institutional memory carried around by two senior engineers who left last quarter. That is not a failure of ambition — it is the default state of most enterprise policy corpora, and it is the state a Senior AI Governance / Risk Architect inherits on day one.

The temptation is to fix this by encoding *everything* as policy-as-code. That is worse. Encoding a policy at the wrong tier costs developer velocity forever: a runtime policy that fires on every request adds latency, adds a failure mode in the request path, and forces the platform team to debug governance in production. A gate policy that ought to have been runtime gets bypassed the moment a hotfix goes out of band. An attestation-only policy that ought to have been a gate gets signed with a shrug on the last Friday of the quarter.

The architectural work is therefore *placement* — deciding which policies belong at which tier — and it is a decision that lives with the architect, not with the platform team who will be handed the implementation ticket. This chapter frames the three tiers, the five-question decision framework, and the developer-experience cost profile of each. `01-ai-policy-hierarchy-authority-and-cadence.md` established what a policy *is* and who signs it; `02-principle-to-policy-to-standard-to-control-traceability.md` established how a policy dereferences into a testable control from mod-102. This chapter is about which of those controls are enforced by a machine synchronously, which by a machine at build / deploy time, and which by a human signature.

## The three enforcement tiers at a glance

| Tier | Where it runs | Failure mode | Suitable for | DX cost |
|---|---|---|---|---|
| **Runtime** | Request path, admission controller, sidecar, SDK | Rejects the action synchronously | Authorisation, egress, rate / quota, guardrail routing | Highest — latency, request-path failure, ops burden |
| **CI/CD gate** | Build / deploy pipeline | Blocks the pipeline | Model-card completeness, evaluation-report presence, dataset-provenance existence, licence compatibility, ML-BOM signature | Medium — pipeline time, requires good error messages and local parity |
| **Attestation-only** | Human signature on a schedule | The absence of a fresh attestation is itself the finding | Human-process policies, expert-judgment policies, policies whose runtime enforcement would be prohibitively expensive | Nearly free at the developer level; expensive at the accountable-role level |

Every policy in the corpus lands in exactly one of these three tiers — with the possible exception of *belt-and-braces* placements (a runtime policy that also has a gate check as a defence-in-depth) that the architect approves case by case. The default should be one tier per policy; belt-and-braces is a design decision, not a habit.

## Runtime enforcement — OPA + Rego, or AWS Cedar

Runtime enforcement means the policy is evaluated *in the request path*. A user (or a service acting on a user's behalf) attempts an action; the enforcement point calls out to a policy decision point; the decision point returns permit or deny; the enforcement point rejects the action synchronously if the decision is deny.

The two mainstream policy-as-code engines for this tier are:

- **Open Policy Agent (OPA)** — a general-purpose policy engine written in Go, with its own declarative policy language, Rego. OPA runs as a sidecar, a library, or a service; policies are hot-reloadable from a signed bundle. See [openpolicyagent.org](https://www.openpolicyagent.org/) and the Rego language documentation. In Kubernetes, OPA is most commonly deployed as [OPA Gatekeeper](https://open-policy-agent.github.io/gatekeeper/), which wires OPA into the Kubernetes admission-controller webhook.
- **AWS Cedar** — a policy language and evaluation engine explicitly designed for authorisation, with a *typed* schema and a formal semantics. Cedar policies are evaluated against principals, actions, resources, and a context object. See [cedarpolicy.com](https://www.cedarpolicy.com/).

Runtime enforcement is suitable for policies whose violation is observable at request time and whose evaluation is a bounded, structured decision:

- **Authorisation** — who may invoke which model. "Only tier-2-cleared users may call tier-1 models on customer PII." Classic Cedar territory; equally expressible in Rego.
- **Egress** — which data classes may reach which endpoints. "Requests classified as `confidential-customer` may not egress to a third-party embedding endpoint." Enforced at the SDK or gateway PEP.
- **Rate and quota** — per-tenant, per-model, per-user token budgets. The PEP is the model gateway.
- **Guardrail routing** — which guardrail stack a request must traverse before reaching a model, based on request classification and target model tier. The PEP is the model gateway or agent orchestrator.

### Worked Rego policy

The rule: *a user may invoke a tier-1 model on a request classified `customer-pii` only if the user holds tier-2 clearance and the request originates from an approved workload identity.*

```rego
package aic.model_access

default allow := false

allow if {
    input.model.tier == "tier-1"
    input.request.data_classification == "customer-pii"
    input.user.clearance == "tier-2"
    input.request.workload_identity in data.approved_workloads
}

# Deny with a structured reason so the PEP can surface it to the caller.
deny_reason contains msg if {
    input.model.tier == "tier-1"
    input.request.data_classification == "customer-pii"
    input.user.clearance != "tier-2"
    msg := sprintf(
        "tier-1 model on customer-pii requires tier-2 clearance; user has %q",
        [input.user.clearance],
    )
}
```

`input` is the request context injected by the PEP; `data.approved_workloads` is the reference data loaded from the signed policy bundle. Rego's data model is dynamic — `input` is untyped JSON, and any missing field silently evaluates to `undefined`. That is a feature at authoring time and a footgun at review time; the test suite (see the testing section) is what stops the footgun.

### Worked Cedar policy

The same rule in Cedar:

```cedar
permit (
    principal in Clearance::"tier-2",
    action == Action::"invokeModel",
    resource in Model::"tier-1"
)
when {
    resource.tier == "tier-1" &&
    context.request.data_classification == "customer-pii" &&
    context.request.workload_identity in Workload::"approved"
};
```

With an accompanying schema fragment that declares the types:

```cedar-schema
entity User in [Clearance] { name: String };
entity Clearance;
entity Model { tier: String };
entity Workload;

action invokeModel appliesTo {
    principal: User,
    resource: Model,
    context: {
        request: {
            data_classification: String,
            workload_identity: Workload,
        },
    },
};
```

The two engines express the same rule. The interesting difference is the *schema*. Cedar's typed schema means the policy author declares the shape of principals, resources, and context up front; a policy that references a non-existent attribute fails to validate before it is ever deployed. Rego's dynamic data model gives more expressive freedom (arbitrary JSON in, arbitrary decision out) at the cost of catching shape errors only through the test suite or at runtime.

For AI-governance work the choice usually comes down to fit:

- **Cedar** is a better fit when the policy corpus is dominantly authorisation-shaped (principal / action / resource / context) and when strong static guarantees matter to the security review — for example, in a regulated context where the auditor wants to know the policy could not possibly evaluate an undeclared attribute.
- **OPA + Rego** is a better fit when the policy corpus spans authorisation *plus* admission control, config validation, egress classification, and CI gates — because the same engine and the same bundle format cover all of it, and the operational surface stays small.

Some organisations run both — Cedar for the authorisation slice, OPA for everything else. That is defensible; what is not defensible is running three or four different policy engines because different teams picked their favourite.

### The PDP / PEP split

Both engines occupy the **Policy Decision Point (PDP)** role: given a request and its context, they return a decision. Neither engine enforces anything on its own. The **Policy Enforcement Point (PEP)** is a separate component — the service-mesh sidecar, the Kubernetes admission-controller webhook, the API gateway, the model gateway, or the SDK — that intercepts the action, calls the PDP, and either lets the action through or rejects it.

The architect owns the PDP / PEP boundary as an interface contract:

- Every PEP calls exactly one PDP for a given decision domain (authorisation, egress, admission). No PEP evaluates policy inline.
- Every PDP returns a structured decision including a machine-readable reason code and a human-readable message, both of which the PEP surfaces to the caller and to the audit log.
- Every PDP evaluation is logged — request context, policy bundle version, decision, reason — into the evidence pipeline mod-108 designs.

Skip the split and you end up with policy logic scattered across a dozen microservices with no way to answer "what policy fired and why."

## CI/CD gate enforcement

Gate enforcement means the policy runs at build or deploy time, as a *gate* in the pipeline. The pipeline pauses, the gate evaluates, and on violation the pipeline fails with a message the engineer can act on. The failure mode is a red build, not a rejected request.

Gate enforcement is suitable for policies whose subject is an *artefact*, not a request:

- **Model-card completeness** — the deploy manifest references a model card whose required fields (intended use, evaluation results, limitations, training data description) are all present.
- **Evaluation-report presence** — an evaluation report for the specific model version being deployed exists in the evidence store and is signed by an authorised evaluator role.
- **Dataset-provenance record existence** — every training dataset referenced by the model has a provenance record in the registry (satisfying the mod-102 chapter 01 `AIC-DAT-*` evidence contract).
- **Licence compatibility** — every dependency (base model, adapter weights, retrieval corpus, library) has a licence compatible with the deployment target's licence policy.
- **ML-BOM signature** — the ML-BOM for the artefact is signed by a trusted key.

Gate policies are the natural home for anything the org wants to *guarantee about what ships*, as opposed to what happens at request time.

### Worked OPA gate policy

The same OPA engine that runs at request time runs perfectly well at gate time — a policy is just a function over structured input. The rule: *a deploy manifest for a tier-1 model must reference a signed evaluation report no more than 90 days old and a complete model card.*

```rego
package aic.deploy_gate

import future.keywords.every

deny contains msg if {
    input.model.tier == "tier-1"
    not input.model.evaluation_report.signed
    msg := "tier-1 deploy requires a signed evaluation report"
}

deny contains msg if {
    input.model.tier == "tier-1"
    age_days := (time.now_ns() - time.parse_rfc3339_ns(input.model.evaluation_report.signed_at)) / (1e9 * 60 * 60 * 24)
    age_days > 90
    msg := sprintf("evaluation report is %.0f days old; max is 90", [age_days])
}

required_card_fields := {
    "intended_use", "evaluation_results", "limitations",
    "training_data_description", "responsible_ai_review",
}

deny contains msg if {
    missing := required_card_fields - {k | input.model.model_card[k]}
    count(missing) > 0
    msg := sprintf("model card is missing required fields: %v", [missing])
}
```

Invoked from a GitHub Actions job (the same shape works for GitLab CI, Jenkins, Buildkite, or any other pipeline):

```yaml
name: aic-deploy-gate
on:
  pull_request:
    paths: [ "deploy/**" ]

jobs:
  policy-gate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install OPA
        run: |
          curl -L -o opa https://openpolicyagent.org/downloads/latest/opa_linux_amd64
          chmod +x opa
      - name: Fetch signed policy bundle
        run: |
          ./opa build --bundle policies/ --output bundle.tar.gz
          # In real use, pull a signed bundle from a trusted registry
          # and verify the signature before evaluating.
      - name: Evaluate deploy manifest
        run: |
          ./opa eval \
            --data bundle.tar.gz \
            --input deploy/manifest.yaml \
            --format pretty \
            --fail-defined \
            "data.aic.deploy_gate.deny"
```

Two design points on the gate side.

**Error messages are UX.** A gate policy that fails with `deny: true` and no explanation forces the engineer to open the policy source and reverse-engineer the intent. Every `deny` should return a structured message naming the rule, the offending field, and — where possible — the fix. Gate errors that read like error messages from a compiler get fixed; gate errors that read like Rego stack traces get bypassed.

**Local pre-commit parity.** Every gate policy that runs in CI should also run locally as a pre-commit hook, against the same bundle. Engineers should see the failure at the desk, not at the pull-request review. This does not mean the pre-commit hook *replaces* the CI gate — the CI gate is the authoritative enforcement, because the pre-commit hook can be skipped. It means the two evaluate the same policy against the same input so that the CI gate is never the first time an engineer sees a failure.

## Attestation-only enforcement

Attestation-only enforcement means *there is no machine enforcement*. An accountable role — named in the policy — signs a periodic attestation that the policy is being observed. The absence of a fresh attestation is itself the finding.

This is not a lesser tier. It is the correct tier for three classes of policy:

- **Human-process policies** — training completion, mandatory incident-review meetings held, quarterly risk-committee minutes recorded, RAI-review-board seat filled. There is nothing to enforce in the request path or the pipeline; what is being enforced is that a human process happened.
- **Expert-judgment policies** — fairness posture reviewed against use-case-appropriate metrics, Responsible AI review outcomes for a specific high-impact deployment, model-suitability assessment for a novel use case. The evaluation requires expert judgment; encoding it as a boolean is either impossible or dishonest.
- **Prohibitive-cost policies** — policies whose runtime enforcement would be technically possible but prohibitively expensive relative to the annual expected harm. A frequently cited example is exhaustive per-request bias evaluation on every generation; the runtime cost is enormous and the value over sampled evaluation plus a periodic attestation is small.

The shape of an attestation record is a small, signed document:

```yaml
attestation:
  id: att-2026-q3-rai-review-board
  policy_ref: POL-AIG-014          # Responsible AI review board convened quarterly
  scope:
    business_unit: consumer-ai
    period_start: 2026-04-01
    period_end: 2026-06-30
  signer:
    role: head-of-ai-governance
    name: <redacted>
    signed_at: 2026-07-05T14:22:00Z
  attestation_statement: >
    I attest that the Responsible AI review board met on the dates listed
    below, that quorum was met at each meeting, that meeting minutes were
    recorded, and that all tier-1 model deployments in the period were
    reviewed prior to production release.
  evidence_pointers:
    - urn:enterprise:evidence:rai-board-minutes/2026-04-15
    - urn:enterprise:evidence:rai-board-minutes/2026-05-13
    - urn:enterprise:evidence:rai-board-minutes/2026-06-17
    - urn:enterprise:evidence:rai-board-attendance/2026-Q2
  next_review_due: 2026-10-05
  signature:
    method: sigstore-cosign
    bundle: <base64-signature-bundle>
```

Three things that make an attestation record more than a rubber stamp:

- **Scope is explicit.** Business unit, period, systems in scope. An attestation whose scope is "the whole company forever" is not an attestation.
- **Evidence pointers are dereferenceable.** The attestation names the artefacts (minutes, attendance lists, review records) that back the statement. The auditor does not have to trust the signer's word; the signer is pointing at the trail.
- **The signature is cryptographic and role-bound.** The signing infrastructure is [Sigstore](https://www.sigstore.dev/) (cosign-style keyless signatures against an OIDC identity) or an equivalent. [in-toto](https://in-toto.io/) provides the attestation-envelope shape (a signed statement about a subject). [SLSA](https://slsa.dev/) provides the maturity model for supply-chain attestations that the same signing infrastructure supports. The point is that the attestation is an artefact that can be verified by any downstream tool without trusting the person's word.

Attestation-only is *nearly free at the developer level* — developers never see it. It is *expensive at the accountable-role level* — the head-of-AI-governance, the RAI review board chair, or whichever role signs actually has to do the work the attestation asserts, on a cadence, or their signature is a lie. Architects sometimes underweight this cost and pile attestation obligations onto a small number of senior roles until the signatures become perfunctory. When that happens the tier has failed.

## The five-question decision framework

Given a policy, the architect walks these five questions in order. Each question is a filter; the first "no" that ends in an attestation or gate outcome ends the walk.

**1. Is the violation observable at request time?**

If the violation only manifests days later (a fairness drift that becomes visible over a rolling window; a documentation obligation that matures at end of quarter), runtime cannot see it. Move to gate or attestation.

**2. Is the rule expressible as a boolean over structured inputs?**

If the evaluation genuinely requires expert judgment ("is this deployment's fairness posture appropriate for this use case?"), no engine can help you. Move to attestation. The temptation here is to *pretend* an expert judgment is a boolean by encoding a proxy — do not; the proxy will drift from the judgment and someone will optimise against it.

**3. Is a false positive tolerable in the developer path?**

If a false positive at runtime would reject a legitimate production request (paying user cannot complete purchase; agentic workflow silently fails), the tolerance is low. Prefer the gate tier, where a false positive fails a build (annoying) rather than a request (harmful). Runtime remains correct when the rule is precise enough that false positives are rare — authorisation, egress classification against a well-maintained data catalog, hard rate limits.

**4. Is enforcement cost less than annual expected harm from unenforced violations?**

Runtime enforcement has a cost — latency budget, PEP operational burden, ongoing rule maintenance. Gate enforcement has a smaller but real cost. Attestation has almost no machine cost. If the annual expected harm from *not* enforcing is smaller than the enforcement cost, move to attestation and accept the residual risk explicitly. This is where the architect uses the risk-tiering work from mod-104 as input.

**5. Does the policy need to be updated more frequently than the platform's deploy cadence?**

If the rule changes weekly (a rapidly evolving blocklist, an egress classification that follows a data-catalog release cadence) and the platform deploys quarterly, gate-only enforcement means the policy is quarterly-fresh at best. Move to runtime *with hot-reload* against a signed policy bundle so the policy can update independently of the platform.

The framework is deliberately ordered. Questions 1 and 2 kick a policy out of runtime and into a lower tier. Question 3 kicks it out of runtime into the gate. Question 4 checks that enforcement is worth its cost at all. Question 5 pulls a policy *back* into runtime when the change cadence demands it.

## Developer-experience cost analysis

The DX cost profile of each tier deserves to be spelled out, because underestimating it is the most common way policy-as-code programmes fail.

**Runtime — highest DX cost per policy.** Every runtime policy adds latency to the request path (usually low milliseconds per PDP call, occasionally more), a failure mode in the request path (a PDP outage now becomes a request-path outage unless the PEP has a safe-fail posture the architect has approved), and an operational burden (the PDP needs monitoring, alerting, on-call, capacity planning). The debugging experience must be on par with the rest of the request path — structured logs, trace propagation, decision reasons surfaced to the caller. Every new runtime policy compounds this cost. A ten-policy runtime slice is manageable; a two-hundred-policy runtime slice is a distributed system in its own right and needs a team.

**Gate — medium DX cost per policy.** Every gate policy adds pipeline time (usually seconds to tens of seconds), requires clear error messages, and requires local pre-commit parity so engineers see failures before pull-request review. The failure mode is a red build, not a rejected user request — annoying, not harmful. Gate policies that surface confusing errors will be routed around ("skip the gate for this hotfix, please"), so gate quality is largely gate-message quality. Gate policies also decay: a policy that was correct against the manifest shape of 2025 is subtly wrong when the manifest shape changes in 2026, and no user complains, so it silently permits the wrong thing.

**Attestation — nearly free at the developer level, expensive at the accountable-role level.** Developers never see attestations. The role that signs them, though, is doing real work — reviewing evidence, confirming meetings happened, forming an expert judgment. If a single role accumulates twenty quarterly attestations, the signatures become perfunctory and the tier fails. Architects must budget attestation load per accountable role and refactor when it becomes a rubber-stamping ritual.

Cross-tier: every tier's evidence flows into the mod-108 evidence architecture. Runtime decisions log; gate outcomes emit a build record; attestations are themselves signed evidence. The GRC-for-AI platform from mod-111 is what stitches those three streams into a single posture view for a given control.

## Testing policies-as-code

Every runtime and gate policy ships with a test suite. This is not optional. The library entry from mod-102 chapter 01 whose evidence contract points at a policy also requires the test-suite artefact as evidence.

- **Rego** ships `opa test`, a first-class test runner that evaluates test cases (`test_*` functions in `.rego` files) against fixture inputs and asserts on decisions. A production Rego bundle without a test suite is a finding.
- **Cedar** ships schema-based test cases: given a policy, a schema, and a set of authorisation requests with expected decisions, the Cedar CLI validates that the policy behaves as expected. Cedar's typed schema catches a class of authoring errors *before* the test suite runs; the test suite is still required for behavioural correctness.

The test-suite artefact is what an assessor examines to confirm that the enforced policy actually implements the written policy. Without it, the assessor is trusting that the deployed bundle matches the intent, which is not a defensible audit posture.

At minimum, every policy has:

- A *permit* test per authorised path (positive coverage).
- A *deny* test per rule (negative coverage).
- A regression test per bug ever found in the policy (guarding against reintroduction).

The test suite versions with the policy bundle; the release contract from `05-policy-change-communications-and-deprecation-windows.md` requires both.

## Policy version pinning

Production PEPs must pin to a signed policy-bundle version. Unpinned production ("PEP pulls the latest bundle from the registry on start") is a finding, for the same reason unpinned dependencies are a finding: the running behaviour of the enforcement point becomes non-deterministic and cannot be reproduced.

The pinning discipline mirrors the mod-102 chapter 04 catalog-release discipline:

- Policy bundles are signed (Sigstore or equivalent) and published to an immutable location with a version identifier.
- PEP configuration references a specific bundle version.
- Bundle upgrades are a deploy event with a rollback path.
- Hot-reload of bundles (question 5 in the decision framework) is *within the pinned version's lineage* — the bundle can be swapped for a newer signed version by a controlled orchestrator, but the PEP does not simply follow `latest`.

Change communications for policy-bundle updates follow `05-policy-change-communications-and-deprecation-windows.md`; the exception path when a system cannot immediately consume a new bundle version follows `04-exception-and-waiver-workflow.md`.

## Two common shape mistakes

**Mistake 1 — Encoding the entire policy document in Rego.** A policy document is prose written for humans (see `01-ai-policy-hierarchy-authority-and-cadence.md`). The Rego (or Cedar) implementation is a *specific decision function* that operationalises one testable clause of that document. Trying to encode the whole document — its scope statement, its ownership, its review cadence, its rationale — as Rego yields a 3,000-line unmaintainable file whose relationship to the signed policy document is opaque. The correct decomposition: the policy document lives in the policy repository; each enforceable clause maps to a distinct Rego (or Cedar) module with its own tests; the traceability from clause to module lives in the traceability matrix from `02-principle-to-policy-to-standard-to-control-traceability.md`.

**Mistake 2 — Putting policies at the gate tier that should have been runtime.** The gate blocks deploys. Once the artefact is deployed, the gate is silent — nothing checks it again. A rule like "customer PII may not reach a third-party embedding endpoint" implemented only as a gate check on the deploy manifest (via a static analysis of code paths, for example) is bypassed the moment a new code path is added by a hotfix that skips the gate, or the moment the manifest becomes stale relative to the running system. Rules about *what happens at request time* belong at runtime, not at the gate. The gate is for rules about *what shipped*.

## Summary

The policy-as-code slice has three tiers — runtime (OPA + Rego, or Cedar), CI/CD gate, and attestation-only — and every policy in the corpus belongs at exactly one. Runtime enforcement fits authorisation, egress, quota, and guardrail routing; gate enforcement fits artefact-shape rules like model-card completeness and evaluation-report presence; attestation fits human-process and expert-judgment obligations. The five-question decision framework places a policy by asking whether the violation is observable at request time, whether the rule is a boolean over structured inputs, whether false-positive tolerance is high enough for runtime, whether enforcement cost is worth the harm avoided, and whether the update cadence exceeds the platform deploy cadence. The developer-experience cost rises steeply from attestation to gate to runtime, and the architect budgets that cost as carefully as they budget latency or infrastructure spend. Every runtime and gate policy ships with a test suite; every production PEP pins to a signed policy-bundle version. Get placement right and the enforcement slice is a lever the org can actually pull; get it wrong and you either burn developer velocity forever or leave real risk uncovered behind a signature nobody reads.
