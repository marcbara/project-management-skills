---
name: risk-management
description: >
  Project risk management with rigorous risk metalanguage: generate risk
  registers and review or rewrite weak risk register entries. Use when
  identifying risks for a new or running project, when an existing risk
  register needs a quality review, or when risk entries must be reformulated
  so they can actually be monitored and acted on.
---

# Risk Management

## Context

Most risk registers fail twice: first as writing, then as practice.

As writing, because the entries are mush. "The supplier could fail" is not a risk statement — it blends a present fact (we depend on that supplier), a future event (the failure), and a consequence (the delay) into one unfalsifiable sentence. A register written this way cannot be monitored: nobody can say when the risk has materialized, so nobody reacts until the impact has already landed.

As practice, because the register becomes a compliance checkbox. It gets produced once, filed, and never drives a single decision. A risk register that no one reads, updates, or acts on is risk theater — and it is worse than no register, because it creates the illusion that risk is being managed.

The discipline that fixes the writing problem is the **risk metalanguage**: every risk is a causal chain with three strictly separated parts. The **Root Cause** is a stack of present-tense, verifiable facts about how the project is configured today. The **Incident** is the earliest concrete, verifiable event that tells you the risk has materialized and forces you to react. The **Impact** is the consequence of the incident on project objectives — scope, time, cost, quality — with an order of magnitude. LLMs and most practitioners blur these three constantly; keeping them separate is what makes a register actionable.

Scope note: `/risk-snapshot` (project-charter skill) produces the 5-8 charter-level risks a sponsor needs to authorize a project. This skill produces and maintains the **working risk register** the team manages throughout delivery. The snapshot frames; the register operates.

## Commands

### `/risk-register`

Generate an initial risk register from a project description.

**Ask the user for:** The project description. It can be brief or extensive — the more concrete, the more precise the register. If it is clearly insufficient (e.g., "a software project" with no context), ask for at least: project type, approximate scope, duration, team, and any known constraints.

If the user specifies how many risks they want, respect it. Otherwise decide a reasonable number from the scenario's complexity (normally 6-15).

**Threat/opportunity distribution:** Default to roughly 80% threats, 20% opportunities. This reflects project reality — there are more ways for things to go wrong than right, but opportunities are not zero and must appear. Adjust if the scenario clearly favors a different split (e.g., an innovation project), but always include at least one opportunity.

**Category sweep:** Before generating risks, think through ALL of these categories and assess which are relevant to this project. Not one risk per category — some categories will have several, others none — but every category must have been considered:

- **Technical/Technological** — Architecture, performance, integration with existing systems, maturity of chosen technology, inherited technical debt
- **Suppliers and third parties** — External vendor dependency, subcontractors, software licenses, cloud services, hardware
- **People and team** — Availability, turnover, training, key competencies, team cohesion, internal conflicts
- **Stakeholders and users** — Misaligned expectations, sponsor changes, resistance to change, uncommitted key users, missing management buy-in
- **Scope and requirements** — Scope creep, poorly defined requirements, late functional changes, missing prioritization
- **Planning and schedule** — Optimistic estimates, task dependencies, overloaded critical path, external milestones outside project control
- **Financial and budgetary** — Cost overruns, materials inflation, exchange rates, cash flow, budget constraints
- **Quality** — Undetected defects, insufficient test coverage, standards non-conformities, rework
- **Regulatory and legal** — Regulatory changes, compliance (GDPR, sector-specific), contracts, intellectual property, licensing
- **Operational** — Transition to production, post-implementation support, business continuity during rollout, end-user training
- **External and context** — Macroeconomic situation, geopolitics, competition, industry events, weather on field projects
- **Security** — Cybersecurity, data protection, physical security, access control, audits
- **Typical opportunities** — Synergies with other projects, public grants, emerging technologies maturing during the project, favorable market shifts, scope improvements possible within the same budget

**Field rules — apply to every risk generated:**

| Field | Rule |
|-------|------|
| Type | Threat or Opportunity |
| Root Cause | A stack of present-tense, verifiable facts about the project: dependencies, decisions already made, properties of the plan. Never a future or hypothetical event. Never contains "could", "may", or "might" — the possibility of failure is what justifies the risk existing, but it is not part of the root cause. Best form: several short present-tense sentences stacked. |
| Incident | The concrete event you detect that forces you to react. A point-in-time occurrence that can be unambiguously verified as having happened or not. Not a continuous condition, not a vague judgment. The earliest actionable moment, not the impact. |
| Impact | The consequence of the incident on project objectives: scope, time, cost, quality. Not post-project operational effects, not vague judgments. Give an order of magnitude even if not precisely quantified ("2-3 week delay", "€15,000-25,000 overrun"). |
| Strategy | Canonical labels only. Threats: Avoid / Mitigate / Transfer / Accept. Opportunities: Exploit / Enhance / Share / Accept. Just the label, no added description. Transfer is NOT valid for opportunities — the correct label there is usually Share. |
| Action | The concrete, executable step that implements the strategy. What is done, when, and how — specific enough that someone could start next week. Not a generic objective ("improve communication") but a concrete step ("establish a 15-minute daily stand-up with one representative per sub-team"). Must be coherent with the chosen strategy: if the strategy is Accept, the action cannot consist of acting on the risk. |

**The root cause test:** Can every clause be checked today against project documentation (contracts, plan, org chart) without predicting the future? If yes, it is valid. If any clause requires prediction, that clause belongs in the incident.

**Output format:**

1. A short 2-3 sentence opening explaining which risk categories were prioritized for this project and why.
2. The register as a table: `| ID | Type | Root Cause | Incident | Impact | Strategy | Action |` — numbered R1, R2, R3… Keep cells compact but precise; stacked root-cause sentences separated by periods in the same cell.
3. A **Category coverage** section: which categories are covered and with how many risks, and which were considered but discarded as not relevant here.
4. If the scenario lacks information critical for identifying important risks (no mention of team or suppliers, for example), say so briefly before the table.
5. Close by asking: "Do you want me to add qualitative probability and impact (High/Medium/Low) and tentative owners, or keep the register as is?" Wait for the answer before extending. If the user says yes, add a Probability and an Impact column (High/Medium/Low, each with one line of reasoning from the project context, not a gut label) and a tentative owner per risk — a named role, not a department.

Precision over volume: ten well-formed risks beat twenty badly written ones.

### `/risk-review`

Review one or more risk register entries and rewrite them cleanly. This command teaches — it names each error and explains why it is an error, not just how to fix it.

**Minimum fields required:** Type, Root Cause, Incident, Impact, Strategy, Action. If any is missing or clearly incomplete, stop and tell the user which — do NOT invent missing content.

**Extra fields** the user includes (probability, impact score, owner, ID, date, status, category, trigger, residual risk, etc.) are NOT part of this review. Pass them through to the rewrite exactly as written — unmodified, unreordered, unrenamed, undeleted.

**Multiple entries:** Review each separately under its own heading (Risk 1, Risk 2…). Do not merge commentary.

**Phase A — Review.** For each of the six main fields, evaluate:

- **Precision.** Is the field clear, specific, verifiable? If vague, point to the exact word or phrase that is too generic.
- **Category correctness.** Does the content match the field's definition? The frequent errors, in order of frequency:
  - **Root cause stated as a future event** — the most common error. Test each clause against the present-tense test. "We depend on a single supplier that could fail" is still wrong: "could fail" smuggles a future hypothesis into a purely factual field. "We depend on a single supplier, no backup, on the critical path" is right — bare present facts.
  - **Incident described as a continuous condition.** "Internal communication keeps failing" is not an incident. "At sprint review, two sub-teams are found to have built incompatible features" is.
  - **Incident that is not verifiable.** "There will be problems with the supplier" is a judgment, not an event.
  - **Incident confused with the impact.** "The supplier misses the milestone date" is not the incident — it is the impact (the delay). The incident is the event that warns you and lets you act earlier: "the supplier emails notice of a significant delay two weeks before the milestone." That is the moment the risk materializes in an actionable way.
  - **Impact described as a post-project operational effect.** "Users will be unhappy with the product" is operational. "Scope is cut 15% to hold the deadline" is a project impact.
  - **Action stated as an objective.** "Improve quality" is an objective. "Run a code review before each sprint demo" is a step.
  - **Invalid strategy label.** Only the eight canonical labels are admitted. Frequent reminder: Transfer is not valid for opportunities — use Share.
  - **Action incoherent with strategy.** If the strategy is Accept, the action cannot act on the risk — that would be Mitigate. If the strategy is Avoid, the action must eliminate the exposure, not reduce it.

**Phase B — Rewrite.** Rewrite the full entry cleanly: root cause as stacked present facts; incident as a concrete event at a concrete moment, as early as possible; impact tied to scope/time/cost/quality with order of magnitude; canonical strategy label; action executable next week and coherent with the strategy; all extra user fields preserved verbatim in their original order. If the original had the right idea but wrong formulation, keep the intent and fix the wording. If it had a fundamental category error (root cause and incident swapped), say so explicitly.

**Output structure per risk:**

```
## Risk [N]: [short title inferred from content]

[One-line scope note: which fields were reviewed, which extra fields are preserved untouched]

### Review

Type: [assessment]
Root Cause: [assessment — precision and category errors]
Incident: [assessment]
Impact: [assessment]
Strategy: [assessment]
Action: [assessment]

### Rewritten entry

Type: ...
Root Cause: ...
Incident: ...
Impact: ...
Strategy: ...
Action: ...
[any extra user fields, original order, copied verbatim]
```

After all entries, add a short **Main improvements** section with the two or three most important fixes applied to the set. Then ask: "Do you want me to compile the improved entries into a clean table?" Wait before producing it.

**Reference example (target quality level):**

```
Type: Threat
Root Cause: We depend on a single supplier. There is no contracted backup. The supplier's delivery is on the critical path.
Incident: The supplier emails notice of a significant delay two weeks before the milestone.
Impact: Project delay of 3 to 4 weeks.
Strategy: Mitigate
Action: Add delay-penalty clauses to the contract and schedule biweekly follow-up meetings with verifiable intermediate milestones (development environment, test environment, first functional iteration).
```

Every root-cause clause checks against project documentation today. The incident is the actionable warning, not the delay itself. The impact has an order of magnitude. The action implements Mitigate with executable steps.

Tone: direct but constructive. Many users are students or practitioners learning to formulate risks. When flagging a category error, explain briefly why it is one. The goal is that the user understands why the original was weak, not just how the fix reads.

## Anti-patterns

### The Future-Tense Root Cause
The most common register error. "The supplier could fail to deliver" as root cause. The root cause is what is true about the project TODAY — the dependency, the missing backup, the critical-path position. The future failure belongs to the incident. A register full of future-tense root causes cannot be audited against project documentation, so the causes are never challenged or fixed.

### The Continuous-Condition Incident
"Communication is failing constantly" — when did the risk materialize? Nobody can say, so nobody reacts. An incident is a point event: it either happened or it did not. If you cannot name the moment you would be forced to act, you have not identified the incident yet.

### Incident-Impact Confusion
Writing the consequence in the incident field: "the supplier misses the delivery date." By then the impact has landed and the response options are gone. The incident is the earliest verifiable warning — the email announcing the delay, the failed integration test, the resignation letter. Finding the earliest actionable moment is the whole point of the field.

### The Objective Disguised as an Action
"Improve stakeholder communication." "Strengthen quality." These are wishes, not actions. Nobody can start them Monday morning. An action names what is done, by whom, and when — and its absence is detectable.

### Strategy-Action Incoherence
Strategy says Accept, action says "negotiate a backup supplier" — that is Mitigate wearing an Accept label. Strategy says Avoid, action reduces exposure instead of eliminating it. The label and the action must tell the same story, or the register misleads everyone who reads only one of them.

### Risk Theater
The register exists, was produced with care, and drives nothing. Nobody reads it, updates it, or acts on it — it was written for the audit, not for the project. Producing a beautiful register and filing it is worse than not having one: it signals that risk is handled when it is not. A register earns its existence by being reviewed on a cadence and by driving the response actions it contains.

## Frameworks

### The Risk Metalanguage

Every risk is a causal chain with three strictly separated parts:

| Part | Time | Test |
|------|------|------|
| Root Cause | Present | Every clause verifiable today against project documentation (contracts, plan, org chart). No "could/may/might". |
| Incident | Future, point-in-time | A concrete event you can unambiguously say has occurred or not. The earliest actionable warning. |
| Impact | Future, consequence | Effect on scope, time, cost, or quality, with an order of magnitude. |

Canonical example: *We depend on a single supplier. No backup is contracted. The delivery is on the critical path* (root cause) → *the supplier emails notice of a significant delay two weeks before the milestone* (incident) → *3-4 week project delay* (impact).

### The Present-Tense Test
For each clause of a root cause, ask: can I check this today against a project document without predicting anything? If yes, it stays. If it requires prediction, move it to the incident. This single test catches the majority of register-writing errors.

### Canonical Response Strategies

| Threats | Opportunities |
|---------|---------------|
| Avoid — eliminate the exposure | Exploit — make the opportunity certain |
| Mitigate — reduce probability or impact | Enhance — increase probability or impact |
| Transfer — shift the impact to a third party | Share — team with a third party to capture it |
| Accept — no proactive action; monitor | Accept — no proactive action; monitor |

Transfer does not exist for opportunities; its mirror is Share. The action must implement the label: Accept + acting on the risk = mislabeled Mitigate.

### Risk Category Sweep
The 13-category checklist in `/risk-register` exists to defeat availability bias — the tendency to fill a register with the risk types that come to mind first (usually technical) while missing the ones that actually kill projects (stakeholders, suppliers, regulatory calendars). Sweeping all categories and explicitly discarding the irrelevant ones is cheap; discovering a missed category in month six is not.
