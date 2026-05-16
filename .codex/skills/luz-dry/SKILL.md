---
name: luz-dry
description: Apply DRY, Don't Repeat Yourself, when reviewing, refactoring, or changing code. Use to remove duplicated business rules, repeated calculations, repeated mappings, copied validation, repeated frontend logic, repeated Java/Spring Boot service logic, or duplicated configuration while avoiding premature abstraction.
---

# DRY

## What It Is
DRY means each piece of business knowledge should have one clear source of truth. It is about duplicated knowledge, not merely similar-looking code.

## When To Use It
- The same rule, calculation, validation, mapping, query, or workflow appears in multiple places.
- A bug fix would need to be applied repeatedly.
- Similar frontend components or backend services are drifting inconsistently.
- A shared concept has a clear domain name.

## When Not To Use It
- Similar code exists for different reasons and will evolve differently.
- Extraction would create a vague helper such as `CommonUtil`.
- The abstraction would make call sites harder to read.
- Simplicity is the bigger problem; use `$luz-kiss` first.

## Main Benefits
- Reduces inconsistent fixes.
- Gives important rules clear names.
- Makes behavior easier to test in one place.
- Lowers maintenance cost when reuse is real.

## Trade-Offs
- Premature DRY couples unrelated flows.
- Shared abstractions can become dumping grounds.
- Over-generalization often hides edge cases.

## Common Mistakes
- Extracting code because it looks similar, not because it means the same thing.
- Creating generic helpers with weak names.
- Centralizing code that needs separate security, validation, or audit behavior.
- Removing duplication before understanding why it exists.

## Implementation Checklist
- Confirm the duplicated code represents the same business knowledge.
- Name the shared concept in domain language.
- Keep the abstraction small and focused.
- Preserve each caller's validation, authorization, and error semantics.
- Add tests for the extracted rule or helper.
- Leave intentional duplication in place when independence is more valuable.

## Java / Spring Boot Example
```java
final class ReimbursementPolicy {
    boolean isReimbursable(Expense expense) {
        return expense.amount().isPositive() && expense.hasReceipt();
    }
}

@Service
class SubmitExpenseUseCase {
    private final ReimbursementPolicy policy;

    void submit(Expense expense) {
        if (!policy.isReimbursable(expense)) {
            throw new BusinessRuleViolation("expense is not reimbursable");
        }
    }
}
```
The repeated reimbursement rule gets one domain name and one test surface.

## TypeScript / Frontend Example
```typescript
function formatCurrency(value: number, currency: string, locale = "en-US") {
  return new Intl.NumberFormat(locale, { style: "currency", currency }).format(value);
}

function ExpenseAmount({ amount, currency }: { amount: number; currency: string }) {
  return <span>{formatCurrency(amount, currency)}</span>;
}
```
This is useful when currency formatting must be consistent across screens.

## Testing Strategy
- Add tests for the extracted shared behavior before changing callers.
- Keep caller-level tests for important integration paths.
- Include edge cases that were previously duplicated.
- Avoid tests that assert private helper structure.

## Security Considerations
- Centralize security policy only when every caller needs the exact same rule.
- Do not centralize sanitized and unsanitized paths together.
- Avoid shared logging helpers that accidentally log secrets or personal data.

## Observability Considerations
- Keep logs and metrics meaningful at each caller boundary.
- Do not remove context just to share a helper.
- Track shared rule failures with safe, low-cardinality labels.

## Related Skills
- Use `$luz-solid` when duplication suggests a missing responsibility or boundary.
- Use `$luz-kiss` when the DRY abstraction would be harder to understand than the duplication.
