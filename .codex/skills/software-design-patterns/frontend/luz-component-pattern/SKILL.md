---
name: luz-component-pattern
description: Apply the Component Pattern design pattern. Use when ui elements repeat across screens. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Component Pattern

## What It Is
Build UI from focused, reusable components with explicit inputs and outputs.

## When To Use It
- UI elements repeat across screens.
- State and rendering can be scoped clearly.
- Accessibility and behavior should be encapsulated.

## When Not To Use It
- A one-off element is simpler inline.
- The component hides side effects.
- Props are broad and unclear.

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
type ExpenseCardProps = {
  expense: Expense;
  onSelect: (id: string) => void;
};

function ExpenseCard({ expense, onSelect }: ExpenseCardProps) {
  return <button onClick={() => onSelect(expense.id)}>{expense.description}</button>;
}
```
Keep components focused on a small, reusable UI responsibility.

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
- TypeScript/Frontend: Use typed props, explicit state ownership, accessible markup, and framework-native primitives such as Angular signals/services or React hooks where appropriate.
- Kubernetes/Cloud: Keep platform concerns in configuration, infrastructure adapters, or deployment manifests rather than leaking them into domain logic.

## Choosing Between Related Patterns
- Use `$luz-container-presenter` when that pattern better matches the problem.
- Use `$luz-atomic-design` when that pattern better matches the problem.
- Use `$luz-composite` when that pattern better matches the problem.
