# ProjectWorkLab Project Management Skills

Project management skills for AI coding assistants. Encode practitioner judgment, not textbook definitions.

## Install

**Claude Code** (full experience):
```bash
/plugin marketplace add marcbara/project-management-skills
/plugin install project-management-skills@project-management-skills
```

**Any platform** (skills only):
```bash
# All skills
npx skills add marcbara/project-management-skills --all

# Specific skill
npx skills add marcbara/project-management-skills --skill project-charter
```

By using these skills, you agree that the job of a project manager is to lead people, not to fill templates. Read the [Manifesto](MANIFESTO.md).

## Skills

| Skill | Commands | Description |
|-------|----------|-------------|
| [project-charter](skills/project-charter/) | 5 | Generate and review sponsor-ready project charters with honest scope, risk, and stakeholder analysis |
| [document-analysis](skills/document-analysis/) | 4 | Cross-check project documents for contradictions, gaps, and evolving risks |
| [scope-management](skills/scope-management/) | 3 | Scope statements with negotiation transparency and WBS creation following the 100% rule |
| [schedule-management](skills/schedule-management/) | 6 | Schedule development with critical path, compression techniques, milestone planning, and MS Project XML export |

## License

Free and open source. The value is the methodology, not the paywall.
