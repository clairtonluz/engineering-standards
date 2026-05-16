---
name: luz-kiss
description: Apply KISS, Keep It Simple, Stupid, when designing, reviewing, refactoring, or changing code. Use to reduce unnecessary abstraction, simplify control flow, remove speculative design, clarify Java/Spring Boot services, simplify TypeScript/frontend components, and keep implementations readable, secure, and testable.
---

# KISS

## What It Is
KISS means choosing the simplest solution that correctly solves the current problem and remains easy to read, test, secure, and maintain.

## When To Use It
- Code has too many layers, flags, callbacks, abstractions, or generic helpers.
- A feature can be implemented clearly with direct code.
- A proposed design solves imagined future requirements.
- Tests are hard to write because the implementation is too indirect.

## When Not To Use It
- Simplicity would hide an important boundary such as authorization, transactions, persistence, or external calls.
- Multiple real variants already exist and need a stable abstraction; use `$luz-solid`.
- Duplicated business knowledge is creating inconsistency; use `$luz-dry`.

## Main Benefits
- Improves readability and review speed.
- Reduces bug surface from unnecessary moving parts.
- Keeps tests focused on behavior.
- Makes security and error paths easier to inspect.

## Trade-Offs
- Too much simplicity can become under-design.
- Direct code may need refactoring when real variation appears.
- Avoiding abstraction can duplicate knowledge if the same rule appears in several places.

## Common Mistakes
- Calling code simple because it is short, even when it is unclear.
- Avoiding all abstraction even at external boundaries.
- Replacing domain names with primitive strings and maps.
- Hiding complexity in one large function.

## Implementation Checklist
- State the current requirement in one sentence.
- Remove speculative branches, unused extension points, and generic wrappers.
- Prefer clear names and direct control flow.
- Keep validation and error handling explicit.
- Use framework conventions instead of custom infrastructure.
- Add abstraction only for a real boundary, repeated rule, or existing variation.

## Java / Spring Boot Example
```java
@Service
class ExpenseApprovalService {
    void approve(Expense expense, User approver) {
        if (!approver.canApprove(expense)) {
            throw new AccessDeniedException("approver cannot approve this expense");
        }
        expense.approveBy(approver.id());
    }
}
```
This direct service is better than a strategy/factory stack when there is only one approval rule.

## TypeScript / Frontend Example
```typescript
function SaveButton({ saving, onSave }: { saving: boolean; onSave: () => void }) {
  return (
    <button type="button" disabled={saving} onClick={onSave}>
      {saving ? "Saving..." : "Save"}
    </button>
  );
}
```
Keep simple UI behavior local until shared state, accessibility complexity, or repeated behavior justifies extraction.

## Testing Strategy
- Write tests that describe visible behavior.
- Prefer fewer, clearer tests over brittle tests for implementation details.
- Add edge-case tests around explicit validation and error handling.
- Remove test setup that only exists because the implementation is over-abstracted.

## Security Considerations
- Simple code should still validate inputs and enforce authorization explicitly.
- Do not remove security layers just to reduce files.
- Prefer readable checks over clever generic policy plumbing unless the policy is truly shared.

## Observability Considerations
- Keep logs close to meaningful use-case and failure boundaries.
- Do not add logging frameworks or metrics wrappers before there is a real operational need.
- Keep messages clear and free of secrets.

## Related Skills
- Use `$luz-solid` when simplicity starts to blur responsibilities or dependency boundaries.
- Use `$luz-dry` when direct code repeats the same business knowledge in multiple places.
