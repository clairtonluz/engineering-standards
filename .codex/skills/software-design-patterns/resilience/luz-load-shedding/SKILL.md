---
name: luz-load-shedding
description: Apply the Load Shedding design pattern. Use when the system must preserve critical work under overload. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Load Shedding

## What It Is
Intentionally reject lower-priority work when capacity is exhausted.

## When To Use It
- The system must preserve critical work under overload.
- Queues or pools are near saturation.
- Clients can retry or degrade gracefully.

## When Not To Use It
- Every request is equally critical and cannot fail.
- No overload signal exists.
- Rejected work is silently dropped.

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
if (queue.remainingCapacity() == 0) {
    throw new ServiceUnavailableException("system is busy");
}
```
Reject low-priority work before the system collapses.

## Testing Strategy
- Unit test the behavior that the pattern is meant to protect.
- Add boundary tests for invalid inputs, failure paths, and edge cases.
- Prefer fakes or in-memory adapters over mocking implementation details.
- Add integration tests when the pattern crosses a framework, persistence, network, or deployment boundary.

## Security Considerations
Validate inputs at the pattern boundary and avoid hiding authorization, identity, or data-handling decisions behind generic abstractions.

## Observability Considerations
Track success/failure counts, latency, saturation, rejected requests, retries, timeout counts, and state transitions with correlation IDs and low-cardinality tags.

## Platform Notes
- Java/Spring Boot: Use dependency injection, small interfaces, and focused unit tests instead of static global access.
- TypeScript/Frontend: Use typed interfaces, dependency injection where practical, and accessible UI states when the pattern affects presentation.
- Kubernetes/Cloud: Keep platform concerns in configuration, infrastructure adapters, or deployment manifests rather than leaking them into domain logic.

## Choosing Between Related Patterns
- Use `$luz-rate-limiting` when that pattern better matches the problem.
- Use `$luz-bulkhead` when that pattern better matches the problem.
- Use `$luz-backpressure` when that pattern better matches the problem.
