# Standard Codex Project Setup

This repository is a starter documentation kit for new Codex-assisted projects.
It gives Codex and human contributors a shared project memory: goals, decisions,
features, dependencies, security checks, testing expectations, and asset rules.

Copy these files into a new project when you want Codex to work with clearer
context from the beginning.

## What This Setup Provides

- A repo-level `AGENTS.md` file with working rules for Codex.
- A `docs/` folder for project knowledge that should survive across tasks.
- Reusable Markdown templates for features, milestones, dependencies, assets,
  and security checks.
- Optional game-development examples, such as documenting spritesheet layouts,
  mechanics, and asset pipelines.

## Recommended Project Structure

```text
.
|-- AGENTS.md
|-- README.md
`-- docs/
    |-- ASSETS.md
    |-- CHANGELOG.md
    |-- CODEX_WORKFLOW.md
    |-- DEPENDENCIES.md
    |-- FEATURES.md
    |-- MILESTONES.md
    |-- PROJECT_OVERVIEW.md
    |-- SECURITY.md
    |-- TESTING.md
    `-- templates/
        |-- asset-entry.md
        |-- dependency-entry.md
        |-- feature-entry.md
        |-- milestone-entry.md
        `-- security-check-entry.md
```

## How To Use It

1. Copy `AGENTS.md` and the `docs/` folder into a new or existing project.
2. Fill in `docs/PROJECT_OVERVIEW.md` before asking Codex to make larger
   changes.
3. Ask Codex to update the relevant docs whenever it implements or changes
   behavior.
4. Keep dependencies, test expectations, and security assumptions explicit.

## Suggested First Prompt

```text
Read AGENTS.md and the docs folder. Then inspect the current project and update
docs/PROJECT_OVERVIEW.md, docs/FEATURES.md, docs/DEPENDENCIES.md, and
docs/TESTING.md with the current state. Do not invent missing facts; mark
unknowns clearly.
```

## Documentation Principles

- Keep documentation close to decisions and behavior.
- Prefer structured entries over scattered notes.
- Record why a system exists, what depends on it, and how to verify it.
- Treat assets, mechanics, workflows, and security assumptions as project
  knowledge, not temporary task context.
- When a change affects tests or risk, update the docs before considering the
  task complete.

## Good Fit

This setup works for software projects, tools, websites, games, experiments,
and internal systems. The game-development sections are optional examples, not
requirements.
# StandardProjectSetup
