# Repository Explorer

A reusable agent skill for understanding an unfamiliar codebase quickly and safely before making changes.

## What it does

Repository Explorer helps an AI coding agent move from a task request to the smallest useful understanding of the relevant code path. It focuses on:

- Finding the implementation that actually controls a behavior
- Locating related tests, configuration, and documentation
- Learning local conventions before editing
- Separating repository facts from hypotheses and assumptions
- Producing a concise handoff for implementation or debugging

## When to use it

Use this skill when an agent needs to:

- Start work in an unfamiliar repository
- Find where a feature, bug, command, or configuration value is implemented
- Identify the right tests before changing code
- Understand project conventions and validation commands
- Reduce repeated or unfocused repository exploration

It is especially useful as the first step before refactoring, debugging, code review, dependency upgrades, or feature development.

## Expected output

The skill produces a focused report covering:

1. Scope of the investigation
2. Relevant files and their roles
3. The current execution path
4. Local coding and testing conventions
5. A focused validation command
6. Open questions and assumptions
7. The recommended next step

## Design goals

- **Bounded:** exploration ends when the relevant behavior is understood.
- **Evidence-based:** conclusions point back to files, symbols, tests, or commands.
- **Tool-aware:** the skill uses the search and inspection tools available in the host agent.
- **Composable:** the output can hand off cleanly to implementation, debugging, review, or planning skills.
- **Non-destructive:** exploration does not modify the repository.

## Structure

- [SKILL.md](SKILL.md) — operating procedure and handoff format.
