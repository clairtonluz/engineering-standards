---
name: luz-tdd
description: Apply Test-Driven Development practices across any project, language, framework, or architecture. Use when Codex is implementing features, fixing bugs, refactoring behavior, adding validations, changing APIs, or when the user requests TDD, test-first development, red-green-refactor, regression tests, or behavior-driven code changes.
---

# TDD

Use this skill to guide code work with a test-first red-green-refactor cycle while preserving existing behavior.

## Default Stance

- Prefer tests before production code whenever the task changes observable behavior.
- Start by identifying expected behavior, acceptance criteria, edge cases, and regressions.
- Follow the project's existing test structure, naming, framework, and command conventions.
- Add new focused tests without asking for permission.
- Preserve existing behavior unless the user explicitly asks to change it.
- Avoid overengineering until a failing test proves the need.
- Do not claim tests passed unless they were executed successfully.
- If TDD is not applicable, such as a documentation-only or exploratory task, say so briefly and proceed normally.

## Existing Tests Policy

Do not modify, delete, weaken, skip, or rewrite existing tests without permission.

Before changing an existing test, explain and ask for permission. Include:

- Which existing test needs to change.
- Why the current test is incorrect, outdated, flaky, duplicated, or incompatible with the requested behavior.
- The exact change proposed.
- Whether the change could hide a regression.
- Why adding a new test is not enough.

After permission is granted, keep the test change as small and explicit as possible. If permission is not granted, leave existing tests unchanged and report the constraint.

## Workflow

1. Identify the behavior being added, changed, or fixed.
2. Inspect the existing test layout, naming style, fixtures, helpers, and commands.
3. Add a new failing test for the expected behavior or regression.
4. Run the smallest relevant test target when feasible to confirm the test fails for the right reason.
5. Implement the simplest production code that should make the test pass.
6. Run the relevant test target again.
7. Refactor production code and newly added test code only after tests pass.
8. Run the relevant tests after refactoring, then broader tests when risk or project conventions justify it.

If confirming the red phase is not feasible, explain why, keep the test focused, and continue with the smallest safe implementation.

## Test Design

- Test observable behavior, not private methods or internal structure.
- Use clear names that describe the scenario and expected result.
- Structure tests with Arrange / Act / Assert or Given / When / Then.
- Keep one main behavior per test.
- Cover happy paths, edge cases, validation errors, failure scenarios, and regressions when relevant.
- Keep test data minimal, explicit, deterministic, and local to the test.
- Ensure tests are isolated and do not depend on execution order.
- Avoid sleeps, randomness, external services, and environment-specific assumptions.
- Avoid brittle checks tied to timing, ordering, exact formatting, or implementation details unless they are part of the contract.
- Use factories, builders, fixtures, or helpers only when they improve readability without hiding important behavior.
- Prefer fast unit tests for business rules; add integration or end-to-end tests when behavior crosses important boundaries.

## Doubles and Boundaries

- Use mocks, stubs, or fakes mainly at system boundaries: external APIs, databases, queues, file systems, clocks, network calls, authentication providers, and third-party services.
- Prefer real domain objects and lightweight fakes over excessive mocking.
- Avoid testing framework behavior, trivial getters/setters, or behavior already covered elsewhere.

## Bug Fixes

- Add a regression test that reproduces the bug before changing production code.
- Make the regression test fail for the observed bug when feasible.
- Fix only the behavior required by the failing test and acceptance criteria.
- Keep nearby behavior unchanged unless the user requested a broader change.

## Refactoring

- Treat refactoring as behavior-preserving.
- Add characterization tests first when existing coverage is insufficient and behavior must be protected.
- Do not alter existing tests to fit a refactor unless permission is granted under the Existing Tests Policy.
- Run tests before and after refactoring when feasible.

## Reporting

When summarizing work, separate these items:

- Tests added.
- Production changes.
- Existing test changes requested.
- Commands run, with pass/fail status.
- Tests not run and why.
- Remaining risks or follow-up items.
