---
name: luz-prototype
description: Apply the Prototype design pattern. Use when object setup is expensive or repetitive. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Prototype

## What It Is
Create new objects by copying a configured example instance.

## When To Use It
- Object setup is expensive or repetitive.
- Templates should be reused safely.
- Copying preserves a known-good configuration.

## When Not To Use It
- Objects contain mutable shared references that are hard to clone safely.
- Identity, ownership, or security context must not be copied.
- A factory can express construction more clearly.

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
type InvoiceTemplate = Readonly<{
  currency: string;
  taxRate: number;
  lineItems: ReadonlyArray<{ description: string; amount: number }>;
}>;

function createInvoice(template: InvoiceTemplate, overrides: Partial<InvoiceTemplate>) {
  return structuredClone({ ...template, ...overrides });
}
```
Clone immutable templates or value objects. Avoid sharing mutable nested state between copies.

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
- Use `$luz-builder` when that pattern better matches the problem.
- Use `$luz-factory-method` when that pattern better matches the problem.
- Use `$luz-memento` when that pattern better matches the problem.
