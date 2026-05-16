---
name: luz-chain-of-responsibility
description: Apply the Chain of Responsibility design pattern. Use when validation, middleware, filters, authorization checks, or processing pipelines. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Chain of Responsibility

## What It Is
Pass a request through a sequence of handlers until one handles it or all contribute.

## When To Use It
- Validation, middleware, filters, authorization checks, or processing pipelines.
- Handlers should be independently testable and reorderable.
- The caller should not know which handler handles the request.

## When Not To Use It
- Handler order is ambiguous.
- Every handler must always run in a strict workflow.
- Failures need centralized transaction semantics.

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
type Handler = (request: Request, next: () => Promise<Response>) => Promise<Response>;

function compose(handlers: Handler[]): Handler {
  return handlers.reduceRight(
    (nextHandler, handler) => (request, next) => handler(request, () => nextHandler(request, next)),
    async (_request, next) => next()
  );
}
```
Each handler either processes the request or passes it to the next handler.

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
- Use `$luz-pipe-and-filter` when that pattern better matches the problem.
- Use `$luz-decorator` when that pattern better matches the problem.
- Use middleware when that pattern better matches the problem.
