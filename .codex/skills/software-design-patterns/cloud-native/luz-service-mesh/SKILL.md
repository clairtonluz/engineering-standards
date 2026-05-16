---
name: luz-service-mesh
description: Apply the Service Mesh design pattern. Use when mtls, traffic shaping, retries, or telemetry need consistent enforcement. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Service Mesh

## What It Is
Use platform-managed sidecar or node proxies for service-to-service traffic policy.

## When To Use It
- mTLS, traffic shaping, retries, or telemetry need consistent enforcement.
- Many services need the same communication policy.
- Platform teams can operate the mesh.

## When Not To Use It
- A few services can handle communication directly.
- Mesh complexity exceeds team maturity.
- Application-level authorization is delegated entirely to the mesh.

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
trafficPolicy:
  tls:
    mode: ISTIO_MUTUAL
```
A service mesh moves cross-cutting service communication policy into the platform.

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
- Use `$luz-api-gateway` when that pattern better matches the problem.
- Use `$luz-zero-trust-architecture` when that pattern better matches the problem.
