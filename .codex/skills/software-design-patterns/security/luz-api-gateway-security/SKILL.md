---
name: luz-api-gateway-security
description: Apply the API Gateway Security design pattern. Use when jwt validation, mtls, rate limiting, header normalization, or threat filtering belongs at the edge. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# API Gateway Security

## What It Is
Apply consistent edge security policies at the API gateway.

## When To Use It
- JWT validation, mTLS, rate limiting, header normalization, or threat filtering belongs at the edge.
- Clients need one secure ingress path.
- Services still enforce domain authorization.

## When Not To Use It
- The gateway becomes the only authorization layer.
- Sensitive headers are forwarded blindly.
- Policies differ by route without documentation.

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
policies:
  - validate-jwt
  - enforce-mtls
  - rate-limit
  - remove-sensitive-headers
```
Apply edge security controls consistently while keeping domain authorization in services.

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
- Use `$luz-api-gateway` when that pattern better matches the problem.
- Use `$luz-rate-limiting` when that pattern better matches the problem.
- Use `$luz-zero-trust-architecture` when that pattern better matches the problem.
