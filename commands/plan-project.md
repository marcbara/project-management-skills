---
name: plan-project
description: >
  End-to-end project planning from a briefing. Produces charter, scope statement,
  WBS, schedule, and milestone plan in sequence. Use when starting a new project
  or when a sponsor needs a complete planning package.
agent: project-advisor
---

# /plan-project

Generate a complete project planning package from a briefing or set of inputs.

## Workflow

Execute these steps in order. After each step, present the output and confirm with the user before proceeding to the next. Errors compound — do not rush through.

### Step 1: Diagnose

Before producing anything, ask:
- What is the project? (briefing, documents, or description)
- Who is the sponsor and what do they need to decide?
- Are there known stakeholder tensions or political dynamics?
- What is the hard deadline and what drives it?
- What has already been produced? (Don't duplicate existing work.)

### Step 2: Charter → `/charter`

Generate the project charter. This frames everything that follows: purpose, objectives, scope boundaries, stakeholders, risks, budget, governance.

### Step 3: Scope → `/scope-statement`

Produce the detailed three-part scope statement (in / out / under negotiation). The charter's scope section is the input; this step expands it with full trade-off analysis.

### Step 4: WBS → `/wbs`

Decompose the scope into a complete Work Breakdown Structure down to the activity level. Verify the 100% rule against the scope statement.

### Step 5: Schedule → `/schedule`

Build the project schedule from the WBS with dependencies, durations, and critical path. Flag regulatory gates and parallel opportunities.

### Step 6: Milestones → `/milestone-plan`

Extract 6-10 executive-level milestones suitable for steering committee reporting. Work backward from the hard deadline.

### Step 7: Export (optional)

If the user needs the schedule in MS Project format, run `/schedule-export`.

## Output

A coherent planning package:
1. Project Charter (4-8 pages)
2. Scope Statement (three-part)
3. WBS (activity-level)
4. Project Schedule (with critical path)
5. Milestone Plan (6-10 gates)

All documents must be internally consistent. If a scope decision in Step 3 changes the charter assumptions, update the charter.
