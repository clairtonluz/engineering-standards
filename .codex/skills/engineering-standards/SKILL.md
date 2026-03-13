---
name: engineering-standards
description: Apply SOLID, Clean Code, Clean Architecture, secure coding, and safe refactoring practices when analyzing, creating, or modifying software.
---

# Engineering Standards Skill

## Purpose

Use this skill whenever working on application code, architecture, refactoring, new features, bug fixes, tests, code review, or technical documentation.

This skill enforces:
- SOLID principles
- Clean Code practices
- Clean Architecture boundaries
- secure-by-default implementation
- testability and maintainability
- minimal, well-justified changes
- production-ready quality

## Core Principles

### 1. SOLID
Always prefer designs that:
- keep classes and functions focused on one reason to change
- are open for extension but closed for risky modification
- preserve substitutability across abstractions and implementations
- keep interfaces small and cohesive
- depend on abstractions rather than concretions

### 2. Clean Code
Always aim for code that is:
- easy to read
- intention-revealing
- small in scope
- low in duplication
- explicit over clever
- consistent with local project conventions
- simple before generic

### 3. Clean Architecture
Preserve clear boundaries:
- domain rules must not depend on frameworks
- application/use-case layer orchestrates business flows
- infrastructure adapts external concerns
- UI/controllers only translate input/output
- dependencies must point inward toward business rules

## Mandatory Rules

### Rule A — Understand before changing
Before modifying code:
1. inspect the existing structure
2. identify the layer and responsibility of the target code
3. determine whether the change belongs in domain, application, infrastructure, or interface layer
4. avoid patching symptoms in the wrong layer

### Rule B — Smallest safe change
Prefer the smallest change that:
- solves the root cause
- preserves existing behavior
- does not introduce speculative abstractions
- is easy to review and test

### Rule C — Refactor only with purpose
Refactor when it improves:
- correctness
- readability
- testability
- separation of concerns
- dependency direction
- security
Do not perform broad refactors without strong justification.

### Rule D — Naming
Use names that:
- reflect business meaning
- avoid vague words like Manager, Util, Helper, Processor, Data, Thing
- distinguish command/query responsibilities when useful
- match ubiquitous language of the domain

### Rule E — Functions and methods
Prefer methods/functions that are:
- small
- cohesive
- at one abstraction level
- explicit in inputs/outputs
Avoid:
- hidden side effects
- boolean flag explosions
- long parameter lists
- mixing orchestration with low-level details

### Rule F — Classes and modules
Prefer modules with:
- one clear responsibility
- explicit boundaries
- constructor-injected dependencies
- framework concerns isolated from core rules

Avoid:
- god classes
- anemic service blobs
- static utility dumping grounds
- feature leakage across layers

### Rule G — Error handling
Handle errors with:
- domain-meaningful exceptions/results
- contextual messages for operators/developers
- no swallowed exceptions
- no generic catch blocks without reason
- no leaking sensitive internal details to external responses

### Rule H — Security
Always apply secure defaults:
- validate inputs at boundaries
- encode/escape outputs where appropriate
- use least privilege
- never hardcode secrets
- avoid insecure cryptography
- avoid logging secrets, tokens, passwords, personal data
- preserve authorization checks
- preserve tenant/user scoping rules
- keep deserialization, file handling, and network access constrained

### Rule I — Tests
When changing behavior:
- add or update tests
- prefer unit tests for domain logic
- add integration tests for boundaries/adapters when relevant
- do not rewrite unrelated tests
- ensure tests describe behavior, not implementation trivia

### Rule J — Documentation
When the change affects architecture or behavior:
- update the nearest relevant documentation
- explain why the change exists
- keep docs concise and actionable

## Architectural Guidance

### Domain Layer
Should contain:
- entities
- value objects
- domain services
- domain rules
Should not contain:
- framework annotations unless unavoidable
- database concerns
- HTTP concerns
- external SDK logic

### Application Layer
Should contain:
- use cases
- orchestration
- transaction boundaries where appropriate
- ports/interfaces for external dependencies

### Infrastructure Layer
Should contain:
- database adapters
- messaging adapters
- HTTP clients
- filesystem/external provider integrations
Should implement application/domain contracts instead of driving business rules.

### Interface Layer
Should contain:
- controllers
- presenters
- DTO mapping
- request/response translation
Should not contain business rules.

## Decision Heuristics

When adding new logic, ask:
1. Is this a business rule or a technical detail?
2. Which layer owns this decision?
3. Can this dependency be inverted behind an interface?
4. Is this abstraction solving a real need now?
5. Would another engineer understand this quickly?
6. Can this be tested in isolation?
7. Does this increase coupling unnecessarily?

## Code Review Checklist

For each change, verify:
- responsibility is in the correct layer
- abstractions are justified
- names are clear
- duplication was reduced, not relocated
- dependencies point inward
- side effects are controlled
- authorization/validation still hold
- logs do not expose sensitive data
- tests cover the intended behavior
- change is minimal and reviewable

## Refactoring Patterns to Prefer

Prefer:
- extract method
- extract class when responsibility is mixed
- introduce port/interface at boundaries
- replace conditionals with polymorphism only when complexity justifies it
- replace primitive obsession with value objects when domain meaning benefits
- isolate framework code from business rules
- move mapping logic to dedicated mappers when needed

Avoid:
- speculative generalization
- deep inheritance trees
- unnecessary design patterns
- premature modularization
- overuse of factories/builders where simple constructors are enough

## Output Expectations

When producing code changes:
1. briefly explain the issue
2. explain the architectural reasoning
3. implement the fix with minimal safe change
4. add/update tests
5. mention trade-offs if relevant

When reviewing code:
- identify violations of SOLID, Clean Code, Clean Architecture, or security
- classify issues by severity
- suggest concrete improvements
- prefer actionable review comments over generic criticism

## Language-Specific Defaults

### For Java
Prefer:
- constructor injection
- immutable value objects where possible
- package-by-feature when suitable
- records for simple immutable carriers when appropriate
- checked vs unchecked exceptions intentionally, not by habit
- Bean Validation at input boundaries
- transactional boundaries in application/service orchestration, not entities
- MapStruct/manual mappers when useful, but avoid needless mapping layers

Avoid:
- field injection
- massive service classes
- JPA entities used directly as API contracts
- leaking persistence annotations into core domain when avoidable
- generic Exception/RuntimeException without meaning

### For TypeScript / JavaScript
Prefer:
- strict typing
- narrow interfaces
- explicit return types in important boundaries
- pure functions for domain logic
- schema validation at external input boundaries
- separation between transport DTOs and domain models

Avoid:
- any without justification
- giant service files
- business rules in controllers/components
- hidden mutation

## Anti-Patterns

Flag and avoid:
- god object
- shotgun surgery
- feature envy
- primitive obsession
- tight framework coupling
- magic strings
- copy-paste programming
- mixed abstraction levels
- silent fallback behavior that hides defects
- infrastructure-driven domain design

## Final Standard

Every change should optimize for:
- correctness
- clarity
- maintainability
- testability
- security
- architectural integrity

If there is tension between cleverness and clarity, choose clarity.
If there is tension between speed and safety, choose the safest change that still solves the problem.
If there is tension between ideal architecture and pragmatic delivery, preserve boundaries as much as reasonably possible and document trade-offs.