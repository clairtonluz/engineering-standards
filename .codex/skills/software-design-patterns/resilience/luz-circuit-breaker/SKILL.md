---
name: luz-circuit-breaker
description: Apply the Circuit Breaker design pattern. Use when a remote dependency has repeated failures or high latency. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Circuit Breaker

## What It Is
Stop calling an unhealthy dependency temporarily so the system can fail fast and recover.

## When To Use It
- A remote dependency has repeated failures or high latency.
- Fail-fast behavior protects threads, queues, or users.
- Recovery can be tested with half-open probes.

## When Not To Use It
- Failures are not from a remote or unstable dependency.
- Retrying is required before opening but not configured.
- The fallback hides data corruption or security failures.

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
CircuitBreaker breaker = circuitBreakerRegistry.circuitBreaker("payment-provider");
Supplier<PaymentReceipt> guarded = CircuitBreaker.decorateSupplier(breaker, () -> client.charge(request));
```
Open the circuit after repeated failures so callers fail fast instead of amplifying an outage.

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
- Use `$luz-retry` when that pattern better matches the problem.
- Use `$luz-timeout` when that pattern better matches the problem.
- Use `$luz-bulkhead` when that pattern better matches the problem.
