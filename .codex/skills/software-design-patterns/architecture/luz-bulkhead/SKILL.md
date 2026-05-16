---
name: luz-bulkhead
description: Apply the Bulkhead design pattern. Use when workloads have different criticality or resource profiles. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Bulkhead

## What It Is
Isolate resources so failure or saturation in one area does not sink the whole system.

## When To Use It
- Workloads have different criticality or resource profiles.
- One dependency can become slow or overloaded.
- Separate pools, queues, or limits reduce blast radius.

## When Not To Use It
- The system is too small for the added operational tuning.
- The same bottleneck is shared underneath all pools.
- Metrics do not show which bulkhead is saturated.

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
ExecutorService reportsPool = Executors.newFixedThreadPool(8);
ExecutorService paymentsPool = Executors.newFixedThreadPool(16);
```
Separate resource pools so one overloaded workload does not exhaust capacity for others.

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
- Use `$luz-circuit-breaker` when that pattern better matches the problem.
- Use `$luz-timeout` when that pattern better matches the problem.
- Use `$luz-load-shedding` when that pattern better matches the problem.
