---
name: luz-facade
description: Apply the Facade design pattern. Use when callers need one clear workflow over several services. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Facade

## What It Is
Provide a small, simple interface over a larger subsystem.

## When To Use It
- Callers need one clear workflow over several services.
- Subsystem details are noisy or unstable.
- A boundary should shield clients from implementation churn.

## When Not To Use It
- The facade becomes a god service.
- It hides errors or authorization decisions.
- Callers still need the full subsystem flexibility.

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
@Service
class CheckoutFacade {
    private final CartService carts;
    private final PaymentService payments;
    private final ReceiptService receipts;

    CheckoutFacade(CartService carts, PaymentService payments, ReceiptService receipts) {
        this.carts = carts;
        this.payments = payments;
        this.receipts = receipts;
    }

    Receipt checkout(UUID cartId, PaymentDetails details) {
        Cart cart = carts.requireOpenCart(cartId);
        Payment payment = payments.charge(cart.total(), details);
        return receipts.create(cart, payment);
    }
}
```
The facade simplifies orchestration for callers while domain rules remain in the underlying services.

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
- Use `$luz-adapter` when that pattern better matches the problem.
- Use `$luz-application-service` when that pattern better matches the problem.
- Use `$luz-backend-for-frontend` when that pattern better matches the problem.
