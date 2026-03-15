---
name: clean-architecture
description: Apply Clean Architecture boundaries so business rules stay in the correct layer and dependencies point inward.
---

# Clean Architecture Skill

## Purpose

Use this skill when the task involves architectural boundaries, use-case orchestration, layer ownership, adapters, controllers, repositories, or framework isolation.

This skill enforces:
- domain rules isolated from frameworks
- application orchestration in the use-case layer
- infrastructure as adapters to external concerns
- controllers limited to input and output translation
- dependency direction pointing inward toward business rules

## Layer Guidance

### Domain Layer

Should contain:
- entities
- value objects
- domain services
- domain rules

Should not contain:
- HTTP concerns
- database concerns
- external SDK logic
- framework-driven behavior unless unavoidable

### Application Layer

Should contain:
- use cases
- orchestration
- transaction boundaries when appropriate
- ports/interfaces for external dependencies

### Infrastructure Layer

Should contain:
- database adapters
- messaging adapters
- HTTP clients
- filesystem and provider integrations

Infrastructure should implement application or domain contracts instead of owning business rules.

### Interface Layer

Should contain:
- controllers
- presenters
- DTO mapping
- request and response translation

Interface code should not contain business rules.

## Core Rules

### Place Logic In The Correct Layer

Before changing code:
1. identify the current layer
2. identify where the decision actually belongs
3. move or add logic in the owning layer instead of fixing symptoms at the edge

### Preserve Dependency Direction

Prefer dependencies that point inward toward domain and application rules.

Avoid:
- controllers calling infrastructure-specific logic directly for business decisions
- domain code depending on frameworks
- application logic coupled to transport or persistence details

### Use Ports At Boundaries

When core logic needs external behavior, depend on an abstraction and implement it in infrastructure.

### Keep Framework Glue Thin

Controllers, routes, handlers, and presenters should translate data, call use cases, and return results. They should not own policy or domain decisions.

## Decision Heuristics

Ask:
1. Is this a business rule or a technical detail?
2. Which layer should own this decision?
3. Can the dependency be inverted behind a port?
4. Does this change leak transport or persistence concerns into core logic?
5. Can the rule be tested without the framework?

## Review Checklist

Verify that:
- business rules live in domain or application code
- controllers remain thin
- infrastructure adapts instead of decides
- dependencies point inward
- transaction boundaries are intentional
- boundary types do not leak where they do not belong

## Output Expectations

When using this skill:
1. explain the layer violation or boundary issue
2. explain where the logic belongs
3. implement the smallest safe boundary correction
4. preserve public behavior unless change is required
5. mention any trade-off from adding a port or mapper
