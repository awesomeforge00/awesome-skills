---
name: repository-explorer
description: Use this skill when a task requires understanding an unfamiliar repository, locating the code path that owns a behavior, identifying project conventions, or finding relevant tests and configuration before making changes.
---

# Repository Explorer

Build a focused, evidence-based understanding of a repository before implementation work begins.

## Operating procedure

### 1. Establish the task anchor

Identify the most concrete starting point available:

- A named file, symbol, command, error, failing test, or user-visible behavior
- The nearest call site or implementation when the starting point only forwards or registers behavior
- The expected outcome and the smallest behavior that must be understood

If the request is ambiguous, ask one focused clarification question after checking facts that can be found in the repository.

### 2. Map the repository briefly

Inspect only enough of the top level to identify:

- Application or package entry points
- Source, test, configuration, and documentation directories
- Package manifests, build files, and CI configuration
- The language, framework, and test tooling in use

Prefer targeted file listing and search over reading every file.

### 3. Trace the relevant behavior

Follow the task from its anchor through the nearest controlling code path:

- Search for the symbol, route, command, configuration key, or error text
- Read the owning implementation and its immediate callers or dependencies
- Find the closest tests, fixtures, examples, and configuration
- Check recent history only when current code leaves behavior or intent unclear

Stop exploring once the behavior can be explained and a small implementation or validation step is identifiable.

### 4. Extract local conventions

Record conventions that affect the planned work, such as:

- Naming, module boundaries, and public API patterns
- Error handling and logging style
- Test organization and how tests are run
- Formatting, linting, type checking, and build commands
- Documentation or generated-file expectations

Treat observed code as evidence, not as a reason to expand the task unnecessarily.

### 5. Produce a compact handoff

Return the findings using this structure:

## Scope

What behavior or task was investigated.

## Relevant files

The smallest set of files that control or validate the behavior, with each file's role.

## Execution path

A short explanation of how the behavior currently flows through the code.

## Local conventions

Patterns the implementation should follow.

## Validation

The cheapest focused command or test that could confirm or disconfirm the working hypothesis.

## Open questions

Only unresolved questions that block a safe implementation. Mark assumptions separately from repository facts.

## Recommended next step

The smallest grounded edit or investigation that should happen next.

## Exploration rules

- Do not modify files while performing repository exploration.
- Do not scan unrelated directories or summarize the entire repository.
- Prefer one nearby hop at a time from the current anchor.
- Separate facts, hypotheses, assumptions, and recommendations.
- Do not claim a behavior is understood without identifying the code or test that supports it.
- When evidence is insufficient, say what is missing and choose the cheapest check that would resolve it.
- Preserve user changes and work with a dirty working tree.
