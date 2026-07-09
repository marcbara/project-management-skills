---
name: project-charter
description: >
  Generate and review sponsor-ready project charters from briefings, stakeholder
  inputs, or messy initial documentation. Use when starting a new project,
  recovering a troubled project, or when a sponsor needs a decision document
  that honestly frames scope, risks, stakeholders, and constraints.
---

# Project Charter

## Context

The project charter is the most important document a PM produces — and the one AI gets most dangerously wrong. LLMs default to producing clean, balanced, professional-sounding charters that present every project as orderly and manageable. This is exactly backwards. A charter exists to surface the hard truths early: unresolved scope conflicts, political tensions between stakeholders, unrealistic timelines, and budget gaps that will kill the project if ignored.

A charter that sanitizes reality does not protect the PM — it destroys their credibility in the first steering committee meeting when the real situation surfaces. The value of a charter is not in looking complete. It is in being honest.

Most PMs also confuse the charter with the project plan. A charter authorizes the project and frames key decisions. It does NOT include a WBS, detailed task breakdowns, resource allocation tables, or procurement plans. If a charter exceeds 8 pages, it is the wrong document.

## Commands

### `/charter`

Generate a complete project charter from a project briefing or set of inputs.

**Before generating anything, ask the user:**
- What is the project briefing or source material? (paste or describe)
- Who is the sponsor and what do they need to decide?
- Are there any known political dynamics or stakeholder tensions?
- What is the hard deadline, if any, and what drives it?

**The charter must follow these rules:**

1. **Never sanitize.** If the project is in trouble, the charter says so. If there are unresolved political conflicts, the charter names them. Every sentence must carry specific, actionable information about THIS project.

2. **Extract, don't invent.** Every claim must come from the briefing or be a logical inference from it. If information is missing, mark it as "TO BE CONFIRMED" — never fabricate a plausible-sounding answer.

3. **Separate facts from decisions.** Distinguish between what has been decided, what is assumed, and what still needs a decision. Mark unresolved items as **DECISION REQUIRED** with who decides and by when.

4. **Write for the sponsor, not for a textbook.** The sponsor needs to understand THIS project's situation in 10 minutes. Cut every sentence that does not carry project-specific information.

**Charter sections:**

1. **Project Purpose & Justification** — Why this project exists and why it matters NOW. Connect to strategic context, hard deadlines, and current state (fresh start vs. recovery).

2. **Objectives & Success Criteria** — 4-6 measurable objectives. Each states what is measured, the target, and the deadline. At least one must address stakeholder perception. "Deliver on time and on budget" is a platitude, not a criterion.

3. **Scope Statement** — Three parts: IN SCOPE (concrete deliverables), OUT OF SCOPE (items someone might assume are included but are not), and SCOPE UNDER NEGOTIATION (unresolved scope decisions with competing options, who advocates for each, trade-offs, and a decision deadline). See `/scope-statement` in the scope-management skill for detailed scope work.

4. **Key Stakeholders** — For each: name/role, real interest (which may differ from official position), influence level with reasoning (budget authority? veto power? public platform?), potential conflicts with other stakeholders, and concrete risk if mismanaged. Identify the real decision-maker (may not be the official sponsor). Flag sidelined stakeholders — they are often the most dangerous.

5. **Key Risks** — Top 5-8 risks ranked by actual impact on THIS project. For each: clear risk statement, likelihood with reasoning, impact across dimensions (schedule, budget, political, legal), a SPECIFIC response strategy (not a generic verb — "mitigate" is not a strategy), owner, and deadline. Include political and stakeholder risks. Quantify financial risks with numbers. Note risk interdependencies.

6. **High-Level Timeline & Milestones** — Work BACKWARD from the hard deadline. 6-10 milestones. State the logic chain: "If X must be done by Date Y, then Z must be decided by Date W." Flag critical path decisions that are currently blocking progress. Be honest if the timeline is unrealistic.

7. **Budget Summary** — Total budget with funding sources and conditions, amount committed/spent, contingency (original, used, remaining), known unbudgeted costs, and financial risk exposure. Show the math. The sponsor needs to see the gap between available and needed.

8. **Authority & Governance** — Reporting line (address ambiguity or dual-reporting directly), decision authority boundaries, escalation path for each major unresolved decision, meeting cadence.

**Format:** Professional, direct. Tables for stakeholders, risks, budget, milestones. Mark every unresolved item with **DECISION REQUIRED** and a deadline. Target 4-8 pages.

### `/charter-review`

Review an existing project charter and identify weaknesses.

**Check for these failure patterns:**
- Sanitized language that hides problems ("challenges" instead of naming the actual risk)
- Unmeasurable objectives ("ensure alignment", "deliver quality results")
- Scope presented as clean and settled when it clearly is not
- Missing "scope under negotiation" section
- Flat stakeholder table (Name/Role/"Keep informed") with no conflict or consequence analysis
- Generic risks from a textbook instead of project-specific risks
- Risk responses that are verbs ("mitigate", "monitor") instead of action plans
- Budget shown as a single number with no pressure points
- Timeline that does not work backward from the hard deadline
- Charter that is actually a project plan (too long, too detailed, includes WBS or task breakdowns)
- Invented information not traceable to source material

**Output:** A structured assessment with specific findings, severity (critical / significant / minor), and recommended fixes for each issue.

### `/stakeholder-map`

Produce a deep stakeholder analysis for a project.

**Ask the user for:** Project description, known stakeholders, any political dynamics they are aware of.

**For each stakeholder, analyze:**

| Field | What to capture |
|-------|----------------|
| Name / Role | Who they are |
| Stated position | What they officially want |
| Real interest | What they actually want (may differ from stated position) |
| Influence source | WHY they have influence: budget authority, veto power, political connections, public platform, technical expertise, organizational seniority |
| Influence level | High / Medium / Low — justified by the source above |
| Allies & opponents | Which other stakeholders they align or conflict with, and over what |
| Risk if mismanaged | What happens concretely if this stakeholder is ignored or antagonized |
| Engagement strategy | Specific actions, not categories. "Schedule bi-weekly 1:1 to surface concerns before they become public objections" — not "manage closely." |

**Rules:**
- Identify the REAL decision-maker, who may not be the official sponsor
- Flag anyone who has been sidelined or bypassed — they have motivation to undermine
- Include external stakeholders (community, regulators, media, end users) — not just the org chart
- Surface the political dynamics: who is competing for credit, who feels threatened, who is trying to expand their influence through this project

### `/risk-snapshot`

Generate a charter-level risk assessment.

**Ask for:** Project context, known constraints, stakeholder landscape.

**For each risk (5-8, ranked by impact on THIS project):**

| Field | Content |
|-------|---------|
| Risk | Clear statement of what could go wrong |
| Likelihood | High / Medium / Low — with reasoning from the project context |
| Impact | What happens if this materializes — across schedule, budget, political, legal dimensions |
| Response strategy | A SPECIFIC action plan. "Schedule weekly meetings with the regulatory office to pre-negotiate modifications before formal submission" is a strategy. "Mitigate" is not. |
| Owner | Who executes the response |
| Deadline | When must the response be in place |
| Dependencies | Which other risks this connects to or amplifies |

**Rules:**
- Focus on risks SPECIFIC to this project's situation. Every project has "scope creep" and "resource constraints" — only include these if the context gives you reason to believe they are significant HERE.
- Political and stakeholder risks are real risks. A key sponsor leaving the organization is as concrete as a supply chain disruption.
- Quantify financial risks. Show the numbers.
- If a risk has been partially realized (early warning signs already visible), say so.

### `/budget-snapshot`

Generate a financial snapshot that reveals pressure points, not just totals.

**Ask for:** Available budget information, funding sources, any known cost issues.

**Output structure:**

| Item | Amount | Notes |
|------|--------|-------|
| Total budget | | Funding sources and any conditions or restrictions |
| Committed / spent to date | | What has already been allocated or spent |
| Contingency — original | | What was set aside |
| Contingency — used | | What has already been consumed |
| Contingency — remaining | | What is left for the rest of the project |
| Known unbudgeted costs | | Items identified but not in the original budget |
| Financial risk exposure | | What happens if costs exceed remaining contingency |

**Rules:**
- Show the math. The sponsor must see the gap between what is available and what is needed.
- Flag funding conditions (grant expiration dates, approval gates, matching requirements).
- If contingency is being consumed faster than expected, calculate the burn rate and project when it runs out.
- Identify costs that are politically sensitive (who approved them, who benefits).

## Anti-patterns

### The Sanitized Charter
Every section reads as balanced, manageable, and professional. Risks are "challenges." Stakeholders are "aligned." The budget is "adequate." The PM presents this to the steering committee and loses credibility the moment someone asks a hard question, because the charter did not prepare them for the real situation.

### The Invented Charter
The AI fills information gaps with plausible-sounding answers instead of flagging unknowns. The charter states a budget breakdown that was never provided, assigns stakeholder influence levels based on job titles rather than actual dynamics, and presents timelines that sound reasonable but have no basis in the briefing. The PM discovers the fabrications when the sponsor says "where did you get that number?"

### The Scope-is-Settled Illusion
Scope is presented as clean boundaries: in scope / out of scope, done. But the briefing clearly shows competing stakeholder priorities, unresolved trade-offs, and items that may be cut if budget tightens. By not surfacing these negotiations, the charter sets the PM up to look incompetent when the conflicts emerge later — and they always emerge.

### The Flat Stakeholder List
A table of Name / Role / "Keep informed" or "Manage closely." This provides zero tactical value. It does not reveal who the real decision-maker is, which stakeholders are in tension, who has been sidelined and might undermine the project, or what concretely happens if a stakeholder is mismanaged.

### The Textbook Risk Register
Ten generic risks pulled from a PMBOK chapter: scope creep, resource constraints, communication gaps, technology risk. None of them are specific to THIS project. The response strategies are single verbs: mitigate, transfer, accept. The risk register becomes a compliance checkbox that helps no one.

### The Single-Number Budget
"Total project budget: $2.4M." No visibility into contingency status, funding conditions, committed vs. remaining, or what happens when costs exceed the reserve. The sponsor sees a number; the PM sees a crisis they cannot communicate.

### The Charter-as-Project-Plan
Twenty pages including a WBS, weekly task breakdowns, resource allocation tables, and procurement timelines. This is not a charter — it is a project plan wearing a charter's title. A charter authorizes and frames. It does not plan. If it exceeds 8 pages, scope it back.

## Frameworks

### Backward Planning from Hard Deadlines
Start with the immovable end date and work backward. For each milestone, state the logic: "If the system must be live by September 1, then UAT must complete by August 15, which means development must freeze by July 31, which means the design decision on X must happen by June 15." This exposes decisions that are already late.

### Stakeholder Conflict Analysis
For every pair of stakeholders with overlapping interests, ask: What does each want? Where do those wants conflict? Who has more organizational power? What happens if the conflict is not resolved? This produces the "potential conflict" and "risk if mismanaged" fields that make a stakeholder analysis actually useful.

### The Decision-Required Protocol
Every unresolved item in the charter gets tagged: **DECISION REQUIRED** — who decides, what the options are, what the trade-offs are, and when the decision must be made. This converts a passive document into an active decision-forcing tool.

### Charter Length Discipline
A charter is 4-8 pages. Under 4 means critical information is missing. Over 8 means the document is trying to be a project plan. The discipline forces prioritization: only the information the sponsor needs to authorize the project and understand its key risks.

## Templates

See [resources/charter-template.md](resources/charter-template.md) for the charter section structure.
