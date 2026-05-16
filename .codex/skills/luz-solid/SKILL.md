---
name: luz-solid
description: Apply SOLID principles when designing, reviewing, refactoring, or changing code. Use for responsibility boundaries, object design, abstractions, dependency inversion, interface design, Java/Spring Boot services, TypeScript/frontend modules, and maintainability reviews.
---

# SOLID

## What It Is
SOLID is a set of object and module design principles that help keep code cohesive, extensible, substitutable, focused, and testable.

## When To Use It
- Code has mixed responsibilities or unclear reasons to change.
- New variants are causing repeated edits across stable code.
- Interfaces are broad, leaky, or hard to implement.
- Business logic depends directly on persistence, HTTP clients, SDKs, or framework glue.
- Refactoring should improve maintainability without changing behavior.

## When Not To Use It
- A direct function or simple class is already clear and stable.
- An abstraction would have only one implementation and no boundary value.
- The issue is mostly duplicated knowledge; use `$luz-dry`.
- The issue is accidental complexity; use `$luz-kiss` first.

## Principles
- Single Responsibility: one clear reason to change.
- Open/Closed: extend behavior through focused collaborators when repeated modification is risky.
- Liskov Substitution: implementations must honor caller expectations.
- Interface Segregation: expose small, cohesive contracts for each consumer.
- Dependency Inversion: depend on abstractions at external or volatile boundaries.

## Main Benefits
- Clarifies ownership and responsibility.
- Improves testability by isolating policy from infrastructure.
- Reduces coupling to frameworks, providers, and unstable details.
- Makes extension safer when variation already exists.

## Trade-Offs
- Adds names, interfaces, and indirection.
- Can become ceremony if applied before there is real variation.
- Can hide simple logic behind too many layers.

## Common Mistakes
- Creating interfaces for every class by default.
- Treating Single Responsibility as one method per class.
- Using inheritance where composition is clearer.
- Moving business rules into controllers, repositories, or adapters.
- Adding extension points for imaginary future requirements.

## Implementation Checklist
- Name the responsibility of the code being changed.
- Identify the reason it changes today.
- Separate orchestration, business rules, persistence, and presentation only where the split improves clarity.
- Introduce interfaces at boundaries, not around every implementation.
- Prefer constructor injection.
- Keep contracts small and consumer-oriented.
- Preserve behavior and add tests around the extracted responsibility.

## Java / Spring Boot Example
```java
interface ExchangeRateProvider {
    Rate currentRate(CurrencyPair pair);
}

@Service
class ConvertCurrencyUseCase {
    private final ExchangeRateProvider rates;

    ConvertCurrencyUseCase(ExchangeRateProvider rates) {
        this.rates = rates;
    }

    Money convert(Money source, Currency target) {
        CurrencyPair pair = CurrencyPair.of(source.currency(), target);
        return source.convertWith(rates.currentRate(pair));
    }
}
```
The use case depends on an application-owned abstraction. The HTTP client or SDK belongs in an adapter.

## TypeScript / Frontend Example
```typescript
interface ExpenseFormatter {
  format(expense: Expense): string;
}

class CurrencyExpenseFormatter implements ExpenseFormatter {
  format(expense: Expense) {
    return `${expense.description}: ${expense.amount.toFixed(2)}`;
  }
}
```
Use this when formatting truly varies. For one fixed format, a plain function is simpler.

## Testing Strategy
- Unit test extracted policies and use cases without framework startup.
- Use fakes for ports and adapters.
- Test each implementation against the same contract when substitutability matters.
- Add integration tests for Spring wiring, persistence adapters, or HTTP clients.

## Security Considerations
- Keep authorization decisions explicit in use cases or domain policies.
- Do not hide validation or access control behind generic abstractions.
- Avoid leaking sensitive framework or persistence details into domain contracts.

## Observability Considerations
- Preserve trace and log context when extracting collaborators.
- Put metrics at stable use-case or adapter boundaries.
- Avoid spreading logging across many tiny classes without useful context.

## Related Skills
- Use `$luz-dry` when the main force is duplicated business knowledge.
- Use `$luz-kiss` when the main force is unnecessary complexity.
