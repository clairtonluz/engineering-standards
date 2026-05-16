---
name: luz-backpressure
description: Apply the Backpressure design pattern. Use when streams, queues, or event processing can overload consumers. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Backpressure

## What It Is
Make producers slow down or stop when consumers cannot keep up.

## When To Use It
- Streams, queues, or event processing can overload consumers.
- Bounded memory is required.
- The protocol or API can signal capacity.

## When Not To Use It
- Dropping data is acceptable and simpler.
- Producers cannot respond to pressure.
- Buffers are unbounded.

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
```typescript
const stream = source.pipeThrough(new TransformStream({ highWaterMark: 16 }));
```
Make producers respect consumer capacity instead of buffering without bounds.

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
- Use `$luz-producer-consumer` when that pattern better matches the problem.
- Use `$luz-load-shedding` when that pattern better matches the problem.
- Use `$luz-message-queue` when that pattern better matches the problem.
