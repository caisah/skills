---
name: simplifying-code
description: Simplify and refactor existing code with deep nesting, hidden state, or mixed abstraction levels. Use when reviewing or refactoring a specific function or module for readability and maintainability.
---

# Simplifying Code

Apply a low-risk simplification workflow to existing code. Favor changes that reduce mental overhead without changing behavior.

## Workflow

1. Inspect the target code and identify the highest-cost complexity signals.
2. Read [resources/pillars.md](resources/pillars.md) and select the 2-4 pillars that best match the observed complexity.
3. Rank the candidate pillars by readability gain, behavior risk, and edit scope, then keep only the smallest relevant set.
4. From those selected pillars, derive candidate changes and choose 1-3 simplifications with the highest readability gain and lowest behavior risk.
5. Prefer local, behavior-preserving refactors over broad structural rewrites.
6. Apply the smallest set of changes that materially improves readability and maintainability.
7. Summarize what changed, why it is simpler, and what was intentionally left unchanged.

## Guidelines

Apply the framework through the selected pillars only. Do not try to optimize for all pillars at once.

Prefer changes that:

- Reduce branching and nesting first.
- Keep each function at one level of abstraction.
- Make data flow explicit.
- Minimize mutable state and state combinations.
- Remove trivial indirection.
- Name functions and variables by intent.

Avoid changes that:

- Extract helpers that only rename complexity.
- Introduce abstractions with no immediate payoff.
- Spread one concept across more files than before.
- Replace local simplification with a broad redesign.
- List or discuss all pillars when only a few are relevant.
- Force a change to satisfy a pillar without concrete evidence in the code.

## Output Format

Return a concise result with:

1. The main complexity issues found.
2. The selected pillars.
3. The evidence in the code for each selected pillar.
4. The changes chosen and how they map to those pillars.
5. Why those changes make the code simpler.
6. Any remaining tradeoffs or follow-up opportunities.

## Reference Files

- For the full simplification framework, see [resources/pillars.md](resources/pillars.md).
- For before/after refactoring patterns, see [resources/examples.md](resources/examples.md).
- For scientific bases and citation hints, see [resources/references.md](resources/references.md).
