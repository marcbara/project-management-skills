# Project Management Skills

Project management skills for AI coding assistants. Encode practitioner judgment, not textbook definitions.

## What's Inside

- **4 skills** (18 commands) — Universal, work on any platform that supports the Agent Skills standard
- **1 agent** — Project advisor that orchestrates skills into workflows (Claude Code only)
- **3 commands** — End-to-end workflows: `/plan-project`, `/health-check`, `/takeover` (Claude Code only)

## Install

### Claude Code (full experience: skills + agent + commands)

```bash
/plugin marketplace add marcbara/project-management-skills
/plugin install project-management-skills@project-management-skills
```

This gives you the agent, the workflow commands, and all skills. The agent chains skills together and behaves like an experienced PM consultant — it diagnoses before prescribing, flags organizational dysfunction, and pushes back on unrealistic plans.

### Any other platform (skills only)

Works on Cursor, Gemini CLI, Codex, Copilot, and any tool that reads the Agent Skills open standard.

```bash
# All skills
npx skills add marcbara/project-management-skills --all

# Specific skill
npx skills add marcbara/project-management-skills --skill project-charter
```

Skills work standalone without the agent. Each SKILL.md contains the full methodology, commands, frameworks, and anti-patterns.

## Skills

### project-charter
Generate and review sponsor-ready project charters with honest scope, risk, and stakeholder analysis.

| Command | Description |
|---------|-------------|
| `/charter` | Full charter generation from a briefing |
| `/charter-review` | Review an existing charter and identify weaknesses |
| `/stakeholder-map` | Deep stakeholder analysis with conflict detection |
| `/risk-snapshot` | Charter-level risk assessment with specific response strategies |
| `/budget-snapshot` | Financial snapshot showing pressure points, not just totals |

### document-analysis
Cross-check project documents for contradictions, gaps, and evolving risks.

| Command | Description |
|---------|-------------|
| `/consistency-check` | Iterative document-by-document cross-checking |
| `/document-audit` | Batch analysis of multiple documents at once |
| `/gap-analysis` | Identify missing information from documents reviewed |
| `/contradiction-report` | Structured contradiction report ranked by impact |

### scope-management
Scope statements with negotiation transparency and WBS creation following the 100% rule.

| Command | Description |
|---------|-------------|
| `/scope-statement` | Three-part scope: in, out, and under negotiation |
| `/wbs` | Generate complete WBS decomposed to activity level |
| `/wbs-review` | Review an existing WBS against quality criteria |

### schedule-management
Schedule development with critical path, compression techniques, milestone planning, and MS Project XML export.

| Command | Description |
|---------|-------------|
| `/schedule` | Generate schedule with dependencies and critical path |
| `/critical-path` | Analyze critical path and identify vulnerabilities |
| `/compress-schedule` | Apply crashing and fast-tracking to meet a deadline |
| `/schedule-risks` | Identify schedule-specific risks and vulnerable activities |
| `/milestone-plan` | High-level milestone plan for steering committee |
| `/schedule-export` | Export schedule to Microsoft Project XML format |

## Workflows (Claude Code only)

| Command | Description |
|---------|-------------|
| `/plan-project` | End-to-end project planning: charter, scope, WBS, schedule, milestones |
| `/health-check` | Diagnose project health from documents, schedule, and stakeholder landscape |
| `/takeover` | Structured project takeover with document cross-check, stakeholder mapping, and 30-day plan |

By using these skills, you agree that the job of a project manager is to deliver through people, not to fill templates. Read the [Manifesto](MANIFESTO.md).

## Contributing

Contributions are welcome. If you have real-world PM experience and want to improve an existing skill or propose a new one, open an issue or submit a pull request.

## License

Free and open source. The value is the methodology, not the paywall.

---

&copy; [ProjectWorkLab](https://www.projectworklab.com)
