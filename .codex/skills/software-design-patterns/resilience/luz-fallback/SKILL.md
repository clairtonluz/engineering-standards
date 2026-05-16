---
name: luz-fallback
description: Apply the Fallback design pattern. Use when a degraded response is safer than total failure. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Fallback

## What It Is
Use an alternate response or path when the preferred path fails.

## When To Use It
- A degraded response is safer than total failure.
- Fallback data freshness and correctness are clear.
- Users can be told when data is degraded.

## When Not To Use It
- Stale or partial data would cause harm.
- The fallback hides authorization failures.
- The fallback becomes the normal path.

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
ExchangeRate rate = rates.latest(pair)
    .orElseGet(() -> cachedRates.lastKnownGood(pair));
```
Fallbacks must be explicit about freshness and business impact.

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
- Use `$luz-circuit-breaker` when that pattern better matches the problem.
- Use `$luz-cache-aside` when that pattern better matches the problem.
- Use `$luz-timeout` when that pattern better matches the problem.
