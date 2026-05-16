---
name: luz-api-gateway
description: Apply the API Gateway design pattern. Use when clients need one stable entry point. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# API Gateway

## What It Is
Provide an edge entry point for routing, protocol translation, and cross-cutting edge policies.

## When To Use It
- Clients need one stable entry point.
- Authentication, routing, rate limits, or request metadata should be consistent at the edge.
- Internal services should not expose every detail to clients.

## When Not To Use It
- Business authorization is moved entirely to the gateway.
- The gateway becomes a large orchestration service.
- A BFF is needed for client-specific composition.

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
```yaml
routes:
  - path: /api/expenses/**
    service: expense-service
    policies:
      - authenticate
      - rate-limit
      - request-id
```
Centralize edge routing and shared edge policies without moving business rules into the gateway.

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
- Use `$luz-backend-for-frontend` when that pattern better matches the problem.
- Use `$luz-api-gateway-security` when that pattern better matches the problem.
- Use reverse proxy guidance when that pattern better matches the problem.
