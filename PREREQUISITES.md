# Prerequisites — Senior AI Governance / Risk Architect track

**Role level:** 50 (architectural design authority, AI Governance family)
**Track:** `senior-ai-governance-architect-learning`

This track is an architectural design-authority specialty. It assumes the learner arrives with the operational analyst legwork and the hands-on AI-risk engineering craft already fluent — so the modules can step directly onto control-library architecture, policy-taxonomy design, AIMS scope authoring, cross-jurisdiction reconciliation, and GRC-for-AI reference architecture. Analyst legwork and engineering craft are **not** re-taught here.

## Assumed lower-level curriculum

Complete or be practically fluent in the equivalent of these lower-level tracks before starting:

- **[`ai-infra-junior-engineer-learning`](https://github.com/ai-infra-curriculum/ai-infra-junior-engineer-learning) — level 10** — engineering-craft prerequisites (Linux, Git, Python packaging, HTTP APIs, unit testing, logging, docker basics). Enough to reason about what happens at the *runtime* of the systems the architect governs, not to build them.
- **[`ml-engineer-learning`](https://github.com/ml-engineering-curriculum/ml-engineer-learning) — level 20** — classical ML with sklearn, deep learning with PyTorch and Hugging Face Transformers, evaluation with standard metrics, packaging with FastAPI + Docker, experiment tracking with MLflow / Weights & Biases. Enough to reason about which ML system a given control applies to.
- **[`ai-governance-analyst-learning`](https://github.com/ai-governance-curriculum/ai-governance-analyst-learning) — level 15** — operational analyst work: intake, inventory, framework crosswalks, first-draft impact assessments, model / system / dataset cards at analyst tier, control-tracking, jurisdictional tracking, reading eval / audit evidence into governance records. The architect *designs the schemas + templates* this role executes against; understanding the analyst workflow inside-out is required to design defensible schemas.
- **[`ai-risk-engineer-learning`](https://github.com/ai-governance-curriculum/ai-risk-engineer-learning) — level 25** — the hands-on engineering craft of AI risk: harm-model authoring, risk quantification, LLM / agent red-team engineering, adversarial-ML engineering, fairness / bias / explainability engineering, privacy-risk engineering, guardrail engineering + effectiveness measurement, monitoring-into-risk-register integration, MRM / supply-chain assurance engineering, AI-specific incident response. The architect *names the controls that role builds*; you must know the craft well enough to review deliverables and sign off, without needing to author them.

## Assumed working knowledge

Even without holding the equivalent role formally, you should be practically fluent in:

- **Enterprise architecture practice** — TOGAF or Zachman shape target-state architecture, capability modelling, reference-architecture authorship, technical-architecture-document (TAD) / RFC writing, architecture-review-board (ARB) facilitation. You should be able to author a target-state diagram + written architecture that stakeholders across CISO / GC / CIO / CFO / audit committee scope can each read without a translator.
- **Enterprise risk management vocabulary** — ISO 31000 / ISO/IEC 27005 / COSO ERM shape (risk taxonomy, appetite, tolerance, treatment, aggregation), IIA Three Lines Model, COBIT 2019 — enough that the AI risk architecture composes with the enterprise ERM without a schema fight.
- **Information-security management-system fluency** — ISO/IEC 27001 ISMS shape (scope, statement of applicability, risk-treatment plan, internal audit, management review) — the AIMS reuses the same shape. If you have not seen an ISMS from the inside, plan to read the ISO/IEC 27001 standard front-to-back before starting mod-105.
- **Regulatory reading** — comfortable reading primary regulatory text (EU AI Act, US federal regulation, US state statutes, OMB memoranda, sector regulator letters) without a summary intermediary. Comfortable reading standards (NIST / ISO / IEC / IEEE / CEN-CENELEC) at a level sufficient to author a compliant control library.
- **Policy-as-code literacy** — comfortable reading Rego (OPA) and Cedar policies; comfortable evaluating where policy is best enforced (runtime vs CI/CD gate vs attestation-only).
- **GRC platform fluency** — comfortable navigating at least one enterprise GRC platform (Archer, ServiceNow GRC, MetricStream, LogicGate) and at least one AI-specific governance platform (Credo AI, Holistic AI, ModelOp, Monitaur, ServiceNow AI Control Tower, IBM watsonx.governance).
- **Stakeholder communication** — comfortable running a cross-functional architecture-review-board session with senior stakeholders present; comfortable packaging position papers for the head of AI governance to take to regulators; comfortable briefing an audit committee.
- **AI security threat vocabulary** — OWASP LLM Top 10, OWASP ML Top 10, MITRE ATLAS, Google SAIF, CISA/NCSC Secure AI System Development, ENISA AI Threat Landscape — at the level required to compose them into a control library, not to attack a model.

## Not assumed, taught here

You do **not** need prior experience with:

- **The full AI standards stack read as an architectural input** — NIST AI RMF + Playbook + GenAI Profile, ISO/IEC 42001 / 42005 / 42006 / 23894 / 38507 / 22989 / 23053 / 24028 / 25059 / 27001 / 27005, ISO 31000, NIST SP 800-53 / 800-37 / CSF 2.0 / OSCAL, IEEE 7000-series. Mod-101 covers the landscape and the positioning.
- **Control-library architecture at enterprise scale for AI** — mod-102 covers the design end-to-end, including the OSCAL representation and the control-inheritance model.
- **AI policy taxonomy + policy-as-code composition** — mod-103 covers the hierarchy design, the OPA/Rego and Cedar policy-as-code slices, and the exception / waiver workflow.
- **Cross-jurisdiction requirement reconciliation architecture** — mod-104 covers the EU + US federal + US state + UK + Canada + China + Singapore + Australia + India + Korea + Brazil overlays and the reconciliation schema.
- **ISO/IEC 42001 AIMS architecture certifiable under ISO/IEC 42006** — mod-105 covers scope, statement of applicability, risk-treatment plan, internal audit, management review, plus the ISO/IEC 27001 ISMS integration and ISO/IEC 42005 impact-assessment integration.
- **Enterprise AI risk taxonomy + appetite + aggregation** — mod-106 covers the taxonomy design, the risk-appetite translation, and the portfolio-aggregation model.
- **AI assurance architecture** — three lines of defense for AI, pre-deployment assurance, ongoing assurance, third-line audit — mod-107 covers the architecture end-to-end.
- **AI evidence architecture** — model / system / dataset / risk-card schemas, audit-log architecture, ML-BOM + SPDX AI + SLSA + Sigstore at enterprise scale, regulator-facing artifact templates, OSCAL representation. Mod-108 covers the design end-to-end.
- **Third-party + AI-supply-chain governance program design** — SR 23-4 + OMB M-24-18 / M-25-22 shape. Mod-109 covers tiering, due-diligence, contract templates, and ongoing monitoring.
- **Enterprise post-market surveillance architecture** — EU AI Act Article 72 + 73 at enterprise scale. Mod-110 covers the architecture and the observability-platform wiring.
- **GRC-for-AI reference architecture + build-vs-buy** — mod-111 covers the reference architecture, the vendor-evaluation matrix, the workflow layer, and the RBAC + SoD model.
- **AI governance operating model + org shape** — mod-112 covers the council charter, the three-lines-of-defense RACI, the role descriptions linked to the correct owner packet, and the certifications portfolio.
- **Sector + jurisdiction blueprints** — banking, insurance, health, pharma, public sector, critical infrastructure. Mod-113 covers the six blueprints and the sector-adaptation methodology.

## Recommended reading before starting

- [NIST AI Risk Management Framework 1.0 (AI 100-1)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf) — front-to-back once.
- [NIST AI RMF Generative AI Profile (AI 600-1)](https://airc.nist.gov/AI_RMF_Knowledge_Base/AI_RMF/Uses/GenAI-Profile) — front-to-back once.
- [ISO/IEC 42001:2023 — Artificial intelligence management system](https://www.iso.org/standard/81230.html) — front-to-back once (paywalled — get through your enterprise's ISO subscription if possible).
- [ISO/IEC 27001:2022](https://www.iso.org/standard/82875.html) — if you have not seen an ISMS from the inside, read the ISO/IEC 27001 standard first; the AIMS reuses the same shape.
- [EU AI Act (Regulation 2024/1689)](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — read Articles 6-15, 16-29, 50, 51-56, 61, 72, 73 verbatim.
- [NIST SP 800-53 rev5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) + [OSCAL overview](https://pages.nist.gov/OSCAL/) — the reference control-catalog shape + machine-readable representation the AI control library composes with.
- [IIA Three Lines Model](https://www.theiia.org/en/content/position-papers/2020/the-iia-three-lines-model/) — foundational for the assurance architecture in mod-107.
- [US Federal Reserve SR 11-7](https://www.federalreserve.gov/supervisionreg/srletters/sr1107.htm) + [OCC Bulletin 2011-12](https://www.occ.gov/news-issuances/bulletins/2011/bulletin-2011-12.html) — foundational for validation-vs-development independence used throughout.
- [MIT AI Risk Repository](https://airisk.mit.edu/) — the reference meta-taxonomy for enterprise AI risk taxonomy design.
- Two or three recent public system cards (a Claude, GPT, or Gemini disclosure) + [Anthropic RSP](https://www.anthropic.com/rsp) + [OpenAI Preparedness Framework](https://openai.com/safety/preparedness/) — read them as an architect would, tracing each claim to an evidence artifact and each tier to a control-library entry.

## Not required, useful to have

- Prior IAPP AIGP certification is the most-cited signal in senior AI governance / risk architect postings; earning it before starting is not required but reduces the mod-101 lift substantially. [IAPP AIGP](https://iapp.org/certify/aigp/).
- Prior ISO/IEC 42001 Lead Implementer or Lead Auditor training makes mod-105 much faster.
- Prior audit-side experience (Big Four, ForHumanity, BABL AI, internal audit function) makes mod-107 much faster.
- Prior enterprise-architecture certification (TOGAF, Zachman) or comparable practical experience makes mod-101 and mod-111 faster.
- Familiarity with one or more GRC-for-AI platforms (Credo AI, Holistic AI, ModelOp, Monitaur, ServiceNow AI Control Tower, IBM watsonx.governance) before starting mod-111.

## Not in scope for this track (linked out)

- **Hands-on AI risk engineering craft** — harm-model authoring, red-team / adversarial-ML / fairness / privacy / guardrail engineering, quantification, MRM engineering, incident RCA — owned by [`ai-risk-engineer-learning`](https://github.com/ai-governance-curriculum/ai-risk-engineer-learning) (level 25). The architect reviews and signs off; that role builds.
- **Operational analyst legwork** — intake, inventory, framework-crosswalk drafting, first-draft impact assessments, model-card authoring at analyst tier, control-tracking, jurisdictional tracking — owned by [`ai-governance-analyst-learning`](https://github.com/ai-governance-curriculum/ai-governance-analyst-learning) (level 15). The architect designs the schemas + templates; that role executes.
- **Application-layer LLM/agent evaluation-engineering depth** — owned by [`ai-eval-engineer-learning`](https://github.com/ai-engineering-curriculum/ai-eval-engineer-learning) (peer, level 30, AI Engineering family).
- **Statistical / benchmark / judge-vs-human calibration methodology depth** — owned by [`model-evaluation-engineer-learning`](https://github.com/ml-engineering-curriculum/model-evaluation-engineer-learning) (peer, level 30, ML Engineering family).
- **MLOps automation into which the governance-runtime slice plugs** — owned by [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning) (peer, level 25).
- **Deep ML/AI security at platform scale** — inference-edge adversarial defence, model-extraction defence, MLSecOps, judge supply-chain, eval-set exfiltration prevention — owned by [`security-learning`](https://github.com/ai-infra-curriculum/security-learning) and [`ai-infra-security-learning`](https://github.com/ai-infra-curriculum/ai-infra-security-learning) (level 35).
- **Release-assurance / audit-trail / regulator-facing methodology depth** — owned by [`ai-evaluation-engineer-learning`](https://github.com/ai-governance-curriculum/ai-evaluation-engineer-learning) (peer / lower specialist, level 35, Governance family). The architect designs the assurance system; that peer executes inside it.
- **Frontier-agent red-team methodology and dangerous-capability evaluation depth** — owned by [`agentic-safety-engineer-learning`](https://github.com/ai-governance-curriculum/agentic-safety-engineer-learning) (level 40). The architect defines the tripwire / rollback contract; that peer owns the methodology.
- **Program leadership, board-level reporting, regulator engagement** — owned by [`head-of-ai-governance-learning`](https://github.com/ai-governance-curriculum/head-of-ai-governance-learning) (level 60). The architect hands operating design; that role leads.
- **AI strategy, P&L alignment, external positioning** — owned by [`chief-ai-officer-learning`](https://github.com/ai-governance-curriculum/chief-ai-officer-learning) (level 70).
- **Legal opinion** — out of scope. The architect packages regulatory obligations for counsel to interpret; counsel delivers the legal opinion.
- **SOC-side incident handling** — out of scope. The architect designs the interface between AI-specific signal and the SOC; SecOps runs the runbook.
