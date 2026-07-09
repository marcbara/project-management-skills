# Project Management Skills

Project management skills for AI coding assistants. Encode practitioner judgment, not textbook definitions.

## What's Inside

- **4 skills** — Universal, work on any platform that supports the Agent Skills standard
- **21 commands** — 18 skill commands plus 3 end-to-end workflows: `/plan-project`, `/health-check`, `/takeover` (Claude Code only)
- **1 agent** — Project advisor that orchestrates skills into workflows (Claude Code only)

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

Skills work standalone without the agent. Each SKILL.md contains the full methodology, commands, frameworks, and anti-patterns. Slash commands are a Claude Code feature — on other platforms, ask for the same operations in natural language; each SKILL.md documents them.

## Skills

Full command documentation lives in each SKILL.md — the README only indexes them.

| Skill | What it does | Commands |
|-------|--------------|----------|
| [project-charter](skills/project-charter/SKILL.md) | Sponsor-ready charters with honest scope, risk, and stakeholder analysis | `/charter` `/charter-review` `/stakeholder-map` `/risk-snapshot` `/budget-snapshot` |
| [document-analysis](skills/document-analysis/SKILL.md) | Cross-check documents for contradictions, gaps, and evolving risks | `/consistency-check` `/document-audit` `/gap-analysis` `/contradiction-report` |
| [scope-management](skills/scope-management/SKILL.md) | Scope with negotiation transparency; WBS following the 100% rule | `/scope-statement` `/wbs` `/wbs-review` |
| [schedule-management](skills/schedule-management/SKILL.md) | Critical path, compression, milestone planning, MS Project XML export | `/schedule` `/critical-path` `/compress-schedule` `/schedule-risks` `/milestone-plan` `/schedule-export` |

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

[MIT](LICENSE). The value is the methodology, not the paywall.

---

&copy; [ProjectWorkLab](https://www.projectworklab.com)
