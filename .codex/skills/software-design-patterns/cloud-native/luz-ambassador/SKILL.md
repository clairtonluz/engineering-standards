---
name: luz-ambassador
description: Apply the Ambassador design pattern. Use when outbound service discovery, retries, tls, or protocol translation should be externalized. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Ambassador

## What It Is
Use a helper container or proxy to manage outbound communication for an application.

## When To Use It
- Outbound service discovery, retries, TLS, or protocol translation should be externalized.
- The app should remain unaware of communication infrastructure.
- The helper shares the app lifecycle.

## When Not To Use It
- The platform service mesh already solves it.
- The helper needs independent scaling.
- The app and ambassador configuration drift.

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
apiVersion: v1
kind: Pod
spec:
  containers:
    - name: app
      image: example/app:1.0
    - name: ambassador
      image: example/egress-proxy:1.0
```
The ambassador handles outbound communication concerns for the application container.

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
- Kubernetes/Cloud: Use Kubernetes primitives such as Deployments, Pods, Services, ConfigMaps, Secrets, probes, and leases deliberately; keep application images least-privileged.

## Choosing Between Related Patterns
- Use `$luz-sidecar` when that pattern better matches the problem.
- Use `$luz-service-mesh` when that pattern better matches the problem.
- Use `$luz-api-gateway` when that pattern better matches the problem.
