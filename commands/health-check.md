---
name: health-check
description: >
  Assess current project health from documents, schedule, and stakeholder landscape.
  Use when a project feels off track, before a steering committee, or when taking
  over an existing project and need to understand the real state quickly.
agent: project-advisor
---

# /health-check

Diagnose the current health of a project and produce an actionable assessment.

## Workflow

### Step 1: Gather

Ask the user to provide whatever they have:
- Project documents (charter, status reports, meeting minutes, contracts, schedules)
- Current concerns or symptoms ("the schedule keeps slipping", "the sponsor is disengaged")
- Any specific questions they need answered

Work with what is available. A health check with three documents is better than no health check.

### Step 2: Document Analysis → `/document-audit` or `/consistency-check`

Cross-check the provided documents for contradictions, gaps, and evolving risk signals.

If only one or two documents are provided, skip to Step 3 and work from the user's description.

### Step 3: Risk Assessment → `/risk-snapshot`

Based on everything gathered, produce a current risk picture. Focus on risks that are specific to this project's situation — not textbook risks.

### Step 4: Budget Pressure → `/budget-snapshot`

If financial information is available, produce the budget snapshot showing pressure points. If no financial data exists, flag that as a gap.

### Step 5: Schedule Check → `/critical-path` and `/schedule-risks`

If a schedule exists, analyze the critical path and identify vulnerable activities. If no schedule exists, flag that as a gap and produce a `/milestone-plan` based on known deadlines.

### Step 6: Diagnosis

Synthesize everything into a single health assessment:

**Project Status:** Red / Amber / Green — with justification. Do not default to Amber. If it is Red, say Red.

**Top 3 Issues:** The three things most likely to cause project failure, ranked by severity. For each: what is happening, why it matters, and what to do about it.

**Contradictions Found:** Summary of document inconsistencies that indicate deeper problems.

**Information Gaps:** What the PM needs to find out that no document currently answers.

**Recommended Actions:** Prioritized list of what to do next, with owners and deadlines. Be specific — "schedule a meeting with the sponsor to resolve the scope dispute on X by next Friday" not "improve stakeholder engagement."

## Output

A single health assessment document that a PM can use to:
1. Understand the real project state (not the reported state)
2. Walk into a steering committee prepared for hard questions
3. Prioritize the next 2 weeks of PM effort on what matters most
