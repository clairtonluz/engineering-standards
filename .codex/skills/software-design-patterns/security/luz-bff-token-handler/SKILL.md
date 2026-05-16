---
name: luz-bff-token-handler
description: Apply the Backend-for-Frontend Token Handler design pattern. Use when a browser app needs secure server-side token storage. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Backend-for-Frontend Token Handler

## What It Is
Keep OAuth tokens in a backend-for-frontend and expose only session-specific responses to the browser.

## When To Use It
- A browser app needs secure server-side token storage.
- The BFF can use secure, HttpOnly, SameSite cookies.
- Frontend calls should avoid direct token handling.

## When Not To Use It
- The backend cannot protect sessions.
- Multiple frontends with very different needs share one handler.
- CSRF and session fixation controls are missing.

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
@RestController
class SessionController {
    @GetMapping("/session")
    SessionView session(Authentication authentication) {
        return SessionView.from(authentication);
    }
}
```
The BFF holds tokens server-side and exposes only session-shaped data to the browser.

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
- Use `$luz-backend-for-frontend` when that pattern better matches the problem.
- Use `$luz-oauth2-authorization-code` when that pattern better matches the problem.
- Use `$luz-api-gateway-security` when that pattern better matches the problem.
