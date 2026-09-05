# Open Agent Skills

Open Agent Skills is a collection of vendor-neutral `SKILL.md` instructions for coding agents. Each skill is a focused procedure for work such as debugging, repository exploration, security review, API compatibility, refactoring, accessibility, and release safety.

A skill is useful only when its scope, steps, evidence, and failure handling improve an agent's behavior. The collection does not require a specific model, host application, tool protocol, or programming language.

## Install

List the available skills:

```sh
npx skills add ChloeVPin/open-agent-skills --list
```

Install one skill into a project:

```sh
npx skills add ChloeVPin/open-agent-skills --skill evidence-driven-debugging
```

Install the full collection globally:

```sh
npx skills add ChloeVPin/open-agent-skills --all -g
```

Without `-g`, installation is project-scoped. Inspect a skill before installing it because its instructions influence agent behavior.

## Skill structure

The directories under [skills/](skills/) are the distributable skills. The research and review records live in:

- [docs/evidence-standard.md](docs/evidence-standard.md)
- [docs/research.md](docs/research.md)
- [docs/skill-audit.md](docs/skill-audit.md)

The current collection is a draft baseline. Its lifecycle status describes the evidence available for each skill; it is not a guarantee that every recommendation applies to every repository.

## Contributing

Keep a skill narrow enough to load for one task. State its trigger, procedure, boundaries, evidence, and validation path. Remove or revise a skill when repository work shows that it adds ceremony without improving decisions.
