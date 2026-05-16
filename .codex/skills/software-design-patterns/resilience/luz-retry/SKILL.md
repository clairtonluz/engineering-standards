---
name: luz-retry
description: Apply the Retry design pattern. Use when failures are transient. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Retry

## What It Is
Repeat a failed operation under bounded, safe conditions.

## When To Use It
- Failures are transient.
- The operation is idempotent or has idempotency keys.
- Backoff, jitter, and max attempts are configured.

## When Not To Use It
- The operation is not safe to repeat.
- The failure is permanent validation or authorization failure.
- Retries would amplify overload.

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
Retry retry = retryRegistry.retry("exchange-rates");
Supplier<Rate> guarded = Retry.decorateSupplier(retry, () -> client.latestRate(pair));
```
Retry only idempotent or explicitly safe operations, with bounded attempts and backoff.

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
- Use `$luz-timeout` when that pattern better matches the problem.
- Use `$luz-circuit-breaker` when that pattern better matches the problem.
- Use `$luz-dead-letter-queue` when that pattern better matches the problem.
