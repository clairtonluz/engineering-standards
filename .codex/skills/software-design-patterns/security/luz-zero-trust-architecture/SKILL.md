---
name: luz-zero-trust-architecture
description: Apply the Zero Trust Architecture design pattern. Use when networks are not trusted by default. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Zero Trust Architecture

## What It Is
Design systems so every request is explicitly verified, least privileged, and monitored.

## When To Use It
- Networks are not trusted by default.
- Identity, device, workload, and context should influence access.
- Sensitive systems need continuous verification.

## When Not To Use It
- It is treated as a product instead of an architecture principle.
- Legacy trust boundaries are left unmeasured.
- Monitoring and policy enforcement are not operationalized.

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
verify explicitly -> least privilege -> assume breach -> continuous monitoring
```
Every request is authenticated, authorized, and evaluated with context.

## Testing Strategy
- Unit test the behavior that the pattern is meant to protect.
- Add boundary tests for invalid inputs, failure paths, and edge cases.
- Prefer fakes or in-memory adapters over mocking implementation details.
- Add integration tests when the pattern crosses a framework, persistence, network, or deployment boundary.

## Security Considerations
Use least privilege, validate issuer/audience/scope/expiry where tokens are involved, protect secrets in server-side stores, and never log credentials or bearer tokens.

## Observability Considerations
Log authentication and authorization outcomes with correlation IDs and safe identifiers; emit metrics for denied requests, policy errors, and unusual token exchange or gateway behavior.

## Platform Notes
- Java/Spring Boot: Use dependency injection, small interfaces, and focused unit tests instead of static global access.
- TypeScript/Frontend: Use typed interfaces, dependency injection where practical, and accessible UI states when the pattern affects presentation.
- Kubernetes/Cloud: Keep platform concerns in configuration, infrastructure adapters, or deployment manifests rather than leaking them into domain logic.

## Choosing Between Related Patterns
- Use `$luz-api-gateway-security` when that pattern better matches the problem.
- Use `$luz-service-mesh` when that pattern better matches the problem.
- Use `$luz-token-exchange` when that pattern better matches the problem.
