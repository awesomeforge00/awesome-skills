# Co-Plan (`coplan`)

An interactive collaborative planning skill for stress-testing, refining, and clarifying plans, architectural choices, and technical decisions through structured question rounds and decision tracking.

## Overview

When planning complex tasks, jumping straight into execution often leads to hidden assumptions, rework, and unaddressed trade-offs. The `coplan` skill guides the assistant to act as a structured co-planner that models choices as a decision tree, unblocking questions in logical rounds until full alignment is achieved.

## Structure

- [SKILL.md](SKILL.md) — Main prompt instructions defining the co-planning methodology, round structure, and stopping conditions.

## How It Works

1. **Decision Tree Modeling**: Tracks decisions hierarchically. Decisions that depend on earlier choices are held until their prerequisites are settled.
2. **Batched Question Rounds**: Asks focused, numbered questions in manageable batches.
3. **Structured Choices & Recommendations**: For every question, provides:
   - Specific choices or trade-offs.
   - A concrete recommendation with rationale.
   - An easy way to accept, override, propose alternatives, or request further analysis.
4. **Distinction of Concepts**: Keeps facts, assumptions, recommendations, and finalized decisions strictly distinct.
5. **Autonomous Fact Checking**: Checks facts directly using available environment tools instead of asking questions for details that can be looked up.
6. **Convergence**: Completes when all relevant decisions are resolved, followed by a concise summary of decided items and any remaining uncertainties.

## When to Use

- Architectural and system design choices before writing code.
- Scoping out multi-step refactors or migration plans.
- Defining project requirements or feature specs with ambiguous constraints.
- Brainstorming and stress-testing new product or technical ideas.
