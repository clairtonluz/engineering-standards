---
name: luz-redux
description: Apply the Redux Pattern design pattern. Use when shared client state is complex and benefits from time-travel/debugging. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Redux Pattern

## What It Is
Manage client state through actions, pure reducers, and a single predictable store.

## When To Use It
- Shared client state is complex and benefits from time-travel/debugging.
- Updates should be serializable and traceable.
- Reducers can remain pure.

## When Not To Use It
- Server state should be handled by a data-fetching cache.
- State is local to one component.
- Reducers contain side effects.

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
const expensesSlice = createSlice({
  name: "expenses",
  initialState,
  reducers: {
    approved(state, action: PayloadAction<string>) {
      state.items[action.payload].status = "approved";
    }
  }
});
```
Keep reducers pure and side effects in thunks, effects, or middleware.

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
- Use `$luz-flux` when that pattern better matches the problem.
- Use `$luz-cqrs` when that pattern better matches the problem.
- Use `$luz-mvvm` when that pattern better matches the problem.
