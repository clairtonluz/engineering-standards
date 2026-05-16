---
name: luz-cqrs
description: Apply the CQRS design pattern. Use when read and write models have different shapes, scale, or security needs. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# CQRS

## What It Is
Separate command models that change state from query models that read state.

## When To Use It
- Read and write models have different shapes, scale, or security needs.
- Commands need domain invariants while queries need optimized projections.
- Auditability and task-focused APIs matter.

## When Not To Use It
- CRUD is simple and symmetric.
- CQRS is used only to create boilerplate handlers.
- Read models become stale without explicit consistency rules.

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
@Service
class ExpenseQueries {
    private final JdbcClient jdbc;

    ExpenseDetails details(UUID id) {
        return jdbc.sql("select id, amount, status from expenses where id = :id")
            .param("id", id)
            .query(ExpenseDetails.class)
            .single();
    }
}
```
Use separate command handlers for writes and query models for reads when their needs diverge.

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
- Use `$luz-event-sourcing` when that pattern better matches the problem.
- Use `$luz-command` when that pattern better matches the problem.
- Use `$luz-repository` when that pattern better matches the problem.
