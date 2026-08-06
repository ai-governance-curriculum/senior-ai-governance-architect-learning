# mod-104-multi-jurisdiction-reconciliation: Cross-Jurisdiction Requirement Reconciliation Architecture

**Estimated effort:** 18 hours

## Why this module exists

Every AI system the enterprise ships is subject to more than one regulatory regime — EU AI Act, GDPR, US federal executive orders and OMB memoranda, US state statutes (Colorado, California, Illinois, Utah, Texas, ...), city ordinances (NYC LL 144), and the international patchwork (UK, Canada, China, Singapore, Australia, India, Korea, Brazil). The naïve approach — one compliance programme per regime — fails within a quarter. The architect's job is to design the *reconciliation architecture* that lets one control library discharge the union of obligations across the regimes the enterprise operates in, with per-jurisdiction applicability filters and per-obligation evidence renderings, and to plan for the future-state simplification the CEN-CENELEC JTC 21 presumption-of-conformity pathway provides.

## Learning objectives

- Read the EU AI Act (Regulation 2024/1689) as an architectural input — Articles 6-15 (high-risk obligations), 16-29 (provider / deployer duties), 50 (transparency), 51-56 (GPAI systemic-risk), 61 (registration), 72 (post-market monitoring), 73 (serious-incident reporting) — and locate each obligation in the enterprise control library.
- Layer GDPR Article 22, the Council of Europe Framework Convention on AI, OECD AI Principles, UNESCO Recommendation on the Ethics of AI, and the G7 Hiroshima Process Code of Conduct as cross-cutting international obligations.
- Read the US federal frame — Executive Order 14179 (with EO 14110 as historical context), OMB M-24-10 / M-24-18 / M-25-21 / M-25-22, US AISI methodology — and design the federal-facing crosswalk.
- Read the US state and municipal patchwork — Colorado AI Act, NYC LL 144, EEOC AI guidance, California CPPA ADMT + SB 942 + AB 2013, Utah SB 149, Texas HB 149 (TRAIGA), Illinois HB 3773, plus emerging Virginia / Connecticut / Washington activity — and design the multi-state overlay.
- Read the international patchwork — UK Pro-Innovation Approach + AISI methodology + ATRS; Canada AIDA + TBS Directive on Automated Decision-Making; China Interim Measures for GenAI + TC260 Basic Safety Requirements; Singapore Model AI Governance Framework + AI Verify; Australia Voluntary AI Safety Standard + Guardrails; India DPDPA + NITI Aayog RAI; Korea AI Basic Act; Brazil PL 2338/2023 — and design the multi-country overlay.
- Design the reconciliation architecture — for each obligation, name the parent control in the library, the applicability filter (jurisdiction / product / use case), the evidence contract, and the deprecation path when regulation changes.
- Position the CEN-CENELEC JTC 21 harmonised standards programme (presumption of conformity for the EU AI Act) as the future-state architecture target.

## Chapters

1. [`01-the-reconciliation-problem-and-the-architects-lens.md`](01-the-reconciliation-problem-and-the-architects-lens.md) — Why cross-jurisdiction reconciliation is a distinct architecture problem, the four axes every obligation moves along, the two shapes reconciliation takes on any given control, and the three consequential design choices made once at library-preface time.
2. [`02-reading-the-eu-ai-act-as-architectural-input.md`](02-reading-the-eu-ai-act-as-architectural-input.md) — Regulation (EU) 2024/1689 walked article group by article group, mapped onto the enterprise control library. Articles 6-15 (high-risk), 16-29 (provider / deployer / notified-body), 50 (transparency), 51-56 (GPAI), registration and post-market and incident-reporting.
3. [`03-cross-cutting-international-obligations.md`](03-cross-cutting-international-obligations.md) — GDPR Article 22, Council of Europe Framework Convention, OECD AI Principles, UNESCO Recommendation, G7 Hiroshima Code — the hard-and-soft layer beneath the jurisdictional regimes.
4. [`04-the-us-federal-frame.md`](04-the-us-federal-frame.md) — EO 14179 (with EO 14110 historical context), OMB M-24-10 / M-24-18 / M-25-21 / M-25-22, US AISI methodology, and the sector regulators (FTC, EEOC, HHS/OCR, financial-services agencies) that always run alongside.
5. [`05-the-us-state-and-municipal-patchwork.md`](05-the-us-state-and-municipal-patchwork.md) — Colorado, NYC LL 144, EEOC, California (CPPA ADMT, SB 942, AB 2013), Utah, Texas TRAIGA, Illinois, plus the watch list on Virginia / Connecticut / Washington.
6. [`06-the-international-patchwork.md`](06-the-international-patchwork.md) — UK, Canada, China, Singapore, Australia, India, Korea, Brazil — with the six design decisions the multi-country overlay makes once.
7. [`07-designing-the-reconciliation-architecture.md`](07-designing-the-reconciliation-architecture.md) — The four artefacts: the obligation record, the applicability filter, the evidence contract per attached obligation, and the deprecation-path state machine. Includes YAML schemas and a worked example.
8. [`08-cen-cenelec-jtc-21-and-the-future-state.md`](08-cen-cenelec-jtc-21-and-the-future-state.md) — The harmonised standards programme, the presumption-of-conformity mechanism, the transition path from per-obligation evidence to standards-conformance evidence, and the parallel non-EU tracks (SC 42, NIST, UK AISI, Singapore AI Verify, China TC260) to watch.

## Structure

- `01-…md` … `08-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs.
- `quizzes/`: knowledge checks.
- `resources.md`: external references.
