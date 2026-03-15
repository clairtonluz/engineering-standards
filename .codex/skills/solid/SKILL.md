---
name: solid
description: Apply SOLID principles when analyzing, reviewing, refactoring, or changing application code and design.
---

# SOLID Skill

## Purpose

Use this skill when the task involves code structure, object design, abstractions, dependency boundaries, or refactoring for maintainability.

This skill enforces:
- single responsibility
- open/closed design
- substitutability across abstractions
- interface segregation
- dependency inversion at boundaries

## Core Rules

### Single Responsibility Principle

Prefer classes, modules, and functions with one clear reason to change.

Avoid:
- god classes
- mixed orchestration and low-level detail
- utility dumping grounds
- handlers that combine validation, business rules, persistence, and presentation

### Open/Closed Principle

Prefer extension through composition, polymorphism, or isolated collaborators instead of risky modification to unrelated code.

Avoid:
- spreading conditionals across the codebase for each new variant
- changing stable code in multiple places when one extension point would do
- speculative abstractions with no current use

### Liskov Substitution Principle

Preserve substitutability between interfaces, implementations, and inherited types.

Avoid:
- implementations that weaken guarantees expected by callers
- surprise exceptions or behavior changes in subtype implementations
- inheritance where composition would be safer

### Interface Segregation Principle

Keep interfaces focused on a cohesive responsibility.

Avoid:
- broad contracts that force consumers to depend on methods they do not need
- adapters that expose unrelated concerns through one interface

### Dependency Inversion Principle

Depend on abstractions at architectural boundaries.

Prefer:
- ports/interfaces for persistence, external services, and framework integrations
- constructor injection
- core logic that is independent from frameworks and transport details

Avoid:
- business logic depending directly on concrete infrastructure classes
- hidden global dependencies
- field injection

## Decision Heuristics

Before changing the design, ask:
1. What is the single responsibility of this module?
2. Is the new behavior an extension or a modification?
3. Are callers able to rely on the abstraction safely?
4. Is the interface cohesive for this consumer?
5. Can this dependency point to an abstraction instead of a concrete implementation?

## Review Checklist

Verify that:
- responsibilities are cohesive
- extension points are justified
- abstractions are not leaky
- interfaces are narrow and purposeful
- dependencies point toward abstractions
- the change reduces coupling instead of relocating it

## Output Expectations

When using this skill:
1. identify the SOLID concern involved
2. explain the structural issue briefly
3. make the smallest safe design change
4. preserve behavior unless change is required
5. mention trade-offs if the abstraction has cost
