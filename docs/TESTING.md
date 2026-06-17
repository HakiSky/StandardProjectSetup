# Testing Guide

Use this file to document how the project should be verified. Include automated
tests, manual checks, regression areas, and dependency-aware scenarios.

## Test Strategy

- Unit tests: TODO
- Integration tests: TODO
- End-to-end tests: TODO
- Manual checks: TODO
- Visual checks: TODO
- Security checks: TODO

## Required Checks By Change Type

| Change Type | Required Checks |
| --- | --- |
| Documentation-only | Verify links and referenced files |
| Feature behavior | Unit or integration tests plus changelog and feature docs |
| Shared dependency | Tests for direct and dependent systems |
| Security-sensitive behavior | Security review plus targeted tests |
| Asset or game content | Asset validation plus manual or visual verification |
| Build or deployment | Build command plus release checklist |

## Regression Areas

- TODO

## Dependency-Aware Testing

Before testing a change:

1. Check `docs/DEPENDENCIES.md`.
2. Identify systems that depend on the changed behavior.
3. Add or run tests that cover those dependents.
4. Record new regression areas here when discovered.

## Manual Test Entry Format

```md
### Scenario: TODO

- Change under test: TODO
- Setup: TODO
- Steps:
  - TODO
- Expected result: TODO
- Related dependencies:
  - TODO
```
