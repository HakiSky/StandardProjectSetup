# Codex Project Instructions

These instructions apply to the entire repository unless a more specific
`AGENTS.md` exists in a subdirectory.

## Working Style

- Read the relevant files in `docs/` before implementing non-trivial changes.
- Keep implementation changes and documentation changes aligned.
- Prefer small, structured documentation updates over long narrative notes.
- Do not invent project facts. If something is unknown, write `Unknown` or add a
  clear TODO.
- Preserve existing user work. Do not rewrite unrelated documentation or code.

## Documentation Responsibilities

When a task changes project behavior, update the docs that describe it:

- Update `docs/CHANGELOG.md` for user-visible or meaningful internal changes.
- Update `docs/FEATURES.md` when adding or changing systems, workflows,
  mechanics, or reusable patterns.
- Update `docs/DEPENDENCIES.md` when one feature, system, asset, package, or
  workflow depends on another.
- Update `docs/TESTING.md` when test coverage, manual checks, regression areas,
  or validation strategy changes.
- Update `docs/ASSETS.md` when adding asset conventions, file naming rules,
  spritesheet layouts, import pipelines, or generated media.
- Update `docs/SECURITY.md` when secrets, permissions, authentication,
  authorization, networking, file handling, user data, or deployment risk is
  involved.
- Update `docs/MILESTONES.md` when work completes a milestone or changes planned
  acceptance criteria.

## Dependency Awareness

Before changing shared behavior:

1. Check `docs/DEPENDENCIES.md` for affected systems.
2. Identify direct and indirect dependents.
3. Update or add tests for the affected dependency chain.
4. Record any new dependency or changed contract.

## Testing Expectations

- Run the narrowest useful test set for the change.
- If tests cannot be run, document the reason in the final response.
- When a change affects documented dependencies, include dependent systems in
  the test plan.
- For game or asset changes, include a manual verification step when automated
  tests cannot validate the result.

## Security Expectations

- Never commit secrets, tokens, private keys, personal credentials, or production
  data.
- Prefer environment variables or secret managers for sensitive configuration.
- Treat file uploads, external URLs, shell commands, authentication, database
  access, and generated code as security-sensitive surfaces.
- If security impact is uncertain, add a note to `docs/SECURITY.md` and call it
  out in the final response.

## Changelog Style

Use short, human-readable entries grouped by date. Prefer:

```md
## YYYY-MM-DD

- Added ...
- Changed ...
- Fixed ...
```

## Final Response Checklist

When completing a task, summarize:

- What changed.
- Which docs were updated.
- Which tests or checks were run.
- Any known risks, unknowns, or follow-up work.
