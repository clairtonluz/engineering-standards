---
name: clean-code
description: Apply clean code practices for readability, naming, cohesion, maintainability, and low-risk refactoring.
---

# Clean Code Skill

## Purpose

Use this skill when the task involves improving readability, naming, function design, local refactoring, reducing duplication, or reviewing implementation quality.

This skill enforces:
- intention-revealing names
- small cohesive functions
- low duplication
- explicit code over clever code
- consistent local conventions

## Core Rules

### Understand Before Changing

Before modifying code:
1. inspect the existing structure
2. identify the responsibility of the target code
3. confirm the root cause instead of patching symptoms

### Smallest Safe Change

Prefer the smallest change that:
- solves the root cause
- preserves public behavior unless explicitly changed
- is easy to review
- is easy to test

### Naming

Use names that:
- reflect business meaning
- reveal intent
- match local domain language

Avoid vague words like:
- `Manager`
- `Util`
- `Helper`
- `Processor`
- `Thing`
- `Data`

### Functions And Methods

Prefer functions that are:
- small
- cohesive
- at one abstraction level
- explicit in inputs and outputs

Avoid:
- hidden side effects
- boolean flag explosions
- long parameter lists
- mixing orchestration with low-level detail

### Modules And Classes

Prefer modules with one clear purpose.

Avoid:
- large classes with unrelated responsibilities
- static dumping grounds
- duplication spread across similar flows

### Error Handling

Handle errors with:
- meaningful messages
- contextual detail for maintainers
- no swallowed exceptions
- no broad catch blocks without reason

Do not leak internal details to external consumers.

### Documentation

When behavior or design changes, update the nearest relevant documentation and keep it concise.

## Refactoring Patterns To Prefer

Prefer:
- extract method
- extract class when responsibilities are mixed
- isolate side effects
- simplify branching when it improves readability
- remove duplication by clarifying ownership

Avoid:
- broad rewrite churn
- speculative generalization
- pattern-heavy code without a real need

## Review Checklist

Verify that:
- names are clear
- functions are cohesive
- code is readable without mental jumps
- duplication was removed instead of moved
- side effects are controlled
- the diff stays focused

## Output Expectations

When using this skill:
1. explain the readability or maintainability issue
2. show why the existing code is hard to change safely
3. implement the smallest clear improvement
4. update tests when behavior changes
5. mention any readability vs indirection trade-off
