---
name: luz-leader-election
description: Apply the Leader Election design pattern. Use when replicated services need exactly one active scheduler or coordinator. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Leader Election

## What It Is
Elect one active instance among replicas to perform singleton work.

## When To Use It
- Replicated services need exactly one active scheduler or coordinator.
- The platform provides leases or locks.
- Failover must be automatic.

## When Not To Use It
- Work can be partitioned across all replicas.
- The task is not idempotent.
- Lease expiry and clock behavior are not understood.

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
```text
replicas compete for lease -> current holder performs singleton work -> lease renews or expires
```
Use leader election for exactly-one active worker among replicated instances.

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
- Kubernetes/Cloud: Use Kubernetes primitives such as Deployments, Pods, Services, ConfigMaps, Secrets, probes, and leases deliberately; keep application images least-privileged.

## Choosing Between Related Patterns
- Use `$luz-singleton` when that pattern better matches the problem.
- Use `$luz-leader-follower` when that pattern better matches the problem.
- Use `$luz-health-check` when that pattern better matches the problem.
