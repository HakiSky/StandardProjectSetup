# Asset Guide

Use this file to document asset conventions, naming rules, import workflows, and
runtime assumptions. This is especially useful for game projects, generated
media, design systems, and content-heavy applications.

## Asset Index

| Asset Or Convention | Type | Used By | Notes |
| --- | --- | --- | --- |
| Example spritesheet layout | Convention | Animation readers | Optional game-dev example |

## General Asset Rules

- Store source assets and generated assets in clearly named folders.
- Document naming conventions before relying on them in code.
- Record dimensions, frame counts, export settings, and compression assumptions
  when they affect runtime behavior.
- Link asset conventions to dependent systems in `docs/DEPENDENCIES.md`.

## Example: Spritesheet Animation Convention

- Asset type: Spritesheet
- Frame order: Left to right, top to bottom
- Columns: TODO
- Rows: TODO
- Frame size: TODO
- Animation mapping:
  - Row 1: Idle
  - Row 2: Walk
  - Row 3: Attack
  - Row 4: Hurt
- Runtime reader: TODO
- Dependent systems:
  - Animation controller
  - Character renderer
- Validation:
  - Confirm frame dimensions match the configured reader.
  - Confirm animation names match runtime expectations.
  - Confirm transparent background and padding rules.

## Codex Asset Prompt

```text
Document any new asset conventions in docs/ASSETS.md. If code depends on the
asset layout, also update docs/DEPENDENCIES.md and docs/TESTING.md.
```
