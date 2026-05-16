---
name: luz-mvc
description: Apply the MVC design pattern. Use when server-rendered web applications or ui frameworks that naturally separate controller and view. Includes when to use it, when not to use it, benefits, trade-offs, mistakes, checklist, examples, testing, security, observability, and related pattern guidance.
---

# MVC

## What It Is
Separate model, view, and controller responsibilities for UI-backed applications.

## When To Use It
- Server-rendered web applications or UI frameworks that naturally separate controller and view.
- Controllers should translate input while models own data and rules.
- Views should render state without owning business decisions.

## When Not To Use It
- Complex client state needs MVVM or Flux.
- Controllers start owning business logic.
- The framework already provides a simpler component model.

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
@Controller
class ExpenseController {
    private final ExpenseService expenses;

    @PostMapping("/expenses")
    String create(@Valid ExpenseForm form, RedirectAttributes redirect) {
        ExpenseId id = expenses.create(form.toCommand());
        redirect.addFlashAttribute("expenseId", id.value());
        return "redirect:/expenses";
    }
}
```
The controller handles web translation, the model owns data and rules, and the view renders output.

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
- Use `$luz-mvp` when that pattern better matches the problem.
- Use `$luz-mvvm` when that pattern better matches the problem.
- Use `$luz-layered-architecture` when that pattern better matches the problem.
