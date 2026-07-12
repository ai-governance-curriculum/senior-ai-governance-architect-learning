# exercise-01: Level Ladder Scope Map

**Estimated effort:** 2 hours

## Objective

Produce a **scope map** that pins the senior AI governance architect (level 50) role against the six adjacent AI Governance family seats — `ai-governance-analyst` (15), `ai-risk-engineer` (25), `ai-evaluation-engineer` (35), `agentic-safety-engineer` (40), `head-of-ai-governance` (60), `chief-ai-officer` (70) — so that every governance activity you encounter in the rest of the track has an unambiguous owner.

The deliverable is a triage tool you will *use*, not documentation you will *shelve*. It should let you route a request in under a minute.

## Prerequisites

- Chapter [`01-level-ladder-and-architectural-authority.md`](../01-level-ladder-and-architectural-authority.md) read once.
- Skim of [`CURRICULUM.md`](../../../CURRICULUM.md) — the ownership rule, and each module summary in one pass.
- Skim of [`PREREQUISITES.md`](../../../PREREQUISITES.md) — especially the "assumed lower-level curriculum" and "not in scope for this track (linked out)" sections.

## Deliverables

Author two artefacts in a working directory of your choice (Markdown is fine; you can also produce them as a single Notion / Confluence page in real work):

1. **`role-scope-map.md`** — a scope table plus a triage flow.
2. **`role-scope-map-worked-examples.md`** — six realistic requests, each routed to a role with a one-sentence justification.

## Requirements

### `role-scope-map.md`

Must contain **all** of the following:

- **Ladder table** covering levels 15, 25, 35, 40, **50 (this role)**, 60, 70. For each, list: role name, one-line scope statement, three canonical deliverable examples the role owns, and one canonical deliverable it does *not* own that could be confused with its scope.
- **Boundary diagram** (ASCII art, Mermaid, or a rendered PNG committed alongside) showing this role's *design-authority* scope, the *hand-off surfaces* to the roles above and below, and — critically — the three most-common scope-slip directions (into level 15 analyst work, into level 25 engineering craft, into level 60 leadership work).
- **Triage flow** in three questions (chapter 01 gives the template) that any incoming request runs through. The output of the triage flow must be a named role.
- **Named soft-boundary artefacts** — at least three artefacts where you own the *schema / contract* and an adjacent role owns the *content / execution*. Name both owners and the artefact they each own.

### `role-scope-map-worked-examples.md`

Six worked routing examples. Each must be:

- A **one-paragraph situation** phrased as if a stakeholder walked into your office ("The CISO's team wants us to review this LLM-based intake tool that HR is planning to deploy. Where should this go first?").
- The **triage flow answers** on your three questions.
- The **routed role**, and — if the situation implicates more than one role — the *primary* owner and any *joint* owners.
- **One-sentence justification** citing the specific ownership rule from your scope table.

The six examples must cover, at minimum:

1. A request that is clearly *below* this role (analyst or engineer scope).
2. A request that is clearly *above* this role (head-of-governance or CAO scope).
3. A request that lands squarely in *this* role's scope.
4. A request that implicates the sideways-and-below evaluation-engineer role at level 35.
5. A request that looks like this role but is really the sibling agentic-safety-engineer role at level 40.
6. A soft-boundary request where the answer is joint ownership with a named tiebreaker.

## Starter guidance

- Write the ladder table before any diagram. If the table cannot be filled in, the diagram will be prettier but wrong.
- Draw the boundary diagram *last*. The diagram is the compressed representation of the table — if you draw it first, you will make the table match the diagram instead of matching the ownership rule.
- The triage flow should be exactly three questions, in the order chapter 01 gives them. Do not add branches. If a real request needs more branches, your ladder table needs more columns — fix the table.
- Do not use any AI system names or vendor product names in the worked examples. Keep the examples abstract enough that they generalise to any enterprise.

## Acceptance criteria

- [ ] Ladder table covers all seven levels (15, 25, 35, 40, 50, 60, 70) with all four columns filled per role.
- [ ] Boundary diagram is present, is legible without the table, and calls out the three scope-slip directions.
- [ ] Triage flow is exactly three questions, ordered, deterministic — no "it depends" branches.
- [ ] At least three soft-boundary artefacts are named, each with two owners and an explicit split.
- [ ] Six worked examples are present and cover all six required categories.
- [ ] No worked example uses AI system names or vendor product names.
- [ ] The scope map is one page rendered (both files combined ≤ 2 pages of prose + one diagram). If it exceeds this, cut — the tool has to fit on the desk.

## Stretch goals

- Add a **historic mistake column** to the ladder table: for each of the six adjacent roles, name one canonical way this role *has* over-reached (from published memoirs, post-mortems, or your own experience) and the artefact that would have prevented it.
- Author a *fourth* worked example under category (6) — a soft-boundary situation where the joint owners come from *different* families (e.g. AI Governance and Security), to test the ladder against cross-family collaboration.
- Produce a version of the ladder table filtered for a specific sector (banking, health, or public sector) where the sibling framework language (SR 11-7 for banking, FDA GMLP for health, OMB M-25-21 for public sector) has renamed the seats. Note where the naming differs.
