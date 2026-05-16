---
name: luz-mediator
description: Apply the Mediator design pattern. Use when ui widgets, domain collaborators, or services need orchestration. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Mediator

## What It Is
Centralize coordination between peers so they do not depend on each other directly.

## When To Use It
- UI widgets, domain collaborators, or services need orchestration.
- Peer-to-peer references are becoming tangled.
- Coordination logic should be tested separately.

## When Not To Use It
- The mediator becomes a god object.
- Simple parent-child communication is enough.
- Domain rules are moved into UI coordination.

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
class CheckoutMediator {
  constructor(private readonly form: CheckoutForm, private readonly summary: OrderSummary) {}

  bind() {
    this.form.onAddressChanged(address => this.summary.updateShipping(address));
    this.form.onPaymentChanged(payment => this.summary.updatePayment(payment));
  }
}
```
Use a mediator to coordinate peers without making each peer know every other peer.

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
- Use `$luz-facade` when that pattern better matches the problem.
- Use `$luz-observer` when that pattern better matches the problem.
- Use `$luz-application-service` when that pattern better matches the problem.
