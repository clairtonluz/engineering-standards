---
name: luz-event-driven-architecture
description: Apply the Event-Driven Architecture design pattern. Use when systems need loose coupling and asynchronous workflows. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Event-Driven Architecture

## What It Is
Coordinate systems by publishing and reacting to events.

## When To Use It
- Systems need loose coupling and asynchronous workflows.
- Multiple consumers react to business facts.
- Temporal decoupling improves resilience or scale.

## When Not To Use It
- Callers need immediate request-response consistency.
- Events are used as commands.
- Consumers cannot tolerate eventual consistency.

## Main Benefits
- Clarifies ownership and intent when the problem genuinely matches the pattern.
- Improves testability by giving collaborators explicit roles.
- Reduces duplication when repeated decisions are moved into one clear structure.

## Trade-Offs
- Adds names and indirection that must earn their keep.
- Can make debugging harder if responsibilities are split without a clear boundary.
- May be unnecessary when a direct function, class, or module is enough.

## Common Mistakes
- Applying the pattern because it is familiar instead of because the problem needs it.
- Letting the pattern hide business rules, validation, authorization, or failure handling.
- Creating abstractions before there are at least two real variations or a clear boundary.

## Implementation Checklist
- Name the problem this pattern is solving.
- Keep the public API small and explicit.
- Keep business rules in the owning layer, not in framework glue.
- Prefer constructor injection over hidden globals.
- Write tests around behavior and boundaries, not internal class structure.
- Document the trade-off if the pattern adds visible indirection.

## Example Implementation
```java
public record ExpenseApproved(UUID expenseId, UUID approvedBy, Instant occurredAt) {}

interface DomainEventPublisher {
    void publish(Object event);
}
```
Publish meaningful facts after state changes commit, not commands disguised as events.

## Testing Strategy
- Unit test the behavior that the pattern is meant to protect.
- Add boundary tests for invalid inputs, failure paths, and edge cases.
- Prefer fakes or in-memory adapters over mocking implementation details.
- Add integration tests when the pattern crosses a framework, persistence, network, or deployment boundary.

## Security Considerations
Validate inputs at the pattern boundary and avoid hiding authorization, identity, or data-handling decisions behind generic abstractions.

## Observability Considerations
Expose useful logs, metrics, or traces at the boundary where this pattern changes control flow, data flow, or external calls.

## Platform Notes
- Java/Spring Boot: Use dependency injection, small interfaces, and focused unit tests instead of static global access.
- TypeScript/Frontend: Use typed interfaces, dependency injection where practical, and accessible UI states when the pattern affects presentation.
- Kubernetes/Cloud: Keep platform concerns in configuration, infrastructure adapters, or deployment manifests rather than leaking them into domain logic.

## Choosing Between Related Patterns
- Use `$luz-publish-subscribe` when that pattern better matches the problem.
- Use `$luz-event-bus` when that pattern better matches the problem.
- Use `$luz-outbox` when that pattern better matches the problem.
