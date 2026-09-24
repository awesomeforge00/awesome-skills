---
name: test-generation
description: Use this skill when writing, improving, or extending automated tests for existing behavior. Covers test discovery, behavior-based case design, boundary and failure cases, fixtures, mocking, deterministic tests, and focused validation.
---

# Test Generation

Create automated tests that specify observable behavior, expose meaningful failures, and fit the repository's existing testing practices.

## Operating procedure

### 1. Establish the behavior under test

Identify the concrete anchor:

- A function, class, endpoint, command, component, or user-visible workflow
- A bug report, acceptance criterion, failing test, or recent behavior change
- The inputs, outputs, side effects, errors, and state transitions that matter

Read the implementation and its nearest callers only far enough to understand the public behavior. Do not design tests from names or comments alone.

### 2. Inspect the local test system

Find and follow the repository's existing conventions:

- Test framework, runner, and configuration
- Test directory and file naming patterns
- Fixtures, factories, helpers, and shared setup
- Mocking, dependency injection, and assertion style
- Commands used in CI and by nearby tests
- Coverage, snapshot, property-based, or integration-test conventions

Reuse existing helpers when they express the same setup clearly. Do not introduce a new testing library or pattern without a reason.

### 3. Define the behavior matrix

Before writing code, list the smallest useful cases:

- Normal or representative input
- Empty, missing, null, or minimal input
- Lower and upper boundaries
- Invalid input and expected validation behavior
- Dependency failures, timeouts, or unavailable resources
- Repeated calls, ordering, state changes, or idempotency when relevant
- Security-sensitive or permission-sensitive cases when relevant

Prioritize cases that distinguish correct behavior from likely regressions. Avoid generating large numbers of near-duplicate tests.

### 4. Choose the narrowest useful test level

Prefer the fastest level that proves the behavior:

- Unit test for isolated logic and clear input/output contracts
- Integration test for module, database, filesystem, queue, or service boundaries
- End-to-end test for critical user workflows that cannot be proven lower in the stack

Keep external systems out of unit tests unless their integration is the behavior being tested. Use realistic fakes or narrowly scoped mocks at boundaries, and assert meaningful interactions only when they are part of the contract.

### 5. Write clear, deterministic tests

Each test should:

- Describe one behavior in its name
- Arrange only the state it needs
- Act through the public interface under test
- Assert outcomes, errors, state, or side effects that users depend on
- Clean up resources and avoid order dependence
- Use controlled time, randomness, network, and environment state

Do not weaken an assertion merely to make a test pass. If the expected behavior is unclear, identify the ambiguity rather than encoding a guess.

### 6. Run and interpret incrementally

Run the nearest test or smallest relevant test selection first, then the broader suite when practical. When a test fails:

1. Confirm the failure reproduces.
2. Decide whether the test, implementation, fixture, or environment is responsible.
3. Fix the root cause or clarify the expected contract.
4. Rerun the focused test before expanding validation.

Report skipped tests, flaky behavior, unavailable dependencies, and unrelated failures separately from test results.

### 7. Review the test value

Before finishing, check that the tests:

- Would fail for the regression they are meant to prevent
- Do not only reproduce the implementation's current mechanics
- Cover the important success and failure paths
- Avoid unnecessary duplication and brittle exactness
- Follow the repository's naming, layout, and tooling conventions

## Output format

When planning or reporting test work, use this structure:

## Behavior under test

The contract or workflow being verified.

## Existing test conventions

The local framework, layout, helpers, and relevant commands.

## Test cases

A concise behavior matrix showing normal, boundary, invalid, and failure cases as applicable.

## Implementation

The tests added or the smallest next test change, including the files involved.

## Validation

Commands run and their results. Separate focused results, full-suite results, and pre-existing failures.

## Remaining gaps

Important behavior that is not covered and why.

## Rules

- Do not modify production behavior just to satisfy a test unless the task explicitly requires it.
- Do not add tests for behavior that has not been established by requirements, implementation, or existing conventions.
- Prefer public behavior over private implementation details.
- Keep tests isolated, repeatable, and independent of execution order.
- Avoid excessive mocking; mock only unstable or external boundaries.
- Preserve existing user changes and do not rewrite unrelated tests.
- Treat a passing test as evidence for one behavior, not proof that the entire feature is correct.
