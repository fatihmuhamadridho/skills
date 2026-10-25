# Fatih's Codex Skills

Reusable skills, agent profiles, and automation configurations for Codex and related AI agent workflows.

## Install skills

Install this repository's skills with the Skills CLI:

```bash
npx skills add fatihmuhamadridho/skills
```

## Repository contents

- `skills/` — reusable workflows and domain guidance. Each skill is defined by a `SKILL.md` file; some include supporting references, scripts, templates, or agent metadata.
- `agents/` — Codex agent profiles for roles such as business analysis, full-stack development, and QA.
- `automations/` — recurring or multi-step automation configurations and their supporting files.
- `AGENTS.md` — repository-level instructions for agents working with this repository.
- `RTK.md` — local guidance for using RTK in shell workflows.

## Selected skills

- `project-analyst` — analyzes product requirements, scope, flows, acceptance criteria, risks, and backlog breakdown.
- `solutioning-analyst` — prepares backend solution analysis, API flows, and contract planning.
- `feature-driven-development` — organizes implementation around feature-focused slices.
- `atomic-design` — applies atomic design principles to UI structure.
- `clean-architecture` — helps define and maintain architecture boundaries.
- `figma-repo-compliance` — checks implementation against Figma designs and repository expectations.
- `automation-patrol-builder` — creates and repairs recurring patrol automation workflows.
- `codex-session-activity-summary` — summarizes activity from local Codex session logs.
- `commit-push` — prepares commits and push-ready Git delivery.
- `request-pr-mr` — prepares pull request or merge request content from a diff.

Browse `skills/` for the full set of skills. Skill folders can also contain supporting materials specific to that workflow.

## Contributing

Keep each skill's `SKILL.md` focused on when and how to use it. Put detailed examples and supporting material in that skill's own `references/`, `scripts/`, or `templates/` directories where appropriate. Update this README when repository contents or installation instructions change.
