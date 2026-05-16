---
name: luz-serverless-architecture
description: Apply the Serverless Architecture design pattern. Use when workloads are event-driven, bursty, or operationally suited to managed infrastructure. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Serverless Architecture

## What It Is
Run application capabilities on managed event-driven compute and services.

## When To Use It
- Workloads are event-driven, bursty, or operationally suited to managed infrastructure.
- Functions can be idempotent and short-lived.
- Platform limits are acceptable.

## When Not To Use It
- Long-running stateful processes are central.
- Cold starts or platform limits violate requirements.
- Local testing and observability cannot be addressed.

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
export async function handler(event: QueueEvent) {
  for (const message of event.records) {
    await processMessage(JSON.parse(message.body));
  }
}
```
Keep functions idempotent, short-lived, and explicit about retries and timeouts.

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
- Use `$luz-event-driven-architecture` when that pattern better matches the problem.
- Use `$luz-message-queue` when that pattern better matches the problem.
- Use `$luz-timeout` when that pattern better matches the problem.
