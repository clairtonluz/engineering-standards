---
name: luz-strangler-security-migration
description: Apply the Strangler Security Migration design pattern. Use when a legacy system is being replaced route by route. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Strangler Security Migration

## What It Is
Migrate legacy security behavior incrementally while preserving access control parity.

## When To Use It
- A legacy system is being replaced route by route.
- Security rules must be compared and enforced during migration.
- Risk must be reduced without a big-bang cutover.

## When Not To Use It
- The legacy rules are unknown and cannot be observed.
- The new system weakens authorization for convenience.
- No rollback or parity validation exists.

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
legacy auth -> compatibility layer -> new policy enforcement -> retired legacy path
```
Migrate security behavior one route or capability at a time with parity checks.

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
- Use `$luz-strangler-fig` when that pattern better matches the problem.
- Use `$luz-api-gateway-security` when that pattern better matches the problem.
- Use `$luz-zero-trust-architecture` when that pattern better matches the problem.
