# Dependency Map

Use this file to document relationships between systems, features, assets,
packages, data, and workflows. The goal is to help Codex identify what else must
be tested or reviewed when something changes.

## Dependency Index

| Source | Depends On | Type | Risk |
| --- | --- | --- | --- |
| Documentation workflow | `docs/` structure | Internal process | Low |

## Dependency Entries

### Documentation Workflow -> Docs Structure

- Source: Documentation workflow
- Depends on: `docs/` structure
- Dependency type: Internal process
- Contract: Codex and contributors use the same documentation files for project
  memory.
- Risk if changed: Documentation updates may become scattered or incomplete.
- Required checks:
  - Verify referenced docs exist.
  - Verify `AGENTS.md` points to the current docs.
- Related docs:
  - `AGENTS.md`
  - `docs/CODEX_WORKFLOW.md`
  - `docs/TESTING.md`

## Dependency Types

- Internal process
- Code module
- API contract
- Database or schema
- External package
- External service
- Asset convention
- Build or deployment step
- Security control

## Dependency Review Prompt

```text
Before changing this system, inspect docs/DEPENDENCIES.md. Identify direct and
indirect dependents, update affected tests, and record any new or changed
dependencies.
```
