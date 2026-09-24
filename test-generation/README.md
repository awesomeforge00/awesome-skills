# Test Generation

A reusable agent skill for creating automated tests that verify real behavior and prevent regressions.

## What it does

Test Generation helps an AI coding agent turn a behavior, bug, or acceptance criterion into a focused test suite. It guides the agent to:

- Discover the repository's existing testing conventions
- Identify observable behavior and important contracts
- Cover normal, boundary, invalid, and dependency-failure cases
- Choose an appropriate unit, integration, or end-to-end test level
- Write deterministic tests through public interfaces
- Validate tests incrementally and distinguish code failures from environment failures

## When to use it

Use this skill when an agent needs to:

- Add tests for new functionality
- Create a regression test for a bug
- Improve missing or weak test coverage
- Design edge-case and error-path tests
- Add tests around an unfamiliar module or service
- Review whether existing tests would catch a likely regression

It works especially well after [Repository Explorer](../repository-explorer/) has identified the relevant implementation, fixtures, and test commands.

## Test design principles

- **Behavior first:** test what users and callers can observe.
- **Focused:** each test should explain one important behavior.
- **Deterministic:** control time, randomness, network, filesystem, and environment state.
- **Layered:** use the fastest test level that proves the behavior.
- **Boundary-aware:** include empty, minimal, maximal, invalid, and failure inputs when relevant.
- **Maintainable:** follow local conventions and avoid brittle implementation details.
- **Honest:** never weaken an assertion simply to make a test pass.

## Expected output

The skill produces:

1. A summary of the behavior under test
2. The repository's relevant testing conventions
3. A concise behavior matrix
4. Tests added at the appropriate level
5. Focused and broader validation results
6. Remaining coverage gaps or test-environment limitations

## Structure

- [SKILL.md](SKILL.md) — test-design workflow, rules, and reporting format.
