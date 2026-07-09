---
name: schedule-management
description: >
  Project schedule development, critical path analysis, and schedule compression.
  Use when building a project schedule from a WBS, analyzing dependencies and
  critical path, compressing a schedule under deadline pressure, identifying
  schedule risks, or creating milestone plans for executive reporting.
---

# Schedule Management

## Context

Most project schedules are wrong in the same way: every activity follows the previous one in a neat sequence, durations are optimistic, dependencies exist because activities are adjacent in a list rather than because of a real constraint, and milestones are decorative markers rather than decision gates. The result is a schedule that takes twice as long as it should and breaks the moment reality intervenes.

The core scheduling insight that AI consistently misses: real projects have significant parallelism. Activities that can run simultaneously should overlap. The default should be parallel execution, with sequential dependencies only where a real constraint exists — technical, regulatory, logistical, or contractual. A PM who schedules everything in strict sequence is wasting time that cannot be recovered.

The second insight: schedules are driven by decisions, not just by work. A regulatory approval that takes 60 days does not respond to project pressure. A design decision that three stakeholders must agree on will take longer than the work it unblocks. These fixed-duration gates determine the critical path more often than the effort-based activities between them.

## Commands

### `/schedule`

Generate a project schedule from a WBS with dependencies and critical path.

**Ask the user for:** A complete WBS (or use `/wbs` from scope-management to generate one first). Also ask: Are there hard deadlines? Known regulatory or approval timelines? Resource constraints?

**Step 1: Activity analysis**
- Identify all schedulable activities from the WBS (lowest-level items)
- Identify regulatory or approval activities that act as gates (fixed durations, not effort-driven)
- Count total activities and major phases

**Step 2: Dependency determination**

For each activity, determine predecessors. Every dependency must have a justification:

| Dependency type | When to use | Example |
|----------------|-------------|---------|
| FS (Finish-to-Start) | Most common. Predecessor must finish before successor starts. | Testing starts after development completes |
| SS (Start-to-Start) | Activities can start together (possibly with a lag). | Documentation can start when development starts, lagged by 2 weeks |
| FF (Finish-to-Finish) | Activities must finish together. | Training materials must finish when the system build finishes |
| SF (Start-to-Finish) | Rare. Only use with clear justification. | New system start triggers old system decommission |

**Rules:**
- **Parallel by default.** If two activities have no technical, regulatory, or resource dependency, they should overlap. Challenge every sequential link: "Why can't these run at the same time?"
- **Dependencies based on real constraints, not list position.** Adjacent activities in the WBS are not automatically sequential. The dependency must reflect a technical need (can't test before building), a regulatory requirement (can't begin work before permit), a logical prerequisite (can't train users on a system that doesn't exist), or a resource constraint (same team cannot do both simultaneously).
- **Include lags where real waiting time exists.** Approval cycles, curing times, procurement lead times, review periods — these are lags on dependencies, not separate activities (unless the waiting time requires active management).

**Step 3: Duration estimation**

Estimate durations in working days. Apply these principles:

| Activity type | Estimation approach |
|--------------|-------------------|
| Effort-driven work | Based on scope complexity and typical team capacity. Do NOT assume unlimited resources. |
| Regulatory / approval gates | Based on administrative timelines, not effort. These do not compress with more resources. |
| Activities with high uncertainty | Use conservative estimates. Note the uncertainty range. |
| Activities dependent on external parties | Estimate based on contractual or historical timelines, not internal productivity. |

**Do NOT:**
- Use "1 week" as a default for everything
- Assume perfect conditions (full availability, no rework, no interruptions)
- Compress regulatory timelines — permits, approvals, and inspections follow their own calendar

**Step 4: Critical path identification**

After defining all dependencies and durations, identify the critical path: the longest chain of dependent activities that determines the minimum project duration. Activities on the critical path have zero float — any delay extends the project.

**Step 5: Output**

Deliver two things:

1. **Schedule notes** (text):
   - Total project duration
   - The critical path activities that determine the end date
   - The most important sequencing decisions made (where parallelism was applied, where it was not and why)
   - Key assumptions
   - Schedule risks: which activities are most likely to slip and what that would do to the end date

2. **Schedule table:**

| WBS | Activity | Duration | Predecessors | Type | Lag | Start | Finish | Float | Critical? |
|-----|----------|----------|-------------|------|-----|-------|--------|-------|-----------|
| | | | | | | | | | |

Include milestones as zero-duration rows. Summary tasks show phase rollups.

### `/critical-path`

Analyze the critical path from an existing schedule.

**Ask the user for:** The project schedule (paste or describe).

**Output:**

1. **Critical path chain** — List every activity on the critical path in sequence, with duration and dependency type. Show the total duration.

2. **Near-critical paths** — Activities with float under 5 days. These are one delay away from becoming critical.

3. **Bottleneck analysis:**
   - Which activities have the most successors depending on them?
   - Which decision gates are blocking the most downstream work?
   - Are there resource constraints creating artificial sequential dependencies?

4. **Vulnerability assessment:**
   - Which critical path activities have the highest uncertainty?
   - Which depend on external parties (vendors, regulators, other departments)?
   - Which have been estimated optimistically?
   - What is the single activity most likely to delay the project?

5. **Recommendations:**
   - Where could parallelism be introduced to shorten the critical path?
   - Which dependencies could be changed (e.g., FS to SS with lag)?
   - Which activities are candidates for crashing or fast-tracking?

### `/compress-schedule`

Apply schedule compression techniques to meet a deadline.

**Ask the user for:** The current schedule, the target end date (or how many days/weeks to compress), and any constraints (budget ceiling, fixed regulatory timelines, resource limits).

**Compression techniques (PMI standard):**

**Crashing** — Adding resources to critical path activities to shorten their duration.
- Always increases cost
- Only works on effort-driven activities (not regulatory gates, not fixed-duration work)
- Has diminishing returns: doubling resources does not halve duration
- Creates coordination overhead that partially offsets the time savings

**Fast-tracking** — Overlapping activities that were originally sequential.
- Increases risk (if the predecessor output changes, the successor must rework)
- Does not necessarily increase direct cost
- Only possible where partial output from the predecessor is sufficient for the successor to begin
- Not possible when the predecessor must fully complete before the successor can start (e.g., regulatory approval)

**What NOT to compress:**
- Regulatory activities (permits, inspections, approvals) — these follow administrative timelines
- Activities where compression would create safety or compliance risks
- Activities that are already on aggressive estimates

**Output:**

1. **Compression options** — For each critical path activity:

| Activity | Current duration | Technique | Compressed duration | Cost impact | Risk impact | Feasibility |
|----------|-----------------|-----------|--------------------| ------------|-------------|-------------|
| | | | | | | |

2. **Recommended compression plan** — The combination of crashing and fast-tracking that achieves the target with the least cost and risk increase.

3. **Comparison:** Original vs. compressed schedule — total duration, total cost impact, the 3 main sequencing changes, and new risks introduced.

4. **Honest assessment** — If the target date is not achievable without unacceptable risk or cost, say so. Recommend what IS achievable and what scope trade-offs would be needed to hit the original target.

### `/schedule-risks`

Identify schedule-specific risks and vulnerable activities.

**Ask the user for:** The project schedule and any known constraints or concerns.

**Analyze:**

1. **Critical path vulnerability** — Which critical path activities are most likely to slip? Why? (Uncertainty, external dependencies, resource constraints, technical complexity)

2. **Near-critical activities** — Activities with low float that could become critical. What would push them onto the critical path?

3. **Dependency concentration** — Activities where multiple parallel streams converge. These are bottlenecks: if any feeder is late, the convergence point slips.

4. **External dependencies** — Activities dependent on parties outside the project team (vendors, regulators, other departments, clients). These are the least controllable and most likely to introduce delay.

5. **Resource-driven risks** — Where the schedule assumes specific resource availability. What happens if a key person is unavailable? What happens if a shared resource is pulled to another project?

6. **Calendar and seasonal risks** — Holiday periods, regulatory filing windows, budget cycles, organizational events that could affect availability or approvals.

7. **Estimation confidence** — Which durations were estimated with high confidence (similar past work, contractual commitments) and which are rough guesses? Flag low-confidence estimates on or near the critical path.

**Output:** Risk register focused on schedule threats, ranked by impact on the end date, with specific mitigation actions.

### `/milestone-plan`

Generate a high-level milestone plan suitable for steering committee or executive reporting.

**Ask the user for:** Project scope, key deliverables, hard deadlines, known decision gates.

**Rules:**

- **6-10 milestones.** Fewer than 6 means the plan lacks control points. More than 10 means it is too detailed for executive consumption — that is a schedule, not a milestone plan.

- **Work backward from the hard deadline.** State the logic chain: "If the system must be live by September 1, then UAT must complete by August 15, which means development must freeze by July 31, which means the design decision on the integration approach must happen by June 15." This exposes decisions that are already late.

- **Every milestone must be a gate.** It represents one of:
  - A decision point (go/no-go, scope decision, funding approval)
  - A deliverable handover (to the client, to the next phase, to an external party)
  - A mandatory inspection, audit, or regulatory approval
  - A convergence point where parallel workstreams must synchronize

- **Decorative milestones are prohibited.** "Project kickoff" is only a milestone if it represents a decision gate. "Phase 2 start" is only a milestone if something must be true before Phase 2 can begin. If removing the milestone would change nothing about how the project is managed, it is decorative.

- **Flag blocking decisions.** Milestones that depend on decisions not yet made get a **DECISION REQUIRED** tag with: who decides, what the options are, and when the decision is needed to avoid schedule impact.

**Output:**

| # | Milestone | Date | Gate type | Depends on | Status | Blocking? |
|---|-----------|------|-----------|------------|--------|-----------|
| | | | | | | |

Plus a narrative: the critical path through the milestones, which decisions are most urgent, and what the consequences are if key milestones slip.

### `/schedule-export`

Export a project schedule to Microsoft Project XML format, ready to import into MS Project or ProjectLibre.

**Ask the user for:** A schedule (generated by `/schedule` or provided by the user). If dates are relative, ask for the project start date.

**Before generating any XML, read [resources/msproject-xml-spec.md](resources/msproject-xml-spec.md) and follow it exactly.** It contains the field-level rules that determine whether the file imports cleanly — most import failures come from violating one of them (task-level `<Start>`/`<Finish>` naming, `<Manual>0</Manual>`, `<DurationFormat>7</DurationFormat>`, duration encoding, assignment placement). Run the quality verification checklist at the end of the spec before delivering the file.

**Output:** A complete, valid XML file the user can save as `.xml` and import directly into MS Project or ProjectLibre.

## Anti-patterns

### The Shopping-List Schedule
Every activity follows the previous one in strict sequence. No parallelism. The schedule takes twice as long as it should because the PM (or the AI) defaulted to sequential thinking. In real projects, significant work happens simultaneously. The question is not "what comes next?" but "what else can happen at the same time?"

### Phantom Dependencies
Finish-to-Start links between every pair of consecutive activities, regardless of whether a real constraint exists. Activity 7 depends on Activity 6 because it is listed after it, not because there is a technical, regulatory, or logistical reason. These phantom dependencies add weeks or months to the schedule. Challenge every dependency: "What specifically prevents these from overlapping?"

### Textbook Durations
Estimating "1 week" or "2 weeks" for everything regardless of complexity, scale, or uncertainty. Duration estimation must reflect the specific characteristics of each activity: regulatory approvals follow administrative timelines (weeks to months), creative work has higher uncertainty than repetitive work, activities depending on external parties take longer than internally controlled work.

### Decorative Milestones
Milestones placed at regular intervals or at the end of every phase, representing nothing actionable. "End of Phase 1" is not a milestone unless something must be true at that point for the project to proceed. Every milestone must be a decision gate, a deliverable handover, a mandatory inspection, or a convergence point. If removing it changes nothing, remove it.

### Ignoring the Regulatory Calendar
Permits, inspections, approvals, certifications, and legal reviews follow their own timelines. They do not respond to project pressure, additional resources, or management urgency. A schedule that shows a 5-day regulatory approval when the actual administrative timeline is 60 days is fiction. These fixed-duration gates often determine the critical path — and they cannot be compressed.

### The Optimistic Baseline
Every duration assumes perfect conditions: full resource availability, no rework, no interruptions, no coordination overhead, no learning curve. This schedule will never be met. Professional estimates account for the realities of the working environment: shared resources, review cycles, rework rates, and organizational overhead.

### One-Type Dependencies
Using only Finish-to-Start (FS) dependencies everywhere. While FS is the most common type, real schedules use Start-to-Start (SS) with lags for overlapping work, and Finish-to-Finish (FF) for coordinated completions. Over-reliance on FS forces sequential execution where parallel execution is possible.

### Tasks Without Successors
Activities that complete but feed nothing downstream. The work "gets lost" — it does not contribute to any milestone or subsequent activity. Every activity except the final project milestone must be a predecessor to at least one other task. Orphaned tasks indicate gaps in schedule logic that will produce unreliable critical path analysis.

## Frameworks

### Dependency Type Selection Guide

| Situation | Type | Example |
|-----------|------|---------|
| B cannot start until A is fully complete | FS | Testing after development complete |
| B can start when A starts (or shortly after) | SS + lag | Documentation starts 2 weeks after development starts |
| B must finish when A finishes | FF | Training materials finish when system build finishes |
| A's start triggers B's completion | SF | Rare. New system go-live triggers old system decommission |
| Waiting time between A and B | FS + lag | Concrete pour, then 7-day curing lag, then next construction phase |

**Default to FS, but always ask:** "Could these overlap?" If yes, consider SS with an appropriate lag.

### Duration Estimation by Activity Type

| Type | Approach | Watch out for |
|------|----------|---------------|
| Effort-driven (internal team) | Scope-based estimate adjusted for team capacity | Assuming unlimited availability; ignoring coordination overhead |
| Fixed-duration (regulatory) | Based on administrative timeline, not effort | Trying to compress these with resources or urgency |
| External dependency | Based on contractual terms or historical performance | Assuming external parties share your urgency |
| High-uncertainty | Conservative estimate with explicit range | Anchoring on the optimistic end; not flagging the uncertainty |
| Repetitive (per-unit work) | Unit rate x quantity, adjusted for learning curve | Assuming linear scaling; ignoring setup/teardown per unit |

### Schedule Compression Decision Tree

1. **Is the activity on the critical path?** If no, compressing it does not help. Stop.
2. **Is it a regulatory or fixed-duration activity?** If yes, it cannot be compressed. Look elsewhere.
3. **Can it overlap with its predecessor?** If yes → **fast-track** (change FS to SS with lag). Assess: what rework risk does this introduce?
4. **Can more resources shorten it?** If yes → **crash** (add resources). Assess: what is the cost? What is the diminishing return?
5. **Can the scope of this activity be reduced?** If yes → negotiate with the scope owner. This is a scope trade-off, not a scheduling technique.
6. **None of the above?** The schedule cannot be compressed further without scope reduction. Report this honestly.

### Milestone Gate Taxonomy

| Gate type | Purpose | Example |
|-----------|---------|---------|
| Decision gate | A decision must be made before work can proceed | Architecture approach approval |
| Deliverable handover | A completed work product is transferred to the next phase, client, or external party | System delivered to UAT environment |
| Regulatory gate | An external authority must inspect, approve, or certify | Building permit issued; FDA review complete |
| Convergence point | Multiple parallel workstreams must synchronize before the next phase | All module integrations complete before system testing |
| Funding gate | Budget approval or funding release required to proceed | Phase 2 funding approved by finance committee |
