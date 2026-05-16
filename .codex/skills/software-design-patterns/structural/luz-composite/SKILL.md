---
name: luz-composite
description: Apply the Composite design pattern. Use when tree structures such as menus, documents, permissions, or ui nodes. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# Composite

## What It Is
Represent part-whole hierarchies so clients can treat individual and grouped objects uniformly.

## When To Use It
- Tree structures such as menus, documents, permissions, or UI nodes.
- Operations apply recursively to leaves and groups.
- The hierarchy should hide traversal details.

## When Not To Use It
- Leaf and group behavior differs too much.
- The tree is shallow and direct code is clearer.
- Mutation and ownership rules cannot be enforced.

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
interface MenuNode {
  label: string;
  render(): HTMLElement;
}

class MenuGroup implements MenuNode {
  constructor(public label: string, private readonly children: MenuNode[]) {}

  render() {
    const list = document.createElement("ul");
    this.children.forEach(child => list.append(child.render()));
    return list;
  }
}
```
Treat leaves and groups through the same interface, but keep mutation rules explicit.

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
- Use `$luz-iterator` when that pattern better matches the problem.
- Use `$luz-visitor` when that pattern better matches the problem.
- Use `$luz-component-pattern` when that pattern better matches the problem.
