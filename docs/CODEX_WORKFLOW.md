# Codex Workflow Guide

Use these prompts and habits to keep Codex aligned with the project memory in
this repository.

## Before Starting A New Project

```text
Read AGENTS.md and every file in docs/. Summarize the current project state,
unknowns, and the docs that should be updated first. Do not change files yet.
```

## Before Implementing A Feature

```text
Before implementing this feature, inspect docs/PROJECT_OVERVIEW.md,
docs/FEATURES.md, docs/DEPENDENCIES.md, and docs/TESTING.md. Then explain the
affected systems, required tests, and documentation updates.
```

## During Implementation

```text
Implement the feature and update the relevant docs in the same change. Update
the changelog and feature documentation for this change.
```

## Dependency-Aware Work

```text
Before implementing this system, inspect docs/DEPENDENCIES.md and update
affected tests. Record any new dependencies introduced by the change.
```

## Asset Or Game-Development Work

```text
Document any new asset conventions in docs/ASSETS.md. If a mechanic or runtime
system depends on the asset layout, update docs/DEPENDENCIES.md and
docs/TESTING.md too.
```

## Security-Sensitive Work

```text
Review this change against docs/SECURITY.md. Identify any new sensitive
surfaces, update security notes, and include security checks in the final
verification summary.
```

## Documentation Refresh

```text
Inspect the repository and refresh the docs folder. Keep existing facts, remove
stale claims, mark unknowns clearly, and add missing dependency or testing notes.
```

## Review Prompt

```text
Review this change for bugs, missing tests, stale documentation, dependency
impact, and security risk. Lead with findings and reference exact files.
```
