---
name: scope-management
description: >
  Project scope definition and work breakdown structure creation. Use when
  writing scope statements, decomposing project scope into a WBS, reviewing
  existing WBS quality, or when stakeholders disagree about what is in or
  out of scope. Covers the pipeline from scope definition through activity-
  level decomposition.
---

# Scope Management

## Context

Scope management fails in organizations not because PMs cannot write a WBS, but because they avoid the hard conversations about what is NOT in scope and what is STILL BEING NEGOTIATED. The instinct — reinforced by most AI tools — is to present scope as settled: a clean list of in-scope items and a brief out-of-scope section. This is a fiction. In real projects, scope is contested territory. Stakeholders have competing priorities. Budget pressure forces trade-offs. Items that "everyone assumes" are included have never been formally agreed.

The second failure mode is WBS quality. A poorly decomposed WBS cascades into an unreliable schedule, inaccurate cost estimates, and unclear responsibility assignments. The 100% rule — every level must capture exactly 100% of the work in the level above — sounds simple but is violated constantly. The most common violation is the WBS that adds work not in the scope statement (scope creep through decomposition) or omits management, quality, and closeout work (optimistic decomposition).

## Commands

### `/scope-statement`

Generate a three-part scope statement that surfaces negotiation, not just boundaries.

**Ask the user for:** Project briefing, stakeholder inputs, any known scope disputes.

**Output three sections:**

**IN SCOPE** — Concrete deliverables and work packages. Each item must be specific enough to verify: "delivered" or "not delivered" should be unambiguous. Avoid vague scope items like "project documentation" or "stakeholder engagement" — specify which documents and which stakeholders.

**OUT OF SCOPE** — Items that a reasonable stakeholder might assume are included but are NOT. This section prevents scope creep by making exclusions explicit. If no one would assume it is included, it does not belong here. Focus on the boundary cases.

**SCOPE UNDER NEGOTIATION** — The section most scope statements miss entirely. For each unresolved scope item:

| Field | Content |
|-------|---------|
| Item | What is being debated |
| Option A | One approach, with implications |
| Option B | Alternative approach, with implications |
| Advocates | Who is pushing for each option (name roles, not just "stakeholders") |
| Trade-off | What is gained and lost with each option (cost, schedule, political, quality) |
| Decision deadline | When this must be resolved before it blocks progress |
| PM recommendation | Optional — if the PM has a view, state it as a recommendation, not a decision |

**Rules:**
- If the briefing shows ANY unresolved scope dispute, it MUST appear in Scope Under Negotiation. Presenting contested scope as settled is the single most damaging scope management failure.
- The out-of-scope section must include items that will generate pushback. If no one would object, the exclusion is not doing its job.
- Scope items must be testable. If you cannot determine whether the item was delivered, it is too vague.

### `/wbs`

Generate a complete Work Breakdown Structure decomposed to the activity level.

**Ask the user for:** Project scope statement (or use `/scope-statement` to generate one first).

**Structure rules (PMI Practice Standard for WBS):**

- **Deliverable-oriented decomposition** at the upper levels. The WBS organizes work by what is produced, not by who does it or when.
- **100% rule at every level.** Each level must capture exactly 100% of the work in the level above. No more (scope creep through decomposition), no less (missing work).
- **Consistent hierarchy:**
  - Level 1 = Project
  - Level 2 = Major deliverables or phases
  - Level 3 = Sub-deliverables
  - Level 4 = Work packages
  - Level 5 = Activities (schedulable, estimable, assignable)
- **Numbering scheme:** 1.0, 1.1, 1.1.1, 1.1.1.1, etc.
- **Project management branch at Level 2.** This is mandatory. It includes: planning, monitoring and reporting, stakeholder coordination, quality management, procurement management, change control, and project closeout.
- **No orphan packages.** Every branch must decompose to at least 2 children. A branch with only one child indicates incomplete decomposition.

**Content rules:**

- Include ALL work required to deliver the scope: technical delivery, management, quality assurance, procurement, regulatory compliance, and closeout.
- Include activities for project closeout and handover — these are consistently underestimated.
- Do NOT include work explicitly listed as out of scope.
- Each activity (lowest level) must be specific enough to: estimate duration, assign to a responsible party, and schedule.
- Note obvious dependencies where they exist (e.g., "regulatory approval must precede construction phase").

**Quality checks before delivering:**

- [ ] 100% rule holds at every decomposition level
- [ ] No orphan branches (every node has 0 or 2+ children)
- [ ] Project management branch exists and is complete
- [ ] Closeout and handover activities are included
- [ ] No work outside the scope statement has been added
- [ ] Activities at the lowest level are schedulable and estimable
- [ ] Regulatory, compliance, and approval activities are included where applicable

**Format:** Present as an indented outline with WBS numbering. For each activity (lowest level), note in brackets: [estimated duration range, responsible party or role].

After the WBS, provide a **summary table of key milestones** with target timing (relative, unless the user provides specific dates).

### `/wbs-review`

Review an existing WBS against quality criteria.

**Ask the user to provide:** The WBS to review and the scope statement it is based on.

**Check for these issues:**

**100% rule violations:**
- Work in the scope statement that is not represented in the WBS (missing work)
- Work in the WBS that is not in the scope statement (scope creep through decomposition)
- Levels where children do not fully cover their parent (incomplete decomposition)

**Structural issues:**
- Orphan branches (node with only one child)
- Inconsistent decomposition depth (some branches go to Level 5, others stop at Level 3 without justification)
- Missing project management branch
- Missing closeout / handover activities
- Missing regulatory / compliance activities where they should exist

**Naming and clarity:**
- Activities that are too vague to estimate or schedule
- Activities that are actually multiple activities bundled together
- Inconsistent naming conventions across the WBS
- Activities named as outcomes ("system working") rather than work ("system integration testing")

**Common omissions:**
- Quality assurance activities (reviews, inspections, testing)
- Change control activities
- Procurement activities (if external vendors or purchases are involved)
- Training and knowledge transfer
- Documentation (user manuals, technical docs, as-built records)
- Commissioning and testing where systems must work together

**Output:** Structured findings with severity (critical / significant / minor) and specific fix recommendations.

## Anti-patterns

### The Settled Scope Illusion
Presenting scope as clean boundaries when the briefing shows contested territory. Stakeholders have competing priorities that have not been resolved. Budget may force trade-offs that have not been decided. The PM who presents this as settled will lose credibility when the conflicts emerge — and they always emerge. The scope statement must surface the negotiation, not hide it.

### The Polite Out-of-Scope Section
Listing items that no one would ever assume are in scope. "Building a new headquarters is out of scope" for an IT project is not useful. The out-of-scope section must name the items that WILL generate pushback — the things a specific stakeholder expects to be included and will be disappointed to find excluded. If the out-of-scope section is uncontroversial, it is not doing its job.

### Scope Creep Through Decomposition
The WBS adds work that is not in the scope statement. This happens subtly: the PM thinks "we should also do X" and adds it during decomposition. The 100% rule works in both directions — the WBS must not contain LESS than the scope, but also not MORE. Every item in the WBS must trace back to a scope statement item.

### Optimistic Decomposition
The WBS includes the exciting technical delivery work but omits management overhead, quality activities, regulatory compliance, change control, and closeout. This produces a schedule and budget that look achievable but are missing 15-25% of the actual work.

### The Activity Soup
A flat list of activities without clear deliverable-oriented grouping. The WBS reads like a to-do list rather than a structured decomposition. This makes it impossible to verify the 100% rule and difficult to assign accountability at the work package level.

### Inconsistent Depth
Some branches are decomposed to Level 5 with detailed activities while others stop at Level 2 with a vague summary. This usually indicates that the PM understands some parts of the project well and has not thought through others. Decomposition depth should be consistent, or the variation should be explicitly acknowledged ("Phase 3 will be decomposed in detail after the Phase 2 design review").

## Frameworks

### The 100% Rule

At every level of the WBS, the sum of child elements must equal exactly 100% of the parent. Not 95% (missing work), not 110% (scope creep).

**How to verify:** For each parent node, read the scope of the parent, then read all children. Ask: "If all children are completed, is the parent fully delivered?" (tests for missing work) and "Is any child doing work that goes beyond the parent's scope?" (tests for creep).

### Deliverable-Oriented Decomposition

The upper levels of the WBS (Levels 1-3) must be organized by WHAT is produced, not by WHO does it or WHEN it happens.

**Wrong:** Level 2 = "Phase 1", "Phase 2", "Phase 3" (time-based)
**Wrong:** Level 2 = "Engineering Team", "Marketing Team", "Legal" (org-based)
**Right:** Level 2 = "Core Platform", "User Interface", "Data Migration", "Project Management" (deliverable-based)

Time-based and org-based decomposition create confusion about scope boundaries, make the 100% rule harder to verify, and blur accountability.

### Scope Statement Traceability

Every activity in the WBS must trace to a scope statement item. Every scope statement item must appear in the WBS. This bidirectional traceability is the mechanism that prevents both scope creep and scope omission. When reviewing a WBS, the first step is always: map WBS branches to scope statement items and look for orphans in both directions.

### Decomposition Completeness Checklist

For any project, verify the WBS includes activities for:

- [ ] Core technical / business deliverables
- [ ] Project management (planning, monitoring, reporting, meetings)
- [ ] Quality management (reviews, inspections, testing, acceptance)
- [ ] Procurement (if external resources or materials are needed)
- [ ] Regulatory and compliance (permits, approvals, certifications)
- [ ] Change control (process for handling scope changes)
- [ ] Risk management activities (risk reviews, contingency execution)
- [ ] Stakeholder management (communication, reporting, engagement)
- [ ] Training and knowledge transfer
- [ ] Documentation (deliverable-specific and project-level)
- [ ] Closeout (handover, lessons learned, administrative closure)
